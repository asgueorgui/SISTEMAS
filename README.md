# ASO
## Scripting
### ----------------------------------------------------------
### Crear un usuario mediante el cmdlet New-ADObject
New-ADObject -name Usuario22 -type user -ProtectedFromAccidentalDeletion $true -

# Mover elementos con Move-ADObject (OU y usuarios)
Move-ADObject -Identity "OU=UnidadPSOrigen,DC=Andel,DC=Local" -TargetPath "OU=UnidadDestino,DC=Andel,DC=Local"

$SAMAccountName = "usuario1"
$mover = (Get-ADUser -LDAPFilter "(samaccountname=$SAMAccountName)").DistinguishedName
Move-ADObject -Identity $mover -TargetPath "OU=UnidadDestino,DC=Andel,DC=Local"

Get-ADUser -LDAPFilter "(samaccountname=$SAMAccountName)" | Move-ADObject -TargetPath "OU=UnidadDestino,DC=Andel,DC=Local"
