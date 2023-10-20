# INSTAL·LACIÓ I CONFIGURACIÓ BÀSICA D'*ACTIVE DIRECTORY* DES DE POWERSHELL

##  Instal·lació de Active Directory
Les eines d'administrador no venen per defecte.Per això afegim el *-IncludeManagementTools*

```powershell
Install-windowsfeature -name AD-Domain-Services -IncludeManagementTools
```

```powershell
Import-module addsdeployment
```
```powershell
Install-ADDSForest -DatabasePath "C:\Windows\NTDS" -LogPath "C:\Windows\NTDS" -SYSVOLPath "C:\Windows\SYSVOL" 
-DomainName "tfm.local" -DomainNetBIOSName "tfm" -ForestMode "Win2012" -InstallDNS:$true -NoRebootOnCompletion:$false -Force:$true
```

```powershell
Add-DnsServerPrimaryZone -Name "tfm.local" -ZoneFile "tfm.local"
```

```powershell
Set-DnsServerForwarder -IPAddress 192.168.1.100
```

```powershell
Add-DnsServerResourceRecordA -Name "www" -ZoneName "www.construirlasostenibilitat.net" -IPv4Address "192.168.1.1"
```
## CONFIGURACIÓ DELS SERVIDOR

```powershell
sconfig
```
<img width=60% src="../png/sconfig.png"></img>



