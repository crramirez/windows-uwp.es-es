---
description: Esta guía te ayuda a crear interfaces de usuario de UWP basadas en Fluent directamente en tus aplicaciones de WPF y Windows Forms
title: Controles de UWP en aplicaciones de escritorio
ms.date: 07/17/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Islas XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 79dcd6069a5746e04565db660e6f5c03988a94a4
ms.sourcegitcommit: 2062d06567ef087ad73507a03ecc726a7d848361
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68303562"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Hospedar controles XAML de UWP en aplicaciones de escritorio (Islas XAML)

A partir de Windows 10, versión 1903, puede hospedar controles de UWP en aplicaciones de escritorio que no son de UWP con una característica denominada *islas XAML*. Esta característica permite mejorar la apariencia, el funcionamiento y la funcionalidad de las aplicaciones de escritorio existentes con las características más recientes de la interfaz de usuario de Windows 10 que solo están disponibles a través de los controles de UWP. Esto significa que puede usar las características de UWP, como [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) y los controles que admiten el [sistema de diseño fluida](/windows/uwp/design/fluent-design-system/index) , en las aplicaciones C++ de WPF, Windows Forms y Win32 existentes.

Se proporcionan varias maneras de usar las islas XAML en las aplicaciones de WPF, C++ Windows Forms y Win32, en función de la tecnología o el marco de trabajo que se use. 

> [!NOTE]
> Si tiene comentarios sobre las islas XAML, cree un nuevo problema en el [repositorio Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) y deje los comentarios allí. Si prefiere enviar sus comentarios de forma privada, puede enviarlos a XamlIslandsFeedback@microsoft.com. Sus conocimientos y escenarios son muy importantes para nosotros.

## <a name="how-do-xaml-islands-work"></a>¿Cómo funcionan las islas XAML?

A partir de Windows 10, versión 1903, se proporcionan dos maneras de utilizar las islas XAML en las aplicaciones de WPF C++ , Windows Forms y Win32:

* El Windows SDK proporciona varias clases de Windows Runtime e interfaces COM que la aplicación puede usar para hospedar cualquier control de UWP que se derive de [**Windows. UI. Xaml. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement). Colectivamente, estas clases e interfaces se denominan *API de hospedaje XAML de UWP*y permiten hospedar controles de UWP en cualquier elemento de la interfaz de usuario de la aplicación que tenga un identificador de ventana asociado (HWND). Para obtener más información sobre esta API, vea [uso de la API de hospedaje de XAML](using-the-xaml-hosting-api.md).

* El [Kit de herramientas](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) de la comunidad de Windows también proporciona controles adicionales de la isla XAML para WPF y Windows Forms. Estos controles usan internamente la API de hospedaje XAML de UWP e implementan todo el comportamiento que, de lo contrario, tendría que administrarse si usara la API de hospedaje XAML de UWP directamente, incluidos los cambios de diseño y navegación del teclado. En el caso de las aplicaciones de WPF y Windows Forms, se recomienda encarecidamente usar estos controles en lugar de la API de hospedaje XAML de UWP directamente porque abstraen muchos de los detalles de implementación del uso de la API. Tenga en cuenta que, a partir de la versión 1903 de Windows 10, estos controles están [disponibles como vista previa para desarrolladores](#feature-roadmap).

> [!NOTE]
> C++Las aplicaciones de escritorio de Win32 deben usar la API de hospedaje XAML de UWP para hospedar controles de UWP. Los controles de la isla de XAML en el kit de herramientas de C++ la comunidad de Windows no están disponibles para las aplicaciones de escritorio de Win32.

Hay dos tipos de controles de isla XAML proporcionados por el kit de herramientas de la comunidad de Windows para WPF y Windows Forms aplicaciones: *controles encapsulados* y *controles host*. 

### <a name="wrapped-controls"></a>Controles ajustados

Las aplicaciones de WPF y Windows Forms pueden usar una selección de controles de UWP ajustados en el kit de herramientas de la [comunidad de Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Se denominan *controles ajustados* porque ajustan la interfaz y la funcionalidad de un control UWP específico. Puede agregar estos controles directamente a la superficie de diseño de su proyecto de WPF o Windows Forms y, a continuación, usarlos como cualquier otro control de WPF o Windows Forms en el diseñador.

Los siguientes controles de UWP ajustados para implementar islas XAML están disponibles actualmente para WPF (vea el paquete [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) ) y Windows Forms (vea el paquete [Microsoft. Toolkit. Forms. UI. Controls).](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) ).

| Control | Sistema operativo mínimo admitido | Descripción |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, versión 1903 | Proporcione una superficie y barras de herramientas relacionadas para la interacción del usuario basada en Windows Ink en la aplicación de escritorio de Windows Forms o WPF. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, versión 1903 | Incrusta una vista que transmite y representa contenido multimedia, como vídeo, en la aplicación de escritorio Windows Forms o WPF. |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, versión 1903 | Permite mostrar un mapa simbólico o de semireal en la aplicación de escritorio de Windows Forms o WPF. |

Además de los controles ajustados para las islas XAML, el kit de herramientas de la comunidad de Windows también proporciona los siguientes controles para hospedar contenido web en WPF (vea el paquete [Microsoft. Toolkit. WPF. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView) ) y Windows Forms aplicaciones (vea el paquete [Microsoft. Toolkit. Forms. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView) ).

| Control | Sistema operativo mínimo admitido | Descripción |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, versión 1803 | Usa el motor de representación de Microsoft Edge para mostrar el contenido Web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Proporciona una versión de **WebView** que es compatible con más versiones del sistema operativo. Este control usa el motor de representación de Microsoft Edge para mostrar el contenido web en Windows 10 versión 1803 y versiones posteriores, y el motor de representación de Internet Explorer para mostrar el contenido web en versiones anteriores de Windows 10, Windows 8. x y Windows 7. |

### <a name="host-controls"></a>Controles host

En el caso de escenarios más allá de los incluidos en los controles ajustados disponibles, las aplicaciones WPF y Windows Forms también pueden usar el control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en el kit de herramientas de la [comunidad de Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Este control puede hospedar cualquier control de UWP que se derive de [**Windows. UI. Xaml. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluido cualquier control de UWP proporcionado por el Windows SDK, así como los controles de usuario personalizados. Este control es compatible con Windows 10 Insider Preview SDK Build 17709 y versiones posteriores.

Estos controles están disponibles en los paquetes [Microsoft. Toolkit. WPF. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (para WPF) y [Microsoft. Toolkit. Forms. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (para Windows Forms). Estos paquetes se incluyen en los paquetes [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) y [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) que contienen los controles ajustados.

### <a name="architecture-overview"></a>Introducción a la arquitectura

Echa un vistazo rápido a la manera en que estos controles se organizan desde el punto de vista de la arquitectura.

![Arquitectura de control de host](images/xaml-islands/host-controls.png)

Las API que aparecen en la parte inferior de este diagrama se incluye con el Windows SDK. Los controles encapsulados y los controles host están disponibles a través de paquetes Nuget en el kit de herramientas de la comunidad de Windows.

<span id="requirements" />

## <a name="configure-your-project-to-use-xaml-islands"></a>Configurar el proyecto para usar islas XAML

Las islas XAML requieren Windows 10, versión 1903 y versiones posteriores. Para usar islas XAML en la aplicación, primero debe configurar el proyecto.

### <a name="wpf-and-windows-forms"></a>WPF y Windows Forms

* Modifique el proyecto para usar Windows Runtime API. Para obtener instrucciones, consulte [este artículo](desktop-to-uwp-enhance.md#set-up-your-project).

* Instale los paquetes NuGet más recientes de [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) (para WPF) o [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) (para Windows Forms) en el proyecto. Asegúrese de instalar la versión 6.0.0-Preview 6.4 o una versión posterior del paquete.

### <a name="cwin32"></a>C++/Win32

* Modifique el proyecto para usar Windows Runtime API. Para obtener instrucciones, consulte [este artículo](desktop-to-uwp-enhance.md#set-up-your-project).
* Realiza una de las siguientes acciones:

    **Empaquete su aplicación en un paquete MSIX**. Empaquetar la aplicación en un [paquete de MSIX](https://docs.microsoft.com/windows/msix/) proporciona muchas ventajas de implementación y tiempo de ejecución.
    1. Instale el SDK de Windows 10, versión 1903 (o una versión posterior).
    2. Empaquete la aplicación en un paquete de MSIX agregando un proyecto de paquete de [aplicación de Windows](https:/docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) a la solución y C++agregando una referencia al proyecto de/Win32.

    **Establezca el valor de maxversiontested en el manifiesto de aplicación**. Si no quiere empaquetar la aplicación en un paquete de MSIX, antes de poder usar las islas XAML, debe agregar un [manifiesto de aplicación](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) al proyecto y agregar el elemento **maxversiontested** al manifiesto para especificar que la aplicación es compatible con Windows 10, versión 1903 o posterior.
    1. Si aún no tiene un manifiesto de aplicación en el proyecto, agregue un nuevo archivo XML al proyecto y asígnele el nombre **app. manifest**.
    2. En el manifiesto de aplicación, incluya el elemento de **compatibilidad** y los elementos secundarios que se muestran en el ejemplo siguiente. Reemplace el atributo **ID** del elemento **maxversiontested** por el número de versión de Windows 10 que tiene como destino (debe ser windows 10, versión 1903 o una versión posterior).

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
            <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
                <application>
                    <!-- Windows 10 -->
                    <maxversiontested Id="10.0.18362.0"/>
                    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
                </application>
            </compatibility>
        </assembly>
        ```

        > [!NOTE]
        > Al agregar un elemento **maxversiontested** a un manifiesto de aplicación, es posible que vea la siguiente advertencia de compilación en el `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`proyecto:. Esta advertencia no indica que nada esté mal en el proyecto y se puede omitir.

## <a name="feature-roadmap"></a>Guía básica de características

A partir del lanzamiento de Windows 10, versión 1903, los controles ajustados y los controles host del kit de herramientas de la comunidad de Windows están todavía en la vista previa del desarrollador hasta que la versión 1,0 de los controles esté disponible.

* La versión 1,0 de los controles para el .NET Framework 4.6.2 y versiones posteriores está planeada para su lanzamiento en la [versión 6,0 del kit de herramientas](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones).
* La versión 1,0 de los controles para .NET Core 3 está planeada para una versión posterior del kit de herramientas.
* Si desea probar las versiones preliminares más recientes de la versión 1,0 de estos controles para el .NET Framework y .NET Core 3, consulte los paquetes de NuGet **6.0.0-Preview 6.4** en la galería de [UWP Community Toolkit](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) .

Para obtener más información, consulte [esta entrada de blog](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap).

## <a name="additional-resources"></a>Recursos adicionales

Para obtener información general y tutoriales sobre el uso de las islas XAML, consulte los siguientes artículos y recursos:

* [Modernización de un tutorial de aplicación de WPF](modernize-wpf-tutorial.md): En este tutorial se proporcionan instrucciones paso a paso para usar los controles ajustados y los controles host en el kit de herramientas de la comunidad de Windows para agregar controles de UWP a una aplicación de línea de negocio de WPF existente. En este tutorial se incluye el código completo de la aplicación WPF, así como instrucciones detalladas para cada paso del proceso.
* [Islas de XAML v1: actualizaciones y mapa de ruta](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): En esta entrada de blog se describen muchas preguntas comunes sobre las islas XAML y se proporciona un mapa de ruta de desarrollo detallado.
