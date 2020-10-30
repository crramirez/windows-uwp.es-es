---
description: Ofrezca productos en aplicación consumibles&\# 8212; elementos que se pueden adquirir, usar y volver a comprar&\# 8212; a través de la plataforma de comercio de la tienda para proporcionar a los clientes una experiencia de compra sólida y confiable.
title: Habilitar compras de productos consumibles desde la aplicación
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: UWP, consumible, complementos, compras desde la aplicación, IAPs, Windows. ApplicationModel. Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fdd09fdb0d80f7811014dc910772175e152505d0
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033548"
---
# <a name="enable-consumable-in-app-product-purchases"></a>Habilitar compras de productos consumibles desde la aplicación

Ofrece productos consumibles desde la aplicación (artículos que se pueden comprar, usar y volver a comprar), a través de la plataforma de comercio de la Tienda para proporcionar a tus clientes una experiencia de compra sólida y de confianza. Esto es especialmente útil para cosas como monedas de juego (oro, monedas, etc.) que se pueden comprar y usar para comprar bonificaciones concretas.

> [!IMPORTANT]
> En este artículo se muestra cómo usar los miembros del espacio de nombres [Windows. ApplicationModel. Store](/uwp/api/windows.applicationmodel.store) para habilitar las compras de productos consumibles en la aplicación. Este espacio de nombres ya no se actualiza con nuevas características y se recomienda usar el espacio de nombres [Windows. Services. Store](/uwp/api/windows.services.store) en su lugar. El espacio de nombres **Windows. Services. Store** es compatible con los tipos de complementos más recientes, como las suscripciones y los complementos consumibles administrados por el almacén, y está diseñado para ser compatible con los tipos de productos y características futuros admitidos por el centro de Partners y el almacén. El espacio de nombres **Windows. Services. Store** se presentó en la versión 1607 de Windows 10 y solo se puede usar en proyectos que tengan como destino la **edición de aniversario de windows 10 (10,0; Compilación 14393)** o una versión posterior en Visual Studio. Para obtener más información sobre cómo habilitar las compras de productos consumibles en la aplicación mediante el espacio de nombres **Windows. Services. Store** , consulte [este artículo](enable-consumable-add-on-purchases.md).

## <a name="prerequisites"></a>Requisitos previos

-   Este tema se centra en la compra y los informes de suministro de productos consumibles dentro de la aplicación. Si no estás familiarizado con los productos desde la aplicación, consulta [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md) para ver información sobre la licencia y cómo incluir de forma adecuada los productos desde la aplicación en la Tienda.
-   Cuando codificas y pruebas nuevos productos desde la aplicación por primera vez, debes usar el objeto [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) en lugar del objeto [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp). De esta manera, puedes comprobar la lógica de la licencia con llamadas simuladas al servidor de licencias, en lugar de llamar al servidor activo. Para ello, debe personalizar el archivo denominado WindowsStoreProxy.xml en% userprofile% \\ AppData local Packages \\ \\ \\ &lt; name &gt; \\ LocalState \\ Microsoft \\ Windows Store \\ ApiData. El simulador de Microsoft Visual Studio crea este archivo la primera vez que se ejecuta la aplicación, aunque también puedes cargar un archivo personalizado en el tiempo de ejecución. Para obtener más información, consulta [Uso del archivo WindowsStoreProxy.xml con CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Este tema también hace referencia a los ejemplos de código que se proporcionan en la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Esta muestra es ideal para conseguir experiencia práctica con las diferentes opciones de monetización que se proporcionan para las aplicaciones para la Plataforma universal de Windows (UWP).

## <a name="step-1-making-the-purchase-request"></a>Paso 1: Realizar la solicitud de compra

La solicitud de compra inicial se realiza con [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), como cualquier otra compra que se lleva a cabo a través de la Tienda. La diferencia de los productos consumibles desde la aplicación es que, tras completar la compra, el cliente no puede volver a comprar el mismo producto hasta que la aplicación haya notificado a la Tienda que la anterior compra de dicho producto se suministró correctamente. Es responsabilidad de tu aplicación suministrar los consumibles comprados y notificar a la Tienda del suministro.

En el siguiente ejemplo se muestra una solicitud de compra de un producto consumible desde la aplicación. Verás comentarios en el código que indican cuándo debería realizar la aplicación el suministro local del producto consumible desde la aplicación en dos casos distintos: cuando la solicitud se realiza correctamente y cuando la solicitud no se realiza correctamente debido a una compra no suministrada del mismo producto.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="MakePurchaseRequest":::

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>Paso 2: Seguimiento del suministro local del consumible

Cuando se concede al cliente acceso al producto consumible desde la aplicación, es importante realizar un seguimiento del producto suministrado ( *productId* ) y de la transacción asociada al mismo ( *transactionId* ).

> [!IMPORTANT]
> La aplicación es responsable de notificar con exactitud el cumplimiento de la tienda. Este paso es esencial para mantener una experiencia de compra justa y de confianza para tus clientes.

En el siguiente ejemplo, se demuestra el uso de las propiedades [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults) desde la llamada a [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) en el paso anterior, para identificar el producto comprado que se va a suministrar. Se usa una colección para almacenar la información de los productos en una ubicación a la que después se pueda hacer referencia para confirmar que el suministro local se ha realizado correctamente.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="GrantFeatureLocally":::

En el siguiente ejemplo, se muestra cómo usar la matriz del ejemplo anterior para acceder a los pares id. del producto/ id. de transacción que se usan más tarde cuando se informa del suministro a la Tienda.

> [!IMPORTANT]
> Con independencia de la metodología que use la aplicación para realizar un seguimiento y confirmar su cumplimiento, la aplicación debe demostrar la diligencia debida para asegurarse de que no se cobra a los clientes por los elementos que no recibieron.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="IsLocallyFulfilled":::

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>Paso 3: Informar del suministro de productos a la Tienda

Después de completar el suministro local, la aplicación debe hacer una llamada a [ReportConsumableFulfillmentAsync](/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync) que incluya el elemento *productId* y la transacción en la que estaba incluida la compra del producto.

> [!IMPORTANT]
> Si se produce un error al notificar productos consumibles en la aplicación en la tienda, el usuario no podrá comprar el producto de nuevo hasta que se notifique la entrega de la compra anterior.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="ReportFulfillment":::

## <a name="step-4-identifying-unfulfilled-purchases"></a>Paso 4: Identificación de compras sin suministrar

La aplicación puede usar el método [GetUnfulfilledConsumablesAsync](/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) para buscar en cualquier momento productos consumibles desde la aplicación que no se hayan completado. Se debe llamar a este método con regularidad para buscar consumibles sin suministrar que existan como consecuencia de eventos no anticipados de la aplicación, como una interrupción en la conectividad de red o el cierre de la aplicación.

En el siguiente ejemplo, se demuestra cómo se puede usar [GetUnfulfilledConsumablesAsync](/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) para enumerar consumibles sin suministrar, y cómo la aplicación puede iterar por esta lista para completar el suministro local.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="GetUnfulfilledConsumables":::

## <a name="related-topics"></a>Temas relacionados

* [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md)
* [Muestra de la Tienda (muestra pruebas y compras desde la aplicación)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](/uwp/api/Windows.ApplicationModel.Store)
 

 
