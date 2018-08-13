---
author: v-angraf
title: Novedades de UWP en Xbox One
description: Destaca las nuevas características de las aplicaciones para UWP en Xbox One.
ms.author: v-angraf
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: cbabe9d31b5b9762320df8e4a92d19ae4e33497d
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "301461"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Qué novedades encontrarán los desarrolladores en la última actualización de UWP en Xbox One

La actualización más reciente plataforma de Windows Universal (UWP) en una Xbox contiene las siguientes características nuevas, las actualizaciones de las características existentes y correcciones de errores.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 aplicaciones y juegos ya no se admiten en Xbox  
Xbox ya no admite el desarrollo o envío de aplicaciones x86 a la Store.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>Las aplicaciones pueden ahora admiten volver a desplazarse a la aplicación anterior 
UWP en aplicaciones Xbox uno ahora puede admitir volver a desplazarse a la aplicación anterior. Para ello, suscríbase al evento [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595) y establezca la propiedad **Handled** en **false** en su controlador de eventos.

> [!NOTE]
> Por motivos de compatibilidad, esta funcionalidad está disponible sólo para las aplicaciones que se generan con la versión más reciente de UWP en una Xbox. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>Página principal de desarrollo es ahora la experiencia de la página principal predeterminada en consolas de desarrollo
Consolas de desarrollo inicie ahora desarrollo principal como la experiencia de página principal predeterminada. Esto le permite obtener hacia la derecha para trabajar sin necesidad de hacer clic en a través de la pantalla de inicio de venta por menor. Página principal de desarrollo ahora incluye una acción rápida para iniciar la pantalla de inicio de venta por menor. Además, una nueva configuración de permite realizar la clave comercial particular la experiencia predeterminada de. 

## <a name="new-dev-home-user-interface"></a>Nueva interfaz de usuario principal de desarrollo
La interfaz de usuario principal de desarrollo ahora incluye las siguientes mejoras de productividad:
 - Datos importantes como dirección IP y versión de recuperación se muestra ahora en la parte superior de la pantalla de visibilidad. 
 - Página principal de desarrollo ahora tiene una IU con fichas que agrupa herramientas en conjuntos de lógicos, lo que permite la exploración rápida.
 - Botones de acción de rápido en la primera ficha de la página principal de desarrollo de permiten el acceso rápido a las acciones usadas con más frecuencia. 

## <a name="wdp-for-xbox-enhancements"></a>WDP para mejoras de Xbox
El Portal de dispositivo de Windows (WDP) ahora incluye compatibilidad adicional para la configuración de la consola. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>Ahora puede cambiar el tipo de su título UWP entre "App" y "Juego"
Cambiar el tipo de su título UWP entre "App" y "Juego" le permite probar escenarios de juego sin publicar en el almacén. En la página principal de desarrollo, seleccione la aplicación en el panel **juegos y aplicaciones** , presione el botón de vista en el controlador, seleccione **Detalles de la aplicación** y, a continuación, cambie el tipo a "App" o "Juego".

## <a name="see-also"></a>Consulta también
- [Problemas conocidos](known-issues.md)
- [UWP en Xbox One](index.md)
 - Ya puedes obtener una captura de pantalla de la consola. Para obtener más información acerca de cómo hacer una captura de pantalla, consulta el tema de referencia [/ext/screenshot](wdp-media-capture-api.md).
 - La herramienta puede implementar una compilación de archivos sueltos de la aplicación. Para obtener más información sobre compilaciones de archivos sueltos, consulta el tema de referencia [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Puedes acceder a los archivos de desarrollador de la consola desde el Explorador de archivos del equipo de desarrollo. Para obtener más información sobre cómo obtener acceso a archivos mediante el Explorador de archivos, consulta el tema de referencia [/ext/smb/developerfolder](wdp-smb-api.md).

## <a name="see-also"></a>Consulta también
- [Problemas conocidos](known-issues.md)
- [UWP en Xbox One](index.md)
