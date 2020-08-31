---
ms.assetid: 9ca1f880-2ced-46b4-8ea7-aba43d2ff863
description: Obtenga información sobre los problemas conocidos de la versión actual del SDK de Microsoft Advertising.
title: Problemas conocidos y solución de problemas para anuncios en aplicaciones
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, problemas conocidos, solución de problemas
ms.localizationpriority: medium
ms.openlocfilehash: c69c61cc1db0796edbaedb2f8e2970e1100c5774
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158539"
---
# <a name="known-issues-and-troubleshooting-for-ads-in-apps"></a>Problemas conocidos y solución de problemas para anuncios en aplicaciones

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

En este tema se enumeran los problemas conocidos de la versión actual del SDK de Microsoft Advertising. Para obtener más información sobre la solución de problemas, vea los temas siguientes.

* [Guía de solución de problemas de HTML y JavaScript](html-and-javascript-troubleshooting-guide.md)
* [Guía de solución de problemas de XAML y C#](xaml-and-c-troubleshooting-guide.md)

## <a name="adcontrol-interface-unknown-in-xaml"></a>Interfaz de AdControl desconocida en XAML

El marcado XAML de un [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) puede mostrar incorrectamente una línea curva azul que indica que la interfaz es desconocida. Esto ocurre solo cuando el destino es x86 y se puede ignorar.

## <a name="lasterror-from-previous-ad-request"></a>lastError de una solicitud de anuncio anterior

Si hay un **lastError** sobrante de la solicitud de anuncio anterior, se puede desencadenar el evento dos veces en la siguiente llamada al anuncio. Si bien la nueva solicitud del anuncio se realizará y puede producir un anuncio válido, este comportamiento puede causar confusión.

## <a name="interstitial-ads-and-navigation-buttons-on-phones"></a>Botones de navegación y anuncios intersticiales en los teléfonos

En teléfonos (o emuladores) que tengan botones **atrás**, **Inicio**y **búsqueda** de software en lugar de botones de hardware, el temporizador de cuenta atrás y los botones de clic a través de los anuncios intersticiales pueden verse ocultos.

## <a name="recently-created-ads-are-not-being-served-to-your-app"></a>Los anuncios creados recientemente no se envían a la aplicación

Si has creado un anuncio recientemente (menos de un día), podría no estar disponible de inmediato. Si el anuncio ha sido aprobado para contenido editorial, se enviará una vez que el servidor de publicidad lo haya procesado y el anuncio esté disponible como inventario.

## <a name="no-ads-are-shown-in-your-app"></a>No se muestran anuncios en la aplicación

Existen muchos motivos para que no veas anuncios, incluidos errores de red. Entre otros motivos se incluyen:

* Seleccionar una unidad de anuncio en el centro de Partners con un tamaño mayor o menor que el tamaño del **control** de la aplicación en el código de la aplicación.

* Los anuncios no aparecerán si usas un [valor del modo de prueba](set-up-ad-units-in-your-app.md#test-ad-units) para tu identificador de unidad de anuncio cuando se ejecuta una aplicación dinámica.

* Si has creado un nuevo identificador de unidad de anuncio en última media hora, puede que no veas un anuncio hasta que los servidores propaguen nuevos datos a través del sistema. Los identificadores existentes que han mostrado anuncios antes deberían mostrar anuncios de inmediato.

Si puedes ver anuncios de prueba en la aplicación, el código funciona y es capaz de mostrar anuncios. Si tienes problemas, ponte en contacto con [soporte técnico](https://developer.microsoft.com/windows/support). En esa página, elija **póngase en contacto con nosotros**.

También puedes publicar una pregunta en el [foro](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps).

## <a name="test-ads-are-showing-in-your-app-instead-of-live-ads"></a>En la aplicación se muestran anuncios de prueba, en lugar de anuncios dinámicos

Pueden mostrarse anuncios de prueba, incluso cuando esperas anuncios dinámicos. Esto puede suceder en los siguientes escenarios:

* Nuestra plataforma de publicidad no puede comprobar o buscar el identificador de aplicación activo que se usa en el almacén. En este caso, cuando un usuario crea una unidad de anuncio, su estado puede empezar como dinámico (no de prueba), pero se moverá al estado de prueba dentro de las 6 horas posteriores a la primera solicitud del anuncio. Volverá a ser dinámico si no hay ninguna solicitud de las aplicaciones de prueba durante 10 días.

* Las aplicaciones de prueba o las aplicaciones que se ejecutan en el emulador no mostrará anuncios dinámicos.

Cuando una unidad de anuncios en directo está atendiendo a los anuncios de prueba, el estado de la unidad de anuncio muestra los **anuncios de prueba activos y de servicio** en el centro de Partners. Esto no corresponde actualmente a las aplicaciones para teléfonos.


<span id="reference_errors"/>

## <a name="reference-errors-caused-by-targeting-any-cpu-in-your-project"></a>Errores de referencia provocados por dirigirse a Cualquier CPU en el proyecto

Al usar el SDK de Microsoft Advertising, no puede tener como destino **ninguna CPU** del proyecto. Si el proyecto se dirige a la plataforma **Cualquier CPU**, puede que veas una advertencia después de agregar la referencia similar a esta.

![ReferenceError \- solutionexplorer](images/13-19629921-023c-42ec-b8f5-bc0b63d5a191.jpg)

Para quitar esta advertencia, actualiza el proyecto para usar una salida de compilación específica de la arquitectura (por ejemplo, **x86**). Usa **Configuration Manager** para establecer los destinos de la plataforma para configuraciones de depuración y lanzamiento.

![configurationmanagerwin10](images/13-87074274-c10d-4dbd-9a06-453b7184f8de.png)

Cuando crees los paquetes de la aplicación para el envío a la tienda (como se muestra en las siguientes imágenes), asegúrate de incluir las arquitecturas que quieres como destino. Puedes optar por omitir x64 si vas a ejecutar compilaciones x86 en el sistema operativo x64.

![projectstorecreateapppackages](images/13-a99b05a4-8917-4c53-822e-2548fadf828a.png)

![createapppackages](images/13-16280cb1-a838-42b9-9256-eac7f33f5603.png)

## <a name="z-order-in-javascripthtml-apps"></a>Orden Z en las aplicaciones de JavaScript/HTML

Las aplicaciones de JavaScript/HTML no deben colocar elementos en el intervalo MAX-10 reservado de orden z. La única excepción es una superposición de interrupción, como una notificación de llamada entrante para una aplicación de Skype.

<span id="bkmk-ui"/>

## <a name="do-not-use-borders"></a>No uses bordes

Establecer las propiedades relacionadas con los bordes heredados por **AdControl** de su clase primaria hará que la colocación del anuncio sea incorrecta.

## <a name="more-information"></a>Más información

Para obtener más información acerca de los problemas conocidos más recientes y publicar preguntas relacionadas con el SDK de Microsoft Advertising, visite el [Foro](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps)de.

 

 