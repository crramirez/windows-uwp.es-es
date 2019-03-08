---
title: Novedades de UWP en Xbox One
description: Destaca las nuevas características de las aplicaciones para UWP en Xbox One.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: aff65e5f1b4771cbb33bc8b8219224042b7bf7e2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660690"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Qué novedades encontrarán los desarrolladores en la última actualización de UWP en Xbox One

La actualización más reciente de la Plataforma universal de Windows (UWP) en Xbox One contiene las nuevas características, las actualizaciones de funciones ya existentes y las correcciones de errores siguientes.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>Las aplicaciones y juegos x86 ya no se admiten en Xbox  
Xbox ya no admite el desarrollo o envío de aplicaciones x86 a la Store.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>Las aplicaciones ahora pueden admitir la vuelta a la aplicación anterior 
Las aplicaciones para UWP en Xbox One ahora pueden admitir la vuelta a la aplicación anterior. Para ello, suscríbete al evento [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595) y establece la propiedad **Handled** en **false** en el controlador de eventos.

> [!NOTE]
> Por motivos de compatibilidad, esta funcionalidad está disponible solo para las aplicaciones creadas con la versión más reciente de UWP en Xbox One. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>Dev Home hora es la experiencia de inicio predeterminada en las consolas de desarrollo
Las consolas de desarrollo ahora se inician con Dev Home como la experiencia de inicio predeterminada. Esto te permite comenzar a trabajar de inmediato sin la necesidad de desplazarse desde la pantalla Inicio del modo comercial. Dev Home ahora incluye una acción rápida para iniciar la pantalla Inicio del modo comercial. Además, una nueva configuración te permite que Inicio en el modo comercial sea la experiencia predeterminada. 

## <a name="new-dev-home-user-interface"></a>Nueva interfaz de usuario de Dev Home
La interfaz de usuario de Dev Home ahora incluye las siguientes mejoras de productividad:
 - Los datos importantes, como la dirección IP y la versión de recuperación, ahora se muestran en la parte superior de la pantalla para mayor visibilidad. 
 - Dev Home ahora tiene una interfaz de usuario con pestañas que agrupa herramientas en conjuntos lógicos, lo que permite una navegación rápida.
 - Los botones de acción rápida en la primera pestaña de Dev Home permiten un rápido acceso a las acciones más usadas. 

## <a name="wdp-for-xbox-enhancements"></a>Mejoras de WDP para Xbox
Windows Device Portal (WDP) ahora incluye compatibilidad adicional para la configuración de la consola. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>Ahora puedes cambiar el tipo de título de UWP entre "Aplicación" y "Juego"
Cambiar el tipo de título de UWP entre "Aplicación" y "Juego" te permite probar los escenarios de juego sin publicar en la Tienda. En Dev Home, selecciona la aplicación en el panel **Juegos y aplicaciones**, presiona el botón Vista en el mando, selecciona **Detalles de la aplicación** y luego cambia el tipo a "Aplicación" o "Juego".

## <a name="see-also"></a>Consulte también
- [Problemas conocidos](known-issues.md)
- [UWP en Xbox One](index.md)
 - Ya puedes obtener una captura de pantalla de la consola. Para obtener más información acerca de cómo hacer una captura de pantalla, consulta el tema de referencia [/ext/screenshot](wdp-media-capture-api.md).
 - La herramienta puede implementar una compilación de archivos sueltos de la aplicación. Para obtener más información sobre compilaciones de archivos sueltos, consulta el tema de referencia [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Puedes acceder a los archivos de desarrollador de la consola desde el Explorador de archivos del equipo de desarrollo. Para obtener más información sobre cómo obtener acceso a archivos mediante el Explorador de archivos, consulta el tema de referencia [/ext/smb/developerfolder](wdp-smb-api.md).

## <a name="see-also"></a>Consulte también
- [Problemas conocidos](known-issues.md)
- [UWP en Xbox One](index.md)
