# INSTAL·LACIÓ DEL WINDOWS SERVER CORE.

Hem de triar l'opció que no duu "experiencia escritorio"

<img width=60% src="../png/core/Instal1.png">

<img width=60% src="../png/core/Install2.png">

En **VirtualBox** instal·lem el GuestAdditions. Insertant a ISO al CD i excutant l'exe.

<img width=60% src="../png/core/InstallGuestAdditions0.png">


<img width=60% src="../png/core/InstallGuestAdditions.png">


## SCONFIG. CONFIGURACIÓ INICIAL.

Encara amb els cmdLets de PowerShell es poden fer totes les tasques de confifguració inicial, disposem d'una utilitat específica interactiva més pràctica: **sconfig**

<img width=60% src="../png/core/sconfig.png">

### Configuració de la NIC
<img width=60% src="../png/core/sconfig.png">

### Configuració d'actualitzacions automàtiques
<img width=60% src="../png/core/sconfig.png">

### Habilitar Escriptori remot

<img width=60% src="../png/core/sconfig.png">

## INSTAL·LACIÓ DE L'ACTIVE DIRECTORY.
# INSTAL·LACIÓ I CONFIGURACIÓ BÀSICA D'*ACTIVE DIRECTORY* DES DE POWERSHELL

##  Instal·lació de Active Directory
Les eines d'administrador no venen per defecte.Per això afegim el *-IncludeManagementTools*

```powershell
Install-windowsfeature -name AD-Domain-Services -IncludeManagementTools
```
Per disposar de les eines per instal·lar i configurar el domini
```powershell
Import-module addsdeployment
```
```powershell
Install-ADDSForest -DatabasePath "C:\Windows\NTDS" -LogPath "C:\Windows\NTDS" -SYSVOLPath "C:\Windows\SYSVOL" 
-DomainName "tfm.local" -DomainNetBIOSName "tfm" -ForestMode "Win2012" -InstallDNS:$true -NoRebootOnCompletion:$false -Force:$true
```
Ens demana reinciar per aplicar la nova configuració...Quan acabe ens deman iniciar sessió. Tornem a executar el PowerShell.

```PowerShell
Get-DnsServerSetting
```

<img width=60% src="../png/core/instalADDS.png">

<img width=60% src="../png/core/InstallAD1.png">

Per començar a fer algunes accions, ara ja carreguem el PowerShell

```cmd
PowerShell
```

Comprovem que el domini s'ha creat correctament.

```powershell
get-ADdomain
```

```powershell
(get-addomain).DNSRoot
```

```powershell
(get-addomain).name
```

Amb el *get-ADdomain* veiem tota la informació del domini, nom, nom net bios, contenidor de computadors...






