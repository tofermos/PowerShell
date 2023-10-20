# GESTIÓ DE SERVEIS

* Get-Service
    * RequiredServices
    * DependantServices
* Stop-Service
* Start-Service
* Set-Service


1.- Mostrar cmdLest relacionat amb servicis

```powershell
    PS C:\WINDOWS\system32> Get-Command *service*

CommandType     Name                                               Version    Source                                                                                                 
-----------     ----                                               -------    ------                                                                                                 
Function        Get-NetFirewallServiceFilter                       2.0.0.0    NetSecurity                                                                                            
Function        Set-NetFirewallServiceFilter                       2.0.0.0    NetSecurity                                                                                            
Cmdlet          Get-Service                                        3.1.0.0    Microsoft.PowerShell.Management                                                                        
Cmdlet          New-Service                                        3.1.0.0    Microsoft.PowerShell.Management                                                                        
Cmdlet          New-WebServiceProxy                                3.1.0.0    Microsoft.PowerShell.Management                                                                        
Cmdlet          Restart-Service                                    3.1.0.0    Microsoft.PowerShell.Management                                                                        
Cmdlet          Resume-Service                                     3.1.0.0    Microsoft.PowerShell.Management                                                                        
Cmdlet          Set-Service                                        3.1.0.0    Microsoft.PowerShell.Management                                                                        
Cmdlet          Start-Service                                      3.1.0.0    Microsoft.PowerShell.Management                                                                        
Cmdlet          Stop-Service                                       3.1.0.0    Microsoft.PowerShell.Management                                                                        
Cmdlet          Suspend-Service                                    3.1.0.0    Microsoft.PowerShell.Management                                                                        
Application     igfxCUIService.exe                                 6.15.10... C:\WINDOWS\system32\igfxCUIService.exe                                                                 
Application     SecurityHealthService.exe                          4.18.19... C:\WINDOWS\system32\SecurityHealthService.exe                                                          
Application     SensorDataService.exe                              10.0.19... C:\WINDOWS\system32\SensorDataService.exe                                                              
Application     services.exe                                       10.0.19... C:\WINDOWS\system32\services.exe                                                                       
Application     services.msc                                       0.0.0.0    C:\WINDOWS\system32\services.msc                                                                       
Application     TieringEngineService.exe                           10.0.19... C:\WINDOWS\system32\TieringEngineService.exe                                                           
Application     Windows.WARP.JITService.exe                        0.0.0.0    C:\WINDOWS\system32\Windows.WARP.JITService.exe     
```

2.- Consultar per una propietat
```
PS C:\WINDOWS\system32> Get-Service |where status -eq "Running"|ft -AutoSize

Status  Name                           DisplayName                                                           
------  ----                           -----------                                                           
Running AdobeARMservice                Adobe Acrobat Update Service                                          
Running Appinfo                        Información de la aplicación                                          
Running AppXSvc                        Servicio de implementación de AppX (AppXSVC)                          
Running AtherosSvc                     AtherosSvc                           
```
O també
```powershell
Get-Service |?{$_.Status -eq "Running"}

PS C:\WINDOWS\system32> Get-Service -name "spooler"

Status   Name               DisplayName                           
------   ----               -----------                           
Running  spooler            Cola de impresión                     


```
3.- Buscar servicis depenents

``` powershell 
PS C:\WINDOWS\system32> Get-Service -name "spooler"|fl *


Name                : spooler
RequiredServices    : {RPCSS, http}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
DisplayName         : Cola de impresión
DependentServices   : {Fax}
MachineName         : .
ServiceName         : spooler
ServicesDependedOn  : {RPCSS, http}
ServiceHandle       : SafeServiceHandle
Status              : Running
ServiceType         : Win32OwnProcess, InteractiveProcess
StartType           : Automatic
Site                : 
Container           : 


PS C:\WINDOWS\system32> (Get-Service -name "spooler").RequiredServices

Status   Name               DisplayName                           
------   ----               -----------                           
Running  RPCSS              Llamada a procedimiento remoto (RPC)  
Running  http               Servicio HTTP                         


PS C:\WINDOWS\system32> (Get-Service -name "spooler").RequiredServices|where name -eq "http"

Status   Name               DisplayName                           
------   ----               -----------                           
Running  http               Servicio HTTP                         
```

4.- Detindre i iniciar un servici

```powershell
PS C:\WINDOWS\system32> Get-Service -name Spooler

Status   Name               DisplayName                           
------   ----               -----------                           
Running  Spooler            Cola de impresión                     



PS C:\WINDOWS\system32> stop-Service -name Spooler

PS C:\WINDOWS\system32> Get-Service -name Spooler

Status   Name               DisplayName                           
------   ----               -----------                           
Stopped  Spooler            Cola de impresión                     


PS C:\WINDOWS\system32> start-Service -name Spooler
```
5.- Modificar propietats

```powershell
PS C:\WINDOWS\system32> Get-Service -Name spooler|fl *

Name                : spooler
RequiredServices    : {RPCSS, http}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
DisplayName         : Cola de impresión
DependentServices   : {Fax}
MachineName         : .
ServiceName         : spooler
ServicesDependedOn  : {RPCSS, http}
ServiceHandle       : SafeServiceHandle
Status              : Running
ServiceType         : Win32OwnProcess, InteractiveProcess
StartType           : Automatic
Site                : 
Container           : 

```
Modifiquem la propietat

```powershell
PS C:\WINDOWS\system32> Set-Service -Name spooler -StartupType Manual

PS C:\WINDOWS\system32> Get-Service -Name spooler|fl *


Name                : spooler
RequiredServices    : {RPCSS, http}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
DisplayName         : Cola de impresión
DependentServices   : {Fax}
MachineName         : .
ServiceName         : spooler
ServicesDependedOn  : {RPCSS, http}
ServiceHandle       : SafeServiceHandle
Status              : Running
ServiceType         : Win32OwnProcess, InteractiveProcess
StartType           : Manual
Site                : 
Container           : 

```

6.- Dependències entre servicis. 

Exemple:

El servici de Fax necessita el servici spooler

El servici Spooler necessita els servicis HTTP i RPC

```powershell
PS C:\WINDOWS\system32> Get-Service -Name spooler -DependentServices

Status   Name               DisplayName                           
------   ----               -----------                           
Stopped  Fax                Fax                                   



PS C:\WINDOWS\system32> Get-Service -Name spooler -requiredServices

Status   Name               DisplayName                           
------   ----               -----------                           
Running  RPCSS              Llamada a procedimiento remoto (RPC)  
Running  http               Servicio HTTP                         
```


  