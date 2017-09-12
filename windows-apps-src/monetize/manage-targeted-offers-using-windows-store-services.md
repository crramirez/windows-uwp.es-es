---
author: mcleanbyron
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: "Usa la API de ofertas dirigidas de la Tienda Windows para reclamen ofertas dirigidas que están disponibles para una aplicación."
title: Administrar ofertas dirigidas usando los servicios de la Tienda
ms.author: mcleans
ms.date: 05/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, servicios de la Tienda, API de ofertas de destino de la Tienda Windows, ofertas dirigidas
ms.openlocfilehash: 684c37d4439f415ad607b7f3e6a166966cc9f835
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2017
---
# <a name="manage-targeted-offers-using-store-services"></a>Administrar ofertas dirigidas usando los servicios de la Tienda

Si creas una *oferta dirigida* en la página **Interactuar > Ofertas de destino** para la aplicación en el panel del Centro de desarrollo de Windows, usa la *API de ofertas dirigidas de la Tienda Windows* en el código de la aplicación para implementar la experiencia en la aplicación para la oferta dirigida. Para obtener más información sobre las ofertas dirigidas y cómo crearlas en el panel, consulta [Usar ofertas dirigidas para maximizar la interacción y las conversiones](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md).

La API de ofertas dirigidas es una API de REST que puedes usar para realizar estas tareas:

* Obtener las ofertas dirigidas que están disponibles para el usuario actual, en función de si el usuario es parte del segmento de cliente para la oferta dirigida o no.
* Si el usuario compra la oferta dirigida, puedes enviar una reclamación a la Tienda para cobrar la bonificación que está asociada con la oferta dirigida. Esto solo es necesario si la oferta dirigida está asociada con una promoción patrocinada por Microsoft que paga una bonificación a los desarrolladores por cada conversión correcta.

Para usar esta API en el código de tu aplicación, sigue estos pasos:

1.  [Obtén un token de cuenta de Microsoft](#obtain-a-microsoft-account-token) para el usuario que inició sesión actualmente en la aplicación.
2.  [Obtén las ofertas dirigidas para el usuario actual](#get-targeted-offers).
3.  [Compra el complemento que está asociado con una oferta dirigida](#purchase-add-on).
3.  [Reclama la oferta dirigida](#claim-targeted-offer) (si la oferta dirigida está asociada con una promoción patrocinada por Microsoft que paga una bonificación a los desarrolladores por cada conversión correcta).

> [!NOTE]
> La capacidad de asociar una oferta dirigida con una promoción patrocinada por Microsoft y luego enviar una reclamación por la oferta no está disponible actualmente para todas las cuentas de desarrollador.

Para ver un ejemplo de código completo que muestra todos estos pasos, consulta el [ejemplo de código](#code-example) al final de este artículo. En las siguientes secciones se proporcionan más detalles sobre cada paso.

<span id="obtain-a-microsoft-account-token" />
## <a name="get-a-microsoft-account-token-for-the-current-user"></a>Obtener un token de cuenta de Microsoft para el usuario actual

En el código de la aplicación, obtén un token de cuenta de Microsoft (MSA) para el usuario que inició sesión actualmente. Debes pasar este token en el encabezado de la solicitud ```Authorization``` para cada uno de los métodos de la API de ofertas de destino de la Tienda Windows. La Tienda usa este token para recuperar las ofertas dirigidas que están a disposición del usuario actual.

Para obtener el token de MSA, usa la clase [WebAuthenticationCoreManager](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) para solicitar un token usando el ámbito ```devcenter_implicit.basic,wl.basic```. En el siguiente ejemplo, se muestra cómo hacerlo. Este ejemplo es un fragmento del [ejemplo completo](#code-example) y requiere instrucciones **using** que se proporcionan en el ejemplo completo.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetMSAToken)]

Para obtener más información sobre cómo obtener tokens de MSA, consulta [Administrador de cuentas web](../security/web-account-manager.md).

<span id="get-targeted-offers" />
## <a name="get-the-targeted-offers-for-the-current-user"></a>Obtener las ofertas dirigidas para el usuario actual

Una vez que tengas un token de MSA para el usuario actual, llama el método GET de la URI ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` para obtener las ofertas dirigidas disponibles para el usuario actual. Para obtener más información sobre este método REST, consulta [Obtener ofertas dirigidas](get-targeted-offers.md).

Este método devuelve los identificadores de producto de los complementos que están asociados con las ofertas dirigidas que están disponibles para el usuario actual. Con esta información, puedes ofrecer al usuario una o varias de las ofertas dirigidas como una compra desde la aplicación. Además, este método devuelve un identificador de seguimiento que puedes usar para [enviar una reclamación](#claim-targeted-offer) a la Tienda para cobrar cualquier bonificación que esté asociada con una de las ofertas dirigidas.

El siguiente ejemplo muestra cómo obtener las ofertas dirigidas para el usuario actual. Este ejemplo es un fragmento del [ejemplo completo](#code-example). Requiere la biblioteca de [Json.NET](http://www.newtonsoft.com/json) de Newtonsoft y clases adicionales e instrucciones **using** que se proporcionan en el ejemplo completo.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffers)]

<span id="purchase-add-on" />
## <a name="purchase-the-add-on-that-is-associated-with-a-targeted-offer"></a>Comprar el complemento que está asociado con una oferta dirigida

A continuación, ofrece una de las ofertas dirigidas para que la compre el usuario. Si el usuario acepta comprar la oferta dirigida, usa uno de los siguientes métodos para comprar mediante programación el complemento que está asociado con la oferta dirigida. Usa los valores de id. de producto que hayas recuperado en el paso anterior para identificar el complemento que quieras adquirir.

* Si la aplicación está destinada a Windows10, versión 1607 o una versión posterior, se recomienda usar uno de los métodos **RequestPurchaseAsync** en el espacio de nombres [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store). Para obtener más información sobre el uso de estos métodos, consulta [Habilitar compras desde la aplicación de aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md).

* Si la aplicación está destinada a una versión anterior de Windows10, usa el método [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_RequestProductPurchaseAsync_System_String_) en el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) para comprar el complemento. Para obtener más información sobre cómo usar este método, consulta [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md).

Para ver ejemplos de código que muestren cada opción, consulta el [ejemplo de código](#code-example) al final de este artículo.

> [!NOTE]
> Hay dos espacios de nombres diferentes para implementar las compras desde la aplicación en una aplicación para UWP: [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) (introducida en Windows10, versión1607) y [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) (disponible en todas las versiones de Windows10). Para obtener más información sobre las diferencias entre estos espacios de nombres, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md).

<span id="claim-targeted-offer" />
## <a name="submit-a-claim-for-a-targeted-offer"></a>Enviar una reclamación para una oferta dirigida

Si el usuario compra una oferta dirigida que está asociada con una promoción patrocinada por Microsoft que paga una bonificación a los desarrolladores por cada conversión correcta, debes enviar una reclamación a la Tienda para poder cobrar la bonificación. Cuando envíes la solicitud, debes proporcionar un valor que represente la confirmación de compra. La manera de obtener esta confirmación de compra depende de si la aplicación usa las API en el espacio de nombres **Windows.ApplicationModel.Store** o **Windows.Services.Store** para comprar el complemento.

> [!NOTE]
> La capacidad de asociar una oferta dirigida con una promoción patrocinada por Microsoft y luego enviar una reclamación por la oferta no está disponible actualmente para todas las cuentas de desarrollador.

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>Aplicaciones que usan el espacio de nombres Windows.ApplicationModel.Store

1. Tras comprar el complemento mediante el método [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_RequestProductPurchaseAsync_System_String_) en el espacio de nombres **Windows.ApplicationModel.Store** para comprar el complemento, asegúrate de recuperar un [recibo de compra](use-receipts-to-verify-product-purchases.md). Este recibo está disponible en el objeto [PurchaseResults.ReceiptXml](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.purchaseresults#Windows_ApplicationModel_Store_PurchaseResults_ReceiptXml_) que se devuelve por el método **RequestProductPurchaseAsync**. Este recibo representa la confirmación de compra que usará en el siguiente paso.

2. Envía un mensaje POST a la URI ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` para enviar una reclamación para la conversión de la oferta dirigida. Para obtener más información sobre este método REST, consulta [Reclamar una oferta dirigida](claim-a-targeted-offer.md). En el cuerpo de la solicitud, pasa los siguientes datos:

  * Campo *storeOffer*: pasa uno de los objetos JSON que recibiste en el cuerpo de la respuesta del método [Obtener ofertas dirigidas](get-targeted-offers.md) (este objeto debe incluir los campos *offers* y *trackingId*).

  * Campo *receipt*: pasa la cadena de recibo que recuperaste en el paso anterior (está disponible en el objeto **PurchaseResults.ReceiptXml** devuelto por el método **RequestProductPurchaseAsync**).

El siguiente ejemplo muestra cómo comprar y reclamar una oferta dirigida mediante las API del espacio de nombres **Windows.ApplicationModel.Store**. Este ejemplo es un fragmento del [ejemplo completo](#code-example). Requiere la biblioteca de [Json.NET](http://www.newtonsoft.com/json) de Newtonsoft y clases adicionales e instrucciones **using** que se proporcionan en el ejemplo completo.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#ClaimOfferOnAnyVersionWindows10)]

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>Aplicaciones que usan el espacio de nombres Windows.Services.Store

1. Cuando hayas comprado el complemento mediante uno de los métodos **RequestPurchaseAsync** en el espacio de nombres [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store), obtén el *identificador de pedido de la compra* siguiendo estos pasos. Este valor representa la confirmación de compra.

  1. Llama al método [GetUserCollectionAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext#Windows_Services_Store_StoreContext_GetUserCollectionAsync_Windows_Foundation_Collections_IIterable_System_String__) para obtener la colección de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representan los complementos que el usuario ha adquirido.

  2. En esta colección, obtén el objeto **StoreProduct** que representa el complemento que se compró para la oferta dirigida.

  3. Obtén el valor de la propiedad [ExtendedJsonData](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_) para este objeto **StoreProduct**. Esta propiedad devuelve una cadena con formato JSON que contiene los datos completos relacionados con la Tienda para el complemento.

  4. Analiza esta cadena JSON y recupera el valor del campo *orderId* de la cadena.

2. Envía un mensaje POST a la URI ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` para enviar una reclamación para la conversión de la oferta dirigida. Para obtener más información sobre este método REST, consulta [Reclamar una oferta dirigida](claim-a-targeted-offer.md). En el cuerpo de la solicitud, pasa los siguientes objetos:

  * Campo *storeOffer*: pasa uno de los objetos JSON que recibiste en el cuerpo de la respuesta del método [Obtener ofertas dirigidas](get-targeted-offers.md) (este objeto debe incluir los campos *offers* y *trackingId*).

  * Campo *receipt*: pasa el valor *orderId* que recuperaste en el paso anterior.

El siguiente ejemplo muestra cómo comprar y reclamar una oferta dirigida mediante las API del espacio de nombres **Windows.Services.Store**. Este ejemplo es un fragmento del [ejemplo completo](#code-example). Requiere la biblioteca de [Json.NET](http://www.newtonsoft.com/json) de Newtonsoft y clases adicionales e instrucciones **using** que se proporcionan en el ejemplo completo.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#ClaimOfferOnWindows1607AndLater)]

<span id="code-example" />
## <a name="complete-code-example"></a>Ejemplo de código completo

En el siguiente ejemplo de código se demuestran las siguientes tareas:

* Obtener un token de MSA para el usuario actual.
* Obtener todas las ofertas dirigidas para el usuario actual mediante el método [Obtener ofertas dirigidas](get-targeted-offers.md).
* Compra el complemento que está asociado con una oferta dirigida.
* Reclamar la oferta de destino mediante el método [Reclamar una oferta dirigida](claim-a-targeted-offer.md).

Este ejemplo requiere la biblioteca [Json.NET](http://www.newtonsoft.com/json) de Newtonsoft. El ejemplo usa esta biblioteca para serializar y deserializar los datos con formato JSON.

> [!NOTE]
> Hay dos espacios de nombres diferentes para implementar las compras desde la aplicación en una aplicación para UWP: [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) (introducida en Windows10, versión1607) y [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) (disponible en todas las versiones de Windows10). Este ejemplo usa [código adaptativo para versiones](../debug-test-perf/version-adaptive-code.md) usar ambos de estos espacios de nombres en la misma aplicación para comprar un complemento y enviar una reclamación a la oferta dirigida. Para usar este código, la versión del SO de destino de tu proyecto debe ser **Windows 10 Anniversary Edition (10.0; compilación 14394)** o posterior, aunque la versión mínima del SO puede ser una versión anterior si quieres que tu aplicación se ejecute en versiones anteriores de Windows 10. Para obtener más información sobre las diferencias entre estos espacios de nombres, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md).

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffersSample)]

## <a name="related-topics"></a>Temas relacionados

* [Usar ofertas dirigidas para maximizar la interacción y las conversiones](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [Obtener ofertas dirigidas](get-targeted-offers.md)
* [Reclamar una oferta dirigida](claim-a-targeted-offer.md)
