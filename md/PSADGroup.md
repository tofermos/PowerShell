# GESTIÓ DE GRUPS DEL AD EN POWERSHELL

🗃️ [INDEX POWERSHELL][POWERSHELL]

[POWERSHELL]:https://github.com/tofermos/PowerShell/blob/main/README.md

  ## 1.0 PRÈVIA.
  ### INVESTIGUEM L'ENTORN GRÀFIC
  Obeservem que , si més no, hem d'indicar un valor per als següents atributs:
  
  ### cmdLets INTERESSANTS
  * Per trobar el cmdLets interessant: get-Command
  * Per trobar ajuda sobre ells: get-help
  * No hi ha cap alias predefinit en tractar-se d'objetes propis del AD
  ```powershell
get-command *adgroup*
```
```code

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Add-ADGroupMember                                  1.0.1.0    ActiveDirectory
Cmdlet          Get-ADGroup                                        1.0.1.0    ActiveDirectory
Cmdlet          Get-ADGroupMember                                  1.0.1.0    ActiveDirectory
Cmdlet          New-ADGroup                                        1.0.1.0    ActiveDirectory
Cmdlet          Remove-ADGroup                                     1.0.1.0    ActiveDirectory
Cmdlet          Remove-ADGroupMember                               1.0.1.0    ActiveDirectory
Cmdlet          Set-ADGroup                                        1.0.1.0    ActiveDirectory
```
:computer:
Fes proves amb eks cdmLet d'ajuda indicat i observa els paràmetres útil.
```powershell
get-help Add-ADgroup -exemples
```

