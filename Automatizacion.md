### Comprobar que puertos tenemos activos y ver que servicio ejecuta con el identificador del proceso.
```powershell
(Get-NetTCPConnection -LocalPort 80).OwningProcess
Get-Process -Id 7016
```
## Saber si Apache está funcionando, si no es asi arrancarlo.
```powershell
$apache = Get-Service | where name -Match "apache"

    if($apache.Status -match "Stopped")
    {
        Start-Service $apache.Name
    }
    else
    {
        Stop-Service $apache.Name
    }
```
## Parar todo lo que esta esuchando el puerto 80 que no sea Apache.
