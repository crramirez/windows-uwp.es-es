---
title: Monetización para juegos
description: Implementa anuncios de banner, anuncios intersticiales en vídeo y compras desde la aplicación para juegos para Plataforma universal de Windows (UWP) en Windows 10.
ms.assetid: 79f4e177-d8e7-45d3-8a78-31d4c2fe298a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, monetización
ms.localizationpriority: medium
ms.openlocfilehash: c827c257947ea0f365bafe497e627841b501d40d
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878588"
---
#  <a name="monetization-for-games"></a>Monetización para juegos

Como desarrollador de juegos, debes conocer las opciones de monetización para que puedas mantener tu negocio y seguir haciendo lo que te apasiona: crear juegos espectaculares. En este artículo se proporciona información general sobre los métodos de monetización de juegos para Plataforma universal de Windows (UWP) y cómo implementar dichos métodos.

En el pasado, simplemente hubieses puesto un precio a tu juego y luego hubieses esperado a que las personas lo compraran en una tienda. Pero hoy en día, tienes diferentes opciones. Puedes distribuir un juego en tiendas "físicas", venderlo en línea (copias físicas o electrónicas) o permitir que todas las personas lo usen de forma gratuita, pero incorporar algún tipo de anuncio o artículos dentro del juego que se puedan comprar. Los juegos ya no son simplemente productos independientes. Suelen venir con contenido adicional que se puede comprar además del juego principal.

Puedes promocionar y monetizar un juego para UWP de una o varias de las siguientes maneras:
* Ponga tu juego en el Microsoft Store, que es una tienda en línea segura que ofrece una [distribución en todo el mundo](#worldwide-distribution-channel). Los jugadores de todo el mundo pueden comprar un juego en línea al [precio que determines](#set-a-price-for-your-game).
* Usa las API de Windows SDK para crear [compras dentro del juego](#in-game-purchases). Los jugadores pueden comprar artículos desde el juego o comprar contenido adicional, por ejemplo, equipo adicional, máscaras, mapas o niveles de juego.
* Use las API del [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) para mostrar anuncios de redes de ad. Puedes [mostrar anuncios en el juego](#display-ads-in-your-game) y ofrecer a los jugadores la opción de ver anuncios en vídeo a cambio de recompensas dentro del juego.
* [Maximice el potencial del juego a través de las campañas de anuncios](#maximize-your-games-potential-through-ad-campaigns). Promociona tu juego mediante campañas de anuncios de pago, comunitarias (gratuitas) o internas (gratuitas) para aumentar la base de usuarios del juego.

## <a name="worldwide-distribution-channel"></a>Canal de distribución en todo el mundo

El Microsoft Store puede hacer que su juego esté disponible para su descarga en más de 200 países y regiones de todo el mundo, con soporte para la facturación a través de diferentes formas de pago, como Visa, MasterCard y PayPal. Para obtener una lista completa de países y regiones, consulte [definir la selección del mercado](../publish/define-market-selection.md).

## <a name="set-a-price-for-your-game"></a>Establecer un precio para el juego

Los juegos para UWP que se publican en la Tienda pueden ser _de pago_ o _gratuitos_. Un juego de pago te permite cobrar a los jugadores de antemano por el juego un precio que estableces, mientras que un juego gratuito permite a los usuarios descargar y usar el juego sin necesidad de pagarlo.

A continuación se detallan algunos conceptos importantes en relación con el precio del juego en la Tienda.

### <a name="base-price"></a>Precio base

El precio base del juego es lo que determina si el juego se clasifica como _de pago_ o _gratuito_. Puede usar el [centro de Partners](https://partner.microsoft.com/dashboard) para configurar el precio base según el país y la región.
Es posible que el proceso de determinación del precio incluya tus [responsabilidades fiscales si vendes en diferentes países](../publish/tax-details-for-paid-apps.md) y las [consideraciones relativas a los precios para mercados concretos](../publish/define-market-selection.md). También puedes [establecer precios personalizados para mercados específicos](../publish/set-and-schedule-app-pricing.md#override-base-price-for-specific-markets).

### <a name="sale-price"></a>Precio de venta

Una forma de promocionar el juego es reducir el precio durante un tiempo limitado. También es posible establecer el precio de venta en __Gratuito__ para permitir la descarga del juego sin necesidad de pago.
Puedes programar campañas de oferta de antemano al definir la fecha de inicial y final de la oferta. Para obtener más información, consulta [Poner aplicaciones y complementos en oferta](../publish/put-apps-and-add-ons-on-sale.md).

## <a name="in-game-purchases"></a>Compras desde el juego

Las compras desde el juego son productos que se compran dentro de un juego. También se conocen de forma genérica como _compras desde la aplicación_. En el Microsoft Store, estos productos se denominan _Complementos_. Los [Complementos se publican](../publish/add-on-submissions.md) a través del centro de Partners. También tendrás que habilitar los complementos en el código del juego.

### <a name="types-of-add-ons"></a>Tipos de complementos

Puedes crear dos tipos de complementos en la Tienda: _duraderos_ o _consumibles_. Los complementos duraderos son artículos que persisten durante un período de tiempo especificado y se pueden comprar una sola vez hasta que expiran. Los complementos consumibles son artículos que se puedan comprar y usar una y otra vez.

Al crear consumibles, decida cómo desea realizar un seguimiento de ellos, &mdash; ya sea administrado por el _desarrollador_ o _administrado_ por el almacén (esta característica está disponible a partir de Windows 10, versión 1607). Con un consumible administrado por el desarrollador, usted es responsable de realizar un seguimiento del saldo del artículo para el jugador. con un consumible administrado por el almacén, el Microsoft Store realiza un seguimiento del saldo del elemento. Para obtener más información, consulta [Introducción a los complementos consumibles](../monetize/enable-consumable-add-on-purchases.md).

### <a name="create-in-game-purchases"></a>Crear compras desde el juego

Las API más recientes de información de licencia y compras desde la aplicación forman parte del espacio de nombres [Windows.Services.Store](/uwp/api/windows.services.store) de Windows SDK (a partir de Windows 10, versión 1607). Si vas a desarrollar un juego nuevo destinado a la versión 1607 o posterior, te recomendamos que uses el espacio de nombres __Windows.Services.Store__ porque admite los tipos de complemento más recientes y tiene un mejor rendimiento.
También está diseñado para ser compatible con los tipos de productos y características futuros admitidos por el centro de Partners y el almacén. Al desarrollar para versiones anteriores de Windows 10, usa el espacio de nombres [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) en su lugar.

Para obtener más información, visita [Pruebas y compras desde la aplicación](../monetize/in-app-purchases-and-trials.md).

#### <a name="simplified-purchase-example"></a>Ejemplo de compra simplificado

En esta sección se usa un ejemplo de compra simplificado para ilustrar el uso de llamadas a diferentes métodos para implementar el flujo de compra.

|Acciones dentro del juego/actividad                                                | Tareas en segundo plano del juego                  |
|--------------------------------------------------------------------------|----------------------------------------|
|El jugador entra a una tienda. Aparece el menú de la tienda para mostrar los complementos disponibles y el precio de compra. |  El juego [recupera la información del producto](../monetize/get-product-info-for-apps-and-add-ons.md) de los complementos, [determina si los complementos tienen la licencia adecuada](../monetize/get-license-info-for-apps-and-add-ons.md) y muestra los complementos que están disponibles para que el jugador los compre en el menú de la tienda.                           |
|El jugador hace clic en __Comprar__ para comprar un artículo.             |La acción __Comprar__ envía una solicitud para comprar el artículo e inicia el proceso de pago para obtenerlo. La implementación varía según el tipo de artículo. Si se trata de [un artículo duradero o una compra de pago único](../monetize/enable-in-app-purchases-of-apps-and-add-ons.md), el cliente puede ser propietario de un solo artículo hasta que expire. Si el artículo es un [complemento consumible](../monetize/enable-consumable-add-on-purchases.md), el cliente puede ser propietario de uno o varios de ellos. |

### <a name="test-in-game-purchases-during-game-development"></a>Probar las compras desde el juego durante el desarrollo del juego

Dado que un complemento se debe crear junto con un juego, tu juego debe publicarse y estar disponible en la Tienda. Los pasos descritos en esta sección muestran cómo crear complementos mientras el juego aún está en la etapa de desarrollo.
(Si el juego terminado ya está disponible en la Tienda, puedes omitir los tres primeros pasos e ir directamente a [Crear un complemento en la Tienda](#create-an-add-on-in-the-store)).

Para crear complementos mientras el juego aún está en la etapa de desarrollo:
1. [Creación de un paquete](#create-a-package)
2. [Publicar el juego como oculto](#publish-the-game-as-hidden)
3. [Asociar la solución de juego de Visual Studio a la Tienda](#associate-your-game-solution-with-the-store)
4. [Crear un complemento en la Tienda](#create-an-add-on-in-the-store)

#### <a name="create-a-package"></a>Creación de un paquete

Para publicar cualquier juego, este debe cumplir los requisitos mínimos de certificación de aplicaciones en Windows. Puedes usar el [Kit para la certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md), que se incluye en el SDK de Windows 10, para ejecutar pruebas en el juego que te ayudarán a asegurarte de que está listo para publicarlo en la Tienda. Si todavía no descargaste el SDK de Windows 10 que incluye el Kit para la certificación de aplicaciones en Windows, ve a [SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk).

Para crear un paquete que se puede cargar en la Tienda:

1. Abre la solución de juego en Visual Studio.
2. En Visual Studio, vaya a almacén de __proyectos__  >  __Store__  >  __crear paquetes de aplicaciones...__
3. En la opción __¿desea compilar paquetes para cargar en el Microsoft Store?__ , seleccione __sí__.
4. Inicie sesión en su cuenta de desarrollador del [centro de Partners](https://partner.microsoft.com/dashboard) . O bien, [regístrate](https://developer.microsoft.com/store/register) para obtener una cuenta de desarrollador si no tienes ninguna.
5. Selecciona una aplicación para la que se va a crear el paquete de carga. Si aún no creaste un envío de aplicación, proporciona un nuevo nombre de aplicación para crear un nuevo envío. Para obtener más información, consulta [Crear la aplicación reservando un nombre](../publish/create-your-app-by-reserving-a-name.md).
6. Una vez creado correctamente el paquete, haz clic en __Iniciar el Kit para la certificación de aplicaciones en Windows__ para comenzar el proceso de prueba.
7. Corrige los errores para crear un paquete de juego.

#### <a name="publish-the-game-as-hidden"></a>Publicar el juego como oculto

1. Vaya al [centro de Partners](https://partner.microsoft.com/dashboard) e inicie sesión.
2. En la página __Información general del panel__ o __Todas las aplicaciones__, haz clic en la aplicación con la que quieres trabajar. Si aún no creaste un envío de aplicación, haz clic en __Crear una nueva aplicación__ y reserva un nombre.
3. En la página __Información general de la aplicación__, haz clic en __Iniciar el envío__.
4. Configura este nuevo envío. En la página de envío:
    * Haz clic en __Precios y disponibilidad__. En la sección __visibilidad__ , elija "__ocultar esta aplicación y evitar la adquisición...__" para asegurarse de que solo el equipo de desarrollo tiene acceso al juego. Para obtener más información, consulta [Distribución y visibilidad](../publish/set-app-pricing-and-availability.md).
    * Haga clic en __Propiedades__. En la sección __Categoría y subcategoría__, elige __Juegos__ y luego elige una subcategoría adecuada para el juego.
    * Haz clic en __Clasificaciones por edades__. Completa el cuestionario con precisión.
    * Haz clic en __Paquetes__. Carga el paquete de juego creado en el paso anterior.
5. Sigue toda otra indicación de envío que se muestre en el panel para que puedas publicar correctamente este juego que permanece oculto para el público.
6. Haz clic en __Enviar a la Tienda__.

Para obtener más información, consulta [Envíos de aplicaciones](../publish/app-submissions.md).

Cuando el juego se envía a la Tienda, pasa al [proceso de certificación de la aplicación](../publish/the-app-certification-process.md). Este proceso puede tardar hasta 16 horas antes de que se muestre el juego.

#### <a name="associate-your-game-solution-with-the-store"></a>Asociar la solución de juego a la Tienda

Con la solución de juego abierta en Visual Studio:

1. Vaya a __Project__  >  __Store__  >  __la tienda del proyecto asociar aplicación con la tienda..__ .
2. Inicie sesión en su cuenta de desarrollador del centro de Partners y seleccione el nombre de la aplicación con la que asociar esta solución.
3. Haz doble clic en el __archivo Package.appxmanifest.xml__ y ve a la pestaña __Empaquetado__ para comprobar que el juego se asoció correctamente.

Si asociaste la solución a un juego publicado que está disponible y figura en la Tienda, la solución tendrá una licencia activa y, de este modo, ya estás un paso más cerca de la creación de complementos para tu juego. Para más información, consulta [Empaquetado de aplicaciones](../packaging/index.md).

#### <a name="create-an-add-on-in-the-store"></a>Crear un complemento en la Tienda

A medida que creas complementos, asegúrate de asociarlos al envío de juego correcto. Para conocer más detalles sobre cómo configurar toda la información asociada a un complemento, consulta [Envíos de complementos](../publish/add-on-submissions.md).

1. Vaya al [centro de Partners](https://partner.microsoft.com/dashboard) e inicie sesión.
2. En la página __Información general del panel__ o __Todas las aplicaciones__, haz clic en la aplicación para la que quieres crear el complemento.
3. En la página __Información general de la aplicación__, en la sección __Complementos__, selecciona __Crear un nuevo complemento__.
4. Selecciona el tipo de producto para el complemento: __consumible administrado por el desarrollador__, __consumible administrado por la Tienda__ o __Duradero__.
5. Escribe un id. del producto único que se usará como una variable de cadena al integrar este complemento en el código del juego. Este identificador no se mostrará a los consumidores. Para obtener más información, consulta [Establecer el tipo del producto de tu complemento y el id. del producto](../publish/set-your-add-on-product-id.md).

Otras configuraciones para complementos incluyen lo siguiente:
* [Propiedades](../publish/enter-add-on-properties.md)
* [Precios y disponibilidad](../publish/set-add-on-pricing-and-availability.md)
* [Lista de tiendas](../publish/create-add-on-store-listings.md)

Si el juego tiene muchos complementos, puede crearlos mediante programación con la __API de envío de Microsoft Store__. Para obtener más información, vea [crear y administrar envíos con Microsoft Store Services](../monetize/create-and-manage-submissions-using-windows-store-services.md).

## <a name="display-ads-in-your-game"></a>Mostrar anuncios en el juego

Las bibliotecas y herramientas del SDK de Microsoft Advertising le ayudan a configurar un servicio en el juego para recibir anuncios de una red de anuncios. A los jugadores se les mostrarán anuncios dinámicos y podrás ganar dinero con los anunciantes cuando los jugadores vean los anuncios que se muestran o interactúen con ellos.
Para obtener más información, consulte [mostrar anuncios en la aplicación](../monetize/display-ads-in-your-app.md).

### <a name="ad-formats"></a>Formatos de anuncio

Se pueden mostrar varios tipos de anuncios mediante el SDK de Microsoft Advertising:

* Anuncios &mdash; de banner que forman parte de la pantalla de juegos y que normalmente se colocan dentro de un juego.
* Anuncios intersticiales &mdash; en vídeo en pantalla completa, que pueden ser muy eficaces cuando se usan entre niveles. Si se implementan correctamente, pueden ser menos molestos que los anuncios de banner.
* Anuncios nativos &mdash; basados en componentes, donde cada parte de la creatividad de ad (como el título, la imagen, la descripción y el texto de llamada a acción) se entrega a la aplicación como un elemento individual que se puede integrar en la aplicación.

### <a name="which-ads-are-displayed"></a>¿Qué anuncios se muestran?

De forma predeterminada, la aplicación mostrará anuncios de la red de Microsoft para anuncios de pago. Para maximizar los ingresos de anuncios, puede habilitar la mediación de anuncios para que la unidad de anuncio muestre anuncios de redes de anuncios de pago adicionales. Para obtener más información sobre las ofertas actuales, consulte nuestra guía de [mediación de anuncios](../publish/in-app-ads.md#mediation) .

### <a name="which-markets-allow-ads-to-be-displayed"></a>¿Qué mercados permiten mostrar anuncios?

Para obtener la lista completa de los países y las regiones que admiten anuncios, consulte [mercados de soporte técnico para redes de ad](../publish/in-app-ads.md#network-markets).

### <a name="apis-for-displaying-ads"></a>API para mostrar anuncios

Las clases [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol), [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad)y [NativeAd](/uwp/api/microsoft.advertising.winrt.ui.nativead) del SDK de Microsoft Advertising se usan para ayudar a mostrar anuncios en juegos.

Para empezar, descargue e instale el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) con Visual Studio 2015 o una versión posterior. Para obtener más información, vea [instalar el SDK de Microsoft Advertising](../monetize/install-the-microsoft-advertising-libraries.md).

#### <a name="implementation-guides"></a>Guías de implementación

En estos tutoriales se muestra cómo implementar anuncios mediante __AdControl__, __InterstitialAd__y __NativeAd__:

* [Crear anuncios de banner en XAML y .NET](../monetize/adcontrol-in-xaml-and--net.md)
* [Creación de anuncios de banner en HTML5 y JavaScript](../monetize/adcontrol-in-html-5-and-javascript.md)
* [Crear anuncios intersticiales](../monetize/interstitial-ads.md)
* [Crear anuncios nativos](../monetize/native-ads.md)

Durante el desarrollo, puede usar los valores de [unidad de ad de prueba](../monetize/set-up-ad-units-in-your-app.md) para ver cómo se representan los anuncios. Estos valores de unidad de ad de prueba también se usan en los tutoriales anteriores.

Algunos procedimientos recomendados destinados a ayudarte en el proceso de diseño e implementación son los siguientes:

* [Prácticas recomendadas para anuncios de banner](../monetize/ui-and-user-experience-guidelines.md)
* [Procedimientos recomendados para anuncios intersticiales](../monetize/ui-and-user-experience-guidelines.md)

Para obtener soluciones a problemas comunes de desarrollo (por ejemplo, anuncios que no se muestran, caja negra que parpadea y desaparece o anuncios que no se actualizan), consulta [Guías de solución de problemas](../monetize/known-issues-for-the-advertising-libraries.md).

### <a name="prepare-for-release-by-replacing-ad-unit-test-values"></a>Preparar el lanzamiento reemplazando los valores de prueba de unidad de anuncio

Cuando estés listo para realizar la prueba en directo o para recibir anuncios de juegos publicados, debes actualizar los valores de prueba de unidad de anuncio a los valores reales proporcionados para el juego. Para crear unidades de anuncios para el juego, consulta [Configurar unidades de anuncios en la aplicación](../monetize/set-up-ad-units-in-your-app.md).

### <a name="other-ad-networks"></a>Otras redes de anuncios

Se trata de otras redes de ad que proporcionan SDK para el servicio de anuncios a aplicaciones y juegos de UWP.

#### <a name="vungle"></a>Vungle

El SDK de Vungle para Windows ofrece anuncios de vídeo en aplicaciones y juegos. Para descargar el SDK, ve a [Vungle SDK](https://publisher.vungle.com/sdk/).

#### <a name="smaato"></a>Smaato

Smaato permite incorporar anuncios de banner en juegos y aplicaciones para UWP. Descarga el [SDK](https://www.smaato.com/resources/sdks/). Para obtener más información, consulta la [documentación](https://wiki.smaato.com/display/SPX/Windows+Phone).

#### <a name="adduplex"></a>AdDuplex

Puedes usar AdDuplex para implementar banners o anuncios intersticiales en el juego.

Para conocer más detalles sobre cómo integrar AdDuplex directamente en un proyecto de Windows 10 en XAML, visita el sitio web de AdDuplex:
* Anuncios de banner: [Windows 10 SDK for XAML](https://adduplex.zendesk.com/hc/articles/204849031-Windows-10-SDK-for-XAML-apps-installation-and-usage) (SDK de Windows 10 para XAML)
* Anuncios intersticiales: [Windows 10 XAML AdDuplex Interstitial Ad Installation and Usage](https://adduplex.zendesk.com/hc/articles/204849091-Windows-10-XAML-AdDuplex-Interstitial-Ad-Installation-and-Usage) (Uso e instalación de anuncios intersticiales de AdDuplex para Windows 10 en XAML)

Para obtener información sobre cómo integrar el SDK de AdDuplex en juegos para UWP de Windows 10 creados con Unity, consulta [Windows 10 SDK for Unity apps installation and usage](https://adduplex.zendesk.com/hc/articles/207279435-Windows-10-SDK-for-Unity-apps-installation-and-usage) (SDK de Windows 10 para la instalación y el uso de aplicaciones de Unity).

## <a name="maximize-your-games-potential-through-ad-campaigns"></a>Maximizar el potencial del juego a través de campañas publicitarias

El siguiente paso consiste en promocionar el juego a través de anuncios. Al [crear una campaña publicitaria](../monetize/index.md) para tu juego, otras aplicaciones y juegos mostrarán anuncios promocionándolo.

Elige entre varios tipos de campañas que pueden ayudarte a aumentar la base de jugadores.

|Tipo de campaña             | Los anuncios para el juego aparecen en...                                                                                                                                                                   |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Pagado                      |Aplicaciones que coinciden con el dispositivo o la categoría del juego.                                                                                                                                                   |
|Comunidad gratuita            |Aplicaciones publicadas por otros desarrolladores que también participan en campañas de anuncios de la comunidad. Para obtener más información, consulta [Acerca de los anuncios de la comunidad](../monetize/index.md).|
|Interno gratuito                |Solo las aplicaciones que hayas publicado. Para obtener más información, consulta [Acerca de los anuncios internos](../monetize/index.md).                                                            |

## <a name="related-links"></a>Vínculos relacionados

* [Proceso de pago](../publish/getting-paid-apps.md)
* [Tipos de cuenta, ubicaciones y tarifas](../publish/account-types-locations-and-fees.md)
* [Analytics](../publish/analytics.md)
* [Globalización y localización](../design/globalizing/globalizing-portal.md)
* [Implementar una versión de prueba de la aplicación](../monetize/implement-a-trial-version-of-your-app.md)
* [Ejecuta experimentos para aplicaciones con pruebas A/B](../monetize/run-app-experiments-with-a-b-testing.md)