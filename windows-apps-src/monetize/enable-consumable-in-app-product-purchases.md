---
author: mcleanbyron
Description: "Ofrece productos consumibles desde la aplicación (artículos que se pueden comprar, usar y volver a comprar) a través de la plataforma de comercio de la Tienda para proporcionar a tus clientes una experiencia de compra sólida y de confianza."
title: "Habilitar compras de productos consumibles desde la aplicación"
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: "muestra de código de oferta desde la aplicación"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 15092f726283f36c8dc5970157d3cd54dea9b837

---

# Habilitar compras de productos consumibles desde la aplicación




>**Nota**&nbsp;&nbsp;En este artículo se muestra cómo usar miembros del espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx). Si la aplicación está orientada a Windows 10, versión 1607, o posterior, te recomendamos que uses miembros del espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para administrar los complementos (también conocidos como productos dentro de la aplicación o IAP), en lugar del espacio de nombres **Windows.ApplicationModel.Store**. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md).

Ofrece productos consumibles desde la aplicación (artículos que se pueden comprar, usar y volver a comprar) a través de la plataforma de comercio de la Tienda para proporcionar a tus clientes una experiencia de compra sólida y de confianza. Esto es especialmente útil para cosas como monedas de juego (oro, monedas, etc.) que se puedan comprar y se usen para comprar bonificaciones concretas.

## Requisitos previos

-   Este tema se centra en la compra y los informes de suministro de productos consumibles dentro de la aplicación. Si no estás familiarizado con los productos desde la aplicación, consulta [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md) para ver información sobre la licencia y cómo incluir de forma adecuada los productos desde la aplicación en la Tienda.
-   Cuando codificas y pruebas nuevos productos desde la aplicación por primera vez, debes usar el objeto [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) en lugar del objeto [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765). De esta manera, puedes comprobar la lógica de la licencia con llamadas simuladas al servidor de licencias, en lugar de llamar al servidor activo. Para ello, debes personalizar el archivo llamado "WindowsStoreProxy.xml" in %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. El simulador de Microsoft Visual Studio crea este archivo la primera vez que ejecutas la aplicación, aunque también puedes cargar un archivo personalizado en tiempo de ejecución. Para obtener más información, consulta **CurrentAppSimulator**.
-   Este tema también hace referencia a los ejemplos de código que se proporcionan en la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Esta muestra es ideal para conseguir experiencia práctica con las diferentes opciones de monetización que se proporcionan para las aplicaciones para la Plataforma universal de Windows (UWP).

## Paso 1: Realizar la solicitud de compra

La solicitud de compra inicial se realiza con [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263381), como cualquier otra compra que se lleva a cabo a través de la Tienda. La diferencia de los productos consumibles desde la aplicación es que, tras completar la compra, el cliente no puede volver a comprar el mismo producto hasta que la aplicación haya notificado a la Tienda que la anterior compra de dicho producto se suministró correctamente. Es responsabilidad de tu aplicación suministrar los consumibles comprados y notificar a la Tienda del suministro.

En el siguiente ejemplo se muestra una solicitud de compra de un producto consumible desde la aplicación. Verás comentarios en el código que indican cuándo debería realizar la aplicación el suministro local del producto consumible desde la aplicación en dos casos distintos: cuando la solicitud se realiza correctamente y cuando la solicitud no se realiza correctamente debido a una compra no suministrada del mismo producto.

```CSharp
PurchaseResults purchaseResults = await CurrentAppSimulator.RequestProductPurchaseAsync("product1");
switch (purchaseResults.Status)
{
    case ProductPurchaseStatus.Succeeded:
        product1TempTransactionId = purchaseResults.TransactionId;

        // Grant the user their purchase here, and then pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
        // To indicate local fulfillment to the Windows Store.
        break;

    case ProductPurchaseStatus.NotFulfilled:
        product1TempTransactionId = purchaseResults.TransactionId;

        // First check for unfulfilled purchases and grant any unfulfilled purchases from an earlier transaction.
        // Once products are fulfilled pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
        // To indicate local fulfillment to the Windows Store.
        break;
}
```

## Paso 2: Seguimiento del suministro local del consumible

Cuando se le concede al cliente acceso al producto consumible desde la aplicación, es importante realizar un seguimiento del producto suministrado (*productId*) y de la transacción asociada al mismo (*transactionId*).

**Importante**  Tu aplicación es la responsable de generar informes precisos de suministros para la Tienda. Este paso es esencial para mantener una experiencia de compra justa y de confianza para tus clientes.

En el siguiente ejemplo, se demuestra el uso de las propiedades [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392) desde la llamada a [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263381) en el paso anterior, para identificar el producto comprado que se va a suministrar. Se usa una matriz para almacenar la información de los productos en una ubicación a la que después se pueda hacer referencia para confirmar que el suministro local se realizó correctamente.

```CSharp
private void GrantFeatureLocally(string productId, Guid transactionId)
{
    if (!grantedConsumableTransactionIds.ContainsKey(productId))
    {
        grantedConsumableTransactionIds.Add(productId, new List<Guid>());
    }
    grantedConsumableTransactionIds[productId].Add(transactionId);

    // Grant the user their content. You will likely increase some kind of gold/coins/some other asset count.
}
```

En el siguiente ejemplo, se muestra cómo usar la matriz del ejemplo anterior para acceder a los pares id. del producto/ id. de transacción que se usan más tarde cuando se informa del suministro a la Tienda.

**Importante**  Independientemente de la metodología que use la aplicación para hacer el seguimiento del suministro y confirmarlo, debe demostrar la diligencia debida para garantizar que no se cobre a los clientes artículos que no hayan recibido.

```CSharp
private Boolean IsLocallyFulfilled(string productId, Guid transactionId)
{
    return grantedConsumableTransactionIds.ContainsKey(productId) && grantedConsumableTransactionIds[productId].Contains(transactionId);
}
```

## Paso 3: Informar del suministro de productos a la Tienda

Después de completar el suministro local, la aplicación debe hacer una llamada a [**ReportConsumableFulfillmentAsync**](https://msdn.microsoft.com/library/windows/apps/dn263380) que incluya el elemento *productId* y la transacción en la que estaba incluida la compra del producto.

**Importante**  De no enviar los informes de productos consumibles desde la aplicación suministrados a la Tienda, el usuario no podrá volver a comprar el producto hasta que se notifique el suministro de la compra anterior.

```CSharp
FulfillmentResult result = await CurrentAppSimulator.ReportConsumableFulfillmentAsync("product2", product2TempTransactionId);
```

## Paso 4: Identificación de compras sin suministrar

La aplicación puede usar el método [**GetUnfulfilledConsumablesAsync**](https://msdn.microsoft.com/library/windows/apps/dn263379) para buscar en cualquier momento productos consumibles desde la aplicación que no se hayan completado. Se debe llamar a este método con regularidad para buscar consumibles sin suministrar que existan como consecuencia de eventos no anticipados de la aplicación, como una interrupción en la conectividad de red o el cierre de la aplicación.

En el siguiente ejemplo, se demuestra cómo se puede usar [**GetUnfulfilledConsumablesAsync**](https://msdn.microsoft.com/library/windows/apps/dn263379) para enumerar consumibles sin suministrar, y cómo la aplicación puede iterar por esta lista para completar el suministro local.

```CSharp
private async void GetUnfulfilledConsumables()
{
    products = await CurrentApp.GetUnfulfilledConsumablesAsync();

    foreach (UnfulfilledConsumable product in products)
    {
        logMessage += "\nProduct Id: " + product.ProductId + " Transaction Id: " + product.TransactionId;
        // This is where you would pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
    // To indicate local fulfillment to the Windows Store.
    }
}
```

## Temas relacionados

* [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md)
* [Muestra de la Tienda (muestra pruebas y compras desde la aplicación)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197)
 

 



<!--HONumber=Aug16_HO5-->


