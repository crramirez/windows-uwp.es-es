---
author: Xansky
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: Aprende a usar el espacio de nombres Windows.Services.Store para obtener información del producto relacionada con la Store para la aplicación actual o uno de sus complementos.
title: Obtener información de producto para aplicaciones y complementos
ms.author: mhopkins
ms.date: 02/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, compras desde la aplicación, in-app purchases, IAP, complementos, add-ons, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: 417478df3b82967656d2210b3b532c5341f1fb2e
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "5479353"
---
# <a name="get-product-info-for-apps-and-add-ons"></a>Obtener información de producto para aplicaciones y complementos

En este artículo se demuestra cómo usar métodos de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) en el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para obtener acceso a información relacionada con Microsoft Store para la aplicación actual y uno de sus complementos.

Para una aplicación de ejemplo completa, consulta la [muestra de Microsoft Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> El espacio de nombres **Windows.Services.Store** se introdujo en Windows 10, versión 1607 y solo se puede usar en proyectos destinados a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulta [este artículo](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Requisitos previos

Estos ejemplos cumplen los siguientes requisitos:
* Un proyecto de Visual Studio para una aplicación de la Plataforma universal de Windows (UWP) destinado a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o un versión posterior.
* Has [creado un envío de aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) en el panel del Centro de desarrollo de Windows, y esta aplicación está publicada y disponible en la Store. De manera opcional, puedes configurar la aplicación para que no se pueda descubrir en la Store mientras la pruebas. Para obtener más información, consulta nuestra [guía para prueba](in-app-purchases-and-trials.md#testing).
* Si quieres obtener información de producto de un complemento de la aplicación, también debes [crear el complemento en el panel del Centro de desarrollo](../publish/add-on-submissions.md).

El código de estos ejemplos supone que:
* El código se ejecuta en el contexto de una [página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contiene un elemento [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` y un elemento [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una aplicación de usuario único que se ejecuta solamente en el contexto del usuario que inició la aplicación. Para más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si tienes una aplicación de escritorio que usa el [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop), puede que tengas que agregar código adicional que no se muestra en estos ejemplos para configurar el objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

## <a name="get-info-for-the-current-app"></a>Obtener información de la aplicación actual

Para obtener información de producto de la Store sobre la aplicación actual, usa el método [GetStoreProductForCurrentAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductforcurrentappasync). Este es un método asincrónico que devuelve un objeto [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que puedes usar para obtener información como el precio.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs#GetAppInfo)]

## <a name="get-info-for-add-ons-with-known-store-ids-that-are-associated-with-the-current-app"></a>Obtener información de complementos con Id. de Store asociados con la aplicación actual

Para obtener información de complementos asociados con la aplicación actual para los que ya conoces los [Id. de Store](in-app-purchases-and-trials.md#store_ids), usa el método [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync). Este es un método asincrónico que devuelve una colección de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representan cada uno de los complementos. Además de los Id. de Store, debes pasar una lista de cadenas a este método que identifican los tipos de complementos. Para una lista de los valores de cadena compatibles, consulta la propiedad [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> El método **GetStoreProductsAsync** devuelve la información del producto de los complementos especificados asociados con la aplicación, independientemente de si los complementos están disponibles actualmente para la compra. Para recuperar información sobre todos los complementos de la aplicación actual que actualmente se pueden comprar, usa el método **GetAssociatedStoreProductsAsync** tal como se describe en la [siguiente sección](#get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app) en su lugar.

Este ejemplo recupera información para complementos duraderos con los Id. de Store asociados con la aplicación actual.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetProductInfoPage.xaml.cs#GetProductInfo)]

## <a name="get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app"></a>Obtener información sobre los complementos que están disponibles para su compra desde la aplicación actual

Para obtener información de producto de la Store para los complementos que están actualmente disponibles para su compra desde la aplicación actual, usa el método [GetAssociatedStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstoreproductsasync). Este es un método asincrónico que devuelve una colección de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representan cada uno de los complementos disponibles. Debes pasar una lista de cadenas a este método que identifiquen los tipos de complementos que quieras recuperar. Para una lista de los valores de cadena compatibles, consulta la propiedad [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> Si la aplicación tiene muchos complementos que están disponibles para su compra, puedes usar de forma alternativa el método [GetAssociatedStoreProductsWithPagingAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsWithPagingAsync) para emplear la paginación con el fin de devolver los resultados de complementos.

En el siguiente ejemplo, se recupera información respecto a todos los complementos duraderos, complementos consumibles administrados por la Store y complementos consumibles administrados por el desarrollador que están disponibles para su compra desde la aplicación actual.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAddOnInfoPage.xaml.cs#GetAddOnInfo)]


## <a name="get-info-for-add-ons-for-the-current-app-that-the-user-has-purchased"></a>Obtener información sobre los complementos para la aplicación actual que el usuario ha adquirido

Para obtener información de producto de la Store para los complementos que el usuario ha adquirido, usa el método [GetUserCollectionAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getusercollectionasync). Este es un método asincrónico que devuelve una colección de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representan cada uno de los complementos. Debes pasar una lista de cadenas a este método que identifiquen los tipos de complementos que quieras recuperar. Para una lista de los valores de cadena compatibles, consulta la propiedad [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

> [!NOTE]
> Si la aplicación tiene muchos complementos, puedes usar de forma alternativa el método [GetUserCollectionWithPagingAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getusercollectionwithpagingasync) para emplear la paginación con el fin de devolver los resultados de complementos.

El siguiente ejemplo recupera información sobre complementos duraderos con los [Id. de Store](in-app-purchases-and-trials.md#store_ids).

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetUserCollectionPage.xaml.cs#GetUserCollection)]

## <a name="related-topics"></a>Artículos relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Muestra de la Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
