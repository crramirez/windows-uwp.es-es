---
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: Si tu aplicación ofrece un catálogo de productos de gran tamaño en la aplicación, también puedes seguir el proceso descrito en este tema para ayudar a administrar dicho catálogo.
title: Administrar un catálogo extenso de productos desde la aplicación
ms.date: 08/25/2017
ms.topic: article
keywords: Windows 10, UWP, compras desde la aplicación, IAPs, complementos, catálogo, Windows. ApplicationModel. Store
ms.localizationpriority: medium
ms.openlocfilehash: a6bd4d95365e33ee30df87247b3aec72f70fc5b1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158429"
---
# <a name="manage-a-large-catalog-of-in-app-products"></a>Administrar un catálogo extenso de productos desde la aplicación

Si tu aplicación ofrece un catálogo de productos de gran tamaño en la aplicación, también puedes seguir el proceso descrito en este tema para ayudar a administrar dicho catálogo. En versiones anteriores a Windows 10, la Tienda tiene un límite de 200 descripciones de producto por cada cuenta de desarrollador, y el proceso descrito en este tema puede usarse para evitar esa limitación. A partir de Windows 10, la Tienda no tiene ningún límite para el número de descripciones de producto por cada cuenta de desarrollador, y el proceso descrito en este artículo ya no es necesario.

> [!IMPORTANT]
> En este artículo se muestra cómo usar los miembros del espacio de nombres [Windows. ApplicationModel. Store](/uwp/api/windows.applicationmodel.store) . Este espacio de nombres ya no se actualiza con nuevas características y se recomienda usar el espacio de nombres [Windows. Services. Store](/uwp/api/windows.services.store) en su lugar. El espacio de nombres **Windows. Services. Store** es compatible con los tipos de complementos más recientes, como las suscripciones y los complementos consumibles administrados por el almacén, y está diseñado para ser compatible con los tipos de productos y características futuros admitidos por el centro de Partners y el almacén. El espacio de nombres **Windows. Services. Store** se presentó en la versión 1607 de Windows 10 y solo se puede usar en proyectos que tengan como destino la **edición de aniversario de windows 10 (10,0; Compilación 14393)** o una versión posterior en Visual Studio. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md).

Para habilitar esta funcionalidad, crearás unas cuantas entradas de productos para franjas de precios específicas, cada una de ellas capaz de representar cientos de productos en un catálogo. Usa la sobrecarga del método [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), que especifica una oferta definida por la aplicación asociada con un producto incluido en la Tienda. Además de especificar una asociación entre oferta y producto durante la llamada, tu aplicación también deberá pasar un objeto [ProductPurchaseDisplayProperties](/uwp/api/Windows.ApplicationModel.Store.ProductPurchaseDisplayProperties) que contenga los detalles de la oferta del catálogo de gran volumen. Si estos detalles no se suministran, se usarán los detalles que figuran sobre el producto.

La Tienda usará únicamente el *offerId* de la solicitud de compra en el [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults) resultante. Este proceso no modifica directamente la información suministrada inicialmente al [incluir el producto desde la aplicación en la Tienda](../publish/add-on-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

-   En este tema se aborda el soporte técnico de la Tienda para la representación de varias ofertas desde la aplicación usando un solo producto desde la aplicación incluido en la Tienda. Si no estás familiarizado con las compras desde la aplicación, consulta [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md) para ver información sobre la licencia y cómo incluir de la forma adecuada tu compra desde la aplicación en la Tienda.
-   Cuando codificas y pruebas nuevas ofertas desde la aplicación por primera vez, debes usar el objeto [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) en lugar del objeto [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp). De esta manera, puedes comprobar la lógica de la licencia con llamadas simuladas al servidor de licencias, en lugar de llamar al servidor activo. Para ello, debe personalizar el archivo denominado WindowsStoreProxy.xml en% userprofile% \\ AppData local Packages \\ \\ \\ &lt; name &gt; \\ LocalState \\ Microsoft \\ Windows Store \\ ApiData. El simulador de Microsoft Visual Studio crea este archivo la primera vez que se ejecuta la aplicación, aunque también puedes cargar un archivo personalizado en el tiempo de ejecución. Para obtener más información, consulta [Uso del archivo WindowsStoreProxy.xml con CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Este tema también hace referencia a los ejemplos de código que se proporcionan en la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Esta muestra es ideal para conseguir experiencia práctica con las diferentes opciones de monetización que se proporcionan para las aplicaciones para la Plataforma universal de Windows (UWP).

## <a name="make-the-purchase-request-for-the-in-app-product"></a>Realizar la solicitud de compra del producto desde la aplicación

La solicitud de compra de un producto concreto dentro de un catálogo voluminoso se controla en gran medida como cualquier otra solicitud de compra dentro de la aplicación. Cuando tu aplicación llama a la nueva sobrecarga del método [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), proporciona tanto un *OfferId* como un objeto [ProductPurchaseDisplayProperties](/uwp/api/windows.applicationmodel.store.productpurchasedisplayproperties) rellenado con el nombre del producto desde la aplicación.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#MakePurchaseRequest)]

## <a name="report-fulfillment-of-the-in-app-offer"></a>Notificar la cumplimentación de la oferta desde la aplicación

Tu aplicación deberá notificar a la Tienda la cumplimentación del producto en cuanto la oferta se haya suministrado localmente. En un escenario con un catálogo voluminoso, si tu aplicación no notifica la cumplimentación de la oferta, el usuario no podrá comprar ninguna oferta desde la aplicación a través de la misma lista de productos de la Tienda.

Tal y como se mencionó antes, la Tienda usa la información de las ofertas suministradas solamente para rellenar [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults), y no crea una asociación permanente entre una oferta del catálogo extenso y la lista de productos de la Tienda. En consecuencia, es necesario realizar un seguimiento de la autorización del usuario para productos del catálogo y ofrecer al usuario contexto específico de producto (como el nombre del artículo que se va a comprar o detalles al respecto) fuera de la operación [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync).

El siguiente código refleja la llamada de cumplimentación y un patrón de mensaje de interfaz de usuario en el que se inserta la información de la oferta en cuestión. Cuando no existe información específica del producto, el ejemplo usa información del producto [ListingInformation](/uwp/api/Windows.ApplicationModel.Store.ListingInformation).

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#ReportFulfillment)]

## <a name="related-topics"></a>Temas relacionados

* [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md)
* [Habilitar compras de productos consumibles desde la aplicación](enable-consumable-in-app-product-purchases.md)
* [Muestra de la Tienda (muestra pruebas y compras desde la aplicación)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)
* [ProductPurchaseDisplayProperties](/uwp/api/Windows.ApplicationModel.Store.ProductPurchaseDisplayProperties)