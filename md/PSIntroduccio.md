# INTRODUCCIÓ 
## Ordres bàsiques
* **get-command** Per obtenir cmdLets
* **get-help** Per obtenir ajuda sobre algun cmdLet
* **get-alias** Per obtenir algun cmdLet a partir d'un alias que conegam
  
```powershell
get-command *domain*
```
```powershell
get-help get-ADDomain
```
Potser ens demane actualitzar l'ajuda amb 
```powershell
update-help
```
```powershell
get-alias ls
```
### Eixida
L'eixida del cmdLet pot ser
  * out-host    Pantalla, per defecte
  * out-file    Hem d'indicar el fitxer on volem guardar el resultat ( similar a ">" )
  * out-printer
  * out-null    Similar al > /dev/null de Linux
 

Amb el | podem indicar un format a l'eixida amb el  |
  * ft  format-table
  * fl  format-list
  * fw  format-wide

**Exemple:**

1.- Busquem el cmdLet que faça la funció del comandament Linux "ls" o "dir" de cmd.
  ```powershell

PS C:\Users\Administrador> get-alias ls

CommandType     Name                                               Version    Source                                                                                     
-----------     ----                                               -------    ------                                                                                     
Alias           ls -> Get-ChildItem                                                                                                                                   
        
```
2.- Una vegada l'obtenim, busquem l'ajuda (com el "man ls")

```powershell
PS C:\Users\Administrador> get-help Get-ChildItem

NOMBRE
    Get-ChildItem
    
SINOPSIS
    Gets the items and child items in one or more specified locations.
    
    
SINTAXIS
    Get-ChildItem [[-Filter] <System.String>] [-Attributes {Archive | Compressed | Device | Directory | Encrypted | Hidden | IntegrityStream | Normal | NoScrubData | 
    NotContentIndexed | Offline | ReadOnly | ReparsePoint | SparseFile | System | Temporary}] [-Depth <System.UInt32>] [-Directory] [-Exclude <System.String[]>] [-File] 
    [-Force] [-Hidden] [-Include <System.String[]>] -LiteralPath <System.String[]> [-Name] [-ReadOnly] [-Recurse] [-System] [-UseTransaction] [<CommonParameters>]
    
    Get-ChildItem [[-Path] <System.String[]>] [[-Filter] <System.String>] [-Attributes {Archive | Compressed | Device | Directory | Encrypted | Hidden | IntegrityStream 
    | Normal | NoScrubData | NotContentIndexed | Offline | ReadOnly | ReparsePoint | SparseFile | System | Temporary}] [-Depth <System.UInt32>] [-Directory] [-Exclude 
    <System.String[]>] [-File] [-Force] [-Hidden] [-Include <System.String[]>] [-Name] [-ReadOnly] [-Recurse] [-System] [-UseTransaction] [<CommonParameters>]
    
    
DESCRIPCIÓN
    The `Get-ChildItem` cmdlet gets the items in one or more specified locations. If the item is a container, it gets the items inside the container, known as child 
    items. You can use the Recurse parameter to get items in all child containers and use the Depth parameter to limit the number of levels to recurse.
    
    `Get-ChildItem` doesn't display empty directories. When a `Get-ChildItem` command includes the Depth or Recurse parameters, empty directories aren't included in the 
    output.
    
    Locations are exposed to `Get-ChildItem` by PowerShell providers. A location can be a file system directory, registry hive, or a certificate store. For more 
    information, see about_Providers (../Microsoft.PowerShell.Core/About/about_Providers.md).
    

VÍNCULOS RELACIONADOS
    Online Version: https://learn.microsoft.com/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-5.1&WT.mc_id=ps-gethelp
    about_Certificate_Provider 
    about_Providers 
    about_Quoting_Rules 
    about_Registry_Provider 
    ForEach-Object 
    Get-Alias 
    Get-Item 
    Get-Location 
    Get-Process 
    Get-PSProvider 
    Split-Path 

NOTAS
    Para ver los ejemplos, escriba: "get-help Get-ChildItem -examples".
    Para obtener más información, escriba: "get-help Get-ChildItem -detailed".
    Para obtener información técnica, escriba: "get-help Get-ChildItem -full".
    Para obtener ayuda disponible en línea, escriba: "get-help Get-ChildItem -online"
```
3.- Provem el cmdLet
```powershell
PS C:\Users\Administrador> Get-ChildItem c:\


    Directorio: C:\


Mode                LastWriteTime         Length Name                                                                                                                    
----                -------------         ------ ----                                                                                                                    
d-----       15/09/2018      9:19                PerfLogs                                                                                                                
d-r---       13/01/2023     10:00                Program Files                                                                                                           
d-----       13/01/2023      9:13                Program Files (x86)                                                                                                     
d-r---       13/01/2023      9:13                Users                                                                                                                   
d-----       13/01/2023     10:52                Windows  
 
```
## cmdLet son instàncies de classes (POO)
De forma similar al que passa amb el llenguatges de programació d'objectes, les propietats i mètodes apareixen facilitant-nos la tasca.
 
Veiem un exemple:

![cmdLet1](imatges/cmdLet1.png)

![cmdLet2](imatges/cmdLet2.png)

Compte! si formatem l'eixida ja no tenim estos avantatges.

![cmdLet3](imatges/cmdLet3.png)

![cmdLet4](imatges/cmdLet4.png)


## Actualització de l'ajuda
```powershell
Update-Help
```

Si ens dona l'error per no tindre el mòdul d'ajuda instal·lat, l'instal·lem:
```powershell
Install-Module -Name PowerShellGet -Force
```


