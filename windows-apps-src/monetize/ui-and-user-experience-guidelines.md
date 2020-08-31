---
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: Consulte las instrucciones para proporcionar excelentes experiencias de usuario y de interfaz de usuario con anuncios de Banner, anuncios intersticiales y anuncios nativos en sus aplicaciones.
title: Instrucciones para la experiencia del usuario y la interfaz de usuario para anuncios
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, directrices, procedimientos recomendados
ms.localizationpriority: medium
ms.openlocfilehash: 014cc5a7be370ed5fd579af9bc4ab7600cb96689
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158359"
---
# <a name="ui-and-user-experience-guidelines-for-ads"></a>Instrucciones para la experiencia del usuario y la interfaz de usuario para anuncios

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

En este artículo se proporcionan instrucciones para proporcionar excelentes experiencias con anuncios de Banner, anuncios intersticiales y anuncios nativos en sus aplicaciones. Para obtener instrucciones generales sobre cómo diseñar la apariencia de las aplicaciones, consulta [Diseño e interfaz de usuario](https://developer.microsoft.com/windows/apps/design).

> [!IMPORTANT]
> Cualquier uso de publicidad en la aplicación debe cumplir las directivas de Microsoft Store, incluidas, sin limitación, la [directiva 10,10](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) (realización de publicidad y contenido). En concreto, la implementación de la aplicación de anuncios de banner o anuncios intersticiales debe cumplir los requisitos de la Directiva de directiva de Microsoft Store [10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content). En este artículo se incluyen ejemplos de implementaciones que infringirían dicha directiva. Tales ejemplos se proporcionan para fines informativos solamente, como una forma de ayudarte a comprender mejor la directiva. Estos ejemplos no son completos y puede haber muchas otras formas de infringir las directivas de Microsoft Store que no se enumeran en este artículo.

## <a name="general-best-practices"></a>Procedimientos recomendados generales

Antes de revisar las instrucciones para los distintos tipos de anuncios de este artículo, revise primero estas prácticas recomendadas generales para mejorar los ingresos de anuncios.

* [Planifique sus ubicaciones de anuncios con cuidado](https://blogs.windows.com/buildingapps/2017/04/10/monetizing-app-advertisement-placement/). Vea las instrucciones relacionadas sobre cómo [optimizar la capacidad de visualización de las unidades de anuncios](optimize-ad-unit-viewability.md).
* [Use anuncios de banners intersticiales como reserva para anuncios de vídeo intersticial](https://blogs.windows.com/buildingapps/2017/04/17/monetizing-app-use-interstitial-banner-fallback-interstitial-video/).
* [Sepa que los usuarios atienden mejores anuncios dirigidos](https://blogs.windows.com/buildingapps/2017/05/17/monetize-app-know-user-serve-better-targeted-ads/).
* [Use las bibliotecas de publicidad más recientes](https://blogs.windows.com/buildingapps/2017/05/22/earn-money-moving-latest-advertising-libraries/).
* [Establezca la configuración de COPPA correcta para la aplicación](https://blogs.windows.com/buildingapps/2017/06/21/monetizing-app-set-coppa-settings-app/).


## <a name="guidelines-for-banner-ads"></a>Directrices para anuncios de banner

En las secciones siguientes se proporcionan recomendaciones sobre cómo implementar [anuncios de banner](banner-ads.md) en la aplicación con [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) y ejemplos de implementaciones que infringen la [Directiva 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) de las directivas de Microsoft Store.

### <a name="best-practices"></a>Procedimientos recomendados

Te recomendamos que sigas los siguientes procedimientos recomendados a la hora de implementar anuncios de banner en tu aplicación:

* [Use tamaños de oficina de publicidad interactivos](https://blogs.windows.com/buildingapps/2017/04/03/monetizing-app-use-interactive-advertising-bureau-ad-sizes/) que se ajusten correctamente con el diseño del dispositivo.

* Dedica la mayor parte de la interfaz de usuario de la aplicación al contenido y los controles funcionales.

* Diseña la publicidad en la experiencia. Ofrece a tus diseñadores un anuncio de muestra para planear el aspecto que tendrá la publicidad. Dos ejemplos de anuncios bien planeados en las aplicaciones son el diseño de anuncios como contenido y el diseño de división.

  Para ver el aspecto y el funcionamiento de los distintos tamaños de ad dentro de la aplicación durante la fase de desarrollo y pruebas, puede emplear nuestras [unidades de anuncio de prueba](set-up-ad-units-in-your-app.md#test-ad-units). Cuando haya terminado de realizar las pruebas, recuerde [actualizar la aplicación con las unidades de anuncios en directo](set-up-ad-units-in-your-app.md#live-ad-units) antes de enviar la aplicación para su certificación.

* Planea las ocasiones en que no haya anuncios disponibles. Puede que haya ocasiones en que no se envíen anuncios a la aplicación. Diseña las páginas de manera que tengan un magnífico aspecto tanto si muestran un anuncio como si no. Para obtener más información, consulte [control de errores de ad](error-handling-with-advertising-libraries.md).

* Si tienes un escenario para alertar al usuario que se controla mejor con una superposición, haz una llamada a [AdControl.Suspend](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend) mientras se muestra la superposición y, luego, a [AdControl.Resume](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.resume) cuando termine el escenario de alerta.

### <a name="practices-to-avoid"></a>Procedimientos que deben evitarse

Te recomendamos que evites los siguientes procedimientos cuando implementes anuncios de banner en la aplicación:

* No colocar publicidad en la superficie abierta. El espacio de anuncios no debe colocarse en el primer espacio abierto de superficie que encuentres. En su lugar, debe incluirse en el diseño general de la aplicación.

* No incluir publicidad excesiva ni saturar la aplicación. Demasiados anuncios en la aplicación desmerecerán su apariencia y facilidad de uso. El objetivo de la publicidad es ganar dinero, pero no a costa de la propia aplicación.

* No distraer al usuario de sus tareas principales. La aplicación siempre debe ser el foco principal. El espacio de anuncios debe incluirse de modo que sea un foco secundario.

### <a name="examples-of-policy-violations"></a>Ejemplos de infracciones de directivas

En esta sección se proporcionan ejemplos de escenarios de banner que infringen la [Directiva 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) de las directivas de Microsoft Store. Tales ejemplos se proporcionan para fines informativos solamente, como una forma de ayudarte a comprender mejor la directiva. Estos ejemplos no son integrales y es posible que haya muchas otras formas de infringir la directiva 10.10.1 que no se muestran aquí.

* Interferir de algún modo con la capacidad del usuario para ver el anuncio de banner, por ejemplo, cambiando la opacidad de la clase [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) o colocar otro control por encima de **AdControl** (sin antes hacer una llamada a [AdControl.Suspend](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend)).

* Requerir que los usuarios hagan clic en un banner para realizar una tarea en la aplicación u obligarlos a hacer clic en anuncios de banner como resultado del diseño de la aplicación.

* Omitir, de cualquier manera, el temporizador de actualización mínima integrado para anuncios de banner, lo que incluye, entre otros, intercambiar objetos [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) o forzar una actualización de la página sin interacción del usuario.

* Usar unidades de anuncio en directo (es decir, unidades de anuncios que se obtienen del centro de Partners) durante el desarrollo y las pruebas, o en un emulador.

* Escribir o distribuir código que llame a los servicios de anuncio a través de medios que no sean las bibliotecas de Microsoft Advertising que se ejecutan en el contexto de la aplicación.

* Interactuar con interfaces no documentadas u objetos secundarios creados por las bibliotecas de Microsoft Advertising, por ejemplo, **WebView** o **MediaElement**.

* Colocar anuncios en un Viewbox para reducir el tamaño de los anuncios con el fin de permitir más anuncios en una página de lo normal.

<span id="interstitialbestpractices10" />

## <a name="guidelines-for-interstitial-ads"></a>Directrices para anuncios intersticiales

Cuando se usa de forma elegante, los [anuncios intersticiales](interstitial-ads.md) pueden aumentar enormemente los ingresos de la aplicación, sin afectar negativamente a la satisfacción del usuario. Si no se usan correctamente, estos anuncios pueden tener el efecto totalmente contrario.

En las secciones siguientes se proporcionan recomendaciones sobre cómo implementar anuncios de vídeo intersticial y anuncios de banners intersticiales en la aplicación mediante [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad)y ejemplos de implementaciones que infringen la [Directiva 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) de las directivas de Microsoft Store. Dado que conoces tu aplicación mejor que nadie, excepto en lo que a directivas se refiere, la decisión final está en tus manos. Lo más importante que debes tener en cuenta es que las clasificaciones de la aplicación y los ingresos están estrechamente relacionados.

### <a name="best-practices"></a>Procedimientos recomendados

Te recomendamos que sigas los siguientes procedimientos recomendados a la hora de implementar anuncios intersticiales en tu aplicación:

* Coloca los anuncios intersticiales en el flujo natural de la aplicación, por ejemplo, entre los niveles del juego.

* Asocia los anuncios con ventajas tangibles, como:

    * Sugerencias para la finalización del nivel.

    * Tiempo extra para reintentar un nivel.

    * Características de avatar personalizadas, como un tatuaje o un sombrero.

* Si su aplicación requiere que se muestre un anuncio de vídeo intersticial en el momento de la finalización, mencione esa regla por adelantado para que no sorprenda con un mensaje de error al pulsar el botón Cerrar.

* Realiza la captura previa del anuncio (mediante una llamada a [InterstitialAd.RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)). Lo ideal es hacerla entre 30 y 60 segundos antes de mostrarlo.

* Suscríbete a los cuatro eventos que se exponen en la clase [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) (**Cancelled**, **Completed**, **AdReady** y **ErrorOccurred**) y úsalos para tomar las decisiones correctas para tu aplicación.

* Ten alguna funcionalidad integrada para usarla en lugar de un anuncio asociado al servidor. La encontrarás útil en algunos escenarios:

    * Modo sin conexión, si no se puede acceder a los servidores de anuncios.

    * Si se desencadena el evento **ErrorOccurred**.

    * Si optas por ahorrar ancho de banda del usuario en función de [ConnectionProfile](/uwp/api/Windows.Networking.Connectivity.ConnectionProfile), existen algunas API en la clase **ConnectionProfile** que pueden resultarte útiles.

* Usa el tiempo de espera predeterminado (30 segundos), a menos que haya una razón válida para cambiarlo, pero no uses menos de 10 segundos. Los anuncios intersticiales tardan mucho más en descargarse que los anuncios de banner estándar, especialmente en los mercados que no tienen conexiones de alta velocidad.

* Ten en cuenta el plan de datos del usuario. Por ejemplo, no se muestra ni se advierte al usuario antes de servir una anuncio de vídeo intersticial en un dispositivo móvil que está cerca o superado su límite de datos. Existen algunas API en la clase [ConnectionProfile](/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) que pueden resultarte útiles.

* Mejora continuamente tu aplicación después del envío inicial. Examine los [informes de anuncios](../publish/advertising-performance-report.md) y realice cambios de diseño para mejorar las tasas de finalización de vídeo de relleno e intersticial.

### <a name="practices-to-avoid"></a>Procedimientos que deben evitarse

Te recomendamos que evites los siguientes procedimientos cuando implementes anuncios intersticiales en la aplicación:

* No te excedas. No muestres anuncios con una frecuencia superior a cada 5 minutos, a menos que el usuario obtenga explícitamente una ventaja tangible opcional, además de poder jugar al juego.

* No muestre intersticiales en el inicio de la aplicación, ya que los usuarios pueden creer que hizo clic en el icono equivocado.

* No mostrar los anuncios intersticiales al salir. Este inventario no es adecuado, ya que los índices de finalización se aproximarán a cero.

* No muestres dos o más anuncios intersticiales consecutivos. Los usuarios se frustrarán al ver que la barra de progreso del anuncio vuelve al punto de partida. Muchos pensarán que se trata de un error de codificación o de presentación del anuncio.

* No capture un anuncio de vídeo intersticial superior a 5 minutos antes de llamar a [InterstitialAd. show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show). Un buen inventario maximizará la conversión de anuncios capturados previamente en impresiones facturables.

* No penalices al usuario por errores en la presentación de anuncios, como la ausencia de anuncios disponibles. Por ejemplo, si muestras una opción de la interfaz de usuario destinada a "Ver un anuncio para obtener *xxx*", debes proporcionar *xxx* si el usuario hizo su parte. Existen dos opciones que debes tener en cuenta:

    * No se debe incluir la opción a menos que se desencadene el evento [InterstitialAd.AdReady](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready).

    * La aplicación debe incluir una funcionalidad integrada que genere el mismo beneficio que un anuncio real.

* No uses anuncios intersticiales para permitir que el usuario obtenga una ventaja competitiva en un juego de varios jugadores. Por ejemplo, no seduzcas al usuario con un arma de fuego superior en un juego de tiros en primera persona a condición de que vea un anuncio intersticial. Una camisa personalizada para el avatar del jugador está bien, siempre y cuando no proporcione camuflaje.

### <a name="examples-of-policy-violations"></a>Ejemplos de infracciones de directivas

En esta sección se proporcionan ejemplos de escenarios de anuncios intersticiales que infringen la [Directiva 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) de las directivas de Microsoft Store. Tales ejemplos se proporcionan para fines informativos solamente, como una forma de ayudarte a comprender mejor la directiva. Estos ejemplos no son integrales y es posible que haya muchas otras formas de infringir la directiva 10.10.1 que no se muestran aquí.

* Colocar un elemento de interfaz de usuario sobre el contenedor de anuncio intersticial.

* Hacer una llamada a [InterstitialAd.Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) mientras el usuario está ocupado con la aplicación.

* Usar anuncios intersticiales para obtener todo lo que pueda usarse como moneda o negociarse con otros usuarios.

* Solicitar un anuncio intersticial nuevo en el contexto del controlador de eventos para el evento [InterstitialAd.ErrorOccurred](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.erroroccurred). Esto puede ocasionar un bucle infinito y causar problemas de funcionamiento del servicio de publicidad.

* Solicitar un anuncio intersticial solo para tener un anuncio de copia de seguridad para una secuencia de anuncios en cascada. Si solicitas un anuncio intersticial y luego recibes el evento [InterstitialAd.AdReady](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready), el siguiente anuncio intersticial que se muestre en la aplicación debe ser el anuncio que está listo para mostrarse a través del método [InterstitialAd.Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

* Usar unidades de anuncio en directo (es decir, unidades de anuncios que se obtienen del centro de Partners) durante el desarrollo y las pruebas, o en un emulador.

* Escribir o distribuir código que llame a los servicios de anuncio a través de medios que no sean las bibliotecas de Microsoft Advertising que se ejecutan en el contexto de la aplicación.

* Interactuar con interfaces no documentadas u objetos secundarios creados por las bibliotecas de Microsoft Advertising, por ejemplo, **WebView** o **MediaElement**.

## <a name="guidelines-for-native-ads"></a>Directrices para anuncios nativos

Los [anuncios nativos](native-ads.md) proporcionan un gran control sobre cómo presentar el contenido de publicidad a los usuarios. Siga estos requisitos e instrucciones para asegurarse de que el mensaje del anunciante llega a los usuarios a la vez que también ayuda a evitar la entrega de una experiencia de ADS nativa confusa a los usuarios.

### <a name="register-the-container-for-your-native-ad"></a>Registrar el contenedor para el ad nativo

En el código, debe llamar al método [RegisterAdContainer](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2.registeradcontainer) del objeto [NativeAdV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) para registrar el elemento de la interfaz de usuario que actúa como contenedor para el ad nativo y, opcionalmente, cualquier control específico que desee registrar como destinos seleccionables para el anuncio. Esto es necesario para realizar un seguimiento adecuado de las impresiones de anuncios y hacer clic en ellos.

Hay dos sobrecargas para el método **RegisterAdContainer** que puede usar:

* Si desea que se pueda hacer clic en todo el contenedor de todos los elementos nativos de ad, llame al método **RegisterAdContainer (FrameworkElement)** y pase el control contenedor al método. Por ejemplo, si se muestran todos los elementos de ad nativos en controles independientes que se hospedan en un **StackPanel** y se desea que se pueda hacer clic en el elemento **StackPanel** completo, pase el elemento **StackPanel** a este método.

* Si solo desea que se pueda hacer clic en ciertos elementos de ad nativos, llame al método **RegisterAdContainer (FrameworkElement, IVector (FrameworkElement))** . Solo se puede hacer clic en los controles que se pasan al segundo parámetro.

### <a name="required-native-ad-elements"></a>Elementos de ad nativos necesarios

Como mínimo, debe mostrar siempre los siguientes elementos de ad nativos proporcionados por las propiedades del objeto [NativeAdV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) para el usuario en el diseño de ad nativo. Si no incluye estos elementos, es posible que el rendimiento sea deficiente y que la unidad de anuncio no ofrezca un rendimiento bajo.

1. Mostrar siempre el título del anuncio nativo (disponible en la propiedad **título** ). Proporcione espacio suficiente para mostrar al menos 25 caracteres. Si el título es más largo, reemplace el texto adicional por un botón de puntos suspensivos.
2. Muestre siempre al menos uno de los siguientes elementos para ayudar a diferenciar la experiencia de ad nativa del resto de la aplicación e indicar claramente que el contenido es proporcionado por un anunciante:
    * El icono de *ad* distinguible (disponible en la propiedad de **adicon** ). Este icono lo proporciona Microsoft.
    * *Patrocinado por* texto (disponible en la propiedad **SponsoredBy** ). Este texto es proporcionado por el anunciante.
    * Como alternativa al texto *patrocinado por* , puede elegir mostrar algún otro texto que ayude a diferenciar la experiencia de ad nativa del resto de la aplicación, como "contenido patrocinado", "contenido promocional", "contenido recomendado", etc.

### <a name="user-experience"></a>Experiencia del usuario

El anuncio nativo debe estar claramente delineado desde el resto de la aplicación y tener espacio alrededor para evitar clics accidentales. Use bordes, distintos terrenos o cualquier otra interfaz de usuario para separar el contenido de ad del resto de la aplicación. Tenga en cuenta que los clics accidentales de anuncios no son beneficiosos para los ingresos basados en ad ni para la experiencia del usuario final a largo plazo.

### <a name="description"></a>Descripción

Si elige mostrar la descripción del anuncio (disponible en la propiedad **Descripción** del objeto **NativeAdV2** ), proporcione espacio suficiente para mostrar al menos 75 caracteres. Se recomienda usar una animación para mostrar el contenido completo de la descripción de ad.

### <a name="call-to-action"></a>Llamada a la acción

La *llamada al* texto de la acción (disponible en la propiedad **CallToAction** del objeto **NativeAdV2** ) es un componente crítico de ad. Si opta por mostrar este texto, siga estas instrucciones:

* Muestre siempre la *llamada al* usuario de la acción en un control en el que se pueda hacer clic, como un botón o un hipervínculo.
* Muestre siempre la *llamada al* texto de la acción en su totalidad.
* Asegúrese de que la *llamada al* texto de la acción está separada del resto del texto promocional del anunciante.