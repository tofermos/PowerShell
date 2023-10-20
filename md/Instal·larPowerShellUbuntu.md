# INSTAL·LAR POWERSHELL EN UBUNTU
Consultem l'ajuda de Microsoft,
Per vore quina versió de Linux tenim:
```bash
tomas@portatil:~$ lsb_release -a
LSB Version:	core-11.1.0ubuntu2-noarch:printing-11.1.0ubuntu2-noarch:security-11.1.0ubuntu2-noarch
Distributor ID:	Ubuntu
Description:	Ubuntu 20.04.2 LTS
Release:	20.04
Codename:	focal
```

Per tant tindrem dos alternatives. Amb les 2 alternatives que veurem repassarem el conceptes de :

*   Claus i repositoris.
*   Dependències.
*   Utilitats d'instal·lació  ( *apt*  VS *dpkg*)
*   snap ( el futur...)
*   Paquet de software ( fitxer *.deb* )
  
  
## Alternativa 1: mitjançant la descarrega del *.deb

Descarreguem el paquet: powershell_7.1.3-1.ubuntu.20.04_amd64.deb

```bash
sudo dpkg -i powershell_7.1.3-1.ubuntu.20.04_amd64.deb
sudo apT install -f
```

Els errors de dependències que provoca *dpkgv-i* es ressolel amb *-f*


## Alternativa 2: mitjançant els repositoris.

Actualitzar repositori
```bash
sudo apt update
```
# Instal·lar prerequisits.

```bash
sudo apt install -y wget apt-transport-https software-properties-common
```

Repassem els conceptes de REPOSITORI.
```bash
# Descarrega de les claus del repositori
wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb
# Registre de Microsoft repository GPG keys
sudo dpkg -i packages-microsoft-prod.deb
# Update the list of packages after we added packages.microsoft.com
sudo apt-get update
```
Una vegada actualitzat el repositori, instal·lem el paquet

```bash
# Instal·lar PowerShell
sudo apt install -y powershell
# Iniciem PowerShell
pwsh

```
## Paquets *.deb* 
Paquets d'instal·lació de Debian/GNU i derivades ( Ubuntu ).
Contenen tres fixers:
   * debian-binary -  número de versió del format deb. 
   * control.tar.gz - la metadada.
   * data.tar, data.tar.gz, data.tar.bz2: els arxius a instal·lar.


Per instal·lar
```
    dpkg -i paquete.deb
```
Per verificar la instal·lació :
```
    dpkg -l | grep 'paquet'
```
Per desinstal·lar:
```
    dpkg -r paquet.deb
    dpkg -P paquet.deb (esta última elimina totes les dades 

```
## Repositori
Servidor accessible des de internet que emmmagatzema els paquets ( programes ) a instal·lar. 


## + info sobre Instal·lació de PowerShell el Linux

( MicroSoft)[https://docs.microsoft.com/es-es/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-7.1]
