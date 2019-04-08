---
Description: En este artículo se describen los problemas conocidos del Puente de dispositivo de escritorio.
Search.Product: eADQiWindows 10XVcnh
title: Problemas conocidos (Puente de dispositivo de escritorio)
ms.date: 06/20/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.localizationpriority: medium
ms.openlocfilehash: 9c437e30db7007a6889a822d7d2219f1647bb3d8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637100"
---
# <a name="known-issues-with-packaged-desktop-applications"></a>Problemas conocidos con paquetes de aplicaciones de escritorio

En este artículo contiene problemas conocidos que pueden producirse al crear un paquete de aplicación de Windows para su aplicación de escritorio.

<a id="app-converter" />

## <a name="known-issues-with-the-desktop-app-converter"></a>Problemas conocidos con Desktop App Converter.

### <a name="ecreatingisolatedenvfailed-an-estartingisolatedenvfailed-errors"></a>Errores E_CREATING_ISOLATED_ENV_FAILED y E_STARTING_ISOLATED_ENV_FAILED    

Si se produce cualquiera de estos errores, asegúrate de que estás usando una imagen base válida del [centro de descarga](https://aka.ms/converterimages).
Si estás usando una imagen de base válida, prueba a usar ``-Cleanup All`` en el comando.
Si esto no funciona, envíanos tus registros a converter@microsoft.com para que podamos investigar este error.

### <a name="new-containernetwork-the-object-already-exists-error"></a>New-ContainerNetwork: Ya existe el objeto error

Es posible que se produzca este error al configurar una nueva imagen base. Esto puede suceder si tienes un paquete piloto de Windows Insider en un equipo de desarrollador que anteriormente tenía Desktop App Converter instalado.

Para resolver este problema, prueba a ejecutar el comando `Netsh int ipv4 reset` desde un símbolo del sistema con privilegios elevados y, a continuación, reinicia el equipo.

### <a name="your-net-application-is-compiled-with-the-anycpu-build-option-and-fails-to-install"></a>La aplicación de .NET se compila con la opción de compilación "CualquierCPU" y no se puede instalar

Esto puede suceder si el archivo ejecutable principal (o cualquiera de las dependencias) está colocado en cualquier lugar de la jerarquía de carpetas **Archivos de programa** o **Windows\System32**.

Para resolver este problema, intenta usar un instalador de escritorio específico para cada arquitectura (32 o 64 bits) y así poder generar un paquete de la aplicación de Windows.

### <a name="publishing-public-side-by-side-fusion-assemblies-wont-work"></a>La publicación en paralelo de ensamblados Fusion públicos no funciona.

 Durante la instalación, una aplicación puede publicar ensamblados Fusion públicos en paralelo, accesibles para cualquier otro proceso. Durante la creación de contexto de activación de proceso, estos ensamblados se recuperan mediante un proceso del sistema denominado CSRSS.exe. Cuando esto se realiza para un proceso convertido, se produce un error en la creación de contexto de activación y en la carga del módulo de estos ensamblados. Los ensamblados Fusion en paralelo se registran en las siguientes ubicaciones:
  + Registro: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + Sistema de archivos: % windir %\\SideBySide

Esta es una limitación conocida y no existe ninguna solución actualmente. Dicho esto, los ensamblados de bandeja de entrada, como ComCtl, se envían con el sistema operativo, por lo que se puede depender de ellos con seguridad.

### <a name="error-found-in-xml-the-executable-attribute-is-invalid---the-value-myappexe-is-invalid-according-to-its-datatype"></a>Error en XML. El atributo "Executable" no es válido: el valor "MyApp.EXE" no es válido según el tipo de datos

Esto puede suceder si los archivos ejecutables de la aplicación tienen una extensión **. EXE** escrita en mayúsculas. Sin embargo, no deberían afectar las mayúsculas y minúsculas de esta extensión si se ejecuta la aplicación, esto puede causar la DAC generar este error.

Para resolver este problema, pruebe a especificar la **- AppExecutable** marca cuando se empaqueta y usar el ".exe" como la extensión del archivo ejecutable principal de minúsculas (por ejemplo: MYAPP.exe).    O bien puede cambiar las mayúsculas y minúsculas para todos los archivos ejecutables en la aplicación de minúsculas a mayúsculas (por ejemplo: desde. EXE para .exe).

### <a name="corrupted-or-malformed-authenticode-signatures"></a>Firmas Authenticode dañadas o con formato incorrecto

En esta sección se incluye información detallada sobre cómo identificar problemas con los archivos portables ejecutables (PE) del paquete de la aplicación de Windows que pueden contener firmas Authenticode dañadas o con formato incorrecto. Las firmas Authenticode no válidas en los archivos PE, que pueden estar en cualquier formato binario (como .exe, .dll, chm, etc.) impedirán que el paquete se firme correctamente y, por consiguiente, que se pueda implementar desde un paquete de la aplicación de Windows.

La ubicación de la firma Authenticode de un archivo PE se especifica mediante la entrada de la tabla de certificados en los directorios de datos de encabezado opcionales y la tabla de certificados de atributos asociada. Durante la comprobación de la firma, la información especificada en estas estructuras se usa para buscar la firma en un archivo PE. Si estos valores se dañan, es posible que un archivo aparezca como firmado sin validez.

Para que la firma Authenticode sea correcta, debe cumplirse lo siguiente en la firma Authenticode:

- El inicio de la entrada **WIN_CERTIFICATE** en el archivo PE no se puede extenderse más allá del final del archivo ejecutable.
- La entrada **WIN_CERTIFCATE** debe estar situada al final de la imagen.
- El tamaño de la entrada **WIN_CERTIFICATE** debe ser positivo.
- La entrada **WIN_CERTIFICATE** debe comenzar después de la estructura **IMAGE_NT_HEADERS32** en los archivos ejecutables de 32 bits y la estructura IMAGE_NT_HEADERS64 en los archivos ejecutables de 64 bits.

Para obtener más información, consulta la [especificación del ejecutable del portal Authenticode](https://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx) y la [especificación del formato de archivo PE](https://msdn.microsoft.com/windows/hardware/gg463119.aspx) (en inglés).

Ten en cuenta que SignTool.exe puede generar una lista de los archivos binarios dañados o con formato incorrecto al intentar firmar un paquete de la aplicación de Windows. Para ello, establece la variable de entorno APPXSIP_LOG en 1 (por ejemplo, ```set APPXSIP_LOG=1```) para habilitar el registro detallado y vuelve a ejecutar SignTool.exe.

Para corregir estos archivos binarios con formato incorrecto, asegúrate de que cumplen con los requisitos anteriores.

## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>Recibes el error MSB4018, que indica que se produjo un error inesperado en la tarea "GenerateResource".

Esto puede suceder al intentar convertir ensamblados satélite en archivos de índice de recursos del paquete (PRI).

Conocemos este problema y estamos trabajando en una solución para más largo plazo. Como solución temporal, puedes deshabilitar el generador de recursos al agregar esta línea de XML al primer elemento PropertyGroup que aloja el archivo de proyecto:

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>Pantalla azul con el código de error 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

Después de instalar o iniciar determinadas aplicaciones desde la Microsoft Store, la máquina puede reiniciar inesperadamente con el error: **0 x 139 (KERNEL\_seguridad\_comprobar\_ error)**.

Las aplicaciones afectadas conocidas incluyen Kodi, JT2Go, Ear Trumpet y Teslagrad, entre otras.

Se publicó una [actualización de Windows (versión 14393.351 - KB3197954)](https://support.microsoft.com/kb/3197954) el 27/10/16 que incluye correcciones importantes que abordan este problema. Si se produce este problema, actualiza la máquina. Si no puedes actualizar el equipo porque la máquina se reinicia antes de poder iniciar sesión, debes usar la función Restaurar sistema para recuperar el sistema a un punto anterior a la instalación de una de las aplicaciones afectadas. Para obtener información sobre cómo usar Restaurar sistema, consulta [Opciones de recuperación de Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options).

Si la actualización no soluciona el problema o no estás seguro de cómo recuperar tu equipo, ponte en contacto con el [Soporte técnico de Microsoft](https://support.microsoft.com/contactus/).

Si eres un desarrollador, quizá quieras impedir la instalación de tu aplicación empaquetada en las versiones de Windows que no incluyen esta actualización. Tenga en cuenta que al hacerlo, la aplicación no estará disponible para los usuarios que aún no ha instalado la actualización. Para limitar la disponibilidad de la aplicación a los usuarios que tienen instalada esta actualización, modifique el archivo AppxManifest.xml como sigue:

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

Para conocer más detalles sobre Windows Update, consulta:
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history

## <a name="common-errors-that-can-appear-when-you-sign-your-app"></a>Errores comunes que pueden aparecer al iniciar sesión en la aplicación

### <a name="publisher-and-cert-mismatch-causes-signtool-error-error-signersign-failed--21470248850x8007000b"></a>Error Signtool hace que el publicador y el certificado de falta de coincidencia "Error: No se pudo SignerSign()"(-2147024885/0x8007000b)

La entrada Editor del manifiesto del paquete de la aplicación de Windows debe coincidir con el valor de Firmante del certificado con que firmas.  Puedes usar cualquiera de los métodos siguientes para ver el firmante del certificado.

**Opción 1: PowerShell**

Ejecuta el siguiente comando de PowerShell. Puedes usar .cer o .pfx como el archivo de certificado, ya que tienen la misma información de editor.

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**Opción 2: Explorador de archivos**

Haz doble clic en el certificado en el Explorador de archivos, selecciona la pestaña *Detalles* y después el campo *Firmante* en la lista. A continuación, puedes copiar el contenido.

**Opción 3: CertUtil**

Ejecute **certutil** desde la línea de comandos en el archivo PFX y copie el *asunto* arrastrándolo desde la salida.

```cmd
certutil -dump <cert_file.pfx>
```

<a id="bad-pe-cert" />

### <a name="bad-pe-certificate-0x800700c1"></a>Certificado de PE no válido (0x800700C1)

Esto puede ocurrir cuando el paquete debe contener un archivo binario que tiene un certificado está dañado. Le mostramos algunas de las razones por qué esto puede ocurrir:

* El inicio del certificado no está al final de una imagen.  

* El tamaño del certificado no es positivo.

* No es el inicio del certificado después de la `IMAGE_NT_HEADERS32` estructura para un ejecutable de 32 bits o después de la `IMAGE_NT_HEADERS64` estructura para un ejecutable de 64 bits.

* El puntero de certificado no está alineado correctamente para una estructura WIN_CERTIFICATE.

Para buscar archivos que contienen un certificado de PE incorrecto, abra un **símbolo**y establezca la variable de entorno denominada `APPXSIP_LOG` en un valor de 1.

```
set APPXSIP_LOG=1
```

A continuación, en el **símbolo**, firmar la aplicación de nuevo. Por ejemplo:

```
signtool.exe sign /a /v /fd SHA256 /f APPX_TEST_0.pfx C:\Users\Contoso\Desktop\pe\VLC.appx
```

Se mostrará información acerca de los archivos que contienen un certificado de PE incorrecto en el **ventana de consola**. Por ejemplo:

```
...

ERROR: [AppxSipCustomLoggerCallback] File has malformed certificate: uninstall.exe

...   
```
## <a name="next-steps"></a>Pasos siguientes

**Encuentre respuestas a sus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Proporcionar comentarios o hacer sugerencias**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
