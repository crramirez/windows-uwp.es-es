---
Description: Esta sección le ayuda a entender cómo ciertas características clave de Windows se admiten en distintas plataformas de aplicaciones y cómo empezar a usar las características del código.
title: Características y tecnologías
ms.topic: article
ms.date: 05/08/2019
ms.localizationpriority: medium
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: 4fb49b5145350360be3b86c73110762cfa30e9db
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339342"
---
# <a name="features-and-technologies-for-windows-apps"></a>Características y tecnologías para aplicaciones de Windows

Independientemente del tipo de aplicación que se va a compilar o del dispositivo de destino, Windows admite muchas características que son bloques de creación clave para escenarios de aplicaciones importantes. Algunas de estas características se exponen al Plataforma universal de Windows (UWP), Win32 (API de Windows) y otras plataformas de aplicaciones de maneras diferentes. Los artículos siguientes le ayudarán a entender cómo se admiten determinadas características de Windows en distintas plataformas de aplicaciones y cómo empezar a usar las características del código.

En este artículo se proporciona una lista personalizada de artículos para obtener más información sobre cómo puede tener acceso a las características y tecnologías importantes de Windows en UWP, Win32 (API de Windows), WPF y Windows Forms plataformas de aplicaciones. Para obtener información completa sobre las características de desarrollo de cada plataforma, consulte los siguientes recursos:

* [Documentación de UWP](/windows/uwp/index)
* [Documentación de Win32 (API de Windows)](/windows/desktop/index)
* [Documentación de WPF](https://docs.microsoft.com/dotnet/framework/wpf/index)
* [Windows Forms documentación](https://docs.microsoft.com/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>Características y tecnologías clave de Windows

En las secciones siguientes se resaltan varias características y tecnologías importantes de Windows que le permiten ofrecer experiencias modernas y eficaces a sus clientes.

### <a name="windows-ink"></a>Windows Ink

![Lápiz para Surface](images/hero-small.png)  

La plataforma de Windows Ink, junto con un dispositivo de lápiz, te ofrece una forma natural de crear notas, dibujos y anotaciones manuscritas, todo ello de forma digital. Asimismo, la plataforma te permite capturar los datos de entrada del digitalizador a modo de datos de entrada de lápiz, generar datos de entrada de lápiz, administrarlos, representarlos como trazos de lápiz en el dispositivo de salida y convertirlos en texto a través del reconocimiento de escritura a mano.

Para obtener más información sobre las distintas formas de usar Windows Ink en aplicaciones de Windows, consulte [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

### <a name="speech-interactions"></a>Interacciones de voz

![initial reconocimiento screen for a constraint based on a sgrs grammar file](images/speech-listening-initial.png)

![pantalla de reconocimiento final para una restricción basada en un archivo de gramática SGRS](images/speech-listening-complete.png)

Windows proporciona muchas maneras de integrar el reconocimiento de voz y el texto a voz (también conocido como TTS o síntesis de voz) directamente en la experiencia del usuario de la aplicación. La voz puede ser una forma sólida y agradable de que los usuarios puedan interactuar con la aplicación, complementar o incluso reemplazar, teclado, Mouse, toque y gestos.

Para obtener más información sobre las distintas formas de usar las interacciones de voz en aplicaciones de Windows, consulte [interacciones de voz](/windows/uwp/design/input/speech-interactions).

### <a name="windows-ai"></a>Inteligencia artificial de Windows

![Inteligencia artificial de Windows](images/windows-ai.png)

Ofrecemos varias soluciones de AI diferentes que puede usar para mejorar sus aplicaciones Windows. Con Windows Machine Learning, puede integrar modelos de aprendizaje automático entrenados en sus aplicaciones y ejecutarlos localmente en el dispositivo. Los conocimientos de la visión de Windows permiten usar bibliotecas pregeneradas para realizar tareas comunes de procesamiento de imágenes o crear sus propias soluciones personalizadas. DirectML proporciona API de bajo nivel de DirectX que permiten sacar el máximo partido del hardware.

Para obtener más información sobre las distintas maneras de integrar AI en aplicaciones de Windows, consulte [Windows AI](https://docs.microsoft.com/windows/ai/).

## <a name="features-and-technologies-by-platform"></a>Características y tecnologías por plataforma

En las secciones siguientes se proporcionan vínculos útiles para obtener más información sobre cómo integrar con las principales características y tecnologías de Windows desde nuestras principales plataformas de aplicaciones: UWP, Win32 (API de Windows), WPF y Windows Forms.

### <a name="user-interface-and-accessibility"></a>Interfaz de usuario y accesibilidad

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Diseño](/windows/uwp/design/basics/)<br/><br/>[Diseño](/windows/uwp/design/layout/)<br/><br/>[Controles](/windows/uwp/design/controls-and-patterns/)<br/><br/>[Entradas](/windows/uwp/design/input/)<br/><br/>[Iconos](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[Capa visual](/windows/uwp/composition/visual-layer)<br/><br/>[Plataforma XAML](/windows/uwp/xaml-platform/)<br/><br/>[Inicio, reanudación y tareas en segundo plano](/windows/uwp/launch-resume/)<br/><br/>[Accesibilidad de Windows](/windows/uwp/design/accessibility/accessibility)<br/><br/>  |  [Interfaz de usuario de escritorio](/windows/desktop/windows-application-ui-development)<br/><br/>[Entorno de escritorio y Shell](/windows/desktop/user-interface)<br/><br/>[Controles de Windows](/windows/desktop/controls/window-controls)<br/><br/>[Controles de UWP en aplicaciones de escritorio (islas XAML)](/windows/apps/desktop/modernize/xaml-islands)<br/><br/>[Nivel visual de UWP en aplicaciones de escritorio](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)<br/><br/>[Ventanas y mensajes](/windows/desktop/winmsg/windowing)<br/><br/>[Menús y otros recursos](/windows/desktop/menurc/resources)<br/><br/>[PPP alto](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[Accesibilidad](/windows/desktop/accessibility)<br/><br/>  |  [Windows en WPF](https://docs.microsoft.com/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[Información general sobre navegación](https://docs.microsoft.com/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[XAML en WPF](https://docs.microsoft.com/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[Controles](https://docs.microsoft.com/dotnet/framework/wpf/controls/)<br/><br/>[Programación de capas visuales](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[Entradas](https://docs.microsoft.com/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[Accesibilidad](https://docs.microsoft.com/dotnet/framework/ui-automation/)<br/><br/>  | [Crear un formulario de Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[Controles](https://docs.microsoft.com/dotnet/framework/winforms/controls/)<br/><br/>[Cuadros de diálogo](https://docs.microsoft.com/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[Datos proporcionados por el usuario](https://docs.microsoft.com/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Accesibilidad Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/> |

### <a name="audio-video-and-graphics"></a>Audio, vídeo y gráficos

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Audio, vídeo y cámara](/windows/uwp/audio-video-camera/)<br/><br/>[Reproducción de multimedia](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[Capa visual](/windows/uwp/composition/visual-layer)<br/><br/>[Plataforma XAML](/windows/uwp/xaml-platform/) |  [Audio y vídeo](/windows/desktop/audio-and-video)<br/><br/>[Gráficos y juegos](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[GDI de Windows](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [Elementos gráficos](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Mutimedia](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [Gráficos y dibujo](https://docs.microsoft.com/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms)<br/><br/>[SoundPlayer (clase)](https://docs.microsoft.com/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>Acceso a datos y recursos de la aplicación

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Acceso a datos](/windows/uwp/data-access/)<br/><br/>[Enlace de datos](/windows/uwp/data-binding/)<br/><br/>[Archivos, carpetas y bibliotecas](/windows/uwp/files/)<br/><br/>[Recursos de la aplicación](/windows/uwp/app-resources/) |  [Acceso a datos y almacenamiento](/windows/desktop/data-access-and-storage)<br/><br/>[Sistemas de archivos locales](/windows/desktop/fileio/file-systems)<br/><br/>[Información general sobre recursos](/windows/desktop/menurc/resources-overviews)</li>  |  [Datos y modelado](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Enlace de datos](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[Recursos en aplicaciones .NET](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[Archivos de recursos, contenido y datos de la aplicación](https://docs.microsoft.com/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [Datos y modelado](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Enlace de datos](https://docs.microsoft.com/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[Recursos en aplicaciones .NET](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[Configuración de la aplicación](https://docs.microsoft.com/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>Dispositivos, documentos e impresión

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Habilitar funcionalidades de dispositivos](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[Enumerar dispositivos](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Sensores](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[Impresión y digitalización](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [API de sensor](/windows/desktop/sensorsapi/portal)<br/><br/>[Impresión](/desktop/printdocs/printdocs-printing)<br/><br/>[API de UPnP](/desktop/upnp/universal-plug-and-play-start-page) |  [Impresión y administración del sistema de impresión](https://docs.microsoft.com/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [Compatibilidad con impresión](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>Sistema, red y energía

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Enumerar dispositivos](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Obtener información sobre la batería](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[Subprocesamiento y programación asincrónica](/windows/uwp/threading-async/)<br/><br/>[Servicios web y redes](/windows/uwp/networking/) | [Servicios del sistema](/windows/desktop/system-services)<br/><br/>[Administración de memoria](/windows/desktop/memory/memory-management)<br/><br/>[Administración de energía](/windows/desktop/power/power-management-portal)<br/><br/>[Procesos y subprocesos](/windows/desktop/procthread/processes-and-threads)<br/><br/>[Redes e Internet](/windows/desktop/networking)<br/><br/>[Información del sistema de Windows](/windows/desktop/sysinfo/windows-system-information) |  [Modelo de subprocesos](https://docs.microsoft.com/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[Programación de redes en el .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)  |  [Información del sistema](https://docs.microsoft.com/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[Administración de energía](https://docs.microsoft.com/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[Programación de redes en el .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)<br/><br/>[Funciones de red en Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="packaging-and-deployment"></a>Empaquetado e implementación

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Empaquetado de aplicaciones](/windows/uwp/packaging/)<br/><br/>[MSIX](https://docs.microsoft.com/windows/msix/)<br/><br/>[Esquema del manifiesto del paquete de la aplicación](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [Empaquetar aplicaciones de escritorio de Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Instalación y mantenimiento de aplicaciones](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [Empaquetar aplicaciones de escritorio de Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Implementación de la .NET Framework y las aplicaciones](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Implementar una aplicación de WPF](https://docs.microsoft.com/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [Empaquetar aplicaciones de escritorio de Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Implementación de la .NET Framework y las aplicaciones](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Implementación de ClickOnce para Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |
