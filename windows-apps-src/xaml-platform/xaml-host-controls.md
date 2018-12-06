---
description: Esta guía te ayuda a crear interfaces de usuario de UWP basadas en Fluent directamente en tus aplicaciones de WPF y Windows Forms
title: Controles UWP en aplicaciones de escritorio
ms.date: 09/21/2018
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bd22aa761d4a9a79c95c7bc424ab1d2a31ca6cdf
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8756629"
---
# <a name="uwp-controls-in-desktop-applications"></a>Controles UWP en aplicaciones de escritorio

> [!NOTE]
> Las API y los controles mencionados en este artículo son actualmente disponibles como una vista previa de desarrollador. Aunque te animamos a probarlas en su propio código prototipo ahora, no es recomendable que usas en el código de producción en este momento. Estas API y los controles seguirán madurando y estabilizar en futuras versiones de Windows. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

Windows 10 ahora te permite usar los controles UWP en aplicaciones de escritorio no UWP para que puede mejorar el aspecto, sensación y la funcionalidad de las aplicaciones de escritorio existentes con las últimas características de la interfaz de usuario de Windows 10 que solo están disponibles a través de los controles de UWP. Esto significa que puedes usar características UWP, como [Entrada de lápiz de Windows](../design/input/pen-and-stylus-interactions.md) y los controles que admiten el [Sistema Fluent Design](../design/fluent-design-system/index.md) en tu existente WPF, Windows Forms y las aplicaciones Win32 de C++. Este escenario de desarrollador se conoce como *Islas XAML*.

Ofrecemos varias formas de usar Islas XAML en las aplicaciones WPF, Windows Forms y Win32 de C++, dependiendo de la tecnología o el marco que estás usando.

## <a name="wrapped-controls"></a>Controles ajustados

Aplicaciones de WPF y Windows Forms puede utilizar una selección de los controles UWP encapsulados en el [Kit de herramientas de comunidad Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Nos referimos a estos controles como *encapsuladas controles* como que ajustan a la interfaz y la funcionalidad de un control UWP específico. Puedes agregar estos controles directamente a la superficie de diseño de tu proyecto WPF o Windows Forms y, a continuación, usarlos como cualquier otro control WPF o Windows Forms en el diseñador.

> [!NOTE]
> Controles ajustados no están disponibles para aplicaciones de escritorio de Win32 de C++. Estos tipos de aplicaciones deben usar la [API de hospedaje de XAML de UWP](#uwp-xaml-hosting-api).

Los siguientes controles UWP ajustados están actualmente disponibles para aplicaciones de WPF y Windows Forms. Controles UWP encapsulada más se tiene previstos para las versiones futuras del Kit de herramientas de comunidad Windows.

| Control | Mínima admitida del sistema operativo | Descripción |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, versión 1803 | Usa el motor de representación de Microsoft Edge para mostrar el contenido web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows7 | Proporciona una versión de **WebView** que sea compatible con versiones de sistema operativo más. Este control usa el motor de representación de Microsoft Edge para mostrar contenido web en Windows 10 versión 1803 y versiones posteriores y el motor de representación de Internet Explorer para mostrar contenido web en versiones anteriores de Windows 10, Windows 8.x y Windows 7. |
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 Insider Preview SDK compilación 17709 | Proporcionar las barras de herramientas de una superficie y relacionadas para la interacción de usuario basada en la entrada de lápiz de Windows en la aplicación de escritorio de Windows Forms o WPF. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 Insider Preview SDK compilación 17709 | Inserta una vista que transmite y representa el contenido multimedia, como vídeo en la aplicación de escritorio de Windows Forms o WPF. |

## <a name="host-controls"></a>Controles de host

Para los escenarios más allá de los cubiertos por los controles de encapsulado disponibles, WPF y Windows Forms también puede usar el control de [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en el [Kit de herramientas de comunidad Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Este control puede hospedar cualquier control UWP que se deriva de [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluido cualquier control UWP proporcionado el SDK de Windows, así como los controles de usuario personalizados. Este control es compatible con versiones de 17709 y versiones posteriores de la compilación de Windows 10 Insider Preview SDK.

> [!NOTE]
> Controles de host no están disponibles para aplicaciones de escritorio de Win32 de C++. Estos tipos de aplicaciones deben usar la [API de hospedaje de XAML de UWP](#uwp-xaml-hosting-api).

## <a name="uwp-xaml-hosting-api"></a>API de hospedaje de XAML de UWP

Si tienes una aplicación Win32 de C++, puedes usar la *API de hospedaje de XAML de UWP* para hospedar cualquier control UWP que se deriva de [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) en cualquier elemento de la interfaz de usuario de la aplicación que tiene un identificador de ventana asociada (HWND). Esta API se introdujo en Windows 10 Insider Preview SDK compilación 17709. Para obtener más información sobre cómo usar esta API, consulta [mediante la API en una aplicación de escritorio de hospedaje de XAML](using-the-xaml-hosting-api.md).

> [!NOTE]
> Las aplicaciones de escritorio de Win32 de C++ deben usar la API de hospedaje para hospedar los controles de UWP de XAML de UWP. Controles ajustados y controles host no están disponibles para estos tipos de aplicaciones. Para las aplicaciones de WPF y Windows Forms, te recomendamos que uses los controles ajustados y host en el Kit de herramientas de comunidad de Windows en lugar del XAML de UWP API de hospedaje. Estos controles usan el XAML de UWP, API de hospedaje internamente y proporcionan una experiencia de desarrollo más sencilla. Sin embargo, puedes usar la API de hospedaje directamente en las aplicaciones de WPF y Windows Forms si eliges de XAML de UWP.

## <a name="architecture-overview"></a>Introducción a la arquitectura

Echa un vistazo rápido a la manera en que estos controles se organizan desde el punto de vista de la arquitectura. Los nombres usados en este diagrama están sujetos a cambios.  

![Arquitectura de control de host](images/host-controls.png)

Las API que aparecen en la parte inferior de este diagrama se incluye con el Windows SDK. Los controles que agregarás en el diseñador se incluyen como paquetes de Nuget en el Kit de herramientas de Comunidad Windows.

Estos nuevos controles tienen limitaciones por lo que, antes de usarlos, dedica un momento a revisar qué no se admite aún y qué funciona solo con soluciones alternativas.

## <a name="limitations"></a>Limitaciones

### <a name="whats-supported"></a>Lo que es compatible

En la mayor parte, todo se admite a menos que se mencione de manera explícita en la lista siguiente.

### <a name="whats-supported-only-with-workarounds"></a>Qué se admite solo con soluciones alternativas

:heavy_check_mark: el hospedaje de varios controles de bandeja de entrada dentro de varias ventanas. Tendrás que colocar cada ventana en su propio subproceso.

:heavy_check_mark: uso de ``x:Bind``con controles hospedados. Tendrás que declarar el modelo de datos en una biblioteca .NET Standard.

:heavy_check_mark: controles de terceros basados en C#. Si tienes el código fuente para un control de terceros, puedes compilar con él.

### <a name="whats-not-yet-supported"></a>Lo que no es compatible todavía

:no_entry_sign: herramientas de accesibilidad que funcionan perfectamente en toda la aplicación y los controles hospedados.

:no_entry_sign: contenido localizado en controles que agregas a las aplicaciones que no contienen un paquete de la aplicación de Windows.

:no_entry_sign: referencias a activos realizadas en XAML dentro de las aplicaciones que no contienen un paquete de la aplicación de Windows.

:no_entry_sign: controles que responden correctamente a cambios en PPP y escala.

:no_entry_sign: adición de un control **WebView** a un control de usuario personalizado, (en subproceso, fuera de subproceso o fuera de proceso).

:no_entry_sign: el efecto de Fluent [Mostrar resaltado](https://docs.microsoft.com/windows/uwp/design/style/reveal).

:no_entry_sign: entrada manuscrita en línea, @Places y @People para controles de entrada.

:no_entry_sign: asignación de teclas de aceleración.

:no_entry_sign: controles de terceros basados en C++.

:no_entry_sign: hospedaje de controles de usuario personalizados.

Los elementos de esta lista cambiarán probablemente a medida que seguimos mejorando la experiencia de llevar Fluent al escritorio.  
