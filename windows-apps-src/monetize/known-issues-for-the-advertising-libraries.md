---
author: mcleanbyron
ms.assetid: 9ca1f880-2ced-46b4-8ea7-aba43d2ff863
description: "Aprende sobre los problemas conocidos de la versión actual de las bibliotecas de Microsoft Advertising en el SDK de Microsoft Store Engagement and Monetization."
title: Problemas conocidos de las bibliotecas de Microsoft Advertising
ms.sourcegitcommit: 5b6d3e034b73e6ae693fbeab3ecd3b2b81f38bb1
ms.openlocfilehash: cfaa108cc93b6bae903e86ad141656bf613f185d

---

# Problemas conocidos de las bibliotecas de Microsoft Advertising


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este tema se enumeran los problemas conocidos de la versión actual de las bibliotecas de Microsoft Advertising en el SDK de Microsoft Store Engagement and Monetization.

## La instalación requiere Visual Studio Tools para aplicaciones universales de Windows

Para instalar el [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk) con Visual Studio 2015, debes tener instalada la versión 1.1 o posterior de Visual Studio Tools para aplicaciones universales de Windows. Para obtener más información, consulta las [notas de la versión](http://go.microsoft.com/fwlink/?LinkID=624516) de Visual Studio.

## Proyectos de Windows Phone 8.x Silverlight

Para obtener los ensamblados de Microsoft Advertising para proyectos de Windows Phone 8.x Silverlight, instala el [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk), abre el proyecto en Visual Studio y después ve a **Proyecto** > **Agregar servicio conectado** > **Ad Mediator** para descargar automáticamente los ensamblados. Después de hacer esto, puedes quitar las referencias de Ad Mediator del proyecto si no quieres usar la mediación de anuncios. Para obtener más información, consulta [AdControl in Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md) (AdControl en Windows Phone Silverlight).

## Interfaz de AdControl desconocida en XAML

El marcado XAML de un [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) puede mostrar incorrectamente una línea curva azul que indica que la interfaz es desconocida. Esto ocurre solo cuando el destino es x86 y se puede ignorar.

## lastError de una solicitud de anuncio anterior

Si hay un **lastError** sobrante de la solicitud de anuncio anterior, se puede desencadenar el evento dos veces en la siguiente llamada al anuncio. Si bien la nueva solicitud del anuncio se realizará y puede producir un anuncio válido, este comportamiento puede causar confusión.

## Botones de navegación y anuncios intersticiales en los teléfonos

En los teléfonos (o emuladores) que tienen botones de software **Atrás**, **Inicio** y **Búsqueda**, en lugar de botones de hardware, es posible que los botones de temporizador y de click-through para los anuncios intersticiales en vídeo estén ocultos.

## Los anuncios creados recientemente no se envían a la aplicación

Si has creado un anuncio recientemente (menos de un día), podría no estar disponible de inmediato. Si el anuncio ha sido aprobado para contenido editorial, se enviará una vez que el servidor de publicidad lo haya procesado y el anuncio esté disponible como inventario.

## No se muestran anuncios en la aplicación

Existen muchos motivos para que no veas anuncios, incluidos errores de red. Entre otros motivos se incluyen:

* Seleccionar una unidad de anuncios en el Centro de desarrollo de Windows con un tamaño mayor o menor que el tamaño de **AdControl** en el código de la aplicación.

* Los anuncios no aparecerán si usas un [valor del modo de prueba](test-mode-values.md) para tu identificador de unidad de anuncio cuando se ejecuta una aplicación dinámica.

* Si has creado un nuevo identificador de unidad de anuncio en última media hora, puede que no veas un anuncio hasta que los servidores propaguen nuevos datos a través del sistema. Los identificadores existentes que han mostrado anuncios antes deberían mostrar anuncios de inmediato.

Si puedes ver anuncios de prueba en la aplicación, el código funciona y es capaz de mostrar anuncios. Si tienes problemas, ponte en contacto con [soporte técnico](https://go.microsoft.com/fwlink/p/?LinkId=331508). En esa página, elige **Publicidad en la aplicación**.

También puedes publicar una pregunta en el [foro](http://go.microsoft.com/fwlink/p/?LinkId=401266).

## En la aplicación se muestran anuncios de prueba, en lugar de anuncios dinámicos

Pueden mostrarse anuncios de prueba, incluso cuando esperas anuncios dinámicos. Esto puede suceder en los siguientes escenarios:

* Microsoft Advertising no puede comprobar o encontrar el identificador de la aplicación dinámica usado en la tienda de la aplicación. En este caso, cuando un usuario crea una unidad de anuncio, su estado puede empezar como dinámico (no de prueba), pero se moverá al estado de prueba dentro de las 6 horas posteriores a la primera solicitud del anuncio. Volverá a ser dinámico si no hay ninguna solicitud de las aplicaciones de prueba durante 10 días.

* Las aplicaciones de prueba o las aplicaciones que se ejecutan en el emulador no mostrará anuncios dinámicos.

Cuando una unidad de anuncios dinámicos envía anuncios de prueba, el estado de la unidad de anuncios muestra **Active and serving test ads** en el Centro de desarrollo de Windows. Esto no corresponde actualmente a las aplicaciones para teléfonos.

## Los valores de prueba obsoletos para el identificador de unidad de anuncios y el identificador de aplicación ya no funcionan

Los siguientes valores de prueba para aplicaciones de Windows Phone Silverlight están obsoletos y dejarán de funcionar. Si tienes un proyecto existente que usa estos valores de prueba, actualiza el proyecto para usar los valores proporcionados en [Test mode values](test-mode-values.md) (Valores del modo de prueba).

| Id. de aplicación  |  Id. de unidad de anuncio    |
|-----------------|----------------|
| test_client     |  Image320_50   |
| test_client     |  Image300_50   |
| test_client     |  TextAd   |
| test_client     |  Image480_80   |

<span id="reference_errors"/>
## Errores de referencia provocados por dirigirse a Cualquier CPU en el proyecto

Al usar las bibliotecas de Microsoft Advertising, no puedes dirigirte a **Cualquier CPU** en tu proyecto. Si el proyecto se dirige a la plataforma **Cualquier CPU**, puede que veas una advertencia después de agregar la referencia similar a esta.

![referenceerror\-solutionexplorer](images/13-19629921-023c-42ec-b8f5-bc0b63d5a191.jpg)

Para quitar esta advertencia, actualiza el proyecto para usar una salida de compilación específica de la arquitectura (por ejemplo, **x86**). Usa **Configuration Manager** para establecer los destinos de la plataforma para configuraciones de depuración y lanzamiento.

![configurationmanagerwin10](images/13-87074274-c10d-4dbd-9a06-453b7184f8de.png)

Cuando crees los paquetes de la aplicación para el envío a la tienda (como se muestra en las siguientes imágenes), asegúrate de incluir las arquitecturas que quieres como destino. Puedes optar por omitir x64 si vas a ejecutar compilaciones x86 en el sistema operativo x64.

![projectstorecreateapppackages](images/13-a99b05a4-8917-4c53-822e-2548fadf828a.png)

![createapppackages](images/13-16280cb1-a838-42b9-9256-eac7f33f5603.png)

## Orden Z en las aplicaciones de JavaScript/HTML

Las aplicaciones de JavaScript/HTML no deben colocar elementos en el intervalo MAX-10 reservado de orden z. La única excepción es una superposición de interrupción, como una notificación de llamada entrante para una aplicación de Skype.

<span id="bkmk-ui"/>
## No uses bordes

Establecer las propiedades relacionadas con los bordes heredados por **AdControl** de su clase primaria hará que la colocación del anuncio sea incorrecta.

## Más información


Para obtener más información acerca de los últimos problemas conocidos y para publicar preguntas relacionadas con las bibliotecas de Microsoft Advertising, visita el [foro](http://go.microsoft.com/fwlink/p/?LinkId=401266).

## Soporte técnico


Para ponerte en contacto con soporte técnico para problemas relacionados con las bibliotecas de Microsoft Advertising, visita la [página de soporte técnico](https://go.microsoft.com/fwlink/p/?LinkId=331508) y elige **Publicidad en la aplicación**.

 

 



<!--HONumber=Jun16_HO4-->


