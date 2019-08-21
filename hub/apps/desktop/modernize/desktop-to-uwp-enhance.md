---
Description: Mejore su aplicación de escritorio para los usuarios de Windows 10 mediante el uso de API de Plataforma universal de Windows (UWP).
title: Usar las API de UWP en aplicaciones de escritorio
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 44ea01bbc2200c1b028ed41e7c6a2845c7a1568b
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643363"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>Llamada a las API de UWP en aplicaciones de escritorio

Puede usar las API de Plataforma universal de Windows (UWP) para agregar experiencias modernas a las aplicaciones de escritorio que se encienden para los usuarios de Windows 10.

En primer lugar, configure el proyecto con las referencias necesarias. A continuación, llame a las API de UWP desde el código para agregar experiencias de Windows 10 a la aplicación de escritorio. Puede compilar de forma independiente para los usuarios de Windows 10 o distribuir los mismos archivos binarios a todos los usuarios, independientemente de la versión de Windows que ejecuten.

Algunas API de UWP solo se admiten en aplicaciones de escritorio que se empaquetan en un [paquete MSIX](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root). Para obtener más información, consulte [API de UWP disponibles](desktop-to-uwp-supported-api.md).

## <a name="set-up-your-project"></a>Configurar el proyecto

Tendrás que realizar algunos cambios en tu proyecto para usar las API de UWP.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Modificación de un proyecto .NET para usar Windows Runtime API

Hay dos opciones para los proyectos de .NET:

* Si su aplicación tiene como destino Windows 10 versión 1803 o posterior, puede instalar un paquete de NuGet que proporcione todas las referencias necesarias.
* Como alternativa, puede Agregar las referencias manualmente.

#### <a name="to-use-the-nuget-option"></a>Para usar la opción NuGet

1. Asegúrese de que [las referencias de paquete](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) están habilitadas:

    1. En Visual Studio, haga clic en **herramientas-> administrador de paquetes NuGet-> configuración del administrador de paquetes**.
    2. Asegúrese de que **PackageReference** está seleccionado para el **formato de administración de paquetes predeterminado**.

2. Con el proyecto abierto en Visual Studio, haga clic con el botón derecho en el proyecto en **Explorador de soluciones** y elija **administrar paquetes de NuGet**.

3. En la ventana **Administrador de paquetes NuGet** , asegúrese de que esté seleccionada la opción **incluir versión preliminar** . A continuación, seleccione la pestaña **examinar** y busque `Microsoft.Windows.SDK.Contracts`.

4. Una vez `Microsoft.Windows.SDK.Contracts` encontrado el paquete, en el panel derecho de la ventana **Administrador de paquetes NuGet** , seleccione la **versión** del paquete que desea instalar en función de la versión de Windows 10 a la que desea dirigirse:

    * **10.0.18362. xxxx-Preview**: Elija esta opción para Windows 10, versión 1903.
    * **10.0.17763. xxxx-Preview**: Elija esta opción para Windows 10, versión 1809.
    * **10.0.17134. xxxx-Preview**: Elija esta opción para Windows 10, versión 1803.

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
    |Windows. winmd|C:\Archivos de programa (x86) \Windows\\Kits\10\UnionMetadata<*SDK version*> \Facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Archivos de programa (x86) \Windows\\<Kits\10\References*SDK versión*>\<\Windows.Foundation.UniversalApiContract*versión*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Archivos de programa (x86) \Windows\\<Kits\10\References*SDK versión*>\<\Windows.Foundation.FoundationContract*versión*>|

3. En la ventana **Propiedades**, establece el campo **Copia local** de cada archivo *.winmd* en **False**.

    ![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>Modificar un C++ proyecto de Win32 para usar Windows Runtime API

Use [ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) para consumir Windows Runtime API. C++/WinRT es una completa proyección de lenguaje C++17 estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivo de encabezado y diseñada para darte acceso de primera clase a la moderna API de Windows.

Para configurar el proyecto para C++/WinRT:

* En el caso de los proyectos nuevos, puede instalar la [ C++extensión de Visual Studio/WinRT (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) y C++usar una de las plantillas de proyecto de/WinRT incluidas en esa extensión.
* En el caso de los proyectos existentes, puede instalar el paquete NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) en el proyecto.

Para obtener más información sobre estas opciones, consulte [este artículo](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-windows-10-experiences"></a>Agregar experiencias de Windows 10

Ya estás listo para agregar experiencias modernas que dan a conocer cuándo los usuarios ejecutan tu aplicación en Windows 10. Usa este flujo de diseño.

: white_check_mark: **En primer lugar, decida qué experiencias desea agregar**

Hay muchas entre las que elegir. Por ejemplo, puede simplificar el flujo de pedidos mediante las [API](/windows/uwp/monetize)de monetización o [dirigir la atención a la aplicación](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts) cuando tenga algo interesante de compartir, como una nueva imagen que haya publicado otro usuario.

![Notificación del sistema](images/desktop-to-uwp/toast.png)

Incluso si los usuarios ignoran o descartar el mensaje, pueden verlo de nuevo en el centro de actividades y, a continuación, hacer clic en el mensaje para abrir tu aplicación. Esto aumenta la interacción con la aplicación y tiene la ventaja adicional de que la aplicación aparezca profundamente integrada en el sistema operativo. En este artículo le mostraremos el código para esa experiencia.

Para más ideas, visite la [documentación de UWP](/windows/uwp/get-started/) .

: white_check_mark: **Decida si desea mejorar o extender**

A menudo nos encantará usar los términos *mejorar* y *ampliar*, por lo que dedicaremos un momento a explicar exactamente lo que significa cada uno de estos términos.

Usamos el término *mejorar* para describir Windows Runtime API a las que puede llamar directamente desde la aplicación de escritorio (tanto si ha elegido empaquetar la aplicación en un paquete MSIX) como si no. Cuando haya elegido una experiencia con Windows 10, identifique las API que necesita para crearlo y, a continuación, compruebe si esa API aparece en [esta lista](desktop-to-uwp-supported-api.md). Esta es una lista de las API a las que se puede llamar directamente desde la aplicación de escritorio. Si tu API no aparece en esta lista, se debe a que la funcionalidad asociada a esa API solo se puede ejecutar dentro de un proceso de UWP. A menudo, estas incluyen API que representan XAML de UWP, como un control de mapa de UWP o un aviso de seguridad de Windows Hello.

> [!NOTE]
> Aunque las API que representan el XAML de UWP normalmente no se pueden llamar directamente desde el escritorio, es posible que pueda usar enfoques alternativos. Si quieres hospedar controles XAML de UWP u otras experiencias visuales personalizadas, puedes usar [islas XAML](xaml-islands.md) (a partir de Windows 10, versión 1903) y la [capa visual](visual-layer-in-desktop-apps.md) (a partir de windows 10, versión 1803). Estas características se pueden usar en aplicaciones de escritorio empaquetadas o sin empaquetar.

Si ha elegido empaquetar la aplicación de escritorio en un paquete de MSIX, otra opción es *ampliar* la aplicación agregando un proyecto de UWP a la solución. El proyecto de escritorio sigue siendo el punto de entrada de la aplicación, pero el proyecto de UWP le proporciona acceso a todas las API que no aparecen en [esta lista](desktop-to-uwp-supported-api.md). La aplicación de escritorio puede comunicarse con el proceso UWP mediante un servicio de aplicación y tenemos muchas instrucciones sobre cómo configurarlo. Si desea agregar una experiencia que requiera un proyecto de UWP, vea [extender con componentes de UWP](desktop-to-uwp-extend.md).

: white_check_mark: **Contratos de API de referencia**

Si puede llamar a la API directamente desde la aplicación de escritorio, abra un explorador y busque el tema de referencia de esa API.
Debajo del resumen de la API, encontrarás una tabla en la que se describe el contrato de API para esa API. Este es un ejemplo de esa tabla:

![Tabla de contrato de API](images/desktop-to-uwp/contract-table.png)

Si tienes una aplicación de escritorio basada en .NET, agrega una referencia a ese contrato de API y luego establece la propiedad **Copia local** de ese archivo en **False**. Si tienes un proyecto basado en C++, agrega a **Directorios de inclusión adicionales**, una ruta de acceso a la carpeta que contiene este contrato.

: white_check_mark: **Llame a las API para agregar su experiencia**

Este es el código que usarías para mostrar la ventana de notificación que hemos visto anteriormente. Estas API aparecen en esta [lista](desktop-to-uwp-supported-api.md) , por lo que puede Agregar este código a la aplicación de escritorio y ejecutarlo en este momento.

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

Puede modernizar su aplicación para Windows 10 sin tener que crear una nueva bifurcación y mantener bases de código independientes.

Si quieres compilar archivos binarios independientes para los usuarios de Windows 10, usa la compilación condicional. Si prefieres compilar un conjunto de archivos binarios que implementas en todos los usuarios de Windows, usa las comprobaciones en tiempo de ejecución.

Echemos un vistazo rápido a cada opción.

### <a name="conditional-compilation"></a>Compilación condicional

Puedes mantener una base de código y compilar un conjunto de archivos binarios solo para los usuarios de Windows 10.

En primer lugar, agrega una nueva configuración de compilación al proyecto.

![Configuración de compilación](images/desktop-to-uwp/build-config.png)

Para esa configuración de compilación, cree una constante que identifique el código que llama a las API de Windows Runtime.  

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

Puedes compilar un conjunto de archivos binarios para todos tus usuarios de Windows independientemente de qué versión de Windows ejecuten. La aplicación llama a Windows Runtime API solo si el usuario ejecuta la aplicación como una aplicación empaquetada en Windows 10.

La forma más fácil de agregar comprobaciones en tiempo de ejecución al código es instalar este paquete Nuget: Las [aplicaciones auxiliares de puente de escritorio](https://www.nuget.org/packages/DesktopBridge.Helpers/) y ``IsRunningAsUWP()`` , a continuación, usan el método para activar todo el código que llama a las API de Windows Runtime. vea esta entrada de blog para obtener más detalles: [Puente de escritorio: identifique el contexto de la aplicación](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-samples"></a>Muestras relacionadas

* [Ejemplo de Hola mundo](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Icono secundario](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Ejemplo de API de almacén](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [Aplicación de WinForms que implementa un UpdateTask de UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Ejemplos de puente de aplicación de escritorio a UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="support-and-feedback"></a>Soporte técnico y comentarios

**Encuentre respuestas a sus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Enviar comentarios o realizar sugerencias de características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
