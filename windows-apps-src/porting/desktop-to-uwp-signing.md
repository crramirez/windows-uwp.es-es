---
author: normesta
Description: "En este artículo se explica cómo firmar una aplicación de escritorio que se ha convertido a la Plataforma universal de Windows (UWP)."
Search.Product: eADQiWindows 10XVcnh
title: Puente de dispositivo de escritorio a UWP, firma
ms.author: normesta
ms.date: 03/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 232c3012-71ff-4f76-a81e-b1758febb596
ms.openlocfilehash: bed23b75a966e3a858d1e34a686fa3ba2730354c
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="desktop-to-uwp-bridge-sign"></a>Puente de dispositivo de escritorio a UWP: firma

En este artículo se explica cómo firmar una aplicación de escritorio que se ha convertido a la Plataforma universal de Windows (UWP). Debes firmar el paquete de la aplicación de Windows con un certificado para poder implementarla.

## <a name="automatically-sign-using-the-desktop-app-converter-dac"></a>Firmar automáticamente con Desktop App Converter (DAC)

Usa la marca ```-Sign``` cuando ejecutes DAC para firmar automáticamente el paquete de la aplicación de Windows. Para conocer más detalles, consulta [Vista previa de Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md).

## <a name="manually-sign-using-signtoolexe"></a>Firmar manualmente mediante SignTool.exe

Primero, crea un certificado con MakeCert.exe. Si se te solicita que especifiques una contraseña, selecciona "Ninguna".

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
```

A continuación, copia la información de las claves pública y privada en el certificado mediante pvk2pfx.exe.

```cmd
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
```
Por último, usa SignTool.exe para firmar el paquete de la aplicación de Windows con el certificado.

```cmd
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```

Para obtener más información, consulta [Cómo firmar un paquete de la aplicación con SignTool](https://msdn.microsoft.com/library/windows/desktop/jj835835.aspx).

Las tres herramientas anteriores se incluyen con el SDK de Microsoft Windows 10. Para llamarlas directamente, llama al script ```C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\Tools\VsDevCmd.bat``` desde un símbolo del sistema.

## <a name="common-errors"></a>Errores comunes

### <a name="publisher-and-cert-mismatch-causes-signtool-error-error-signersign-failed--21470248850x8007000b"></a>La falta de coincidencia entre el editor y el certificado produce el error de SignTool "Error: Error de SignerSign()" (-2147024885/0x8007000b)

La entrada Editor del manifiesto del paquete de la aplicación de Windows debe coincidir con el valor de Firmante del certificado con que firmas.  Puedes usar cualquiera de los métodos siguientes para ver el firmante del certificado.

**Opción 1: PowerShell**

Ejecuta el siguiente comando de PowerShell. Puedes usar .cer o .pfx como el archivo de certificado, ya que tienen la misma información de editor.

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**Opción 2: Explorador de archivos**

Haz doble clic en el certificado en el Explorador de archivos, selecciona la pestaña *Detalles* y después el campo *Firmante* en la lista. A continuación, puedes copiar el contenido.

**Opción 3: CertUtil**

Ejecuta **certutil** desde la línea de comandos en el archivo PFX y copia el valor del campo *Asunto* de la salida.

```cmd
certutil -dump <cert_file.pfx>
```

### <a name="corrupted-or-malformed-authenticode-signatures"></a>Firmas Authenticode dañadas o con formato incorrecto

En esta sección se incluye información detallada sobre cómo identificar problemas con los archivos portables ejecutables (PE) del paquete de la aplicación de Windows que pueden contener firmas Authenticode dañadas o con formato incorrecto. Las firmas Authenticode no válidas en los archivos PE, que pueden estar en cualquier formato binario (como .exe, .dll, chm, etc.) impedirán que el paquete se firme correctamente y, por consiguiente, que se pueda implementar desde un paquete de la aplicación de Windows.

La ubicación de la firma Authenticode de un archivo PE se especifica mediante la entrada de la tabla de certificados en los directorios de datos de encabezado opcionales y la tabla de certificados de atributos asociada. Durante la comprobación de la firma, la información especificada en estas estructuras se usa para buscar la firma en un archivo PE. Si estos valores se dañan, es posible que un archivo aparezca como firmado sin validez.

Para que la firma Authenticode sea correcta, debe cumplirse lo siguiente en la firma Authenticode:

- El inicio de la entrada **WIN_CERTIFICATE** en el archivo PE no se puede extenderse más allá del final del archivo ejecutable.
- La entrada **WIN_CERTIFCATE** debe estar situada al final de la imagen.
- El tamaño de la entrada **WIN_CERTIFICATE** debe ser positivo.
- La entrada **WIN_CERTIFICATE** debe comenzar después de la estructura **IMAGE_NT_HEADERS32** en los archivos ejecutables de 32bits y la estructura IMAGE_NT_HEADERS64 en los archivos ejecutables de 64bits.

Para obtener más información, consulta la [especificación del ejecutable del portal Authenticode](http://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx) y la [especificación del formato de archivo PE](https://msdn.microsoft.com/windows/hardware/gg463119.aspx) (en inglés).

Ten en cuenta que SignTool.exe puede generar una lista de los archivos binarios dañados o con formato incorrecto al intentar firmar un paquete de la aplicación de Windows. Para ello, establece la variable de entorno APPXSIP_LOG en 1 (por ejemplo, ```set APPXSIP_LOG=1```) para habilitar el registro detallado y vuelve a ejecutar SignTool.exe.

Para corregir estos archivos binarios con formato incorrecto, asegúrate de que cumplen con los requisitos anteriores.

## <a name="see-also"></a>Consulta también

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)
- [SignTool.exe (Sign Tool)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)
- [Cómo firmar un paquete de la aplicación con SignTool](https://msdn.microsoft.com/library/windows/desktop/jj835835.aspx)
