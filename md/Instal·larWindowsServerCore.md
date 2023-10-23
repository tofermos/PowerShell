# INSTAL·LACIÓ DEL WINDOWS SERVER CORE.

Hem de triar l'opció que no duu "experiencia escritorio"

<img width=60% src="../png/core/Instal1.png">

<img width=60% src="../png/core/Install2.png">

En **VirtualBox** instal·lem el GuestAdditions. Insertant a ISO al CD i excutant l'exe.

<img width=60% src="../png/core/InstallGuestAdditions0.png">


<img width=60% src="../png/core/InstallGuestAdditions.png">PS C:\Users\Administrador> get-addomain

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

<img width=60% src="../png/core/installAD1.png">

Per començar a fer algunes accions, ara ja carreguem el PowerShell

```cmd
PpowerShell
```

Comprovem que el domini s'ha creat correctament.

PS C:\Users\Administrador> (get-addomain).DNSRoot
tfm.local
PS C:\Users\Administrador> (get-addomain).name
tfm

<img width=60% src="../png/core/sconfig.png">

```
PS C:\Users\Administrador> get-addomain


AllowedDNSSuffixes                 : {}
ChildDomains                       : {}
ComputersContainer                 : CN=Computers,DC=tfm,DC=local
DeletedObjectsContainer            : CN=Deleted Objects,DC=tfm,DC=local
DistinguishedName                  : DC=tfm,DC=local
DNSRoot                            : tfm.local
DomainControllersContainer         : OU=Domain Controllers,DC=tfm,DC=local
DomainMode                         : Windows2012Domain
DomainSID                          : S-1-5-21-2020013004-4000729229-4201603488
ForeignSecurityPrincipalsContainer : CN=ForeignSecurityPrincipals,DC=tfm,DC=local
Forest                             : tfm.local
InfrastructureMaster               : WINSERVCORE1.tfm.local
LastLogonReplicationInterval       :
LinkedGroupPolicyObjects           : {CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=System,DC=tfm,DC=local}
LostAndFoundContainer              : CN=LostAndFound,DC=tfm,DC=local
ManagedBy                          :
Name                               : tfm
NetBIOSName                        : tfm
ObjectClass                        : domainDNS
ObjectGUID                         : 64a5f901-b978-47ea-a3d6-a3865cdb4017
ParentDomain                       :
PDCEmulator                        : WINSERVCORE1.tfm.local
PublicKeyRequiredPasswordRolling   :
QuotasContainer                    : CN=NTDS Quotas,DC=tfm,DC=local
ReadOnlyReplicaDirectoryServers    : {}
ReplicaDirectoryServers            : {WINSERVCORE1.tfm.local}
RIDMaster                          : WINSERVCORE1.tfm.local
SubordinateReferences              : {DC=ForestDnsZones,DC=tfm,DC=local, DC=DomainDnsZones,DC=tfm,DC=local,
                                     CN=Configuration,DC=tfm,DC=local}
SystemsContainer                   : CN=System,DC=tfm,DC=local
UsersContainer                     : CN=Users,DC=tfm,DC=local

```
```powershell

PS C:\Users\Administrador> (get-addomain).DNSRoot
tfm.local
```
```powershell
PS C:\Users\Administrador> (get-addomain).name
tfm
```





