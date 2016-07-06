# Firmar la aplicación de escritorio convertida

En este artículo se explica cómo firmar una aplicación de escritorio que se ha convertido a la Plataforma universal de Windows (UWP). Debes firmar el paquete .appx con un certificado para poder implementarla.

## Firmar el paquete .appx

Primero, crea un certificado con MakeCert.exe. Si se te solicita que introduzcas una contraseña, selecciona "Ninguna". 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
```

A continuación, copia la información de las claves pública y privada en el certificado mediante pvk2pfx.exe. 

```cmd
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
```
Por último, usa SignTool.exe para firmar el .appx con el certificado.

```cmd
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
``` 

Para obtener más información, consulta [Cómo firmar un paquete de la aplicación con SignTool](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835835(v=vs.85).aspx). 

Las tres herramientas anteriores se incluyen con el SDK de Microsoft Windows 10. Para llamarlos directamente, llama al script ```C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\Tools\VsDevCmd.bat``` desde un símbolo del sistema.

## Errores comunes

### Firmas Authenticode dañadas o mal formadas

Esta sección contiene información detallada sobre cómo identificar problemas con los archivos Portable ejecutable (PE) del paquete AppX que pueden contener firmas Authenticode dañadas o mal formadas. Las firmas Authenticode no válidas en los archivos PE, que pueden estar en cualquier formato binario (como .exe, .dll, chm, etc.) impedirán que el paquete se firme correctamente y, por consiguiente, que se pueda implementar desde un paquete AppX. 

La ubicación de la firma Authenticode de un archivo PE se especifica mediante la entrada de la tabla de certificados en los directorios de datos de encabezado opcionales y la tabla de certificados de atributos asociada. Durante la comprobación de la firma, la información especificada en estas estructuras se usa para buscar la firma en un archivo PE. Si estos valores se dañan, es posible que un archivo aparezca como firmado sin validez. 

Para que la firma Authenticode sea correcta, debe cumplirse lo siguiente en la firma Authenticode:

- El inicio de la entrada **WIN_CERTIFICATE** en el archivo PE no se puede extenderse más allá del final del archivo ejecutable.
- La entrada **WIN_CERTIFCATE** debe estar situada al final de la imagen.
- El tamaño de la entrada **WIN_CERTIFICATE** debe ser positivo.
- La entrada **WIN_CERTIFICATE** debe comenzar después de la estructura **IMAGE_NT_HEADERS32** en los archivos ejecutables de 32 bits y la estructura IMAGE_NT_HEADERS64 en los archivos ejecutables de 64 bits.

Para obtener más información, consulta la [especificación del ejecutable del portal Authenticode](http://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx) y la [especificación del formato de archivo PE](https://msdn.microsoft.com/en-us/windows/hardware/gg463119.aspx) (en inglés). 

Ten en cuenta que SignTool.exe puede generar una lista de los archivos binarios dañados o con formato incorrecto al intentar firmar un paquete AppX. Para ello, establece la variable de entorno APPXSIP_LOG en 1 (por ejemplo, ```set APPXSIP_LOG=1```) para habilitar el registro detallado y vuelve a ejecutar SignTool.exe.

Para corregir estos archivos binarios con formato incorrecto, asegúrate de que cumplen con los requisitos anteriores.

## Consulta también

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx)
- [SignTool.exe (Sign Tool)](https://msdn.microsoft.com/library/8s9b9yaz(v=vs.110).aspx)
- [Cómo firmar un paquete de la aplicación con SignTool](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835835(v=vs.85).aspx)

<!--HONumber=Jun16_HO5-->


