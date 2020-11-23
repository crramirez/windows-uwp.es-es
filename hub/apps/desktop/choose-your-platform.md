---
description: Cuando quieres crear una nueva aplicación de escritorio de Windows, la primera decisión que tomas es si emplear las API Win32 y COM o .NET.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Elección de la plataforma de aplicaciones de Windows
ms.topic: article
ms.date: 11/04/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
keywords: windows win32, desarrollo de escritorio
ms.openlocfilehash: cdd21279e987f329024c53434e47777e427b95ab
ms.sourcegitcommit: b69edc6d73370923f31df61c7e42b53de6c928ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870911"
---
# <a name="choose-your-windows-app-platform"></a>Elección de la plataforma de aplicaciones de Windows

Cuando quieres crear una nueva aplicación de escritorio para equipos Windows, la primera decisión que debes tomar es la plataforma de aplicaciones que vas a usar. Windows ofrece cuatro plataformas de aplicaciones principales, cada una de ellas con ventajas diferentes:

* [Plataforma universal de Windows (UWP)](#uwp): Esta plataforma proporciona un sistema de tipos común, las API y el modelo de aplicación para todos los dispositivos que ejecutan Windows 10. Las aplicaciones para UWP pueden ser nativas o administradas.
* [WPF](#wpf) y [Windows Forms](#windows-forms): Estas plataformas basadas en .NET proporcionan un sistema de tipos común, las API y el modelo de aplicación para las aplicaciones administradas.
* [Win32](#win32): Esta es la plataforma original para aplicaciones Windows C/C++ nativas que requieren acceso directo a Windows y al hardware. Esto hace que Win32 API sea la plataforma preferida para las aplicaciones que necesitan el mayor nivel de rendimiento y acceso directo al hardware del sistema.

Todas estas plataformas incluyen un completo marco de trabajo de la interfaz de usuario, así como un conjunto de controles, que te permiten crear aplicaciones de escritorio como Word, Excel y Photoshop, que se ejecutan en el escritorio clásico de Windows y aprovechan al máximo las características específicas de ese entorno. En Windows 10, todas estas plataformas también admiten el uso de la [biblioteca de interfaz de usuario de Windows (WinUI)](#windows-ui-library) para crear sus interfaces de usuario.

Algunas de estas plataformas disponen de características especiales que resultan más adecuadas para determinados tipos de aplicaciones. Por ejemplo, tanto UWP como .NET presentan una integración profunda con Visual Studio. Esto proporciona muchas ventajas, especialmente en las áreas de productividad de desarrollo, interfaz de usuario sofisticada y personalizable, y seguridad de las aplicaciones. Como estas plataformas admiten diseñadores visuales y el marcado de la interfaz de usuario para crear rápidamente dicha interfaz, resultan especialmente adecuadas para las aplicaciones de línea de negocio.

> [!NOTE]
> Con independencia de la plataforma de aplicaciones que elijas, puedes usar muchas de las características de Windows 10 para ofrecer una experiencia moderna en tu aplicación. Por ejemplo, aunque la aplicación de escritorio se haya creado con WPF, Windows Forms o Win32 API, podrás seguir usando el desarrollo del paquete MSIX. Para más información sobre todas las formas de modernizar las aplicaciones de escritorio, consulta [Modernización de las aplicaciones de escritorio para Windows](modernize/index.md).

## <a name="uwp"></a>UWP

UWP es la plataforma más vanguardista para juegos y aplicaciones de Windows 10. Se trata de una plataforma muy personalizable que usa el marcado XAML para separar la interfaz de usuario (presentación) del código (lógica de negocios). UWP es adecuada para aplicaciones de escritorio que requieren una interfaz de usuario sofisticada, personalización de estilos y escenarios de uso intensivo de gráficos. UWP también dispone de compatibilidad integrada con el [sistema Fluent Design](/windows/uwp/design/fluent-design-system/) para la experiencia predeterminada de usuario y proporciona acceso a las [API de Windows Runtime (WinRT)](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). Gracias a la adopción de Fluent Design, UWP admite automáticamente los métodos de entrada habituales: entrada manuscrita, escritura táctil, controlador para juegos, teclado y mouse.

No solo puedes usar UWP para crear aplicaciones de escritorio para equipos Windows, sino que también es la única plataforma compatible con las aplicaciones de Xbox, HoloLens y Surface Hub. UWP es nuestra plataforma de aplicaciones más reciente y vanguardista.

Para obtener más información sobre UWP, consulta los artículos siguientes:

* [Introducción](/windows/uwp/get-started/)
* [Plantillas de proyecto](visual-studio-templates.md#uwp-templates)
* [Diseño e interfaz de usuario](/windows/uwp/design/)
* [Tecnologías y características](/windows/uwp/develop/)
* [Referencia de las API](/uwp/)
* [Ejemplos](https://github.com/Microsoft/Windows-universal-samples)

## <a name="wpf"></a>WPF

WPF es la plataforma establecida para las aplicaciones Windows administradas con acceso a .NET Core y a la instancia completa de .NET Framework, y también usa el marcado XAML para separar la interfaz de usuario del código. Esta plataforma está diseñada para aplicaciones de escritorio que requieren una interfaz de usuario sofisticada, personalización de estilos y escenarios de uso intensivo de gráficos. Las aptitudes de desarrollo en WPF son similares a las de UWP, por lo que migrar aplicaciones de WPF a UWP es más fácil que migrar desde Windows Forms.

Para obtener más información sobre WPF, consulta los artículos siguientes:

* [Introducción (WPF)](/dotnet/framework/wpf/getting-started/)
* [Plantillas de proyecto](visual-studio-templates.md#net-templates)
* [Creación de la primera aplicación (.NET Core)](/visualstudio/get-started/csharp/tutorial-wpf/)
* [Creación de la primera aplicación (.NET Framework)](/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application/)
* [Migración de aplicaciones de WPF a .NET Core](/dotnet/desktop-wpf/migration/convert-project-from-net-framework/)
* [Referencia de API (.NET)](/dotnet/api/index)
* [Ejemplos](https://github.com/Microsoft/WPF-Samples)

## <a name="windows-forms"></a>Windows Forms

Windows Forms es la plataforma original para aplicaciones Windows administradas, con un modelo de interfaz de usuario ligero y acceso a .NET Core y a la instancia completa de .NET Framework. Esta plataforma es más adecuada para que los desarrolladores puedan empezar a compilar aplicaciones rápidamente, incluso para aquellos que no están familiarizados con ella. Se trata de una plataforma de desarrollo de aplicaciones rápida, basada en formularios, con una gran colección integrada de controles visuales y de arrastrar y colocar que no son visuales. Windows Forms no usa XAML, por lo que si decides más adelante llevar tu aplicación a UWP, tendrás que reescribir completamente la interfaz de usuario.

Para obtener más información sobre Windows Forms, consulta los siguientes artículos:

* [Introducción a Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms)
* [Plantillas de proyecto](visual-studio-templates.md#net-templates)
* [Crear tu primera aplicación de Windows Forms](/dotnet/framework/winforms/creating-a-new-windows-form)
* [Tutorial: Crear un visor de imágenes](/visualstudio/ide/tutorial-1-create-a-picture-viewer?view=vs-2019)
* [Referencia de API (.NET)](/dotnet/api/index)
* [Mejorar las aplicaciones de Windows Forms](/dotnet/framework/winforms/advanced/)

## <a name="win32"></a>Win32

El uso de Win32 API con C++ permite lograr los niveles más altos de rendimiento y eficiencia y tomar un mayor control de la plataforma de destino con código no administrado de lo que es posible en un entorno de ejecución administrado como WinRT y .NET. Sin embargo, ejercer este nivel de control sobre la ejecución de la aplicación requiere un mayor cuidado y atención para hacerlo bien, y sacrifica productividad durante el desarrollo a cambio de rendimiento en tiempo de ejecución.

Estos son algunos aspectos destacados de lo que ofrece Win32 API y C++ que te permitirán crear aplicaciones de alto rendimiento.

* Optimizaciones de nivel de hardware, como un estrecho control sobre la asignación de recursos, la duración de los objetos, el diseño de los datos, la alineación, el empaquetado de bytes, etc.
* Acceso a conjuntos de instrucciones orientadas al rendimiento como SSE y AVX a través de funciones intrínsecas.
* Programación genérica eficaz y con seguridad de tipos mediante el uso de plantillas.
* Contenedores y algoritmos eficaces y seguros.
* DirectX, concretamente Direct3D y DirectCompute (ten en cuenta que UWP también ofrece interoperabilidad de DirectX).

Para obtener más información, consulta los artículos siguientes:

* [Introducción](/windows/win32/desktop-programming/)
* [Plantillas de proyecto](visual-studio-templates.md#cwin32-templates)
* [Creación de su primera aplicación C++ y Win32](/windows/win32/learnwin32/learn-to-program-for-windows/)
* [Tecnologías y características](/windows/win32/desktop-app-technologies)
* [Referencia de las API](/windows/win32/apiindex/windows-api-list/)
* [Ejemplos](https://github.com/Microsoft/Windows-classic-samples)

## <a name="windows-ui-library"></a>Biblioteca de interfaz de usuario de Windows

En Windows 10, todas las plataformas de escritorio principales también admiten el uso de la [biblioteca de interfaz de usuario de Windows (WinUI)](../winui/index.md) para crear sus interfaces de usuario. WinUI comenzó como un kit de herramientas que ofrecía versiones nuevas y actualizadas de controles de UWP para aplicaciones para UWP destinadas a versiones de nivel inferior de Windows 10. WinUI ha crecido en el ámbito y ahora es la plataforma de interfaz de usuario nativa (IU) moderna para aplicaciones de Windows 10 en UWP, .NET y Win32.

Puedes usar WinUI de las siguientes maneras en las aplicaciones de escritorio:

* Las aplicaciones para UWP pueden usar controles WinUI en lugar de los controles de UWP proporcionados por Windows SDK.
* Puedes actualizar las aplicaciones de WPF, Windows Forms y C++ o Win32 existentes para usar [islas de XAML](modernize/xaml-islands.md) con objetivo de hospedar los controles de WinUI 2.x en las aplicaciones.
* A partir de [WinUi 3.0](../winui/winui3/index.md), puede crear [aplicaciones .NET y C++ o Win32 que usan una interfaz de usuario totalmente basada en WinUi](../winui/winui3/get-started-winui3-for-desktop.md).

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>Comparación de las plataformas: UWP, WPF y Windows Forms

En la tabla siguiente se comparan las distintas características de Windows Forms, WPF y UWP en detalle.

| Característica o escenario  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **Versiones admitidas**      |  Windows 10   |  Windows 7 y versiones posteriores |  Windows 7 y versiones posteriores  |
| **Lenguajes**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI (Extensiones administradas para C++), F\#, VB |  C\#, C++/CLI (Extensiones administradas para C++), F\#, VB   |
| **Entorno de ejecución de la interfaz de usuario** |    Nativo (C++/WinRT y C++/CX) y administrado (.NET Native)  |  Administrado (.NET Framework y .NET Core 3) |   Administrado (.NET Framework y .NET Core 3)   |
| **Código abierto** | [Sí (solo para la biblioteca de la interfaz de usuario de Windows)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [Sí (solo para .NET Core)](https://github.com/dotnet/wpf) | [Sí (solo para .NET Core)](https://github.com/dotnet/winforms)  |
| **Admite XAML** |   Sí   |  Sí  |   No   |
| **Ventajas**  |  <ul><li>Marcado XAML para la interfaz de usuario</li><li>Experiencia de usuario enriquecida y personalizable</li><li>Las bases de código existentes son compatibles con .NET Standard</li><li>Compatibilidad con valores altos de PPP</li><li>Compatibilidad con varios tipos de entrada en dispositivos Windows (entrada táctil, lápiz, mando para juegos, mouse y teclado)</li><li>Compatibilidad con Xbox, HoloLens, IoT o Surface Hub</li><li>Interfaz de usuario densa (compacta) opcional</li><li>Compatibilidad con C++ nativo</li><li>Optimización de la duración de la batería</li><li>Compatibilidad con dispositivos modernos de accesibilidad (como lectores de pantalla)</li><li>Funcionalidades de datos de texto enriquecido (como la revisión ortográfica integrada)</li><li>Compatibilidad con entrada manuscrita</li><li>Ejecución segura mediante contenedores de aplicaciones (por ejemplo, el contenido que no es de confianza está en un espacio aislado)</li></ul>  |  <ul><li>Marcado XAML para la interfaz de usuario</li><li>Experiencia de usuario enriquecida y personalizable</li><li>Gran colección de controles de Microsoft y asociados</li><li>Interfaz de usuario densa</li><li>Compatibilidad con Windows 7</li><li>Compatibilidad de la plataforma con la validación de entrada</li></ul> | <ul><li>Desarrollo rápido de aplicaciones</li><li>Editor WYSIWYG para compilar la interfaz de usuario</li><li>Gran colección de controles de Microsoft y asociados</li><li>Interfaz de usuario densa</li><li>Compatibilidad con Windows 7</li><li>Entrada mediante teclado y mouse</li></ul>          |
| **Escenarios con compatibilidad limitada** |  <ul><li>Compatibilidad con varias ventanas<sup>1</sup></li><li>Compatibilidad de la plataforma con la validación de los datos de entrada<sup>1</sup></li><li>No se admite Windows 7</li><li>Algunas API de Windows Runtime requieren versiones mínimas específicas de Windows 10</li><li>Compatibilidad completa con la plataforma e integración con el shell (por ejemplo, UWP no admite actualmente la integración de la bandeja del sistema ni el acceso completo a todos los dispositivos)</li><li>Acceso directo a todos los archivos del disco</li><li>ADO.NET</li><li>Bibliotecas de clases de bases de código existentes que usan las API compatibles con el kit para la certificación de aplicaciones que no son de .NET Standard ni de Windows</li><li>Compatibilidad con bucle invertido de red local (es decir, si la aplicación necesita comunicarse con localhost sin crear una exención de bucle invertido en el dispositivo de destino)</li><li>E/S de archivos de uso intensivo</li></ul>     |  <ul><li>Compatibilidad con valores altos de PPP<sup>2</sup></li><li>Entrada táctil<sup>2</sup></li></ul>  |  <ul><li>Compatibilidad con valores altos de PPP<sup>2</sup></li><li>Entrada táctil<sup>2</sup></li><li>Interfaz de usuario personalizable</li><li>Gráficos y experiencias de usuario enriquecidas (como la entrada táctil y las animaciones)</li><li>Abstracción enriquecida de vistas y modelos de datos</li></ul>    |   |

<sup>1</sup> Hemos anunciado públicamente características que abordarán este escenario en una versión futura de Windows 10.

<sup>2</sup> Aunque la plataforma no es compatible con la API de primera clase de este escenario, los desarrolladores pueden sortear este inconveniente mediante soluciones alternativas.

## <a name="other-app-platforms"></a>Otras plataformas de aplicaciones

### <a name="progressive-web-apps-pwas"></a>Aplicaciones web progresivas (PWA)

Estas aplicaciones permiten a los desarrolladores empaquetar su código de sitio web para poderlo instalar y ejecutar como una aplicación en equipos con Windows 10. Para más información, consulta [Aplicaciones web progresivas](/microsoft-edge/progressive-web-apps/get-started).

### <a name="xamarin"></a>Xamarin

Usa Xamarin para crear aplicaciones multiplataforma para Windows 10 que también se puedan ejecutar en iOS y Android. Para más información, consulta [Xamarin](/xamarin/xamarin-forms/get-started/index).

### <a name="uno-platform"></a>Plataforma Uno

La plataforma Uno permite que el código basado en UWP de Windows (C# y XAML) se ejecute en iOS, Android, macOS, Linux y WebAssembly. Proporciona definiciones de API completas para UWP en [Windows 10 2004 (19041)](/windows/uwp/whats-new/windows-10-build-19041) y la implementación de partes de la API de UWP, como [Windows.UI.Xaml](/uwp/api/windows.ui.xaml.documents?view=winrt-19041), para permitir que las aplicaciones para UWP se ejecuten en estas plataformas. Para obtener más información, consulte los [documentos de la plataforma Uno](https://platform.uno/docs/articles/intro.html).
