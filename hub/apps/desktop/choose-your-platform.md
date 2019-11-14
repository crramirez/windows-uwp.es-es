---
Description: Cuando quieres crear una nueva aplicación de escritorio, la primera decisión que tomas es si emplear Win32 API y COM API o .NET.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Elección de la plataforma de aplicaciones
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 57f3101b1134b4fdceb6a38f392e54677d2b884d
ms.sourcegitcommit: f3dd633a3149d2e206981fa52ad424d408e5508c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799580"
---
# <a name="choose-your-app-platform"></a>Elección de la plataforma de aplicaciones

Cuando quieres crear una nueva aplicación de escritorio para equipos Windows, la primera decisión que debes tomar es la plataforma de aplicaciones que vas a usar. Windows ofrece cuatro plataformas de aplicaciones principales, cada una de ellas con ventajas diferentes:

* [Plataforma universal de Windows (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [Win32](#win32)

Todas estas plataformas de aplicaciones te permiten crear aplicaciones de escritorio como Word, Excel y Photoshop, que se ejecutan en el escritorio clásico de Windows y aprovechan al máximo las características específicas de ese entorno. Sin embargo, algunas de estas plataformas disponen de características especiales que resultan más adecuadas para determinados tipos de aplicaciones:

* **UWP, WPF y Windows Forms**. Estas plataformas proporcionan entornos de ejecución administrados (Windows Runtime para UWP, y .NET para Windows Forms y WPF) que aportan muchas ventajas, especialmente en las áreas de productividad del desarrollador, una interfaz de usuario sofisticada y personalizable, y la seguridad de las aplicaciones. Como estas plataformas admiten diseñadores visuales y el marcado de la interfaz de usuario para crear rápidamente dicha interfaz, resultan especialmente adecuadas para las aplicaciones de línea de negocio.

* **Win32 API**. Win32 API (también conocida como la API de Windows) es la plataforma original para aplicaciones Windows en C y C++ nativas que requieren acceso directo a Windows y al hardware. Proporciona una experiencia de desarrollo de primera clase sin depender de un entorno de ejecución administrado como .NET y WinRT. Esto hace que Win32 API sea la plataforma preferida para las aplicaciones que necesitan el mayor nivel de rendimiento y acceso directo al hardware del sistema.

En este artículo se describen con más detalle estas plataformas, y esa información te ayudará a determinar cuál es la mejor para tu aplicación. 

> [!NOTE]
> Con independencia de la plataforma de aplicaciones que elijas, puedes usar muchas de las características de la Plataforma universal de Windows (UWP) para ofrecer una experiencia moderna en tu aplicación en Windows 10. Por ejemplo, aunque la aplicación de escritorio se haya compilado con WPF, Windows Forms o Win32 API, podrás usar muchas de las características que se introdujeron por primera vez con UWP, por ejemplo, la implementación de paquetes MSIX y los controles XAML de UWP. Para más información, consulta [Modernización de las aplicaciones de escritorio](modernize/index.md).

## <a name="uwp"></a>UWP

UWP es la plataforma más vanguardista para juegos y aplicaciones de Windows 10. Se trata de una plataforma muy personalizable que usa el marcado XAML para separar la interfaz de usuario (presentación) del código (lógica de negocios). UWP es adecuada para aplicaciones de escritorio que requieren una interfaz de usuario sofisticada, personalización de estilos y escenarios de uso intensivo de gráficos. UWP también dispone de compatibilidad integrada con el [sistema Fluent Design](/windows/uwp/design/fluent-design-system/) para la experiencia predeterminada de usuario y proporciona acceso a las [API de Windows Runtime (WinRT)](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). Gracias a la adopción de Fluent Design, UWP admite automáticamente los métodos de entrada habituales: entrada manuscrita, escritura táctil, controlador para juegos, teclado y mouse.

No solo puedes usar UWP para crear aplicaciones de escritorio para equipos Windows, sino que también es la única plataforma compatible con las aplicaciones de Xbox, HoloLens y Surface Hub. UWP es nuestra plataforma de aplicaciones más reciente y vanguardista.

Para más información sobre UWP, consulta [Introducción a las aplicaciones de Windows 10](/windows/uwp/get-started/).

## <a name="wpf"></a>WPF

WPF es la plataforma establecida para las aplicaciones para Windows administradas con acceso a todo .NET Framework y también usa el marcado XAML para separar la interfaz de usuario del código. Esta plataforma está diseñada para aplicaciones de escritorio que requieren una interfaz de usuario sofisticada, personalización de estilos y escenarios de uso intensivo de gráficos. Las aptitudes de desarrollo en WPF son similares a las de UWP, por lo que migrar aplicaciones de WPF a UWP es más fácil que migrar desde Windows Forms.

Para más información acerca de WPF, consulta [Introducción (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/).

## <a name="windows-forms"></a>Windows Forms

Windows Forms es la plataforma original para aplicaciones Windows administradas, con un modelo de interfaz de usuario ligero y acceso a la totalidad de .NET Framework. Esta plataforma es más adecuada para que los desarrolladores puedan empezar a compilar aplicaciones rápidamente, incluso para aquellos que no están familiarizados con ella. Se trata de una plataforma de desarrollo de aplicaciones rápida, basada en formularios, con una gran colección integrada de controles visuales y de arrastrar y colocar que no son visuales. Windows Forms no usa XAML, por lo que si decides más adelante llevar tu aplicación a UWP, tendrás que reescribir completamente la interfaz de usuario.

Para más información sobre Windows Forms, consulta [Introducción a Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms).

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
| **Escenarios con compatibilidad limitada** |  <ul><li>Compatibilidad con varias ventanas<sup>1</sup></li><li>Compatibilidad de la plataforma con la validación de los datos de entrada<sup>1</sup></li><li>No se admite Windows 7</li><li>Algunas API de UWP requieren versiones mínimas específicas de Windows 10</li><li>Compatibilidad completa con la plataforma e integración con el shell (por ejemplo, UWP no admite actualmente la integración de la bandeja del sistema ni el acceso completo a todos los dispositivos)</li><li>Acceso directo a todos los archivos del disco</li><li>ADO.NET</li><li>Bibliotecas de clases de bases de código existentes que usan las API compatibles con el kit para la certificación de aplicaciones que no son de .NET Standard ni de Windows</li><li>Compatibilidad con bucle invertido de red local (es decir, si la aplicación necesita comunicarse con localhost sin crear una exención de bucle invertido en el dispositivo de destino)</li><li>E/S de archivos de uso intensivo</li></ul>     |  <ul><li>Compatibilidad con valores altos de PPP<sup>2</sup></li><li>Entrada táctil<sup>2</sup></li></ul>  |  <ul><li>Compatibilidad con valores altos de PPP<sup>2</sup></li><li>Entrada táctil<sup>2</sup></li><li>Interfaz de usuario personalizable</li><li>Gráficos y experiencias de usuario enriquecidas (como la entrada táctil y las animaciones)</li><li>Abstracción enriquecida de vistas y modelos de datos</li></ul>    |   |

<sup>1</sup> Hemos anunciado públicamente características que abordarán este escenario en una versión futura de Windows 10.

<sup>2</sup> Aunque la plataforma no es compatible con la API de primera clase de este escenario, los desarrolladores pueden sortear este inconveniente mediante soluciones alternativas.

## <a name="win32"></a>Win32

El uso de Win32 API con C++ permite lograr los niveles más altos de rendimiento y eficiencia y tomar un mayor control de la plataforma de destino con código no administrado de lo que es posible en un entorno de ejecución administrado como WinRT y .NET. Sin embargo, ejercer este nivel de control sobre la ejecución de la aplicación requiere un mayor cuidado y atención para hacerlo bien, y sacrifica productividad durante el desarrollo a cambio de rendimiento en tiempo de ejecución.

Estos son algunos aspectos destacados de lo que ofrece Win32 API y C++ que te permitirán crear aplicaciones de alto rendimiento.

-   Optimizaciones de nivel de hardware, como un estrecho control sobre la asignación de recursos, la duración de los objetos, el diseño de los datos, la alineación, el empaquetado de bytes, etc.
-   Acceso a conjuntos de instrucciones orientadas al rendimiento como SSE y AVX a través de funciones intrínsecas.
-   Programación genérica eficaz y con seguridad de tipos mediante el uso de plantillas.
-   Contenedores y algoritmos eficaces y seguros.
-   DirectX, concretamente Direct3D y DirectCompute (ten en cuenta que UWP también ofrece interoperabilidad de DirectX).
-   C++ AMP.

Para más información, consulta [Introducción a las aplicaciones Windows de escritorio que utilizan Win32 API](/windows/desktop/desktop-programming) y [Tecnología de aplicaciones de escritorio](/windows/desktop/desktop-app-technologies).

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 y C++ para aplicaciones de escritorio tradicionales

Cuando escribes una aplicación de escritorio en C++, puedes elegir Win32 o MFC para la interfaz de usuario, o un host de plataformas de aplicaciones de terceros que también admita plataformas que no sean de Windows.

-   **Win32:** esta es la API de la plataforma Windows, en lenguaje C y basada en identificadores que incluye, entre otras, una funcionalidad de interfaz de usuario con ventanas, dibujos y controles. Dado que es una API de bajo nivel, escrita en lenguaje C y basada en identificadores, es una opción poco frecuente para crear aplicaciones modernas con uso intensivo de la interfaz de usuario. Sin embargo, proporciona las API básicas necesarias para interactuar con la plataforma de Windows y es una opción adecuada para aplicaciones que necesitan una interfaz de usuario sencilla o que simplemente quieren que la interfaz de usuario de Windows aparezca lo menos posible como, por ejemplo, en el caso de los juegos.
-   **Biblioteca MFC (Microsoft Foundation Class):** esta es la venerable plataforma de aplicaciones y biblioteca de interfaz de usuario que los desarrolladores de Windows llevan utilizando desde 1992. Es un contenedor fino de C++ sobre Win32 API, la API en lenguaje C basada en identificadores, que proporciona interfaces orientadas a objetos para muchas de las ventanas predefinidas, los controles comunes y otros objetos de Windows. Aunque muchas plataformas modernas de interfaz de usuario del ecosistema de .NET superan a MFC en comodidad, sigue siendo la plataforma de interfaz de usuario nativa de referencia para muchos desarrolladores de C++ que crean aplicaciones para el escritorio de Windows.
-   **Plataformas de aplicaciones de terceros:** como C++ se puede ejecutar en una amplia variedad de plataformas y no está ligado a Windows ni al entorno en tiempo de ejecución de .NET, hay terceros que han desarrollado nuevas aplicaciones y plataformas de interfaz de usuario para C++ con el fin de facilitar el desarrollo de aplicaciones multiplataforma con interfaces de usuario enriquecidas. Algunas de estas plataformas ofrecen una apariencia propia mientras que otras, como wxWidgets o Qt, usan o emulan el conjunto de controles nativos de la plataforma. Con estas bibliotecas, es posible compartir casi todo el código fuente de una aplicación entre las versiones de la aplicación que se ejecutan en Windows o en otras plataformas, como OSX o Linux.

## <a name="other-app-platforms"></a>Otras plataformas de aplicaciones

### <a name="progressive-web-apps-pwas"></a>Aplicaciones web progresivas (PWA)

Estas aplicaciones permiten a los desarrolladores empaquetar su código de sitio web para poderlo instalar y ejecutar como una aplicación en equipos con Windows 10. Para más información, consulta [Aplicaciones web progresivas](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started).

### <a name="xamarin"></a>Xamarin

Usa Xamarin para crear aplicaciones multiplataforma para Windows 10 que también se puedan ejecutar en iOS y Android. Para más información, consulta [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index).
