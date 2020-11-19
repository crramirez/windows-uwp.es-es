---
title: WinUI 3, versión preliminar 3 (noviembre de 2020)
description: Introducción a WinUI 3, versión preliminar 3.
ms.date: 11/17/2020
ms.topic: article
ms.openlocfilehash: f6c8b8730aeea12534c0e6595d220c837c0852dd
ms.sourcegitcommit: 60638c0403ff67eadda994d82c5c851bc1271bc5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94810102"
---
# <a name="windows-ui-library-3-preview-3-november-2020"></a>Biblioteca de interfaz de usuario de Windows 3, versión preliminar 3 (noviembre de 2020)

La Biblioteca de interfaz de usuario de Windows (WinUI) 3 es un marco de experiencia de usuario nativo (UX) para aplicaciones de escritorio de Windows y UWP.

**WinUI 3, versión preliminar 3** ofrece una funcionalidad nueva y mejorada, además de correcciones de errores importantes.

**Consulte [Limitaciones y problemas conocidos de la versión preliminar 3](#preview-3-limitations-and-known-issues)** .

> [!Important]
> La versión preliminar de WinUI 3 se ha lanzado no solo para que se pueda realizar una evaluación temprana de la misma, sino también para recopilar comentarios de la comunidad de desarrolladores. **No** debe usarse para aplicaciones de producción.
>
> Seguiremos enviando versiones preliminares de WinUI 3 en 2021, seguidas de la primera versión oficial.
>
> Use el [repositorio de GitHub de WinUI](https://github.com/microsoft/microsoft-ui-xaml) para proporcionar comentarios y registrar sugerencias y problemas.

## <a name="install-winui-3-preview-3"></a>Instalación de WinUI 3, versión preliminar 3

WinUI 3 versión preliminar 3 incluye plantillas de proyecto de Visual Studio para ayudarle a empezar a compilar aplicaciones con una interfaz de usuario basada en WinUI y un paquete NuGet que contiene las bibliotecas de WinUI. Para instalar WinUI 3, versión preliminar 3, siga estos pasos.

> [!NOTE]
> También puede clonar y compilar WinUI 3 versión preliminar 3 de [XAML Controls Gallery](#xaml-controls-gallery-winui-3-preview-3-branch).

1. Asegúrese de que el equipo de desarrollo tenga instalada la versión 1803 de Windows 10 (compilación 17134), o cualquier versión más reciente.

2. Instale [Visual Studio 2019, versión 16.9 versión preliminar](https://visualstudio.microsoft.com/vs/preview/).

    Debe incluir las siguientes cargas de trabajo al instalar Visual Studio:
    - Desarrollo de escritorio de .NET (también se instala .NET 5)
    - Desarrollo con la Plataforma universal de Windows

    Para compilar aplicaciones de C++, también debe incluir las siguientes cargas de trabajo:
    - Desarrollo de escritorio con C++
    - El componente opcional *Herramientas de la Plataforma universal de Windows para C++ (v142)* para la carga de trabajo de la Plataforma universal de Windows (vea "Detalles de la instalación" en la sección "Desarrollo con la Plataforma universal de Windows", en el panel derecho).
3. Asegúrese de que el sistema tiene un origen de paquete NuGet habilitado para **nuget.org**. Para obtener más información, consulte [Configuraciones comunes de NuGet](/nuget/consume-packages/configuring-nuget-behavior).[Windows Community Toolkit](#windows-community-toolkit)

4. Descargue e instale el [paquete VSIX de WinUI 3, versión preliminar 3](https://aka.ms/winui3/preview3-download). Con ello, se agregan las plantillas de proyecto WinUI 3 y el paquete NuGet que contiene las bibliotecas de WinUI 3 a Visual Studio 2019.

    Para obtener instrucciones sobre cómo agregar el paquete VSIX a Visual Studio, consulte [Buscar y usar extensiones de Visual Studio](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box).

#### <a name="webview2"></a>WebView2

Si usa el control WebView2 en la aplicación, instale la **versión del canal de desarrollo del explorador Microsoft Edge** desde [Microsoft Edge Insider Channels](https://www.microsoftedgeinsider.com/en-us/download). Asegúrese de desinstalar todas las instancias existentes de Microsoft Edge Beta, Microsoft Edge Dev y Microsoft Edge WebView2 Runtime.

#### <a name="windows-community-toolkit"></a>Kit de la comunidad de Windows

Si usa Windows Community Toolkit, [descargue la versión más reciente](https://aka.ms/wct-winui3).

## <a name="create-winui-projects"></a>Creación de proyectos WinUI

Después de instalar el paquete VSIX de WinUI 3, versión preliminar 3, está listo para crear un nuevo proyecto mediante una de las plantillas de proyecto WinUI en Visual Studio. Para tener acceso a las plantillas de proyecto WinUI, en el cuadro de diálogo **Crear un proyecto nuevo**, filtre el lenguaje en **C++** o **C#** , la plataforma en **Windows** y el tipo de proyecto en **WinUI**. Como alternativa, puede buscar *WinUI* y seleccionar una de las plantillas en C# o C++ disponibles.

![Plantillas de proyecto WinUI](images/winui-projects-csharp.png)

Para obtener más información sobre cómo empezar a trabajar con las plantillas de proyecto WinUI, consulte los siguientes artículos:

- [Introducción a WinUI 3 para aplicaciones de escritorio](get-started-winui3-for-desktop.md)
- [Introducción a WinUI 3 para aplicaciones para UWP](get-started-winui3-for-uwp.md)

Además de las [limitaciones y los problemas conocidos](#preview-3-limitations-and-known-issues), compilar una aplicación con los proyectos WinUI es similar a compilar una aplicación para UWP con XAML y WinUI 2.x. Por lo tanto, se pueden aplicar la mayor parte de la [documentación sobre las instrucciones](/windows/uwp/design/) de las aplicaciones para UWP y los espacios de nombres de WinRT de **Windows.UI** de Windows SDK.

En esta versión, también hemos agregado [documentación de referencia de API de WinUI 3](/uwp/api/overview/winui/) para todas las API de WinRT que se han trasladado a WinUI 3.

Si ha creado un proyecto con WinUI 3, versión preliminar 2, puede actualizarlo para que use la versión preliminar 3. Consulte el [repositorio de GitHub sobre WinUI](https://aka.ms/winui3/upgrade-instructions) para las instrucciones detalladas.

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
| Componente de Windows Runtime (WinUI) | C++ | Crea un [componente de Windows Runtime](/windows/uwp/winrt-components/) escrito en C++/WinRT que puede consumir cualquier aplicación para UWP o de escritorio con una interfaz de usuario basada en WinUI, independientemente del lenguaje de programación en el que esté escrita la aplicación. |
| Componente de Windows Runtime (UWP) | C# | Crea un [componente de Windows Runtime](/windows/uwp/winrt-components/) escrito en C# que puede consumir cualquier aplicación para UWP con una interfaz de usuario basada en WinUI, independientemente del lenguaje de programación en el que esté escrita la aplicación. |

### <a name="item-templates-for-winui-3"></a>Plantillas de elementos para WinUI 3

Las siguientes plantillas de elementos están disponibles para usarse en un proyecto WinUI. Para acceder a estas plantillas de elemento WinUI, haga clic con el botón secundario en el nodo del proyecto en **Explorador de soluciones**, seleccione **Agregar** -> **Nuevo elemento** y haga clic en **WinUI** en el cuadro de diálogo **Agregar nuevo elemento**.

![Plantillas de elementos de WinUI](images/winui-items-csharp.png)

| Plantilla | Language | Descripción |
|----------|----------|-------------|
| Página en blanco (WinUI) | C# y C++ | Agrega un archivo XAML y un archivo de código que define una nueva página que se deriva de la clase **Microsoft.UI.Xaml.Controls.Page** de la biblioteca de WinUI. |
| Ventana en blanco (WinUI en Escritorio) | C# y C++ | Agrega un archivo XAML y un archivo de código que define una nueva ventana derivada de la clase **Microsoft.UI.Xaml.Window** de la biblioteca de WinUI. |
| Control personalizado (WinUI) | C# y C++ | Agrega un archivo de código para crear un control con plantilla con un estilo predeterminado. El control con plantilla se deriva de la clase **Microsoft.UI.Xaml.Controls.Control** de la biblioteca de WinUI.<p></p>Para ver un tutorial que muestra cómo usar esta plantilla de elemento, vea [Controles XAML con plantilla para aplicaciones para UWP y WinUI 3 con C++ /WinRT](xaml-templated-controls-cppwinrt-winui-3.md) y [Controles XAML con plantilla para aplicaciones para UWP y WinUI 3 con C#](xaml-templated-controls-csharp-winui-3.md). Para más información sobre los controles con plantilla, vea [Controles XAML personalizados](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls). |
| Diccionario de recursos (WinUI) | C# y C++ | Agrega una colección vacía y con clave de recursos XAML. Para más información, consulte [Referencias a ResourceDictionary y a los recursos XAML](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references). |
| Archivo de recursos (WinUI) | C# y C++ | Agrega un archivo para almacenar los recursos de cadena y condicionales de la aplicación. Puede usar este elemento para ayudar a localizar la aplicación. Para más información, consulte [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de aplicación](/windows/uwp/app-resources/localize-strings-ui-manifest). |
| Control de usuario (WinUI) | C# y C++ | Agrega un archivo XAML y un archivo de código para crear un control de usuario que se deriva de la clase **Microsoft.UI.Xaml.Controls.UserControl** de la biblioteca de WinUI. Normalmente, un control de usuario encapsula controles existentes relacionados y proporciona su propia lógica.<p></p>Para más información sobre los controles de usuario, vea [Controles XAML personalizados](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls). |

### <a name="visual-studio-support"></a>Compatibilidad de Visual Studio

Con el fin de aprovechar las últimas características de las herramientas agregadas a WinUI 3 versión preliminar 3, como la recarga activa, el árbol visual dinámico y el explorador de propiedades dinámico, debe usar la versión preliminar más reciente de Visual Studio con la versión preliminar más reciente de WinUI 3. En la tabla siguiente se muestra la compatibilidad de las versiones futuras con WinUI 3 versión preliminar 3:

| Versión de VS  | WinUI 3, versión preliminar 3  |
|---|---|
| RTM 16.8  | No   |
| Versiones preliminares 16.9  | Sí  | 
| RTM 16.9  | No   |
| Versiones preliminares 16.10  | Sí   |


## <a name="new-features-and-capabilities-in-preview-3"></a>Nuevas características y funcionalidades en la versión preliminar 3

- Compatibilidad con ARM64
- Arrastrar y colocar dentro y fuera de las aplicaciones
- RenderTargetBitmap (actualmente solo contenido XAML, sin contenido SwapChainPanel)
- Mejoras en nuestra experiencia del desarrollador o herramientas:
  - Árbol visual dinámico, recarga activa, explorador de propiedades dinámico y herramientas similares
  - IntelliSense funciona ahora para WinUI 3 
- Compatibilidad con MRT Core
  - Esto hace que las aplicaciones sean más rápidas y ligeras en el inicio y proporciona una búsqueda de recursos más rápida.
- Compatibilidad con cursores personalizados
- Entrada fuera de subproceso
- Mejoras en el rendimiento con respecto a la versión preliminar 2
- Varias ventanas en aplicaciones de escritorio: compatibilidad mejorada con respecto a la versión preliminar 2


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>Nuevas características y capacidades introducidas en versiones preliminares anteriores de WinUI 3

Las siguientes características y capacidades se introdujeron en WinUI 3, versión preliminar 1 y 2, y continúan siendo compatibles con WinUI 3, versión preliminar 3.

- Capacidad para crear aplicaciones de escritorio con WinUI, incluido [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0) para aplicaciones Win32
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [Actualizaciones de TabView](/windows/uwp/design/controls-and-patterns/tab-view)
- Actualizaciones de temas oscuros
- Mejoras y actualizaciones para [WebView2](/microsoft-edge/hosting/webview2)
  - Compatibilidad con valores altos de PPP
  - Compatibilidad con el cambio de tamaño y desplazamiento de ventanas
  - Actualizado para poder usar una versión más reciente de Edge
  - Desaparece la necesidad de hacer referencia a un paquete NuGet específico de WebView2
- SwapChainPanel
- Mejoras necesarias para la migración del código abierto

Para más información sobre las ventajas de WinUI 3 y la hoja de ruta de WinUI, consulte el artículo en el que se explica la [hoja de ruta de la biblioteca de interfaz de usuario de Windows](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) en GitHub.

### <a name="provide-feedback-and-suggestions"></a>Comentarios y sugerencias

Agradecemos todos los comentarios que dejéis en el [repositorio de WinUI de GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

### <a name="whats-coming-next"></a>¿Qué novedades se esperan?

Eche un vistazo a nuestro [mapa de ruta de características](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap) para ver cuándo se incorporarán características específicas en WinUI 3. 

## <a name="preview-3-limitations-and-known-issues"></a>Limitaciones y problemas conocidos de la versión preliminar 3

La versión preliminar 3 es simplemente eso, una versión preliminar. Los escenarios que rodean las aplicaciones de escritorio son especialmente nuevos. por lo que puedes encontrar errores, limitaciones y problemas.

Estos son algunos de los problemas conocidos que pueden aparecer en WinUI 3 versión preliminar 3. Si encuentra algún problema que no se muestra a continuación, háganoslo saber colaborando en un problema existente o presentando un nuevo problema en el [repositorio de WinUI de GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

### <a name="platform-and-os-support"></a>Compatibilidad con la plataforma y el sistema operativo

WinUI 3 versión preliminar 3 es compatible con equipos en los que se ejecuta la actualización de abril de 2018 de Windows 10 (versión 1803, compilación 17134), o cualquier versión más reciente.

### <a name="developer-tools"></a>Herramientas para desarrolladores

- Solo se admiten las aplicaciones C# y C++/WinRT.
- Las aplicaciones de escritorio admiten .NET 5 y C# 9, y se deben empaquetar en una aplicación MSIX.
- Las aplicaciones para UWP admiten .NET Native y C# 7.3
- Es posible que las herramientas de desarrollo e IntelliSense no funcionen correctamente en Visual Studio versión 16.8.
- No se admiten las nuevas aplicaciones escritas en C++/CX. Sin embargo, las aplicaciones existentes seguirán funcionando (es aconsejable empezar a usar  C++/WinRT lo antes posible)
- La compatibilidad con varias ventanas en aplicaciones de escritorio está en curso, pero aún no se ha completado y no es estable.
  - Envíe un error a nuestro repositorio si encuentra nuevos problemas o regresiones en el comportamiento de varias ventanas.
- No se admite la implementación de dispositivos de escritorio sin empaquetar
- Al ejecutar una aplicación de escritorio con F5, asegúrese de que está ejecutando el proyecto de empaquetado. Si presiona F5 en el proyecto de aplicación, se ejecutará una aplicación sin empaquetar, que ya no es compatible con WinUI 3.

### <a name="missing-platform-features"></a>Faltan características de la plataforma

- Compatibilidad con Xbox
- Compatibilidad con HoloLens
- Elementos emergentes con ventanas
  - Más concretamente, la propiedad `ShouldConstrainToRootBounds` siempre actúa como si estuviera establecida en `true`, independientemente del valor de la propiedad.
- Compatibilidad con entrada manuscrita
- Acrílico
- MediaElement y MediaPlayerElement
- MapControl
- RenderTargetBitmap para el contenido SwapChainPanel y no XAML
- SwapChainPanel no admite la transparencia
- Global Reveal usa el comportamiento de reserva, que es un pincel sólido
- XAML Islands no se admite en esta versión
- Las bibliotecas del ecosistema de terceros no funcionarán completamente
- Los IME no funcionan

### <a name="known-issues"></a>Problemas conocidos

- Alt+F4 no cierra las ventanas de la aplicación de escritorio.

-   Si usa una instancia de WebView2 en la aplicación, pero no se representa ni se carga, es posible que esté ejecutando una versión incompatible del explorador Edge. [Descargue el canal Dev de Microsoft Edge](https://www.microsoftedgeinsider.com/en-us/download) y asegúrese de desinstalar todas las instancias existentes de Microsoft Edge Beta, Microsoft Edge Dev y Microsoft Edge WebView2 Runtime.

- Las funciones de serialización no deben usarse en aplicaciones WinUI de .NET 5, ya que no interoperan correctamente con C#/WinRT. Para más información, vea [esta página](https://github.com/microsoft/CsWinRT/blob/master/docs/interop.md).

- Si experimenta un bloqueo al configurar una propiedad de URI (por ejemplo, mediante un elemento `{Binding}` en el marcado XAML), puede solucionarlo mediante un elemento `{x:Bind}`, o bien mediante una versión preliminar de C#/WinRT. Para ello, agregue las líneas siguientes al archivo .csproj:

  ```xml
  <ItemGroup>
    <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" 
                        RuntimeFrameworkVersion="10.0.18362.11-preview" />
    <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" 
                        TargetingPackVersion="10.0.18362.11-preview" />
  </ItemGroup>
  ```

- En aplicaciones C# para UWP:

  El marco WinUI 3 es un conjunto de componentes de WinRT que se pueden usar de C++ (mediante C++/WinRT) o C# . Cuando se usa C#, hay dos versiones de .NET, en función del modelo de aplicación: cuando se usa WinUI 3 en una aplicación para UWP, se usa .NET Native y, cuando se usa en una aplicación de escritorio, se usa .NET 5 (y C#/WinRT).

  Cuando se usa C# para una aplicación WinUI 3 en UWP, hay algunas diferencias en el espacio de nombres de API en comparación con C# en una aplicación de escritorio WinUI 3 o C# en una aplicación WinUI 2: algunos tipos se encuentran en un espacio de nombres `Microsoft` en lugar de un espacio de nombres `System`. Por ejemplo, en lugar de que la interfaz `INotifyPropertyChanged` se encuentre en el espacio de nombres `System.ComponentModel`, está en el espacio de nombres `Microsoft.UI.Xaml.Data`. 

  Esto se aplica a lo siguiente:
    - `INotifyPropertyChanged` (y tipos relacionados)
    - `INotifyCollectionChanged`
    - `ICommand`

  Todavía existen las versiones del espacio de nombres `System`, pero no se pueden usar con WinUI 3. Esto significa que `ObservableCollection` no funciona como en C# de las aplicaciones para UWP de WinUI 3. Para obtener una solución alternativa, vea el [ejemplo CollectionsInterop](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs) del [ejemplo de XAML Controls Gallery](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview).

## <a name="xaml-controls-gallery-winui-3-preview-3-branch"></a>XAML Controls Gallery (rama de WinUI 3, versión preliminar 3)

Consulte la [rama de WinUI 3 versión preliminar 3 de XAML Controls Gallery](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview) para ver una aplicación de ejemplo que incluya todos los controles y características de WinUI 3 versión preliminar 3.

![Aplicación XAML Controls Gallery de WinUI 3, versión preliminar 3](images/WinUI3XamlControlsGalleryP3.PNG)<br/>
*Ejemplo de la aplicación XAML Controls Gallery de WinUI 3, versión preliminar 3*

Para descargar el ejemplo, clona la rama **winui3preview** mediante el siguiente comando:

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

Después, asegúrate de cambiar a la rama **winui3preview** en el entorno de Git local: 

```
git checkout winui3preview
```
