---
Description: Mejore su aplicación de escritorio para los usuarios de Windows 10 mediante el uso de las API de Universal Windows Platform (UWP).
Search.Product: eADQiWindows 10XVcnh
title: Mejorar tu aplicación de escritorio para Windows 10
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 55e91c96b7a978f0c90365073aa655553d4a658a
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63805694"
---
# <a name="enhance-your-desktop-application-for-windows-10"></a>Mejorar tu aplicación de escritorio para Windows 10

Puede usar Windows Runtime APIs para agregar experiencias modernas que se habilitan para los usuarios de Windows 10.

En primer lugar, configura tu proyecto. A continuación, agrega las experiencias de Windows 10. Puedes compilar por separado para los usuarios de Windows 10 o distribuir los mismos binarios exactos a todos los usuarios independientemente de qué versión de Windows ejecuten.

## <a name="first-set-up-your-project"></a>En primer lugar, configura tu proyecto.

Tendrás que realizar algunos cambios en tu proyecto para usar las API de UWP.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Modificar un proyecto de .NET para usar Windows Runtime APIs

Abre el cuadro de diálogo **Administrador de referencias**, elige el botón **Examinar** y, a continuación, selecciona **Todos los archivos**.

![Cuadro de diálogo Agregar referencia](images/desktop-to-uwp/browse-references.png)

A continuación, agrega una referencia a estos archivos.

|Archivo|Location|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|windows.winmd|C:\Program archivos (x86) \Windows Kits\10\UnionMetadata\\<*sdk versión*> \Facade|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program archivos (x86) \Windows Kits\10\References\\<*sdk versión*> \Windows.Foundation.UniversalApiContract\<*versión*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program archivos (x86) \Windows Kits\10\References\\<*sdk versión*> \Windows.Foundation.FoundationContract\<*versión*>|

En la ventana **Propiedades**, establece el campo **Copia local** de cada archivo *.winmd* en **False**.

![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-windows-runtime-apis"></a>Modificar un proyecto de C++ para usar Windows Runtime APIs

Use [C++ / c++ / WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) para usar Windows Runtime APIs. C++/WinRT es una completa proyección de lenguaje C++17 estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivo de encabezado y diseñada para darte acceso de primera clase a la moderna API de Windows.

Para configurar el proyecto de C++ / c++ / WinRT, consulte [modificar un proyecto de aplicación de escritorio de Windows para agregar C++ / c++ / WinRT soporte](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

## <a name="add-windows-10-experiences"></a>Agregar experiencias de Windows 10

Ya estás listo para agregar experiencias modernas que dan a conocer cuándo los usuarios ejecutan tu aplicación en Windows 10. Usa este flujo de diseño.

: white_check_mark: **En primer lugar, decida qué experiencias que desea agregar**

Hay muchas entre las que elegir. Por ejemplo, puede simplificar el flujo de pedidos de compra mediante la monetización de las API o atención directa a la aplicación cuando tenga algo interesante para compartir, como una imagen nueva que otro usuario ha publicado.

![Notificación del sistema](images/desktop-to-uwp/toast.png)

Incluso si los usuarios ignoran o descartar el mensaje, pueden verlo de nuevo en el centro de actividades y, a continuación, hacer clic en el mensaje para abrir tu aplicación. Esto aumenta el compromiso con la aplicación y tiene la ventaja adicional de que parezca que la aplicación está profundamente integrado con el sistema operativo. Te mostraremos el código para esa experiencia más adelante.

Visita nuestro [centro para desarrolladores](https://developer.microsoft.com/windows) para obtener ideas.

: white_check_mark: **Decida si desea mejorar o ampliar**

A menudo nos escucharás usar los términos "mejorar" y "ampliar", por lo que vamos a detenernos un momento para explicar exactamente qué significan cada uno de estos términos.

Utilizaremos el término "mejoran" al describir Windows en tiempo de ejecución APIs que se puede llamar directamente desde su aplicación de escritorio. Cuando hayas elegido una experiencia de Windows 10, identifica las API que necesitas para crearla y comprueba si esa API aparece en esta [lista](desktop-to-uwp-supported-api.md). Esta es una lista de API a las que puedes llamar directamente desde tu aplicación de escritorio. Si tu API no aparece en esta lista, se debe a que la funcionalidad asociada a esa API solo se puede ejecutar dentro de un proceso de UWP. A menudo, estos incluyen API que muestran interfaces de usuario modernas como un control de mapa de UWP o una advertencia de seguridad de Windows Hello.

Dicho esto, si quieres incluir estas experiencias en tu aplicación, solo tienes que "ampliar" la aplicación agregando un proyecto de UWP a la solución. El proyecto de escritorio sigue siendo el punto de entrada de tu aplicación, pero el proyecto de UWP le proporciona acceso a todas las API que no aparecen en esta [lista](desktop-to-uwp-supported-api.md). La aplicación de escritorio puede comunicarse con el proceso de UWP mediante un servicio de aplicaciones y tenemos muchos consejos para darte sobre cómo configurarlo. Si quieres agregar una experiencia que requiere un proyecto de UWP, consulta [Ampliar con UWP](desktop-to-uwp-extend.md).

: white_check_mark: **Contratos de API de referencia**

Si puedes llamar a la API directamente desde tu aplicación de escritorio, abre un navegador y busca el tema de referencia para esa API.
Debajo del resumen de la API, encontrarás una tabla en la que se describe el contrato de API para esa API. Este es un ejemplo de esa tabla:

![Tabla de contrato de API](images/desktop-to-uwp/contract-table.png)

Si tienes una aplicación de escritorio basada en .NET, agrega una referencia a ese contrato de API y luego establece la propiedad **Copia local** de ese archivo en **False**. Si tienes un proyecto basado en C++, agrega a **Directorios de inclusión adicionales**, una ruta de acceso a la carpeta que contiene este contrato.

: white_check_mark: **Llamar a las API para agregar su experiencia**

Este es el código que usarías para mostrar la ventana de notificación que hemos visto anteriormente. Estas API aparecen en esta [lista](desktop-to-uwp-supported-api.md) para que puedas agregar este código a la aplicación de escritorio y ejecutarlo en este momento.

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

## <a name="related-video"></a>Vídeo relacionado

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Use-UWP-APIs-in-Your-Code-3d78c6WhD_9506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

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
