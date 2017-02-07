---
author: joannaleecy
title: "Monetización para juegos"
description: "Implementa anuncios de banner, anuncios intersticiales en vídeo y compras desde la aplicación para juegos para Plataforma universal de Windows (UWP) en Windows10."
ms.assetid: 79f4e177-d8e7-45d3-8a78-31d4c2fe298a
translationtype: Human Translation
ms.sourcegitcommit: 1afcb25eeacadee89097a69ca64404f1102b3e57
ms.openlocfilehash: 00ab2e2f8b9b086bfcfe31972816a56f7e043d5c

---
#  Monetización para juegos

Como desarrollador de juegos, debes conocer las opciones de monetización para que puedas mantener tu negocio y seguir haciendo lo que te apasiona: crear juegos espectaculares. En este artículo se proporciona información general sobre los métodos de monetización de juegos para Plataforma universal de Windows (UWP) y cómo implementar dichos métodos.

En el pasado, simplemente hubieses puesto un precio a tu juego y luego hubieses esperado a que las personas lo compraran en una tienda. Pero hoy en día, tienes diferentes opciones. Puedes distribuir un juego en tiendas "físicas", venderlo en línea (copias físicas o electrónicas) o permitir que todas las personas lo usen de forma gratuita, pero incorporar algún tipo de anuncio o artículos dentro del juego que se puedan comprar. Los juegos ya no son simplemente productos independientes. Suelen venir con contenido adicional que se puede comprar además del juego principal. 

Puedes promocionar y monetizar un juego para UWP de una o varias de las siguientes maneras:
* Coloca el juego en la Tienda Windows, que es una oferta de tienda en línea segura con [distribución en todo el mundo](#worldwide-distribution-channel). Los jugadores de todo el mundo pueden comprar un juego en línea al [precio que determines](#set-a-price-for-your-game).
* Usa las API de Windows SDK para crear [compras dentro del juego](#in-game-purchases). Los jugadores pueden comprar artículos desde el juego o comprar contenido adicional, por ejemplo, equipo adicional, máscaras, mapas o niveles de juego.
* Usa las API de [Microsoft Store Services SDK](https://visualstudiogallery.msdn.microsoft.com/229b7858-2c6a-4073-886e-cbb79e851211) para mostrar anuncios procedentes de redes de anuncios. Puedes [mostrar anuncios en el juego](#display-ads-in-your-game) y ofrecer a los jugadores la opción de ver anuncios en vídeo a cambio de recompensas dentro del juego.
* [Maximiza el potencial del juego a través de campañas publicitarias](#maximize-your-games-potential-through-ad-campaigns). Promociona tu juego mediante campañas de anuncios de pago, comunitarias (gratuitas) o internas (gratuitas) para aumentar la base de usuarios del juego.
 
## Canal de distribución en todo el mundo

La Tienda Windows puede hacer que el juego esté disponible para descargar en más de 200 países y regiones en todo el mundo, con compatibilidad para la facturación a través de diversas formas de pago, como Visa, MasterCard y PayPal. Para obtener una lista completa de los países y las regiones, consulta [Mercados y precios personalizados](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices).

## Establecer un precio para el juego 

Los juegos para UWP que se publican en la Tienda pueden ser _de pago_ o _gratuitos_. Un juego de pago te permite cobrar a los jugadores de antemano por el juego un precio que estableces, mientras que un juego gratuito permite a los usuarios descargar y usar el juego sin necesidad de pagarlo.

A continuación se detallan algunos conceptos importantes en relación con el precio del juego en la Tienda.

### Precio base 

El precio base del juego es lo que determina si el juego se clasifica como _de pago_ o _gratuito_. Puedes usar el [panel del Centro de desarrollo](https://developer.microsoft.com/windows) para configurar el precio base en función del país y la región. Es posible que el proceso de determinación del precio incluya tus [responsabilidades fiscales si vendes en diferentes países](https://msdn.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps) y las [consideraciones relativas a los precios para mercados concretos](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#price-considerations-for-specific-markets). También puedes [establecer precios personalizados para mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Para obtener más información, consulta [Definir los precios y la selección del mercado](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection).

### Precio de venta

Una forma de promocionar el juego es reducir el precio durante un tiempo limitado. También es posible establecer el precio de venta en __Gratuito__ para permitir la descarga del juego sin necesidad de pago.
Puedes programar campañas de oferta de antemano al definir la fecha de inicial y final de la oferta. Para obtener más información, consulta [Poner aplicaciones y complementos en oferta](https://msdn.microsoft.com/windows/uwp/publish/put-apps-and-add-ons-on-sale).

## Compras desde el juego

Las compras desde el juego son productos que se compran dentro de un juego. También se conocen de forma genérica como _compras desde la aplicación_. En la Tienda Windows, estos productos se denominan _complementos_. [Los complementos se publican](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions) a través del panel del Centro de desarrollo de Windows. También tendrás que habilitar los complementos en el código del juego.

### Tipos de complementos

Puedes crear dos tipos de complementos en la Tienda: _duraderos_ o _consumibles_. Los complementos duraderos son artículos que persisten durante un período de tiempo especificado y se pueden comprar una sola vez hasta que expiran. Los complementos consumibles son artículos que se puedan comprar y usar una y otra vez.

Al crear complementos consumibles, decide cómo quieres hacer su seguimiento; es decir, si son _administrados por el desarrollador_ o _administrados por la Tienda_ (esta característica está disponible a partir de Windows10, versión 1607). Con un complemento consumible administrado por el desarrollador, es tu responsabilidad hacer un seguimiento de saldo del artículo para el jugador; con un complemento consumible administrado por la Tienda, la Tienda Windows hace el seguimiento de saldo del artículo por ti. Para obtener más información, consulta [Introducción a los complementos consumibles](https://msdn.microsoft.com/windows/uwp/monetize/enable-consumable-add-on-purchases#overview-of-consumable-add-ons).

### Crear compras desde el juego

Las API más recientes de información de licencia y compras desde la aplicación forman parte del espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) de Windows SDK (a partir de Windows 10, versión 1607). Si vas a desarrollar un juego nuevo destinado a la versión 1607 o posterior, te recomendamos que uses el espacio de nombres __Windows.Services.Store__ porque admite los tipos de complemento más recientes y tiene un mejor rendimiento.
También se ha diseñado para que sea compatible con los tipos futuros de productos y características admitidos por el Centro de desarrollo de Windows y la Tienda. Al desarrollar para versiones anteriores de Windows 10, usa el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) en su lugar.

Para obtener más información, visita [Pruebas y compras desde la aplicación](https://msdn.microsoft.com/windows/uwp/monetize/in-app-purchases-and-trials).

#### Ejemplo de compra simplificado

En esta sección se usa un ejemplo de compra simplificado para ilustrar el uso de llamadas a diferentes métodos para implementar el flujo de compra.

|Acciones dentro del juego/actividad                                                | Tareas en segundo plano del juego                  |
|--------------------------------------------------------------------------|----------------------------------------|
|El jugador entra a una tienda. Aparece el menú de la tienda para mostrar los complementos disponibles y el precio de compra. |  El juego [recupera la información del producto](https://msdn.microsoft.com/windows/uwp/monetize/get-product-info-for-apps-and-add-ons) de los complementos, [determina si los complementos tienen la licencia adecuada](https://msdn.microsoft.com/windows/uwp/monetize/get-license-info-for-apps-and-add-ons) y muestra los complementos que están disponibles para que el jugador los compre en el menú de la tienda.                           |
|El jugador hace clic en __Comprar__ para comprar un artículo.             |La acción __Comprar__ envía una solicitud para comprar el artículo e inicia el proceso de pago para obtenerlo. La implementación varía según el tipo de artículo. Si se trata de [un artículo duradero o una compra de pago único](https://msdn.microsoft.com/windows/uwp/monetize/enable-in-app-purchases-of-apps-and-add-ons), el cliente puede ser propietario de un solo artículo hasta que expire. Si el artículo es un [complemento consumible](https://msdn.microsoft.com/windows/uwp/monetize/enable-consumable-add-on-purchases), el cliente puede ser propietario de uno o varios de ellos. |

### Probar las compras desde el juego durante el desarrollo del juego

Dado que un complemento se debe crear junto con un juego, tu juego debe publicarse y estar disponible en la Tienda. Los pasos descritos en esta sección muestran cómo crear complementos mientras el juego aún está en la etapa de desarrollo.
(Si el juego terminado ya está disponible en la Tienda, puedes omitir los tres primeros pasos e ir directamente a [Crear un complemento en la Tienda](#create-an-add-on-in-the-store)).

Para crear complementos mientras el juego aún está en la etapa de desarrollo: 
1. [Crear un paquete](#create-a-package)
2. [Publicar el juego como oculto](#publish-the-game-as-hidden)
3. [Asociar la solución de juego de Visual Studio a la Tienda](#associate-your-game-solution-with-the-store)
4. [Crear un complemento en la Tienda](#create-an-add-on-in-the-store)

#### Crear un paquete

Para publicar cualquier juego, este debe cumplir los requisitos mínimos de certificación de aplicaciones en Windows. Puedes usar el [Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit), que se incluye en el SDK de Windows 10, para ejecutar pruebas en el juego que te ayudarán a asegurarte de que está listo para publicarlo en la Tienda. Si todavía no descargaste el SDK de Windows 10 que incluye el Kit para la certificación de aplicaciones en Windows, ve a [SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk).

Para crear un paquete que se puede cargar en la Tienda:

1. Abre la solución de juego en Visual Studio.
2. En Visual Studio, ve a __Proyecto__ > __Tienda__ > __Crear paquetes de aplicaciones...__
3. Para la opción __¿Desea compilar paquetes para cargarlos en la Tienda Windows?__, selecciona __Sí__.
4. Inicia sesión en tu cuenta de desarrollador del Centro de desarrollo. O bien, [regístrate](https://developer.microsoft.com/store/register) para obtener una cuenta de desarrollador si no tienes ninguna.
5. Selecciona una aplicación para la que se va a crear el paquete de carga. Si aún no creaste un envío de aplicación, proporciona un nuevo nombre de aplicación para crear un nuevo envío. Para obtener más información, consulta [Crear la aplicación reservando un nombre](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name).
6. Una vez creado correctamente el paquete, haz clic en __Iniciar el Kit para la certificación de aplicaciones en Windows__ para comenzar el proceso de prueba.
7. Corrige los errores para crear un paquete de juego.

#### Publicar el juego como oculto

1. Ve al [Centro de desarrollo](https://developer.microsoft.com/store) e inicia sesión.
2. En la página __Información general del panel__ o __Todas las aplicaciones__, haz clic en la aplicación con la que quieres trabajar. Si aún no creaste un envío de aplicación, haz clic en __Crear una nueva aplicación__ y reserva un nombre.
3. En la página __Información general de la aplicación__, haz clic en __Iniciar el envío__.
4. Configura este nuevo envío. En la página de envío: 
    * Haz clic en __Precios y disponibilidad__. En la sección __Visibilidad__, elige '__Ocultar esta aplicación e impedir la compra...__' para asegurarte de que solo el equipo de desarrollo tenga acceso al juego. Para obtener más información, consulta [Distribución y visibilidad](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#distribution-and-visibility).
    * Haz clic en __Propiedades__. En la sección __Categoría y subcategoría__, elige __Juegos__ y luego elige una subcategoría adecuada para el juego.
    * Haz clic en __Clasificaciones por edades__. Completa el cuestionario con precisión.
    * Haz clic en __Paquetes__. Carga el paquete de juego creado en el paso anterior.
5. Sigue toda otra indicación de envío que se muestre en el panel para que puedas publicar correctamente este juego que permanece oculto para el público.
6. Haz clic en __Enviar a la Tienda__.

Para obtener más información, consulta [Envíos de aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/app-submissions).

Cuando el juego se envía a la Tienda, pasa al [proceso de certificación de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process). Este proceso puede tardar hasta 16 horas antes de que se muestre el juego.

#### Asociar la solución de juego a la Tienda

Con la solución de juego abierta en Visual Studio:

1. Ve a __Proyecto__ > __Tienda__ > __Asociar aplicación con la Tienda...__
2. Inicia sesión en tu cuenta de desarrollador del Centro de desarrollo y selecciona el nombre de la aplicación a la que se va a asociar esta solución.
3. Haz doble clic en el __archivo Package.appxmanifest.xml__ y ve a la pestaña __Empaquetado__ para comprobar que el juego se asoció correctamente.

Si asociaste la solución a un juego publicado que está disponible y figura en la Tienda, la solución tendrá una licencia activa y, de este modo, ya estás un paso más cerca de la creación de complementos para tu juego. Para más información, consulta [Empaquetado de aplicaciones](https://msdn.microsoft.com/windows/uwp/packaging/index).

#### Crear un complemento en la Tienda

A medida que creas complementos, asegúrate de asociarlos al envío de juego correcto. Para conocer más detalles sobre cómo configurar toda la información asociada a un complemento, consulta [Envíos de complementos](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).

1. Ve al [Centro de desarrollo](https://developer.microsoft.com/store) e inicia sesión.
2. En la página __Información general del panel__ o __Todas las aplicaciones__, haz clic en la aplicación para la que quieres crear el complemento.
3. En la página __Información general de la aplicación__, en la sección __Complementos__, selecciona __Crear un nuevo complemento__.
4. Selecciona el tipo de producto para el complemento: __consumible administrado por el desarrollador__, __consumible administrado por la Tienda__ o __Duradero__.
5. Escribe un id. del producto único que se usará como una variable de cadena al integrar este complemento en el código del juego. Este identificador no se mostrará a los consumidores. Para obtener más información, consulta [Establecer el tipo del producto de tu complemento y el id. del producto](https://msdn.microsoft.com/windows/uwp/publish/set-your-add-on-product-id).

Otras configuraciones para complementos incluyen lo siguiente:
* [Propiedades](https://msdn.microsoft.com/windows/uwp/publish/enter-add-on-properties)
* [Precios y disponibilidad](https://msdn.microsoft.com/windows/uwp/publish/set-add-on-pricing-and-availability)
* [Descripción de la Tienda](https://msdn.microsoft.com/windows/uwp/publish/create-add-on-store-listings)

Si el juego tiene muchos complementos, puedes crearlos mediante programación con la __API de envío a la Tienda Windows__. Para obtener más información, consulta [Creación y administración de envíos mediante el uso de servicios de la Tienda Windows](https://msdn.microsoft.com/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services).

## Mostrar anuncios en el juego

Las bibliotecas y las herramientas incluidas en Microsoft Store Services SDK te ayudan a configurar un servicio en tu juego para recibir anuncios provenientes de una red de anuncios. A los jugadores se les mostrarán anuncios dinámicos y podrás ganar dinero con los anunciantes cuando los jugadores vean los anuncios que se muestran o interactúen con ellos. Para obtener más información, consulta [Flujos de trabajo para crear aplicaciones con anuncios](https://msdn.microsoft.com/windows/uwp/monetize/workflows-for-creating-apps-with-ads).

### Formatos de anuncio

Se pueden mostrar dos tipos de anuncios mediante Microsoft Store Services SDK:

* Anuncios de banner: anuncios que ocupan una parte de la pantalla del juego y generalmente se colocan dentro de un juego.
* Anuncios intersticiales en vídeo: anuncios de pantalla completa, que pueden ser muy eficaces cuando se usan entre niveles. Si se implementan correctamente, pueden ser menos molestos que los anuncios de banner.

### ¿Qué anuncios se muestran?

Actualmente, los anuncios se proporcionan a través de nuestras redes de partners cuando usas Microsoft Store Services SDK. Para obtener más información sobre las ofertas actuales, consulta [Rentabilizar las aplicaciones con anuncios](https://developer.microsoft.com/store/monetize/ads-in-apps).
Si usas AdControl para mostrar anuncios, puedes optar por mostrar [anuncios de filiales](https://msdn.microsoft.com/windows/uwp/publish/about-affiliate-ads) al expandir los anuncios de productos que se muestran en el juego.

### ¿Qué mercados permiten mostrar anuncios?

Los anuncios de banner y los anuncios intersticiales en vídeo pueden mostrarse a usuarios de determinados países. Para obtener la lista completa de los países y las regiones que admiten anuncios, consulta [Mercados admitidos para Microsoft Advertising](https://msdn.microsoft.com/windows/uwp/monetize/supported-markets-for-microsoft-advertising).

### API para mostrar anuncios

Las clases [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) e [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) incluidas en Microsoft Store Services SDK, que forma parte del espacio de nombres [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.aspx), se usan para mostrar anuncios en los juegos.

Para empezar, descarga e instala [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) con VisualStudio2015. Para obtener más información, consulta [Características disponibles en el SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk#features-available-in-the-sdk).

#### Guías de implementación

En los siguientes tutoriales se muestra cómo implementar anuncios mediante las clases __AdControl__ y __InterstitialAd__:

* [Crear anuncios de banner mediante la clase AdControl en XAML y .NET](https://msdn.microsoft.com/windows/uwp/monetize/adcontrol-in-xaml-and--net)
* [Crear anuncios de banner mediante la clase AdControl en HTML5 y JavaScript](https://msdn.microsoft.com/windows/uwp/monetize/adcontrol-in-html-5-and-javascript)
* [Crear anuncios intersticiales en vídeo mediante la clase InterstitialAd](https://msdn.microsoft.com/windows/uwp/monetize/interstitial-ads)

Durante el desarrollo, puedes usar los valores de prueba que se muestran a continuación para ver cómo se representan los anuncios. Estos mismos valores también se usan en los tutoriales antes mencionados.

|AdType             | AdUnitId  | AppId                              |
|-------------------|-----------|------------------------------------|
|Anuncios de banner         |10865270   |3f83fe91-d6be-434d-a0ae-7351c5a997f1|
|Anuncios intersticiales   |11389925   |d25517cb-12d4-4699-8bdc-52040c712cab|

Algunos procedimientos recomendados destinados a ayudarte en el proceso de diseño e implementación son los siguientes:

* [Procedimientos recomendados para anuncios de banner mediante la clase AdControl](https://msdn.microsoft.com/windows/uwp/monetize/ui-and-user-experience-guidelines)
* [Procedimientos recomendados para anuncios intersticiales mediante la clase InterstitialAd](https://msdn.microsoft.com/windows/uwp/monetize/ui-and-user-experience-guidelines#interstitialbestpractices10)

Para obtener soluciones a problemas comunes de desarrollo (por ejemplo, anuncios que no se muestran, caja negra que parpadea y desaparece o anuncios que no se actualizan), consulta [Guías de solución de problemas](https://msdn.microsoft.com/windows/uwp/monetize/troubleshooting-guides).

### Preparar el lanzamiento reemplazando los valores de prueba de unidad de anuncio

Cuando estés listo para realizar la prueba en directo o para recibir anuncios de juegos publicados, debes actualizar los valores de prueba de unidad de anuncio a los valores reales proporcionados para el juego.
Para crear unidades de anuncios para el juego, consulta [Configurar unidades de anuncios en la aplicación](https://msdn.microsoft.com/windows/uwp/monetize/set-up-ad-units-in-your-app).

### Otras redes de anuncios

A continuación se detallan otras redes de anuncios que admiten la presentación de anuncios en juegos y aplicaciones para UWP.

#### Vungle

El SDK de Vungle para Windows ofrece anuncios de vídeo en aplicaciones y juegos. Para descargar el SDK, ve a [Vungle SDK](https://v.vungle.com/sdk).

#### Smaato

Smaato permite incorporar anuncios de banner en juegos y aplicaciones para UWP. Descarga el [SDK](https://www.smaato.com/resources/sdks/). Para obtener más información, consulta la [documentación](https://wiki.smaato.com/display/SPX/Windows+Phone).

#### AdDuplex

Puedes usar AdDuplex para implementar banners o anuncios intersticiales en el juego.

Para conocer más detalles sobre cómo integrar AdDuplex directamente en un proyecto de Windows 10 en XAML, visita el sitio web de AdDuplex:
* Anuncios de banner: [Windows 10 SDK for XAML](https://adduplex.zendesk.com/hc/en-us/articles/204849031-Windows-10-SDK-for-XAML-apps-installation-and-usage) (SDK de Windows 10 para XAML) 
* Anuncios intersticiales: [Windows 10 XAML AdDuplex Interstitial Ad Installation and Usage](https://adduplex.zendesk.com/hc/en-us/articles/204849091-Windows-10-XAML-AdDuplex-Interstitial-Ad-Installation-and-Usage) (Uso e instalación de anuncios intersticiales de AdDuplex para Windows 10 en XAML)

Para obtener información sobre cómo integrar el SDK de AdDuplex en juegos para UWP de Windows 10 creados con Unity, consulta [Windows 10 SDK for Unity apps installation and usage](https://adduplex.zendesk.com/hc/en-us/articles/207279435-Windows-10-SDK-for-Unity-apps-installation-and-usage) (SDK de Windows 10 para la instalación y el uso de aplicaciones de Unity).

## Maximizar el potencial del juego a través de campañas publicitarias

El siguiente paso consiste en promocionar el juego a través de anuncios. Al [crear una campaña publicitaria](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app) para tu juego, otras aplicaciones y juegos mostrarán anuncios promocionándolo. 

Elige entre varios tipos de campañas que pueden ayudarte a aumentar la base de jugadores.

|Tipo de campaña             | Los anuncios para el juego aparecen en...                                                                                                                                                                   |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|De pago                      |Aplicaciones que coinciden con el dispositivo o la categoría del juego.                                                                                                                                                   |
|Comunidad gratuita            |Aplicaciones publicadas por otros desarrolladores que también participan en campañas de anuncios de la comunidad. Para obtener más información, consulta [Acerca de los anuncios de la comunidad](https://msdn.microsoft.com/windows/uwp/publish/about-community-ads).|
|Interno gratuito                |Solo las aplicaciones que hayas publicado. Para obtener más información, consulta [Acerca de los anuncios internos](https://msdn.microsoft.com/windows/uwp/publish/about-house-ads).                                                            |

## Vínculos relacionados

* [Recibir pagos](https://msdn.microsoft.com/windows/uwp/publish/getting-paid-apps)
* [Tipos de cuenta, ubicaciones y precios](https://msdn.microsoft.com/windows/uwp/publish/account-types-locations-and-fees)
* [Análisis](https://msdn.microsoft.com/windows/uwp/publish/analytics)
* [Globalización y localización](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [Implementar una versión de prueba de la aplicación](https://msdn.microsoft.com/windows/uwp/monetize/implement-a-trial-version-of-your-app)
* [Ejecutar experimentos para aplicaciones con pruebas A/B](https://msdn.microsoft.com/windows/uwp/monetize/run-app-experiments-with-a-b-testing)


<!--HONumber=Nov16_HO1-->

