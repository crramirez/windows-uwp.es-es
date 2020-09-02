---
description: Aprenda a usar el espacio de nombres Windows. Services. Store para implementar complementos de suscripción.
title: Habilitar complementos de una suscripción para la aplicación
keywords: Windows 10, UWP, suscripciones, complementos, compras desde la aplicación, IAPs, Windows. Services. Store
ms.date: 12/06/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 844af95545e34dab8adb6698624fcd0dccb2ab30
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362808"
---
# <a name="enable-subscription-add-ons-for-your-app"></a>Habilitar complementos de una suscripción para la aplicación

La aplicación Plataforma universal de Windows (UWP) puede ofrecer compras desde la aplicación de complementos de *suscripción* a los clientes. Puede usar las suscripciones para vender productos digitales en la aplicación (por ejemplo, características de la aplicación o contenido digital) con períodos de facturación recurrentes.

> [!NOTE]
> Para habilitar la compra de complementos de suscripción en la aplicación, el proyecto debe tener como destino la **edición de aniversario de Windows 10 (10,0; Compilación 14393)** o una versión posterior en Visual Studio (esto corresponde a Windows 10, versión 1607) y debe usar las API del espacio de nombres **Windows. Services. Store** para implementar la experiencia de compra desde la aplicación en lugar del espacio de nombres **Windows. ApplicationModel. Store** . Para obtener más información sobre las diferencias entre estos espacios de nombres, consulte [compras y pruebas desde la aplicación](in-app-purchases-and-trials.md).

## <a name="feature-highlights"></a>Características destacadas

Los complementos de suscripción para aplicaciones UWP admiten las siguientes características:

* Puede elegir entre períodos de suscripción de 1 mes, 3 meses, 6 meses, 1 año o 2 años.
* Puede Agregar períodos de evaluación gratuita de 1 semana o 1 mes a su suscripción.
* El Windows SDK [proporciona las API](#code-examples) que puede usar en la aplicación para obtener información sobre los complementos de suscripción disponibles para la aplicación y para habilitar la compra de un complemento de suscripción. También proporcionamos las API de REST a las que puede llamar desde los servicios para [administrar las suscripciones de un usuario](#manage-subscriptions).
* Puede ver los informes analíticos que proporcionan el número de adquisiciones de suscripción, los suscriptores activos y las suscripciones canceladas en un período de tiempo determinado.
* Los clientes pueden administrar su suscripción en la [https://account.microsoft.com/services](https://account.microsoft.com/services) Página para su cuenta de Microsoft. Los clientes pueden usar esta página para ver todas las suscripciones adquiridas, cancelar una suscripción y cambiar la forma de pago asociada a su suscripción.

## <a name="steps-to-enable-a-subscription-add-on-for-your-app"></a>Pasos para habilitar un complemento de suscripción para la aplicación

Para habilitar la compra de complementos de suscripción en la aplicación, siga estos pasos.

1. [Cree un envío de complementos](../publish/add-on-submissions.md) para su suscripción en el centro de Partners y publique el envío. A medida que siga el proceso de envío de complementos, preste especial atención a las siguientes propiedades:

    * [Tipo de producto](../publish/set-your-add-on-product-id.md#product-type): Asegúrese de seleccionar **suscripción**.

    * [Período de suscripción](../publish/enter-add-on-properties.md#subscription-period): elija el período de facturación recurrente de su suscripción. No se puede cambiar el período de suscripción después de publicar el complemento.

        Cada complemento de suscripción admite un único período de suscripción y un período de prueba. Debe crear un complemento de suscripción diferente para cada tipo de suscripción que quiera ofrecer en la aplicación. Por ejemplo, si desea ofrecer una suscripción mensual sin evaluación, una suscripción mensual con una versión de prueba de un mes, una suscripción anual sin evaluación y una suscripción anual con una versión de prueba de un mes, deberá crear cuatro complementos de suscripción.

    * [Período de prueba](../publish/enter-add-on-properties.md#free-trial): considere la posibilidad de elegir un período de prueba de 1 semana o un mes para su suscripción a fin de permitir que los usuarios prueben el contenido de la suscripción antes de comprarlo. No se puede cambiar o quitar el período de prueba después de publicar el complemento de suscripción.

        Para adquirir una evaluación gratuita de su suscripción, el usuario debe comprar su suscripción a través del proceso de compra estándar de la aplicación, incluida una forma de pago válida. No se les cobra ningún dinero durante el período de prueba. Al final del período de prueba, la suscripción se convierte automáticamente en la suscripción completa y se cobrará el instrumento de pago del usuario por el primer período de la suscripción de pago. Si el usuario decide cancelar su suscripción durante el período de prueba, la suscripción permanece activa hasta el final del período de prueba. Algunos períodos de prueba no están disponibles para todos los períodos de suscripción.

        > [!NOTE]
        > Cada cliente puede adquirir una evaluación gratuita de un complemento de suscripción solo una vez. Una vez que un cliente adquiere una evaluación gratuita de una suscripción, el almacén impide que el mismo cliente adquiera la misma suscripción de evaluación gratuita de nuevo.

    * [Visibilidad](../publish/set-add-on-pricing-and-availability.md#visibility): Si va a crear un complemento de prueba que solo usará para probar la experiencia de compra desde la aplicación para su suscripción, se recomienda que seleccione una de las opciones de **almacenamiento ocultas** . De lo contrario, puede seleccionar la mejor opción de visibilidad para su escenario.

    * [Precios](../publish/set-add-on-pricing-and-availability.md?#pricing): elija el precio de la suscripción en esta sección. No se puede aumentar el precio de la suscripción después de publicar el complemento. Sin embargo, puede reducir el precio más adelante.
        > [!IMPORTANT]
        > De forma predeterminada, cuando se crea un complemento, el precio se establece inicialmente en **gratis**. Dado que no puede aumentar el precio de un complemento de suscripción después de completar el envío del complemento, asegúrese de elegir el precio de su suscripción aquí.

2. En la aplicación, use las API del espacio de nombres [**Windows. Services. Store**](/uwp/api/windows.services.store) para determinar si el usuario actual ya ha adquirido el complemento de suscripción y, a continuación, ofrecer la venta al usuario como una compra desde la aplicación. Vea los [ejemplos de código](#code-examples) de este artículo para obtener más detalles.

3. Pruebe la implementación de compras desde la aplicación de su suscripción en la aplicación. Deberá descargar la aplicación una vez desde la tienda a su dispositivo de desarrollo para usar su licencia para las pruebas. Para obtener más información, consulte nuestra [Guía de pruebas](in-app-purchases-and-trials.md#testing) para compras desde la aplicación.  

4. Cree y publique un envío de aplicación que incluya el paquete de la aplicación actualizado, incluido el código probado. Para obtener más información, consulte [envíos de aplicaciones](../publish/app-submissions.md).

<span id="code-examples"/>

## <a name="code-examples"></a>Ejemplos de código

En los ejemplos de código de esta sección se muestra cómo usar las API en el espacio de nombres [**Windows. Services. Store**](/uwp/api/windows.services.store) para obtener información sobre los complementos de suscripción para la aplicación actual y solicitar la compra de un complemento de suscripción en nombre del usuario actual.

Estos ejemplos cumplen los siguientes requisitos:
* Un proyecto de Visual Studio para una aplicación Plataforma universal de Windows (UWP) destinada a **Windows 10 aniversario Edition (10,0; Compilación 14393)** o una versión posterior.
* Ha [creado un envío de aplicación](../publish/app-submissions.md) en el centro de Partners y esta aplicación se publica en la tienda. Opcionalmente, puede configurar la aplicación para que no se pueda detectar en el almacén mientras la prueba. Para obtener más información, consulta la [guía para prueba](in-app-purchases-and-trials.md#testing).
* Ha [creado un complemento de suscripción para la aplicación en el](../publish/add-on-submissions.md) centro de Partners.

En el código de estos ejemplos se presupone:
* El archivo de código tiene instrucciones **using** para los espacios de nombres **Windows. Services. Store** y **System. Threading. Tasks** .
* La aplicación es una de usuario único que se ejecuta solamente en el contexto del usuario que la inició. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si tiene una aplicación de escritorio que usa el [puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop), es posible que deba agregar código adicional que no se muestra en estos ejemplos para configurar el objeto [**StoreContext**](/uwp/api/Windows.Services.Store.StoreContext) . Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

### <a name="purchase-a-subscription-add-on"></a>Comprar un complemento de suscripción

En este ejemplo se muestra cómo solicitar la compra de un complemento de suscripción conocido para su aplicación en nombre del cliente actual. En este ejemplo también se muestra cómo controlar el caso en el que la suscripción tiene un período de prueba.

1. En primer lugar, el código determina si el cliente ya tiene una licencia activa para la suscripción. Si el cliente ya tiene una licencia activa, el código debe desbloquear las características de suscripción según sea necesario (porque es propiedad de la aplicación, esto se identifica con un Comentario en el ejemplo).
2. Después, el código obtiene el objeto [**StoreProduct**](/uwp/api/windows.services.store.storeproduct) que representa la suscripción que desea comprar en nombre del cliente. En el código se supone que ya conoce el identificador de la [tienda](in-app-purchases-and-trials.md#store-ids) del complemento de suscripción que desea comprar y que ha asignado este valor a la variable *subscriptionStoreId* .
3. A continuación, el código determina si una evaluación está disponible para la suscripción. Opcionalmente, la aplicación puede usar esta información para mostrar detalles sobre la versión de prueba o la suscripción completa al cliente.
4. Por último, el código llama al método [**RequestPurchaseAsync**](/uwp/api/windows.services.store.storeproduct.RequestPurchaseAsync) para solicitar la compra de la suscripción. Si hay disponible una versión de evaluación para la suscripción, la evaluación se ofrecerá al cliente para su compra. De lo contrario, la suscripción completa se ofrecerá para su compra.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/PurchaseSubscriptionAddOnTrialPage.xaml.cs" id="PurchaseTrialSubscription":::

### <a name="get-info-about-subscription-add-ons-for-the-current-app"></a>Obtener información sobre los complementos de suscripción para la aplicación actual

En este ejemplo de código se muestra cómo obtener información de todos los complementos de suscripción que están disponibles en la aplicación. Para obtener esta información, use primero el método [**GetAssociatedStoreProductsAsync**](/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsAsync) para obtener la colección de objetos [**StoreProduct**](/uwp/api/Windows.Services.Store.StoreProduct) que representan cada uno de los complementos disponibles para la aplicación. A continuación, obtenga el [**StoreSku**](/uwp/api/windows.services.store.storesku) para cada producto y use las propiedades [**IsSubscription**](/uwp/api/windows.services.store.storesku.IsSubscription) y [**SubscriptionInfo**](/uwp/api/windows.services.store.storesku.SubscriptionInfo) para tener acceso a la información de la suscripción.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/GetSubscriptionAddOnsPage.xaml.cs" id="GetSubscriptions":::

<span id="manage-subscriptions" />

## <a name="manage-subscriptions-from-your-services"></a>Administrar suscripciones de los servicios

Una vez que la aplicación actualizada se encuentra en la tienda y los clientes pueden comprar el complemento de suscripción, es posible que tenga escenarios en los que necesite administrar la suscripción de un cliente. Proporcionamos las API de REST a las que puede llamar desde sus servicios para realizar las siguientes tareas de administración de suscripciones:

* [Obtener las suscripciones que un usuario tiene derecho a usar](get-subscriptions-for-a-user.md). Si su suscripción forma parte de un servicio multiplataforma, puede llamar a esta API para determinar si el usuario especificado tiene derecho a su suscripción y el estado de su suscripción en el contexto de la aplicación para UWP. Después, puede usar esta información para actualizar el estado de la suscripción en otras plataformas que admita su servicio.

* [Cambiar el estado de facturación de una suscripción para un usuario determinado](change-the-billing-state-of-a-subscription-for-a-user.md). Use esta API para cancelar, extender o deshabilitar la renovación automática de una suscripción.

## <a name="cancellations"></a>Cancelaciones

Los clientes pueden usar la [https://account.microsoft.com/services](https://account.microsoft.com/services) Página para su cuenta de Microsoft para ver todas las suscripciones que han adquirido, cancelar una suscripción y cambiar la forma de pago asociada a su suscripción. Cuando un cliente cancela una suscripción a través de esta página, sigue teniendo acceso a la suscripción durante el período de facturación actual. No reciben un reembolso para ninguna parte del período de facturación actual. Al final del período de facturación actual, su suscripción se desactiva.

También puede cancelar una suscripción en nombre de un usuario mediante nuestra API de REST para [cambiar el estado de facturación de una suscripción para un usuario determinado](change-the-billing-state-of-a-subscription-for-a-user.md).

## <a name="subscription-renewals-and-grace-periods"></a>Períodos de gracia y renovaciones de suscripción

En algún momento durante cada período de facturación, se intentará cargar la tarjeta de crédito del cliente durante el siguiente período de facturación. Si se produce un error en la carga, la suscripción del cliente entra en el estado de *estiba* . Esto significa que su suscripción todavía está activa durante el resto del período de facturación actual, pero se intentará cargar periódicamente su tarjeta de crédito para renovar la suscripción automáticamente. Este estado puede durar hasta dos semanas hasta el final del período de facturación actual y la fecha de renovación para el siguiente período de facturación.

No ofrecemos períodos de gracia para la facturación de suscripciones. Si no se puede cargar correctamente la tarjeta de crédito del cliente al final del período de facturación actual, se cancelará la suscripción y el cliente dejará de tener acceso a la suscripción después del período de facturación actual.

## <a name="unsupported-scenarios"></a>Escenarios no admitidos

Los siguientes escenarios no se admiten actualmente para los complementos de suscripción.

* En este momento no se admite la venta de suscripciones a clientes directamente a través de la tienda. Las suscripciones solo están disponibles para compras desde la aplicación de productos digitales.
* Los clientes no pueden cambiar los períodos de suscripción mediante la [https://account.microsoft.com/services](https://account.microsoft.com/services) Página de su cuenta de Microsoft. Para cambiar a otro período de suscripción, los clientes deben cancelar su suscripción actual y, después, comprar una suscripción con un período de suscripción diferente de la aplicación.
* La conmutación de niveles no se admite actualmente para los complementos de suscripción (por ejemplo, el cambio de un cliente de una suscripción básica a una suscripción Premium con más características).
* Los [códigos promocionales](../publish/generate-promotional-codes.md) y de [ventas](../publish/put-apps-and-add-ons-on-sale.md) no se admiten actualmente para los complementos de suscripción.
* Renovación de las suscripciones existentes después de establecer la visibilidad del complemento de suscripción para **detener la adquisición**. Para más información [, consulte establecimiento de precios y disponibilidad de complementos](../publish/set-add-on-pricing-and-availability.md) .

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
