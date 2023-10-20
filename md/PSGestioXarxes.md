# POWERSHELL

## GESTIÓ DE LA XARXA

## Adaptadors de xarxa
* Get-NetAdapter
* Disable-NetAdapter
* Enable-NetAdapter 


Per poder vore cmdLets relacionats amb adaptadors de xarxa:
```powershell
PS C:\WINDOWS\system32> Get-Command -Module netadapter
                                                                                      
```

Provem el cmd que ens dóna informació sobre adpatadors existents

```powershell
PS C:\WINDOWS\system32> Get-NetAdapter

Name                      InterfaceDescription                    ifIndex Status       MacAddress             LinkSpeed
----                      --------------------                    ------- ------       ----------             ---------
Wi-Fi                     Qualcomm Atheros AR956x Wireless Net...      24 Up           80-A5-89-3B-C5-1B       150 Mbps
Ethernet 5                VirtualBox Host-Only Ethernet Adapter        17 Up           0A-00-27-00-00-11         1 Gbps
Ethernet                  Realtek PCIe GBE Family Controller           12 Disconnected 2C-56-DC-04-61-7A          0 bps
Conexión de red Bluetooth Bluetooth PAN HelpText                       10 Disconnected 80-A5-89-3B-C5-1A         3 Mbps
```

Busquem informació sobre un adaptador en concret

```powershell
PS C:\WINDOWS\system32> get-netadapter -name "Wi-Fi"|fl


Name                       : Wi-Fi
InterfaceDescription       : Qualcomm Atheros AR956x Wireless Network Adapter
InterfaceIndex             : 24
MacAddress                 : 80-A5-89-3B-C5-1B
MediaType                  : Native 802.11
PhysicalMediaType          : Native 802.11
InterfaceOperationalStatus : Down
AdminStatus                : Up
LinkSpeed(Mbps)            : 0
MediaConnectionState       : Disconnected
ConnectorPresent           : True
DriverInformation          : Driver Date 2016-03-26 Version 3.0.2.201 NDIS 6.40```

```
Desactivem i tornem a activar un adaptador.

```powershell
PS C:\WINDOWS\system32> disable-netadapter "Ethernet 5"
PS C:\WINDOWS\system32> get-netadapter "Ethernet 5"|fl

Name                       : Ethernet 5
InterfaceDescription       : VirtualBox Host-Only Ethernet Adapter
InterfaceIndex             : 17
MacAddress                 : 0A-00-27-00-00-11
MediaType                  : 802.3
PhysicalMediaType          : 802.3
InterfaceOperationalStatus : Down
AdminStatus                : Down
LinkSpeed(Gbps)            : 1
MediaConnectionState       : Unknown
ConnectorPresent           : False
DriverInformation          : Driver Date 2020-09-04 Version 6.1.14.40239 NDIS 6.0


PS C:\WINDOWS\system32> enable-netadapter "Ethernet 5"
PS C:\WINDOWS\system32> get-netadapter "Ethernet 5"|fl

Name                       : Ethernet 5
InterfaceDescription       : VirtualBox Host-Only Ethernet Adapter
InterfaceIndex             : 17
MacAddress                 : 0A-00-27-00-00-11
MediaType                  : 802.3
PhysicalMediaType          : 802.3
InterfaceOperationalStatus : Up
AdminStatus                : Up
LinkSpeed(Gbps)            : 1
MediaConnectionState       : Connected
ConnectorPresent           : False
DriverInformation          : Driver Date 2020-09-04 Version 6.1.14.40239 NDIS 6.0 
```

### Configuració de xarxes

Obtenir la informació de IP's 
* Get-NetIPAddress o gip
* Remove-NetIPAddress
  
Informació d'un en concret
```PS C:\WINDOWS\system32> Get-NetIPAddress -InterfaceAlias Wi-Fi

IPAddress         : fe80::54f6:ac90:2aa1:7284%24
InterfaceIndex    : 24
InterfaceAlias    : Wi-Fi
AddressFamily     : IPv6
Type              : Unicast
PrefixLength      : 64
PrefixOrigin      : WellKnown
SuffixOrigin      : Link
AddressState      : Preferred
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : 192.168.1.130
InterfaceIndex    : 24
InterfaceAlias    : Wi-Fi
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24
PrefixOrigin      : Dhcp
SuffixOrigin      : Dhcp
AddressState      : Preferred
ValidLifetime     : 2.23:39:39
PreferredLifetime : 2.23:39:39
SkipAsSource      : False
PolicyStore       : ActiveStore ```

```
Averiguar dades més concretes:

```powershell
PS C:\WINDOWS\system32> Get-NetIPAddress -InterfaceAlias "Wi-fi" -addressfamily "IPv4"

IPAddress         : 192.168.1.130
InterfaceIndex    : 24
InterfaceAlias    : Wi-Fi
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24
PrefixOrigin      : Dhcp
SuffixOrigin      : Dhcp
AddressState      : Preferred
ValidLifetime     : 2.23:35:46
PreferredLifetime : 2.23:35:46
SkipAsSource      : False
PolicyStore       : ActiveStore


PS C:\WINDOWS\system32> (Get-NetIPAddress -InterfaceAlias "Wi-fi" -addressfamily "IPv4").IPv4Address
192.168.1.130
```

Assignar una IP

```powershell
Remove-NetIPAddress -InterfaceAlias "Wi-fi" -Confirm:$false
```

## Taula d'enrutament

* Obtenir la porta d'enllaç
  
```powershell

PS C:\WINDOWS\system32> get-netroute -interfacealias "Wi-fi" -addressfamily "IPv4"

ifIndex DestinationPrefix                              NextHop                                  RouteMetric ifMetric PolicyStore
------- -----------------                              -------                                  ----------- -------- -----------
24      255.255.255.255/32                             0.0.0.0                                          256 50       ActiveStore
24      224.0.0.0/4                                    0.0.0.0                                          256 50       ActiveStore
24      192.168.1.255/32                               0.0.0.0                                          256 50       ActiveStore
24      192.168.1.130/32                               0.0.0.0                                          256 50       ActiveStore
24      192.168.1.0/24                                 0.0.0.0                                          256 50       ActiveStore
24      0.0.0.0/0                                      192.168.1.1                                        0 50       ActiveStore
```

```powershell
PS C:\WINDOWS\system32> get-netadapter

Name                      InterfaceDescription                    ifIndex Status       MacAddress             LinkSpeed
----                      --------------------                    ------- ------       ----------             ---------
Wi-Fi                     Qualcomm Atheros AR956x Wireless Net...      24 Up           80-A5-89-3B-C5-1B       150 Mbps
Ethernet 5                VirtualBox Host-Only Ethernet Adapter        17 Up           0A-00-27-00-00-11         1 Gbps
Ethernet                  Realtek PCIe GBE Family Controller           12 Disconnected 2C-56-DC-04-61-7A          0 bps
Conexión de red Bluetooth Bluetooth PAN HelpText                       10 Disabled     80-A5-89-3B-C5-1A         3 Mbps


PS C:\WINDOWS\system32> Get-NetRoute -ifIndex 24 -AddressFamily "IPv4"

ifIndex DestinationPrefix                              NextHop                                  RouteMetric ifMetric PolicyStore
------- -----------------                              -------                                  ----------- -------- -----------
24      255.255.255.255/32                             0.0.0.0                                          256 50       ActiveStore
24      224.0.0.0/4                                    0.0.0.0                                          256 50       ActiveStore
24      192.168.1.255/32                               0.0.0.0                                          256 50       ActiveStore
24      192.168.1.130/32                               0.0.0.0                                          256 50       ActiveStore
24      192.168.1.0/24                                 0.0.0.0                                          256 50       ActiveStore
24      0.0.0.0/0                                      192.168.1.1                                        0 50       ActiveStore

```

```powershell
PS C:\WINDOWS\system32> Get-NetRoute -ifIndex 24 -AddressFamily "IPv4"|ft nexthop, DestinationAddress

nexthop     DestinationAddress
-------     ------------------
0.0.0.0
0.0.0.0
0.0.0.0
0.0.0.0
0.0.0.0
192.168.1.1
```

### DNS

* Averiguar la IP del DNS

```PS C:\WINDOWS\system32> Get-DnsClientServerAddress -interfacealias "wi-fi"

InterfaceAlias               Interface Address ServerAddresses
                             Index     Family
--------------               --------- ------- ---------------
Wi-Fi                               24 IPv4    {192.168.1.1}
Wi-Fi                               24 IPv6    {}

  ``` 

Resoldre un nom. Comprovem si els DNS estan resolvent.
```
PS C:\WINDOWS\system32> Resolve-DnsName www.construirlasostenibilitat.net

Name                                           Type   TTL   Section    IPAddress
----                                           ----   ---   -------    ---------
www.construirlasostenibilitat.net              AAAA   3600  Answer     2001:8d8:100f:f000::23c
www.construirlasostenibilitat.net              A      3600  Answer     217.160.0.205
```

Mirem la cache.
```powershell
PS C:\WINDOWS\system32> get-dnsclientCache

Entry                     RecordName                Record Status    Section TimeTo Data L Data
                                                    Type                     Live   ength
-----                     ----------                ------ ------    ------- ------ ------ ----
wwis-dubc1-vip60.adobe...                           AAAA   NoRecords
wwis-dubc1-vip60.adobe... wwis-dubc1-vip60.adobe... A      Success   Answer  407599      4 127.0.0.1
wwis-dubc1-vip60.adobe... wwis-dubc1-vip60.adobe... A      Success   Answer  407599      4 127.0.0.1
1.0.0.127.in-addr.arpa    1.0.0.127.in-addr.arpa.   PTR    Success   Answer  407599      8 activate.adobe.com
1.0.0.127.in-addr.arpa    1.0.0.127.in-addr.arpa.   PTR    Success   Answer  407599      8 practivate.adobe.com
1.0.0.127.in-addr.arpa    1.0.0.127.in-addr.arpa.   PTR    Success   Answer  407599      8 ereg.adobe.com
1.0.0.127.in-addr.arpa    1.0.0.127.in-addr.arpa.   PTR    Success   Answer  407599      8 activate.wip3.adobe.com
1.0.0.127.in-addr.arpa    1.0.0.127.in-addr.arpa.   PTR    Success   Answer  407599      8 wip3.adobe.com
1.0.0.127.in-addr.arpa    1.0.0.127.in-addr.arpa.   PTR    Success   Answer  407599      8 3dns-3.adobe.com
```
Netejar la caché
```
PS C:\WINDOWS\system32> Clear-DnsClientCache
```
```
PS C:\Users\Tomàs> Get-DnsClientCache -Name www*

Entry                     RecordName                Record Status    Section TimeTo Data L Data
                                                    Type                     Live   ength
-----                     ----------                ------ ------    ------- ------ ------ ----
www.google.com            www.google.com            A      Success   Answer     210      4 216.58.215.132


PS C:\Users\Tomàs> Get-DnsClientCache -Name constr*

Entry                     RecordName                Record Status    Section TimeTo Data L Data
                                                    Type                     Live   ength
-----                     ----------                ------ ------    ------- ------ ------ ----
construirlasostenibili... construirlasostenibili... A      Success   Answer    3219      4 217.160.0.205


PS C:\Users\Tomàs> Get-DnsClientCache -recordname con*

Entry                     RecordName                Record Status    Section TimeTo Data L Data
                                                    Type                     Live   ength
-----                     ----------                ------ ------    ------- ------ ------ ----
construirlasostenibili... construirlasostenibili... A      Success   Answer    3202      4 217.160.0.205
```

### Ports

* Averiguar ports oberts:       Get-NetTCPConnection
  
``` PS C:\Users\Tomàs> Get-NetTCPConnection -State Established|ft LocalAddress, RemoteAddress,LocalPort

LocalAddress  RemoteAddress  LocalPort
------------  -------------  ---------
127.0.0.1     127.0.0.1          23560
192.168.1.130 104.123.23.222     23111
192.168.1.130 152.199.19.160     23110
192.168.1.130 2.20.44.184        23094
192.168.1.130 52.97.170.2        23061
127.0.0.1     127.0.0.1          19749
127.0.0.1     127.0.0.1           5809
127.0.0.1     127.0.0.1           1804
192.168.1.130 74.125.140.188      1057
192.168.1.130 40.67.254.36        1034
```
## RESUM DE cmdLets VISTOS:

* Get-NetAdapter
* Enable-NetAdapter
* Disable-NetAdapter
  
* Get-NetIpAddress
* Remove-NetIpAddress
  
* Get-NetRoute
* Remove-NetRoute
  
* Get-DnsClientServerAddress
* Clear-DnsClientCache

## COMPROVAR CONNECTIVITAT

Test-connection
A diferència del Ping torna un objecte.

``` PS C:\Users\Tomàs> Test-Connection 192.168.1.1

Source        Destination     IPV4Address      IPV6Address                              Bytes    Time(ms)
------        -----------     -----------      -----------                              -----    --------
PORTATIL1     192.168.1.1                                                               32       5
PORTATIL1     192.168.1.1                                                               32       5
PORTATIL1     192.168.1.1                                                               32       6
PORTATIL1     192.168.1.1                                                               32       46
```
Només enviem un paquet
```
PS C:\Users\Tomàs> Test-Connection 192.168.1.1 -Count 1

Source        Destination     IPV4Address      IPV6Address                              Bytes    Time(ms)
------        -----------     -----------      -----------                              -----    --------
PORTATIL1     192.168.1.1                                                               32       7
```
Només volem saber si hi ha o no connexió
```
PS C:\Users\Tomàs> Test-Connection 192.168.1.1 -Count 1 -quiet
True
```

Provem amb un nom de domini:
```
PS C:\Users\Tomàs> test-connection construirlasostenibilitat.net

Source        Destination     IPV4Address      IPV6Address                              Bytes    Time(ms)
------        -----------     -----------      -----------                              -----    --------
PORTATIL1     construirlas... 217.160.0.205                                             32       159
PORTATIL1     construirlas... 217.160.0.205                                             32       77
PORTATIL1     construirlas... 217.160.0.205                                             32       79
PORTATIL1     construirlas... 217.160.0.205                                             32       83


PS C:\Users\Tomàs> test-connection construirlasostenibilitat.net -count 1

Source        Destination     IPV4Address      IPV6Address                              Bytes    Time(ms)
------        -----------     -----------      -----------                              -----    --------
PORTATIL1     construirlas... 217.160.0.205                                             32       76


PS C:\Users\Tomàs> test-connection construirlasostenibilitat.net -count 1 -quiet
True
```

### Configuració Estàtica i Dinàmica

Farem la prova de assignar una IP Estàtica.
1. Esborrem la IP d'un interface
2. Esborrem la porta d'enllaç
3. Canviem la IP ( IP EStàtiva )
4. Coanviem els DNS



```PS C:\WINDOWS\system32> Get-NetIPConfiguration -InterfaceAlias "Wi-fi"


InterfaceAlias       : Wi-Fi
InterfaceIndex       : 24
InterfaceDescription : Qualcomm Atheros AR956x Wireless Network Adapter
NetProfile.Name      : WLAN_4E28PA 2
IPv4Address          : 192.168.1.130
IPv6DefaultGateway   :
IPv4DefaultGateway   : 192.168.1.1
DNSServer            : 192.168.1.1


PS C:\WINDOWS\system32> Remove-NetIPAddress -InterfaceAlias "Wi-Fi" -Confirm:$false
PS C:\WINDOWS\system32> Get-NetIPConfiguration -InterfaceAlias "Wi-fi"

InterfaceAlias       : Wi-Fi
InterfaceIndex       : 24
InterfaceDescription : Qualcomm Atheros AR956x Wireless Network Adapter
NetAdapter.Status    : Disconnected ```

2.- Esborrem el GateWay
PS C:\WINDOWS\system32> remove-netroute -InterfaceAlias "Wi-fi" -confirm:$false
PS C:\WINDOWS\system32>
```

3.- Assignem la IP nova
```powershell
PS C:\WINDOWS\system32> New-NetIPAddress -interfaceAlias "Wi-fi" 192.168.1.58 -PrefixLength 24 -defaultgateway 192.168.1.1

IPAddress         : 192.168.1.58
InterfaceIndex    : 24
InterfaceAlias    : Wi-Fi
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24
PrefixOrigin      : Manual
SuffixOrigin      : Manual
AddressState      : Tentative
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : 192.168.1.58
InterfaceIndex    : 24
InterfaceAlias    : Wi-Fi
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24
PrefixOrigin      : Manual
SuffixOrigin      : Manual
AddressState      : Invalid
Va lidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : PersistentStore
```

4.- Canviem els DNS
```
PS C:\WINDOWS\system32> Set-DnsClientServerAddress -interfaceAlias "Wi-fi" -serverAddresses 8.8.4.4
```

1. Eliminem la IP 
2. Eliminem el GateWay: 
3. Activem el DHCP
4. Ressetegem els DNS

```powershell
PS C:\WINDOWS\system32> Remove-NetIPAddress -InterfaceAlias "Wi-fi" -Confirm:$false
PS C:\WINDOWS\system32> Remove-NetRoute -InterfaceAlias "wi-fi" -Confirm:$false
PS C:\WINDOWS\system32> set-netIpinterface -InterfaceAlias "wi-fi" -Dhcp Enabled
PS C:\WINDOWS\system32> Set-DnsClientServerAddress -InterfaceAlias "wi-fi" -ResetServerAddresses
PS C:\WINDOWS\system32> Get-NetIPConfiguration -InterfaceAlias "wi-fi"

InterfaceAlias       : Wi-Fi
InterfaceIndex       : 24
InterfaceDescription : Qualcomm Atheros AR956x Wireless Network Adapter
NetProfile.Name      : WLAN_4E28PA 2
IPv4Address          : 192.168.1.130
IPv6DefaultGateway   :
IPv4DefaultGateway   : 192.168.1.1
DNSServer            : 192.168.1.1
```

* En cas de que no accepte el valor nous, podem reiniciar l'adaptador
  
