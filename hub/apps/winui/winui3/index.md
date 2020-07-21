---
title: WinUI 3, versión preliminar 2 (julio de 2020)
description: Introducción a WinUI 3, versión preliminar 2.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: 6dd29b7da0ce2d0f3a08538d392792337f1e1b5a
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493077"
---
# <a name="windows-ui-library-3-preview-2-july-2020"></a>Biblioteca de interfaz de usuario de Windows 3, versión preliminar 2 (julio de 2020)

La Biblioteca de interfaz de usuario de Windows (WinUI) 3 es un marco de experiencia de usuario nativo (UX) para aplicaciones de escritorio de Windows y UWP.

**WinUI 3, versión preliminar 2** es una versión orientada a la calidad y la estabilidad, que se centra en la corrección de errores y problemas conocidos de la versión preliminar 1.

**Consulte [Limitaciones y problemas conocidos de la versión preliminar 2](#preview-2-limitations-and-known-issues)** .

> [!Important]
> La versión preliminar de WinUI 3 se ha lanzado no solo para que se pueda realizar una evaluación temprana de la misma, sino también para recopilar comentarios de la comunidad de desarrolladores. **No** debe usarse para aplicaciones de producción.
>
> Seguiremos enviando versiones preliminares de WinUI 3 durante 2020 y principios de 2021, después estará disponible la primera versión oficial.
>
> Use el [repositorio de GitHub de WinUI](https://github.com/microsoft/microsoft-ui-xaml) para proporcionar comentarios y registrar sugerencias y problemas.

## <a name="install-winui-3-preview-2"></a>Instalación de WinUI 3, versión preliminar 2

WinUI 3 versión, preliminar 2 incluye plantillas de proyecto de Visual Studio para ayudarle a empezar a compilar aplicaciones con una interfaz de usuario basada en WinUI y un paquete NuGet que contiene las bibliotecas de WinUI. Para instalar WinUI 3, versión preliminar 2, siga estos pasos.

> [!NOTE]
> También puede clonar y compilar la versión preliminar 2 de WinUI 3 de [XAML Controls Gallery](#xaml-controls-gallery-winui-3-preview-2-branch).

1. Asegúrese de que el equipo de desarrollo tenga instalada la versión 1803 de Windows 10 (compilación 17134), o cualquier versión más reciente.

2. Instale [Visual Studio 2019, versión 16.7 versión preliminar 3](https://visualstudio.microsoft.com/vs/preview).

    Debe incluir las siguientes cargas de trabajo al instalar Visual Studio:
    - Desarrollo de escritorio de .NET
    - Desarrollo con la Plataforma universal de Windows

    Para compilar aplicaciones de C++, también debe incluir las siguientes cargas de trabajo:
    - Desarrollo de escritorio con C++
    - El componente opcional *Herramientas de la Plataforma universal de Windows para C++ (v142)* para la carga de trabajo de la Plataforma universal de Windows (vea "Detalles de la instalación" en la sección "Desarrollo con la Plataforma universal de Windows", en el panel derecho).

3. Si desea crear proyectos de escritorio WinUI para aplicaciones de C#/.NET 5 y C++/Win32, también debe instalar las versiones x64 y x86 de .NET 5, versión preliminar 5:

    - x64: [https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x64.exe](https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x64.exe)
    - x86: [https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x86.exe](https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x86.exe)

4. Descargue e instale el [paquete VSIX de WinUI 3, versión preliminar 2](https://aka.ms/winui3/previewdownload). Este paquete VSIX agrega las plantillas de proyecto WinUI 3 y el paquete NuGet que contiene las bibliotecas de WinUI 3 a Visual Studio 2019.

    Para obtener instrucciones sobre cómo agregar el paquete VSIX a Visual Studio, consulte [Buscar y usar extensiones de Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2019#install-without-using-the-manage-extensions-dialog-box).

## <a name="create-winui-projects"></a>Creación de proyectos WinUI

Después de instalar el paquete VSIX de WinUI 3, versión preliminar 2, está listo para crear un nuevo proyecto mediante una de las plantillas de proyecto WinUI en Visual Studio. Para tener acceso a las plantillas de proyecto WinUI, en el cuadro de diálogo **Crear un proyecto nuevo**, filtre el lenguaje en **C++** o **C#** , la plataforma en **Windows** y el tipo de proyecto en **WinUI**. Como alternativa, puede buscar *WinUI* y seleccionar una de las plantillas en C# o C++ disponibles.

![Plantillas de proyecto WinUI](images/winui-projects-csharp.png)

Para obtener más información sobre cómo empezar a trabajar con las plantillas de proyecto WinUI, consulte los siguientes artículos:

- [Introducción a WinUI 3 para aplicaciones de escritorio](get-started-winui3-for-desktop.md)
- [Introducción a WinUI 3 para aplicaciones para UWP](get-started-winui3-for-uwp.md)

Además de las [limitaciones y los problemas conocidos](#preview-2-limitations-and-known-issues), compilar una aplicación con los proyectos WinUI es similar a compilar una aplicación para UWP con XAML y WinUI 2.x. Por lo tanto, se pueden aplicar la mayoría de las instrucciones y documentación de las aplicaciones para UWP y los espacios de nombres de WinRT de **Windows.UI** del Windows SDK.

Si ha creado un proyecto con WinUI 3, versión preliminar 1, puede actualizarlo para que use la versión preliminar 2. Consulte las instrucciones detalladas en [nuestro repositorio de GitHub](https://aka.ms/winui3/upgrade-instructions).

### <a name="project-templates-for-winui-3"></a>Plantillas de proyecto para WinUI 3

Puede usar estas plantillas de proyecto WinUI para crear aplicaciones.

| Plantilla | Language | Descripción |
|----------|----------|-------------|
| Blank App, Packaged (WinUI in Desktop) (Aplicación vacía, empaquetada [WinUI en el escritorio]) | C# y C++ | Crea una aplicación de escritorio .NET 5 (C#) o Win32 nativo (C++) con una interfaz de usuario basada en WinUI. El proyecto generado incluye una ventana básica que se deriva de la clase **Microsoft.UI.Xaml.Window** de la biblioteca de WinUI y que puede usar para empezar a compilar la interfaz de usuario. Para más información sobre este tipo de proyecto, vea [Introducción a WinUI 3 para aplicaciones de escritorio](get-started-winui3-for-desktop.md).<p></p>La solución también incluye un [Proyecto de paquete de aplicación de Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) que está configurado para compilar la aplicación en un [paquete MSIX](/windows/msix/overview). Con ello se proporciona una experiencia de implementación moderna, la capacidad de integrarse con las características de Windows 10 a través de extensiones de paquete y mucho más.  |
| Aplicación vacía (WinUI en UWP)  | C# y C++ | Crea una aplicación para UWP con una interfaz de usuario basada en WinUI. El proyecto generado incluye una página básica que se deriva de la clase **Microsoft.UI.Xaml.Controls.Page** de la biblioteca de WinUI y que puede usar para empezar a compilar la interfaz de usuario. Para más información sobre este tipo de proyecto, vea [Introducción a WinUI 3 para aplicaciones para UWP](get-started-winui3-for-uwp.md). |

Puede usar estas plantillas de proyecto WinUI para compilar componentes que una aplicación basada en WinUI puede cargar y usar.

| Plantilla | Language | Descripción |
|----------|----------|-------------|
| Class Library (WinUI in Desktop) (Biblioteca de clases [WinUI en el escritorio]) | Solo C# | Crea una biblioteca de clases administradas (DLL) de .NET 5 en C# que otras aplicaciones de escritorio .NET 5 pueden usar con una interfaz de usuario basada en WinUI.  |
| Biblioteca de clases (WinUI en UWP)  | Solo C# | Crea una biblioteca de clases administradas (DLL) en C# que otras aplicaciones para UWP pueden usar con una interfaz de usuario basada en WinUI. |
| Componente de Windows Runtime (WinUI en UWP) | C# y C++ | Crea un [componente de Windows Runtime](/windows/uwp/winrt-components/) escrito en C# o C++/WinRT que puede consumir cualquier aplicación para UWP con una interfaz de usuario basada en WinUI, independientemente del lenguaje de programación en el que esté escrita la aplicación. |

### <a name="item-templates-for-winui-3"></a>Plantillas de elementos para WinUI 3

Las siguientes plantillas de elementos están disponibles para usarse en un proyecto WinUI. Para acceder a estas plantillas de proyecto WinUI, haga clic con el botón secundario en el nodo del proyecto en **Explorador de soluciones**, seleccione **Agregar** -> **Nuevo elemento** y haga clic en **WinUI** en el cuadro de diálogo **Agregar nuevo elemento**.

![Plantillas de elementos de WinUI](images/winui-items-csharp.png)

| Plantilla | Language | Descripción |
|----------|----------|-------------|
| Página en blanco (WinUI) | C# y C++ | Agrega un archivo XAML y un archivo de código que define una nueva página que se deriva de la clase **Microsoft.UI.Xaml.Controls.Page** de la biblioteca de WinUI. |
| Ventana en blanco (WinUI en Escritorio) | C# y C++ | Agrega un archivo XAML y un archivo de código que define una nueva ventana que se deriva de la clase **Microsoft.UI.Xaml.Window** de la biblioteca de WinUI. |
| Control personalizado (WinUI) | C# y C++ | Agrega un archivo de código para crear un control con plantilla con un estilo predeterminado. El control con plantilla se deriva de la clase **Microsoft.UI.Xaml.Controls.Control** de la biblioteca de WinUI.<p></p>Para ver un tutorial que muestra cómo usar esta plantilla de elemento, vea [Controles XAML con plantilla para aplicaciones para UWP y WinUI 3 con C++ /WinRT](xaml-templated-controls-cppwinrt-winui3.md). Para más información sobre los controles con plantilla, vea [Controles XAML personalizados](https://docs.microsoft.com/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls). |
| Diccionario de recursos (WinUI) | C# y C++ | Agrega una colección vacía y con clave de recursos XAML. Para más información, consulte [Referencias a ResourceDictionary y a los recursos XAML](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references). |
| Archivo de recursos (WinUI) | C# y C++ | Agrega un archivo para almacenar los recursos de cadena y condicionales de la aplicación. Puede usar este elemento para ayudar a localizar la aplicación. Para más información, consulte [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de aplicación](/windows/uwp/app-resources/localize-strings-ui-manifest). |
| Control de usuario (WinUI) | C# y C++ | Agrega un archivo XAML y un archivo de código para crear un control de usuario que se deriva de la clase **Microsoft.UI.Xaml.Controls.UserControl** de la biblioteca de WinUI. Normalmente, un control de usuario encapsula controles existentes relacionados y proporciona su propia lógica.<p></p>Para más información sobre los controles de usuario, vea [Controles XAML personalizados](https://docs.microsoft.com/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls). |

## <a name="bug-fixes-and-other-improvements-in-winui-3-preview-2"></a>Correcciones de errores y otras mejoras en WinUI 3, versión preliminar 2

Esta es una lista completa de correcciones de errores y otras actualizaciones para la versión preliminar 2. Consulte el [anuncio de la versión](https://aka.ms/winui3/preview2-announcement) para obtener una lista de las correcciones de errores más importantes que se han solucionado en esta versión.

- [INotifyCollectionChanged](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged?view=net-5.0) e [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=net-5.0) ahora funcionan según lo previsto en las aplicaciones de escritorio en C#.
  - Esto ha resuelto un par de otros problemas en torno a los controles de colecciones que no se actualizaban en la interfaz de usuario, aunque sí lo hacían en el back-end.
  - *Gracias a @hshristov por presentar una [incidencia similar](https://github.com/microsoft/microsoft-ui-xaml/issues/2490) en GitHub.*
- Ahora, la versión preliminar 2 es compatible con [.NET 5 versión preliminar 5](https://docs.microsoft.com/dotnet/api/?view=net-5.0) para aplicaciones de escritorio.
- WinUI 3 ahora tiene paridad con [WinUI 2.4](../winui2/release-notes/winui-2.4.md), que incluye nuevos controles y características como un control [NavigationView jerárquico](../winui2/release-notes/winui-2.4.md#hierarchical-navigation) y [ProgressRing](../winui2/release-notes/winui-2.4.md#progressring).
- Bloqueo corregido: al usar [TabView](/windows/uwp/design/controls-and-patterns/tab-view) con la función táctil.
- [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) en el [Ejemplo de XAML Controls Gallery](#xaml-controls-gallery-winui-3-preview-2-branch) ahora usa el modo izquierdo en lugar del modo compacto izquierdo.
- Bloqueo corregido: al escribir demasiado rápido en la validación de entrada y [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box).
  - *Gracias a @paulovilla por presentar [esta incidencia](https://github.com/microsoft/microsoft-ui-xaml/issues/2563) en GitHub.*
- Bloqueo corregido: al interactuar con la interfaz de usuario XAML mientras el menú [TextBox](/windows/uwp/design/controls-and-patterns/text-box) está activo.
- El texto del título del [Ejemplo de XAML Controls Gallery ](#xaml-controls-gallery-winui-3-preview-2-branch) ya no aparece codificado después de navegar a varias páginas.
- El uso de la función táctil con [WebView2](https://docs.microsoft.com/microsoft-edge/webview2/) ya no desplaza ligeramente la posición.
- Las clases de WinUIEdit.dll se han movido del espacio de nombres Windows.UI.Text al espacio de nombres Microsoft.UI.Text.
- Bloqueo corregido: al seleccionar un elemento en [TreeView](/windows/uwp/design/controls-and-patterns/tree-view) en modo de selección múltiple (en la versión 1803 de Windows 10).
- Los miembros de punto, rectángulo y tamaño ahora son de tipo doble en la proyección C# de las API para aplicaciones de escritorio.
  - *Gracias a @dotMorten por presentar [esta incidencia](https://github.com/microsoft/microsoft-ui-xaml/issues/2474) en GitHub.*
- Bloqueo corregido: al usar [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box) con un archivo .rtf.
- El botón de cierre de [TabView](/windows/uwp/design/controls-and-patterns/tab-view) ya no tiene información sobre herramientas en blanco.
- El control [Image](/windows/uwp/design/controls-and-patterns/images-imagebrushes) ahora representa correctamente los archivos SVG.
  - *Gracias a @mqudsi por presentar [esta incidencia](https://github.com/microsoft/microsoft-ui-xaml/issues/2565) en GitHub.*
- Bloqueo corregido: al usar o navegar hasta el elemento de página.
- Al usar la función táctil para seleccionar elementos de [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) ahora se anula la selección de todos los demás elementos (en modo de selección única).
- Bloqueo corregido: ya no se produce la excepción LayoutSliderException debido al valor establecido en el control [Slider](/windows/uwp/design/controls-and-patterns/slider) de tamaño específico. 
  - *Gracias a @hig-dev por presentar [esta incidencia](https://github.com/microsoft/microsoft-ui-xaml/issues/477) en GitHub.*
- Bloqueo corregido: al usar [ColorPicker](/windows/uwp/design/controls-and-patterns/color-picker) se provocaba un bloqueo al apagar.
- Bloqueo corregido: al usar [Pivot](/windows/uwp/design/controls-and-patterns/pivot) se provocaba un bloqueo al apagar.
- Bloqueo corregido: bloqueo de [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) causado porque faltaba un recurso en la versión 1803 de Windows 10.
- Bloqueo corregido: Al centrarse en el editor personalizado [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box). 
- Bloqueo corregido: [SemanticZoom](/windows/uwp/design/controls-and-patterns/semantic-zoom) 
- El enlace funciona ahora como se esperaba en la revisión, con Mode=OneWay implícito.
  - *Gracias a @tomasfabian por presentar [esta incidencia](https://github.com/microsoft/microsoft-ui-xaml/issues/2630) en GitHub.*
- Animación corregida: novedades en el [Ejemplo de XAML Controls Gallery](#xaml-controls-gallery-winui-3-preview-2-branch)

## <a name="new-features-and-capabilities-introduced-in-winui-3-preview-1"></a>Nuevas características y capacidades introducidas en WinUI 3 versión preliminar 1

Las siguientes características y capacidades se introdujeron en WinUI 3, versión preliminar 1 y continúan siendo compatibles con WinUI 3, versión preliminar 2.

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

Para más información sobre las ventajas de WinUI 3 y la hoja de ruta de WinUI, consulte el artículo en el que se explica la [hoja de ruta de la biblioteca de interfaz de usuario de Windows](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) en GitHub.

### <a name="provide-feedback-and-suggestions"></a>Comentarios y sugerencias

Agradecemos todos los comentarios que dejéis en el [repositorio de WinUI de GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

## <a name="preview-2-limitations-and-known-issues"></a>Limitaciones y problemas conocidos de la versión preliminar 2

La versión preliminar 2 es simplemente eso, una versión preliminar. Los escenarios que rodean las aplicaciones Win32 de escritorio son especialmente nuevos, por lo que puedes encontrar errores, limitaciones y problemas.

Estos son algunos de los problemas conocidos que pueden aparecer en la versión preliminar 2 de WinUI 3. Si encuentra algún problema que no se muestra a continuación, háganoslo saber con la contribución a un problema existente o la presentación de un nuevo problema en el [repositorio de WinUI de GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

### <a name="platform-and-os-support"></a>Compatibilidad con la plataforma y el sistema operativo

La versión preliminar 2 de WinUI 3 es compatible con equipos en los que se ejecuta la actualización de abril de 2018 de Windows 10 (versión 1803, compilación 17134), o cualquier versión más reciente.

### <a name="developer-tools"></a>Herramientas de desarrollo

- Por el momento solo se admiten las aplicaciones C# y C++/WinRT.
- Las aplicaciones de escritorio admiten .NET 5 y C# 8, y se deben empaquetar
- Las aplicaciones para UWP admiten .NET Native y C# 7.3
- IntelliSense está incompleto
- Sin diseñador visual
- Sin recarga activa
- Sin árbol visual activo
- Aún no se admite el desarrollo con VS Code
- No se admiten las nuevas aplicaciones escritas en C++/CX. Sin embargo, las aplicaciones existentes seguirán funcionando (es aconsejable empezar a usar  C++/WinRT lo antes posible)
- El contenido de WinUI 3 solo puede estar en una ventana por proceso, o bien en una instancia de ApplicationView por aplicación
- No se admite la implementación de dispositivos de escritorio sin empaquetar
- Sin compatibilidad con ARM64
- Controles personalizados en C# en aplicaciones para UWP: `Themes/Generic.xaml` no se genera automáticamente. Puede evitar esto creando manualmente una carpeta Temas en la clase y colocando un archivo XAML dentro de ella, denominado `Generic.xaml`.
- Después de agregar un control personalizado de WinUI al proyecto, es posible que falte el encabezado "CustomControl.h" en los archivos. Una solución alternativa para este problema puede ser agregar manualmente el encabezado en el archivo `pch.h`.
- Agregar DataGrid, otros controles de Windows Community Toolkit y controles de bibliotecas de terceros puede provocar un error en la compilación. Para solucionar este problema, agregue este diccionario combinado al archivo `App.xaml`:
  ```xaml
  <ResourceDictionary Source="ms-appx:///<library_name>/Themes/Generic.xaml"/>
  ```

### <a name="missing-platform-features"></a>Faltan características de la plataforma

- Compatibilidad con Xbox
- Compatibilidad con HoloLens
- Elementos emergentes con ventanas
- Compatibilidad con entrada manuscrita
- Acrílico en el fondo
- MediaElement y MediaPlayerElement
- RenderTargetBitmap
- MapControl
- SwapChainPanel no admite la transparencia
- Global Reveal usa el comportamiento de reserva, que es un pincel sólido
- XAML Islands no se admite en esta versión
- Las bibliotecas del ecosistema de terceros no funcionarán completamente
- Los IME no funcionan

### <a name="known-issues"></a>Problemas conocidos

- En aplicaciones de escritorio de C#:
  - Debe usar `WinRT.WeakReference<T>` en lugar de `System.WeakReference<T>` para las referencias débiles a objetos de Windows (incluidos los objetos XAML).

## <a name="xaml-controls-gallery-winui-3-preview-2-branch"></a>XAML Controls Gallery (rama de WinUI 3, versión preliminar 2)

Consulta la [rama de la versión preliminar 2 de WinUI 3 de XAML Controls Gallery](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview) para ver una aplicación de ejemplo que incluya todos los controles y características de la versión preliminar 2 de WinUI 3.

![Aplicación XAML Controls Gallery de WinUI 3, versión preliminar 2](images/WinUI3XamlControlsGalleryP2.png)<br/>
*Ejemplo de la aplicación XAML Controls Gallery de WinUI 3, versión preliminar 2*

Para descargar el ejemplo, clona la rama **winui3preview** mediante el siguiente comando:

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

Después, asegúrate de cambiar a la rama **winui3preview** en el entorno de Git local:

```
git checkout winui3preview
```
