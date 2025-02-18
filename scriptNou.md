
# 1 Completar script

Completa l'script per a que, de forma interactiva, cree una UO, i un Grup d'usuaris.

```powershell
# Demana el nom de la Unitat Organitzativa
$OUName = Read-Host "Introdueix el nom de la Unitat Organitzativa (OU)"

# Domini actual
$Domain = (Get-ADDomain).DistinguishedName
$OUPath = "OU=$OUName,$Domain"

# Comprova si l'OU ja existeix.
# Pista: Quan usem un cmdLet de tipus "Get-...", si no existeix el que busquem ens torna un NULL.

$OUExists = 

if ($OUExists) {
    Write-Host "La Unitat Organitzativa '$OUName' ja existeix en el domini." -ForegroundColor Yellow
} else {

# La creem

    }

```

# 2 Comprovar que funciona

