---
Description: Agrega interfaces de usuario modernas de XAML, crea paquetes MSIX e incorpora otros componentes actuales en la aplicación de escritorio.
title: Modernización de las aplicaciones de escritorio para Windows
ms.topic: article
ms.date: 04/17/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 6153a0a094d03081388c15ec31696ef277ef7081
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215214"
---
# <a name="modernize-your-desktop-apps"></a>Modernización de las aplicaciones de escritorio

Windows 10 y la Plataforma universal de Windows (UWP) ofrecen muchas características que puedes usar para ofrecer una experiencia moderna en las aplicaciones de escritorio. La mayoría de estas características están disponibles como componentes modulares que puedes adoptar en las aplicaciones de escritorio a tu propio ritmo sin tener que volver a escribir la aplicación para una plataforma diferente. Puedes mejorar las aplicaciones de escritorio existentes eligiendo qué partes de Windows 10 y UWP adoptar.

En este artículo se describen las características de Windows 10 y UWP que puedes usar en las aplicaciones de escritorio hoy mismo.

> [!NOTE]
> ¿Necesitas ayuda para migrar las aplicaciones de escritorio a Windows 10? El servicio [Asesoría de aplicaciones de escritorio](https://docs.microsoft.com/FastTrack/win-10-desktop-app-assure) proporciona soporte directo y gratuito a los desarrolladores que porten sus aplicaciones a Windows 10. Este programa está disponible para todos los fabricantes de software independiente y las empresas aptas. Para más información sobre los requisitos que hay que cumplir y sobre el propio programa, visita [https://aka.ms/DesktopAppAssure](https://aka.ms/DesktopAppAssure). Para empezar a trabajar ahora, [envía la solicitud](https://aka.ms/DesktopAppAssureRequest).

## <a name="msix-packages"></a>Paquetes MSIX

MSIX es un formato moderno de paquete de la aplicación de Windows que proporciona una experiencia de empaquetado universal para todas las aplicaciones de Windows, incluidas las aplicaciones de UWP, WPF, Windows Forms y Win32. MSIX reúne los mejores aspectos de las tecnologías de instalación de MSI, .appx, App-V y ClickOnce, y está compilado para ser seguro y confiable.

El empaquetado de las aplicaciones de escritorio de Windows en paquetes MSIX le permite acceder a una sólida experiencia de instalación y actualización, a un modelo de seguridad administrado con un sistema de funcionalidades flexible, a compatibilidad con Microsoft Store, a la administración empresarial y a muchos modelos de distribución personalizados.

Para más información, consulta [Empaquetar aplicaciones de escritorio](/windows/msix/desktop/desktop-to-uwp-root) en la documentación de MSIX.

## <a name="net-core-3"></a>.NET Core 3

.NET Core 3 es la siguiente versión principal de .NET Core. Lo más destacado de la próxima versión es la compatibilidad con aplicaciones de escritorio de Windows, incluidas las aplicaciones de Windows Forms y WPF. Podrás ejecutar aplicaciones de escritorio de Windows nuevas y existentes en .NET Core 3 y disfrutar de todas las ventajas que ofrece .NET Core. Los controles de UWP que se hospedan en [islas XAML](xaml-islands.md) también se pueden usar en las aplicaciones de Windows Forms y WPF que tienen como destino .NET Core 3.

Para obtener más información, consulta los artículos siguientes:

* [Anuncio de la versión preliminar 1 de .NET Core 3.0](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-1-and-open-sourcing-windows-desktop-frameworks/)
* [Anuncio de la versión preliminar 2 de .NET Core 3.0](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-2/)
* [Anuncio de la versión preliminar 3 de .NET Core 3.0](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-3/)
* [Anuncio de la versión preliminar 4 de .NET Core 3.0](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-4/)
* [Novedades en .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).

## <a name="uwp-apis"></a>Api de UWP

Puedes llamar a muchas API de UWP directamente en la aplicación de escritorio de WPF, Windows Forms o C++ Win32 para integrar las experiencias más actuales creadas para los usuarios de Windows 10. Por ejemplo, puedes llamar a las API de UWP para que agreguen notificaciones del sistema a la aplicación de escritorio.

Para más información, consulta [Uso de API de UWP en aplicaciones de escritorio](desktop-to-uwp-enhance.md).

## <a name="host-uwp-controls-xaml-islands"></a>Hospedaje de controles de UWP (islas XAML)

A partir de Windows 10, versión 1903, puede agregar [controles XAML para UWP](/windows/uwp/design/controls-and-patterns/controls-by-function) directamente en cualquier elemento de la interfaz de usuario de una aplicación de WPF, Windows Forms o C++ Win32 que esté asociada a un identificador de ventana (HWND). Esto significa que puedes integrar totalmente las características más recientes de UWP como, por ejemplo, [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) y los controles que admiten [Fluent Design System](/windows/uwp/design/fluent-design-system/index) en ventanas y en otras superficies de presentación de las aplicaciones de escritorio. Este escenario para desarrolladores se denomina a veces *islas XAML*.

Para más información, consulta [Uso de controles de UWP en aplicaciones de escritorio](xaml-islands.md).

## <a name="use-the-visual-layer-in-desktop-apps"></a>Uso de una capa visual en aplicaciones de escritorio

Ahora puedes usar las API de UWP en aplicaciones de escritorio que no son de UWP para mejorar la apariencia, aspecto y funcionalidad de las aplicaciones de WPF, Windows Forms y C++ Win32 y aprovechar las ventajas de las características más recientes de la interfaz de usuario de Windows 10 que solo están disponibles a través de UWP. Esto es útil cuando necesitas crear experiencias personalizadas que van más allá de los controles integrados de UWP que puedes hospedar mediante islas XAML.

Para más información, consulta [Modernización de una aplicación de escritorio mediante la capa visual](visual-layer-in-desktop-apps.md).

## <a name="additional-features-available-to-packaged-apps"></a>Hay características adicionales disponibles para aplicaciones empaquetadas

Algunas experiencias modernas de Windows 10 solo están disponibles en las aplicaciones de escritorio que se empaquetan en un [paquete MSIX](/windows/msix/desktop/desktop-to-uwp-root). Si empaquetas la aplicación de escritorio en un paquete MSIX, puedes usar las API de UWP que requieren identidad del paquete, extensiones de paquete y componentes de UWP en la aplicación empaquetada.

Para más información, consulta [Características que requieren identidad del paquete](modernize-packaged-apps.md).

<a id="desktop-uwp-controls"/>

## <a name="uwp-controls-optimized-for-desktop-apps"></a>Controles UWP optimizados para aplicaciones de escritorio

Tanto si va a compilar una aplicación para UWP centrada exclusivamente en la familia de dispositivos de escritorio como si desea usar los controles de UWP en una aplicación de escritorio de WPF, Windows Forms o C++ Win32, los siguientes controles de UWP nuevos y actualizados están diseñados para ofrecer experiencias optimizadas para escritorio gracias a [Fluent Design System](/windows/uwp/design/fluent-design-system/index). Estos controles se introdujeron en Windows 10, versión 1809 (actualización de octubre de 2018 o versión 10.0.17763).

| Control |  Descripción |
|------ |--------------|
| [MenuBar](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#create-a-menu-bar) | Proporciona una manera rápida y sencilla de exponer un conjunto de comandos para las aplicaciones que podrían necesitar una mayor organización o agrupación que las que permite un control **CommandBar**. |
| [DropDownButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button) | Muestra un botón de contenido adicional como un indicador visual que tiene un control flotante asociado que contiene más opciones.  |
| [SplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) | Proporciona un botón que tiene dos partes que se pueden invocar por separado. Una parte se comporta como un botón estándar e invoca una acción inmediata. La otra parte invoca un control flotante que contiene opciones adicionales entre las que puede elegir el usuario.|
| [ToggleSplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-toggle-split-button) | Proporciona un botón que tiene dos partes que se pueden invocar por separado. Una parte se comporta como un botón de alternancia que se puede activar o desactivar. La otra parte invoca un control flotante que contiene opciones adicionales entre las que puede elegir el usuario. |
| [CommandBarFlyout](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/command-bar-flyout) |  Permite mostrar las tareas comunes de usuario en el contexto de un elemento en el lienzo de la interfaz de usuario. |
| [ComboBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) | Ahora ya puedes realizar un cuadro combinado editable para que el usuario escriba los valores que no aparecen en el control.  |
| [TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) | Ya puedes configurar una vista de árbol que permita el enlace de datos, las plantillas de elementos y la opción para arrastrar y colocar.  |
| [DataGridView](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid) |   Ofrece una manera flexible de mostrar una colección de datos en filas y columnas. Este control está disponible en el [kit de herramientas de la comunidad Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/).  |

## <a name="windows-ui-library"></a>Biblioteca de interfaz de usuario de Windows

La biblioteca de interfaz de usuario de Windows es un conjunto de paquetes NuGet que proporciona nuevos controles y otros elementos de interfaz de usuario para aplicaciones de UWP. Las API de la biblioteca de interfaz de usuario de Windows funcionan en versiones anteriores de Windows 10, por lo que la aplicación funcionará incluso aunque los usuarios no estén ejecutando la última versión de Windows 10. Esto te permitirá adoptar nuevos controles a medida que se publican en la biblioteca de interfaz de usuario de Windows sin tener que preocuparte de incluir comprobaciones de la versión o XAML condicionales.

Consulta [Biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

## <a name="other-technologies-for-modern-desktop-apps"></a>Otras tecnologías para aplicaciones modernas de escritorio

### <a name="microsoft-graph"></a>Microsoft Graph

Microsoft Graph es una colección de API que puede usar para compilar aplicaciones para las organizaciones y consumidores que interactúan con los datos de millones de usuarios. Microsoft Graph expone las API REST y las bibliotecas de cliente para acceder a los datos de:
* Azure Active Directory
* Servicios de Office 365: SharePoint, OneDrive, Outlook, Exchange, Microsoft Teams, OneNote, Planner y Excel
* Servicios de Enterprise Mobility + Security: Identity Manager, Intune, Advanced Threat Analytics y Advanced Threat Protection.
* Servicios de Windows 10: actividades y dispositivos

Para más información, consulta los [documentos de Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/overview).

### <a name="adaptive-cards"></a>Tarjetas adaptables

Tarjetas adaptables es un marco abierto, multiplataforma que puedes usar para intercambiar contenido de la interfaz de usuario basado en tarjetas de forma común y coherente entre los distintos dispositivos y plataformas.

Para más información, consulta los [documentos de Tarjetas adaptables](https://docs.microsoft.com/adaptive-cards/).