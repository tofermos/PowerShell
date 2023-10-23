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






