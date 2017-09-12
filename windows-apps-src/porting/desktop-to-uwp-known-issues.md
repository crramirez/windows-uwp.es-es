---
author: normesta
Description: "En este artículo se describen los problemas conocidos del Puente de dispositivo de escritorio."
Search.Product: eADQiWindows 10XVcnh
title: Problemas conocidos (Puente de dispositivo de escritorio)
ms.author: normesta
ms.date: 07/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.openlocfilehash: bf0e81d4a6ff7bd091f25963b1cf26cdecc93636
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2017
---
# <a name="known-issues-desktop-bridge"></a>Problemas conocidos (Puente de dispositivo de escritorio)

En este artículo se describen los problemas conocidos del Puente de dispositivo de escritorio.

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>Pantalla azul con el código de error 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

Después de instalar o iniciar determinadas aplicaciones desde la Tienda Windows, la máquina se reinicia inesperadamente con el error: **0x139 (KERNEL\_SECURITY\_CHECK\_FAILURE)**.

Las aplicaciones afectadas conocidas incluyen Kodi, JT2Go, Ear Trumpet y Teslagrad, entre otras.

Se publicó una [actualización de Windows (versión 14393.351 - KB3197954)](https://support.microsoft.com/kb/3197954) el 27/10/16 que incluye correcciones importantes que abordan este problema. Si se produce este problema, actualiza la máquina. Si no puedes actualizar el equipo porque la máquina se reinicia antes de poder iniciar sesión, debes usar la función Restaurar sistema para recuperar el sistema a un punto anterior a la instalación de una de las aplicaciones afectadas. Para obtener información sobre cómo usar Restaurar sistema, consulta [Opciones de recuperación de Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options).

Si la actualización no soluciona el problema o no estás seguro de cómo recuperar tu equipo, ponte en contacto con el [Soporte técnico de Microsoft](https://support.microsoft.com/contactus/).

Si eres un desarrollador, quizá quieras impedir la instalación de tus aplicaciones de puente de escritorio en las versiones de Windows que no incluyen esta actualización. Ten en cuenta que, al hacerlo, la aplicación no estará disponible para los usuarios que aún no han instalado la actualización. Para limitar la disponibilidad de la aplicación a los usuarios que han instalado esta actualización, modifica el archivo AppxManifest.xml de la siguiente manera:

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

Para conocer más detalles sobre Windows Update, consulta:
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history

## <a name="common-errors-that-can-appear-when-you-sign-your-app"></a>Errores comunes que pueden aparecer al iniciar sesión en la aplicación

### <a name="publisher-and-cert-mismatch-causes-signtool-error-error-signersign-failed--21470248850x8007000b"></a>La falta de coincidencia entre el editor y el certificado produce el error de SignTool "Error: error de SignerSign()" (-2147024885/0x8007000b)

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

Para corregir estos archivos binarios con formato incorrecto, asegúrate de que cumplen los requisitos anteriores.

<span id="known-issues-anchor" />
## <a name="known-issues-with-cvbnet-and-c-uwp-projects"></a>Problemas conocidos con proyectos de C++ UWP y C#/VB.NET

Si prefieres usar un proyecto de C# para empaquetar tu aplicación, debes tener en cuenta los siguientes problemas conocidos.

- La compilación de la aplicación en la depuración genera el error: _Microsoft.Net.CoreRuntime.targets(235,5): error, que causa que no se admitan aplicaciones con archivos ejecutables de punto de entrada personalizados. Comprueba el atributo Executable del elemento Application en el manifiesto del paquete._

  Para resolver este problema, usa en su lugar el modo Lanzamiento.

- Los archivos binarios de Win32 que se almacenan en la carpeta raíz del proyecto UWP se eliminan en el Lanzamiento. El compilador de .NET Native los quitará del paquete final, lo que dará como resultado un error de validación del manifiesto, porque no se encuentra el punto de entrada del archivo ejecutable.

  Para resolver este problema, crea una subcarpeta en el proyecto para guardar archivos binarios de Win32.


## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>Recibes el error MSB4018, que indica que se produjo un error inesperado en la tarea "GenerateResource".

Esto puede suceder al intentar convertir ensamblados satélite en archivos de índice de recursos del paquete (PRI).

Conocemos este problema y estamos trabajando en una solución para más largo plazo. Como solución temporal, puedes deshabilitar el generador de recursos al agregar esta línea de XML al primer elemento PropertyGroup que aloja el archivo de proyecto:

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``
