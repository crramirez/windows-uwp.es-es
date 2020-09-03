---
Description: Esta sección te ayuda a comprender cómo se admiten determinadas características clave de Windows en diferentes plataformas de aplicaciones y cómo puedes empezar a usarlas en tu código.
title: Características y tecnologías
ms.topic: article
ms.date: 05/08/2019
ms.localizationpriority: medium
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: eef7ffd2e84e05bb127120ea7dfa56764782bee9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174169"
---
# <a name="features-and-technologies-for-windows-apps"></a>Características y tecnologías de las aplicaciones de Windows

Independientemente del tipo de aplicación que compiles o del dispositivo de destino, Windows admite muchas características que son bloques de creación clave para escenarios de aplicaciones importantes. Algunas de estas características se exponen a la Plataforma universal de Windows (UWP), Win32 (API de Windows) y otras plataformas de aplicaciones de maneras diferentes. Los artículos siguientes te ayudan a comprender cómo se admiten determinadas características de Windows en diferentes plataformas de aplicaciones y cómo puedes empezar a usarlas en tu código.

En este artículo se proporciona una lista personalizada de artículos para obtener más información sobre cómo puedes acceder a las características y tecnologías importantes de Windows en las plataformas de aplicaciones de Windows Forms, UWP, Win32 (API de Windows) y WPF. Para obtener información completa sobre las características de desarrollo de cada plataforma, consulta los siguientes recursos:

* [Documentación de UWP](/windows/uwp/index)
* [Documentación de Win32 (API de Windows)](/windows/desktop/index)
* [Documentación de WPF](/dotnet/framework/wpf/index)
* [Documentación de Windows Forms](/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>Características y tecnologías clave de Windows

En las secciones siguientes se resaltan varias características y tecnologías importantes de Windows que te permiten ofrecer experiencias modernas y atractivas a tus clientes.

### <a name="windows-ink"></a>Windows Ink

![Lápiz para Surface](images/hero-small.png)  

La plataforma de Windows Ink, junto con un dispositivo de lápiz, te ofrece una forma natural de crear notas, dibujos y anotaciones manuscritas, todo ello de forma digital. Asimismo, la plataforma te permite capturar los datos de entrada del digitalizador a modo de datos de entrada de lápiz, generar datos de entrada de lápiz, administrarlos, representarlos como trazos de lápiz en el dispositivo de salida y convertirlos en texto a través del reconocimiento de escritura a mano.

Para obtener más información sobre las distintas formas de usar Windows Ink en aplicaciones de Windows, consulta [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

### <a name="speech-interactions"></a>Interacciones de voz

![pantalla de reconocimiento inicial para una restricción basada en un archivo de gramática SGRS](images/speech-listening-initial.png)

![pantalla de reconocimiento final para una restricción basada en un archivo de gramática SGRS](images/speech-listening-complete.png)

Windows proporciona varias formas de integrar el reconocimiento de voz y la función de texto a voz (también denominada TTS o síntesis de voz) directamente en la experiencia del usuario de la aplicación. La voz puede ser una forma eficaz y divertida de que la gente interactúe con tu aplicación, que además complementa (llegando incluso a sustituir) el teclado, el mouse, la función táctil y los gestos.

Para obtener más información sobre las distintas formas de usar las interacciones de voz en aplicaciones de Windows, consulta [Habla, voz y conversación en Windows 10](speech.md).

### <a name="windows-ai"></a>Inteligencia artificial de Windows

![Inteligencia artificial de Windows](images/windows-ai.png)

Ofrecemos varias soluciones de IA distintas que puedes usar para mejorar tus aplicaciones de Windows. Con Machine Learning de Windows, puedes integrar modelos de aprendizaje automático entrenados en tus aplicaciones y ejecutarlos localmente en el dispositivo. Windows Vision Skills te permite usar bibliotecas precompiladas para realizar tareas comunes de procesamiento de imágenes o bien crear tus propias soluciones personalizadas. DirectML proporciona API de bajo nivel de estilo de DirectX que permiten sacar el máximo partido del hardware.

Para obtener más información sobre las distintas formas de integrar la IA en las aplicaciones de Windows, consulta [Inteligencia artificial de Windows](/windows/ai/).

## <a name="features-and-technologies-by-platform"></a>Características y tecnologías según la plataforma

En las secciones siguientes se proporcionan vínculos útiles para obtener más información sobre cómo realizar la integración con las características y tecnologías básicas de Windows desde nuestras principales plataformas de aplicaciones: UWP, Win32 (API de Windows), WPF y Windows Forms.

### <a name="user-interface-and-accessibility"></a>Interfaz de usuario y accesibilidad

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Diseño](/windows/uwp/design/basics/)<br/><br/>[Diseño](/windows/uwp/design/layout/)<br/><br/>[Controles](/windows/uwp/design/controls-and-patterns/)<br/><br/>[Entrada](/windows/uwp/design/input/)<br/><br/>[Iconos](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[Capa visual](/windows/uwp/composition/visual-layer)<br/><br/>[Plataforma XAML](/windows/uwp/xaml-platform/)<br/><br/>[Inicio, reanudación y tareas en segundo plano](/windows/uwp/launch-resume/)<br/><br/>[Accesibilidad de Windows](/windows/uwp/design/accessibility/accessibility)<br/><br/>[Interacciones de voz](/windows/uwp/design/input/speech-interactions)<br/><br/> |  [Interfaz de usuario del escritorio](/windows/desktop/windows-application-ui-development)<br/><br/>[Entorno de escritorio y shell](/windows/desktop/user-interface)<br/><br/>[Controles de Windows](/windows/desktop/controls/window-controls)<br/><br/>[Controles de UWP en aplicaciones de escritorio (islas XAML)](./desktop/modernize/xaml-islands.md)<br/><br/>[Capa visual de UWP en aplicaciones de escritorio](./desktop/modernize/visual-layer-in-desktop-apps.md)<br/><br/>[Ventanas y mensajes](/windows/desktop/winmsg/windowing)<br/><br/>[Menús y otros recursos](/windows/desktop/menurc/resources)<br/><br/>[Valores altos de PPP](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[Accesibilidad](/windows/desktop/accessibility)<br/><br/>[Microsoft Speech Platform: kit de desarrollo de software (SDK) (versión 11)](https://www.microsoft.com/download/details.aspx?id=27226)<br/><br/>[Microsoft Speech SDK, versión 5.1](https://www.microsoft.com/download/details.aspx?id=10121)<br/><br/>  |  [Windows in WPF](/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[Información general sobre la navegación](/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[XAML en WPF](/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[Controles](/dotnet/framework/wpf/controls/)<br/><br/>[Programación de capas visuales](/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[Entrada](/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[Accesibilidad](/dotnet/framework/ui-automation/)<br/><br/>[Guía de programación de System.Speech para .NET Framework](/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))<br/><br/>  | [Crear un formulario de Windows Forms](/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[Controles](/dotnet/framework/winforms/controls/)<br/><br/>[Cuadros de diálogo](/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[Entrada de usuario](/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Accesibilidad de formularios de Windows Forms](/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/>[Guía de programación de System.Speech para .NET Framework](/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))<br/><br/> |

### <a name="audio-video-and-graphics"></a>Audio, vídeo y gráficos

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Audio, vídeo y cámara](/windows/uwp/audio-video-camera/)<br/><br/>[Reproducción de multimedia](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[Capa visual](/windows/uwp/composition/visual-layer)<br/><br/>[Plataforma XAML](/windows/uwp/xaml-platform/) |  [Audio y vídeo](/windows/desktop/audio-and-video)<br/><br/>[Gráficos y juegos](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[GDI de Windows](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [Elementos gráficos](/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Multimedia](/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [Gráficos y dibujos](/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms)<br/><br/>[Clase SoundPlayer](/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>Acceso a datos y recursos de la aplicación

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Acceso a datos](/windows/uwp/data-access/)<br/><br/>[Enlace de datos](/windows/uwp/data-binding/)<br/><br/>[Archivos, carpetas y bibliotecas](/windows/uwp/files/)<br/><br/>[Recursos de la aplicación](/windows/uwp/app-resources/) |  [Acceso y almacenamiento de datos](/windows/desktop/data-access-and-storage)<br/><br/>[Sistemas de archivos locales](/windows/desktop/fileio/file-systems)<br/><br/>[Información general sobre recursos](/windows/desktop/menurc/resources-overviews)</li>  |  [Datos y modelado](/dotnet/framework/data/)<br/><br/>[Enlace de datos](/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[Recursos en aplicaciones .NET](/dotnet/framework/resources/)<br/><br/>[Archivos de datos, recursos y contenido de aplicaciones](/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [Datos y modelado](/dotnet/framework/data/)<br/><br/>[Enlace de datos](/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[Recursos en aplicaciones .NET](/dotnet/framework/resources/)<br/><br/>[Configuración de la aplicación](/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>Dispositivos, documentos e impresión

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Habilitar funcionalidades de dispositivos](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[Enumerar dispositivos](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Sensores](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[Impresión y digitalización](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [API de sensor](/windows/desktop/sensorsapi/portal)<br/><br/>[Impresión](/desktop/printdocs/printdocs-printing)<br/><br/>[API de UPnP](/desktop/upnp/universal-plug-and-play-start-page) |  [Impresión y administración de sistemas de impresión](/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [Compatibilidad con la impresión](/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>Sistema, red y energía

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Enumerar dispositivos](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Obtener información sobre la batería](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[Subprocesamiento y programación asincrónica](/windows/uwp/threading-async/)<br/><br/>[Servicios web y redes](/windows/uwp/networking/) | [Servicios del sistema](/windows/desktop/system-services)<br/><br/>[Administración de la memoria](/windows/desktop/memory/memory-management)<br/><br/>[Administración de la energía](/windows/desktop/power/power-management-portal)<br/><br/>[Procesos y subprocesos](/windows/desktop/procthread/processes-and-threads)<br/><br/>[Redes e Internet](/windows/desktop/networking)<br/><br/>[Información del sistema de Windows](/windows/desktop/sysinfo/windows-system-information) |  [Modelo de subprocesos](/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[Programación para redes en .NET Framework](/dotnet/framework/network-programming/)  |  [Información del sistema](/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[Administración de la energía](/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[Programación para redes en .NET Framework](/dotnet/framework/network-programming/)<br/><br/>[Redes en Windows Forms](/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="debugging-and-performance"></a>Depuración y rendimiento

|  UWP  |  Win32 (API de Windows) |  WPF y Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Depuración, pruebas y rendimiento](/windows/uwp/debug-test-perf)<br/><br/>[Implementación y depuración de aplicaciones para UWP](/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)<br/><br/>[Kit para la certificación de aplicaciones en Windows](/windows/uwp/debug-test-perf/windows-app-certification-kit)<br/><br/>[Rendimiento](/windows/uwp/debug-test-perf/performance-and-xaml-ui)| [Depuración y control de errores](/windows/win32/debugging-and-error-handling)<br/><br/>[Herramientas de depuración para Windows](/windows-hardware/drivers/debugger/)<br/><br/>[Seguimiento de eventos para Windows (ETW)](/windows/win32/etw/event-tracing-portal)<br/><br/>[API .NET TraceProcessing](./trace-processing/index.yml)<br/><br/>[TraceLogging](/windows/win32/tracelogging/trace-logging-portal)<br/><br/>[Contadores de rendimiento](/windows/win32/perfctrs/performance-counters-portal) |  [Depuración, seguimiento y generación de perfiles](/dotnet/framework/debug-trace-profile/)<br/><br/>[Seguimiento e instrumentación de aplicaciones](/dotnet/framework/debug-trace-profile/tracing-and-instrumenting-applications)<br/><br/>[Diagnóstico de errores con asistentes para la depuración administrada](/dotnet/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants)<br/><br/>[Generación de perfiles en tiempo de ejecución](/dotnet/framework/debug-trace-profile/runtime-profiling)<br/><br/>[Contadores de rendimiento](/dotnet/framework/debug-trace-profile/performance-counters)<br/><br/>[Implementación de ClickOnce para Windows Forms](/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |

### <a name="packaging-and-deployment"></a>Empaquetado e implementación

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Empaquetado de aplicaciones](/windows/uwp/packaging/)<br/><br/>[MSIX](/windows/msix/)<br/><br/>[Esquema del manifiesto del paquete de la aplicación](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [Empaquetado de aplicaciones de escritorio para Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Instalación y mantenimiento de aplicaciones](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [Empaquetado de aplicaciones de escritorio para Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Implementación de .NET Framework y aplicaciones](/dotnet/framework/deployment/)<br/><br/>[Implementación de una aplicación WPF](/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [Empaquetado de aplicaciones de escritorio para Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Implementación de .NET Framework y aplicaciones](/dotnet/framework/deployment/)<br/><br/>[Implementación de ClickOnce para Windows Forms](/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |