---
author: Xansky
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: Si tu aplicación ofrece un catálogo de productos de gran tamaño en la aplicación, también puedes seguir el proceso descrito en este tema para ayudar a administrar el catálogo.
title: Administrar un catálogo extenso de productos desde la aplicación
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
keywords: windows 10, Windows 10, uwp, UWP, in-app purchases, compras desde la aplicación, IAPs, IAP, add-ons, complementos, catalog, catálogo, Windows.ApplicationModel.Store, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: f57adf62939c28794e3ecdf6e59f2c4763de9c21
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6980916"
---
# <a name="manage-a-large-catalog-of-in-app-products"></a>Administrar un catálogo extenso de productos desde la aplicación

Si tu aplicación ofrece un catálogo de productos de gran tamaño en la aplicación, también puedes seguir el proceso descrito en este tema para ayudar a administrar dicho catálogo. En versiones anteriores a Windows 10, la Store tiene un límite de 200 descripciones de producto por cada cuenta de desarrollador, y el proceso descrito en este tema puede usarse para evitar esa limitación. A partir de Windows 10, la tienda no tiene ningún límite en el número de descripciones de producto por cada cuenta de desarrollador y el proceso descrito en este artículo ya no es necesario.

> [!IMPORTANT]
> En este artículo se muestra cómo usar miembros del espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx). Este espacio de nombres ya no se actualiza con las nuevas características por lo que te recomendamos que uses el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) en su lugar. El espacio de nombres **Windows.Services.Store** admite los tipos de complemento más recientes, como los complementos consumibles administrados por la tienda y suscripciones y está diseñado para ser compatible con futuros tipos de productos y características compatibles con el centro de partners y la tienda. El espacio de nombres **Windows.Services.Store** se introdujo en Windows 10, versión 1607 y solo se puede usar en proyectos destinados a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio. Para obtener más información, consulta [pruebas y compras desde la aplicación](in-app-purchases-and-trials.md).

Para habilitar esta funcionalidad, crearás unas cuantas entradas de productos para franjas de precios específicas, cada una de ellas capaz de representar cientos de productos en un catálogo. Usa la sobrecarga del método [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), que especifica una oferta definida por la aplicación asociada con un producto incluido en la Store. Además de especificar una asociación entre la oferta y el producto durante la llamada, tu aplicación también debería pasar un objeto [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263384) que contenga los detalles de la oferta del catálogo de gran volumen. Si estos detalles no se suministran, se usarán los detalles que figuran sobre el producto.

La Tienda usará únicamente el *offerId* de la solicitud de compra en la clase [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) resultante. Este proceso no modifica directamente la información suministrada inicialmente al [incluir el producto desde la aplicación en la Store](../publish/add-on-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

-   En este tema se aborda el soporte técnico de la Storepara la representación de varias ofertas desde la aplicación usando un solo producto desde la aplicación incluido en la Store. Si no estás familiarizado con las compras desde la aplicación, consulta [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md) para ver información sobre la licencia y cómo incluir de la forma adecuada tu compra desde la aplicación en la Store.
-   Cuando codificas y pruebas nuevas ofertas desde la aplicación por primera vez, debes usar el objeto [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) en lugar del objeto [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765). De esta manera, puedes comprobar la lógica de la licencia con llamadas simuladas al servidor de licencias, en lugar de llamar al servidor activo. Para ello, debes personalizar el archivo llamado WindowsStoreProxy.xml en %perfil_usuario%\\AppData\\local\\packages\\&lt;nombre_paquete&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. El simulador de Microsoft Visual Studio crea este archivo la primera vez que se ejecuta la aplicación, aunque también puedes cargar un archivo personalizado en el tiempo de ejecución. Para obtener más información, consulta [Uso del archivo WindowsStoreProxy.xml con CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Este tema también hace referencia a los ejemplos de código que se proporcionan en la [muestra de la Store](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Esta muestra es ideal para conseguir experiencia práctica con las diferentes opciones de monetización que se proporcionan para las aplicaciones para la Plataforma universal de Windows (UWP).

## <a name="make-the-purchase-request-for-the-in-app-product"></a>Realizar la solicitud de compra del producto desde la aplicación

La solicitud de compra de un producto concreto dentro de un catálogo voluminoso se controla en gran medida como cualquier otra solicitud de compra dentro de la aplicación. Cuando tu aplicación llama a la nueva sobrecarga del método [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), proporciona tanto un *OfferId* como un objeto [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263390) rellenado con el nombre del producto desde la aplicación.

> [!div class="tabbedCodeSnippets"]
[!code-cs[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#MakePurchaseRequest)]

## <a name="report-fulfillment-of-the-in-app-offer"></a>Notificar la cumplimentación de la oferta desde la aplicación

Tu aplicación deberá notificar a la Store la cumplimentación del producto en cuanto la oferta se haya suministrado localmente. En un escenario con un catálogo voluminoso, si tu aplicación no notifica la cumplimentación de la oferta, el usuario no podrá comprar ninguna oferta desde la aplicación a través de la misma lista de productos de la Store.

Tal y como se ha mencionado anteriormente, la Store usa la información de las ofertas suministrada solamente para rellenar [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) y no crea ninguna asociación permanente entre una oferta del catálogo extenso y la lista de productos de la Store. En consecuencia, es necesario realizar un seguimiento de la autorización del usuario para los productos y ofrecer al usuario contexto específico del producto (como el nombre del artículo que se va a comprar o detalles al respecto) fuera de la operación [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync).

El siguiente código refleja la llamada de cumplimentación y un patrón de mensaje de interfaz de usuario en el que se inserta la información de la oferta en cuestión. Si no existe información específica del producto, el ejemplo usa información del elemento [ListingInformation](https://msdn.microsoft.com/library/windows/apps/br225163) del producto.

> [!div class="tabbedCodeSnippets"]
[!code-cs[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#ReportFulfillment)]

## <a name="related-topics"></a>Temas relacionados

* [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md)
* [Habilitar compras de productos consumibles desde la aplicación](enable-consumable-in-app-product-purchases.md)
* [Muestra de la Store (muestra pruebas y compras desde la aplicación)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263382)
* [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263384)
