---
description: Mejora tu aplicación de escritorio para los usuarios de Windows 10 mediante las API de Windows Runtime.
title: Llamada a las a API de Windows Runtime en aplicaciones de escritorio
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: e58315ed70b889e1369e8c13a563f320c0ca1948
ms.sourcegitcommit: a222ad0e2d97e35a60000c473808c678395376ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2020
ms.locfileid: "89479085"
---
# <a name="call-windows-runtime-apis-in-desktop-apps"></a>Llamada a las a API de Windows Runtime en aplicaciones de escritorio

Puedes usar las API de la Plataforma universal de Windows (UWP) para agregar experiencias modernas a tus aplicaciones de escritorio que agraden a los usuarios de Windows 10.

En primer lugar, configura el proyecto con las referencias necesarias. A continuación, llama a las API de Windows Runtime desde el código para agregar experiencias de Windows 10 a la aplicación de escritorio. Puedes compilar por separado para los usuarios de Windows 10 o distribuir los mismos binarios a todos los usuarios independientemente de qué versión de Windows ejecuten.

Algunas API de Windows Runtime solo se admiten en aplicaciones de escritorio que tienen [identidad de paquete](modernize-packaged-apps.md). Para obtener más información, consulta [API de Windows Runtime disponibles](desktop-to-uwp-supported-api.md).

## <a name="set-up-your-project"></a>Configuración del proyecto

Tendrás que realizar algunos cambios en tu proyecto para usar las API de Windows Runtime.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Modificación de un proyecto de .NET para usar las API de Windows Runtime

Existen dos opciones para los proyectos de .NET:

* Si tu aplicación está dirigida a Windows 10, versión 1803 o posterior, puedes instalar un paquete de NuGet que proporcione todas las referencias necesarias.
* Como alternativa, puedes agregar las referencias manualmente.

#### <a name="to-use-the-nuget-option"></a>Uso de la opción NuGet

1. Asegúrate de que las [referencias de paquete](/nuget/consume-packages/package-references-in-project-files) están habilitadas:

    1. En Visual Studio, haz clic en **Herramientas-> Administrador de paquetes NuGet-> Configuración del Administrador de paquetes**.
    2. Asegúrate de que **PackageReference** está seleccionado para **Formato predeterminado de administración de paquetes**.

2. Una vez hayas creado tu proyecto en Visual Studio, haz clic con el botón derecho en el **Explorador de soluciones** y elige **Administrar paquetes NuGet**.

3. En la ventana **Administrador de paquetes NuGet**, selecciona la pestaña **Examinar** y busca `Microsoft.Windows.SDK.Contracts`.

4. Una vez encontrado el paquete `Microsoft.Windows.SDK.Contracts`, en el panel derecho de la ventana **Administrador de paquetes NuGet**, selecciona la **Versión** del paquete que quieres instalar en función de la versión de Windows 10 de destino deseada:

    * **10.0.18362.xxxx**: elije esta versión para Windows 10, versión 1903.
    * **10.0.17763.xxxx**: elije esta versión para Windows 10, versión 1809.
    * **10.0.17134.xxxx**: elije esta versión para Windows 10, versión 1803.

5. Haga clic en **Instalar**.

#### <a name="to-add-the-required-references-manually"></a>Adición manual de las referencias necesarias

1. Abre el cuadro de diálogo **Administrador de referencias**, elige el botón **Examinar** y, a continuación, selecciona **Todos los archivos**.

    ![Cuadro de diálogo Agregar referencia](images/desktop-to-uwp/browse-references.png)

2. Agregue una referencia a todos los archivos siguientes.

    |Archivo|Ubicación|
    |--|--|
    |System.Runtime.WindowsRuntime.dll|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.WindowsRuntime.UI.Xaml.dll|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.InteropServices.WindowsRuntime.dll|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |windows.winmd|C:\Archivos de programa (x86)\Windows Kits\10\UnionMetadata\\<*versión de SDK*>\Facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Archivos de programa (x86)\Windows Kits\10\References\\<*versión de SDK*>\Windows.Foundation.UniversalApiContract\\<*versión*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Archivos de programa (x86)\Windows Kits\10\References\\<*versión de SDK*>\Windows.Foundation.FoundationContract\\<*versión*>|

3. En la ventana **Propiedades**, establece el campo **Copia local** de cada archivo *.winmd* en **False**.

    ![Campo Copia local](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>Modificación de un proyecto de C++ de Win32 para usar las API de Windows Runtime

Use [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) para utilizar las API de Windows Runtime. C++/WinRT es una moderna proyección de lenguaje C++17 totalmente estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivos de encabezado y diseñada para darte acceso de primera clase a la API moderna de Windows.

Para configurar el proyecto para C++/WinRT:

* En el caso de los proyectos nuevos, puedes instalar la [extensión de Visual Studio para C++/WinRT (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) y usar una de las plantillas de proyecto de C++/WinRT incluidas en esa extensión.
* Para los proyectos existentes, instala el paquete NuGet [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) en el proyecto.

Para obtener más información sobre estas opciones, consulta [este artículo](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-windows-10-experiences"></a>Adición de experiencias de Windows 10

Ya estás listo para agregar experiencias modernas que dan a conocer cuándo los usuarios ejecutan tu aplicación en Windows 10. Usa este flujo de diseño.

: white_check_mark: **En primer lugar, decide qué experiencias quieres agregar**

Hay muchas entre las que elegir. Por ejemplo, puedes simplificar el flujo de pedidos de compra mediante las [API de rentabilidad](/windows/uwp/monetize) o [dirigir la atención a tu aplicación](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts) cuando tengas algo interesante que compartir, por ejemplo, una nueva imagen que otro usuario ha publicado.

![Notificación del sistema](images/desktop-to-uwp/toast.png)

Aunque los usuarios ignoren o descarten el mensaje, pueden verlo de nuevo en el centro de actividades y, a continuación, hacer clic en el mensaje para abrir tu aplicación. Esto aumenta la relación con tu aplicación y tiene la ventaja adicional de que la aplicación parezca perfectamente integrada con el sistema operativo. Te mostraremos el código para esa experiencia más adelante en este artículo.

Consulta la [documentación de UWP](/windows/uwp/get-started/) para obtener más ideas.

: white_check_mark: **Decide si prefieres mejorar o ampliar**

A menudo nos escucharás usar los términos *mejorar* y *ampliar*, por lo que vamos a detenernos un momento para explicar exactamente qué significan cada uno.

Usamos el término *mejorar* para describir las API de Windows Runtime a las que puedes llamar directamente desde la aplicación de escritorio (independientemente de si has elegido empaquetar la aplicación en un paquete MSIX). Cuando hayas elegido una experiencia de Windows 10, identifica las API que necesitas para crearla y comprueba si esa API aparece en [esta lista](desktop-to-uwp-supported-api.md). Esta es una lista de las API a las que puedes llamar directamente desde tu aplicación de escritorio. Si tu API no aparece en esta lista, se debe a que la funcionalidad asociada a esa API solo se puede ejecutar dentro de un proceso de UWP. A menudo, incluyen API que representan XAML de UWP, como un control de mapa de UWP o una advertencia de seguridad de Windows Hello.

> [!NOTE]
> Aunque normalmente no se puede llamar a las API que representan XAML de UWP directamente desde el escritorio, es posible que puedas usar enfoques alternativos. Si quieres hospedar controles XAML de UWP u otras experiencias visuales personalizadas, puedes usar [islas XAML](xaml-islands.md) (a partir de Windows 10, versión 1903) y la [capa visual](visual-layer-in-desktop-apps.md) (a partir de Windows 10, versión 1803). Estas características se pueden usar en aplicaciones de escritorio empaquetadas o sin empaquetar.

Si has optado por empaquetar la aplicación de escritorio en un paquete MSIX, otra opción es *ampliar* la aplicación agregando un proyecto de UWP a la solución. El proyecto de escritorio sigue siendo el punto de entrada de tu aplicación, pero el proyecto de UWP te proporciona acceso a todas las API que no aparecen en [esta lista](desktop-to-uwp-supported-api.md). La aplicación de escritorio puede comunicarse con el proceso de UWP mediante un servicio de aplicaciones. Tenemos muchos consejos para darte sobre cómo configurarlo. Si quieres agregar una experiencia que requiere un proyecto de UWP, consulta [Ampliación con componentes de UWP](desktop-to-uwp-extend.md).

: white_check_mark: **Consulta los contratos de API**

Si puedes llamar a la API directamente desde tu aplicación de escritorio, abre un navegador y busca el tema de referencia para esa API.
Debajo del resumen de la API, encontrarás una tabla en la que se describe el contrato de API para esa API. Este es un ejemplo de esa tabla:

![Tabla de contrato de API](images/desktop-to-uwp/contract-table.png)

Si tienes una aplicación de escritorio basada en .NET, agrega una referencia a ese contrato de API y luego establece la propiedad **Copia local** de ese archivo en **False**. Si tienes un proyecto basado en C++, agrega a **Directorios de inclusión adicionales** una ruta de acceso a la carpeta que contiene este contrato.

: white_check_mark: **Llama a las API para agregar tu experiencia**

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

```cppwinrt
#include <sstream>
#include <winrt/Windows.Data.Xml.Dom.h>
#include <winrt/Windows.UI.Notifications.h>

using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::System;
using namespace winrt::Windows::UI::Notifications;
using namespace winrt::Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    std::wstring const title = L"featured picture of the day";
    std::wstring const content = L"beautiful scenery";
    std::wstring const image = L"https://picsum.photos/360/180?image=104";
    std::wstring const logo = L"https://picsum.photos/64?image=883";

    std::wostringstream xmlString;
    xmlString << L"<toast><visual><binding template='ToastGeneric'>" <<
        L"<text>" << title << L"</text>" <<
        L"<text>" << content << L"</text>" <<
        L"<image src='" << image << L"'/>" <<
        L"<image src='" << logo << L"'" <<
        L" placement='appLogoOverride' hint-crop='circle'/>" <<
        L"</binding></visual></toast>";

    XmlDocument toastXml;

    toastXml.LoadXml(xmlString.str().c_str());

    ToastNotificationManager::CreateToastNotifier().Show(ToastNotification(toastXml));
}
```

```cppcx
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

Para obtener más información acerca de notificaciones, consulta [Notificaciones del sistema interactivas y adaptables](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts).

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Compatibilidad con bases de instalación de Windows XP, Windows Vista y Windows 7/8

Puedes modernizar tu aplicación para Windows 10 sin tener que crear una nueva rama ni mantener bases de código independientes.

Si quieres compilar archivos binarios independientes para los usuarios de Windows 10, usa la compilación condicional. Si prefieres compilar un conjunto de archivos binarios que implementas en todos los usuarios de Windows, usa las comprobaciones en tiempo de ejecución.

Echemos un vistazo rápido a cada opción.

### <a name="conditional-compilation"></a>Compilación condicional

Puedes mantener una base de código y compilar un conjunto de archivos binarios solo para los usuarios de Windows 10.

En primer lugar, agrega una nueva configuración de compilación al proyecto.

![Configuración de la compilación](images/desktop-to-uwp/build-config.png)

Para esa configuración de compilación, crea una constante para identificar el código que llama a las API de Windows Runtime.  

Para los proyectos basados en .NET, la constante se llama **constante de compilación condicional**.

![Contante de compilación condicional](images/desktop-to-uwp/compilation-constants.png)

Para los proyectos basados en C++, la constante se llama **definición del preprocesador**.

![Constante de definición del preprocesador](images/desktop-to-uwp/pre-processor.png)

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

El compilador compila ese código solo si esa constante está definida en la configuración de compilación activa.

### <a name="runtime-checks"></a>Comprobaciones en tiempo de ejecución

Puedes compilar un conjunto de archivos binarios para todos tus usuarios de Windows independientemente de qué versión de Windows ejecuten. La aplicación solo llama a las API de Windows Runtime si el usuario ejecuta la aplicación como una aplicación empaquetada en Windows 10.

La forma más fácil de agregar comprobaciones en tiempo de ejecución al código es instalar este paquete NuGet: [Aplicaciones auxiliares de Puente de dispositivo de escritorio](https://www.nuget.org/packages/DesktopBridge.Helpers/) y, a continuación, usa el método ``IsRunningAsUWP()`` para desactivar todo el código que llama a las API de Windows Runtime. Consulta esta entrada de blog para obtener más información: [Puente de dispositivo de escritorio: identifica el contexto de la aplicación](/archive/blogs/appconsult/desktop-bridge-identify-the-applications-context).

## <a name="related-samples"></a>Ejemplos relacionados

* [Ejemplo de Hola mundo](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Icono secundario](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Ejemplo de la API de Store](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [Aplicación de WinForms que implementa una UpdateTask de UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Ejemplos de puente de aplicación de escritorio a UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="find-answers-to-your-questions"></a>Encontrar respuestas a tus preguntas

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).