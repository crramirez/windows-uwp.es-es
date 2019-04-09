---
description: Aprende a usar el espacio de nombres Windows.Services.Store para implementar complementos de una suscripción.
title: Habilitar complementos de una suscripción para tu aplicación
keywords: windows 10, uwp, suscripciones, complementos, add-ons, compras desde la aplicación, in-app purchases, IAP, Windows.Services.Store
ms.date: 12/06/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b937ca61110452e233061179c398cae0d047686e
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58335063"
---
# <a name="enable-subscription-add-ons-for-your-app"></a>Habilitar complementos de una suscripción para tu aplicación

Tu aplicación de Plataforma universal de Windows (UWP) puede ofrecer compras desde la aplicación de complementos de *suscripción* a los clientes. Puedes usar suscripciones para vender productos digitales en tu aplicación (como las características de la aplicación o el contenido digital) con períodos de facturación periódicos automatizados.

> [!NOTE]
> Para habilitar la compra de complementos de suscripción en la aplicación, el proyecto debe estar dirigido a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio (esto corresponde a Windows 10, versión 1607), y debe usar las API del espacio de nombres **Windows.Services.Store** para implementar la experiencia de compra desde la aplicación en lugar de usar el espacio de nombres **Windows.ApplicationModel.Store**. Para obtener más información sobre las diferencias entre estos espacios de nombres, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md).

## <a name="feature-highlights"></a>Aspectos destacados de las características

Los complementos de una suscripción para aplicaciones para UWP admiten las siguientes características:

* Puedes elegir entre períodos de suscripción de 1 mes, 3 meses, 6 meses, 1 año o 2 años.
* Puedes agregar períodos de prueba gratuita de 1 semana o 1 mes a tu suscripción.
* Windows SDK [proporciona API](#code-examples) que puedes usar en tu aplicación para obtener información acerca de los complementos de suscripción disponibles para la aplicación y permitir la compra del complemento de una suscripción. También proporcionamos las API de REST a las que puedes llamar desde tus servicios para [administrar suscripciones para un usuario](#manage-subscriptions).
* Puedes ver informes analíticos que proporcionan el número de adquisiciones de suscripción, suscriptores activos y suscripciones canceladas en un período de tiempo determinado.
* Los clientes pueden administrar su suscripción en la página [https://account.microsoft.com/services](https://account.microsoft.com/services) de su cuenta de Microsoft. Los clientes pueden usar esta página para ver todas las suscripciones que han adquirido, cancelar una suscripción y cambiar la forma de pago asociada a su suscripción.

## <a name="steps-to-enable-a-subscription-add-on-for-your-app"></a>Pasos para habilitar el complemento de una suscripción para tu aplicación

Para habilitar la compra de complementos de una suscripción en la aplicación, sigue estos pasos.

1. [Crear una presentación de complemento](../publish/add-on-submissions.md) para su suscripción en el centro de partners y publicar el envío. Al seguir el proceso de envío del complemento, presta mucha atención a las siguientes propiedades:

    * [Tipo de producto](../publish/set-your-add-on-product-id.md#product-type): Asegúrese de seleccionar **suscripción**.

    * [Período de suscripción](../publish/enter-add-on-properties.md#subscription-period): Elija el período de facturación periódico para la suscripción. No puedes cambiar el período de suscripción después de publicar el complemento.

        Cada complemento de suscripción admite un período de suscripción único y período de prueba. Debes crear un complemento de suscripción diferente para cada tipo de suscripción que quieres ofrecer en tu aplicación. Por ejemplo, si quisieras ofrecer una suscripción mensual sin ninguna versión de prueba, una suscripción mensual con una versión de prueba de un mes, una suscripción anual sin ninguna versión de prueba y una suscripción anual con una versión de prueba de un mes, necesitará crear cuatro complementos de suscripción.

    * [Período de prueba](../publish/enter-add-on-properties.md#free-trial): Considere la posibilidad de elegir un período de prueba de 1 semana o mes su suscripción permitir que los usuarios probar el contenido de la suscripción antes de comprarlo. No puedes cambiar ni quitar el período de prueba después de publicar el complemento de la suscripción.

        Para obtener una evaluación gratuita de tu suscripción, un usuario debe adquirir tu suscripción a través del proceso de compra desde la aplicación estándar, como una forma de pago válida. No se le cobra dinero durante el período de prueba. Al final del período de prueba, la suscripción se convierte automáticamente en la suscripción completa y se te cobrará el instrumento de pago del usuario para el primer período de la suscripción de pago. Si el usuario elige cancelar su suscripción durante el período de prueba, la suscripción permanece activa hasta el final del período de prueba. Algunos períodos de prueba no están disponibles para todos los períodos de suscripción.

        > [!NOTE]
        > Cada cliente puede adquirir una prueba gratuita durante un complemento de suscripción solo una vez. Una vez que un cliente adquiere una prueba gratuita de una suscripción, la Store impide que el mismo cliente adquiera de nuevo la mismo suscripción de prueba gratuita.

    * [Visibilidad](../publish/set-add-on-pricing-and-availability.md#visibility): Si va a crear un complemento de prueba que sólo va a utilizar para probar la experiencia de compra en la aplicación para la suscripción, se recomienda que seleccione uno de los **oculto en el Store** opciones. De lo contrario, puedes seleccionar la mejor opción de visibilidad para tu escenario.

    * [Precios](../publish/set-add-on-pricing-and-availability.md?#pricing): Elija el precio de la suscripción en esta sección. No puedes subir el precio de la suscripción después de publicar el complemento. Sin embargo, puede bajar el precio posteriormente:
        > [!IMPORTANT]
        > De manera predeterminada, al crear cualquier complemento, el precio se establece inicialmente en **Gratis**. Dado que no puedes subir el precio del complemento de una suscripción después de completar el envío del complemento, asegúrate de elegir el precio de tu suscripción aquí.

2. En tu aplicación, usa las API en el espacio de nombres [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) para determinar si el usuario actual ya ha adquirido el complemento de la suscripción y luego ofrécelo para su venta al usuario como una compra desde la aplicación. Consulta los [ejemplos de código](#code-examples) en este artículo para obtener más detalles.

3. Prueba la implementación de la compra desde la aplicación de la suscripción en la aplicación. Deberás descargar la aplicación una vez de la Tienda en el dispositivo de desarrollo para usar su licencia para las pruebas. Para obtener más información, consulta nuestra [guía para prueba](in-app-purchases-and-trials.md#testing) para las compras desde la aplicación.  

4. Crea y publica un envío de aplicación que incluya el paquete de la aplicación actualizada, incluido el código probado. Para más información, consulta [Envíos de aplicaciones](../publish/app-submissions.md).

<span id="code-examples"/>

## <a name="code-examples"></a>Ejemplos de código

Los ejemplos de código de esta sección muestran cómo usar las API en el espacio de nombres [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) para obtener información acerca de los complementos de una suscripción para la aplicación actual y solicitar la compra del complemento de una suscripción en nombre del usuario actual.

Estos ejemplos cumplen los siguientes requisitos:
* Un proyecto de Visual Studio para una aplicación de la Plataforma universal de Windows (UWP) destinado a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o un versión posterior.
* Tiene [creó un envío de la aplicación](https://docs.microsoft.com/windows/uwp/publish/app-submissions) en el centro de partners y esta aplicación se publica en el Store. De manera opcional, puedes configurar la aplicación para que no se pueda descubrir en la Tienda mientras la pruebas. Para obtener más información, consulta la [guía para prueba](in-app-purchases-and-trials.md#testing).
* Tiene [crea un complemento de suscripción para la aplicación](../publish/add-on-submissions.md) en el centro de partners.

El código de estos ejemplos supone que:
* El archivo de código requiere el **uso** de instrucciones para los espacios de nombres **Windows.Services.Store** y **System.Threading.Tasks**.
* La aplicación es una de usuario único que se ejecuta solamente en el contexto del usuario que la inició. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si tienes una aplicación de escritorio que usa el [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop), puede que tengas que agregar código adicional que no se muestra en estos ejemplos para configurar el objeto [**StoreContext**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext). Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

### <a name="purchase-a-subscription-add-on"></a>Comprar el complemento de una suscripción

Este ejemplo muestra cómo solicitar la compra de un complemento de suscripción conocido para tu aplicación en nombre del cliente actual. En este ejemplo también se muestra cómo controlar el caso de que la suscripción posea un período de prueba.

1. En primer lugar, el código determina si el cliente ya tiene una licencia activa para la suscripción. Si el cliente ya tiene una licencia activa, el código debe desbloquear las características de suscripción según sea necesario (dado que es propiedad de la aplicación, se identifica con un comentario en el ejemplo).
2. A continuación, el código obtiene el objeto [**StoreProduct**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) que representa la suscripción que quieras adquirir en nombre del cliente. El código da por hecho que ya conoces el [Id. de Store](in-app-purchases-and-trials.md#store-ids) del complemento de suscripción que quieras adquirir y que has asignado este valor a la variable *subscriptionStoreId*.
3. El código, a continuación, determina si una versión de prueba está disponible para la suscripción. Opcionalmente, tu aplicación puede utilizar esta información para mostrar los detalles acerca de la prueba o suscripción completa disponible para el cliente.
4. Finalmente, el código llama al método [**RequestPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.RequestPurchaseAsync) para solicitar la compra de la suscripción. Si hay una versión de prueba para la suscripción, se ofrecerá al cliente la versión de prueba para la compra. De lo contrario, se te ofrecerá la suscripción completa para la compra.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseSubscriptionAddOnTrialPage.xaml.cs#PurchaseTrialSubscription)]

### <a name="get-info-about-subscription-add-ons-for-the-current-app"></a>Obtener información sobre los complementos de una suscripción para la aplicación actual

Este ejemplo de código muestra cómo obtener información para todos los complementos de una suscripción que están disponibles en tu aplicación. Para obtener esta información, usa primero el método [**GetAssociatedStoreProductsAsync**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsAsync) para obtener la colección de objetos [**StoreProduct**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) que representan cada uno de los complementos disponibles para la aplicación. A continuación, obtén el objeto [**StoreSku**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) para cada producto y usa las propiedades [**IsSubscription**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.IsSubscription) y [**SubscriptionInfo**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.SubscriptionInfo) para acceder a la información de la suscripción.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/GetSubscriptionAddOnsPage.xaml.cs#GetSubscriptions)]

<span id="manage-subscriptions" />

## <a name="manage-subscriptions-from-your-services"></a>Administrar suscripciones desde tus servicios

Una vez que la aplicación actualizada esté en la Tienda y los clientes puedan comprar el complemento de la suscripción, es posible que tengas escenarios en los que tengas que administrar la suscripción para un cliente. Proporcionamos API de REST a las puedes llamar desde tus servicios para realizar las siguientes tareas de administración de suscripciones:

* [Obtener las suscripciones que un usuario tiene derecho de usar](get-subscriptions-for-a-user.md). Si tu suscripción forma parte de un servicio multiplataforma, puede llamar a esta API para determinar si el usuario especificado tiene un derecho para tu suscripción y el estado de su suscripción en el contexto de tu aplicación para UWP. A continuación, puedes usar esta información para actualizar el estado de la suscripción en otras plataformas admitidas por tu servicio.

* [Cambiar el estado de facturación de la suscripción de un usuario determinado](change-the-billing-state-of-a-subscription-for-a-user.md). Usa esta API para cancelar, ampliar o deshabilitar la renovación automática de una suscripción.

## <a name="cancellations"></a>Cancelaciones

Los clientes pueden usar la página [https://account.microsoft.com/services](https://account.microsoft.com/services) de su cuenta de Microsoft para ver todas las suscripciones que han adquirido, cancelar una suscripción y cambiar la forma de pago asociada a su suscripción. Cuando un cliente cancela una suscripción con esta página, continúa teniendo acceso a la suscripción durante la vigencia del periodo de facturación actual. No obtienen reembolso de ninguna parte del periodo de facturación actual. Al final del periodo de facturación actual, su suscripción se desactiva.

También puedes cancelar una suscripción en nombre de un usuario con nuestra API de REST para [cambiar el estado de facturación de la suscripción de un usuario determinado](change-the-billing-state-of-a-subscription-for-a-user.md).

## <a name="subscription-renewals-and-grace-periods"></a>Renovaciones de suscripciones y períodos de gracia

En algún momento durante cada período de facturación, intentamos realizar el cargo en la tarjeta de crédito del cliente del siguiente período de facturación. Si se produce un error en el cargo, la suscripción del cliente entra en el estado de *notificación*. Esto significa que su suscripción aún está activa durante el resto del período de facturación actual, pero se intentará periódicamente realizar el cargo en su tarjeta de crédito para renovar la suscripción. Este estado puede durar hasta dos semanas hasta el final del período de facturación actual y la fecha de renovación del siguiente período de facturación.

No ofrecemos período de gracia para la facturación de la suscripción. Si no podemos realizar el cargo correctamente en la tarjeta de crédito del cliente al final del período de facturación actual, se cancelará la suscripción y el cliente ya no tendrá acceso a la suscripción después del período de facturación actual.

## <a name="unsupported-scenarios"></a>Escenarios no admitidos

Los siguientes escenarios no son compatibles actualmente con complementos de una suscripción.

* En este momento no se admite la venta de suscripciones a clientes directamente a través de la Tienda. Las suscripciones solo están disponibles para compras desde la aplicación de productos digitales.
* Los clientes no pueden cambiar periodos de suscripción utilizando la página [https://account.microsoft.com/services](https://account.microsoft.com/services) de su cuenta de Microsoft. Para cambiar a un período de suscripción diferente, los clientes deben cancelar su suscripción actual y, después, comprar una suscripción con un período de suscripción diferente de la aplicación.
* El cambio de nivel no se admite actualmente para complementos de una suscripción (por ejemplo, el cambio de un cliente de una suscripción básica a una suscripción premium con más características).
* Las [Ventas](../publish/put-apps-and-add-ons-on-sale.md) y los [códigos promocionales](../publish/generate-promotional-codes.md) no se admiten actualmente para complementos de una suscripción.


## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener la información de producto para las aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
