# Requisits previs
- Windows Server 2019 amb el rol de DHCP instal·lat.
- IP fixa configurada per al servidor DHCP.

Obri una sessió PowerShell amb permisos d'administrador.

# 1. Configurar la IP fixa del servidor Windows Server 2019

Estableix la següent IP al servidor: 192.168.100.100.

## 1.1 Averigua el nom de la NIC i la IP.

```powershell
Get-NetIPInterface
```
Suposem una resposta:

fIndex InterfaceAlias                  AddressFamily NlMtu(Bytes) InterfaceMetric Dhcp     ConnectionState PolicyStore
------- --------------                  ------------- ------------ --------------- ----     --------------- -----------
1       Loopback Pseudo-Interface 1     IPv6            4294967295              75 Disabled Connected       ActiveStore
7       Ethernet                        IPv4                  1500              25 Disabled Connected       ActiveStore
1       Loopback Pseudo-Interface 1     IPv4            4294967295              75 Disabled Connected       ActiveStore


Ja sabem el nom: "Ethernet". També que és fixa ( Dhcp: Disabled). Podem mirar si la IP

```powershell
Get-NetIPAddress -InterfaceAlias "ETHER*"
```

## 1.2 Executa la següent comanda per assignar l'IP fixa:

```powershell
Set-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.100.100 -PrefixLength 24
```


# 2. Configuració de la NIC (interfície de xarxa)

Assignem el DNS loopback (127.0.0.1) al servidor.

## Passos:
1. Executa la següent comanda PowerShell:

   ```powershell
   Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1
   ```

   Això assignarà l'adreça del servidor DNS com a 127.0.0.1 (loopback).

# 3. Configurar el servei DHCP i crear un rang d'IPs

1. Afegeix el rol de DHCP, si no s'ha fet anteriorment:

   ```powershell
   Install-WindowsFeature -Name DHCP -IncludeManagementTools
   ```

2. Importa el mòdul de DHCP per gestionar el servei DHCP:

   ```powershell
   Import-Module DhcpServer
   ```

3. Crea un rang d'IPs que vagen des de 192.168.100.1 fins a 192.168.100.99:

   ```powershell
   Add-DhcpServerv4Scope -Name "Xarxa Interna" -StartRange 192.168.100.1 -EndRange 192.168.100.99 -SubnetMask 255.255.255.0 -State Active
   ```

# 4. Configuració d'exclusions d'IP

Per excloure un rang d'IPs de l'assignació automàtica (en aquest cas de 192.168.100.80 fins a 192.168.100.99), s'ha de fer el següent:

1. Executa aquesta comanda PowerShell:

   ```powershell
   Add-DhcpServerv4ExclusionRange -ScopeId 192.168.100.0 -StartRange 192.168.100.80 -EndRange 192.168.100.99
   ```

# 5. Reserva d'IP

Per reservar l'adreça 192.168.100.1 per a un equip amb Windows 11 basant-se en la seua adreça MAC:

1. Obtenim l'adreça MAC del client Windows 11 i la utilitzem per crear la reserva.

# 6. Diferents formes d'obtindre la MAC en un PC amb Windows 11

## 6.1 Utilitzant PowerShell:

1. Al Windows 11, obri PowerShell i executa la següent comanda per obtindre la MAC:

   ```powershell
   Get-NetAdapter | Select-Object Name, MacAddress
   ```

   Apareixerà una llista amb les interfícies de xarxa i les seues adreces MAC.

2. Una vegada tens l'adreça MAC, utilitza aquesta informació al servidor per fer la reserva d'IP.

3. Al servidor, executa la següent comanda per reservar l'IP 192.168.100.1 per a l'adreça MAC:

   ```powershell
   Add-DhcpServerv4Reservation -ScopeId 192.168.100.0 -IPAddress 192.168.100.1 -ClientId "XX-XX-XX-XX-XX-XX" -Description "Reserva per a Windows 11"
   ```

   Substitueix "XX-XX-XX-XX-XX-XX" per l'adreça MAC del client Windows 11.

## 6.2 Obtenció de la MAC des de l'entorn gràfic de Windows 11

1. Fes clic amb el botó dret sobre la icona de xarxa en la barra de tasques i selecciona "Obri Configuració de xarxa i Internet".
2. A la finestra que s'obri, selecciona "Estat" en el panell esquerre, i després selecciona "Canvia les propietats de connexió" de la connexió activa (Wi-Fi o Ethernet).
3. Al final de la pàgina, en l'apartat "Propietats", trobaràs l'adreça MAC sota "Adreça física (MAC)".

## 6.3 Utilitzant l'eina **ipconfig** en la línia de comandes

1. Obri el símbol del sistema (CMD) al Windows 11.
2. Executa la següent comanda:

   ```cmd
   ipconfig /all
   ```

   En la secció corresponent a la interfície de xarxa que utilitzes (Wi-Fi o Ethernet), busca l'apartat "Adreça física". Aquesta serà l'adreça MAC de la targeta de xarxa.

# 7. Confirmació i verificació

Pots comprovar que la configuració del DHCP s'ha realitzat correctament utilitzant les següents comandes:

1. Comprova els rangs del DHCP:

   ```powershell
   Get-DhcpServerv4Scope
   ```

2. Comprova les exclusions:

   ```powershell
   Get-DhcpServerv4ExclusionRange -ScopeId 192.168.100.0
   ```

3. Comprova les reserves:

   ```powershell
   Get-DhcpServerv4Reservation -ScopeId 192.168.100.0
   ```