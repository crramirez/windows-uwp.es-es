---
title: Biblioteca de interfaz de usuario de Windows (WinUI)
description: Bibliotecas de WinUI para el desarrollo de aplicaciones de Windows.
ms.topic: article
ms.date: 05/11/2020
keywords: Windows 10, UWP, SDK del kit de herramientas, WinUI, biblioteca de interfaz de usuario de Windows
ms.openlocfilehash: 01eed4a82bfe14b70c86ade1b82c487e33e6f6f6
ms.sourcegitcommit: 3a7f9f05f0127bc8e38139b219e30a8df584cad3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775890"
---
# <a name="windows-ui-library-winui"></a>Biblioteca de interfaz de usuario de Windows (WinUI)

![Imagen principal del kit de herramientas](../images/logo-winui.png)

La biblioteca de interfaz de usuario de Windows (WinUI) es la plataforma moderna de la interfaz de usuario nativa para aplicaciones de Windows en Win32 y UWP.

Al incorporar el [sistema Fluent Design](https://www.microsoft.com/design/fluent/#/) a todos los controles y estilos, WinUI proporciona experiencias coherentes, intuitivas y accesibles, ya que usa los patrones de interfaz de usuario más recientes.

Y gracias a la compatibilidad con aplicaciones Win32 y UWP, puedes realizar compilaciones con WinUI desde cero, o bien migrar las aplicaciones MFC, WinForms o WPF existentes a tu ritmo, así como usar lenguajes conocidos como C++, C#, Visual Basic, e incluso JavaScript, a través de [React Native para Windows](https://microsoft.github.io/react-native-windows/).

Hay dos versiones de WinUI que se deben tener en cuenta, **WinUI 2.x** y **WinUI 3.0**.

## <a name="windows-ui-2x-library"></a>Biblioteca de interfaz de usuario de Windows 2.x

WinUI 2.x ya se puede usar en las aplicaciones para UWP y se puede incorporar a las aplicaciones de escritorio existentes a través de [XAML Islands](/windows/apps/desktop/modernize/xaml-islands).

La biblioteca de WinUI 2.x está estrechamente acoplada al SDK de UWP y proporciona controles nativos oficiales de la interfaz de usuario de Windows, así como otros elementos de la interfaz de usuario de las aplicaciones para UWP.

Al mantener la compatibilidad de nivel inferior con versiones anteriores de Windows 10, los controles de WinUI 2.x funcionan aunque los usuarios no tengan el sistema operativo más reciente.

![Compatibilidad con plataformas de WinUI 2.x](../images/platforms-winui2.png)

> [!NOTE]
> WinUI 2,4 es la versión más reciente de WinUI 2.x. En el [hito de WinUI 2.5](https://github.com/microsoft/microsoft-ui-xaml/milestone/10), encontrarás una lista del trabajo planeado para la próxima versión.

Para obtener instrucciones de instalación, consulta [Introducción a la biblioteca de interfaz de usuario de Windows](winui2/getting-started.md).

### <a name="related-links-for-winui-2x"></a>Vínculos relacionados de WinUI 2.x

- [Introducción a la biblioteca de WinUI 2.x](winui2/index.md)
- [Documentos de API](https://docs.microsoft.com/uwp/api/overview/winui/)
- [Código fuente](https://aka.ms/winui)
- [Aplicación XAML Controls Gallery](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-30-library-preview-1"></a>Biblioteca de Windows UI 3.0 (versión preliminar 1)

WinUI 3 es la versión siguiente de WinUI, una plataforma nativa de la interfaz de usuario de Windows 10 que está completamente desacoplada del SDK de UWP.

Al desacoplar completamente las API de XAML, composición y entrada del SDK de UWP, la biblioteca de Windows UI 3.0 puede expandir considerablemente el ámbito de WinUI para que incluya la plataforma completa de la interfaz de usuario nativa de Windows 10.

WinUI sirve como ruta de acceso para todas las aplicaciones de Windows (se puede usar como capa de la interfaz de usuario en cualquier aplicación nativa de UWP o Win32, o bien para modernizar una aplicación de escritorio pieza a pieza con [Xaml Islands](https://docs.microsoft.com/windows/apps/desktop/modernize/xaml-islands)).

> [!NOTE]
> La versión preliminar 1 de WinUI 3.0 es una compilación previa a la versión de WinUI 3.0. Agradecemos todos los comentarios que dejéis en el [repositorio de WinUI de GitHub](https://github.com/microsoft/microsoft-ui-xaml).

Todas las nuevas características de XAML se incluirán como parte de WinUI. Las API de XAML de UWP existentes que se incluyen como parte del sistema operativo no recibirán nuevas actualizaciones de las características. Sin embargo, seguirán recibiendo actualizaciones de seguridad y correcciones críticas de acuerdo con el ciclo de vida de soporte técnico de Windows 10.

La Plataforma universal de Windows contiene algo más que el marco de trabajo de Xaml (por ejemplo, los modelos de seguridad y aplicación, la canalización multimedia, las integraciones de shells de Xbox y Windows 10, y la compatibilidad con gran cantidad de dispositivos) y seguirá siendo compatible.

![Compatibilidad con plataformas de WinUI 3.0.x](../images/platforms-winui3.png)

> [!Important]
> La versión preliminar 1 de WinUI 3.0 está pensada no solo para que se pueda realizar una evaluación temprana de la misma, sino también para recopilar comentarios de la comunidad de desarrolladores. **No** debe usarse para aplicaciones de producción.

### <a name="related-links-for-winui-30"></a>Vínculos relacionados de WinUI 3.0

- [WinUI 3.0, versión preliminar 1 (mayo de 2020)](winui3/index.md)
- [Aplicación XAML Controls Gallery (WinUI 3.0, versión preliminar 1)](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)

## <a name="winui-resources"></a>Recursos de WinUI

**GitHub**: WinUI es un proyecto de código abierto que se hospeda en GitHub. En el [repositorio de WinUI](https://github.com/microsoft/microsoft-ui-xaml), se pueden archivar solicitudes de características o errores, interactuar con el equipo WinUI y ver los planes del equipo tanto para WinUI 3 como para las versiones posteriores en su [mapa de ruta](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md).

**Sitio web**: el [sitio web de WinUI](https://aka.ms/winui) tiene información útil sobre las comparaciones de productos, las ventajas de WinUI y las formas de mantenerse al día con el producto y el equipo del producto.
