---
Description: Ejecuta la aplicación empaquetada y revisa su aspecto sin tener que iniciar sesión en ella. A continuación, establece los puntos de interrupción y revisa el código. Cuando estés listo para probar la aplicación en un entorno de producción, firma la aplicación e instálala.
Search.Product: eADQiWindows 10XVcnh
title: Ejecutar, depurar y probar una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)
ms.date: 08/31/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: 8b2350c8164548121baec231335e747166f1c082
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589760"
---
# <a name="run-debug-and-test-a-packaged-desktop-application"></a>Ejecutar, depurar y probar una aplicación de escritorio empaquetada

Ejecute la aplicación empaquetada y ver su aspecto sin necesidad de firmarlo. A continuación, establece los puntos de interrupción y revisa el código. Cuando esté listo para probar la aplicación en un entorno de producción, firmar la aplicación y, a continuación, vuelva a instalarlo. En este tema se explica cómo realizar cada uno de estos pasos.

<a id="run-app" />

## <a name="run-your-application"></a>Ejecute la aplicación

Puede ejecutar la aplicación para probarla localmente sin tener que obtener un certificado y firmarlo. Cómo ejecutar la aplicación depende de qué herramienta se utiliza para crear el paquete.

### <a name="you-created-the-package-by-using-visual-studio"></a>Has creado el paquete con Visual Studio

Establece el proyecto de empaquetado como el proyecto de inicio y presiona CTRL+F5 para iniciar tu aplicación.

### <a name="you-created-the-package-manually-or-by-using-the-desktop-app-converter"></a>Has creado el paquete manualmente o con Desktop App Converter

Abre un símbolo del sistema de Windows PowerShell y en la subcarpeta **PackageFiles** de la carpeta de salida, ejecuta este cmdlet:

```
Add-AppxPackage –Register AppxManifest.xml
```
Para iniciar la aplicación, búscala en el menú Inicio de Windows.

![Aplicación empaquetada en el menú Inicio](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> Una aplicación empaquetada siempre se ejecuta como un usuario interactivo y cualquier unidad que se instala la aplicación empaquetada a debe tener el formato al formato NTFS.

## <a name="debug-your-app"></a>Depurar la aplicación

Cómo depurar la aplicación depende de qué herramienta se utiliza para crear el paquete.

Si has creado el paquete utilizando el nuevo [proyecto de empaquetado](desktop-to-uwp-packaging-dot-net.md#new-packaging-project) disponible en la versión 15.4 de Visual Studio 2017, establece el proyecto de empaquetado como proyecto de inicio y luego presiona F5 para depurar la aplicación.

Si has creado el paquete utilizando otra herramienta, sigue estos pasos.

1. Asegúrese de iniciar la aplicación empaquetada al menos una vez para que se instala en el equipo local.

   Consulta la sección anterior [Ejecutar la aplicación](#run-app).

2. Inicia Visual Studio.

   Si desea depurar la aplicación con permisos elevados, inicie Visual Studio mediante el **ejecutar como administrador** opción.

3. En Visual Studio, elige **Depurar**->**Otros destinos de depuración**->**Depurar paquete de aplicaciones instalado**.

4. En la lista **Paquetes de aplicación instalados**, selecciona el paquete de la aplicación y luego elige el botón **Adjuntar**.

#### <a name="modify-your-application-in-between-debug-sessions"></a>Modificar la aplicación entre las sesiones de depuración

Si realiza los cambios en la aplicación para corregir los errores, vuelva a empaquetar con la herramienta MakeAppx. Consulta [Ejecutar la herramienta MakeAppX](desktop-to-uwp-manual-conversion.md#make-appx)

### <a name="debug-the-entire-application-lifecycle"></a>Depurar el ciclo de vida de toda la aplicación

En algunos casos, es posible que desee control más preciso sobre el proceso de depuración, incluida la capacidad para depurar la aplicación antes de que comience.

Puede usar [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) para obtener un control total sobre el ciclo de vida de aplicación incluida la suspensión, reanudación y la terminación.

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) se incluye en Windows SDK.

## <a name="test-your-app"></a>Probar la aplicación

Para probar la aplicación en una configuración realista mientras se prepara para su distribución, es mejor firmar la aplicación y, a continuación, vuelva a instalarlo.

### <a name="test-an-application-that-you-packaged-by-using-visual-studio"></a>Probar una aplicación que incluye con Visual Studio

Visual Studio inicia la aplicación con un certificado de prueba. Encontrarás dicho certificado en la carpeta de salida que genera el asistente **Crear paquetes de aplicaciones**. El archivo de certificado tiene el *.cer* extensión y tendrá que instalar ese certificado en el **entidades emisoras raíz de confianza** almacenar en el equipo que desea probar la aplicación en. Consulta [Realizar la instalación de prueba del paquete de la aplicación](../packaging/packaging-uwp-apps.md#sideload-your-app-package).

### <a name="test-an-application-that-you-packaged-by-using-the-desktop-app-converter-dac"></a>Probar una aplicación que empaqueta mediante el uso de Desktop App Converter (DAC)

Si empaqueta la aplicación mediante el uso de la Desktop App Converter, puede usar el ``sign`` parámetro para iniciar sesión automáticamente la aplicación mediante el uso de un certificado generado. Tendrás que instalar el certificado y, a continuación, instalar la aplicación. Consulta [Ejecutar la aplicación empaquetada](desktop-to-uwp-run-desktop-app-converter.md#run-app).   


### <a name="manually-sign-apps-optional"></a>Firmar manualmente aplicaciones (opcional)

También puede firmar la aplicación manualmente. A continuación te indicamos cómo

1. Crea un certificado. Consulta [Crear un certificado](../packaging/create-certificate-package-signing.md).

2. Instala el certificado en el almacén de certificados **Raíz de confianza** o **Personas de confianza** del sistema.

3. Iniciar la aplicación mediante el uso de ese certificado, consulte [firmar un paquete de aplicación mediante SignTool](../packaging/sign-app-package-using-signtool.md).

  > [!IMPORTANT]
  > Asegúrate de que el nombre del publicador del certificado coincide con el de la aplicación.

**Ejemplo relacionado**

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-application-for-windows-10-s"></a>Probar la aplicación para Windows 10 S

Antes de publicar la aplicación, asegúrese de que funcionará correctamente en los dispositivos que ejecutan Windows 10 S. De hecho, si tiene previsto publicar la aplicación en la Microsoft Store, debe hacerlo porque es un requisito de la tienda. Las aplicaciones que no funcionan correctamente en dispositivos que ejecutan Windows 10 S no estarán certificadas.

Consulte [probar la aplicación de Windows para Windows 10 S](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s).

### <a name="run-another-process-inside-the-full-trust-container"></a>Ejecutar otro proceso dentro del contenedor de plena confianza

Puedes invocar procesos personalizados dentro del contenedor de un paquete de la aplicación especificada. Esto puede ser útil para los escenarios de prueba (por ejemplo, si tienes una herramienta de ejecución de pruebas personalizada y quieres probar la salida de la aplicación). Para ello, usa el cmdlet ```Invoke-CommandInDesktopPackage``` de PowerShell:

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>Pasos siguientes

**Encuentre respuestas a sus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Proporcionar comentarios o hacer sugerencias**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
