---
author: normesta
Description: Run your packaged app and see how it looks without having to sign it. Then, set breakpoints and step through code. When you're ready to test your app in a production environment, sign your app and then install it.
Search.Product: eADQiWindows 10XVcnh
title: Ejecutar, depurar y probar una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)
ms.author: normesta
ms.date: 08/31/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: b5110eebde087593f07704e89c2e4708b2fcbb8b
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "5130494"
---
# <a name="run-debug-and-test-a-packaged-desktop-application"></a>Ejecutar, depurar y probar una aplicación de escritorio empaquetada

Ejecutar la aplicación empaquetada y ver su aspecto sin tener que iniciar sesión. A continuación, establece los puntos de interrupción y revisa el código. Cuando estés listo para probar la aplicación en un entorno de producción, firmar la aplicación e instalarla. En este tema se explica cómo realizar cada uno de estos pasos.

<a id="run-app" />

## <a name="run-your-application"></a>Ejecutar la aplicación

Puedes ejecutar la aplicación para probarla de forma local sin tener que obtener un certificado y firmarlo. Cómo se ejecuta la aplicación dependerá de la herramienta utilizada para crear el paquete.

### <a name="you-created-the-package-by-using-visual-studio"></a>Has creado el paquete con Visual Studio

Establece el proyecto de empaquetado como el proyecto de inicio y presiona CTRL+F5 para iniciar tu aplicación.

### <a name="you-created-the-package-manually-or-by-using-the-desktop-app-converter"></a>Has creado el paquete manualmente o con Desktop App Converter

Abre un símbolo del sistema de Windows PowerShell y en la subcarpeta **PackageFiles** de la carpeta de salida, ejecuta este cmdlet:

```
Add-AppxPackage –Register AppxManifest.xml
```
Para iniciar la aplicación, búscala en el menú Inicio de Windows.

![Paquete de la aplicación en el menú Inicio](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> Una aplicación empaquetada siempre se ejecuta como usuario interactivo y cualquier unidad que se instala la aplicación empaquetada en debe tener el formato al formato NTFS.

## <a name="debug-your-app"></a>Depurar la aplicación

Cómo depurar la aplicación dependerá de la herramienta utilizada para crear el paquete.

Si has creado el paquete utilizando el nuevo [proyecto de empaquetado](desktop-to-uwp-packaging-dot-net.md#new-packaging-project) disponible en la versión 15.4 de Visual Studio 2017, establece el proyecto de empaquetado como proyecto de inicio y luego presiona F5 para depurar la aplicación.

Si has creado el paquete utilizando otra herramienta, sigue estos pasos.

1. Asegúrate de que inicias tu aplicación empaquetada al menos una vez por lo que está instalado en el equipo local.

   Consulta la sección anterior [Ejecutar la aplicación](#run-app).

2. Inicia Visual Studio.

   Si desea depurar la aplicación con permisos elevados, inicie Visual Studio mediante la opción de **Ejecutar como administrador** .

3. En Visual Studio, elige **Depurar**->**Otros destinos de depuración**->**Depurar paquete de aplicaciones instalado**.

4. En la lista **Paquetes de aplicación instalados**, selecciona el paquete de la aplicación y luego elige el botón **Adjuntar**.

#### <a name="modify-your-application-in-between-debug-sessions"></a>Modificar la aplicación entre sesiones de depuración

Si realizas los cambios en la aplicación para corregir errores, vuelve a empaquetarla mediante la herramienta MakeAppx. Consulta [Ejecutar la herramienta MakeAppX](desktop-to-uwp-manual-conversion.md#make-appx)

### <a name="debug-the-entire-application-lifecycle"></a>Depurar el ciclo de vida de toda la aplicación

En algunos casos, es posible que quieras precisa control sobre el proceso de depuración, incluida la posibilidad de depurar la aplicación antes de que empiece.

Puedes usar [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) para obtener el control total sobre el ciclo de vida de aplicación incluyendo suspender, reanudar y finalización.

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) se incluye en Windows SDK.

## <a name="test-your-app"></a>Probar la aplicación

Para probar la aplicación en una configuración realista mientras la preparas para su distribución, es mejor firmar la aplicación e instalarla.

### <a name="test-an-application-that-you-packaged-by-using-visual-studio"></a>Probar una aplicación empaquetada con Visual Studio

Visual Studio, firma la aplicación con un certificado de prueba. Encontrarás dicho certificado en la carpeta de salida que genera el asistente **Crear paquetes de aplicaciones**. El archivo de certificado tiene la extensión *.cer* y tendrás que instalar el certificado al almacén de **Entidades de certificación raíz de confianza** en el equipo que quieres probar la aplicación. Consulta [Realizar la instalación de prueba del paquete de la aplicación](../packaging/packaging-uwp-apps.md#sideload-your-app-package).

### <a name="test-an-application-that-you-packaged-by-using-the-desktop-app-converter-dac"></a>Probar una aplicación empaquetada utilizando Desktop App Converter (DAC)

Si empaquetaste la aplicación mediante Desktop App Converter, puedes usar el ``sign`` parámetro para iniciar sesión automáticamente la aplicación mediante el uso de un certificado generado. Tendrás que instalar el certificado y, a continuación, instalar la aplicación. Consulta [Ejecutar la aplicación empaquetada](desktop-to-uwp-run-desktop-app-converter.md#run-app).   


### <a name="manually-sign-apps-optional"></a>Firmar manualmente aplicaciones (opcional)

También puedes firmar la aplicación manualmente. A continuación te indicamos cómo

1. Crea un certificado. Consulta [Crear un certificado](../packaging/create-certificate-package-signing.md).

2. Instala el certificado en el almacén de certificados **Raíz de confianza** o **Personas de confianza** del sistema.

3. Firmar la aplicación con ese certificado, consulta el [signo de un paquete de la aplicación con SignTool](../packaging/sign-app-package-using-signtool.md).

  > [!IMPORTANT]
  > Asegúrate de que el nombre del publicador del certificado coincide con el de la aplicación.

    **Muestra relacionada**

    [SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-application-for-windows-10-s"></a>Probar la aplicación para Windows 10 S

Antes de publicar la aplicación, asegúrate de que funcionará correctamente en dispositivos que ejecutan Windows 10 S. De hecho, si vas a publicar la aplicación en la Microsoft Store, debes hacerlo porque es un requisito de la tienda. Las aplicaciones que no funcionan correctamente en dispositivos que ejecutan Windows 10S no estarán certificadas.

Consulta [tu aplicación de Windows para Windows 10 S de prueba](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s).

### <a name="run-another-process-inside-the-full-trust-container"></a>Ejecutar otro proceso dentro del contenedor de plena confianza

Puedes invocar procesos personalizados dentro del contenedor de un paquete de la aplicación especificada. Esto puede ser útil para los escenarios de prueba (por ejemplo, si tienes una herramienta de ejecución de pruebas personalizada y quieres probar la salida de la aplicación). Para ello, usa el cmdlet ```Invoke-CommandInDesktopPackage``` de PowerShell:

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>Pasos siguientes

**Encuentra respuestas a tus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Enviar comentarios o realizar sugerencias acerca de las características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
