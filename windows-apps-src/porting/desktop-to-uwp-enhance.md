---
Description: Enhance your desktop application for Windows 10 users by using Universal Windows Platform (UWP) APIs.
Search.Product: eADQiWindows 10XVcnh
title: Mejorar tu aplicación de escritorio para Windows 10
ms.date: 10/15/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 42229212a0f54e307eaa841849c1a279c4354d2a
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "8191877"
---
# <a name="enhance-your-desktop-application-for-windows-10"></a>Mejorar tu aplicación de escritorio para Windows 10

Puedes usar Windows Runtime APIs para agregar experiencias modernas que destacan para los usuarios de Windows 10.

En primer lugar, configura tu proyecto. A continuación, agrega las experiencias de Windows 10. Puedes compilar por separado para los usuarios de Windows 10 o distribuir los mismos binarios exactos a todos los usuarios independientemente de qué versión de Windows ejecuten.

## <a name="first-set-up-your-project"></a>En primer lugar, configura tu proyecto.

Tendrás que realizar algunos cambios en tu proyecto para usar las API de UWP.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Modificar un proyecto de .NET para usar Windows Runtime APIs

Abre el cuadro de diálogo **Administrador de referencias**, elige el botón **Examinar** y, a continuación, selecciona **Todos los archivos**.

![Cuadro de diálogo Agregar referencia](images/desktop-to-uwp/browse-references.png)

A continuación, agrega una referencia a estos archivos.

|Archivo|Ubicación|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Archivos de programa (x86)\Windows Kits\10\References\<*versión de sdk*>\Windows.Foundation.UniversalApiContract\<*versión*>|
|Windows.Foundation.FoundationContract.winmd|C:\Archivos de programa (x86)\Windows Kits\10\References\<*versión de sdk*>\Windows.Foundation.FoundationContract\<*versión*>|

En la ventana **Propiedades**, establece el campo **Copia local** de cada archivo *.winmd* en **False**.

![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-windows-runtime-apis"></a>Modificar un proyecto de C++ para usar Windows Runtime APIs

Uso [C++ / WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) consumir Windows Runtime APIs. C++/WinRT es una completa proyección de lenguaje C++17 estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivo de encabezado y diseñada para darte acceso de primera clase a la moderna API de Windows.

Configurar el proyecto para C++ / WinRT, consulta [modificar un proyecto de aplicación de escritorio de Windows para agregar C++ / WinRT soporte](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

## <a name="add-windows-10-experiences"></a>Agregar experiencias de Windows 10

Ya estás listo para agregar experiencias modernas que dan a conocer cuándo los usuarios ejecutan tu aplicación en Windows 10. Usa este flujo de diseño.

:white_check_mark: **En primer lugar, decide qué experiencias quieres agregar**

Hay muchas entre las que elegir. Por ejemplo, puedes simplificar el flujo de pedidos de compra mediante API de rentabilidad o dirigir la atención a la aplicación cuando tengas algo interesante que compartir, por ejemplo, una nueva imagen que otro usuario ha publicado.

![Notificación del sistema](images/desktop-to-uwp/toast.png)

Incluso si los usuarios ignoran o descartar el mensaje, pueden verlo de nuevo en el centro de actividades y, a continuación, hacer clic en el mensaje para abrir tu aplicación. Esto aumenta la relación con la aplicación y tiene la ventaja adicional de que la aplicación parezca estrechamente integrada con el sistema operativo. Te mostraremos el código para esa experiencia más adelante.

Visita nuestro [centro para desarrolladores](https://developer.microsoft.com/windows) para obtener ideas.

:white_check_mark: **Decidir si quieres mejorar o ampliar**

A menudo nos escucharás usar los términos "mejorar" y "ampliar", por lo que vamos a detenernos un momento para explicar exactamente qué significan cada uno de estos términos.

Usamos el término "mejorar" para describir Windows en tiempo de ejecución APIs que puedes llamar directamente desde la aplicación de escritorio. Cuando hayas elegido una experiencia de Windows 10, identifica las API que necesitas para crearla y comprueba si esa API aparece en esta [lista](desktop-to-uwp-supported-api.md). Esta es una lista de API a las que puedes llamar directamente desde tu aplicación de escritorio. Si tu API no aparece en esta lista, se debe a que la funcionalidad asociada a esa API solo se puede ejecutar dentro de un proceso de UWP. A menudo, estos incluyen API que muestran interfaces de usuario modernas como un control de mapa de UWP o una advertencia de seguridad de Windows Hello.

Dicho esto, si quieres incluir estas experiencias en tu aplicación, solo tienes que "ampliar" la aplicación agregando un proyecto de UWP a la solución. El proyecto de escritorio sigue siendo el punto de entrada de tu aplicación, pero el proyecto de UWP le proporciona acceso a todas las API que no aparecen en esta [lista](desktop-to-uwp-supported-api.md). La aplicación de escritorio puede comunicarse con el proceso de UWP mediante un servicio de aplicaciones y tenemos muchos consejos para darte sobre cómo configurarlo. Si quieres agregar una experiencia que requiere un proyecto de UWP, consulta [Ampliar con UWP](desktop-to-uwp-extend.md).

:white_check_mark: **Contratos de API de referencia**

Si puedes llamar a la API directamente desde tu aplicación de escritorio, abre un navegador y busca el tema de referencia para esa API.
Debajo del resumen de la API, encontrarás una tabla en la que se describe el contrato de API para esa API. Este es un ejemplo de esa tabla:

![Tabla de contrato de API](images/desktop-to-uwp/contract-table.png)

Si tienes una aplicación de escritorio basada en .NET, agrega una referencia a ese contrato de API y luego establece la propiedad **Copia local** de ese archivo en **False**. Si tienes un proyecto basado en C++, agrega a **Directorios de inclusión adicionales**, una ruta de acceso a la carpeta que contiene este contrato.

:white_check_mark: **Llama a las API para agregar tu experiencia**

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

Puedes modernizar tu aplicación para Windows 10 sin tener que crear una nueva rama y mantener las bases de código independiente.

Si quieres compilar archivos binarios independientes para los usuarios de Windows 10, usa la compilación condicional. Si prefieres compilar un conjunto de archivos binarios que implementas en todos los usuarios de Windows, usa las comprobaciones en tiempo de ejecución.

Echemos un vistazo rápido a cada opción.

### <a name="conditional-compilation"></a>Compilación condicional

Puedes mantener una base de código y compilar un conjunto de archivos binarios solo para los usuarios de Windows 10.

En primer lugar, agrega una nueva configuración de compilación al proyecto.

![Configuración de compilación](images/desktop-to-uwp/build-config.png)

Para esa configuración de compilación, crea una constante para identificar el código que llama Windows Runtime APIs.  

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

Puedes compilar un conjunto de archivos binarios para todos tus usuarios de Windows independientemente de qué versión de Windows ejecuten. La aplicación llama Windows Runtime APIs solo si el usuario se ejecuta como una aplicación empaquetada en Windows 10.

La forma más sencilla de agregar comprobaciones en tiempo de ejecución en el código es instalar este paquete de Nuget: [Aplicaciones auxiliares de puente de dispositivo de escritorio](https://www.nuget.org/packages/DesktopBridge.Helpers/) y, a continuación, usa el ``IsRunningAsUWP()`` método para desactivar todo el código que llama Windows Runtime APIs. Consulta esta entrada de blog para obtener más información: [Puente de dispositivo de escritorio: identificar el contexto de la aplicación](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-video"></a>Vídeo relacionado

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Use-UWP-APIs-in-Your-Code-3d78c6WhD_9506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="related-samples"></a>Muestras relacionadas

* [Muestra de ¡Hello World!](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Icono secundario](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Muestra de la API de la Store](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [Aplicación de WinForms que implementa una UpdateTask de UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Ejemplos de puente de aplicación de escritorio a UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


## <a name="support-and-feedback"></a>Soporte técnico y comentarios

**Encuentra respuestas a tus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Enviar comentarios o realizar sugerencias acerca de las características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
