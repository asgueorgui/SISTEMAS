## Instalacion de OpenSSH desde linea de comando con PowerShell.
``` powershell
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
```
## Instalacion de las dos caracteristicas tanto el cliente como le servidor.
``` powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```
## Arrancar el servicio SSh
``` powershell
Start-service sshd
```
