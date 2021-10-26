### Tenemos un cliente y un servidor y queremos mover el raton de nuestro servidor el cual esta escuchando.

``` powershell
## -------- Cliente -------- ##
 
$ip = New-Object System.Net.IPEndPoint ([IPAddress]"192.168.50.50",2020)
$udp = New-Object System.Net.Sockets.UdpClient

$enviar = [System.Windows.Forms.Cursor]::Position.x.ToString()+"*"+[System.Windows.Forms.Cursor]::Position.y.ToString()

$mensaje = [Text.Encoding]::ASCII.GetBytes($enviar)
 
$udp.Send($mensaje,$mensaje.length,$ip) | Out-Null
 
$udp.Close()
```
``` powershell 
## -------- Server -------- ##

$ip = New-Object System.Net.IPEndPoint ([IPAddress]::Loopback,2020)
$udp = New-Object System.Net.Sockets.UdpClient 2020

$posiciones = [Text.Encoding]::ASCII.GetString($udp.Receive([ref]$ip))

[System.Windows.Forms.Cursor]::Position = New-Object System.Drawing.Point(($posiciones.split("*")[0]+100),$posiciones.split("*")[1])
 
$udp.Close()
```
