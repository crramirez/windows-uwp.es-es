---
title: WinUI 3.0, versión preliminar 1 (mayo de 2020)
description: Introducción a WinUI 3.0, versión preliminar.
ms.date: 05/14/2020
ms.topic: article
ms.openlocfilehash: 9141fe7ff215f28557020c7f76dd35c3560edfe5
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580142"
---
# <a name="windows-ui-library-30-preview-1-may-2020"></a>Biblioteca de interfaz de usuario de Windows 3.0, versión preliminar 1 (mayo de 2020)

La biblioteca de interfaz de usuario de Windows (WinUI) 3.0 es una actualización importante que transforma WinUI en una plataforma completa de experiencia del usuario para todos los tipos de aplicaciones de Windows, desde Win32 a UWP.

> [!Important]
> La versión preliminar de WinUI 3.0 se ha lanzado no solo para que se pueda realizar una evaluación temprana de la misma, sino también para recopilar comentarios de la comunidad de desarrolladores. **No** debe usarse para aplicaciones de producción.
>
> **Consulta [Limitaciones y problemas conocidos de la versión preliminar 1](#preview-1-limitations-and-known-issues)** .
## <a name="new-features-in-winui-30-preview-1"></a>Características de WinUI 3.0, versión preliminar 1

- Capacidad para crear aplicaciones de escritorio con WinUI, incluido [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0) para aplicaciones Win32
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [Actualizaciones de TabView](/windows/uwp/design/controls-and-patterns/tab-view)
- Actualizaciones de temas oscuros
- Mejoras y actualizaciones para [WebView2](https://docs.microsoft.com/microsoft-edge/hosting/webview2)
  - Compatibilidad con valores altos de PPP
  - Compatibilidad con el cambio de tamaño y desplazamiento de ventanas
  - Actualizado para poder usar una versión más reciente de Edge
  - Desaparece la necesidad de hacer referencia a un paquete NuGet específico de WebView2
- SwapChainPanel
- Mejoras necesarias para la migración del código abierto

Para más información sobre las ventajas de WinUI 3.0 y la hoja de ruta de WinUI, consulta el artículo en el que se explica la [hoja de ruta de la biblioteca de interfaz de usuario de Windows](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) en GitHub.

### <a name="provide-feedback-and-suggestions"></a>Comentarios y sugerencias

Agradecemos todos los comentarios que dejéis en el [repositorio de WinUI de GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

## <a name="try-winui-30-preview-1"></a>Prueba de WinUI 3.0, versión preliminar 1

Configura tu entorno de desarrollo (para obtener instrucciones más detalladas, consulta el apartado relativo a la [configuración de entornos de desarrollo](#configure-your-dev-environment)), instala el VSIX de la versión preliminar 1 de WinUI 3.0 desde el siguiente vínculo y prueba las plantillas de proyecto de WinUI 3.0.

<table>
<tr>
<td align="center">
<a href="https://aka.ms/winui3/previewdownload"><img src="images/downloadbuttontx.png" alt="Download the WinUI 3.0 Preview 1 VSIX"/></a>
<!--
<br/>
<a href="https://aka.ms/winui3/previewdownload">Download the WinUI 3.0 Preview 1 VSIX</a>
-->
</td>
</tr>
</table>

También puedes clonar y compilar la versión preliminar 1 de WinUI 3.0 de [XAML Controls Gallery](#xaml-controls-gallery-winui-30-preview-1-branch).

### <a name="configure-your-dev-environment"></a>Configuración de entornos de desarrollo

Asegúrate de que el equipo de desarrollo tenga instalada la actualización de abril de 2018 de Windows 10 (versión 1803, compilación 17134), o cualquier versión más reciente.

Instala la versión preliminar 1 de Visual Studio 2019, versión 16.7. Puedes descargarlo de [Visual Studio Preview](https://visualstudio.microsoft.com/vs/preview).

Debes incluir las siguientes cargas de trabajo al instalar Visual Studio Preview:

- Desarrollo de Win32 .NET
- Desarrollo con la Plataforma universal de Windows

Para compilar aplicaciones de C++ también debes incluir las siguientes cargas de trabajo:

- Desarrollo de Win32 con C++
- Componente opcional *Herramientas de la Plataforma universal de Windows para C++ (v142)* para la carga de trabajo de la Plataforma universal de Windows

### <a name="visual-studio-project-templates"></a>plantillas de proyecto de Visual Studio

Para acceder tanto a la versión preliminar 1 de WinUI 3.0 como a las plantillas de proyecto, ve a **https://aka.ms/winui3/previewdownload**

Descarga la extensión de Visual Studio (`.vsix`) para agregar las plantillas de proyecto y un paquete NuGet de WinUI a Visual Studio 2019.

Para obtener instrucciones sobre cómo agregar la extensión `.vsix` a Visual Studio, consulta [Buscar y usar extensiones de Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2019#install-without-using-the-manage-extensions-dialog-box).

Después de instalar la extensión `.vsix` puedes crear un nuevo proyecto de WinUI 3.0. Para ello, busca "WinUI" y selecciona una de las plantillas de C# o C++ disponibles.

![Plantillas de Visual Studio para WinUI 3.0](images/WinUI3Templates.png)<br/>
*Ejemplo de plantillas de Visual Studio para WinUI 3.0*

### <a name="visual-studio-project-configuration"></a>Configuración de proyectos de Visual Studio

Si usas alguna de las plantillas de la versión preliminar 1 de WinUI 3.0 para crear un proyecto, en **Versión de destino** selecciona Windows 10, versión 1903 (compilación 18362), mientras que en **Versión mínima**, debes seleccionar Windows 10, versión 1803 (compilación 17134).

Para cambiar estos valores después de crear un proyecto, haz clic con el botón derecho en el proyecto, en el **Explorador de soluciones** y selecciona **Propiedades**.

### <a name="creating-a-desktop-win32-app-with-winui-30-preview-1"></a>Creación una aplicación Win32 de Escritorio con WinUI 3.0, versión preliminar 1

Consulta [Introducción a WinUI 3.0 para aplicaciones de escritorio](get-started-winui3-for-desktop.md).

Además de las limitaciones y los problemas conocidos que se describen a continuación, la compilación de una aplicación mediante la versión preliminar 1 de WinUI 3.0 es muy similar a la compilación de una aplicación para UWP con XAML y WinUI 2.x, por lo que se aplican la mayoría de las instrucciones y la documentación de las API de aplicaciones para UWP y `Windows.UI`.

## <a name="preview-1-limitations-and-known-issues"></a>Limitaciones y problemas conocidos de la versión preliminar 1

La versión preliminar 1 es simplemente eso, una versión. Los escenarios que rodean las aplicaciones Win32 de escritorio son especialmente nuevos, por lo que puedes encontrar errores, limitaciones y problemas.

Estos son algunos de los problemas conocidos que pueden aparecer en la versión preliminar 1 de WinUI 3.0. Si encuentra algún problema que no se muestra a continuación, háganoslo saber con la contribución a un problema existente o la presentación de un nuevo problema en el [repositorio de WinUI de GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

### <a name="platform-and-os-support"></a>Compatibilidad con la plataforma y el sistema operativo

La versión preliminar 1 de WinUI 3.0 es compatible con equipos en los que se ejecuta la actualización de abril de 2018 de Windows 10 (versión 1803, compilación 17134), o cualquier versión más reciente.

### <a name="developer-tools"></a>Herramientas de desarrollo

- Las aplicaciones de escritorio admiten .NET 5 y C# 8, y se deben empaquetar.
- Las aplicaciones para UWP admiten .NET Native y C# 7.3
- IntelliSense está incompleto
- Sin diseñador visual
- Sin recarga activa
- Sin árbol visual activo
- Aún no se admite el desarrollo con VS Code
- No se admiten las nuevas aplicaciones escritas en C++/CX. Sin embargo, las aplicaciones existentes seguirán funcionando (es aconsejable empezar a usar  C++/WinRT lo antes posible)
- El contenido de WinUI 3.0 solo puede estar en una ventana por proceso, o bien en una instancia de ApplicationView por aplicación
- No se admite la implementación de dispositivos de escritorio sin empaquetar
- Sin compatibilidad con ARM64

### <a name="missing-platform-features"></a>Faltan características de la plataforma

- Sin compatibilidad con Xbox
- Sin compatibilidad con HoloLens
- No se admiten elementos emergentes con ventanas
- Sin compatibilidad con entradas de lápiz
- Acrílico en el fondo
- MediaElement y MediaPlayerElement
- RenderTargetBitmap
- MapControl
- Navegación jerárquica con NavigationView
- SwapChainPanel no admite la transparencia
- En C# , se debe usar `WinRT.WeakReference<T>`, en lugar de `System.WeakReference<T>`.
- Global Reveal usa el comportamiento de reserva, que es un pincel sólido
- XAML Islands no se admite en esta versión
- Las bibliotecas del ecosistema de terceros no funcionarán completamente
- Los IME no funcionan
- No se puede llamar a los métodos del espacio de nombres Windows.UI.Text
  
## <a name="xaml-controls-gallery-winui-30-preview-1-branch"></a>Xaml Controls Gallery (rama de WinUI 3.0, versión preliminar 1)

Consulta la [rama de la versión preliminar 1 de WinUI 3.0 de Xaml Controls Gallery](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview1) para ver una aplicación de ejemplo que incluya todos los controles y características de la versión preliminar 1 de WinUI 3.0.

![Aplicación Xaml Controls Gallery de WinUI 3.0, versión preliminar 1](images/WinUI3XamlControlsGallery.png)<br/>
*Ejemplo de la aplicación Xaml Controls Gallery de WinUI 3.0, versión preliminar 1*

Para descargar el ejemplo, clona la rama **winui3preview1** mediante el siguiente comando:

> `git clone --single-branch --branch winui3preview1 https://github.com/microsoft/Xaml-Controls-Gallery.git`

Después, asegúrate de cambiar a la rama **winui3preview1** en el entorno de Git local:

> `git checkout winui3preview1`
