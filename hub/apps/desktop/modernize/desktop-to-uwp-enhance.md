---
Description: Mejore su aplicación de escritorio para los usuarios de Windows 10 mediante el uso de las API de Universal Windows Platform (UWP).
title: Use las API de UWP en aplicaciones de escritorio
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 0545ea525b96d3a9310f3a761fd60a644f21baeb
ms.sourcegitcommit: b8087f8b6cf8367f8adb7d6db4581d9aa47b4861
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2019
ms.locfileid: "67414080"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>Llamar a las API de UWP en aplicaciones de escritorio

Puede usar las API de Universal Windows Platform (UWP) para agregar experiencias modernas para las aplicaciones de escritorio que se habilitan para los usuarios de Windows 10.

En primer lugar, configure el proyecto con las referencias necesarias. A continuación, llamar a las API de UWP desde el código para agregar que experiencias de Windows 10 a su aplicación de escritorio. Puede crear por separado para los usuarios de Windows 10 o distribuir los mismos binarios para todos los usuarios independientemente de qué versión de Windows se ejecutan.

Algunas API de UWP solo se admiten en aplicaciones de escritorio que se empaquetan en un [paquete MSIX](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root). Para obtener más información, consulte [disponibles las API de UWP](desktop-to-uwp-supported-api.md).

## <a name="set-up-your-project"></a>Configurar el proyecto

Tendrás que realizar algunos cambios en tu proyecto para usar las API de UWP.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Modificar un proyecto de .NET para usar Windows Runtime APIs

Hay dos opciones para proyectos de. NET:

* Si la aplicación tiene como destino Windows 10, versión 1803 o posterior, puede instalar un paquete de NuGet que proporciona todas las referencias necesarias.
* Como alternativa, puede agregar manualmente las referencias.

#### <a name="to-use-the-nuget-option"></a>Para usar la opción de NuGet

1. Asegúrese de que [referencias de paquete](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) están habilitadas:

    1. En Visual Studio, haga clic en **Herramientas -> Administrador de paquetes NuGet -> configuración del Administrador de paquetes**.
    2. Asegúrese de que **PackageReference** está seleccionada para **formato de administración de paquetes predeterminado**.

2. Con el proyecto abierto en Visual Studio, haga clic en el proyecto en **el Explorador de soluciones** y elija **administrar paquetes de NuGet**.

3. En el **Administrador de paquetes de NuGet** ventana, seleccione el **examinar** pestaña y busque `Microsoft.Windows.SDK.Contracts`.

4. Después de la `Microsoft.Windows.SDK.Contracts` paquete se encuentra en el panel derecho de la **Administrador de paquetes de NuGet** ventana Seleccione el **versión** del paquete que desea instalar en función de la versión de Windows 10 como destino:

    * **10.0.18362.xxxx-preview**: Elija esta opción para Windows 10, versión 1903.
    * **10.0.17763.xxxx-preview**: Elija esta opción para Windows 10, versión 1809.
    * **10.0.17134.xxxx-preview**: Elija esta opción para Windows 10, versión 1803.

5. Haga clic en **Instalar**.

#### <a name="to-add-the-required-references-manually"></a>Para agregar manualmente las referencias necesarias

1. Abre el cuadro de diálogo **Administrador de referencias**, elige el botón **Examinar** y, a continuación, selecciona **Todos los archivos**.

    ![Cuadro de diálogo Agregar referencia](images/desktop-to-uwp/browse-references.png)

2. Agregue una referencia a estos archivos.

    |Archivo|Location|
    |--|--|
    |System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |windows.winmd|C:\Program archivos (x86) \Windows Kits\10\UnionMetadata\\<*sdk versión*> \Facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Program archivos (x86) \Windows Kits\10\References\\<*sdk versión*> \Windows.Foundation.UniversalApiContract\<*versión*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Program archivos (x86) \Windows Kits\10\References\\<*sdk versión*> \Windows.Foundation.FoundationContract\<*versión*>|

3. En la ventana **Propiedades**, establece el campo **Copia local** de cada archivo *.winmd* en **False**.

    ![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-windows-runtime-apis"></a>Modificar un proyecto de C++ para usar Windows Runtime APIs

Use [C++ / c++ / WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) para usar Windows Runtime APIs. C++/WinRT es una completa proyección de lenguaje C++17 estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivo de encabezado y diseñada para darte acceso de primera clase a la moderna API de Windows.

Para configurar el proyecto de C++ / c++ / WinRT, consulte [modificar un proyecto de aplicación de escritorio de Windows para agregar C++ / c++ / WinRT soporte](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

## <a name="add-windows-10-experiences"></a>Agregar experiencias de Windows 10

Ya estás listo para agregar experiencias modernas que dan a conocer cuándo los usuarios ejecutan tu aplicación en Windows 10. Usa este flujo de diseño.

: white_check_mark: **En primer lugar, decida qué experiencias que desea agregar**

Hay muchas entre las que elegir. Por ejemplo, puede simplificar el flujo de pedidos de compra mediante [monetización API](/windows/uwp/monetize), o [dirigir la atención a la aplicación](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts) cuando tenga algo interesante para compartir, como una nueva imagen que otro usuario ha publicado.

![Notificación del sistema](images/desktop-to-uwp/toast.png)

Incluso si los usuarios ignoran o descartar el mensaje, pueden verlo de nuevo en el centro de actividades y, a continuación, hacer clic en el mensaje para abrir tu aplicación. Esto aumenta el compromiso con la aplicación y tiene la ventaja adicional de que parezca que la aplicación está profundamente integrado con el sistema operativo. Le mostraremos el código de esa experiencia un poco más adelante en este artículo.

Visite el [documentación de UWP](/windows/uwp/get-started/) para obtener más ideas.

: white_check_mark: **Decida si desea mejorar o ampliar**

Escuchamos con frecuencia nos utilizan los términos *mejorar* y *ampliar*, por lo que tomaremos un momento para explicar exactamente lo que cada uno de estos términos de Media.

Utilizaremos el término *mejorar* para describir Windows en tiempo de ejecución APIs que se puede llamar directamente desde su aplicación de escritorio (o no se ha elegido empaquetar la aplicación en un paquete MSIX). Cuando haya elegido una experiencia de Windows 10, identificar las API que deba crearlo y, a continuación, vea si aparece dicha API en [esta lista](desktop-to-uwp-supported-api.md). Se trata de una lista de las API que se pueden llamar directamente desde su aplicación de escritorio. Si tu API no aparece en esta lista, se debe a que la funcionalidad asociada a esa API solo se puede ejecutar dentro de un proceso de UWP. A menudo, estos incluyen las API que se procesan UWP XAML como un indicador de seguridad Windows Hello o un control de mapa UWP.

> [!NOTE]
> Aunque las API que se procesan normalmente UWP XAML no se puede llamar directamente desde el escritorio, puede usar enfoques alternativos. Si desea hospedar controles de UWP XAML u otras experiencias visuales personalizadas, puede usar [XAML Islas](xaml-islands.md) (a partir de Windows 10, versión 1903) y el [capa Visual](visual-layer-in-desktop-apps.md) (a partir de Windows 10, versión 1803). Estas características pueden utilizarse en aplicaciones de escritorio sin empaquetar o empaquetadas.

Si ha elegido empaquetar la aplicación de escritorio en un paquete MSIX, otra opción consiste en *ampliar* la aplicación mediante la adición de un proyecto UWP a la solución. El proyecto de escritorio sigue siendo el punto de entrada de la aplicación, pero el proyecto de UWP proporciona acceso a todas las API que no aparecen en [esta lista](desktop-to-uwp-supported-api.md). La aplicación de escritorio puede comunicarse con el proceso UWP mediante el uso de un app service y se tiene una gran cantidad de instrucciones sobre cómo configurarlo. Si desea agregar una experiencia que requiere un proyecto de UWP, consulte [extender con componentes UWP](desktop-to-uwp-extend.md).

: white_check_mark: **Contratos de API de referencia**

Si se puede llamar a la API directamente desde su aplicación de escritorio, abra un explorador y busque el tema de referencia de dicha API.
Debajo del resumen de la API, encontrarás una tabla en la que se describe el contrato de API para esa API. Este es un ejemplo de esa tabla:

![Tabla de contrato de API](images/desktop-to-uwp/contract-table.png)

Si tienes una aplicación de escritorio basada en .NET, agrega una referencia a ese contrato de API y luego establece la propiedad **Copia local** de ese archivo en **False**. Si tienes un proyecto basado en C++, agrega a **Directorios de inclusión adicionales**, una ruta de acceso a la carpeta que contiene este contrato.

: white_check_mark: **Llamar a las API para agregar su experiencia**

Este es el código que usarías para mostrar la ventana de notificación que hemos visto anteriormente. Estas API aparecen en este [lista](desktop-to-uwp-supported-api.md) para que pueda agregar este código a su aplicación de escritorio y ejecutarlo ahora mismo.

```csharp
using Windows.Foundation;
using Windows.System;
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
...

private void ShowToast()
{
    string title = "featured picture of the day";
    string content = "beautiful scenery";
    string image = "https://picsum.photos/360/180?image=104";
    string logo = "https://picsum.photos/64?image=883";

    string xmlString =
    $@"<toast><visual>
       <binding template='ToastGeneric'>
       <text>{title}</text>
       <text>{content}</text>
       <image src='{image}'/>
       <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
       </binding>
      </visual></toast>";

    XmlDocument toastXml = new XmlDocument();
    toastXml.LoadXml(xmlString);

    ToastNotification toast = new ToastNotification(toastXml);

    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

```C++
using namespace Windows::Foundation;
using namespace Windows::System;
using namespace Windows::UI::Notifications;
using namespace Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    Platform::String ^title = "featured picture of the day";
    Platform::String ^content = "beautiful scenery";
    Platform::String ^image = "https://picsum.photos/360/180?image=104";
    Platform::String ^logo = "https://picsum.photos/64?image=883";

    Platform::String ^xmlString =
        L"<toast><visual><binding template='ToastGeneric'>" +
        L"<text>" + title + "</text>" +
        L"<text>"+ content + "</text>" +
        L"<image src='" + image + "'/>" +
        L"<image src='" + logo + "'" +
        L" placement='appLogoOverride' hint-crop='circle'/>" +
        L"</binding></visual></toast>";

    XmlDocument ^toastXml = ref new XmlDocument();

    toastXml->LoadXml(xmlString);

    ToastNotificationManager::CreateToastNotifier()->Show(ref new ToastNotification(toastXml));
}
```

Para obtener más información acerca de notificaciones, consulta [Notificaciones del sistema interactivas y adaptables](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts).

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Compatibilidad con bases de instalación de Windows XP, Windows Vista y Windows 7/8

Puede modernizar la aplicación para Windows 10 sin tener que crear una nueva rama y mantener bases de código independiente.

Si quieres compilar archivos binarios independientes para los usuarios de Windows 10, usa la compilación condicional. Si prefieres compilar un conjunto de archivos binarios que implementas en todos los usuarios de Windows, usa las comprobaciones en tiempo de ejecución.

Echemos un vistazo rápido a cada opción.

### <a name="conditional-compilation"></a>Compilación condicional

Puedes mantener una base de código y compilar un conjunto de archivos binarios solo para los usuarios de Windows 10.

En primer lugar, agrega una nueva configuración de compilación al proyecto.

![Configuración de compilación](images/desktop-to-uwp/build-config.png)

Para esa configuración de compilación, crear una constante que se va a identificar el código que llama a Windows Runtime APIs.  

Para los proyectos basados en .NET, la constante se llama **constante de compilación condicional**.

![preprocesador](images/desktop-to-uwp/compilation-constants.png)

Para los proyectos basados en C++, la constante se llama **definición del preprocesador**.

![preprocesador](images/desktop-to-uwp/pre-processor.png)

Agrega esa constante antes de cualquier bloque de código UWP.

```csharp

[System.Diagnostics.Conditional("_UWP")]
private void ShowToast()
{
 ...
}

```

```C++

#if _UWP
void UWP::ShowToast()
{
 ...
}
#endif

```

El compilador compila ese código solo si esa constante se define en la configuración de compilación activa.

### <a name="runtime-checks"></a>Comprobaciones en tiempo de ejecución

Puedes compilar un conjunto de archivos binarios para todos tus usuarios de Windows independientemente de qué versión de Windows ejecuten. La aplicación llama a Windows Runtime APIs sólo si el usuario se ejecuta la aplicación como una aplicación empaquetada en Windows 10.

La manera más fácil agregar comprobaciones en tiempo de ejecución en el código consiste en instalar este paquete de Nuget: [Las aplicaciones auxiliares de Desktop Bridge](https://www.nuget.org/packages/DesktopBridge.Helpers/) y, a continuación, utilice el ``IsRunningAsUWP()`` método a la puerta de todo el código que llama a Windows Runtime APIs. Consulte este blog para obtener más detalles: [Desktop Bridge - identificar el contexto de la aplicación](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-samples"></a>Muestras relacionadas

* [Ejemplo Hello World](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Icono secundario](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Ejemplo de API de Store](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [Aplicación de WinForms que implementa un UpdateTask UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Puente de una aplicación de escritorio para ejemplos de UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="support-and-feedback"></a>Soporte técnico y comentarios

**Encuentre respuestas a sus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Proporcionar comentarios o hacer sugerencias**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
