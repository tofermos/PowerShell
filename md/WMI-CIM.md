# WMI/CIM

**CMI Common Information Model**

Estandard per intercanviar informació entre sistemes
  
**WMI Windows Management Instrumentation**

Implementació que ha fet MS per a Windows.

Els CmdLets de CMI estan substituint els de WMI

La informació està en una BD i la llegim amb CmdLets Per obtenir informació de:
* UEFI /BIOS
* RAM
* CPU
* Sistema Operatiu
* Discos
* ...

## WMI Windows Management Instrumentation
```
PS C:\WINDOWS\system32> Get-Command *WMI*

CommandType     Name                                               Version    Source                                            
-----------     ----                                               -------    ------                                            
Alias           gwmi -> Get-WmiObject                                                                                           
Alias           iwmi -> Invoke-WmiMethod                                                                                        
Alias           rwmi -> Remove-WmiObject                                                                                        
Alias           swmi -> Set-WmiInstance                                                                                         
Cmdlet          Get-WmiObject                                      3.1.0.0    Microsoft.PowerShell.Management                   
Cmdlet          Invoke-WmiMethod                                   3.1.0.0    Microsoft.PowerShell.Management                   
Cmdlet          Register-WmiEvent                                  3.1.0.0    Microsoft.PowerShell.Management                   
Cmdlet          Remove-WmiObject                                   3.1.0.0    Microsoft.PowerShell.Management                   
Cmdlet          Set-WmiInstance                                    3.1.0.0    Microsoft.PowerShell.Management                   
Application     WMIADAP.exe                                        10.0.19... C:\WINDOWS\System32\Wbem\WMIADAP.exe              
Application     WmiApSrv.exe                                       10.0.19... C:\WINDOWS\System32\Wbem\WmiApSrv.exe             
Application     WMIC.exe                                           10.0.19... C:\WINDOWS\System32\Wbem\WMIC.exe                 
Application     WmiMgmt.msc                                        0.0.0.0    C:\WINDOWS\system32\WmiMgmt.msc                   
Application     WmiPrvSE.exe                                       10.0.19... C:\WINDOWS\System32\Wbem\WmiPrvSE.exe   
```
 
 ### Get-WmiObject
 Ens centrem en el que ens dóna informació sobre el sistema. 
 
 1. **Investiguem com obtenir sobre la BIOS**

 ```
 PS C:\WINDOWS\system32> Get-WmiObject -list |where-object {$_.name -like "*BIOS*"}


   NameSpace: ROOT\cimv2

Name                                Methods              Properties                                                             
----                                -------              ----------                                                             
Win32_SMBIOSMemory                  {SetPowerState, R... {Access, AdditionalErrorData, Availability, BlockSize...}              
CIM_BIOSElement                     {}                   {BuildNumber, Caption, CodeSet, Description...}                        
Win32_BIOS                          {}                   {BiosCharacteristics, BIOSVersion, BuildNumber, Caption...}            
CIM_VideoBIOSElement                {}                   {BuildNumber, Caption, CodeSet, Description...}                        
CIM_VideoBIOSFeature                {}                   {Caption, CharacteristicDescriptions, Characteristics, Description...} 
CIM_BIOSFeature                     {}                   {Caption, CharacteristicDescriptions, Characteristics, Description...} 
CIM_BIOSLoadedInNV                  {}                   {Antecedent, Dependent, EndingAddress, StartingAddress}                
Win32_SystemBIOS                    {}                   {GroupComponent, PartComponent}                                        
CIM_VideoBIOSFeatureVideoBIOSEle... {}                   {GroupComponent, PartComponent}                                        
CIM_BIOSFeatureBIOSElements         {}                   {GroupComponent, PartComponent}                                        

```

```
PS C:\WINDOWS\system32> Get-WmiObject -class Win32_BIOS


SMBIOSBIOSVersion : X555LJ.504
Manufacturer      : American Megatrends Inc.
Name              : X555LJ.504
SerialNumber      : FAN0CV62483943E     
Version           : _ASUS_ - 1072009
```

Si afegir el _| fl *_ obtenim més informació

2.- **Obtenim informació sobre la CPU**

```
PS C:\WINDOWS\system32> Get-WmiObject -class Win32_processor


Caption           : Intel64 Family 6 Model 61 Stepping 4
DeviceID          : CPU0
Manufacturer      : GenuineIntel
MaxClockSpeed     : 2397
Name              : Intel(R) Core(TM) i7-5500U CPU @ 2.40GHz
SocketDesignation : SOCKET 0
```

Amb el _|fl_ obtenim més detalls

```

PS C:\WINDOWS\system32> Get-WmiObject -class Win32_processor|fl numberofcores, numberoflogicalprocessors


numberofcores             : 2
numberoflogicalprocessors : 4

```

