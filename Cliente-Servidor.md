## Ejecutar comandos atraves de cliente servidor desde un fichero en un GitHub(RAW)
```powershell
## Fichero con cmdlets
$codigo = Invoke-WebRequest -Uri https://raw.githubusercontent.com/asgueorgui/SISTEMAS/main/ejemplo.md

foreach ($linea in $codigo.Content -split "`n")
{
    if($linea[1] -ne "-" -and $linea[1] -ne "#"  -and $linea[1] -ne "*" -and $linea[1] -ne '`')
    {
        $linea.Replace(" ", "*") | Out-File codigo.txt -Append
   }
}
##Client
$port=2020
$endpoint = new-object System.Net.IPEndPoint ([IPAddress]::Loopback,$port)
$udpclient=new-Object System.Net.Sockets.UdpClient
$b=[Text.Encoding]::ASCII.GetBytes((gc codigo.txt))
$bytesSent=$udpclient.Send($b,$b.length,$endpoint)
$udpclient.Close()
```
```powershell
## Ejecutar los comandos que se mandan por la red
 
##Server
$port=2020
$endpoint = new-object System.Net.IPEndPoint ([IPAddress]::Any,$port)
$udpclient=new-Object System.Net.Sockets.UdpClient $port
$content=$udpclient.Receive([ref]$endpoint)
[Text.Encoding]::ASCII.GetString($content) | % {

    $_.Replace("*", " ") | iex
}
$udpclient.Dispose()
```
