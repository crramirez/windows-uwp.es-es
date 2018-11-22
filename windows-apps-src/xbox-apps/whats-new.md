---
author: v-angraf
title: Novedades de UWP en Xbox One
description: Destaca las nuevas características de las aplicaciones para UWP en Xbox One.
ms.author: v-angraf
ms.date: 03/29/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: cc2168014e714de0b43b6ffffe84126764f0a4a3
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7570039"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Qué novedades encontrarán los desarrolladores en la última actualización de UWP en Xbox One

La actualización más reciente de la plataforma Universal Windows (UWP) en Xbox One contiene las siguientes nuevas características, las actualizaciones de características existentes y correcciones de errores.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 aplicaciones y juegos ya no se admite en Xbox  
Xbox ya no admite el desarrollo o envío de aplicaciones x86 a la Store.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>Las aplicaciones ahora pueden admitir la vuelta a la aplicación anterior 
UWP en Xbox One aplicaciones ahora puede admitir la vuelta a la aplicación anterior. Para ello, se suscribe al evento [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595) y establece la propiedad **Handled** en **false** en el controlador de eventos.

> [!NOTE]
> Por motivos de compatibilidad, esta funcionalidad solo está disponible para las aplicaciones creadas con la versión más reciente de UWP en Xbox One. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>Dev Home ahora es la experiencia principal predeterminada en las consolas de desarrollo
Ahora, consolas de desarrollo inicia Dev Home como la experiencia de inicio predeterminado. Esto te permite obtener hacia la derecha para funcionar sin necesidad de hacer clic a través de la pantalla de inicio de la versión comercial. Dev Home incluye ahora una acción rápida para iniciar la pantalla de inicio de la versión comercial. Además, una nueva configuración te permite realizar Home comercial de la experiencia de forma predeterminada. 

## <a name="new-dev-home-user-interface"></a>Nueva interfaz de usuario de Dev Home
La interfaz de usuario de Dev Home ahora incluye las siguientes mejoras de productividad:
 - Datos importantes, como la dirección IP y ahora se muestra la versión de recuperación en la parte superior de la pantalla de visibilidad. 
 - Dev Home ahora tiene una IU con fichas que agrupa herramientas en grupos lógicos, lo que permite la navegación rápida.
 - Botones de acción rápida en la primera pestaña de Dev Home permiten un acceso rápido a las acciones que se usa con más frecuencia. 

## <a name="wdp-for-xbox-enhancements"></a>WDP para mejoras de Xbox
Windows Device Portal (WDP) ahora incluye compatibilidad adicional para la configuración de la consola. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>Ahora puede cambiar el tipo de tu título UWP entre "Aplicación" y "Del juego"
Cambiar el tipo de tu título UWP entre "Aplicación" y "Del juego" te permite probar los escenarios de juego sin publicar en la tienda. En Dev Home, selecciona la aplicación en el panel de **juegos y aplicaciones** , presiona el botón de vista en el controlador, selecciona **los detalles de la aplicación** y, a continuación, cambiar el tipo de "Aplicación" o "Del juego".

## <a name="see-also"></a>Ver también
- [Problemas conocidos](known-issues.md)
- [UWP en Xbox One](index.md)
 - Ya puedes obtener una captura de pantalla de la consola. Para obtener más información acerca de cómo hacer una captura de pantalla, consulta el tema de referencia [/ext/screenshot](wdp-media-capture-api.md).
 - La herramienta puede implementar una compilación de archivos sueltos de la aplicación. Para obtener más información sobre compilaciones de archivos sueltos, consulta el tema de referencia [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Puedes acceder a los archivos de desarrollador de la consola desde el Explorador de archivos del equipo de desarrollo. Para obtener más información sobre cómo obtener acceso a archivos mediante el Explorador de archivos, consulta el tema de referencia [/ext/smb/developerfolder](wdp-smb-api.md).

## <a name="see-also"></a>Consulta también
- [Problemas conocidos](known-issues.md)
- [UWP en Xbox One](index.md)
