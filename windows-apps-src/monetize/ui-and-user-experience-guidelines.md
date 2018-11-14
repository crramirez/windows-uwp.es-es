---
author: Xansky
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: Obtén las directrices de la interfaz de usuario y de la experiencia del usuario para los anuncios en aplicaciones.
title: Directrices de la interfaz de usuario y de la experiencia del usuario para anuncios
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, Windows 10, uwp, UWP, ads, anuncios, advertising, publicidad, guidelines, directrices, best practices, procedimientos recomendados
ms.localizationpriority: medium
ms.openlocfilehash: 4d502c721f98269c1256510a6f91f8c6dc8cd0fb
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6649819"
---
# <a name="ui-and-user-experience-guidelines-for-ads"></a>Directrices de la interfaz de usuario y de la experiencia del usuario para anuncios

En este artículo se proporcionan directrices para ofrecer magníficas experiencias con anuncios de banner, anuncios intersticiales y anuncios nativos en las aplicaciones. Para obtener instrucciones generales sobre cómo diseñar la apariencia de las aplicaciones, consulta [Diseño e interfaz de usuario](https://developer.microsoft.com/windows/apps/design).

> [!IMPORTANT]
> Todo uso de publicidad en tu aplicación debe regirse por las Directivas de Microsoft Store, que incluyen, entre otras, la [directiva 10.10](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) (Conducta y contenido de publicidad). En concreto, la implementación de anuncios de banner o anuncios intersticiales de la aplicación debe cumplir los requisitos de directiva de Microsoft Store [directiva 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content). En este artículo se incluyen ejemplos de implementaciones que infringirían dicha directiva. Tales ejemplos se proporcionan para fines informativos solamente, como una forma de ayudarte a comprender mejor la directiva. Estos ejemplos no son integrales y es posible que haya muchas otras formas de infringir las Directivas de Microsoft Store que no se muestran en este artículo.

## <a name="general-best-practices"></a>Procedimientos recomendados generales

Antes de revisar nuestras directrices para diferentes tipos de anuncios en este artículo, revisa estos procedimientos recomendados generales para mejorar tus ingresos por anuncios.

* [Planear la colocación de anuncios detenidamente](https://blogs.windows.com/buildingapps/2017/04/10/monetizing-app-advertisement-placement/). Consulta nuestras directrices relacionadas acerca de la [optimización la capacidad de visualización de las unidades de anuncio](optimize-ad-unit-viewability.md).
* [Usar anuncios de banner intersticiales como reserva para anuncios intersticiales en vídeo](https://blogs.windows.com/buildingapps/2017/04/17/monetizing-app-use-interstitial-banner-fallback-interstitial-video).
* [Conocer a los usuarios para proporcionar anuncios más dirigidos](https://blogs.windows.com/buildingapps/2017/05/17/monetize-app-know-user-serve-better-targeted-ads/).
* [Usar las bibliotecas de publicidad más recientes](https://blogs.windows.com/buildingapps/2017/05/22/earn-money-moving-latest-advertising-libraries/).
* [Establecer la configuración de COPPA correcta para tu aplicación](https://blogs.windows.com/buildingapps/2017/06/21/monetizing-app-set-coppa-settings-app).


## <a name="guidelines-for-banner-ads"></a>Directrices para anuncios de banner

En las siguientes secciones se proporcionan recomendaciones sobre cómo implementar [anuncios de banner](banner-ads.md) en tu aplicación mediante [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) y ejemplos de implementaciones que infringen la [directiva 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) de las Directivas de Microsoft Store.

### <a name="best-practices"></a>Procedimientos recomendados

Te recomendamos que sigas los siguientes procedimientos recomendados a la hora de implementar anuncios de banner en tu aplicación:

* [Usar tamaños de Interactive Advertising Bureau](https://blogs.windows.com/buildingapps/2017/04/03/monetizing-app-use-interactive-advertising-bureau-ad-sizes) que se ajusten correctamente al diseño del dispositivo.

* Dedica la mayor parte de la interfaz de usuario de la aplicación al contenido y los controles funcionales.

* Diseña la publicidad en la experiencia. Ofrece a tus diseñadores un anuncio de muestra para planear el aspecto que tendrá la publicidad. Dos ejemplos de anuncios bien planeados en las aplicaciones son el diseño de anuncios como contenido y el diseño de división.

  Para ver el aspecto y el funcionamiento de los distintos tamaños de anuncio en la aplicación, puedes usar nuestras [unidades de anuncios de prueba](set-up-ad-units-in-your-app.md#test-ad-units) durante la etapa de desarrollo y pruebas. Cuando termines con las unidades de anuncio del modo de prueba, recuerda [actualizar la aplicación con unidades de anuncios dinámicos](set-up-ad-units-in-your-app.md#live-ad-units) antes de enviar la aplicación para su certificación.

* Planea las ocasiones en que no haya anuncios disponibles. Puede que haya ocasiones en que no se envíen anuncios a la aplicación. Diseña las páginas de manera que tengan un magnífico aspecto tanto si muestran un anuncio como si no. Para obtener más información, consulta [Controlar errores de anuncios](error-handling-with-advertising-libraries.md).

* Si tienes un escenario para alertar al usuario que se controla mejor con una superposición, haz una llamada a [AdControl.Suspend](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend) mientras se muestra la superposición y, luego, a [AdControl.Resume](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.resume) cuando termine el escenario de alerta.

### <a name="practices-to-avoid"></a>Procedimientos que deben evitarse

Te recomendamos que evites los siguientes procedimientos cuando implementes anuncios de banner en la aplicación:

* No colocar publicidad en la superficie abierta. El espacio de anuncios no debe colocarse en el primer espacio abierto de superficie que encuentres. En su lugar, debe incluirse en el diseño general de la aplicación.

* No incluir publicidad excesiva ni saturar la aplicación. Demasiados anuncios en la aplicación desmerecerán su apariencia y facilidad de uso. El objetivo de la publicidad es ganar dinero, pero no a costa de la propia aplicación.

* No distraer al usuario de sus tareas principales. La aplicación siempre debe ser el foco principal. El espacio de anuncios debe incluirse de modo que sea un foco secundario.

### <a name="examples-of-policy-violations"></a>Ejemplos de infracciones de directivas

En esta sección se proporcionan ejemplos de escenarios de anuncios de banner que infringen la [directiva 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) de las Directivas de Microsoft Store. Tales ejemplos se proporcionan para fines informativos solamente, como una forma de ayudarte a comprender mejor la directiva. Estos ejemplos no son integrales y es posible que haya muchas otras formas de infringir la directiva 10.10.1 que no se muestran aquí.

* Interferir de algún modo con la capacidad del usuario para ver el anuncio de banner, por ejemplo, cambiando la opacidad de la clase [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) o colocar otro control por encima de **AdControl** (sin antes hacer una llamada a [AdControl.Suspend](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend)).

* Requerir que los usuarios hagan clic en un banner para realizar una tarea en la aplicación u obligarlos a hacer clic en anuncios de banner como resultado del diseño de la aplicación.

* Omitir, de cualquier manera, el temporizador de actualización mínima integrado para anuncios de banner, lo que incluye, entre otros, intercambiar objetos [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) o forzar una actualización de la página sin interacción del usuario.

* Con unidades de anuncios dinámicos (es decir, unidades de anuncios que se obtienen del centro de partners) durante el desarrollo y pruebas, o en un emulador.

* Escribir o distribuir código que llame a los servicios de anuncio a través de medios que no sean las bibliotecas de Microsoft Advertising que se ejecutan en el contexto de la aplicación.

* Interactuar con interfaces no documentadas u objetos secundarios creados por las bibliotecas de Microsoft Advertising, por ejemplo, **WebView** o **MediaElement**.

<span id="interstitialbestpractices10" />

## <a name="guidelines-for-interstitial-ads"></a>Directrices para anuncios intersticiales

Si se usan con elegancia, los [anuncios intersticiales](interstitial-ads.md) pueden aumentar considerablemente los ingresos de la aplicación, sin incidir de forma negativa en la satisfacción del usuario. Si no se usan correctamente, estos anuncios pueden tener el efecto totalmente contrario.

En las siguientes secciones se proporcionan recomendaciones sobre cómo implementar anuncios de vídeo intersticial y de banner intersticial en la aplicación con la clase [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) y ejemplos de implementaciones que infringen la [directiva 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) de las Directivas de Microsoft Store. Dado que conoces tu aplicación mejor que nadie, excepto en lo que a directivas se refiere, la decisión final está en tus manos. Lo más importante que debes tener en cuenta es que las clasificaciones de la aplicación y los ingresos están estrechamente relacionados.

### <a name="best-practices"></a>Procedimientos recomendados

Te recomendamos que sigas los siguientes procedimientos recomendados a la hora de implementar anuncios intersticiales en tu aplicación:

* Coloca los anuncios intersticiales en el flujo natural de la aplicación, por ejemplo, entre los niveles del juego.

* Asocia los anuncios con ventajas tangibles, como:

    * Sugerencias para la finalización del nivel.

    * Tiempo extra para reintentar un nivel.

    * Características de avatar personalizadas, como un tatuaje o un sombrero.

* Si la aplicación exige la visualización de un anuncio de vídeo intersticial hasta el final, menciona esa regla de antemano para que no vean un mensaje de error por sorpresa al pulsar el botón Cerrar.

* Realiza la captura previa del anuncio (mediante una llamada a [InterstitialAd.RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)). Lo ideal es hacerla entre 30 y 60 segundos antes de mostrarlo.

* Suscríbete a los cuatro eventos que se exponen en la clase [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) (**Cancelled**, **Completed**, **AdReady** y **ErrorOccurred**) y úsalos para tomar las decisiones correctas para tu aplicación.

* Ten alguna funcionalidad integrada para usarla en lugar de un anuncio asociado al servidor. La encontrarás útil en algunos escenarios:

    * Modo sin conexión, si no se puede acceder a los servidores de anuncios.

    * Si se desencadena el evento **ErrorOccurred**.

    * Si optas por ahorrar ancho de banda del usuario en función de [ConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile), existen algunas API en la clase **ConnectionProfile** que pueden resultarte útiles.

* Usa el tiempo de espera predeterminado (30 segundos), a menos que haya una razón válida para cambiarlo, pero no uses menos de 10 segundos. Los anuncios intersticiales tardan mucho más tiempo en descargarse que los anuncios de banner estándar, especialmente en los mercados que no tienen conexiones de alta velocidad.

* Ten en cuenta el plan de datos del usuario. Por ejemplo, no lo muestres o advierte al usuario, antes de presentar un anuncio de vídeo intersticial en un dispositivo móvil que supere o se acerque al límite de datos. Existen algunas API en la clase [ConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) que pueden resultarte útiles.

* Mejora continuamente tu aplicación después del envío inicial. Consulta los [informes de anuncios](../publish/advertising-performance-report.md) y realiza cambios de diseño para mejorar las velocidades de relleno y finalización de vídeo intersticial.

### <a name="practices-to-avoid"></a>Procedimientos que deben evitarse

Te recomendamos que evites los siguientes procedimientos cuando implementes anuncios intersticiales en la aplicación:

* No te excedas. No muestres anuncios con una frecuencia superior a cada 5 minutos, a menos que el usuario obtenga explícitamente una ventaja tangible opcional, además de poder jugar al juego.

* No muestres anuncios intersticiales al inicio de la aplicación, porque los usuarios pueden pensar que han hecho clic en el icono incorrecto.

* No muestres anuncios intersticiales al salir. Este inventario no es adecuado, ya que los índices de finalización se aproximarán a cero.

* No muestres dos o más anuncios intersticiales consecutivos. Los usuarios se frustrarán al ver que la barra de progreso del anuncio vuelve al punto de partida. Muchos pensarán que se trata de un error de codificación o de presentación del anuncio.

* No captures un anuncio de vídeo intersticial más de cinco minutos antes de la llamada al método [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show). Un buen inventario maximizará la conversión de anuncios capturados previamente en impresiones facturables.

* No penalices al usuario por errores en la presentación de anuncios, como la ausencia de anuncios disponibles. Por ejemplo, si muestras una opción de la interfaz de usuario destinada a "Ver un anuncio para obtener *xxx*", debes proporcionar *xxx* si el usuario hizo su parte. Existen dos opciones que debes tener en cuenta:

    * No se debe incluir la opción a menos que se desencadene el evento [InterstitialAd.AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready).

    * La aplicación debe incluir una funcionalidad integrada que genere el mismo beneficio que un anuncio real.

* No uses anuncios intersticiales para permitir que el usuario obtenga una ventaja competitiva en un juego de varios jugadores. Por ejemplo, no seduzcas al usuario con un arma de fuego superior en un juego de tiros en primera persona a condición de que vea un anuncio intersticial. Una camisa personalizada para el avatar del jugador está bien, siempre y cuando no proporcione camuflaje.

### <a name="examples-of-policy-violations"></a>Ejemplos de infracciones de directivas

En esta sección se proporcionan ejemplos de escenarios de anuncios intersticiales que infringen la [directiva 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) de las Directivas de Microsoft Store. Tales ejemplos se proporcionan para fines informativos solamente, como una forma de ayudarte a comprender mejor la directiva. Estos ejemplos no son integrales y es posible que haya muchas otras formas de infringir la directiva 10.10.1 que no se muestran aquí.

* Colocar un elemento de interfaz de usuario sobre el contenedor de anuncio intersticial.

* Hacer una llamada a [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) mientras el usuario está ocupado con la aplicación.

* Usar anuncios intersticiales para obtener todo lo que pueda usarse como moneda o negociarse con otros usuarios.

* Solicitar un anuncio intersticial nuevo en el contexto del controlador de eventos para el evento [InterstitialAd.ErrorOccurred](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.erroroccurred). Esto puede ocasionar un bucle infinito y causar problemas de funcionamiento del servicio de publicidad.

* Solicitar un anuncio intersticial solo para tener un anuncio de copia de seguridad para una secuencia de anuncios en cascada. Si solicitas un anuncio intersticial y luego recibes el evento [InterstitialAd.AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready), el siguiente anuncio intersticial que se muestre en la aplicación debe ser el anuncio que está listo para mostrarse a través del método [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

* Con unidades de anuncios dinámicos (es decir, unidades de anuncios que se obtienen del centro de partners) durante el desarrollo y pruebas, o en un emulador.

* Escribir o distribuir código que llame a los servicios de anuncio a través de medios que no sean las bibliotecas de Microsoft Advertising que se ejecutan en el contexto de la aplicación.

* Interactuar con interfaces no documentadas u objetos secundarios creados por las bibliotecas de Microsoft Advertising, por ejemplo, **WebView** o **MediaElement**.

## <a name="guidelines-for-native-ads"></a>Directrices para anuncios nativos

Los [anuncios nativos](native-ads.md) te proporcionan mucho control sobre la manera en que presentas el contenido de publicidad a los usuarios. Sigue estos requisitos y directrices para ayudar a garantizar que el mensaje del anunciante llega a los usuarios a la vez que se ayuda a evitar ofrecer una experiencia de anuncios nativos confusa a los usuarios.

### <a name="register-the-container-for-your-native-ad"></a>Registrar el contenedor para el anuncio nativo

En el código, debes llamar al método [RegisterAdContainer](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2.registeradcontainer) del objeto [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) para registrar el elemento de interfaz de usuario que actúa como contenedor para el anuncio nativo y, opcionalmente, los controles que quieras registrar como destinos seleccionables para el anuncio. Esto es necesario para realizar un seguimiento adecuado de los clics y de las impresiones de anuncios.

Hay dos sobrecargas para el método **RegisterAdContainer** que puedes usar:

* Si quieres que sea seleccionable el contenedor completo para todos los elementos de anuncios nativos individuales, llama al método **RegisterAdContainer(FrameworkElement)** y pasa el control del contenedor al método. Por ejemplo, si muestras todos los elementos de anuncios nativos en controles independientes que se hospedan en un **StackPanel** y quieres que sea seleccionable todo el **StackPanel**, pasa el **StackPanel** a este método.

* Si quieres que solo sean seleccionables determinados elementos de anuncios nativos, llama al método **RegisterAdContainer(FrameworkElement, IVector(FrameworkElement))**. Solo serán seleccionables los controles que pases al segundo parámetro.

### <a name="required-native-ad-elements"></a>Elementos de anuncios nativos necesarios

Como mínimo, siempre debes mostrar los siguientes elementos de anuncios nativos proporcionados por las propiedades del objeto [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) al usuario en el diseño de anuncio nativo. Si no incluyes estos elementos, puedes ver un rendimiento deficiente y bajas rentabilidades para tu unidad de anuncio.

1. Muestra siempre el título del anuncio nativo (disponible en la propiedad **Título**). Proporciona espacio suficiente para mostrar 25 caracteres como mínimo. Si el título es más largo, reemplaza el texto adicional por puntos suspensivos.
2. Muestra siempre al menos uno de los siguientes elementos para ayudar a diferenciar la experiencia de anuncios nativos del resto de la aplicación y destacar claramente que el contenido lo proporciona un anunciante:
    * El icono *ad* diferenciable (disponible en la propiedad **AdIcon**). Este icono lo proporciona Microsoft.
    * El texto *patrocinado por* (disponible en la propiedad **SponsoredBy**). Este texto lo proporciona el anunciante.
    * Como alternativa al texto *patrocinado por*, puedes elegir mostrar algún otro texto que ayude a diferenciar la experiencia de anuncios nativos del resto de la aplicación, como "Contenido patrocinado", "Contenido promocional", "Contenido recomendado", etc.

### <a name="user-experience"></a>Experiencia del usuario

El anuncio nativo se debe delinear claramente del resto de la aplicación y tiene un espacio alrededor para impedir los clics accidentales. Usa bordes, diferentes fondos o alguna otra interfaz de usuario para separar el contenido del anuncio del resto de la aplicación. Ten en cuenta que los clics en anuncios accidentales no son beneficios para tus ingresos basados en anuncios o tu experiencia de usuario final a largo plazo.

### <a name="description"></a>Descripción

Si eliges mostrar la descripción para la propiedad (disponible en la **Descripción** del anuncio del objeto **NativeAdV2**), proporciona suficiente espacio para mostrar 75 caracteres como mínimo. Te recomendamos que uses una animación para mostrar el contenido completo de la descripción del anuncio.

### <a name="call-to-action"></a>Llamada a la acción

El texto de la *llamada a la acción* (disponible en la propiedad **CallToAction** del objeto **NativeAdV2**) es un componente fundamental del anuncio. Si decides mostrar este texto, sigue estas directrices:

* Muestra siempre el texto de la *llamada a la acción* al usuario en un control interactivo como un botón o hipervínculo.
* Muestra siempre el texto de la *llamada a la acción* en su totalidad.
* Asegúrate de que el texto de la *llamada a la acción* es independiente del resto del texto promocional del anunciante.
