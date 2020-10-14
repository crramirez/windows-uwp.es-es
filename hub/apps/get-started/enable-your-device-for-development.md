---
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: Habilitar el dispositivo para el desarrollo
description: Obtenga información sobre cómo habilitar un dispositivo con Windows 10 para el desarrollo y la depuración activando el Modo de desarrollador en Visual Studio.
keywords: Introducción a Visual Studio con licencia de desarrollador, dispositivo con licencia de desarrollador habilitada
ms.date: 10/13/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 642dd2ac6db5509e9bc4ea6ff8cb756312c3e90b
ms.sourcegitcommit: 56e9cab45d1c6e54841d61fdf23044fa01f50c43
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "92011956"
---
# <a name="enable-your-device-for-development"></a>Habilitar el dispositivo para el desarrollo

## <a name="activate-developer-mode-sideload-apps-and-access-other-developer-features"></a>Activar el modo de desarrollador, transferir aplicaciones localmente y acceder a otras funciones de desarrolladores

![Habilitar los dispositivo para el desarrollo](images/developer-poster.png)

> [!IMPORTANT]
> Si no va a crear sus propias aplicaciones en el equipo, no es necesario que habilite el modo de desarrollador. Si intenta solucionar un problema con el equipo, consulte [Ayuda de Windows](https://support.microsoft.com/hub/4338813/windows-help?os=windows-10). Si está desarrollando contenido por primera vez, también le recomendamos que descargue las herramientas que necesita a fin de [establecer la configuración](get-set-up.md).

Si estás usando tu equipo para actividades diarias normales, como juegos, exploración web, correo electrónico o aplicaciones de Office, *no* es necesario activar el modo de desarrollador y de hecho, no deberías activarlo. El resto de la información de esta página no te concierne y puedes volver tranquilamente a lo que estabas haciendo. Gracias por visitarnos.

Sin embargo, si estás programando software con Visual Studio en un equipo por primera vez, *debes* habilitar el modo de desarrollador en el PC de desarrollo y en todos los dispositivos que usarás para probar el código. Si abres un proyecto de UWP cuando el modo de desarrollador no esté habilitado, se abrirá la página de configuración **Para desarrolladores** o aparecerá el siguiente cuadro de diálogo en Visual Studio:

![Habilitar el cuadro de diálogo de modo de desarrollador que se muestra en Visual Studio](images/latestenabledialog.png)

Cuando veas este cuadro de diálogo, haz clic en **configuración para desarrolladores** para abrir la página de configuración **Para desarrolladores**.

> [!NOTE]
> Puedes ir a la página **Para desarrolladores** en cualquier momento para habilitar o deshabilitar el modo de desarrollador: solo tienes que escribir "para desarrolladores" en el cuadro de búsqueda de Cortana de la barra de tareas.

## <a name="accessing-settings-for-developers"></a>Obtener acceso a la configuración para desarrolladores

Para habilitar el modo de desarrollador u obtener acceso a otras opciones de configuración, haz lo siguiente:

1.  Desde el cuadro de diálogo de configuración **Para desarrolladores**, elige el nivel de acceso que necesitas.
2.  Lee el aviso de declinación de responsabilidades para la configuración elegida y, luego, haz clic en **Sí** para aceptar el cambio.

> [!NOTE]
> Para habilitar el modo de desarrollador se requiere acceso de administrador. Si el dispositivo pertenece a una organización, puede que esta opción esté deshabilitada.

### <a name="developer-mode-features"></a>Características del modo de desarrollador

El modo de desarrollador reemplaza el requisito de Windows 8.1 que exige una licencia de desarrollador.  Además de la transferencia local, la opción de modo de desarrollador habilita la depuración y otras opciones de implementación. Esto incluye el inicio de un servicio SSH para poder implementar el dispositivo. Para detener este servicio, tienes que deshabilitar el modo de desarrollador.

Cuando se habilita el modo de desarrollador en un equipo de escritorio, se instala un paquete de funciones que incluye:
- Portal de dispositivos Windows. Se habilita Device Portal y se configuran reglas de firewall para este solo si la opción **Habilitar Device Portal** está activada.
- Instala y configura reglas de firewall para los servicios SSH que permiten la instalación remota de aplicaciones. Cuando habilites la **Detección de dispositivos**, se activará en el servidor SSH.

Para obtener más información acerca de estas características, o si encuentra dificultades en el proceso de instalación, consulte [las características del modo de programador y la depuración](developer-mode-features-and-debugging.md).

## <a name="see-also"></a>Consulte también

* [Registrarse para obtener una cuenta de Windows](sign-up.md)
* [Características y depuración en modo de programador](developer-mode-features-and-debugging.md).