---
author: awkoren
Description: "Empieza a usar el puente de escritorio a UWP y convierte tu aplicación de escritorio de Windows (por ejemplo, Win32, WPF y Windows Forms) en una aplicación para Plataforma universal de Windows (UWP)."
Search.Product: eADQiWindows 10XVcnh
title: "Convertir la aplicación de escritorio en una aplicación para Plataforma universal de Windows (UWP) con el puente de escritorio"
translationtype: Human Translation
ms.sourcegitcommit: 933dcd48c03de1992bbfcf0d86951170264a309f
ms.openlocfilehash: 170ed75d1cd865bfc8e4feb776fce4332cc67850

---

# Convertir la aplicación de escritorio en una aplicación para Plataforma universal de Windows (UWP) con el puente de escritorio

Empieza a usar el puente de escritorio a UWP y convierte tu aplicación de escritorio de Windows en una aplicación para Plataforma universal de Windows (UWP).

El puente de escritorio es un conjunto de tecnologías que te permite convertir tu aplicación de escritorio de Windows (por ejemplo, Win32, Windows Forms o WPF) o juego en un juego o una aplicación para UWP. Después de la conversión, la aplicación de escritorio de Windows se empaqueta, se le realiza un mantenimiento y se implementa en forma de un paquete de la aplicación para UWP (un archivo .appx o .appxbundle) destinado a escritorio de Windows10.

Hay dos elementos de la tecnología que permiten que las aplicaciones de escritorio se conviertan en paquetes para UWP. El primero es el proceso de conversión, que toma los archivos binarios existentes y vuelve a empaquetarlos como un paquete para UWP. Tu código sigue siendo el mismo, solo se empaqueta de forma diferente. La segunda parte abarca tecnologías en tiempo de ejecución en la actualización de Windows Anniversary que permiten que un paquete para UWP tenga archivos ejecutables que se ejecuten como de plena confianza en lugar de en un contenedor de aplicación. Esta tecnología también ofrece una identidad del paquete a una aplicación convertida, algo necesario para usar algunas API de UWP.

## Ventajas

A continuación se detallan algunas de las ventajas de convertir la aplicación de escritorio de Windows: 

**Implementación optimizada**. Las aplicaciones y los juegos que usan el puente presentan una excelente experiencia de implementación que garantiza que los usuarios puedan instalar y actualizar con confianza. Si un usuario decide desinstalar la aplicación, se quita completamente sin dejar rastros. Esto reduce el tiempo necesario para crear experiencias de instalación y mantener a los usuarios actualizados.

**Actualizaciones automáticas y licencias**. La aplicación puede participar en los servicios de licencia integrada y actualización automática de la Tienda Windows. La actualización automática es un mecanismo muy confiable y eficaz, ya que solo se descargan las partes modificadas de los archivos.

**Mayor alcance y monetización simplificada**. La elección de la distribución a través de la Tienda Windows te permite llegar a millones de usuarios de Windows 10, que pueden adquirir aplicaciones, juegos y compras desde la aplicación con las opciones de pago local.

**Incorporación de características de UWP**.  Puedes agregar características de UWP al paquete de la aplicación a tu propio ritmo, como una interfaz de usuario XAML, actualizaciones de iconos dinámicos, tareas en segundo plano para UWP, servicios de aplicaciones y mucho más.

**Casos de uso ampliado en dispositivos**. Con el puente, puedes migrar gradualmente tu código a la Plataforma universal de Windows para llegar a todos los dispositivos Windows 10, incluidos los teléfonos, Xbox One y HoloLens.

## Preparar

El puente de escritorio a UWP está diseñado para facilitar su uso y es posible que no tengas que hacer mucho para preparar la aplicación para el proceso de conversión. Sin embargo, hay algunas advertencias y situaciones específicas que se deben tener en cuenta antes de la conversión. Consulta el artículo [Preparar la aplicación para el puente de escritorio a UWP](desktop-to-uwp-prepare.md) y soluciona cualquier problema que corresponda a tu aplicación antes de continuar.

## Convertir

Hay varias opciones diferentes para convertir la aplicación.

**Desktop App Converter (DAC)**. DAC es una herramienta que convierte automáticamente y firma la aplicación por ti. El uso de DAC es práctico y automático, y resulta útil si la aplicación realiza un gran número de modificaciones en el sistema o si hay alguna duda sobre lo que hace el instalador. Para empezar a usarlo, consulta el artículo sobre [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md). 

**Conversión manual**. Si la aplicación se instala mediante xcopy, o estás familiarizado con los cambios que realiza el instalador en el sistema, la conversión manual podría ser una elección más sencilla. Esto implica crear un archivo de manifiesto, ejecutar la herramienta MakeAppx.exe y luego firmar el paquete de la aplicación. Para obtener más información sobre cómo convertir manualmente, consulta [Convertir manualmente la aplicación a UWP mediante el puente de escritorio](desktop-to-uwp-manual-conversion.md). 

**Instalador de terceros**. Actualmente, hay tres instaladores populares de terceros, [InstallShield de Flexera](http://www.flexerasoftware.com/producer/products/software-installation/installshield-software-installer), [WiX de FireGiant](https://www.firegiant.com/r/appx) y [Advanced Installer de Caphyon](http://www.advancedinstaller.com/uwp-app-package), que admiten el puente de escritorio y pueden generar un instalador MSI y un paquete de la aplicación convertida con tan solo unos clics. Para obtener más información, visita el sitio web correspondiente de cada instalador. 

## Mejorar 

Puedes destacar tu aplicación de escritorio convertida con una amplia variedad de API de UWP para agregar características como iconos dinámicos, notificaciones de inserción y mucho más. Para ver ejemplos de código completos, consulta los repositorios de [muestras de puente de aplicación de escritorio a UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) y [muestras de aplicaciones para Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples) de GitHub. Para ver una lista completa de las API admitidas, revisa [API de UWP admitidas para aplicaciones convertidas con el puente de escritorio](desktop-to-uwp-supported-api.md). 

Además de llamar a las API de UWP, puedes ampliar la aplicación con características accesibles solo para aplicaciones convertidas. Estas características incluyen escenarios como el inicio de un proceso cuando el usuario inicia sesión y la integración del Explorador de archivos y se han diseñado para facilitar la transición entre la aplicación de escritorio original y un paquete de la aplicación de UWP completo. Para obtener información detallada, consulta [Extensiones para aplicaciones de puente de escritorio](desktop-to-uwp-extensions.md). 

## Migrar

Con el puente, puedes migrar gradualmente el código anterior a UWP a la vez que conservas la capacidad de ejecutar y publicar la aplicación en el escritorio de Windows. Una vez que hayas migrado totalmente a UWP (y que la aplicación ya no tenga ningún componente de WPF o Win32), podrás llegar a todos los dispositivos Windows, incluidos los teléfonos, Xbox One y HoloLens.

## Depurar

Puedes depurar la aplicación con Visual Studio. Consulta [Depurar aplicaciones convertidas con el puente de escritorio](desktop-to-uwp-debug.md) para obtener ayuda detallada. 

Si estás interesado en los aspectos internos de cómo funciona el puente de escritorio en segundo plano, echa un vistazo a [Segundo plano del puente de escritorio](desktop-to-uwp-behind-the-scenes.md). 

## Distribuir

Puedes distribuir la aplicación con la Tienda Windows o a través de la instalación de prueba. Para obtener detalles completos, consulta [Distribuir aplicaciones convertidas con el puente de escritorio](desktop-to-uwp-distribute.md). Ten en cuenta que tendrás que firmar la aplicación antes de implementarla en los usuarios. Consulta [Firmar una aplicación convertida con el puente de escritorio](desktop-to-uwp-signing.md) para obtener instrucciones paso a paso. 

## Soporte técnico y comentarios

Si surgen problemas al convertir tu aplicación, puedes visitar los [foros](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop) para obtener ayuda. 

Para enviar comentarios o realizar sugerencias, envía elementos o vota a favor en [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial). 

## En esta sección

| Tema | Descripción |
|-------|-------------|
| [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) | Muestra cómo ejecutar Desktop App Converter. |
| [Convertir manualmente la aplicación a UWP mediante el puente de escritorio](desktop-to-uwp-manual-conversion.md) | Aprende a crear un paquete de la aplicación y un manifiesto manualmente. |
| [Extensiones para aplicaciones de puente de escritorio](desktop-to-uwp-extensions.md) | Mejora la aplicación convertida con extensiones para habilitar características como las tareas de inicio y la integración del Explorador de archivos. |
| [API de UWP admitidas para aplicaciones convertidas con el puente de escritorio](desktop-to-uwp-supported-api.md) | Consulta qué API de UWP están disponibles para su uso en tu aplicación de escritorio convertida. |
| [Depurar aplicaciones convertidas con el puente de escritorio](desktop-to-uwp-debug.md) | Explica las opciones para depurar la aplicación convertida. | 
| [Firmar una aplicación convertida con el puente de escritorio](desktop-to-uwp-signing.md) | Aprende a firmar el paquete de la aplicación convertida con un certificado. |
| [Distribuir aplicaciones convertidas con el puente de escritorio](desktop-to-uwp-distribute.md) | Consulta cómo distribuir la aplicación convertida a los usuarios.  |
| [Segundo plano del puente de escritorio](desktop-to-uwp-behind-the-scenes.md) | Realiza un análisis más profundo sobre cómo funciona el puente de escritorio a UWP en modo no visible. | 
| [Problemas conocidos de puente de escritorio](desktop-to-uwp-known-issues.md) | Enumera los problemas conocidos del puente de escritorio a UWP. | 
| [Ejemplos de código de puente de aplicación de escritorio a UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | Ejemplos de código en GitHub que muestran las características de las aplicaciones convertidas. |


<!--HONumber=Nov16_HO1-->


