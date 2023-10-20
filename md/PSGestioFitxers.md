# POWERSHELL

## ACTIVITAT 1. DIRECTORIS I FITXERS.

Obri una sessió de PowerShell Màquina Virtual.Has d'usar els cmdLets per a fer les operacions amb fitxers i directoris que s'indiquen a continuació.

En cada apartat de l'activitat tens una llista dels *cmdLets* que calen per resoldre'l.

1.  Crea en PowerShell la següent estructura de directoris i fitxers:

CmdLets ajuda:

*   New-Item
*   Set-Location
*   Copy-Item
*   Rename-Item
*   Remove-Item
*   Move-Item
*   ...


```powershell
├── carpeta1
│   ├── f1
│   └── f3
├── carpeta2
├── f4
└── f2
```

2 directories, 4 file

2.  Situa't en la carpeta *carpeta2* i canvia els fitxers *f4, f2* a esta carpeta.

3.  Sense moure't de *carpeta2*, elimina el fitxer *f3*.

4.  Sense moure't de *carpeta2*, elimina *carpeta1*

5.  Copia *f2* com a *f5*

6.  Fes una còpia sencera de *carpeta2* com *carpeta3* 


## RESOLUCIÓ

1.  
```powershell 
PS C:\Users\Administrador\Documents>new-item -ItemType Directory carpeta1
...
PS C:\Users\Administrador\Documents> new-item -ItemType Directory carpeta2
...
PS C:\Users\Administrador\Documents> new-item carpeta1/f1
...
PS C:\Users\Administrador\Documents> new-item carpeta1/f3
PS C:\Users\Administrador\Documents> PS C:\Users\Administrador\Documents> PS C:\Users\Administrador\Documents> new-item f2, f4
```
2.  
```powershell 
PS C:\Users\Administrador\Documents> set-location carpeta2
PS C:\Users\Administrador\Documents\carpeta2> Move-Item ../f? .
PS C:\Users\Administrador\Documents\carpeta2> Get-ChildItem


    Directorio: C:\Users\Administrador\Documents\carpeta2


Mode                LastWriteTime         Length Name                                                                                                                    
----                -------------         ------ ----                                                                                                                    
-a----       05/05/2021      9:11              0 f2                                                                                                                      
-a----       05/05/2021      9:11              0 f4                                                                         
```
Comprovem
```powershell 
PS C:\Users\Administrador\Documents\carpeta2> Get-ChildItem


    Directorio: C:\Users\Administrador\Documents\carpeta2


Mode                LastWriteTime         Length Name                                                                                                                    
----                -------------         ------ ----                                                                                                                    
-a----       05/05/2021      9:11              0 f2                                                                                                                      
-a----       05/05/2021      9:11              0 f4                                                                         ```

PS C:\Users\Administrador\Documents\carpeta2> Get-ChildItem


    Directorio: C:\Users\Administrador\Documents\carpeta2


Mode                LastWriteTime         Length Name                                                                                                                    
----                -------------         ------ ----                                                                                                                    
-a----       05/05/2021      9:11              0 f2                                                                                                                      
-a----       05/05/2021      9:11              0 f4                                                                                                                      
```


3.  
```powershell 
PS C:\Users\Administrador\Documents\carpeta2> remove-item ..\carpeta1\f3
```
4.  
```powershell 
PS C:\Users\Administrador\Documents\carpeta2> remove-item -recurse ..\carpeta1\
```
5.  

```powershell 
PS C:\Users\Administrador\Documents\carpeta2> copy-item f2 f5
```
o indicant directori origen i destí...
```
PS C:\Users\Administrador\Documents\carpeta2> copy-item -path f2 -destination f5
```
6.  
```powershell 
PS C:\Users\Administrador\Documents\carpeta2> Copy-Item ..\carpeta2 ../carpeta3

PS C:\Users\Administrador\Documents\carpeta2> get-childItem ..


    Directorio: C:\Users\Administrador\Documents


Mode                LastWriteTime         Length Name                                                                                                                    
----                -------------         ------ ----                                                                                                                    
d-----       05/05/2021      9:29                carpeta2                                                                                                                
d-----       05/05/2021      9:37                carpeta3  
```
