---
title: Biblioteca de interfaz de usuario de Windows (WinUI)
description: Bibliotecas de WinUI para el desarrollo de aplicaciones de Windows.
ms.topic: article
ms.date: 07/15/2020
keywords: Windows 10, UWP, SDK del kit de herramientas, WinUI, biblioteca de interfaz de usuario de Windows
ms.openlocfilehash: 54b2d44dab1c311e6d1b75d0be35ed419056a953
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492980"
---
# <a name="windows-ui-library-winui"></a>Biblioteca de interfaz de usuario de Windows (WinUI)

![Logotipo de WinUI](../images/logo-winui.png)

La Biblioteca de interfaz de usuario de Windows (WinUI) es un marco de experiencia de usuario nativo (UX) para aplicaciones de escritorio de Windows y UWP.

Al incorporar el [sistema Fluent Design](https://www.microsoft.com/design/fluent/#/) a todas las experiencias, controles y estilos, WinUI proporciona experiencias coherentes, intuitivas y accesibles, ya que usa los patrones de interfaz de usuario (UI) más recientes.

Gracias a la compatibilidad con aplicaciones de escritorio y para UWP, puede realizar compilaciones con WinUI desde cero, o bien migrar gradualmente las aplicaciones MFC, WinForms o WPF existentes, así como usar lenguajes conocidos como C++, C#, Visual Basic, e incluso JavaScript (a través de [React Native para Windows](https://microsoft.github.io/react-native-windows/)).

> [!Important]
> Hay dos versiones de WinUI: **WinUI 2.x** y **WinUI 3**.

## <a name="windows-ui-2x-library"></a>Biblioteca de interfaz de usuario de Windows 2.x

WinUI 2.x se puede usar en aplicaciones para UWP y se puede incorporar a las aplicaciones de escritorio nuevas o existentes mediante [XAML Islands](/windows/apps/desktop/modernize/xaml-islands).

> [!NOTE]
> WinUI 2,4 es la versión más reciente de WinUI 2.x. En el [hito de WinUI 2.5](https://github.com/microsoft/microsoft-ui-xaml/milestone/10), encontrarás una lista del trabajo planeado para la próxima versión.

La biblioteca de WinUI 2.x está estrechamente acoplada al [SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) y proporciona controles nativos oficiales de la interfaz de usuario de Windows, así como otros elementos de la interfaz de usuario de las aplicaciones para UWP.

Al mantener la compatibilidad de nivel inferior con versiones anteriores de Windows 10, los controles de WinUI 2.x funcionan aunque los usuarios no tengan el sistema operativo más reciente.

![Compatibilidad con plataformas de WinUI 2.x](../images/platforms-winui2.png)

**Para instrucciones de instalación, consulte [Introducción a la biblioteca de interfaz de usuario de Windows](winui2/getting-started.md).**

### <a name="related-links-for-winui-2x"></a>Vínculos relacionados de WinUI 2.x

- [Introducción a la biblioteca de WinUI 2.x](winui2/index.md)
- [Documentos de API](https://docs.microsoft.com/uwp/api/overview/winui/)
- [Código fuente](https://aka.ms/winui)
- [Aplicación XAML Controls Gallery](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-3-library-preview-2"></a>Biblioteca de Windows UI 3 (versión preliminar 2)

WinUI 3 es la versión siguiente de WinUI, una plataforma nativa de la interfaz de usuario de Windows 10 que está completamente desacoplada del [SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk/).

> [!Important]
> La versión preliminar de WinUI 3 se ha lanzado no solo para que se pueda realizar una evaluación temprana de la misma, sino también para recopilar comentarios de la comunidad de desarrolladores. **No** debe usarse para aplicaciones de producción.
>
> Seguiremos enviando versiones preliminares de WinUI 3 durante 2020 y principios de 2021, después estará disponible la primera versión oficial.
>
> Use el [repositorio de GitHub de WinUI](https://github.com/microsoft/microsoft-ui-xaml) para proporcionar comentarios y registrar sugerencias y problemas.

Al desacoplar completamente las API de XAML, composición y entrada del [SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk/), el ámbito de WinUI 3 incluye la plataforma completa de la interfaz de usuario nativa de Windows 10.

WinUI es la ruta de acceso para todas las aplicaciones de Windows (se puede usar como capa de la interfaz de usuario en la aplicación nativa para UWP o Win32, o bien para modernizar gradualmente una aplicación de escritorio, pieza a pieza, con [XAML Islands](https://docs.microsoft.com/windows/apps/desktop/modernize/xaml-islands)).

Todas las nuevas características de XAML se incluirán finalmente como parte de WinUI. Las API de XAML de UWP existentes que se incluyen como parte del sistema operativo ya no recibirán nuevas actualizaciones de las características. Sin embargo, seguirán recibiendo actualizaciones de seguridad y correcciones críticas de acuerdo con el ciclo de vida de soporte técnico de Windows 10.

Tenga en cuenta que la Plataforma universal de Windows contiene algo más que tan solo el marco XAML. Se seguirán desarrollando y admitiendo características como los modelos de aplicación y de seguridad, la canalización multimedia, la integración de los shells de Xbox y Windows 10, y la compatibilidad con una amplia variedad de dispositivos.

![Compatibilidad con la plataforma de WinUI 3](../images/platforms-winui3.png)

### <a name="related-links-for-winui-3"></a>Vínculos relacionados de WinUI 3

- [Biblioteca de interfaz de usuario de Windows 3, versión preliminar 2 (julio de 2020)](winui3/index.md)
- [Aplicación XAML Controls Gallery (WinUI 3, versión preliminar 2)](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)

## <a name="winui-resources"></a>Recursos de WinUI

**GitHub**: WinUI es un proyecto de código abierto que se hospeda en GitHub. Use el [repositorio de WinUI](https://github.com/microsoft/microsoft-ui-xaml) para archivar solicitudes de características o errores, interactuar con el equipo WinUI y ver los planes del equipo tanto para WinUI 3 como para las versiones posteriores en su [mapa de ruta](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md).

**Sitio web**: El [sitio web de WinUI](https://aka.ms/winui) contiene comparaciones de productos, explica las diversas ventajas de WinUI y ayuda a mantenerse al día con el producto y el equipo del producto.
