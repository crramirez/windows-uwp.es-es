---
author: mcleanbyron
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: Si tu aplicación ofrece un catálogo de productos de gran tamaño en la aplicación, también puedes seguir el proceso descrito en este tema para ayudar a administrar el catálogo.
title: Administrar un catálogo extenso de productos desde la aplicación
---

# Administrar un catálogo extenso de productos desde la aplicación


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Si tu aplicación ofrece un catálogo de productos de gran tamaño en la aplicación, también puedes seguir el proceso descrito en este tema para ayudar a administrar el catálogo. Podrás crear infinidad de entradas de productos para determinadas franjas de precios, cada una de ellas capaz de representar cientos de productos en un catálogo.

Para hacer posible esta funcionalidad, usa la sobrecarga del método [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382), que permite comprar una oferta definida por la aplicación asociada con un producto desde la aplicación incluido en la Tienda. Además de especificar una asociación entre oferta y producto durante la llamada, tu aplicación también deberá pasar un objeto [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384) que contenga los detalles de la oferta del catálogo de gran volumen. Si estos detalles no se suministran, se usarán los detalles que figuran sobre el producto.

La Tienda usará únicamente el *offerId* de la solicitud de compra en el [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392) resultante. Este proceso no modifica directamente la información suministrada inicialmente al elaborar la [lista de productos desde la aplicación de la Tienda](https://msdn.microsoft.com/library/windows/apps/mt148551).

**Nota**  A partir de Windows 10, la Tienda no tiene ningún límite en el número de listas de productos por cuenta de desarrollador. En versiones anteriores, la Tienda tiene un límite de 200 listas de productos por cuenta de desarrollador y el proceso descrito en este tema puede usarse para evitar esa limitación.

## Requisitos previos

-   En este tema se aborda el soporte técnico de la Tienda para la representación de varias ofertas desde la aplicación usando un solo producto desde la aplicación incluido en la Tienda. Si no estás familiarizado con las compras desde la aplicación, consulta [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md) para ver información sobre la licencia y cómo incluir de la forma adecuada tu compra desde la aplicación en la Tienda.
-   Cuando codificas y pruebas nuevas ofertas desde la aplicación por primera vez, debes usar el objeto [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) en lugar del objeto [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765). De esta manera, puedes comprobar la lógica de la licencia con llamadas simuladas al servidor de licencias, en lugar de llamar al servidor activo. Para ello, debes personalizar el archivo llamado "WindowsStoreProxy.xml" in %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. El simulador de Microsoft Visual Studio crea este archivo la primera vez que ejecutas la aplicación, aunque también puedes cargar un archivo personalizado en tiempo de ejecución. Para obtener más información, consulta **CurrentAppSimulator**.
-   Este tema también hace referencia a los ejemplos de código que se proporcionan en la [muestra de la Tienda](http://go.microsoft.com/fwlink/p/?LinkID=627610). Esta muestra es ideal para conseguir experiencia práctica con las diferentes opciones de monetización que se proporcionan para las aplicaciones para la Plataforma universal de Windows (UWP).

## Realizar la solicitud de compra del producto desde la aplicación

La solicitud de compra de un producto concreto dentro de un catálogo voluminoso se controla en gran medida como cualquier otra solicitud de compra dentro de la aplicación. Cuando tu aplicación llama a la nueva sobrecarga del método [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382), proporciona tanto un *OfferId* como un objeto [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263390) rellenado con el nombre del producto desde la aplicación.

```CSharp
string offerId = "1234";
string displayPropertiesName = "MusicOffer1";
var displayProperties = new ProductPurchaseDisplayProperties(displayPropertiesName);

try
{
    PurchaseResults purchaseResults = await CurrentAppSimulator.RequestProductPurchaseAsync("product1", offerId, displayProperties);
    switch (purchaseResults.Status)
    {
        case ProductPurchaseStatus.Succeeded:
            // Grant the user their purchase here, and then pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotFulfilled:
            // First check for unfulfilled purchases and grant any unfulfilled purchases from an earlier transaction.
            // Once products are fulfilled pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotPurchased:
            // Notify user that the purchase was not completed due to cancellation or error.
            break;
    }
}
catch (Exception)
{
    //Notify the user of the purchase error.
}
```

## Notificar la cumplimentación de la oferta desde la aplicación

Tu aplicación deberá notificar a Windows en tiempo de ejecución de la cumplimentación del producto en cuanto la oferta se haya suministrado localmente. En un escenario con un catálogo voluminoso, si tu aplicación no notifica la cumplimentación de la oferta, el usuario no podrá comprar ninguna oferta desde la aplicación a través de la misma lista de productos de la Tienda.

Tal y como se mencionó antes, la Tienda usa la información de las ofertas suministradas solamente para rellenar [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392), y no crea una asociación permanente entre una oferta del catálogo extenso y la lista de productos de la Tienda. En consecuencia, es necesario realizar un seguimiento de la autorización del usuario para productos del catálogo y ofrecer al usuario contexto específico de producto (como el nombre del artículo que se va a comprar o detalles al respecto) fuera de la operación [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382).

El siguiente código refleja la llamada de cumplimentación y un patrón de mensaje de interfaz de usuario en el que se inserta la información de la oferta en cuestión. Cuando no existe información específica del producto, el ejemplo usa información del producto [**ListingInformation**](https://msdn.microsoft.com/library/windows/apps/br225163).

```CSharp
string offerId = "1234";
product1ListingName = product1.Name;
string displayPropertiesName = "MusicOffer1";

if (String.IsNullOrEmpty(displayPropertiesName))
{
    displayPropertiesName = product1ListingName;
}
var offerIdMsg = " with offer id " + offerId;
if (String.IsNullOrEmpty(offerId))
{
    offerIdMsg = " with no offer id";
}

FulfillmentResult result = await CurrentAppSimulator.ReportConsumableFulfillmentAsync(productId, transactionId);
switch (result)
{
    case FulfillmentResult.Succeeded:
        Log("You bought and fulfilled " + displayPropertiesName + offerIdMsg, NotifyType.StatusMessage);
        break;
    case FulfillmentResult.NothingToFulfill:
        Log("There is no purchased product 1 to fulfill.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchasePending:
        Log("You bought product 1. The purchase is pending so we cannot fulfill the product.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchaseReverted:
        Log("You bought product 1. But your purchase has been reverted.", NotifyType.StatusMessage);
        // Since the user' s purchase was revoked, they got their money back.
        // You may want to revoke the user' s access to the consumable content that was granted.
        break;
    case FulfillmentResult.ServerError:
        Log("You bought product 1. There was an error when fulfilling.", NotifyType.StatusMessage);
        break;
}
```

## Temas relacionados

* [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md)
* [Habilitar compras de productos consumibles desde la aplicación](enable-consumable-in-app-product-purchases.md)
* [Muestra de la Tienda (muestra pruebas y compras desde la aplicación)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382)
* [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384)


<!--HONumber=May16_HO2-->


