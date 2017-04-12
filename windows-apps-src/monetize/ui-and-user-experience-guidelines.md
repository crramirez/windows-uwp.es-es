---
author: mcleanbyron
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: "Obtén las directrices de la interfaz de usuario y de la experiencia del usuario para los anuncios en aplicaciones."
title: Directrices de la interfaz de usuario y de la experiencia del usuario para anuncios
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP, ads, anuncios, advertising, publicidad, guidelines, directrices, best practices, procedimientos recomendados
ms.openlocfilehash: 75a68977e5edb996a5e2fc1ae9265d11b7492ad9
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="ui-and-user-experience-guidelines-for-ads"></a>Directrices de la interfaz de usuario y de la experiencia del usuario para anuncios

En este artículo se proporcionan directrices para ofrecer magníficas experiencias con anuncios de banner y anuncios intersticiales en las aplicaciones. Para obtener instrucciones generales sobre cómo diseñar la apariencia de las aplicaciones, consulta [Diseño e interfaz de usuario](https://developer.microsoft.com/windows/apps/design).

>**Importante**&nbsp;&nbsp;Todo uso de publicidad en tu aplicación debe regirse por las Directivas de la Tienda Windows, que incluyen, entre otras, la [directiva 10.10](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) (Conducta y contenido de publicidad). En concreto, la implementación de anuncios de banner o anuncios intersticiales de la aplicación debe cumplir los requisitos de directiva de la Tienda Windows [directiva 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10). En este artículo se incluyen ejemplos de implementaciones que infringirían dicha directiva. Tales ejemplos se proporcionan para fines informativos solamente, como una forma de ayudarte a comprender mejor la directiva. Estos ejemplos no son integrales y es posible que haya muchas otras formas de infringir las Directivas de la Tienda Windows que no se muestran en este artículo.

## <a name="guidelines-for-banner-ads"></a>Directrices para anuncios de banner

En las siguientes secciones se proporcionan recomendaciones sobre cómo implementar anuncios de banner en tu aplicación mediante la clase [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) y ejemplos de implementaciones que infringen la [directiva 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) de las Directivas de la Tienda Windows.

### <a name="best-practices"></a>Procedimientos recomendados

Te recomendamos que sigas los siguientes procedimientos recomendados a la hora de implementar anuncios de banner en tu aplicación:

* Dedica la mayor parte de la interfaz de usuario de la aplicación al contenido y los controles funcionales.

* Diseña la publicidad en la experiencia. Ofrece a tus diseñadores un anuncio de muestra para planear el aspecto que tendrá la publicidad. Dos ejemplos de anuncios bien planeados en las aplicaciones son el diseño de anuncios como contenido y el diseño de división.

  Para ver el aspecto y el funcionamiento de los distintos tamaños de anuncio en la aplicación, puedes usar nuestras [unidades de anuncio del modo de prueba](test-mode-values.md) durante la etapa de desarrollo y pruebas. Cuando termines con las unidades de anuncio del modo de prueba, recuerda [actualizar la aplicación con identificadores de unidad de anuncio reales](set-up-ad-units-in-your-app.md) antes de enviar la aplicación para su certificación.

* Usa [tamaños de anuncio](supported-ad-sizes-for-banner-ads.md) que se ajusten correctamente al diseño del dispositivo en ejecución.

* Planea las ocasiones en que no haya anuncios disponibles. Puede que haya ocasiones en que no se envíen anuncios a la aplicación. Diseña las páginas de manera que tengan un magnífico aspecto tanto si muestran un anuncio como si no. Para obtener más información, consulta [Control de errores](error-handling-with-advertising-libraries.md).

* Si tienes un escenario para alertar al usuario que se controla mejor con una superposición, haz una llamada a [AdControl.Suspend](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.suspend.aspx) mientras se muestra la superposición y, luego, a [AdControl.Resume](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.resume.aspx) cuando termine el escenario de alerta.

<span />
### <a name="practices-to-avoid"></a>Procedimientos que deben evitarse

Te recomendamos que evites los siguientes procedimientos cuando implementes anuncios de banner en la aplicación:

* No colocar publicidad en la superficie abierta. El espacio de anuncios no debe colocarse en el primer espacio abierto de superficie que encuentres. En su lugar, debe incluirse en el diseño general de la aplicación.

* No incluir publicidad excesiva ni saturar la aplicación. Demasiados anuncios en la aplicación desmerecerán su apariencia y facilidad de uso. El objetivo de la publicidad es ganar dinero, pero no a costa de la propia aplicación.

* No distraer al usuario de sus tareas principales. La aplicación siempre debe ser el foco principal. El espacio de anuncios debe incluirse de modo que sea un foco secundario.

<span />
### <a name="examples-of-policy-violations"></a>Ejemplos de infracciones de directivas

En esta sección se proporcionan ejemplos de escenarios de anuncios de banner que infringen la [directiva 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) de las Directivas de la Tienda Windows. Tales ejemplos se proporcionan para fines informativos solamente, como una forma de ayudarte a comprender mejor la directiva. Estos ejemplos no son integrales y es posible que haya muchas otras formas de infringir la directiva 10.10.1 que no se muestran aquí.

* Interferir de algún modo con la capacidad del usuario para ver el anuncio de banner, por ejemplo, cambiando la opacidad de la clase [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) o colocar otro control por encima de **AdControl** (sin antes hacer una llamada a [AdControl.Suspend](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.suspend.aspx)).

* Requerir que los usuarios hagan clic en un banner para realizar una tarea en la aplicación u obligarlos a hacer clic en anuncios de banner como resultado del diseño de la aplicación.

* Omitir, de cualquier manera, el temporizador de actualización mínima integrado para anuncios de banner, lo que incluye, entre otros, intercambiar objetos [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) o forzar una actualización de la página sin interacción del usuario.

* Usar unidades de anuncio dinámicas (es decir, unidades de anuncio que se obtienen a partir del panel del Centro de desarrollo de Windows) durante la etapa de desarrollo y pruebas o en un emulador.

* Escribir o distribuir código que llame a los servicios de anuncio a través de medios que no sean las bibliotecas de Microsoft Advertising que se ejecutan en el contexto de la aplicación.

* Interactuar con interfaces no documentadas u objetos secundarios creados por las bibliotecas de Microsoft Advertising, por ejemplo, **WebView** o **MediaElement**.

<span id="interstitialbestpractices10">
## <a name="guidelines-for-interstitial-ads"></a>Directrices para anuncios intersticiales

Si se usan con elegancia, los anuncios intersticiales pueden aumentar considerablemente los ingresos de la aplicación, sin incidir de forma negativa en la satisfacción del usuario. Si no se usan correctamente, estos anuncios pueden tener el efecto totalmente contrario.

En las siguientes secciones se proporcionan recomendaciones sobre cómo implementar anuncios de vídeo intersticial y de banner intersticial en la aplicación con la clase [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) y ejemplos de implementaciones que infringen la [directiva 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) de las Directivas de la Tienda Windows. Dado que conoces tu aplicación mejor que nadie, excepto en lo que a directivas se refiere, la decisión final está en tus manos. Lo más importante que debes tener en cuenta es que las clasificaciones de la aplicación y los ingresos están estrechamente relacionados.

### <a name="best-practices"></a>Procedimientos recomendados

Te recomendamos que sigas los siguientes procedimientos recomendados a la hora de implementar anuncios intersticiales en tu aplicación:

* Coloca los anuncios intersticiales en el flujo natural de la aplicación, por ejemplo, entre los niveles del juego.

* Asocia los anuncios con ventajas tangibles, como:

    * Sugerencias para la finalización del nivel.

    * Tiempo extra para reintentar un nivel.

    * Características de avatar personalizadas, como un tatuaje o un sombrero.

* Si la aplicación exige la visualización de un anuncio de vídeo intersticial hasta el final, menciona esa regla de antemano para que no vean un mensaje de error por sorpresa al pulsar el botón Cerrar.

* Realiza la captura previa del anuncio (mediante una llamada a [InterstitialAd.RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx)). Lo ideal es hacerla entre 30 y 60 segundos antes de mostrarlo.

* Suscríbete a los cuatro eventos que se exponen en la clase [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) (**Cancelled**, **Completed**, **AdReady** y **ErrorOccurred**) y úsalos para tomar las decisiones correctas para tu aplicación.

* Ten alguna funcionalidad integrada para usarla en lugar de un anuncio asociado al servidor. La encontrarás útil en algunos escenarios:

    * Modo sin conexión, si no se puede acceder a los servidores de anuncios.

    * Si se desencadena el evento **ErrorOccurred**.

    * Si optas por ahorrar ancho de banda del usuario en función de [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx), existen algunas API en la clase **ConnectionProfile** que pueden resultarte útiles.

* Usa el tiempo de espera predeterminado (30 segundos), a menos que haya una razón válida para cambiarlo, pero no uses menos de 10 segundos. Los anuncios intersticiales tardan mucho más tiempo en descargarse que los anuncios de banner estándar, especialmente en los mercados que no tienen conexiones de alta velocidad.

<span/>

* Ten en cuenta el plan de datos del usuario. Por ejemplo, no lo muestres o advierte al usuario, antes de presentar un anuncio de vídeo intersticial en un dispositivo móvil que supere o se acerque al límite de datos. Existen algunas API en la clase [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) que pueden resultarte útiles.

* Mejora continuamente tu aplicación después del envío inicial. Consulta los [informes de anuncios](../publish/advertising-performance-report.md) y realiza cambios de diseño para mejorar las velocidades de relleno y finalización de vídeo intersticial.

<span />
### <a name="practices-to-avoid"></a>Procedimientos que deben evitarse

Te recomendamos que evites los siguientes procedimientos cuando implementes anuncios intersticiales en la aplicación:

* No te excedas. No muestres anuncios con una frecuencia superior a cada 5 minutos, a menos que el usuario obtenga explícitamente una ventaja tangible opcional, además de poder jugar al juego.

* No muestres anuncios intersticiales al inicio de la aplicación, porque los usuarios pueden pensar que han hecho clic en el icono incorrecto.

* No muestres anuncios intersticiales al salir. Este inventario no es adecuado, ya que los índices de finalización se aproximarán a cero.

* No muestres dos o más anuncios intersticiales consecutivos. Los usuarios se frustrarán al ver que la barra de progreso del anuncio vuelve al punto de partida. Muchos pensarán que se trata de un error de codificación o de presentación del anuncio.

* No captures un anuncio de vídeo intersticial más de cinco minutos antes de la llamada al método [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx). Un buen inventario maximizará la conversión de anuncios capturados previamente en impresiones facturables.

* No penalices al usuario por errores en la presentación de anuncios, como la ausencia de anuncios disponibles. Por ejemplo, si muestras una opción de la interfaz de usuario destinada a "Ver un anuncio para obtener *xxx*", debes proporcionar *xxx* si el usuario hizo su parte. Existen dos opciones que debes tener en cuenta:

    * No se debe incluir la opción a menos que se desencadene el evento [InterstitialAd.AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx).

    * La aplicación debe incluir una funcionalidad integrada que genere el mismo beneficio que un anuncio real.

* No uses anuncios intersticiales para permitir que el usuario obtenga una ventaja competitiva en un juego de varios jugadores. Por ejemplo, no seduzcas al usuario con un arma de fuego superior en un juego de tiros en primera persona a condición de que vea un anuncio intersticial. Una camisa personalizada para el avatar del jugador está bien, siempre y cuando no proporcione camuflaje.

<span />
### <a name="examples-of-policy-violations"></a>Ejemplos de infracciones de directivas

En esta sección se proporcionan ejemplos de escenarios de anuncios intersticiales que infringen la [directiva 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) de las Directivas de la Tienda Windows. Tales ejemplos se proporcionan para fines informativos solamente, como una forma de ayudarte a comprender mejor la directiva. Estos ejemplos no son integrales y es posible que haya muchas otras formas de infringir la directiva 10.10.1 que no se muestran aquí.

* Colocar un elemento de interfaz de usuario sobre el contenedor de anuncio intersticial.

* Hacer una llamada a [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) mientras el usuario está ocupado con la aplicación.

* Usar anuncios intersticiales para obtener todo lo que pueda usarse como moneda o negociarse con otros usuarios.

* Solicitar un anuncio intersticial nuevo en el contexto del controlador de eventos para el evento [InterstitialAd.ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.erroroccurred.aspx). Esto puede ocasionar un bucle infinito y causar problemas de funcionamiento del servicio de publicidad.

* Solicitar un anuncio intersticial solo para tener un anuncio de copia de seguridad para una secuencia de anuncios en cascada. Si solicitas un anuncio intersticial y luego recibes el evento [InterstitialAd.AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx), el siguiente anuncio intersticial que se muestre en la aplicación debe ser el anuncio que está listo para mostrarse a través del método [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

* Usar unidades de anuncio dinámicas (es decir, unidades de anuncio que se obtienen a partir del panel del Centro de desarrollo de Windows) durante la etapa de desarrollo y pruebas o en un emulador.

* Escribir o distribuir código que llame a los servicios de anuncio a través de medios que no sean las bibliotecas de Microsoft Advertising que se ejecutan en el contexto de la aplicación.

* Interactuar con interfaces no documentadas u objetos secundarios creados por las bibliotecas de Microsoft Advertising, por ejemplo, **WebView** o **MediaElement**.

 

 
