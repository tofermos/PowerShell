# PROCESSOS

1.- Obtenir informació de tots el cmdlets

```powershell

PS C:\WINDOWS\system32> Get-Command *process*|ft -autosize

CommandType Name                              Version Source                         
----------- ----                              ------- ------                         
Cmdlet      ConvertTo-ProcessMitigationPolicy 1.0.12  ProcessMitigations             
Cmdlet      Debug-Process                     3.1.0.0 Microsoft.PowerShell.Management
Cmdlet      Enter-PSHostProcess               3.0.0.0 Microsoft.PowerShell.Core      
Cmdlet      Exit-PSHostProcess                3.0.0.0 Microsoft.PowerShell.Core      
Cmdlet      Get-Process                       3.1.0.0 Microsoft.PowerShell.Management
Cmdlet      Get-ProcessMitigation             1.0.12  ProcessMitigations             
Cmdlet      Get-PSHostProcessInfo             3.0.0.0 Microsoft.PowerShell.Core      
Cmdlet      Set-ProcessMitigation             1.0.12  ProcessMitigations             
Cmdlet      Start-Process                     3.1.0.0 Microsoft.PowerShell.Management
Cmdlet      Stop-Process                      3.1.0.0 Microsoft.PowerShell.Management
Cmdlet      Wait-Process                      3.1.0.0 Microsoft.PowerShell.Management

```
2.- Obtenir informació sobre processos.
```powershell
PS C:\WINDOWS\system32> Get-Process -name firefox 

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName                                                                                                                
-------  ------    -----      -----     ------     --  -- -----------                                                                                                                
    418      29    26240      36456       0,16   2768   5 firefox                                                                                                                    
    478      34    29600      55404       0,33   8824   5 firefox                                                                                                                    
    470      36    42236      64656       0,53   9236   5 firefox                                                                                                                    
    619      34   172232     188108       1,67  10500   5 firefox                                                                                                                    
    440      31    28212      47988       0,23  12412   5 firefox                                                                                                                    
   1291      91   158804     218724       6,27  12904   5 firefox                                                                                                                    



PS C:\WINDOWS\system32> 
PS C:\WINDOWS\system32> Get-Process -name firefox |fl


Id      : 2768
Handles : 423
CPU     : 0,1875
SI      : 5
Name    : firefox

Id      : 8824
Handles : 471
CPU     : 0,34375
SI      : 5
Name    : firefox
```

Propietats dels processos.
```powershell
PS C:\WINDOWS\system32> Get-Process -name notepad |fl path


Path : C:\WINDOWS\system32\notepad.exe

PS C:\WINDOWS\system32> (Get-Process -name notepad).Path
C:\WINDOWS\system32\notepad.exe
PS C:\WINDOWS\system32> get-process -name notepad |ft ws
      WS
      --
15110144


PS C:\WINDOWS\system32> (get-process -name notepad).ws/1mb
14,41015625

PS C:\WINDOWS\system32> 
```

4.- Matar els pocessos.


```powershell
PS C:\Users\tomasw> Get-Process -Name *note*

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName                                                                                                    
-------  ------    -----      -----     ------     --  -- -----------                                                                                                    
    223       7     2380      15548       0,06   2620   1 notepad                                                                                                        
    222       7     2372      15648       0,08   3128   1 notepad                                                                                                        



PS C:\Users\tomasw> Stop-Process -Id 2620

PS C:\Users\tomasw> Get-Process -Name *note*

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName                                                                                                    
-------  ------    -----      -----     ------     --  -- -----------                                                                                                    
    223       7     2372      15648       0,08   3128   1 notepad                                                                                                        


------

```




5.- Iniciant els processos.
```
PS C:\WINDOWS\system32> Start-Process -FilePath c:\windows\notepad.exe

PS C:\WINDOWS\system32> Get-Process -Name notepa*

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName                                                                                                                
-------  ------    -----      -----     ------     --  -- -----------                                                                                                                
    234      13     2688      14756       0,11   7784   5 notepad  7
```


P6.- Obrir aplicacions de MS-Windows. 
    Encara que tenen associat un EXE, han d'obrir-se mitjançant un protocol.


```
PS C:\WINDOWS\system32> start-process Microsoft-edge://

PS C:\WINDOWS\system32> start-process MS-clock://

PS C:\WINDOWS\system32> start-process MS-windows-store://
```