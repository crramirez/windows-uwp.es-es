---
description: Esta guía te ayuda a crear interfaces de usuario de UWP basadas en Fluent directamente en tus aplicaciones de WPF y Windows Forms
title: Controles UWP en aplicaciones de escritorio
ms.date: 04/16/2019
ms.topic: article
keywords: Windows 10, uwp, formularios windows forms, wpf, Islas de xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: e6074202a05c80a9dc759cdf81b2c20c7cc17d07
ms.sourcegitcommit: b8087f8b6cf8367f8adb7d6db4581d9aa47b4861
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2019
ms.locfileid: "67414098"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Los controles host UWP XAML en aplicaciones de escritorio (Islas de XAML)

A partir de Windows 10, versión 1903, puede hospedar controles UWP en aplicaciones de escritorio no de UWP mediante una característica denominada *XAML Islas*. Esta característica permite mejorar la apariencia, sensación y funcionalidad de las aplicaciones de escritorio existentes con las últimas características de interfaz de usuario de Windows 10 que solo están disponibles a través de controles UWP. Esto significa que puede usar las características UWP, como [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) y controles que admiten la [sistema de diseño Fluent](/windows/uwp/design/fluent-design-system/index) en su existente WPF, Windows Forms y aplicaciones de Win32 de C++.

Se proporcionan varias maneras de usar islas de XAML en el WPF, Windows Forms, y C++ aplicaciones Win32, dependiendo de la tecnología o un marco que usa. 

> [!NOTE]
> Si tiene comentarios acerca de las Islas de XAML, cree un nuevo problema en el [Microsoft.Toolkit.Win32 repositorio](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) y deje sus comentarios no existe. Si prefiere enviar sus comentarios de forma privada, puede enviarlo a XamlIslandsFeedback@microsoft.com. Sus opiniones y escenarios son muy importantes para nosotros.

## <a name="how-do-xaml-islands-work"></a>¿Cómo funcionan las Islas de XAML?

A partir de Windows 10, versión 1903, se proporcionan dos maneras de utilizar islas de XAML en el WPF, Windows Forms, y C++ aplicaciones Win32:

* El SDK de Windows proporciona varias clases de Windows en tiempo de ejecución y las interfaces COM que la aplicación puede usar para hospedar cualquier control UWP que se deriva de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement). En conjunto, estas clases e interfaces se denominan el *XAML UWP API de hospedaje*, y permiten a los controles UWP de host en cualquier elemento de interfaz de usuario de la aplicación que tiene un identificador de ventana asociado (HWND). Para obtener más información acerca de esta API, consulte [mediante la API de hospedaje de XAML](using-the-xaml-hosting-api.md).

* El [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) también proporciona controles adicionales de la isla de XAML para WPF y Windows Forms. Estos controles usar el XAML UWP internamente en la API de hospedaje e implementan todo el comportamiento en caso contrario, deberá administrar usted mismo si usa el XAML UWP API directamente, incluidos los cambios de navegación y el diseño de teclado de hospedaje. Para las aplicaciones de WPF y Windows Forms, se recomienda encarecidamente que use estos controles en lugar del XAML UWP hospedaje API directamente porque abstraen muchos de los detalles de implementación del uso de la API. Tenga en cuenta que a partir de Windows 10, versión 1903, estos controles son [disponible en versión preliminar para desarrolladores](#feature-roadmap).

> [!NOTE]
> Las aplicaciones de escritorio de Win32 de C++ deben usar el XAML UWP API de hospedaje para hospedar controles de UWP. Los controles de la isla de XAML en el Kit de herramientas de la Comunidad de Windows no están disponibles para C++ aplicaciones de escritorio de Win32.

Hay dos tipos de controles de la isla de XAML proporcionados por el Kit de herramientas de la Comunidad de Windows para las aplicaciones de WPF y Windows Forms: *ajusta los controles* y *controles host*. 

### <a name="wrapped-controls"></a>Controles ajustados

Las aplicaciones de WPF y Windows Forms pueden usar una selección de controles UWP ajustadas en el [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Se denominan *ajusta los controles* como que ajustan a la interfaz y funcionalidad de un control específico de UWP. Puede agregar estos controles directamente a la superficie de diseño de su proyecto WPF o Windows Forms y, a continuación, utilizarlas como cualquier otro control WPF o Windows Forms en el diseñador.

Los siguientes controles UWP ajustados para la implementación de islas de XAML están disponibles actualmente para las aplicaciones de WPF y Windows Forms.

| Control | Sistema operativo mínimo admitido | Descripción |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, versión 1903 | Proporcione las barras de herramientas de una superficie y relacionados para la interacción del usuario basadas en tinta de Windows en la aplicación de escritorio de Windows Forms o WPF. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, versión 1903 | Inserta una vista que se transmite por secuencias y representa el contenido multimedia como vídeo en su aplicación de escritorio de Windows Forms o WPF. |
| [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, versión 1903 | Le permite mostrar un mapa fotorrealista o simbólico en la aplicación de escritorio de Windows Forms o WPF. |

Además de los controles ajustados para islas de XAML, el Kit de herramientas de la Comunidad de Windows también proporciona los siguientes controles para hospedar contenido web.

| Control | Sistema operativo mínimo admitido | Descripción |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, versión 1803 | Usa el motor de representación Microsoft Edge para mostrar contenido web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Proporciona una versión de **WebView** que es compatible con varias versiones del sistema operativo. Este control usa el motor de representación Microsoft Edge para mostrar contenido web en Windows 10, versión 1803 y posteriores y el motor de representación de Internet Explorer para mostrar contenido web en las versiones anteriores de Windows 10, Windows 8.x y Windows 7. |

### <a name="host-controls"></a>Controles host

Para escenarios más allá de las cubiertas por los controles disponibles ajustados, también pueden usar las aplicaciones de WPF y Windows Forms el [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en controlar la [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Este control puede alojar cualquier control UWP que se derive de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluyendo cualquier control que UWP proporcionada el SDK de Windows, así como los controles de usuario personalizados. Este control es compatible con el SDK de Windows 10 Insider Preview compilación 17709 y versiones posteriores.

### <a name="architecture-overview"></a>Introducción a la arquitectura

Echa un vistazo rápido a la manera en que estos controles se organizan desde el punto de vista de la arquitectura.

![Arquitectura de control de host](images/xaml-islands/host-controls.png)

Las API que aparecen en la parte inferior de este diagrama se incluye con el Windows SDK. Los controles ajustados y controles host están disponibles a través de paquetes de Nuget en el Kit de herramientas de la Comunidad de Windows.

## <a name="requirements"></a>Requisitos

Islas de XAML requieren Windows 10, versión 1903 y versiones posterior. Para usar las Islas de XAML en la aplicación, debe configurar el proyecto.

### <a name="step-1-modify-your-project-to-use-windows-runtime-apis"></a>Paso 1: Modificar el proyecto para usar Windows Runtime APIs

Para obtener instrucciones, consulte [en este artículo](desktop-to-uwp-enhance.md#set-up-your-project).

### <a name="step-2-enable-xaml-island-support-in-your-project"></a>Paso 2: Soporte técnico de habilitar la isla de XAML en el proyecto

Realice uno de los siguientes cambios a su proyecto para habilitar la compatibilidad de la isla de XAML. Para obtener más información, consulte [esta entrada de blog](https://techcommunity.microsoft.com/t5/Windows-Dev-AppConsult/Using-XAML-Islands-on-Windows-10-19H1-fixing-the-quot/ba-p/376330#M117).

#### <a name="option-1-package-your-application-in-an-msix-package"></a>Opción 1: Empaquetar la aplicación en un paquete MSIX  

Instale Windows 10, versión 1903 SDK (o una versión posterior). A continuación, empaquetar la aplicación en un paquete MSIX agregando un [proyecto de empaquetado de aplicaciones de Windows](https:/docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) a la solución y agregar una referencia al proyecto de WPF o Windows Forms.

#### <a name="option-2-set-the-maxversiontested-value-in-your-assembly-manifest"></a>Opción 2: Establezca el valor de maxVersionTested en el manifiesto del ensamblado

Si no desea empaquetar la aplicación en un paquete MSIX, puede agregar un [manifiesto del ensamblado en paralelo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) al proyecto y agregue el **maxVersionTested** valor para el manifiesto para especificar que el aplicación es compatible con Windows 10, versión 1903 o posterior.

1. Si aún no tiene un ensamblado de manifiesto en el proyecto, agregue un nuevo archivo XML al proyecto y asígnele el nombre **app.manifest**. Para una aplicación WPF o Windows Forms, asegúrese de que también asignar el **manifiesto** propiedad **. app.manifest** en el **aplicación** página de su [proyecto propiedades](https://docs.microsoft.com/visualstudio/ide/reference/application-page-project-designer-csharp?view=vs-2019#resources).
2. En el manifiesto del ensamblado, incluya el **compatibilidad** elemento y los elementos secundarios que se muestra en el ejemplo siguiente. Reemplace el **Id** atributo de la **maxVersionTested** elemento con el número de versión de Windows 10 tiene como destino (debe ser Windows 10, versión 1903 o una versión posterior). 

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

## <a name="feature-roadmap"></a>Guía básica de característica

A partir de la versión de Windows 10, versión 1903, los controles ajustados y controles host en el Kit de herramientas de la Comunidad de Windows están aún en versión preliminar para desarrolladores hasta que esté disponible la versión 1.0 de los controles.

* Versión 1.0 de los controles de .NET Framework 4.6.2 y posteriormente se prevé que se publicará en el [versión 6.0 del Kit de herramientas](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones).
* Versión 1.0 de los controles de .NET Core 3 están planificados para una versión posterior del Kit de herramientas.
* Si desea probar las versiones preliminares más recientes de las versiones de la versión 1.0 de estos controles para .NET Framework y .NET Core 3, vea el **6.0.0-preview3** de paquetes de NuGet en el [UWP Community Toolkit](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) Galería.

Para obtener más información, consulte [esta entrada de blog](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap).

## <a name="additional-resources"></a>Recursos adicionales

Para obtener más información general y tutoriales sobre el uso de islas de XAML, vea los siguientes artículos y recursos:

* [Modernice un tutorial de aplicación WPF](modernize-wpf-tutorial.md): Este tutorial proporciona instrucciones paso a paso para usar los controles de host y controles ajustados en el Kit de herramientas de la Comunidad de Windows para agregar controles UWP a una aplicación de línea de negocio WPF existente. En este tutorial se incluye el código completo de la aplicación de WPF, así como instrucciones detalladas para cada paso en el proceso.
* [Islas de XAML v1 - las actualizaciones y guía básica](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): Esta entrada de blog describe muchas preguntas habituales acerca de las Islas de XAML y proporciona una guía detallada de desarrollo.
