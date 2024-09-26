# GESTIÓ USUARIS

## INFORMACIÓ SOBRE USUARIS LOCALS
* Get-LocalUser
* New-LocalUser
* ConvertTo-SecureString set
* Set-LocalUser
* Remove-LocalUser

### Creació d'usuaris.

**Creem l'usuari sense contrassenya primer**

Creem l'usuari

```powershell
New-LocalUser user1 -NoPassword
```
```
Name  Enabled Description
----  ------- -----------
user1 True               
```
Creem la contrassenya

```Powershell

PS C:\Windows\system32> $pas= ConvertTo-SecureString "user1" -AsPlainText 
ConvertTo-SecureString : El sistema no puede proteger los datos de entrada de texto sin formato. Para eliminar la advertencia y convertir el texto sin formato en SecureString, vuelva a 
emitir el comando con el parámetro Force. Para obtener más información, escriba get-help ConvertTo-SecureString.
En línea: 1 Carácter: 7
+ $pas= ConvertTo-SecureString "user1" -AsPlainText
+       ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidArgument: (:) [ConvertTo-SecureString], ArgumentException
    + FullyQualifiedErrorId : ImportSecureString_ForceRequired,Microsoft.PowerShell.Commands.ConvertToSecureStringCommand
```

( Ens demana el paràmetre -force )

```Powershell 

PS C:\Windows\system32> $pas= ConvertTo-SecureString "user1" -AsPlainText -force
```
Assignem la contrassenya

``` powershell

PS C:\Windows\system32> set-LocalUser user1 -Password $pas
```
**Creem un contrassenya encriptada primer**

Creem primer una contrassenya encriptada.

```PowerShell
PS C:\Windows\system32> $pas2=convertTo-SecureString "pas2" -AsPlainText -force

```
Creem l'usuari i assignem la contrassenya
```Powershell

PS C:\Windows\system32> New-LocalUser user2 -Password $pas2

Name  Enabled Description
----  ------- -----------
user2 True               


PS C:\Windows\system32> Get-LocalUser

Name           Enabled Description                                                      
----           ------- -----------                                                      
Administrador  False   Cuenta integrada para la administración del equipo o dominio     
DefaultAccount False   Cuenta de usuario administrada por el sistema.                   
Invitado       False   Cuenta integrada para el acceso como invitado al equipo o dominio                                                                  
tomasw         True                                                                     
user2          True                                                                    
                                                            
```

## Característiques dels comptes d'usuari.

```PowerShell

PS C:\Windows\system32> set-localUser -AccountNeverExpires user1
PS C:\Windows\system32> set-localUser -AccountExpires 01/01/2022 user1
PS C:\Windows\system32> set-localUser -Description descripciouser1 user1
```
