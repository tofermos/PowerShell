# DOMINI AMB UN WINDOWS SERVER CORE.
## INSTAL·LACIÓ DEL SERVIDOR

Hem de triar l'opció que no duu "experiencia escritorio"


En **VirtualBox** instal·lem el GuestAdditions. Insertant a ISO al CD i excutant l'exe.

<img width=60% src="../png/core/InstallGuestAdditions0.png">


<img width=60% src="../png/core/InstallGuestAdditions.png">


## CONFIGURACIÓ INICIAL.

Encara amb els cmdLets de PowerShell es poden fer totes les tasques de confifguració inicial, disposem d'una utilitat específica interactiva més pràctica: **sconfig**

<img width=60% src="../png/core/sconfig.png">

### Configuració de la NIC
<img width=60% src="../png/core/sconfig.png">

### Configuració d'actualitzacions automàtiques
<img width=60% src="../png/core/sconfig.png">

### Habilitar Escriptori remot

<img width=60% src="../png/core/sconfig.png">

## INSTAL·LACIÓ I CONFIGURACIÓ BÀSICA D'*ACTIVE DIRECTORY* DES DE POWERSHELL

Iniciem el PowerShell

```powershell
powershell
```

## Afegim el ROL de l'Active Directory

Les eines d'administrador no venen per defecte. Per això afegim el *-IncludeManagementTools*

```powershell
Install-windowsfeature -name AD-Domain-Services -IncludeManagementTools
```

<img width=60% src="../png/core/InstallAD1.png">

Per disposar de les eines per instal·lar i configurar el domini
```powershell
Import-module addsdeployment
```
## Creem el Domini

El nostre exemple és: tfm.local

```powershell
Install-ADDSForest -DatabasePath "C:\Windows\NTDS" -LogPath "C:\Windows\NTDS" -SYSVOLPath "C:\Windows\SYSVOL" 
-DomainName "tfm.local" -DomainNetBIOSName "tfm" -ForestMode "Win2012" -InstallDNS:$true -NoRebootOnCompletion:$false -Force:$true
```

<img width=60% src="../png/core/instalADDS.png">
Ens demana reinciar per aplicar la nova configuració...

Quan acabe ens demana iniciar sessió. Tornem a executar el PowerShell.

```PowerShell
Get-DnsServerSetting
```

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






