# ASO
## Scripting

#### Crear un usuario mediante el cmdlet New-ADObject
```
New-ADObject -name Usuario22 -type user -ProtectedFromAccidentalDeletion $true -
```
# Guardar en json nombre de proceso y estado de proceso o prioridad y desde directorio activo leer ese proceso.
```
Get-Process | select name,BasePriority | ConvertTo-Json | out-file C:\xampp\htdocs\procesos.json -Encoding default
Invoke-RestMethod "http://localhost/procesos.json"
```
