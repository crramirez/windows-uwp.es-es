---
Description: En esta sección le ayudará a comprender cómo se admiten determinadas características claves de Windows en plataformas diferentes de aplicaciones y cómo empezar a usar las características en el código.
title: Características y tecnologías
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: ff91d8c01e6832e645cc857b638851e1833fc3f9
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984985"
---
# <a name="features-and-technologies-for-windows-apps"></a>Características y tecnologías para aplicaciones de Windows

No importa qué tipo de aplicación sean edificio o dispositivo de que destino, Windows es compatible con muchas características que son bloques de creación claves para escenarios de aplicación importantes. Algunas de estas funciones se exponen a la plataforma de Windows Universal (UWP), Win32 (API de Windows) y otras plataformas de aplicaciones de maneras diferentes. Los siguientes artículos de ayudarle a entender cómo se admiten determinadas características de Windows en plataformas diferentes de aplicaciones y cómo empezar a usar las características en el código.

Este artículo proporciona una lista personalizada de artículos para obtener más información acerca de cómo puede tener acceso a características importantes de Windows y tecnologías en la UWP, Win32 (API de Windows), WPF y las plataformas de aplicaciones de Windows Forms. Para obtener información completa sobre las características de desarrollo de cada plataforma, consulte los siguientes recursos:

* [Documentación de UWP](/windows/uwp/index)
* [Documentación de Win32 (API de Windows)](/windows/desktop/index)
* [Documentación de WPF](https://docs.microsoft.com/dotnet/framework/wpf/index)
* [Documentación de Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>Las tecnologías y características de Windows de la clave

Las secciones siguientes resaltan varias características de Windows importantes y las tecnologías que permiten entregar modernas y proporcionen atractivas experiencias a sus clientes.

### <a name="windows-ink"></a>Windows Ink

![Lápiz para Surface](images/hero-small.png)  

La plataforma de Windows Ink, junto con un dispositivo de lápiz, te ofrece una forma natural de crear notas, dibujos y anotaciones manuscritas, todo ello de forma digital. Asimismo, la plataforma te permite capturar los datos de entrada del digitalizador a modo de datos de entrada de lápiz, generar datos de entrada de lápiz, administrarlos, representarlos como trazos de lápiz en el dispositivo de salida y convertirlos en texto a través del reconocimiento de escritura a mano.

Para obtener más información sobre las distintas formas de usar Windows tinta en aplicaciones de Windows, consulte [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

### <a name="speech-interactions"></a>Interacciones de voz

![initial reconocimiento screen for a constraint based on a sgrs grammar file](images/speech-listening-initial.png)

![pantalla de reconocimiento final para una restricción basada en un archivo de gramática SGRS](images/speech-listening-complete.png)

Windows proporciona muchas formas de integrar el reconocimiento de voz y texto a voz (también conocido como TTS o síntesis de voz) directamente en la experiencia del usuario de la aplicación. Voz puede ser una manera sólida y divertida para las personas a interactuar con su aplicación, al complementar o reemplazar incluso, teclado, mouse, toque y los gestos.

Para obtener más información sobre las distintas formas de usar las interacciones de voz en aplicaciones de Windows, consulte [interacciones de voz](/windows/uwp/design/input/speech-interactions).

### <a name="windows-ai"></a>Inteligencia artificial de Windows

![Inteligencia artificial de Windows](images/windows-ai.png)

Ofrecemos varias soluciones de inteligencia artificial diferentes que puede usar para mejorar las aplicaciones de Windows. Con Windows Machine Learning, puede integrar en las aplicaciones de los modelos de aprendizaje automático entrenado y ejecutarlas localmente en el dispositivo. Habilidades de visión de Windows le permite usar bibliotecas pregeneradas para realizar tareas de procesamiento de imágenes comunes o crear sus propias soluciones personalizadas. DirectML proporciona bajo nivel, las API de estilo de DirectX que le permiten aprovechar al máximo el hardware.

Para obtener más información sobre los distintos modos de integración de AI en aplicaciones de Windows, consulte [Windows AI](https://docs.microsoft.com/windows/ai/).

## <a name="features-and-technologies-by-platform"></a>Características y tecnologías de plataforma

Las siguientes secciones se proporciona vínculos útiles para obtener más información sobre cómo integrar con las características principales de Windows y las tecnologías de nuestras plataformas de aplicación principal: UWP, Win32 (API de Windows), WPF y Windows Forms.

### <a name="user-interface-and-accessibility"></a>Accesibilidad y la interfaz de usuario

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Diseño](/windows/uwp/design/basics/)<br/><br/>[Diseño](/windows/uwp/design/layout/)<br/><br/>[Controles](/windows/uwp/design/controls-and-patterns/)<br/><br/>[entrada](/windows/uwp/design/input/)<br/><br/>[Iconos](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[Capa visual](/windows/uwp/composition/visual-layer)<br/><br/>[Plataforma XAML](/windows/uwp/xaml-platform/)<br/><br/>[Inicio, reanudación y tareas en segundo plano](/windows/uwp/launch-resume/)<br/><br/>[Accesibilidad de Windows](/windows/uwp/design/accessibility/accessibility)<br/><br/>  |  [Interfaz de usuario de escritorio](/windows/desktop/windows-application-ui-development)<br/><br/>[Shell y el entorno de escritorio](/windows/desktop/user-interface)<br/><br/>[controles de Windows](/windows/desktop/controls/window-controls)<br/><br/>[Controles UWP en aplicaciones de escritorio (Islas de XAML)](/windows/apps/desktop/modernize/xaml-islands)<br/><br/>[Capa Visual de UWP en aplicaciones de escritorio](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)<br/><br/>[Windows y los mensajes](/windows/desktop/winmsg/windowing)<br/><br/>[Menús y otros recursos](/windows/desktop/menurc/resources)<br/><br/>[Valores altos de PPP](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[Accesibilidad](/windows/desktop/accessibility)<br/><br/>  |  [Windows en WPF](https://docs.microsoft.com/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[Información general sobre navegación](https://docs.microsoft.com/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[XAML en WPF](https://docs.microsoft.com/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[Controles](https://docs.microsoft.com/dotnet/framework/wpf/controls/)<br/><br/>[Programación de capas visuales](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[entrada](https://docs.microsoft.com/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[Accesibilidad](https://docs.microsoft.com/dotnet/framework/ui-automation/)<br/><br/>  | [Crear un formulario de Windows](https://docs.microsoft.com/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[Controles](https://docs.microsoft.com/dotnet/framework/winforms/controls/)<br/><br/>[Cuadros de diálogo](https://docs.microsoft.com/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[Entrada del usuario](https://docs.microsoft.com/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Accesibilidad de formularios Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/> |

### <a name="audio-video-and-graphics"></a>Audio, vídeo y gráficos

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Audio, vídeo y cámara](/windows/uwp/audio-video-camera/)<br/><br/>[Reproducción de multimedia](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[Capa visual](/windows/uwp/composition/visual-layer)<br/><br/>[Plataforma XAML](/windows/uwp/xaml-platform/) |  [Audio y vídeo](/windows/desktop/audio-and-video)<br/><br/>[Gráficos y juegos](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[Windows GDI](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [Elementos gráficos](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Mutimedia](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [Gráficos y dibujos](https://docs.microsoft.com/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms?view=netframework-4.8)<br/><br/>[SoundPlayer (clase)](https://docs.microsoft.com/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>Recursos de aplicación y el acceso de datos

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Acceso a datos](/windows/uwp/data-access/)<br/><br/>[Enlace de datos](/windows/uwp/data-binding/)<br/><br/>[Archivos, carpetas y bibliotecas](/windows/uwp/files/)<br/><br/>[Recursos de la aplicación](/windows/uwp/app-resources/) |  [Acceso a datos y almacenamiento](/windows/desktop/data-access-and-storage)<br/><br/>[Sistemas de archivos locales](/windows/desktop/fileio/file-systems)<br/><br/>[Información general de recursos](/windows/desktop/menurc/resources-overviews)</li>  |  [Datos y modelado](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Enlace de datos](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[Recursos en aplicaciones .NET](https://docs.microsoft.com/dotnet/framework/resources/?view=netframework-4.8)<br/><br/>[Archivos de recursos, contenido y datos de aplicación](https://docs.microsoft.com/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [Datos y modelado](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Enlace de datos](https://docs.microsoft.com/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[Recursos en aplicaciones .NET](https://docs.microsoft.com/dotnet/framework/resources/?view=netframework-4.8)<br/><br/>[configuración de la aplicación](https://docs.microsoft.com/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>Los dispositivos, documentos e impresión

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Habilitar funcionalidades de dispositivos](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[Enumerar dispositivos](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Sensores](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[Impresión y digitalización](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [API del sensor](/windows/desktop/sensorsapi/portal)<br/><br/>[Impresión](/desktop/printdocs/printdocs-printing)<br/><br/>[API de UPnP](/desktop/upnp/universal-plug-and-play-start-page) |  [Impresión y administración del sistema](https://docs.microsoft.com/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [compatibilidad con la impresión](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>Alimentación, red y sistema

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Enumerar dispositivos](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Obtener información sobre la batería](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[Subprocesamiento y programación asincrónica](/windows/uwp/threading-async/)<br/><br/>[Servicios web y redes](/windows/uwp/networking/) | [Servicios del sistema](/windows/desktop/system-services)<br/><br/>[Administración de memoria](/windows/desktop/memory/memory-management)<br/><br/>[Administración de energía](/windows/desktop/power/power-management-portal)<br/><br/>[Los procesos y subprocesos](/windows/desktop/procthread/processes-and-threads)<br/><br/>[Redes e Internet](/windows/desktop/networking)<br/><br/>[Información del sistema de Windows](/windows/desktop/sysinfo/windows-system-information) |  [Modelo de subprocesos](https://docs.microsoft.com/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[Programación para redes en .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)  |  [Información del sistema](https://docs.microsoft.com/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[Administración de energía](https://docs.microsoft.com/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[Programación para redes en .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)<br/><br/>[Funciones de red en Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="packaging-and-deployment"></a>Empaquetado e implementación

|  UWP  |  Win32 (API de Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Empaquetado de aplicaciones](/windows/uwp/packaging/)<br/><br/>[MSIX](https://docs.microsoft.com/windows/msix/)<br/><br/>[Esquema de manifiesto de paquete de aplicación](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [Empaquetar aplicaciones de escritorio de Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Instalación de la aplicación y mantenimiento](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [Empaquetar aplicaciones de escritorio de Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Implementar .NET Framework y aplicaciones](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Implementar una aplicación WPF](https://docs.microsoft.com/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [Empaquetar aplicaciones de escritorio de Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Implementar .NET Framework y aplicaciones](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Implementación de ClickOnce para Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |
