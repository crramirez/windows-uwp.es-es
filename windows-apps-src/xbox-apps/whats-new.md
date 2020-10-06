---
title: Novedades de UWP en Xbox One
description: Vea nuevas características, actualizaciones de las características existentes y correcciones de errores para desarrolladores en la actualización más reciente de UWP en Xbox One.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: 24c7893f18037d5585670f3e4de6a04998654e3a
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91763098"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Qué novedades encontrarán los desarrolladores en la última actualización de UWP en Xbox One

La actualización más reciente de Plataforma universal de Windows (UWP) en Xbox One contiene las siguientes características nuevas, actualizaciones de las características existentes y correcciones de errores.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>las aplicaciones y los juegos x86 ya no se admiten en Xbox  
Xbox ya no admite el desarrollo de aplicaciones x86 o envíos de aplicaciones x86 a la tienda.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>Ahora, las aplicaciones pueden volver a navegar a la aplicación anterior. 
UWP en Xbox One apps ahora puede admitir la navegación de vuelta a la aplicación anterior. Para ello, suscríbase al evento [**Windows.UI.Core.SystemNavigationManager. Alsolicited**](/uwp/api/Windows.UI.Core.SystemNavigationManager) y establezca la propiedad **Handled** en **false** en el controlador de eventos.

> [!NOTE]
> Por motivos de compatibilidad, esta funcionalidad solo está disponible para las aplicaciones que se compilan con la versión más reciente de UWP en Xbox One. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>Dev Home es ahora la experiencia de hogar predeterminada en las consolas de desarrollo
Ahora, las consolas de desarrollo inician dev Home como la experiencia de inicio predeterminada. Esto le permite tener derecho a trabajar sin necesidad de hacer clic en la pantalla principal de venta directa. Dev Home incluye ahora una acción rápida para iniciar la pantalla principal de venta directa. Además, una nueva configuración le permite realizar la experiencia de venta al por menor. 

## <a name="new-dev-home-user-interface"></a>Nueva interfaz de usuario de dev Home
La interfaz de usuario de dev Home incluye ahora las siguientes mejoras de productividad:
 - Los datos importantes, como la dirección IP y la versión de recuperación, se muestran ahora en la parte superior de la pantalla para su visibilidad. 
 - Dev Home ahora tiene una interfaz de usuario con pestañas que agrupa las herramientas en conjuntos lógicos, lo que permite una navegación rápida.
 - Los botones de acción rápida de la primera pestaña de dev Home permiten un acceso rápido a las acciones que se usan con más frecuencia. 

## <a name="wdp-for-xbox-enhancements"></a>WDP para mejoras de Xbox
Windows Device portal (WDP) ahora incluye compatibilidad adicional para la configuración de la consola. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>Ahora puede cambiar el tipo de título de UWP entre "aplicación" y "juego".
Cambiar el tipo de título de UWP entre "aplicación" y "juego" le permite probar los escenarios de juego sin publicarlos en la tienda. En dev Home, seleccione la aplicación en el panel **juegos & aplicaciones** , haga clic en el botón ver del controlador, seleccione detalles de la **aplicación** y, a continuación, cambie el tipo a "app" o "Game".

## <a name="see-also"></a>Consulte también
- [Problemas conocidos](known-issues.md)
- [UWP en Xbox One](index.md)
 - Ya puedes obtener una captura de pantalla de la consola. Para obtener más información acerca de cómo hacer una captura de pantalla, consulta el tema de referencia [/ext/screenshot](wdp-media-capture-api.md).
 - La herramienta puede implementar una compilación de archivos sueltos de la aplicación. Para obtener más información sobre compilaciones de archivos sueltos, consulta el tema de referencia [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Puedes acceder a los archivos de desarrollador de la consola desde el Explorador de archivos del equipo de desarrollo. Para obtener más información sobre cómo obtener acceso a archivos mediante el Explorador de archivos, consulta el tema de referencia [/ext/smb/developerfolder](wdp-smb-api.md).
