# COMPARTICIÓ DE RECURSOS

>**Note** 
>
> És recomanable que cada canvi que es faça del del CLI (PowerShell) *cmdLets* es comprove amb *cmdLets* de tipus "Get-" però també des de l'entorn gràfic ( GUI de Windows).
> 
> 
## INFORMACIÓ SOBRE RECURSOS COMPARTITS 
### Obtenir tots els cmdLets relacionats amb la compartició de recursos.

```powershell
Get-Command *smbshare*
```

💻 Resultat
```
PS C:\WINDOWS\system32> Get-Command *smbshare*

CommandType     Name                                               Version    Source                                                                                                 
-----------     ----                                               -------    ------                                                                                                 
Function        Block-SmbShareAccess                               2.0.0.0    SmbShare                                                                                               
Function        Get-SmbShare                                       2.0.0.0    SmbShare                                                                                               
Function        Get-SmbShareAccess                                 2.0.0.0    SmbShare                                                                                               
Function        Grant-SmbShareAccess                               2.0.0.0    SmbShare                                                                                               
Function        New-SmbShare                                       2.0.0.0    SmbShare                                                                                               
Function        Remove-SmbShare                                    2.0.0.0    SmbShare                                                                                               
Function        Revoke-SmbShareAccess                              2.0.0.0    SmbShare                                                                                               
Function        Set-SmbShare                                       2.0.0.0    SmbShare                                                                                               
Function        Unblock-SmbShareAccess                             2.0.0.0    SmbShare                                                                   
```


### Consultar els recursos compartits

```powershell
Get-SmbShare
```
💻 Resultat
```

Name   ScopeName Path                              Description               
----   --------- ----                              -----------               
ADMIN$ *         C:\WINDOWS                        Admin remota              
C$     *         C:\                               Recurso predeterminado    
IPC$   *                                           IPC remota                
print$ *         C:\WINDOWS\system32\spool\drivers Controladores de impresora
Users  *         C:\Users                                                    
```
>:mag: **RECURSOS OCULTS**
>Hi ha recursos del sistema que, per defecte, no estan visibles. S'identifiquen perquè el nom duu **$** al final.
>Si volem compartir algun però que no es veja des de l'Explorador, només hem d'afegir el signe $ al final
>
#### Consultar els recursos que no són de sistema
```powershell
Get-SmbShare -Special $false
```
💻 Resultat
```
Name   ScopeName Path                              Description               
----   --------- ----                              -----------               
print$ *         C:\WINDOWS\system32\spool\drivers Controladores de impresora
Users  *         C:\Users                                                    
```

#### Consultar un recurs concret
```powershell
Get-SmbShare -name print$
```
💻 Resultat

```
Name   ScopeName Path                              Description               
----   --------- ----                              -----------               
print$ *         C:\WINDOWS\system32\spool\drivers Controladores de impresora
```

## COMPARTIR UNA CARPETA

### 1. CREEM LA CARPETA

```powershell
new-item -Type directory c:\DADES
```
   
💻 Resultat
```
Directory: C:\

Mode                 LastWriteTime         Length Name                                                                                                                               
----                 -------------         ------ ----                                                                                                                               
d-----          4/3/2021     17:44                DADES                                                                                                                              
```
### 2.  COMPARTIM AMB NOM
```powershell
New-SmbShare -path C:\DADES -Name DADES_COMP
```
💻 Resultat
```
Name       ScopeName Path     Description
----       --------- ----     -----------
DADES_COMP *         C:\DADES            
```
### 3.  COMPROVEM

```powershell
Get-SmbShare
```
💻 Resultat
```
Name       ScopeName Path                              Description               
----       --------- ----                              -----------               
ADMIN$     *         C:\WINDOWS                        Admin remota              
C$         *         C:\                               Recurso predeterminado    
DADES_COMP *         C:\DADES                                                    
IPC$       *                                           IPC remota                
print$     *         C:\WINDOWS\system32\spool\drivers Controladores de impresora
Users      *         C:\Users                                                    
```
#### Mirem els permisos de compartició que té

```powershell
Get-SmbShareAccess -Name DADES_COMP|fl
```
💻 Resultat
```
Name              : DADES_COMP
ScopeName         : *
AccountName       : Todos
AccessControlType : Allow
AccessRight       : Read
```
### 4.  CANVIEM PROPIETATS
```powershell

set-smbshare -name DADES_COMP -Description "dades a compartir" -force
```
💻 Exemple
```
PS C:\WINDOWS\system32> Get-SmbShare

Name       ScopeName Path                              Description               
----       --------- ----                              -----------               
ADMIN$     *         C:\WINDOWS                        Admin remota              
C$         *         C:\                               Recurso predeterminado    
DADES_COMP *         C:\DADES                          dades a compartir         
IPC$       *                                           IPC remota                
print$     *         C:\WINDOWS\system32\spool\drivers Controladores de impresora
Users      *         C:\Users                                                    
```

### Altra. Accessos concurrents.

```powershell
Get-SmbShare -name DADES_COMP|fl *
```
💻 Resultat
```
PresetPathAcl         : System.Security.AccessControl.DirectorySecurity
ShareState            : Online
AvailabilityType      : NonClustered
ShareType             : FileSystemDirectory
FolderEnumerationMode : Unrestricted
CachingMode           : Manual
LeasingMode           : Full
SmbInstance           : Default
CATimeout             : 0
ConcurrentUserLimit   : 0
ContinuouslyAvailable : False
CurrentUsers          : 0
Description           : dades a compartir
EncryptData           : False
IdentityRemoting      : False
Infrastructure        : False
Name                  : DADES_COMP
Path                  : C:\DADES
Scoped                : False
ScopeName             : *
SecurityDescriptor    : O:SYG:SYD:(A;;0x1200a9;;;WD)
ShadowCopy            : False
Special               : False
Temporary             : False
Volume                : \\?\Volume{19e2f4cb-7976-42e0-b276-caaee97495bc}\
PSComputerName        : 
CimClass              : ROOT/Microsoft/Windows/SMB:MSFT_SmbShare
CimInstanceProperties : {AvailabilityType, CachingMode, CATimeout, ConcurrentUserLimit...}
CimSystemProperties   : Microsoft.Management.Infrastructure.CimSystemProperties

```
```powershell
Set-SmbShare -name DADES_COMP -ConcurrentUserLimit 12 -force
```
### 5.  CANVI DE PERMISOS.

```powershell
Get-SmbShareAccess -name DADES_COMP
```
💻 Resultat
```
Name       ScopeName AccountName AccessControlType AccessRight
----       --------- ----------- ----------------- -----------
DADES_COMP *         Todos       Allow             Read       
```

#### Busquem algun usuari:
```powershell
Get-LocalUser
```
💻 Resultat
```
Name               Enabled Description                                                                                                             
----               ------- -----------                                                                                                             
Administrador      False   Cuenta integrada para la administración del equipo o dominio                                                            
DefaultAccount     False   Cuenta de usuario administrada por el sistema.                                                                          
diego              True                                                                                                                            
francesc           True                                                                                                                            
Invitado           False   Cuenta integrada para el acceso como invitado al equipo o dominio                                                       
joan               True    Cap d'equip                                                                                                             
JOSEP              True                                                                                                                            
pepa               True                                                                                                                            
pere               True                                                                                                                            
Tomàs              True                                                                                                                            
                                                                                                 
WDAGUtilityAccount False   Una cuenta de usuario que el sistema administra y usa para escenarios de Protección de aplicaciones de Windows Defender.

```
 
#### Donem accès total a un usuari

Usuari *joan* sobre el recurs compartit

```powershell
Grant-SmbShareAccess -Name DADES_COMP -AccountName joan -AccessRight full -force
```
💻 Resultat
```
Name       ScopeName AccountName     AccessControlType AccessRight
----       --------- -----------     ----------------- -----------
DADES_COMP *         Todos           Allow             Read       
DADES_COMP *         PORTATIL1\Tomàs Allow             Full       
DADES_COMP *         PORTATIL1\joan  Allow             Full       

```
⌨️ Fixa't amb tots els valors possibles de permisos de compartició i relaciona'ls amb les opcions que apareixen al GUI.
Revoquem els permisos
```powershell
Revoke-SmbShareAccess -Name DADES_COMP -AccountName joan -force
```
💻 Resultat
```
Name       ScopeName AccountName     AccessControlType AccessRight
----       --------- -----------     ----------------- -----------
DADES_COMP *         Todos           Allow             Read       
DADES_COMP *         PORTATIL1\Tomàs Allow             Full  
```
Deneguem expressament els permisos
```powershell
Block-SmbShareAccess -Name DADES_COMP -AccountName joan -force
```
💻 Resultat
```
Name       ScopeName AccountName     AccessControlType AccessRight
----       --------- -----------     ----------------- -----------
DADES_COMP *         PORTATIL1\joan  Deny              Full       
DADES_COMP *         Todos           Allow             Read       
DADES_COMP *         PORTATIL1\Tomàs Allow             Full 
```

#### Deixem de compartir el recurs
```powershell
Remove-SmbShare DADES_COMP -Force
```

Només si fa falta podem ja eliminar el recurs. Es tracta de l'eliminació de la carpeta !
Primer la busquem
```powershell
Get-ChildItem -Path c:\ -name DAD*
DADES
```
```powershell
PS C:\WINDOWS\system32> Remove-Item C:\dades
```
>**Note** 
> Recordeu que a Windows no es diferencien MAJÚSCULES de minúscules
> 

