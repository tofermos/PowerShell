# GESTIÓ D'EVENTS

🗃️ [INDEX POWERSHELL](README.md)
## Eventlog

```powershell
PS C:\WINDOWS\system32> get-command *eventlog*

CommandType     Name                                               Version    Source                                                                                                 
-----------     ----                                               -------    ------                                                                                                 
Cmdlet          Clear-EventLog                                     3.1.0.0    Microsoft.PowerShell.Management                                                                        
Cmdlet          Get-EventLog                                       3.1.0.0    Microsoft.PowerShell.Management                                                                        
Cmdlet          Limit-EventLog                                     3.1.0.0    Microsoft.PowerShell.Management                                                                        
Cmdlet          New-EventLog                                       3.1.0.0    Microsoft.PowerShell.Management                                                                        
Cmdlet          Remove-EventLog                                    3.1.0.0    Microsoft.PowerShell.Management                                                                        
Cmdlet          Show-EventLog                                      3.1.0.0    Microsoft.PowerShell.Management                                                                        
Cmdlet          Write-EventLog                                     3.1.0.0    Microsoft.PowerShell.Management   
```


Veiem quins tipus de event ens mostra *Get-EventLog* i quantes entrades ja tenim:
```powershell
PS C:\WINDOWS\system32> Get-EventLog -list

  Max(K) Retain OverflowAction        Entries Log                                                                                                                                    
  ------ ------ --------------        ------- ---                                                                                                                                    
  20.480      0 OverwriteAsNeeded      17.673 Application                                                                                                                            
  20.480      0 OverwriteAsNeeded           0 HardwareEvents                                                                                                                         
     512      7 OverwriteOlder              0 Internet Explorer                                                                                                                      
  20.480      0 OverwriteAsNeeded           0 Key Management Service                                                                                                                 
     128      0 OverwriteAsNeeded         234 OAlerts                                                                                                                                
   5.056      7 OverwriteOlder              0 PRTG Network Monitor                                                                                                                   
     512      7 OverwriteOlder              0 Reason                                                                                                                                 
  20.480      0 OverwriteAsNeeded      30.508 Security                                                                                                                               
  20.480      0 OverwriteAsNeeded      14.783 System                                                                                                                                 
  15.360      0 OverwriteAsNeeded       1.581 Windows PowerShell

```

**InstanceID** indica el tipus d'event.

**Index** indica el registre.

**logname**

*  System
*  Application
*  Hardware Events
*  Internet Explorer
*  Key Mangement Services
*  OAlerts
*  Secutirty
*  Reason
  

```powershell
PS C:\Windows\system32> get-eventlog -LogName system -Newest 1

   Index Time          EntryType   Source                 InstanceID Message                                                                                                                                                                   
   ----- ----          ---------   ------                 ---------- -------                                                                                                                                                                   
    3014 may. 26 19:24 Information Microsoft-Windows...          105 No se encontró la descripción del id. de evento '105' en el origen 'Microsoft-Windows-Kernel-Power'. Es posible que el equipo local no tenga la información de registro...



PS C:\Windows\system32> get-winevent -LogName system -maxevent 1


   ProviderName: Microsoft-Windows-Kernel-Power

TimeCreated                     Id LevelDisplayName Message                                                                                                                                                                                    
-----------                     -- ---------------- -------                                                                                                                                                                                    
26/05/2021 19:24:54            105 Información      Cambio de fuente de energía.                           
```

Podem filtrar entre dates ...
 
 ```powershell

 PS C:\WINDOWS\system32> get-eventlog -LogName System -EntryType error -after 01/01/2021 -before 10/01/2021 |Measure-Object


Count    : 17
Average  : 
Sum      : 
Maximum  : 
Minimum  : 
Property : 


PS C:\WINDOWS\system32> $err=(get-eventlog -LogName System -EntryType error -after 01/01/2021 -before 10/01/2021 |Measure-Object).count

PS C:\WINDOWS\system32> $err
17
```






Exemples:
```
PS C:\WINDOWS\system32> (get-EventLog -LogName system -Source disk -after 01/01/2021 -before $(date)).Index
16720
13101
13100
13099
13098
13097
13096
....
```

Continuem...
```

PS C:\WINDOWS\system32> (get-EventLog -LogName system -Source disk -after 01/01/2021 -before $(date)).count
156

PS C:\WINDOWS\system32> $llista=(get-EventLog -LogName system -Source disk -after 01/01/2021 -before $(date)).Index

```
Un detall. Les "llistes" passen a ser vectors automàticament.

```
PS C:\WINDOWS\system32> $llista[3]
13099
```
## Get-WinEvent
``` 
PS C:\WINDOWS\system32> Get-Command *winevent*

CommandType     Name                                               Version    Source                                            
-----------     ----                                               -------    ------                                            
Cmdlet          Get-WinEvent                                       3.0.0.0    Microsoft.PowerShell.Diagnostics                  
Cmdlet          New-WinEvent                                       3.0.0.0    Microsoft.PowerShell.Diagnostics
```

```
PS C:\WINDOWS\system32> Get-WinEvent -ListLog *

LogMode   MaximumSizeInBytes RecordCount LogName                                                                                
-------   ------------------ ----------- -------                                                                                
Circular            15728640        1762 Windows PowerShell                                                                     
Circular            20971520       16763 System                                                                                 
Circular            20971520       29854 Security                                                                               
Circular             1052672           0 Reason                                                                                 
Circular             5177344           0 PRTG Network Monitor                                                                   
Circular             1052672         239 OAlerts                                                                                
Circular            20971520           0 Key Management Service                                                                 
Circular             1052672           0 Internet Explorer                                                                      
Circular            20971520           0 HardwareEvents                                                                         
Circular            20971520       19072 Application              
```
```powershell
PS C:\WINDOWS\system32> (Get-WinEvent -ListLog *).count
424
```
Comptar els registres
```powershell

PS C:\WINDOWS\system32> Get-WinEvent -ListLog * |Where-Object {$_.recordcount -gt 0 }

LogMode   MaximumSizeInBytes RecordCount LogName                                                                                
-------   ------------------ ----------- -------                                                                                
Circular            15728640        1762 Windows PowerShell                                                                     
Circular            20971520       16763 System                                                                                 
Circular            20971520       29854 Security                                                                               
Circular             1052672         239 OAlerts                                                                                
Circular            20971520       19072 Application                                                                            
Circular             1052672          79 Setup                                                                                  
Circular             1052672           4 Microsoft-Windows-WPD-MTPClassDriver/Operational                                       
Circular             1052672        1823 Microsoft-Windows-WMI-Activity/Operational                                             
Circular             1052672        1341 Microsoft-Windows-WLAN-AutoConfig/Operational
```
## Comparativa. Get-EventLog i Get-WinEvent. 

Comprovarem amb el darrer identificador d'event. Obervem que amb  *Get-EventLog* no ens diu de què es tracta i, en canvi, en *Get-WinEvent* sí que ens ho detalla.


```powershell
PS C:\WINDOWS\system32> Get-Eventlog -LogName system -Newest 5

   Index Time          EntryType   Source                 InstanceID Message                                                    
   ----- ----          ---------   ------                 ---------- -------                                                    
   16763 març 13 00:31 Warning     DCOM                        10016 No se encontró la descripción del id. de evento '10016' ...
   16762 març 13 00:01 Information Microsoft-Windows...          105 No se encontró la descripción del id. de evento '105' en...
   16761 març 13 00:01 Information Microsoft-Windows...          105 No se encontró la descripción del id. de evento '105' en...
   16760 març 13 00:00 Information Microsoft-Windows...          105 No se encontró la descripción del id. de evento '105' en...
   16759 març 13 00:00 Information Microsoft-Windows...          105 No se encontró la descripción del id. de evento '105' en...

```
Ara els busquem amb Get-WinEvent
```powershell

PS C:\WINDOWS\system32> Get-WinEvent -LogName system -MaxEvents 5


   ProviderName: Microsoft-Windows-DistributedCOM

TimeCreated                      Id LevelDisplayName Message                                                                    
-----------                      -- ---------------- -------                                                                    
13/3/2021 0:31:23             10016 Advertencia      La configuración de permisos específico de la aplicación no concede el p...


   ProviderName: Microsoft-Windows-Kernel-Power

TimeCreated                      Id LevelDisplayName Message                                                                    
-----------                      -- ---------------- -------                                                                    
13/3/2021 0:01:48               105 Información      Cambio de fuente de energía.                                               
13/3/2021 0:01:28               105 Información      Cambio de fuente de energía.                                               
13/3/2021 0:00:49               105 Información      Cambio de fuente de energía.                                               
13/3/2021 0:00:37               105 Información      Cambio de fuente de energía.                                               

```
```powershell
PS C:\WINDOWS\system32> Get-WinEvent -FilterHashtable @{
logname='system'
providername='disk'
level=2
}


   ProviderName: disk

TimeCreated                      Id LevelDisplayName Message                                                                    
-----------                      -- ---------------- -------                                                                    
12/3/2021 15:36:10               11 Error            El controlador detectó un error de controladora en \Device\Harddisk1\DR2.  
20/12/2020 19:56:20              11 Error            El controlador detectó un error de controladora en \Device\Harddisk1\DR1.  

```
