---
author: mcleanbyron
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: Obtén las directrices de la interfaz de usuario y de la experiencia del usuario para los anuncios en aplicaciones.
title: Directrices de la interfaz de usuario y de la experiencia del usuario para anuncios en aplicaciones

---

# Directrices de la interfaz de usuario y de la experiencia del usuario para anuncios en aplicaciones


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## Recursos de interfaz de usuario generales para las aplicaciones de Windows

Puedes encontrar información sobre cómo diseñar la apariencia de las aplicaciones en [Diseño e interfaz de usuario](https://developer.microsoft.com/windows/design).

## Procedimientos recomendados de AdControl

* [Procedimientos recomendados de AdControl: QUÉ HACER](#adcontrolbestpracticesdo10)
* [Procedimientos recomendados de AdControl: QUÉ NO HACER](#adcontrolbestpracticesdont10)

<span id="adcontrolbestpracticesdo10"/>
### Procedimientos recomendados de AdControl: QUÉ HACER

* Diseñar la publicidad en la experiencia. Ofrece a tus diseñadores un anuncio de muestra para planear el aspecto que tendrá la publicidad. Dos ejemplos de anuncios bien planeados en las aplicaciones son el diseño de anuncios como contenido y el diseño de división.

  Para ver el aspecto y el funcionamiento de los distintos tamaños de anuncio en la aplicación, puedes usar nuestras unidades de anuncios del modo de prueba para Windows Phone, Windows 8.1 y Windows 10. Cuando termines con las unidades de anuncios del modo de prueba, recuerda [actualizar la aplicación con identificadores de unidades de anuncios reales](set-up-ad-units-in-your-app.md) antes de enviar la aplicación para su certificación.

* Realizar un planeamiento para las ocasiones en que no haya anuncios disponibles. Puede que haya ocasiones en que no se envíen anuncios a la aplicación. Diseña las páginas de manera que tengan un magnífico aspecto tanto si muestran un anuncio como si no. Para obtener más información, consulta [Control de errores](error-handling-with-advertising-libraries.md).

<span id="adcontrolbestpracticesdont10"/>
### Procedimientos recomendados de AdControl: QUÉ NO HACER

* Colocar publicidad en la superficie abierta. El espacio de anuncios no debe colocarse en el primer pedazo abierto de superficie que encuentres. En su lugar, debe incluirse en el diseño general de la aplicación.

* Incluir publicidad excesiva y saturar la aplicación. Demasiados anuncios en la aplicación desmerecerán su apariencia y facilidad de uso. El objetivo de la publicidad es ganar dinero, pero no a costa de la propia aplicación.

* Distraer al usuario de sus tareas principales. La aplicación siempre debe ser el enfoque principal. El espacio de anuncios debe incluirse de modo que sea un enfoque secundario.

<span id="interstitialbestpractices10"/>
## Procedimientos recomendados para anuncios intersticiales

* [Procedimientos recomendados para anuncios intersticiales: QUÉ HACER](#interstitialbestpracticesdo10)
* [Procedimientos recomendados para anuncios intersticiales: QUÉ EVITAR](#interstitialbestpracticesavoid10)
* [Procedimientos recomendados para anuncios intersticiales: LO QUE NUNCA SE DEBE HACER (con aplicación de directiva)](#interstitialbestpracticesnever10)

Si se usan con distinción, los anuncios intersticiales en vídeo pueden aumentar considerablemente los ingresos de la aplicación, sin influir negativamente en la satisfacción del usuario. Si no se usan correctamente, estos anuncios pueden tener el efecto totalmente contrario.

Nuestro objetivo es ayudarte a lograr esta distinción. Dado que conoces tu aplicación mejor que nadie, excepto en lo que a directivas se refiere, la decisión final está en tus manos. Lo más importante a tener en cuenta es que las clasificaciones de la aplicación y los ingresos están estrechamente relacionados.

<span id="interstitialbestpracticesdo10"/>
### Procedimientos recomendados para anuncios intersticiales: QUÉ HACER

* Encaja anuncios intersticiales en el flujo natural de la aplicación, como, por ejemplo, entre los niveles del juego.

* Asocia los anuncios con ventajas tangibles, como:

    * Sugerencias para la finalización del nivel.

    * Tiempo extra para reintentar un nivel.

    * Características de avatar personalizadas, como un tatuaje o un sombrero.

* Si la aplicación exige la visualización de un anuncio de vídeo al finalizar, menciona esa regla de antemano para que no vean un mensaje de error por sorpresa al pulsar el botón Cerrar.

* Realiza la captura previa del anuncio (mediante una llamada a [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx)). Lo ideal es entre 30 y 60 segundos antes de que deba mostrarse.

* Suscríbete a los cuatro eventos que se exponen en la clase [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) (**Cancelled**, **Completed**, **AdReady** y **ErrorOccurred**) y úsalos para tomar las decisiones correctas para tu aplicación.

* Ten alguna funcionalidad integrada para usarla en lugar de un anuncio asociado al servidor. La encontrarás útil en algunos escenarios:

    * Modo sin conexión, si no se puede acceder a los servidores de anuncios.

    * Si se desencadena el evento **ErrorOccurred**.

    * Si optas por ahorrar ancho de banda del usuario en función de [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx), existen algunas API en la clase **ConnectionProfile** que pueden resultarte útiles.

* Usa el tiempo de espera predeterminado (30 s), a menos que haya una razón válida para cambiarlo, pero no uses menos de 10 s.

    * Los anuncios en vídeo tardan mucho más tiempo en descargarse que los banners, especialmente en los mercados que no tienen conexiones de alta velocidad.


* Ten en cuenta el plan de datos del usuario. Por ejemplo, no lo muestres o advierte al usuario, antes de presentar un anuncio en vídeo en un dispositivo móvil que supere o se acerque al límite de datos. Existen algunas API en la clase [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) que pueden resultarte útiles.

* Mejora continuamente tu aplicación después del envío inicial. Consulta los informes de anuncios y realiza cambios de diseño para mejorar las velocidades de relleno y finalización de vídeo.

<span id="interstitialbestpracticesavoid10"/>
### Procedimientos recomendados para anuncios intersticiales: QUÉ EVITAR

* Excederse. No fuerces anuncios con una frecuencia superior a cada 5 minutos, a menos que el usuario use explícitamente una ventaja tangible opcional, además del juego.

* Los anuncios intersticiales en vídeo al inicio de la aplicación, dado que los usuarios pueden pensar que han hecho clic en el icono incorrecto.

* Los anuncios intersticiales en vídeo al salir. Este inventario no es adecuado, ya que las velocidades de finalización se aproximarán a cero.

    * Dos o más anuncios en vídeo consecutivos.

    * Los usuarios se frustrarán al ver que la barra de progreso del anuncio vuelve al punto de partida.

    * Muchos pensarán que se trata de un error de codificación o de presentación del anuncio.

* Capturar un anuncio en vídeo más de 5 minutos antes de llamar a [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

    * Un buen inventario maximizará la conversión de anuncios capturados previamente en impresiones facturables.


* Penalizar a un usuario por errores en la presentación de anuncios, como la ausencia de anuncios disponibles. Por ejemplo, si muestras una opción de la interfaz de usuario destinada a "Ver un anuncio para obtener *xxx*", debes proporcionar *xxx* si el usuario hizo su parte. Existen dos opciones a tener en cuenta:

    * No se debe incluir la opción a menos que se desencadene el evento [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx).

    * La aplicación debe incluir una funcionalidad integrada que genere el mismo beneficio que un anuncio real.

* Permitir que un usuario obtenga una ventaja competitiva en un juego multijugador.

    * Una arma de fuego superior en un juego de tiros en primera persona sería un claro ejemplo.

    * Una camisa personalizada para el avatar del jugador está bien, siempre y cuando no proporcione camuflaje.

<span id="interstitialbestpracticesnever10"/>
### Procedimientos recomendados para anuncios intersticiales: LO QUE NUNCA SE DEBE HACER (con aplicación de directiva)

* Colocar elementos de la interfaz de usuario sobre el contenedor de anuncios.

    * Los anunciantes han pagado por la pantalla completa.


* Llamar a **Show** mientras el usuario está ocupado con la aplicación.

    * Dado que **InterstitialAd** creará una superposición de pantalla completa, que al usuario le parecerá desproporcionada.

    * También se pueden producir tasas de clics exageradas.

* Usar anuncios para obtener todo lo que se pueda usar como moneda o negociarse con otros usuarios.

 

 


<!--HONumber=May16_HO2-->


