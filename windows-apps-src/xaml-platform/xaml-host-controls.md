---
description: Esta guía te ayuda a crear interfaces de usuario de UWP basadas en Fluent directamente en tus aplicaciones de WPF y Windows Forms
title: Controles UWP en aplicaciones de escritorio
ms.date: 01/11/2019
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bf25fea6ca6e8809c12324ae57a42cc712ded2a5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619710"
---
# <a name="uwp-controls-in-desktop-applications"></a>Controles UWP en aplicaciones de escritorio

> [!NOTE]
> Islas XAML están disponibles actualmente como versión preliminar para desarrolladores. Aunque le animamos a probarlas en su propio código del prototipo ahora, no se recomienda usar ellos en código de producción en este momento. Estas API y controles seguirá maduren y estabilizarse en futuras versiones de Windows. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.
>
> Si tiene comentarios acerca de las Islas XAML, cree un nuevo problema en el [WindowsCommunityToolkit repositorio](https://github.com/windows-toolkit/WindowsCommunityToolkit/issues) y deje sus comentarios no existe. Si prefiere enviar sus comentarios de forma privada, puede enviarlo a XamlIslandsFeedback@microsoft.com. Sus opiniones y escenarios son muy importantes para nosotros.

Windows 10 ahora le permite usar controles UWP en aplicaciones de escritorio no de UWP para que se pueden mejorar la apariencia, sensación y funcionalidad de las aplicaciones de escritorio existentes con las últimas características de interfaz de usuario de Windows 10 que solo están disponibles a través de controles UWP. Esto significa que puede usar las características UWP, como [Windows Ink](../design/input/pen-and-stylus-interactions.md) y controles que admiten la [sistema de diseño Fluent](../design/fluent-design-system/index.md) en su existente WPF, Windows Forms y aplicaciones de Win32 de C++. Este escenario de desarrolladores a veces se denomina *Islas XAML*.

Se proporcionan varias maneras de usar Islas XAML en las aplicaciones WPF, Windows Forms y Win32 de C++, dependiendo de la tecnología o un marco que usa.

## <a name="wrapped-controls"></a>Controles ajustados

Las aplicaciones de WPF y Windows Forms pueden usar una selección de controles UWP ajustadas en el [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Nos referimos a estos controles como *ajusta los controles* como que ajustan a la interfaz y funcionalidad de un control específico de UWP. Puede agregar estos controles directamente a la superficie de diseño de su proyecto WPF o Windows Forms y, a continuación, utilizarlas como cualquier otro control WPF o Windows Forms en el diseñador.

> [!NOTE]
> Controles ajustados no están disponibles para las aplicaciones de escritorio de Win32 de C++. Estos tipos de aplicaciones deben usar el [XAML UWP API de hospedaje](#uwp-xaml-hosting-api).

Los controles UWP ajustados siguientes están disponibles actualmente para las aplicaciones de WPF y Windows Forms. Más controles UWP ajustado están planificados para futuras versiones del Kit de herramientas de comunidad de Windows.

| Control | Sistema operativo mínimo admitido | Descripción |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, versión 1803 | Usa el motor de representación Microsoft Edge para mostrar contenido web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Proporciona una versión de **WebView** que es compatible con varias versiones del sistema operativo. Este control usa el motor de representación Microsoft Edge para mostrar contenido web en Windows 10, versión 1803 y posteriores y el motor de representación de Internet Explorer para mostrar contenido web en las versiones anteriores de Windows 10, Windows 8.x y Windows 7. |
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, versión 1809 (compilación 17763) | Proporcione las barras de herramientas de una superficie y relacionados para la interacción del usuario basadas en tinta de Windows en la aplicación de escritorio de Windows Forms o WPF. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, versión 1809 (compilación 17763) | Inserta una vista que se transmite por secuencias y representa el contenido multimedia como vídeo en su aplicación de escritorio de Windows Forms o WPF. |
| [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, versión 1809 (compilación 17763) | Le permite mostrar un mapa fotorrealista o simbólico en la aplicación de escritorio de Windows Forms o WPF. |

## <a name="host-controls"></a>Controles host

Para escenarios más allá de las cubiertas por los controles disponibles ajustados, también pueden usar las aplicaciones de WPF y Windows Forms el [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en controlar la [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Este control puede alojar cualquier control UWP que se derive de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluyendo cualquier control que UWP proporcionada el SDK de Windows, así como los controles de usuario personalizados. Este control es compatible con el SDK de Windows 10 Insider Preview compilación 17709 y versiones posteriores.

> [!NOTE]
> Controles host no están disponibles para aplicaciones de escritorio de Win32 de C++. Estos tipos de aplicaciones deben usar el [XAML UWP API de hospedaje](#uwp-xaml-hosting-api).

## <a name="uwp-xaml-hosting-api"></a>XAML UWP API de hospedaje

Si tiene una aplicación Win32 de C++, puede usar el *XAML UWP API de hospedaje* para hospedar cualquier control UWP que se deriva de [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) en cualquier elemento de interfaz de usuario de su aplicación que tiene un identificador de ventana asociado (HWND). Esta API se introdujo en el SDK de Windows 10 Insider Preview 17709 de compilación. Para obtener más información sobre el uso de esta API, consulte [mediante la API de hospedaje en una aplicación de escritorio de XAML](using-the-xaml-hosting-api.md).

> [!NOTE]
> Las aplicaciones de escritorio de Win32 de C++ deben usar el XAML UWP API de hospedaje para hospedar controles de UWP. Controles ajustados y controles host no están disponibles para estos tipos de aplicaciones. Para las aplicaciones de WPF y Windows Forms, se recomienda usar los controles de host y controles ajustados en el Kit de herramientas de comunidad de Windows en lugar del XAML UWP API de hospedaje. Estos controles usar el XAML UWP internamente en la API de hospedaje y proporcionan una experiencia de desarrollo más sencilla. Sin embargo, puede usar el XAML UWP API de hospedaje directamente en las aplicaciones de WPF y Windows Forms si elige.

## <a name="architecture-overview"></a>Introducción a la arquitectura

Echa un vistazo rápido a la manera en que estos controles se organizan desde el punto de vista de la arquitectura. Los nombres usados en este diagrama están sujetos a cambios.  

![Arquitectura de control de host](images/host-controls.png)

Las API que aparecen en la parte inferior de este diagrama se incluye con el Windows SDK. Los controles que agregarás en el diseñador se incluyen como paquetes de Nuget en el Kit de herramientas de Comunidad Windows.

Estos nuevos controles tienen limitaciones por lo que, antes de usarlos, dedica un momento a revisar qué no se admite aún y qué funciona solo con soluciones alternativas.

## <a name="limitations"></a>Limitaciones

### <a name="whats-supported"></a>Lo que es compatible

En la mayor parte, todo se admite a menos que se mencione de manera explícita en la lista siguiente.

### <a name="whats-supported-only-with-workarounds"></a>Qué se admite solo con soluciones alternativas

:heavy_check_mark: Hospedaje de varios controles de la Bandeja de entrada dentro de varias ventanas. Tendrás que colocar cada ventana en su propio subproceso.

:heavy_check_mark: Uso de ``x:Bind`` con controles alojados. Tendrás que declarar el modelo de datos en una biblioteca .NET Standard.

:heavy_check_mark: C#-en función de los controles de terceros. Si tienes el código fuente para un control de terceros, puedes compilar con él.

### <a name="whats-not-yet-supported"></a>Lo que no es compatible todavía

: no_entry_sign: Herramientas de accesibilidad que funcionan perfectamente en la aplicación y hospedan los controles.

: no_entry_sign: Contenido localizado en controles que se agregan a las aplicaciones que no contienen un paquete de aplicación de Windows.

: no_entry_sign: Referencias de recursos realizadas en XAML en las aplicaciones que no contienen un paquete de aplicación de Windows.

: no_entry_sign: Controles de responder correctamente a los cambios de PPP y la escala.

: no_entry_sign: Agregar un **WebView** control a un control de usuario personalizado (en el subproceso, desactivar subproceso o fuera de proceso).

: no_entry_sign: El [Mostrar resaltado](https://docs.microsoft.com/windows/uwp/design/style/reveal) Fluent efecto.

: no_entry_sign: Inserción de entradas manuscritas en línea, @Places, y @People para los controles de entrada.

: no_entry_sign: Asignar teclas de aceleración.

: no_entry_sign: Controles de terceros basados en C++.

: no_entry_sign: Hospedaje de controles de usuario personalizados.

Los elementos de esta lista cambiarán probablemente a medida que seguimos mejorando la experiencia de llevar Fluent al escritorio.  
