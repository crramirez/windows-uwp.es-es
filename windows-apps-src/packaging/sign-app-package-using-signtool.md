---
author: laurenhughes
title: Firmar un paquete de aplicación con SignTool
description: Usa SignTool para firmar manualmente un paquete de aplicación con un certificado.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 171f332d-2a54-4c68-8aa0-52975d975fb1
ms.localizationpriority: medium
ms.openlocfilehash: c238855f4f018e8e3142509842221c6b9d97fae3
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "4267856"
---
# <a name="sign-an-app-package-using-signtool"></a>Firmar un paquete de aplicación con SignTool


**SignTool** es una herramienta de línea de comandos que se usa para firmar un paquete de aplicación o un lote de aplicaciones con un certificado. El certificado puede crearlo el usuario (con fines de prueba) o emitirlo una empresa (para distribución). Firmar un paquete de aplicación proporciona al usuario la verificación de que los datos de la aplicación no se han modificado después de haberse firmado, a la vez que confirma la identidad del usuario o la empresa que la firmara. **SignTool** puede firmar paquetes de aplicación y lotes de aplicaciones cifrados o sin cifrar.

> [!IMPORTANT] 
> Si usaste Visual Studio para desarrollar tu aplicación, es recomendable usar el Asistente de Visual Studio para crear y firmar el paquete de la aplicación. Para más información, consulta [Empaquetado de aplicaciones para UWP con Visual Studio](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

Para obtener más información sobre la firma de código y los certificados en general, consulta [Introducción a la firma de código](https://msdn.microsoft.com/library/windows/desktop/aa380259.aspx#introduction_to_code_signing).

## <a name="prerequisites"></a>Requisitos previos
- **Una aplicación empaquetada**  
    Para obtener más información sobre cómo crear un paquete de aplicación de forma manual, consulta [Crear un paquete de la aplicación con la herramienta MakeAppx.exe](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool). 

- **Un certificado de firma válido**  
    Para obtener más información sobre la creación o la importación de un certificado de firma válido, consulta [Crear o importar un certificado para la firma del paquete](https://msdn.microsoft.com/windows/uwp/packaging/create-certificate-package-signing).

- **SignTool.exe**  
    En función de la ruta de acceso de instalación del SDK, aquí es donde está **SignTool** en tu equipo Windows 10:
    - x86: C:\Archivos de programa (x86)\Windows Kits\10\bin\x86\SignTool.exe
    - x64: C:\Archivos de programa (x86)\Windows Kits\10\bin\x64\SignTool.exe

## <a name="using-signtool"></a>Uso de SignTool

**SignTool** puede usarse para firmar archivos, verificar firmas o marcas de tiempo, quitar firmas y mucho más. Para firmar un paquete de aplicación, nos centraremos en el comando **sign**. Para obtener información completa sobre **SignTool**, consulta la página de referencia de [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx). 

### <a name="determine-the-hash-algorithm"></a>Determinar el algoritmo hash
Al usar **SignTool** para firmar el paquete de aplicación o el lote de aplicaciones, el algoritmo hash usado en **SignTool** debe ser el mismo que se usara para empaquetar la aplicación. Por ejemplo, si usaste **MakeAppx.exe** para crear el paquete de la aplicación con la configuración predeterminada, debes especificar SHA256 al usar **SignTool**, ya que es el algoritmo predeterminado usado por **MakeAppx.exe**.

Para saber qué algoritmo hash se ha usado al empaquetar la aplicación, extrae el contenido del paquete de la aplicación e inspecciona el archivo AppxBlockMap.xml. Para obtener información sobre cómo desempaquetar o extraer un paquete de aplicación, consulta [Extraer archivos de un paquete o lote](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool#extract-files-from-a-package-or-bundle). El método de hash se encuentra en el elemento BlockMap y tiene este formato:
```
<BlockMap xmlns="http://schemas.microsoft.com/appx/2010/blockmap" 
HashMethod="http://www.w3.org/2001/04/xmlenc#sha256">
```

Esta tabla muestra cada valor de HashMethod y su algoritmo hash correspondiente:


| Valor de HashMethod                              | Algoritmo hash |
|-----------------------------------------------|----------------|
| http://www.w3.org/2001/04/xmlenc#sha256       | SHA256         |
| http://www.w3.org/2001/04/xmldsig-more#sha384 | SHA384         |
| http://www.w3.org/2001/04/xmlenc#sha512       | SHA512         |

> [!NOTE]
> Como el algoritmo predeterminado de **SignTool** es SHA1 (no disponible en **MakeAppx.exe**), siempre debes especificar un algoritmo hash al usar **SignTool**.

### <a name="sign-the-app-package"></a>Firma del paquete de la aplicación

Una vez que tengas todos los requisitos previos y que hayas determinado qué algoritmo hash se usó para empaquetar tu aplicación, estarás listo para firmarlo. 

La sintaxis general de la línea de comandos para la firma del paquete de **SignTool** es:
```
SignTool sign [options] <filename(s)>
```

El certificado usado para firmar la aplicación debe ser un archivo .pfx o instalarse en un almacén de certificados.

Para firmar el paquete de la aplicación con un certificado de un archivo .pfx, usa la sintaxis siguiente:
```
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.msix
```
Ten en cuenta que la opción `/a` permite a **SignTool** elegir el mejor certificado automáticamente.

Si el certificado no es un archivo .pfx, usa la sintaxis siguiente:
```
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.msix
```

Como alternativa, puedes especificar el hash SHA1 del certificado deseado en lugar de &lt;Name of Certificate&gt; con esta sintaxis:
```
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.msix
```

Ten en cuenta que algunos certificados no usan contraseña. Si el certificado no tiene ninguna contraseña, omite "/p &lt;Your Password&gt;" de los comandos de muestra.

Cuando el paquete de la aplicación está firmado con un certificado válido, estarás listo para cargar el paquete en la Store. Para obtener más orientación sobre cómo cargar y enviar aplicaciones a la Store, consulta [Envíos de aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/app-submissions).

## <a name="common-errors-and-troubleshooting"></a>Errores comunes y solución de problemas
Los tipos más comunes de errores al usar **SignTool** son internos y por lo general tienen este aspecto:

```
SignTool Error: An unexpected internal error has occurred.
Error information: "Error: SignerSign() failed." (-2147024885 / 0x8007000B) 
```

Si el código de error empieza por 0x8008, como 0x80080206 (APPX_E_CORRUPT_CONTENT), el paquete que se está firmando no es válido. Si recibes este tipo de error, debes volver a compilar el paquete y ejecutar **SignTool** otra vez.

**SignTool** tiene disponible una opción de depuración para mostrar errores de certificado y filtrado. Para usar la característica de depuración, coloca la opción `/debug` directamente después de `sign`, seguida por el comando **SignTool** completo.
```
SignTool sign /debug [options]
``` 

Un error más común es 0x8007000B. Para este tipo de error, puedes encontrar más información en el registro de eventos.
 
Para encontrar más información en el registro de eventos:
- Ejecuta Eventvwr.msc.
- Abre el registro de eventos: Visor de eventos (locales) -> Registros de aplicaciones y servicios -> Microsoft -> Windows -> AppxPackagingOM -> Microsoft-Windows-AppxPackaging/Operational.
- Busca el evento de error más reciente.

Normalmente, el error interno 0x8007000B se corresponde a uno de estos valores:

| **Id. de evento** | **Ejemplo de cadena de evento** | **Sugerencia** |
|--------------|--------------------------|----------------|
| 150          | Error 0x8007000B: el nombre del editor del manifiesto de la aplicación (CN = Contoso) debe coincidir con el nombre del sujeto del certificado de firma (CN = Contoso, C = EE. UU.). | El nombre del editor del manifiesto de la aplicación debe coincidir exactamente con el nombre de sujeto de la firma.               |
| 151.          | Error 0x8007000B: El método de hash de la firma especificado (SHA512) debe coincidir con el método de hash usado en la asignación de bloques del paquete de la aplicación (SHA256).     | El hashAlgorithm especificado en el parámetro /fd es incorrecto. Vuevle a ejecutar **SignTool** con un hashAlgorithm que coincida con la asignación de bloques del paquete de la aplicación (se usada para crear el paquete de la aplicación).  |
| 152          | Error 0x8007000B: El contenido del paquete de la aplicación debe validarse respecto a su asignación de bloques.                                                           | El paquete de la aplicación está dañado y debe volver a compilarse para generar una nueva asignación de bloques. Para obtener más información sobre cómo crear un paquete de aplicación, consulta [Crear un paquete de la aplicación con la herramienta MakeAppx.exe](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool). |