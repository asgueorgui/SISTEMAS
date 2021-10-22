# ASO
## Scripting

### Crear un usuario mediante el cmdlet New-ADObject
``` powershell
New-ADObject -name Usuario22 -type user -ProtectedFromAccidentalDeletion $true -
```
### Guardar en json nombre de proceso y estado de proceso o prioridad y desde directorio activo leer ese proceso.
``` powershell
Get-Process | select name,BasePriority | ConvertTo-Json | out-file C:\xampp\htdocs\procesos.json -Encoding default
Invoke-RestMethod "http://localhost/procesos.json"
```
## Sacar el padre y abuelo de cada hilo que se está ejecutando
#### Handle: identificador de un subproceso (hilo). 
#### ProcessHandle: proceso que creó el subproceso (hilo).
#### ParentProcessId: identificador único del proceso que crea un proceso.
``` powershell
Get-WmiObject -Class Win32_Service | Select-Object Name,ProcessID, (Get-Process -Id ProcessID).name

Get-WmiObject -Class Win32_Service | Select-Object Name,ProcessID,@{Name="nombre proceso";Expression={(Get-Process -Id $_.ProcessID).name}}

Get-WmiObject -Class Win32_thread | Select-Object handle,ProcessHandle,@{Name="padre";Expression={(Get-Process -Id $_.ProcessHandle).name}},@{Name="abuelo";Expression={(Get-Process -Id (Get-WmiObject -Class Win32_process | where ProcessId -eq $_.ProcessHandle).parentprocessid).name}}
```
## Script que nos muestra la ip de los logs de nuestro servidor apache en este caso en XAMPP
``` powershell
get-content C:\xampp\apache\logs\access.log

foreach($linea in get-content C:\xampp\apache\logs\access.log)
{
    $linea.split(" ")[0]
}
```
# GHz de nuestro procesador
``` powershell
foreach($gh in Get-CimInstance win32_processor)
{
       $gh.name.split("@")[1].trim()
}
``` 
# Con WSL sacar los nombres ordenados
``` powershell
foreach($usuario in (wsl cat -f1 -d : /etc/passwd))
{
    $usuario.split(":")[0] + " -> " $usuario.Split(":")[0].length
}
```

