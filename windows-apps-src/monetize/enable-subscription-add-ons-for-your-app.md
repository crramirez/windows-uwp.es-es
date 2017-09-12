---
author: mcleanbyron
description: "Aprende a usar el espacio de nombres Windows.Services.Store para implementar complementos de una suscripción."
title: "Habilitar complementos de una suscripción para tu aplicación"
keywords: "windows 10, uwp, suscripciones, complementos, add-ons, compras desde la aplicación, in-app purchases, IAP, Windows.Services.Store"
ms.author: mcleans
ms.date: 08/01/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 16e2e8d160ad2236220dc6f19f578bbaa82c9dc0
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2017
---
# <a name="enable-subscription-add-ons-for-your-app"></a>Habilitar complementos de una suscripción para tu aplicación

> [!IMPORTANT]
> Actualmente, la capacidad para crear complementos de una suscripción solo está disponible para un conjunto de cuentas de desarrollador que participan en un programa de usuarios pioneros. En el futuro pondremos los complementos de suscripción a disposición de todas las cuentas de desarrollador. Ahora estamos proporcionando esta documentación preliminar ahora para ofrecer a los desarrolladores una vista previa de esta característica.

Si tu aplicación para UWP se ha diseñado para Windows 10, versión 1607, o posterior, puedes ofrecer compras desde la aplicación de complementos de *suscripción* a los clientes. Puedes usar suscripciones para vender productos digitales en tu aplicación (como las características de la aplicación o el contenido digital) con períodos de facturación periódicos automatizados.

## <a name="feature-highlights"></a>Aspectos destacados de las características

Los complementos de una suscripción para aplicaciones para UWP admiten las siguientes características:

* Puedes elegir entre períodos de suscripción de 1mes, 3meses, 6meses, 1año o 2años. Algunas aplicaciones pueden usar también un período de suscripción de 6 horas solo con fines de prueba.
* Puedes agregar períodos de prueba gratuita de 1semana o 1mes a tu suscripción.
* Windows SDK [proporciona API](#code-examples) que puedes usar en tu aplicación para obtener información acerca de los complementos de suscripción disponibles para la aplicación y permitir la compra del complemento de una suscripción. También proporcionamos las API de REST a las que puedes llamar desde tus servicios para [administrar suscripciones para un usuario](#manage-subscriptions).
* Puedes ver informes analíticos que proporcionan el número de adquisiciones de suscripción, suscriptores activos y suscripciones canceladas en un período de tiempo determinado.
* Los clientes pueden administrar su suscripción en la página [http://account.microsoft.com/services](http://account.microsoft.com/services) de su cuenta de Microsoft. Los clientes pueden usar esta página para ver todas las suscripciones que han adquirido, cancelar una suscripción y cambiar la forma de pago asociada a su suscripción.

> [!NOTE]
> Para habilitar la compra de los complementos de una suscripción en tu aplicación, la aplicación debe destinarse a Windows 10, versión 1607 o una versión posterior, y debe usar las API en el espacio de nombres **Windows.Services.Store** para implementar la experiencia de compra desde la aplicación en el espacio de nombres **Windows.ApplicationModel.Store**. Para obtener más información sobre las diferencias entre estos espacios de nombres, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md).

## <a name="steps-to-enable-a-subscription-add-on-for-your-app"></a>Pasos para habilitar el complemento de una suscripción para tu aplicación

Para habilitar la compra de complementos de una suscripción en la aplicación, sigue estos pasos.

1. [Crear un envío de complemento](../publish/add-on-submissions.md) para tu suscripción en el panel de información del Centro de desarrollo y publicar el envío. Al seguir el proceso de envío del complemento, presta mucha atención a las siguientes propiedades:

  * [Tipo de producto](../publish/set-your-add-on-product-id.md#product-type): Asegúrate de seleccionar **Suscripción**.

  * [Período de suscripción](../publish/enter-add-on-properties.md#subscription-period): Elige el período de facturación periódico para tu suscripción. No puedes cambiar el período de suscripción después de publicar el complemento.

    Cada complemento de suscripción admite un período de suscripción único y período de prueba. Debes crear un complemento de suscripción diferente para cada tipo de suscripción que quieres ofrecer en tu aplicación. Por ejemplo, si quisieras ofrecer una suscripción mensual sin ninguna versión de prueba, una suscripción mensual con una versión de prueba de un mes, una suscripción anual sin ninguna versión de prueba y una suscripción anual con una versión de prueba de un mes, necesitará crear cuatro complementos de suscripción.
        > [!NOTE]
        > If you are in the process of implementing the in-app purchase experience for your subscription, we recommend that you create a test add-on with the **For testing only – 6 hours** subscription period to help you test the experience. You can choose this test period only if you select one of the **Hidden in the Store** [visibility options](../publish/set-add-on-pricing-and-availability.md#visibility) for your test add-on.

  * [Período de prueba](../publish/enter-add-on-properties.md#free-trial): Piense en la posibilidad de elegir un período de prueba de 1 semana o 1mes para tu suscripción para que los usuarios puedan probar el contenido de la suscripción antes de comprarla. No puedes cambiar ni quitar el período de prueba después de publicar el complemento de la suscripción.

    Para obtener una evaluación gratuita de tu suscripción, un usuario debe adquirir tu suscripción a través del proceso de compra desde la aplicación estándar, como una forma de pago válida. No se le cobra dinero durante el período de prueba. Al final del período de prueba, la suscripción se convierte automáticamente en la suscripción completa y se te cobrará el instrumento de pago del usuario para el primer período de la suscripción de pago. Si el usuario elige cancelar su suscripción durante el período de prueba, la suscripción permanece activa hasta el final del período de prueba.
        > [!NOTE]
        > Some trial durations are not available for all subscription periods.

  * [Visibilidad](../publish/set-add-on-pricing-and-availability.md#visibility): Si estás creando un complemento de prueba que solo usarás para probar la experiencia de compra desde la aplicación para la suscripción, te recomendamos que selecciones una de las opciones **Oculto en la Tienda**. De lo contrario, puedes seleccionar la mejor opción de visibilidad para tu escenario.

  * [Precios](../publish/set-add-on-pricing-and-availability.md?#pricing): Elige el precio de tu suscripción en esta sección. No puedes subir el precio de la suscripción después de publicar el complemento (sin embargo, puedes reducir el precio más adelante).
      > [!IMPORTANT]
      > De manera predeterminada, al crear cualquier complemento, el precio se establece inicialmente en **Gratis**. Dado que no puedes subir el precio del complemento de una suscripción después de completar el envío del complemento, asegúrate de elegir el precio de tu suscripción aquí.

2. En tu aplicación, usa las API en el espacio de nombres [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) para confirmar si el usuario actual ya ha adquirido el complemento de la suscripción y luego ofrécelo para su venta al usuario como una compra desde la aplicación. Consulta los [ejemplos de código](#code-examples) en este artículo para obtener más detalles.

3. Prueba la implementación de la compra desde la aplicación de la suscripción en la aplicación. Deberás descargar la aplicación una vez de la Tienda en el dispositivo de desarrollo para usar su licencia para las pruebas. Para obtener más información, consulta nuestra [guía para prueba](in-app-purchases-and-trials.md#testing) para las compras desde la aplicación.  
    > [!NOTE]
    > Si la aplicación tiene acceso al período de suscripción **Solo para pruebas: 6 horas**, te recomendamos que crees un complemento de prueba con este período para ayudarte a probar la experiencia. Puedes elegir este período de prueba solo si seleccionas una de las [opciones de visibilidad](../publish/set-add-on-pricing-and-availability.md#visibility) **Oculto en la Tienda** para el complemento de prueba.

4. Crea y publica un envío de aplicación que incluya el paquete de la aplicación actualizada, incluido el código probado. Para más información, consulta [Envíos de aplicaciones](../publish/app-submissions.md).

<span id="code-examples"/>
## <a name="code-examples"></a>Ejemplos de código

Los ejemplos de código de esta sección muestran cómo usar las API en el espacio de nombres [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) para obtener información acerca de los complementos de una suscripción para la aplicación actual y solicitar la compra del complemento de una suscripción en nombre del usuario actual.

Estos ejemplos cumplen los siguientes requisitos:
* Un proyecto de Visual Studio de una aplicación para la Plataforma universal de Windows (UWP) destinado a Windows 10, versión 1607 o posterior.
* Has [creado un envío de aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) en el panel del Centro de desarrollo de Windows, y esta aplicación está publicada y disponible en la Tienda. De manera opcional, puedes configurar la aplicación para que no se pueda descubrir en la Tienda mientras la pruebas. Para obtener más información, consulta la [guía para prueba](in-app-purchases-and-trials.md#testing).
* Has [creado un complemento de suscripción para la aplicación](../publish/add-on-submissions.md) en el panel del Centro de desarrollo.

El código de estos ejemplos supone que:
* El código se ejecuta en el contexto de una [**Página**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contiene un elemento [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` y un elemento [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una aplicación de usuario único que se ejecuta solamente en el contexto del usuario que inició la aplicación. Para más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si tienes una aplicación de escritorio que usa el [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop), puede que tengas que agregar código adicional que no se muestra en estos ejemplos para configurar el objeto [**StoreContext**](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

### <a name="get-info-about-subscription-add-ons-for-the-current-app"></a>Obtener información sobre los complementos de una suscripción para la aplicación actual

Este ejemplo de código muestra cómo obtener información para los complementos de una suscripción que están disponibles en tu aplicación. Para obtener esta información, usa primero el método [**GetAssociatedStoreProductsAsync**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext#Windows_Services_Store_StoreContext_GetAssociatedStoreProductsAsync_) para obtener la colección de objetos [**StoreProduct**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) que representan cada uno de los complementos disponibles para la aplicación. A continuación, obtén el objeto [**StoreSku**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) para cada producto y usa las propiedades [**IsSubscription**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku#Windows_Services_Store_StoreSku_IsSubscription_) y [**SubscriptionInfo**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku#Windows_Services_Store_StoreSku_SubscriptionInfo_) para acceder a la información de la suscripción.

> [!div class="tabbedCodeSnippets"]
[!code-cs[Suscripciones](./code/InAppPurchasesAndLicenses_RS1/cs/GetSubscriptionAddOnsPage.xaml.cs#GetSubscriptions)]

### <a name="purchase-a-subscription-add-on"></a>Comprar el complemento de una suscripción

En este ejemplo se muestra cómo usar el método [**RequestPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_RequestPurchaseAsync_) de la clase [**StoreProduct**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) para comprar el complemento de una suscripción. En este ejemplo se da por supuesto que ya sabes el [Id. de la Tienda](in-app-purchases-and-trials.md#store-ids) del complemento de la suscripción que quieras adquirir.

> [!div class="tabbedCodeSnippets"]
[!code-cs[Suscripciones](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseSubscriptionAddOnPage.xaml.cs#PurchaseSubscription)]

<span id="manage-subscriptions" />
## <a name="manage-subscriptions-from-your-services"></a>Administrar suscripciones desde tus servicios

Una vez que la aplicación actualizada esté en la Tienda y los clientes puedan comprar el complemento de la suscripción, es posible que tengas escenarios en los que tengas que administrar la suscripción para un cliente. Proporcionamos API de REST a las puedes llamar desde tus servicios para realizar las siguientes tareas de administración de suscripciones:

* [Obtener las suscripciones que un usuario tiene derecho de usar](get-subscriptions-for-a-user.md). Si tu suscripción forma parte de un servicio multiplataforma, puede llamar a esta API para determinar si el usuario especificado tiene un derecho para tu suscripción y el estado de su suscripción en el contexto de tu aplicación para UWP. A continuación, puedes usar esta información para actualizar el estado de la suscripción en otras plataformas admitidas por tu servicio.

* [Cambiar el estado de facturación de la suscripción de un usuario determinado](change-the-billing-state-of-a-subscription-for-a-user.md). Usa esta API para cancelar, ampliar o deshabilitar la renovación automática de una suscripción.

<span id="cancellations" />
## <a name="cancellations"></a>Cancelaciones

Los clientes pueden usar la página [http://account.microsoft.com/services](http://account.microsoft.com/services) para su cuenta de Microsoft para ver todas las suscripciones que han adquirido, cancelar una suscripción y cambiar la forma de pago asociada a su suscripción. Cuando un cliente cancela una suscripción con esta página, continúa teniendo acceso a la suscripción durante la vigencia del ciclo de facturación actual. No obtienen reembolso de ninguna parte del ciclo de facturación actual. Al final del ciclo de facturación actual, su suscripción se desactiva.

También puedes cancelar una suscripción en nombre de un usuario con nuestra API de REST para [cambiar el estado de facturación de la suscripción de un usuario determinado](change-the-billing-state-of-a-subscription-for-a-user.md).

## <a name="unsupported-scenarios"></a>Escenarios no admitidos

Los siguientes escenarios no son compatibles actualmente con complementos de una suscripción.

* En este momento no se admite la venta de suscripciones a clientes directamente a través de la Tienda. Las suscripciones solo están disponibles para compras desde la aplicación de productos digitales.
* Los clientes no pueden cambiar de períodos de suscripción con la página [http://account.microsoft.com/services](http://account.microsoft.com/services) de su cuenta de Microsoft. Para cambiar a un período de suscripción diferente, los clientes deben cancelar su suscripción actual y comprar luego una suscripción con un período de suscripción diferente desde tu aplicación.
* El cambio de nivel no se admite actualmente para complementos de una suscripción (por ejemplo, el cambio de un cliente de una suscripción básica a una suscripción premium con más características).
* Las [Ventas](../publish/put-apps-and-add-ons-on-sale.md) y los [códigos promocionales](../publish/generate-promotional-codes.md) no se admiten actualmente para complementos de una suscripción.


## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
