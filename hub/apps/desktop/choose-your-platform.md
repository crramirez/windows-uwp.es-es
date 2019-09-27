---
Description: Si desea crear una nueva aplicación de escritorio, la primera decisión que tome es si va a usar la API de Win32 y COM o .NET.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Elección de la plataforma de aplicaciones
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: ff32e20c42f613bd9ac3dba9eada2cced0baa64c
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71316993"
---
# <a name="choose-your-app-platform"></a>Elección de la plataforma de aplicaciones

Si desea crear una nueva aplicación de escritorio para equipos Windows, la primera decisión que tome es la plataforma de aplicaciones que se va a usar. Windows proporciona cuatro plataformas de aplicación principales, cada una con diferentes puntos fuertes:

* [Plataforma universal de Windows (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [32](#win32)

Todas estas plataformas de aplicaciones permiten crear aplicaciones de escritorio como Word, Excel y Photoshop que se ejecutan en el escritorio clásico de Windows y aprovechan al máximo las características específicas de ese entorno. Sin embargo, algunas de estas plataformas comparten algunos rasgos y son más adecuadas para determinados tipos de aplicaciones:

* **UWP, WPF y Windows Forms**. Estas plataformas proporcionan entornos en tiempo de ejecución administrados (el Windows Runtime para UWP y .NET para Windows Forms y WPF) con muchas ventajas, especialmente en las áreas de productividad del desarrollador, la interfaz de usuario sofisticada y personalizable, y la seguridad de las aplicaciones. Dado que estos marcos admiten diseñadores visuales y el marcado de la interfaz de usuario para crear rápidamente la interfaz de usuario, son especialmente adecuados para las aplicaciones de línea de negocio.

* **API Win32**. La API de Win32 (también denominada API de Windows) es la plataforma original para las aplicacionesC++ nativas de C/Windows que requieren acceso directo a Windows y hardware. Proporciona una experiencia de desarrollo de primera clase sin depender de un entorno en tiempo de ejecución administrado como .NET y WinRT. Esto hace que la API Win32 sea la plataforma preferida para las aplicaciones que necesitan el mayor nivel de rendimiento y acceso directo al hardware del sistema.

En este artículo se describen con más detalle estas plataformas y ayuda a determinar cuál es la mejor para su aplicación. 

> [!NOTE]
> Con independencia de la plataforma de aplicaciones que elija, puede usar muchas de las características del Plataforma universal de Windows (UWP) para ofrecer una experiencia moderna en la aplicación en Windows 10. Por ejemplo, incluso si la aplicación de escritorio se compila con WPF, Windows Forms o la API de Win32, todavía puede usar muchas características introducidas por primera vez con UWP, como la implementación de paquetes MSIX y los controles XAML de UWP. Para obtener más información, consulte [modernización de las aplicaciones de escritorio](modernize/index.md).

## <a name="uwp"></a>UWP

UWP es la plataforma de vanguardia para aplicaciones y juegos de Windows 10. Se trata de una plataforma muy personalizable que usa el marcado XAML para separar la experiencia de usuario (presentación) del código (lógica de negocios). UWP es adecuado para las aplicaciones de escritorio que requieren una interfaz de usuario sofisticada, personalización de estilos y escenarios de uso intensivo de gráficos. UWP también tiene compatibilidad integrada con el sistema de [diseño fluida](/windows/uwp/design/fluent-design-system/) para la experiencia predeterminada de la experiencia de usuario y proporciona acceso a las [API de Windows Runtime (WinRT)](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). Mediante la adopción de Fluent, UWP admite automáticamente métodos de entrada comunes, como la tinta, la entrada táctil, el controlador de juegos, el teclado y el mouse.

No solo puede usar UWP para crear aplicaciones de escritorio para equipos con Windows, pero UWP es también la única plataforma compatible con las aplicaciones Xbox, HoloLens y Surface Hub. UWP es nuestra plataforma de aplicaciones más reciente y vanguardista.

Para obtener más información sobre UWP, consulte Introducción [a las aplicaciones de Windows 10](/windows/uwp/get-started/).

## <a name="wpf"></a>WPF

WPF es la plataforma establecida para las aplicaciones Windows administradas con acceso a la .NET Framework completa y también usa el marcado XAML para separar la experiencia de usuario del código. Esta plataforma está diseñada para aplicaciones de escritorio que requieren una interfaz de usuario sofisticada, personalización de estilos y escenarios de uso intensivo de gráficos. Los conocimientos de desarrollo de WPF son similares a los conocimientos de desarrollo de UWP, por lo que la migración desde WPF a aplicaciones UWP es más fácil que la migración desde Windows Forms.

Para obtener más información sobre WPF, vea [Introducción (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/).

## <a name="windows-forms"></a>Windows Forms

Windows Forms es la plataforma original para las aplicaciones Windows administradas con un modelo de interfaz de usuario ligera y el acceso a la .NET Framework completa. Excel permite a los desarrolladores empezar a trabajar rápidamente con la creación de aplicaciones, incluso para los desarrolladores nuevos en la plataforma. Se trata de una plataforma de desarrollo de aplicaciones rápida basada en formularios con una gran colección integrada de controles visuales y de arrastrar y colocar que no son visuales. Windows Forms no usa XAML, por lo que decidir más adelante para ampliar su aplicación a UWP conlleva una reescritura completa de la interfaz de usuario.

Para obtener más información sobre Windows Forms, consulte [Introducción a Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms).

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>Comparación de la plataforma: UWP, WPF y Windows Forms

En la tabla siguiente se comparan las distintas características de Windows Forms, WPF y UWP en detalle.

| Característica o escenario  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **Versiones compatibles**      |  Windows 10   |  Windows 7 y versiones posteriores |  Windows 7 y versiones posteriores  |
| **Idiomas**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI (extensiones administradas C++para),\#F, VB |  C\#, C++/CLI (extensiones administradas C++para),\#F, VB   |
| **Tiempo de ejecución de IU** |    Nativo (C++/WinRT y C++/CX) y administrado (.net Native)  |  Administrado (.NET Framework y .NET Core 3) |   Administrado (.NET Framework y .NET Core 3)   |
| **Código abierto** | [Sí (solo biblioteca de la interfaz de usuario de Windows)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [Sí (solo .NET Core)](https://github.com/dotnet/wpf) | [Sí (solo .NET Core)](https://github.com/dotnet/winforms)  |
| **Admite XAML** |   Sí   |  Sí  |   No   |
| **Intensidad**  |  <ul><li>Marcado XAML para la interfaz de usuario</li><li>Experiencia de usuario enriquecida y personalizable</li><li>Las bases de código existentes son compatibles con .NET Standard</li><li>Compatibilidad con PPP alta</li><li>Compatibilidad con varios tipos de entrada en todos los dispositivos de Windows (incluidos Touch, Pen, controlador de juegos, mouse y teclado)</li><li>Compatibilidad con Xbox, HoloLens, IoT o Surface Hub</li><li>Compatibilidad con nativoC++</li><li>Duración de la batería optimizada</li><li>Compatibilidad moderna con la accesibilidad (como lectores de pantalla)</li><li>Capacidades de datos de texto enriquecido (como la revisión ortográfica integrada)</li><li>Compatibilidad con la entrada manuscrita</li><li>La ejecución segura a través de contenedores de aplicaciones (por ejemplo, el contenido que no es de confianza está en espacio aislado)</li></ul>  |  <ul><li>Marcado XAML para la interfaz de usuario</li><li>Experiencia de usuario enriquecida y personalizable</li><li>Gran colección de controles de Microsoft y asociados</li><li>Interfaz de usuario densa</li><li>Compatibilidad con Windows 7</li><li>Compatibilidad de plataforma para la validación de entrada</li></ul> | <ul><li>Desarrollo rápido de aplicaciones</li><li>Editor WYSIWYG para compilar la interfaz de usuario</li><li>Gran colección de controles de Microsoft y asociados</li><li>Interfaz de usuario densa</li><li>Compatibilidad con Windows 7</li><li>Entrada de mouse y teclado</li></ul>          |
| **Escenarios con compatibilidad limitada** |  <ul><li>Interfaz de usuario densa (la creación de una interfaz de usuario densa requiere estilos personalizados)<sup>1</sup></li><li>Compatibilidad con varias ventanas<sup>1</sup></li><li>Compatibilidad de plataforma para la validación de entrada<sup>1</sup></li><li>No se admite Windows 7</li><li>Algunas API de UWP requieren versiones mínimas específicas de Windows 10</li><li>Compatibilidad completa con la plataforma e integración con el Shell (por ejemplo, UWP no admite actualmente la integración de la bandeja del sistema o el acceso completo a todos los dispositivos)</li><li>Acceso directo a todos los archivos del disco</li><li>ADO.NET</li><li>Bibliotecas de clases base de código existentes que usan las API compatibles con el kit de certificación de aplicaciones estándar de non-.NET o que no son de Windows</li><li>Compatibilidad con bucle invertido de la red local (es decir, si la aplicación necesita comunicarse con localhost sin crear una exención de bucle invertido en el dispositivo de destino)</li><li>E/s de archivos intensivas</li></ul>     |  <ul><li>Compatibilidad con PPP alta<sup>2</sup></li><li>Entrada táctil<sup>2</sup></li></ul>  |  <ul><li>Compatibilidad con PPP alta<sup>2</sup></li><li>Entrada táctil<sup>2</sup></li><li>Interfaz de usuario personalizable</li><li>Gráficos y experiencias de usuario enriquecidas (como toque y animaciones)</li><li>Abstracción enriquecida de vistas y modelos de datos</li></ul>    |   |

<sup>1</sup> hemos anunciado públicamente características que abordarán este escenario en una versión futura de Windows 10.

<sup>2</sup> aunque la plataforma carece de la compatibilidad con la API de primera clase para este escenario, los desarrolladores pueden admitir este escenario con soluciones alternativas.

## <a name="win32"></a>Win32

El uso de la API C++ de Win32 con permite lograr los niveles más altos de rendimiento y eficiencia al tomar un mayor control de la plataforma de destino con código no administrado que es posible en un entorno de tiempo de ejecución administrado como WinRT y .net. Sin embargo, el ejercicio de un nivel de control sobre la ejecución de la aplicación requiere un mayor cuidado y atención para obtener el derecho y la productividad de desarrollo para el rendimiento en tiempo de ejecución.

Estos son algunos aspectos destacados de lo que ofrece la API C++ de Win32 y le permite crear aplicaciones de alto rendimiento.

-   Optimizaciones de nivel de hardware, lo que incluye un estrecho control sobre la asignación de recursos, la duración de los objetos, el diseño de los datos, la alineación, el empaquetado de bytes, etc.
-   Acceso a conjuntos de instrucciones orientadas al rendimiento como SSE y AVX a través de funciones intrínsecas.
-   Programación genérica eficaz y con seguridad de tipos mediante el uso de plantillas.
-   Contenedores y algoritmos eficaces y seguros.
-   DirectX, en concreto Direct3D y DirectCompute (tenga en cuenta que UWP también ofrece la interoperabilidad de DirectX).
-   C++AMP.

Para obtener más información, consulte Introducción [a las aplicaciones de escritorio de Windows que usan la API de Win32 y las](/windows/desktop/desktop-programming) [tecnologías de aplicaciones de escritorio](/windows/desktop/desktop-app-technologies).

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 y C++ para aplicaciones de escritorio tradicionales

Al escribir una aplicación de escritorio C++en, puede elegir Win32 o MFC para la interfaz de usuario o un host de marcos de aplicaciones de terceros que también admitan plataformas que no son de Windows.

-   **32** Se trata de la API de lenguaje C basada en identificadores de la plataforma Windows, incluida la funcionalidad de la interfaz de usuario, pero sin limitarse a ella, como ventanas, dibujos y controles de interfaz de usuario. Dado que es una API de lenguaje C de bajo nivel basada en identificadores, es una opción poco frecuente para crear aplicaciones modernas de uso intensivo de la interfaz de usuario. Sin embargo, proporciona las API básicas necesarias para interactuar con la plataforma de Windows, y es una opción adecuada para las aplicaciones que tienen requisitos de interfaz de usuario simples o que simplemente quieren que la interfaz de usuario de Windows se mantenga fuera del camino posible, por ejemplo, los juegos.
-   **MFC (biblioteca MFC):** Este es el marco de trabajo de la aplicación venerable y la biblioteca de interfaz de usuario que ha servido a los desarrolladores de Windows desde 1992. Es un contenedor fino C++ de la API Win32 de lenguaje C basado en identificadores y proporciona interfaces orientadas a objetos para muchas de las ventanas predefinidas, controles comunes y otros objetos de Windows. Aunque muchos marcos de interfaz de usuario modernos en el ecosistema de .NET superan la comodidad de MFC, sigue siendo el marco de C++ interfaz de usuario nativo que se puede elegir para muchos desarrolladores que crean aplicaciones para el escritorio de Windows.
-   **Marcos de aplicaciones de terceros:** Dado C++ que se puede ejecutar en una amplia variedad de plataformas y no está ligada a Windows o al tiempo de ejecución de .net, terceros han desarrollado nuevos marcos C++ de aplicaciones y de interfaz de usuario para facilitar el desarrollo de aplicaciones multiplataforma con interfaces de usuario enriquecidas. Algunos de estos marcos proporcionan su propia apariencia &, mientras que otros, como wxWidgets o Qt, usan o emulan el conjunto de control nativo de la plataforma. Con estas bibliotecas, es posible compartir casi todo el código fuente de una aplicación entre las versiones de la aplicación que se ejecutan en Windows o en otras plataformas, como OSX o Linux.

## <a name="other-app-platforms"></a>Otras plataformas de aplicaciones

### <a name="progressive-web-apps-pwas"></a>Web Apps progresiva (PWA)

PWA permite a los desarrolladores empaquetar su código de sitio web para que se pueda instalar y ejecutar como una aplicación en equipos con Windows 10. Para obtener más información, vea [Web Apps progresivas](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started).

### <a name="xamarin"></a>Xamarin

Use Xamarin para compilar aplicaciones multiplataforma para Windows 10 que también se pueden ejecutar en iOS y Android. Para obtener más información, consulte [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index).
