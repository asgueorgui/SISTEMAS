# Proyecto: analizar procesos utilizando Bases de Datos
``` powershell
## Conexion a mysql

[void][System.Reflection.Assembly]::LoadWithPartialName("MySql.Data")
$Connection = New-Object MySql.Data.MySqlClient.MySqlConnection
$ConnectionString = "server=" + "localhost" + ";port=3306;uid=" + "root" + ";pwd=" + ";database=proyectohash"+"; SslMode=none"
$Connection.ConnectionString = $ConnectionString
$Connection.Open()

# Realiza un hash a los fichero ejecutables system32 y selecciona los 3 primeros.

foreach($hash2 in Get-ChildItem C:\Windows\System32\*.exe | select -First 3)
{
    $nom = $hash2.Name
    $nhash = (Get-FileHash $hash2.FullName).hash

    
    $Query = 'INSERT INTO hash (nombre,hash) VALUES ("'+$nom+'","'+$nhash+'")'
    $Command = New-Object MySql.Data.MySqlClient.MySqlCommand($Query, $Connection)
    $DataAdapter = New-Object MySql.Data.MySqlClient.MySqlDataAdapter($Command)
    $DataSet = New-Object System.Data.DataSet
    $RecordCount = $dataAdapter.Fill($dataSet, "data")
    $DataSet.Tables[0]
    $Connection.Close()
}

# Consultar informacion de la base datos.

    $Query = 'select * from hash'
    $Command = New-Object MySql.Data.MySqlClient.MySqlCommand($Query, $Connection)
    $DataAdapter = New-Object MySql.Data.MySqlClient.MySqlDataAdapter($Command)
    $DataSet = New-Object System.Data.DataSet
    $RecordCount = $dataAdapter.Fill($dataSet, "data")
    $DataSet.Tables[0]
    $Connection.Close()


# Ejecucion de un CMDLET de forma paralela en ambos equipos

$win10 = 'DESKTOP-FC5TSEH'

$var1 = (Invoke-Command -ScriptBlock {foreach ($numhash in Get-ChildItem C:\Windows\System32\*.exe | Where-Object name -Match "AgentService.exe")
{
     $numhash.Name
     (Get-FileHash $numhash.FullName).hash
}} -ComputerName $win10)[1]


foreach($agent in $DataSet.Tables)
{
    $var2 = ($agent | Where-Object nombre -Match "AgentService.exe").hash
    $var1 = (Invoke-Command -ScriptBlock {foreach ($numhash in Get-ChildItem C:\Windows\System32\*.exe | Where-Object name -Match "AgentService.exe")
{
     $numhash.Name
     (Get-FileHash $numhash.FullName).hash
}} -ComputerName $win10)[1]
if ($var2 -eq $var1)
    {
    "Son iguales"
    }
    else
    {
    "Son diferentes"
    }
}
```
