# Gestió d'usuaris del AD en PowerShell

### 1.0 Investiguem des de l'entorn gràfic...

El primer que hem de fer és crear un usuari del Domini des de l'entorn gràfic per vore quines són les propietats necessàries.

Comprovem que, en indicar el "Nombre de Pila" (GivenName) s'ompli la propietat "Nobre de inici de sesión" (SamAccountName), fins que estos camps no estan, no podem continuar.
En passar avant comprovem que podem deixar els checkBox referents a passaword en qualsevol estat però cal indicar un PassWord.
Per tant, els camps mínims necessaris seran:
* El GivenName.
* El SamAccountName.
* El Name
* Password
Estes propietat les veiem en excutar el Get-ADUser...

>🔎 Per buscar els cmdLets que possiblement necessitem.
>```powershell
>PS C:\Users\Administrador> Get-Command *aduser*
>
>CommandType     Name                                               Version    Source
>-----------     ----                                               -------    ------
>Cmdlet          Get-ADUser                                         1.0.1.0    ActiveDirectory
>Cmdlet          Get-ADUserResultantPasswordPolicy                  1.0.1.0    ActiveDirectory
>Cmdlet          New-ADUser                                         1.0.1.0    ActiveDirectory
>Cmdlet          Remove-ADUser                                      1.0.1.0    ActiveDirectory
>Cmdlet          Set-ADUser                                         1.0.1.0    ActiveDirectory
>
>
>PS C:\Users\Administrador> Get-Command *adaccount*
>
>CommandType     Name                                               Version    Source
>-----------     ----                                               -------    ------
>Cmdlet          Clear-ADAccountExpiration                          1.0.1.0    ActiveDirectory
>Cmdlet          Disable-ADAccount                                  1.0.1.0    ActiveDirectory
>Cmdlet          Enable-ADAccount                                   1.0.1.0    ActiveDirectory
>Cmdlet          Get-ADAccountAuthorizationGroup                    1.0.1.0    ActiveDirectory
>Cmdlet          Get-ADAccountResultantPasswordReplicationPolicy    1.0.1.0    ActiveDirectory
>Cmdlet          Search-ADAccount                                   1.0.1.0    ActiveDirectory
>Cmdlet          Set-ADAccountAuthenticationPolicySilo              1.0.1.0    ActiveDirectory
>Cmdlet          Set-ADAccountControl                               1.0.1.0    ActiveDirectory
>Cmdlet          Set-ADAccountExpiration                            1.0.1.0    ActiveDirectory
>Cmdlet          Set-ADAccountPassword                              1.0.1.0    ActiveDirectory
>Cmdlet          Unlock-ADAccount                                   1.0.1.0    ActiveDirectory


### 1.1 Consultem un usuari existent

```powershell
PS C:\Users\Administrador> Get-ADUser -filter "name -like '*pere*'"



DistinguishedName : CN=pere gomis,OU=PV,DC=tfm,DC=org
Enabled           : True
GivenName         : pere
Name              : pere gomis
ObjectClass       : user
ObjectGUID        : 060adf87-5a56-4b28-8623-0c2512a931c4
SamAccountName    : peregomis
SID               : S-1-5-21-2537893746-599189144-3331569356-1114
Surname           :
UserPrincipalName : peregomis@tfm.org
```
Com ja hem vist amb les UO. Pot existir dos objectes amb mateix name (usuaris "pere", UO "La Safor"...) però en ubicacions diferents. 

Això sí, mai podrà haver-hi al Domini dos usuaris amb el mateix SamAccountName !

Per obtenir un sol objecte ADUser concret pots:

* Filtrar per una propietat única ( -filter "distinguishedName/SamAccountName/ SID/ ObjecteGUID -like ...")
* Amb el paràmetre -Identity indican una propietat única: "distinguishedName/SamAccountName/ SID/ ObjecteGUID

### 1.2 Creem un usuari de l'Active Directory

```powershell
new-aduser -GivenName "joana" -name "joana pellicer" -SamAccountName "joanapellicer"
```

#### Contrasenya encriptada
```
Ens faltaria assignar el password...
>:memo: **Apunt 5:: PowerShell disposa de cmdLets i funcions creades tipus "ConvertTo-"
>```powershell
>PS C:\Users\Administrador> get-command convertto*

>CommandType     Name                                               Version    Source
>-----------     ----                                               -------    ------
>Function        ConvertTo-DnsServerPrimaryZone                     2.0.0.0    DnsServer
>Function        ConvertTo-DnsServerSecondaryZone                   2.0.0.0    DnsServer
>Cmdlet          ConvertTo-Csv                                      3.1.0.0    Microsoft.PowerShell.Utility
>Cmdlet          ConvertTo-Html                                     3.1.0.0    Microsoft.PowerShell.Utility
>Cmdlet          ConvertTo-Json                                     3.1.0.0    Microsoft.PowerShell.Utility
>Cmdlet          ConvertTo-ProcessMitigationPolicy                  1.0.11     ProcessMitigations
>Cmdlet          ConvertTo-SecureString                             3.0.0.0    Microsoft.PowerShell.Security
>Cmdlet          ConvertTo-TpmOwnerAuth                             2.0.0.0    TrustedPlatformModule
>Cmdlet          ConvertTo-Xml                                      3.1.0.0    Microsoft.PowerShell.Utility
```
Per assignar el password encriptat farem ús d'una variable com ja sabem i el cmdLet "ConvertTo-SecureString"

```powershell
$password1=ConvertTo-SecureString "UnPasswordArreu13" -AsPlainText -force
```
Per canviar la contrassenya ( resetajar-la ), ho hem de fer am el cmdLet
```powershell
Set-ADAPassword -Identity "joanapellicer" -reset -NewPassword $password1
```
Si no indiquem el paràmetre "-reset", ens demana la constrassenya anterior i la nova.

### 1.3 Ubicació de l'objecte

Intentem crear un altre usuari de l'AD en un altra UO...
```powershell
PS C:\Users\Administrador> new-aduser -GivenName "joana" -name "joana pellicer" -path "OU=La SAFOR,OU=PV,DC=tfm,DC=org"
```
Com hem canviat d'ubicació, no tenim problema en repetir el -name i el -givename. Igual que passava als els 2 objecte UO "La Safor".
Pero per a iniciar sessió en el domini ( des del client ) hauran de logejaar-se amb un login distint... 

Observem-ho:

Intentem crear una altra "joana" repetin el login ( SamAccountName ).
```powersehll
PS C:\Users\Administrador> new-aduser -GivenName "joana" -name "joana pellicer" -SamAccountName "joanapellicer" -path "OU=PV,DC=tfm,DC=org"
new-aduser : La cuenta especificada ya existe
En línea: 1 Carácter: 1
+ new-aduser -GivenName "joana" -name "joana pellicer" -SamAccountName  ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ResourceExists: (CN=joana pellicer,OU=PV,DC=tfm,DC=org:String) [New-ADUser], ADIdentityA
   lreadyExistsException
    + FullyQualifiedErrorId : ActiveDirectoryServer:1316,Microsoft.ActiveDirectory.Management.Commands.NewADUser
 ```
Provem canviant el login en canviar la SamAccountName
```powershell

PS C:\Users\Administrador> new-aduser -GivenName "joana" -name "joana pellicer" -SamAccountName "joanapellicer2" -path "OU=PV,DC=tfm,DC=org"
PS C:\Users\Administrador>
```

El resultat final son 3 usuaris "joana" en diferents OU i diferents login.
```powershell

PS C:\Users\Administrador> Get-ADUser -filter "givenname -like 'joana'"


DistinguishedName : CN=joana pellicer,CN=Users,DC=tfm,DC=org
Enabled           : False
GivenName         : joana
Name              : joana pellicer
ObjectClass       : user
ObjectGUID        : 3c82324e-34be-4a85-9fec-f0f48bf137b6
SamAccountName    : joanapellicer
SID               : S-1-5-21-2537893746-599189144-3331569356-1115
Surname           :
UserPrincipalName :

DistinguishedName : CN=joana pellicer,OU=PV,DC=tfm,DC=org
Enabled           : False
GivenName         : joana
Name              : joana pellicer
ObjectClass       : user
ObjectGUID        : 42e644d5-f565-47c1-b60b-06272b2f0a98
SamAccountName    : joanapellicer2
SID               : S-1-5-21-2537893746-599189144-3331569356-1116
Surname           :
UserPrincipalName :

DistinguishedName : CN=joana pellicer,OU=La Safor,OU=PV,DC=tfm,DC=org
Enabled           : False
GivenName         : joana
Name              : joana pellicer
ObjectClass       : user
ObjectGUID        : 58a2d15b-692f-45af-8d79-9038a0d7955a
SamAccountName    : joana pellicer
SID               : S-1-5-21-2537893746-599189144-3331569356-1117
Surname           :
UserPrincipalName :

```
### 1.4 Moure un usuari de l'AD d'una a una UO a altra
Busquem el usuari on està.
```powershell
PS C:\Users\Administrador> get-aduser

cmdlet Get-ADUser en la posición 1 de la canalización de comandos
Proporcione valores para los parámetros siguientes:
(Escriba !? para obtener Ayuda).
Filter: name -like 'tomas1'

Busquem el usuari on est.
powershellDistinguishedName : CN=tomas1,OU=PV,DC=tfm,DC=org
Enabled           : True
GivenName         : tomas1
Name              : tomas1
ObjectClass       : user
ObjectGUID        : 55b7311c-a631-4181-bb45-95c5359a3d7b
SamAccountName    : tomas1
SID               : S-1-5-21-2537893746-599189144-3331569356-1109
Surname           : ferrandis
UserPrincipalName : tfm1

```

```powershell

PS C:\Users\Administrador> Move-ADObject -Identity "CN=tomas1,OU=PV,DC=tfm,DC=org" -TargetPath "OU=La Marina,OU=PV,DC=tfm,DC=org"
PS C:\Users\Administrador> get-aduser -Identity "S-1-5-21-2537893746-599189144-3331569356-1109"


DistinguishedName : CN=tomas1,OU=La Marina,OU=PV,DC=tfm,DC=org
Enabled           : True
GivenName         : tomas1
Name              : tomas1
ObjectClass       : user
ObjectGUID        : 55b7311c-a631-4181-bb45-95c5359a3d7b
SamAccountName    : tomas1
SID               : S-1-5-21-2537893746-599189144-3331569356-1109
Surname           : ferrandis
UserPrincipalName : tfm1

```
### 1.5 Modificacions i eliminacions.
Per  a la resta de modificacions i l'eliminació, s'usen el
* set-ADuser
* remove-ADuser
Identificant l'objecte i, a continuació la propietat amb el nou valor. Com hem fet en les UO.


:computer: Crea els usuaris següents en les UO indicades en el camp "OU inicial" ( o rel, si s'inidica ). Quan tingues tots els usuaris creats, canvia-los a la "UO destí", si hi ha aluna indicada.
|Nom|Cognoms|Login|Password|UO Inicial|UO destí|
|---|---|---|---|---|---|
|Pepa|Ferrer Martí|pepa|contrasenya123&|PV|PV-La Safor|
|Xavier|Garcia Gomis|xavi|contra1123%|( rel )|PV-La Safor|
|Pepa|Fuster Garcia|pepa|contrasenya4545)|PV-La Safor||

Tens dos usuaris on coicideixen els login i el nom. Fixa't si es produeix algun problema, quan i com l'has de solucionar.
