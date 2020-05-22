---
title: Novedades de Windows 10, compilación 19041
description: Tanto la compilación 18362 de Windows 10 como las nuevas herramientas para desarrolladores proporcionan herramientas, características y experiencias con tecnología de Windows 10.
keywords: what's new, whats new, Windows, Windows 10, update, updates, features, new, newest, developers, 19041, may
ms.date: 05/12/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6e5f07e83d7e2e1b96c4bade5a2a6998c11e0559
ms.sourcegitcommit: dbb368861c85c45f34ea0d5b77eb3af2416be1b2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/13/2020
ms.locfileid: "83382807"
---
# <a name="whats-new-for-developers-in-windows-10-build-19041"></a>Novedades para desarrolladores en Windows 10, compilación 19041

Windows 10, compilación 19041 (también conocido como SDK versión 2004), en combinación con Visual Studio 2019, así como las herramientas y características relacionadas, te proporcionan todo lo que necesitas para crear aplicaciones de Windows destacadas. [Instala las herramientas y el SDK](https://developer.microsoft.com/windows/downloads#_blank) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para explorar cómo puedes usar tu [código de aplicación existente en Windows](../porting/index.md).

A continuación, ofrecemos una recopilación de características e instrucciones nuevas y mejoradas de interés para los desarrolladores de Windows en esta versión. Para obtener una lista completa de los nuevos espacios de nombres agregados a Windows SDK, consulta el artículo sobre los [cambios en las API de Windows 10, compilación 19041](windows-10-build-19041-api-diff.md). Para obtener más información sobre las características más destacadas de Windows 10, consulte [Lo más destacado de Windows 10](https://developer.microsoft.com/windows/windows-10-for-developers).

## <a name="windows-10-apps"></a>Aplicaciones de Windows 10

Característica | Descripción
:------ | :------
Reproducción de audio de Bluetooth | [Habilitar la reproducción de audio desde dispositivos conectados a Bluetooth remotos](../audio-video-camera/enable-remote-audio-playback.md) muestra cómo utilizar [AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection) para habilitar dispositivos remotos conectados a Bluetooth y reproducir audio en la máquina local, lo que permite escenarios como la configuración de un equipo para que se comporte como un altavoz de Bluetooth y que los usuarios escuchen audio desde su teléfono.
Portabilidad de aplicaciones de C# | Hemos documentado el proceso de portabilidad de una aplicación de C# a C++/WinRT. El tema [Portabilidad del ejemplo de Clipboard a C++/WinRT desde C#](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp) es contextual y se basa en una experiencia de portabilidad del mundo real en particular. Su tema complementario [Migrar a C++/WinRT desde C#](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp) es una visión más enciclopédica de los detalles técnicos y los pasos implicados en la portabilidad. 
C++/WinRT | Lee acerca de las actualizaciones de C++/WinRT con respecto a las mejoras de rendimiento en tiempo de compilación y en tiempo de ejecución (se logra en conjunto con el equipo del compilador de Visual C++), en [Paquete acumulativo de mejoras o adiciones recientes a partir de marzo de 2020](/windows/uwp/cpp-and-winrt-apis/news#rollup-of-recent-improvementsadditions-as-of-march-2020). </br> En el caso de C++/WinRT, se ha agregado más información a estos temas: [portabilidad desde C++/CX](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#boxing-and-unboxing), [portabilidad desde C#](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp#boxing-and-unboxing), [ejemplo sencillo de biblioteca de interfaz de usuario de Windows de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/simple-winui-example), [simultaneidad](/windows/uwp/cpp-and-winrt-apis/concurrency), [get_unknown()](/uwp/cpp-ref-for-winrt/get-unknown) y [controles personalizados (con plantilla) de XAML con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl).
DirectX | Hemos incorporado varios temas sobre "Novedades" actualizados relacionados con DirectX para varias versiones anteriores de Windows, desde Creators Update hasta Windows 10, versión 1903. [Novedades de DirectWrite](/windows/win32/directwrite/what-s-new-in-directwrite-for-windows-8-consumer-preview), [Mejoras de DXGI 1.6](/windows/win32/direct3ddxgi/dxgi-1-6-improvements) y [Novedades de Direct3D 12](/windows/win32/direct3d12/new-releases).
DirectXMath | Hemos publicado 21 temas nuevos de DirectXMath, que abarcan dos estructuras de matriz, así como sus funciones miembro y funciones libres. La estructura [XMFLOAT3X4](/windows/win32/api/directxmath/ns-directxmath-xmfloat3x4) es un ejemplo.
Direct3D | El [uso de DirectX con pantallas de alto rango dinámico y de color avanzado](/windows/win32/direct3darticles/high-dynamic-range) proporciona una lista de procedimientos recomendados para aplicaciones de alto rango dinámico de Windows. </br> Una nueva interfaz [ID3D11On12Device2](/windows/win32/api/d3d11on12/nn-d3d11on12-id3d11on12device2) y sus métodos le permiten tomar recursos creados mediante las API de Direct3D 11 y usarlos en Direct3D 12.
Direct3D 12 | Se ha agregado el [nivel de características de Direct3D 12 Core 1.0](/windows/win32/direct3d12/core-feature-levels) para utilizarlo en dispositivos de *solo proceso*. </br> Se han agregado nuevos temas para la [interfaz ID3D12Debug3](/windows/win32/api/d3d12sdklayers/nn-d3d12sdklayers-id3d12debug3).
Direct ML | Se han agregado 18 operadores a DirectML, la API de bajo nivel con aceleración de hardware en la que se integra WinML. Un ejemplo es la [estructura de DML_ACTIVATION_SHRINK_OPERATOR_DESC](/windows/win32/api/directml/ns-directml-dml_activation_shrink_operator_desc).
Informe de errores | Se ha agregado la función RoFailFastWithErrorContextInternal2 a Win32, que genera una excepción que puede contener más contexto de error.
Machine Learning | Windows Machine Learning [admite ahora la versión 1.4 de ONNX y el conjunto de operadores 9](/windows/ai/windows-ml/release-notes). </br>  La API [CloseModelOnSessionCreation](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodelsessionoptions.closemodelonsessioncreation?view=winrt-19041) permite ahorrar memoria al cerrar un modelo de aprendizaje automáticamente una vez que ya no se necesita.
Wi-Fi | Se han agregado varias funciones nuevas y estructuras Wi-Fi nativas, como la función [WlanDeviceServiceCommand](/windows/win32/api/wlanapi/nf-wlanapi-wlandeviceservicecommand).
Zona Wi-Fi 2 | En [Aprovisionamiento de un perfil de Wi-Fi mediante un sitio web](/windows/win32/nativewifi/prov-wifi-profile-via-website) se describe la nueva funcionalidad de la zona Wi-Fi 2.
Interoperabilidad de Windows Holographic | Se ha agregado el encabezado [`windows.graphics.holographic.interop.h`](/windows/win32/api/windows.graphics.holographic.interop), con 17 API de Win32. Las API sirven para interoperar entre Win32 y Windows Runtime. Mientras que las API se agregaron en la compilación 18362 de Windows 10, el encabezado es nuevo en la compilación 19041.
Windows Sockets | Se han realizado mejoras en el contenido de SPI de Windows Sockets 2. Un ejemplo de uno de los muchos temas que hemos mejorado y ampliado es el tema de la [función de devolución de llamada LPWSPEVENTSELECT](/windows/win32/api/ws2spi/nc-ws2spi-lpwspeventselect).
Islas XAML: conceptos básicos | Hospeda controles de XAML de UWP en tus aplicaciones de Windows de escritorio con islas XAML. Más información sobre cómo [hospedar un control estándar de UWP en una aplicación de WPF](/windows/apps/desktop/modernize/host-standard-control-with-xaml-island) y [hospedar un control estándar de UWP en una aplicación de Win32 de C++](/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands-cpp).
Islas XAML: controles personalizados | Los paquetes NuGet [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) y [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) facilitan el hospedaje de controles XAML de UWP personalizados en aplicaciones de .NET y Win32 de C++. </br> Para ver tutoriales paso a paso, consulta [Hospedaje de un control personalizado de UWP en una aplicación de WPF mediante XAML Islands](/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands) y [Hospedaje de un control personalizado de UWP en una aplicación Win32 de C++](/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands-cpp). </br> Por último, para obtener instrucciones sobre escenarios más complejos de Win32 de C++, consulta [Escenarios avanzados para islas XAML en aplicaciones Win32 de C++](/windows/apps/desktop/modernize/advanced-scenarios-xaml-islands-cpp).

## <a name="build-with-windows"></a>Compilación con Windows

Característica | Descripción
:------ | :------
Entorno de desarrollo de Windows | Los documentos del [entorno de desarrollo de Windows](/windows/dev-environment/) proporcionan recursos para usar Windows a fin de desarrollar en una gran variedad de plataformas y lograr todos los objetivos de desarrollo que te puedas plantear.
Python en Windows | En la sección [Python en Windows](/windows/python/) se proporciona información para los desarrolladores que no estén familiarizados con el lenguaje Python, así como para los desarrolladores que desean optimizar el desarrollo de Python con otras herramientas disponibles en Windows. Más información acerca de cómo configurar el entorno de Python para [desarrollo web](/windows/python/web-frameworks) e [interacción con bases de datos](/windows/python/databases).
NodeJS en Windows | La [configuración recomendada para el entorno de desarrollo de node.js](/windows/nodejs/setup-on-wsl2) proporciona instrucciones detalladas para desarrolladores avanzados que implementan en servidores Linux. También están disponibles las instrucciones de configuración de [plataformas web conocidas de node.js](/windows/nodejs/web-frameworks), [interacción con bases de datos](/windows/nodejs/databases) y [contenedores de Docker](/windows/nodejs/containers).
De Mac a Windows | Nuestra [guía para cambiar el entorno de desarrollo](/windows/dev-environment/mac-to-windows) está orientada a los usuarios que estén realizando una transición de su plataforma de desarrollo de Mac a Windows, y proporciona asignaciones para accesos directos y utilidades de desarrollo comparables.
Terminal Windows | Una [aplicación terminal moderna](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab) para usuarios de herramientas de línea de comandos y shells, como el símbolo del sistema, PowerShell y el Subsistema de Windows para Linux (WSL). Entre sus características principales se incluyen varias pestañas, paneles, compatibilidad con caracteres Unicode y UTF-8, un motor de representación de texto acelerado para GPU y la capacidad de crear sus propios temas y personalizar texto, colores, fondos y enlaces de teclas de método abreviado.
WSL 2 | Ahora hay disponible una [nueva versión del Subsistema de Windows para Linux (WSL)](/windows/wsl/wsl2-about). Las características de WSL 2 han reconfigurado la arquitectura para ejecutar un kernel de Linux real en Windows, lo que aumenta el rendimiento del sistema de archivos y agrega compatibilidad completa con llamadas del sistema. Esta nueva arquitectura cambia el modo en que los archivos binarios de Linux interactúan con Windows y con el hardware del equipo, pero con la misma experiencia de usuario que en la versión de WSL anterior. Cada distribución de Linux individual se puede ejecutar como distribución de WSL1 o WSL2, se puede ejecutar en paralelo y se puede cambiar en cualquier momento. </br> [Instala WSL 2](https://docs.microsoft.com/windows/wsl/wsl2-install) para comenzar. </br> Consulta más información sobre los [cambios de experiencia de usuario entre WSL 1 y WSL 2](https://docs.microsoft.com/windows/wsl/wsl2-ux-changes). </br> Consulta [P+F sobre WSL 2](https://docs.microsoft.com/windows/wsl/wsl2-faq).

## <a name="msix-packaging-and-deployment"></a>MSIX, empaquetado e implementación

Característica | Descripción
:------ | :------
MSIX | Se han realizado actualizaciones significativas del [formato de empaquetado de MSIX](/windows/msix/overview) desde la última versión de Windows 10 SDK. 
Empaquetado con servicios | MSIX y la herramienta de empaquetado de MSIX [ahora admiten paquetes de aplicaciones que contienen servicios](/windows/msix/packaging-tool/convert-an-installer-with-services).
Scripts en paquetes de MSIX | Puedes [usar la plataforma de compatibilidad de paquetes (PSF) para ejecutar scripts en un paquete de aplicación de MSIX](/windows/msix/psf/run-scripts-with-package-support-framework), lo que permite a los profesionales de TI personalizar dinámicamente una aplicación en el entorno del usuario después de empaquetarla con MSIX.
Integridad aplicada de paquetes | Ahora puedes aplicar la integridad de paquetes en el contenido de paquetes de MSIX mediante el elemento [uap10:PackageIntegrity](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-packageintegrity) en el manifiesto de paquete. También puedes aplicar la integridad de paquetes al crear paquetes de MSIX con la herramienta de empaquetado de MSIX.
Paquetes dispersos | Puedes [conceder la identidad del paquete a las aplicaciones de escritorio no empaquetadas en un paquete de MSIX](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) mediante la creación y el registro de un *paquete disperso* con la aplicación. Esta característica permite que las aplicaciones de escritorio que todavía no pueden adoptar el empaquetado de MSIX para la implementación usen las características de extensibilidad de Windows 10 que requieren la identidad del paquete.
Aplicaciones hospedadas | Ya puedes [crear aplicaciones hospedadas](/windows/uwp/launch-resume/hosted-apps). Las aplicaciones hospedadas comparten el mismo archivo ejecutable y la misma definición que una aplicación host principal, pero parecen y se comportan como una aplicación independiente en el sistema. Las aplicaciones hospedadas son útiles para los escenarios en los que deseas que un componente (como un archivo ejecutable o un archivo de script) se comporte como una aplicación de Windows 10 independiente, pero el componente requiere un proceso de host para poder ejecutarlo. Una aplicación hospedada puede tener su propio icono de inicio e identidad, así como una profunda integración con las características de Windows 10, como tareas en segundo plano, notificaciones, iconos y destinos de recursos compartidos.

## <a name="windows-ui-library-winui"></a>Biblioteca de interfaz de usuario de Windows (WinUI)

Característica | Descripción
:------ | :------
WinUI 2.4 | [WinUI 2.4](/uwp/toolkits/winui/release-notes/winui-2.4) es la versión pública más reciente de la biblioteca de interfaz de usuario de Windows. Todas las versiones de WinUI proporcionan una gran variedad de controles de interfaz de usuario oficiales para las aplicaciones de Windows y se proporcionan como un paquete NuGet independiente de Windows SDK, por lo que funcionan en versiones anteriores de Windows 10. [Sigue estas instrucciones](/uwp/toolkits/winui) para instalar WinUI.
RadialGradientBrush | Nuevo en WinUI 2.4., un objeto [RadialGradientBrush](../design/style/brushes.md#radial-gradient-brushes) se dibuja en una elipse definida por las propiedades Center, RadiusX y RadiusY. Los colores del degradado se inician en el centro de la elipse y terminan en el radio.
ProgressRing | Nuevo en WinUI 2.4., el [control ProgressRing](../design/controls-and-patterns/progress-controls.md) se usa para las interacciones modales, de modo que el usuario se bloquea hasta que desaparece el control ProgressRing. Usa este control si una operación requiere que la mayor parte de la interacción con la aplicación se suspenda hasta que se complete la operación.
TabView | Las actualizaciones del [control TabView](../design/controls-and-patterns/tab-view.md) te proporcionan más control sobre cómo representar las pestañas. Puedes establecer el ancho de las pestañas no seleccionadas y mostrar solo un icono para ahorrar espacio de pantalla; también puedes ocultar el botón Cerrar en las pestañas no seleccionadas hasta que el usuario mantenga el puntero sobre la pestaña.
TextBox (controles) | Cuando el tema oscuro está habilitado, el color de fondo de los controles de la familia TextBox permanece oscuro de forma predeterminada en la inserción de texto. Los controles afectados son [TextBox](/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock), [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox), [Editable ComboBox](/uwp/api/windows.ui.xaml.controls.combobox) y [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox).
NavigationView | El control [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) ahora admite la navegación jerárquica e incluye los modos de presentación Left, Top y LeftCompact. El control NavigationView jerárquico resulta útil para mostrar categorías de páginas, identificar páginas con páginas secundarias relacionadas o usarse en aquellas aplicaciones que tienen páginas de estilo de concentrador que se vinculan a muchas otras páginas.
Galería de interfaz de usuario de Windows | Los ejemplos de cada una de las características de WinUI descritas están disponibles en la Galería de controles de XAML. Descárgala en [Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) o [mira el código fuente en GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery).
Versiones anteriores | Desde la versión principal anterior de Windows 10 SDK, también se han publicado [WinUI 2.3](/uwp/toolkits/winui/release-notes/winui-2.3) y [WinUI 2.2](/uwp/toolkits/winui/release-notes/winui-2.2), lo que proporciona nuevas características de interfaz de usuario para los desarrolladores de Windows.

## <a name="samples"></a>Ejemplos

Las siguientes aplicaciones de ejemplo se han actualizado para tener como destino la compilación 19041 de Windows 10.

* [Sesiones remotas (juego de preguntas)](https://github.com/microsoft/Windows-appsample-remote-system-sessions)
* [Base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
* [Lector RSS](https://github.com/Microsoft/Windows-appsample-rssreader)
* [Marble Maze](https://github.com/Microsoft/Windows-appsample-marble-maze)
* [Photo Editor](https://github.com/Microsoft/Windows-appsample-photo-editor)
* [Programador de comidas](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
* [Libro para colorear](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [Controlador de luz de tonos](https://github.com/Microsoft/Windows-appsample-huelightcontroller)
* [Laboratorio fotográfico](https://github.com/Microsoft/Windows-appsample-photo-lab)
* [Notas de la familia](https://github.com/Microsoft/Windows-appsample-familynotes)

## <a name="videos"></a>Vídeos

### <a name="windows-terminal-the-secret-to-command-line-happiness"></a>Terminal Windows: ¡el secreto de la felicidad de la línea de comandos!

Obtén más información sobre cómo personalizar Terminal Windows para el flujo de trabajo y consulta las demostraciones de sus características en acción. [Mira el vídeo](https://www.youtube.com/watch?v=2dsnwlnNBzs) y luego [lee los documentos](https://github.com/microsoft/terminal#terminal--console-overview) para más información.

### <a name="wsl2-code-faster-on-the-windows-subsystem-for-linux"></a>WSL2: codificación más rápida en el Subsistema de Windows para Linux

Más información sobre WSL2, la nueva versión del Subsistema de Windows para Linux, y los cambios que se han realizado para mejorar el rendimiento. [Mira el vídeo](https://www.youtube.com/watch?v=MrZolfGm8Zk) y luego [lee los documentos](/windows/wsl/wsl2-about) para más información.

### <a name="msix-package-desktop-apps-for-windows-10-replace-outdated-installers"></a>MSIX: aplicaciones de escritorio de paquete para Windows 10. Reemplaza los instaladores obsoletos.

Más información sobre MSIX, el formato de paquete para instalar aplicaciones de Windows, que incluye cómo empaquetar el código existente con Visual Studio, y cómo implementar y distribuir la aplicación. [Mira el vídeo](https://www.youtube.com/watch?v=yhOnClQrvBk) y luego [lee los documentos](/windows/msix/) para más información.
