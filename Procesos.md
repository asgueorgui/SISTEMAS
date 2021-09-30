### DECIR QUE HILOS ESTA EJECUTANDO CADA SERVICIO, ADEMAS DE SACARLO ORDENADOR.

foreach($servicio in Get-WmiObject win32_service | where state -eq Running)
{
    Write-Host $servicio.Name "->" (Get-Process -id $servicio.ProcessId).name
    (Get-Process -id $servicio.ProcessId).Threads.id
}
