---
title: Procedimientos recomendados de Xbox
description: Cómo optimizar la aplicación para Xbox.
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e273b1b3bb84929005cfbe4a205397fa298ea1c8
ms.sourcegitcommit: 888a4679fa45637b1cc35f62843727ce44322e57
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "9059736"
---
# <a name="xbox-best-practices"></a>Procedimientos recomendados de Xbox

Todas las aplicaciones para UWP se ejecutarán en Xbox One de manera predeterminada sin que tengas que hacer nada más. Sin embargo, si quieres que tu aplicación destaque, haga las delicias de los clientes y pueda competir con las mejores experiencias que ofrecen otras aplicaciones de Xbox, te recomendamos que sigas los siguientes procedimientos a continuación.
  > [!NOTE]
  > Antes de comenzar, échales un vistazo a las pautas de diseño que encontrarás en el artículo [Diseño para Xbox y televisión](../design/devices/designing-for-tv.md).   

## <a name="to-build-the-best-experiences-for-xbox-one"></a>Crear las mejores experiencias para Xbox One

### <a name="do-turn-off-mouse-mode"></a>*Cómo:* desactivar el modo de mouse

Los usuarios de Xbox les encanta sus controladores. Para optimizar la entrada, el controlador [Deshabilitar el modo de mouse](how-to-disable-mouse-mode.md) y habilitar la navegación direccional (también conocida como [interacción y navegación con foco XY](../design/input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction)). Tenga cuidado para capturas de foco y la interfaz de usuario inaccesible.

### <a name="do-draw-a-focus-rectangle-that-is-appropriate-for-a-10-foot-experience"></a>*Cómo:* crear un rectángulo de foco que sea adecuado para una experiencia de 10 pies

La mayoría de usuarios de Xbox tienen la consola conectada a la televisión del salón, así que recuerda que el rectángulo de foco estándar resulta difícil de distinguir a una distancia de 10 pies. Para garantizar que el usuario pueda ver claramente y en todo momento el elemento de interfaz de usuario que tiene el foco de entrada, sigue las instrucciones del [foco visual](../design/input/gamepad-and-remote-interactions.md#focus-visual). En XAML obtendrás este comportamiento de forma gratuita cuando la aplicación se ejecute en Xbox, pero ten en cuenta que para las aplicaciones HTML deberás usar un estilo CSS personalizado.

### <a name="do-integrate-with-the-systemmediatransportcontrols-class"></a>*Cómo:* integrar con la clase SystemMediaTransportControls

A los usuarios de Xbox les gusta controlar las aplicaciones multimedia con el mando multimedia Xbox, Cortana (especialmente los comandos de voz "Reproducir" y "Pausa") y Xbox SmartGlass. Para obtener estas características de forma gratuita, la aplicación debe usar la clase [SystemMediaTransportControls](https://msdn.microsoft.com/library/windows/apps/windows.media.systemmediatransportcontrols.aspx), que se incluye automáticamente en los controles multimedia de Xbox. Si la aplicación tiene controles multimedia personalizados, asegúrate de integrarlos con la clase **SystemMediaTransportControls** para poder proporcionar estas características a los usuarios. Si vas a crear una aplicación de música en segundo plano, intégrala con la clase **SystemMediaTransportControls** para asegurarte de que los controles de música en segundo plano funcionan correctamente en la pestaña de multitarea de Xbox.

<!-- ### *Do:* Use adaptive UI to account for snapped apps
One of the unique features of Xbox One is that users can snap apps such as Cortana next to any other app, so your app should respond gracefully when it runs in *fill mode*. Implement [adaptive UI](../get-started/universal-application-platform-guide.md#design-adaptive-ui-with-adaptive-panels) and make sure to test your app during development by snapping an app next to it. -->

### <a name="consider-draw-to-the-edge-of-the-screen"></a>*A tener en cuenta:* dibujar hasta el borde de la pantalla

Muchos televisores cortan los bordes de la pantalla, por lo que todo el contenido de importancia de la aplicación debería mostrarse en la [zona segura del televisor](../design/devices/designing-for-tv.md#tv-safe-area). UWP usa la opción de *sobrebarrido* para mantener el contenido dentro de la zona segura del televisor, pero ten en cuenta que este comportamiento predeterminado puede dibujar un borde alrededor de la aplicación que puede resultar obvio. Para proporcionar la mejor experiencia, desactiva el comportamiento predeterminado y sigue las instrucciones que se detallan en [Cómo dibujar la interfaz de usuario hasta el borde de la pantalla](turn-off-overscan.md).
> [!IMPORTANT]
  > Si deshabilitas la opción de sobrebarrido, tienes que asegurarte de que los elementos interactivos y el texto permanezcan dentro de la zona segura del televisor. 

### <a name="consider-use-tv-safe-colors"></a>*A tener en cuenta:* usa colores seguros para el televisor

Los televisores no administran las intensidades extremas de color tan bien como los monitores. Evita los colores de gran intensidad en la aplicación para que así los usuarios no vean franjas extrañas o imágenes difuminadas. Asimismo, ten en cuenta las diferencias existentes entre televisores: es posible que aquellos colores que se vean perfectamente en *tu* televisión, no se vean tan bien en las de los usuarios. Leer [los colores](../design/devices/designing-for-tv.md#colors) para comprender cómo hacer que la aplicación tengan un aspecto estupendo para todo el mundo.

### <a name="remember-you-can-disable-scaling"></a>*Recuerda:* puedes deshabilitar el escalado

Las aplicaciones para UWP se escalan automáticamente para garantizar que los elementos de interfaz de usuario (como los controles y las fuentes) sean legibles en todos los dispositivos. Aquellas aplicaciones que usan XAML se escalan en un 200 %, mientras que las aplicaciones que usan HTML se escalan en un 150 %. Si quieres controlar mejor el aspecto de tu aplicación en Xbox, deshabilita el factor de escala predeterminado y usa las dimensiones en píxeles reales de una HDTV (1920 x 1080). Si quieres obtener información acerca de cómo conseguir que tu aplicación tenga un aspecto genial en Xbox, échale un vistazo a [Cómo desactivar el ajuste de escala](disable-scaling.md) y [Escalado y píxeles efectivos](../design/basics/design-and-ui-intro.md#effective-pixels-and-scaling).

Si quieres hacerte una idea de cómo aplicar estas prácticas a una aplicación para UWP, ¡echa un vistazo a este vídeo!
</br>
</br>
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Tailoring-your-UWP-app-for-Xbox/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="channel-9"></a>Channel 9

Las siguientes conversaciones que encontrarás en [Channel 9](https://channel9.msdn.com/) te serán de gran ayuda para crear aplicaciones sorprendentes en Xbox:

- [Building Great Universal Windows Platform (UWP) Apps for Xbox (Crear excelentes aplicaciones para la Plataforma universal de Windows para Xbox)](https://channel9.msdn.com/Events/Build/2016/B883)
- [Adapt Your App for Xbox One and TV (Adapta tu aplicación para Xbox One y el televisor)](https://channel9.msdn.com/Events/Build/2016/T651-R1)
- [UWP Development 1: Building an Adaptive UI (Desarrollo UWP 1: crear una interfaz de usuario adaptable)](https://channel9.msdn.com/Events/Build/2016/L724-R1)
- [Web Apps Beyond the Browser: Cross-Platform Meets Cross Device (Aplicaciones web más allá del navegador: opción multiplataforma para todos los dispositivos)](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="app-dev-on-xbox"></a>Desarrollo de aplicaciones en Xbox

El evento de **Desarrollo de aplicaciones en Xbox** es un buen punto de partida para los desarrolladores nuevos en la creación de aplicaciones en Xbox.

* [Mira las sesiones grabadas](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#WatchNow)
* [Lee las entradas de blog](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#BlogSeries)

## <a name="see-also"></a>Ver también

- [UWP en Xbox One](index.md)
- [Diseño para Xbox y televisión](../design/devices/designing-for-tv.md)
- [Aplicaciones Web progresivas para Xbox One](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/xbox-considerations)
