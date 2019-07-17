---
Description: Cuando desea crear una nueva aplicación de escritorio, la primera decisión que tome es si se debe usar el Win32 y API de COM o. NET.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Elección de la plataforma de aplicaciones
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 2a4de1a43e60250e7efc2faf70f3c49e8253beb3
ms.sourcegitcommit: 734aa941dc675157c07bdeba5059cb76a5626b39
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "68141798"
---
# <a name="choose-your-app-platform"></a>Elección de la plataforma de aplicaciones

Cuando desea crear una nueva aplicación de escritorio para equipos Windows, la primera decisión que tome es la plataforma de aplicación para usar. Windows proporciona cuatro plataformas de aplicación principal, cada uno con diversos puntos fuertes:

* [Plataforma universal de Windows (UWP)](#uwp)
* [WPF (. NET)](#wpf)
* [Windows Forms (. NET)](#windows-forms)
* [Win32](#win32)

Todas estas plataformas de aplicaciones le permiten crear aplicaciones de escritorio como Word, Excel y Photoshop que se ejecutan en el clásico Windows desktop y completa Aproveche características específicas de ese entorno. Sin embargo, algunas de estas plataformas comparten algunos rasgos y son más adecuados para determinados tipos de aplicaciones:

* **Windows Forms, WPF y UWP**. Estas plataformas proporcionan entornos de tiempo de ejecución administrado (el tiempo de ejecución de Windows para UWP y .NET para Windows Forms y WPF) con muchas ventajas, especialmente en las áreas de productividad del desarrollador, sofisticado y personalizable de interfaz de usuario y seguridad de la aplicación. Dado que estos marcos de trabajo admiten diseñadores visuales y el marcado de la interfaz de usuario para crear rápidamente la interfaz de usuario, son especialmente adecuadas para aplicaciones de línea de negocio.

* **API de Win32**. La API de Win32 (también denominada la API de Windows) es la plataforma original para C nativo /C++ las aplicaciones de Windows que requieren acceso directo al hardware y Windows. Proporciona una experiencia de desarrollo de primera clase sin según un entorno de tiempo de ejecución administrado, como .NET y WinRT. Esto hace que la API Win32 de la plataforma de elección para aplicaciones que necesitan el máximo nivel de rendimiento y el acceso directo al hardware del sistema.

En este artículo se describe estas plataformas con más detalle y le ayuda a determinar la mejor para su aplicación. 

> [!NOTE]
> Con independencia de qué plataforma de aplicación que elija, puede utilizar muchas características de la plataforma Universal de Windows (UWP) para ofrecer una experiencia moderna de la aplicación en Windows 10. Por ejemplo, incluso si su aplicación de escritorio se compila con la API de Win32, Windows Forms o WPF, todavía puede utilizar muchas características que se introdujo por primera vez con UWP, como la implementación del paquete MSIX y controles de UWP XAML. Para obtener más información, consulte [modernizar sus aplicaciones de escritorio](modernize/index.md).

## <a name="uwp"></a>UWP

UWP es la plataforma de vanguardia para juegos y aplicaciones de Windows 10. Es una plataforma muy personalizable que usa marcado XAML para separar la experiencia del usuario (presentación) del código (lógica empresarial). UWP es adecuada para aplicaciones de escritorio que requieren una sofisticada interfaz de usuario, la personalización de estilos y escenarios de gráficos. UWP también tiene compatibilidad integrada para la [sistema de diseño Fluent](/windows/uwp/design/fluent-design-system/) para el valor predeterminado experiencia UX y proporciona acceso a la [Windows en tiempo de ejecución (WinRT) API](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). Al adoptar Fluent, UWP admite automáticamente los métodos de entrada común, como tinta, toque, gamepad, teclado y mouse (ratón).

No solo puede usar UWP para crear aplicaciones de escritorio para equipos Windows, pero UWP también es la única plataforma admitida para las aplicaciones Xbox, HoloLens y Surface Hub. UWP es nuestra plataforma de aplicación más reciente y de vanguardia.

Para obtener más información sobre UWP, consulte [empezar a trabajar con aplicaciones de Windows 10](/windows/uwp/get-started/).

## <a name="wpf"></a>WPF

WPF es la plataforma establecida para aplicaciones administradas de Windows con acceso a la versión completa de .NET Framework, y también usa marcado XAML para separar la experiencia de usuario del código. Esta plataforma está diseñada para aplicaciones de escritorio que requieren una sofisticada interfaz de usuario, la personalización de estilos y escenarios de gráficos. Habilidades de desarrollo en WPF son similares a sus conocimientos de desarrollo de UWP para que la migración de WPF a aplicaciones para UWP sea más fácil que la migración de Windows Forms.

Para obtener más información acerca de WPF, vea [Introducción (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/).

## <a name="windows-forms"></a>Windows Forms

Windows Forms es la plataforma original para aplicaciones administradas de Windows con un modelo de interfaz de usuario ligero y acceso a la versión completa de .NET Framework. Destaca en permitir a los desarrolladores a empezar rápidamente a crear aplicaciones, incluso para los desarrolladores la plataforma. Se trata de una plataforma de desarrollo rápido de aplicaciones basada en formularios con una extensa colección integrada de los controles de arrastrar y colocar visuales y no visual. Windows Forms no utiliza XAML, por lo que decidir más tarde extender su aplicación para UWP conlleva una reescritura completa de la interfaz de usuario.

Para obtener más información acerca de los formularios de Windows, consulte [Introducción a Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms).

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>Comparación de las plataformas: UWP, WPF y Windows Forms

La siguiente tabla compara las diversas características de Windows Forms, WPF y UWP con detalle.

| Característica o escenario  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **Versiones admitidas**      |  Windows 10   |  Windows 7 y versiones posteriores |  Windows 7 y versiones posteriores  |
| **Idiomas**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI (extensiones administradas para C++), F\#, VB |  C\#, C++/CLI (extensiones administradas para C++), F\#, VB   |
| **En tiempo de ejecución de la interfaz de usuario** |    Native (C++/WinRT y C++/CX) y administrados (.NET Native)  |  Administrado (.NET Framework)<br/><br/>Próximamente se admitirá para .NET Core 3  |   Administrado (.NET Framework)<br/><br/>Próximamente se admitirá para .NET Core 3    |
| **Código abierto** | [Sí (sólo biblioteca de interfaz de usuario de Windows)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [Sí (solo .NET Core)](https://github.com/dotnet/wpf) | [Sí (solo .NET Core)](https://github.com/dotnet/winforms)  |
| **Es compatible con XAML** |   Sí   |  Sí  |   No   |
| **Puntos fuertes**  |  <ul><li>Marcado XAML para la interfaz de usuario</li><li>Experiencia de usuario enriquecido y personalizable</li><li>Las bases de código existentes son compatibles con .NET Standard</li><li>Compatibilidad con PPP elevado</li><li>Compatibilidad con varios tipos de entrada en todos los dispositivos de Windows (incluido toque, lápiz, gamepad, mouse y teclado)</li><li>Compatibilidad con Xbox, HoloLens, IoT o Surface Hub</li><li>Compatibilidad con nativoC++</li><li>Duración de la batería optimizado</li><li>Compatibilidad de accesibilidad moderno (por ejemplo, los lectores de pantalla)</li><li>Capacidades de datos de texto enriquecido (por ejemplo, revisión ortográfica integrada)</li><li>Compatibilidad de tinta</li><li>Proteger la ejecución a través de contenedores de aplicación (por ejemplo, no es de confianza contenido es un espacio aislado)</li></ul>  |  <ul><li>Marcado XAML para la interfaz de usuario</li><li>Experiencia de usuario enriquecido y personalizable</li><li>Gran colección de controles de Microsoft y asociados</li><li>Interfaz de usuario con gran densidad</li><li>Compatibilidad con Windows 7</li><li>La plataforma admite para la validación de entrada</li></ul> | <ul><li>desarrollo rápido de aplicaciones</li><li>Editor WYSIWYG para crear la interfaz de usuario</li><li>Gran colección de controles de Microsoft y asociados</li><li>Interfaz de usuario con gran densidad</li><li>Compatibilidad con Windows 7</li><li>Teclado y mouse de entrada</li></ul>          |
| **Escenarios que tienen compatibilidad limitada** |  <ul><li>Interfaz de usuario densa (crear una IU con gran densidad requiere estilos personalizados)<sup>1</sup></li><li>Compatibilidad con las múltiples ventanas<sup>1</sup></li><li>Compatibilidad con la plataforma para la validación de entrada<sup>1</sup></li><li>No se admite Windows 7</li><li>Algunas API de UWP necesitan versiones específicas de mínimas de Windows 10</li><li>Integración de shell y de soporte técnico de plataforma completa (por ejemplo, UWP actualmente no admite la integración de bandeja del sistema o acceso completo a todos los dispositivos)</li><li>Acceso directo a todos los archivos en disco</li><li>ADO.NET</li><li>Bibliotecas de clases de base de código existentes que usan no en estándar .NET o API compatibles no - Windows App Certification Kit</li><li>Soporte técnico de bucle invertido de red local (es decir, si la aplicación necesita para comunicarse con el host local sin necesidad de crear una exención de bucle invertido en el dispositivo de destino)</li><li>E/S de archivos de alto rendimiento</li></ul>     |  <ul><li>Compatibilidad con PPP elevado<sup>2</sup></li><li>Entrada táctil<sup>2</sup></li></ul>  |  <ul><li>Compatibilidad con PPP elevado<sup>2</sup></li><li>Entrada táctil<sup>2</sup></li><li>Interfaz de usuario personalizable</li><li>Experiencias de usuario y gráficos enriquecidos (por ejemplo, la función táctil y animaciones)</li><li>Abstracción enriquecido de vistas y modelos de datos</li></ul>    |   |

<sup>1</sup> públicamente hemos anunciado las características que lo solucionará este escenario en una versión futura de Windows 10.

<sup>2</sup> aunque la plataforma no tiene una compatibilidad excelente con API para este escenario, los desarrolladores pueden admitir este escenario con soluciones alternativas.

## <a name="win32"></a>Win32

Mediante la API de Win32 con C++ hace posible alcanzar los mayores niveles de rendimiento y la eficiencia tomando más control de la plataforma de destino con código no administrado que es posible en un entorno de tiempo de ejecución administrado como WinRT y. NET. Sin embargo, ejercer un nivel de este tipo de control sobre la ejecución de la aplicación requiere mayor cuidado y atención para funcionar y productividad de desarrollo para el rendimiento en tiempo de ejecución de transacciones.

Estos son algunos aspectos destacados de lo que la API de Win32 y C++ ofrece para permitirle crear aplicaciones de alto rendimiento.

-   Optimizaciones a nivel de hardware, incluido un control estricto sobre la asignación de recursos, duración de los objetos, diseño de datos, alineación, bytes de empaquetado y mucho más.
-   Acceso a la instrucción orientado al rendimiento se establece como SSE y AVX a través de funciones intrínsecas.
-   Programación genérica eficaz, seguridad de tipos mediante el uso de plantillas.
-   Algoritmos y contenedores de seguros y eficientes.
-   DirectX, en particular, Direct3D y DirectCompute (tenga en cuenta que UWP ofrece también la interoperabilidad de DirectX).
-   C++AMP.

Para obtener más información, consulte [empezar a trabajar con aplicaciones de escritorio de Windows que usan la API de Win32](/windows/desktop/desktop-programming) y [las tecnologías de la aplicación de escritorio](/windows/desktop/desktop-app-technologies).

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 y C++ para aplicaciones de escritorio tradicionales

Cuando se escribe una aplicación de escritorio de C++, puede elegir Win32 o MFC para la interfaz de usuario o un host de marcos de aplicaciones de terceros que también son compatibles con las plataformas que no sean Windows.

-   **Win32:** Se trata de la API basada en el identificador, lenguaje de C de la plataforma Windows, incluyendo pero sin limitarse a la funcionalidad de la interfaz de usuario como ventanas, dibujo y la interfaz de usuario de controles. Dado que es una API de bajo nivel, el lenguaje C basada en identificadores, es una opción poco frecuente para crear aplicaciones modernas, intensivo de la interfaz de usuario. Sin embargo, proporciona las API básicas necesarias interactuar con la plataforma de Windows y es una opción conveniente para las aplicaciones que tienen requisitos de la interfaz de usuario simple o que simplemente desee la IU de Windows para permanecer fuera de la vista lo máximo posible, por ejemplo, juegos.
-   **MFC (biblioteca Microsoft Foundation Class):** Esto es el marco de aplicación venerable y la biblioteca de interfaz de usuario que ha servido a los desarrolladores de Windows desde 1992. Es un fino C++ contenedor a través de la API de Win32 basado en identificador, lenguaje de C y proporciona interfaces orientada a objetos para muchas de las ventanas predefinidas, controles comunes y otros objetos de Windows. Aunque muchos marcos de interfaz de usuario modernas en el ecosistema .NET superan MFC en comodidad, sigue siendo el marco de interfaz de usuario nativo de elección para muchos C++ los desarrolladores que crean aplicaciones para el escritorio de Windows.
-   **Marcos de aplicaciones de terceros:** Dado que C++ puede ejecutar en una amplia variedad de plataformas y no está vinculada a Windows o el tiempo de ejecución. NET, han desarrollado para terceros nueva aplicación y marcos de interfaz de usuario para C++ para facilitar el desarrollo de aplicaciones multiplataforma con interfaces de usuario enriquecidas. Algunos de estos marcos de trabajo proporcionan sus propias aspecto y funcionamiento, mientras que otros como wxWidgets o Qt usarán o emulan el conjunto de controles nativos de la plataforma. Uso de estas bibliotecas, es posible compartir casi todo código de fuente de la aplicación entre las versiones de la aplicación que se ejecutan en Windows o en otras plataformas, como OSX o Linux.

## <a name="other-app-platforms"></a>Otras plataformas de aplicaciones

### <a name="progressive-web-apps-pwas"></a>Aplicaciones Web progresiva (PWA)

PWA permite a los desarrolladores empaquetan su sitio Web de código para que pueda instalar y ejecutar como una aplicación en equipos con Windows 10. Para obtener más información, consulte [aplicaciones Web progresiva](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started).

### <a name="xamarin"></a>Xamarin

Usar Xamarin para compilar aplicaciones multiplataforma para Windows 10 que también se puede ejecutar en iOS y Android. Para obtener más información, consulte [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index).
