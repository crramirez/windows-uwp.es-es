---
author: normesta
Description: "Mejorar tu aplicación de escritorio para los usuarios de Windows 10 mediante las API de la Plataforma universal de Windows."
Search.Product: eADQiWindows 10XVcnh
title: "Mejorar tu aplicación de escritorio para Windows 10"
ms.author: normesta
ms.date: 07/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 9f1522527825d6580ddf4fc36599352a9ca8a243
ms.sourcegitcommit: f93e887fbab6c1f824a8f762ba848f64c7f77c49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2017
---
# <a name="enhance-your-desktop-application-for-windows-10"></a>Mejorar tu aplicación de escritorio para Windows 10

Puedes usar las API de UWP para agregar modernas experiencias que destacan para los usuarios de Windows 10.

En primer lugar, configura tu proyecto. A continuación, agrega las experiencias de Windows 10. Puedes compilar por separado para los usuarios de Windows 10 o distribuir los mismos binarios exactos a todos los usuarios independientemente de qué versión de Windows ejecuten.

## <a name="first-set-up-your-project"></a>En primer lugar, configura tu proyecto.

Tendrás que realizar algunos cambios en tu proyecto para usar las API de UWP.

### <a name="modify-a-net-project-to-use-uwp-apis"></a>Modificar un proyecto de .NET para usar las API de UWP

Abre el cuadro de diálogo **Administrador de referencias**, elige el botón **Examinar** y, a continuación, selecciona **Todos los archivos**.

![Cuadro de diálogo Agregar referencia](images/desktop-to-uwp/browse-references.png)

A continuación, agrega una referencia a estos archivos.

|Archivo|Ubicación|
|--|--|
|System.Runtime.WindowsRuntime|C:\Archivos de programa (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|System.Runtime.WindowsRuntime.UI.Xaml|C:\Archivos de programa (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|System.Runtime.InteropServices.WindowsRuntime|C:\Archivos de programa (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|Windows.winmd|C:\Archivos de programa (x86)\Windows Kits\10\UnionMetadata\Facade|
|Windows.Foundation.UniversalApiContract.winmd|C:\Archivos de programa (x86)\Windows Kits\10\References\<*versión de sdk*>\Windows.Foundation.UniversalApiContract\<*versión*>|
|Windows.Foundation.FoundationContract.winmd|C:\Archivos de programa (x86)\Windows Kits\10\References\<*versión de sdk*>\Windows.Foundation.FoundationContract\<*versión*>|

En la ventana **Propiedades**, establezca el campo **Copia local** de los archivos **Windows.winmd** y **Windows.Foundation.FoundationContract.winmd** en **False**.

![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-uwp-apis"></a>Modificar un proyecto de C++ para usar las API de UWP

Abre las páginas de propiedades del proyecto.

En la configuración **General** del grupo de configuración **C/C++**, establece el campo **Usar extensión de Windows Runtime** en **Sí(/ZW)**.

   ![Usar extensión de Windows Runtime](images/desktop-to-uwp/enable-winrt-objects.png)

Abre el cuadro de diálogo **Directorios #using adicionales** y agrega estos directorios.

* C:\Archivos de programa (x86)\Microsoft Visual Studio 14.0\VC\vcpackages
* C:\Archivos de programa (x86)\Windows Kits\10\UnionMetadata
* C:\Archivos de programa (x86)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\<*versión más reciente*>
* C:\Archivos de programa (x86)\Windows Kits\10\References\Windows.Foundation.FoundationContract\<*versión más reciente*>

![Directorios using adicionales](images/desktop-to-uwp/additional-using.png)

Abre el cuadro de diálogo **Directorios de inclusión adicionales** y agrega este directorio: C:Archivos de programa (x86)\Windows Kits\10\Include\<*versión más reciente*>\um

![Directorios de inclusión adicionales](images/desktop-to-uwp/additional-include.png)

En la configuración de **Generación de código** del grupo de configuración **C/C++**, establece la configuración **Habilitar recompilación mínima** en **No(/GM-)**.

![Habilitar recompilación mínima](images/desktop-to-uwp/disable-min-build.png)


## <a name="add-windows-10-experiences"></a>Agregar experiencias de Windows 10

Ya estás listo para agregar experiencias modernas que dan a conocer cuándo los usuarios ejecutan tu aplicación en Windows 10. Usa este flujo de diseño.

:white_check_mark: **En primer lugar, decide qué experiencias quieres agregar**

Hay muchas entre las que elegir. Por ejemplo, puedes simplificar el flujo de pedidos de compra mediante API de rentabilidad o dirigir la atención a tu aplicación cuando tengas algo interesante que compartir, por ejemplo, una nueva imagen que otro usuario ha publicado.

![Notificación del sistema](images/desktop-to-uwp/toast.png)

Incluso si los usuarios ignoran o descartar el mensaje, pueden verlo de nuevo en el centro de actividades y, a continuación, hacer clic en el mensaje para abrir tu aplicación. Esto aumenta la relación con tu aplicación y tiene la ventaja adicional de que la aplicación parezca estrechamente integrada con el sistema operativo. Te mostraremos el código para esa experiencia más adelante.

Visita nuestro [centro para desarrolladores](https://developer.microsoft.com/windows) para obtener ideas.

:white_check_mark: **Decidir si quieres mejorar o ampliar**

A menudo nos escucharás usar los términos "mejorar" y "ampliar", por lo que vamos a detenernos un momento para explicar exactamente qué significan cada uno de estos términos.

Usamos el término "mejorar" para describir las API de UWP a las que puedes llamar directamente desde tu aplicación de escritorio. Cuando hayas elegido una experiencia de Windows 10, identifica las API que necesitas para crearla y comprueba si esa API aparece en esta [lista](desktop-to-uwp-supported-api.md). Esta es una lista de API a las que puedes llamar directamente desde tu aplicación de escritorio. Si tu API no aparece en esta lista, se debe a que la funcionalidad asociada a esa API solo se puede ejecutar dentro de un proceso de UWP. A menudo, estos incluyen API que muestran interfaces de usuario modernas como un control de mapa de UWP o una advertencia de seguridad de Windows Hello.

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
    string image = "https://unsplash.it/360/180?image=104";
    string logo = "https://unsplash.it/64?image=883";

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
    Platform::String ^image = "https://unsplash.it/360/180?image=104";
    Platform::String ^logo = "https://unsplash.it/64?image=883";

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
Para obtener más información acerca de notificaciones, consulta [Notificaciones del sistema interactivas y adaptables](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts).

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Compatibilidad con bases de instalación de Windows XP, Windows Vista y Windows 7/8

Puedes modernizar tu aplicación para Windows 10 sin tener que crear una nueva rama y mantener las bases de código independientes.

Si quieres compilar archivos binarios independientes para los usuarios de Windows 10, usa la compilación condicional. Si prefieres compilar un conjunto de archivos binarios que implementas en todos los usuarios de Windows, usa las comprobaciones en tiempo de ejecución.

Echemos un vistazo rápido a cada opción.

### <a name="conditional-compilation"></a>Compilación condicional

Puedes mantener una base de código y compilar un conjunto de archivos binarios solo para los usuarios de Windows 10.

En primer lugar, agrega una nueva configuración de compilación al proyecto.

![Configuración de compilación](images/desktop-to-uwp/build-config.png)

Para esa configuración de compilación, crea una constante para identificar el código que llama a las API de UWP.  

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

Puedes compilar un conjunto de archivos binarios para todos tus usuarios de Windows independientemente de qué versión de Windows ejecuten. Tu aplicación llama a las API de UWP solo si el usuario ejecuta tu aplicación como una aplicación empaquetada en Windows 10.

La manera más sencilla de agregar comprobaciones en tiempo de ejecución en el código es instalar este paquete de Nuget: [Aplicaciones auxiliares de Puente de dispositivo de escritorio](https://www.nuget.org/packages/DesktopBridge.Helpers/) y usar luego el método ``IsRunningAsUWP()`` para desactivar todo el código UWP. Consulta esta entrada de blog para obtener más información: [Puente de dispositivo de escritorio: identificar el contexto de la aplicación](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-video"></a>Vídeo relacionado

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Use-UWP-APIs-in-Your-Code-3d78c6WhD_9506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="related-samples"></a>Muestras relacionadas

* [Muestra de ¡Hello World!](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Icono secundario](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Muestra de la API de la Tienda](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [Aplicación de WinForms que implementa una UpdateTask de UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Ejemplos de puente de aplicación de escritorio a UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


## <a name="support-and-feedback"></a>Soporte técnico y comentarios

**Encuentra respuestas a preguntas específicas**

Nuestro equipo supervisa estas [etiquetas de StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Enviar comentarios o realizar sugerencias acerca de las características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)

**Envíanos tus comentarios acerca de este artículo**

Usa la sección comentarios que tienes a continuación.