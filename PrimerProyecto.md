## Primer proyecto para la asignatura de ASO

### Creacion de formularios en Powershell

``` powershell
using assembly System.Windows.Forms
using namespace System.Windows.Forms
 
class Operacion
{ 
  [Int]$operacion
  [String]$nombre
 
    Operacion($operacion,$nombre)
    {
        $this.operacion = $operacion
        $this.nombre = $nombre
    }
}
 
$arrayoperaciones = New-Object System.Collections.ArrayList
 
$form = [Form] @{
 Text = 'ProyectoASO'
}
 
$TextBox = [TextBox] @{
 Location = New-Object System.Drawing.Size(100,80)
}

$TextBox1 = [TextBox] @{
 Location = New-Object System.Drawing.Size(100,120)
}
 
$button = [Button] @{
 Text = 'Operar'
 Location = New-Object System.Drawing.Size(70,160)
}

$button1 = [Button] @{
 Text = 'Cancelar'
 Location = New-Object System.Drawing.Size(150,160)
}

$button.add_Click{
 $arrayoperaciones.add([Operacion]::new($TextBox1.Text,$TextBox.Text))
 $arrayoperaciones | ConvertTo-Json | Out-File C:\xampp\htdocs\operaciones.txt -Append -Encoding default
}

$label = [label] @{
 Location = New-Object System.Drawing.Size(125,65)
 Text = 'Nombre'
 }

 $label1 = [label] @{
 Location = New-Object System.Drawing.Size(120,106)
 Text = 'Operacion'
 }

$form.Controls.Add($TextBox)
$form.Controls.Add($TextBox1)
$form.Controls.Add($label)
$form.Controls.Add($label1)
$form.Controls.Add($button)
$form.Controls.Add($button1)
$form.ShowDialog()
```

### Invocar a la operacion y crear un usuario en el AD, ademas de crear un QR para cada Usuario
``` powershell
Invoke-RestMethod "http://192.168.50.51/operaciones.txt"

foreach($usuario in Invoke-RestMethod "http://192.168.50.51/operaciones.txt")
{
    if($usuario.operacion -eq 1)
    {
        New-ADUser -Name $usuario.nombre -SamAccountName $usuario.nombre -AccountPassword (ConvertTo-SecureString "asdf1234." -AsPlainText -Force) -Enable $true
        $usuario.nombre | wsl qrencode -o qr.png
    }
}
```
