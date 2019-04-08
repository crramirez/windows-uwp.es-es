---
Description: La oferta de productos consumibles desde la aplicación &\#8212; los elementos que se pueden adquirir, usar y se volvió a adquirir &\#8212; a través de la Store plataforma de comercio para proporcionar la experimentan de sus clientes con una compra, que es sólido y fidedigno.
title: Habilitar compras de productos consumibles desde la aplicación
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: uwp, UWP, consumable, consumibles, add-ons, complementos, in-app purchases, compras desde la aplicación, IAPs, IAP, Windows.ApplicationModel.Store, Windows.ApplicationModel.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5588558eff3e9c9b2954f0726995765a2862c43b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655650"
---
# <a name="enable-consumable-in-app-product-purchases"></a>Habilitar compras de productos consumibles desde la aplicación

Ofrece productos consumibles desde la aplicación (artículos que se pueden comprar, usar y volver a comprar), a través de la plataforma de comercio de la Tienda para proporcionar a tus clientes una experiencia de compra sólida y de confianza. Esto es especialmente útil para cosas como monedas de juego (oro, monedas, etc.) que se pueden comprar y usar para comprar bonificaciones concretas.

> [!IMPORTANT]
> En este artículo se muestra cómo usar los miembros del espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) para habilitar las compras de productos consumibles desde la aplicación. Este espacio de nombres ya no se actualiza con las nuevas características por lo que te recomendamos que uses el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) en su lugar. El **Windows.Services.Store** espacio de nombres es compatible con los tipos de complementos más recientes, como Store administrado puede usar complementos y las suscripciones y está diseñado para ser compatible con tipos futuros de productos y características admitidas por asociado El centro y el Store. El espacio de nombres **Windows.Services.Store** se introdujo en Windows 10, versión 1607 y solo se puede usar en proyectos destinados a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio. Para obtener más información sobre cómo habilitar las compras de productos consumibles desde la aplicación con el espacio de nombres **Windows.Services.Store**, consulta [este artículo](enable-consumable-add-on-purchases.md).

## <a name="prerequisites"></a>Requisitos previos

-   Este tema se centra en la compra y los informes de suministro de productos consumibles dentro de la aplicación. Si no estás familiarizado con los productos desde la aplicación, consulta [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md) para ver información sobre la licencia y cómo incluir de forma adecuada los productos desde la aplicación en la Tienda.
-   Cuando codificas y pruebas nuevos productos desde la aplicación por primera vez, debes usar el objeto [CurrentAppSimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) en lugar del objeto [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp). De esta manera, puedes comprobar la lógica de la licencia con llamadas simuladas al servidor de licencias, en lugar de llamar al servidor activo. Para ello, debe personalizar el archivo denominado WindowsStoreProxy.xml en % userprofile %\\AppData\\local\\paquetes\\&lt;nombredelpaquete&gt;\\LocalState\\ Microsoft\\Windows Store\\ApiData. El simulador de Microsoft Visual Studio crea este archivo la primera vez que se ejecuta la aplicación, aunque también puedes cargar un archivo personalizado en el tiempo de ejecución. Para obtener más información, consulta [Uso del archivo WindowsStoreProxy.xml con CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Este tema también hace referencia a los ejemplos de código que se proporcionan en la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Esta muestra es ideal para conseguir experiencia práctica con las diferentes opciones de monetización que se proporcionan para las aplicaciones para la Plataforma universal de Windows (UWP).

## <a name="step-1-making-the-purchase-request"></a>Paso 1: Que realiza la solicitud de compra

La solicitud de compra inicial se realiza con [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), como cualquier otra compra que se lleva a cabo a través de la Tienda. La diferencia de los productos consumibles desde la aplicación es que, tras completar la compra, el cliente no puede volver a comprar el mismo producto hasta que la aplicación haya notificado a la Tienda que la anterior compra de dicho producto se suministró correctamente. Es responsabilidad de tu aplicación suministrar los consumibles comprados y notificar a la Tienda del suministro.

En el siguiente ejemplo se muestra una solicitud de compra de un producto consumible desde la aplicación. Verás comentarios en el código que indican cuándo debería realizar la aplicación el suministro local del producto consumible desde la aplicación en dos casos distintos: cuando la solicitud se realiza correctamente y cuando la solicitud no se realiza correctamente debido a una compra no suministrada del mismo producto.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#MakePurchaseRequest)]

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>Paso 2: Seguimiento de entrega local de la consumibles

Cuando se concede al cliente acceso al producto consumible desde la aplicación, es importante realizar un seguimiento del producto suministrado (*productId*) y de la transacción asociada al mismo (*transactionId*).

> [!IMPORTANT]
> Tu aplicación es responsable de generar informes precisos de suministros para la Store. Este paso es esencial para mantener una experiencia de compra justa y de confianza para tus clientes.

En el siguiente ejemplo, se muestra el uso de las propiedades [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) desde la llamada a [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) en el paso anterior para identificar el producto comprado que se va a suministrar. Se usa una colección para almacenar la información de los productos en una ubicación a la que después se pueda hacer referencia para confirmar que el suministro local se ha realizado correctamente.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GrantFeatureLocally)]

En el siguiente ejemplo, se muestra cómo usar la matriz del ejemplo anterior para acceder a los pares id. del producto/ id. de transacción que se usan más tarde cuando se informa del suministro a la Tienda.

> [!IMPORTANT]
> Independientemente de la metodología que use tu aplicación para hacer el seguimiento y confirmar el suministro, debe demostrar la diligencia debida para garantizar que no se cobre a tus clientes por artículos que no hayan recibido.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#IsLocallyFulfilled)]

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>Paso 3: Informes de cumplimiento de producto para el Store

Después de completar el suministro local, la aplicación debe hacer una llamada a [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync) que incluya el elemento *productId* y la transacción en la que se incluya la compra del producto.

> [!IMPORTANT]
> De no enviar el informe del suministro de productos consumibles desde la aplicación a la Store, el usuario no podrá volver a comprar el producto hasta que se notifique el suministro de la compra anterior.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#ReportFulfillment)]

## <a name="step-4-identifying-unfulfilled-purchases"></a>Paso 4: Identificación de compras no satisfechas

La aplicación puede usar el método [GetUnfulfilledConsumablesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) para buscar en cualquier momento productos consumibles desde la aplicación que no se hayan suministrado. Se debe llamar a este método con regularidad para buscar consumibles sin suministrar que existan como consecuencia de eventos no anticipados de la aplicación, como una interrupción en la conectividad de red o el cierre de la aplicación.

En el siguiente ejemplo, se demuestra cómo se puede usar [GetUnfulfilledConsumablesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) para enumerar consumibles sin suministrar y cómo la aplicación puede iterar por esta lista para completar el suministro local.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GetUnfulfilledConsumables)]

## <a name="related-topics"></a>Temas relacionados

* [Habilitar las compras de productos desde la aplicación](enable-in-app-product-purchases.md)
* [Ejemplo de Store (muestra las pruebas y compras de la aplicación)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/br225197)
 

 
