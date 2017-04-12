---
author: mcleanbyron
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: "Aprende a usar el espacio de nombres Windows.Services.Store para obtener información del producto relacionada con la Tienda para la aplicación actual o uno de sus complementos."
title: "Obtener información de producto para aplicaciones y complementos"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, compras desde la aplicación, in-app purchases, IAP, complementos, add-ons, Windows.Services.Store"
ms.openlocfilehash: 5ce47241bad229d0f44e14d3f9332603e6776f2f
ms.sourcegitcommit: d053f28b127e39bf2aee616aa52bb5612194dc53
translationtype: HT
---
# <a name="get-product-info-for-apps-and-add-ons"></a>Obtener información de producto para aplicaciones y complementos

Las aplicaciones diseñadas para Windows 10, versión 1607 o posterior pueden usar métodos de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) en el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para acceder a información relacionada con la Tienda para la aplicación actual o uno de sus complementos (también conocidos como productos desde la aplicación o IAP). Los siguientes ejemplos de este artículo muestran cómo hacer esto para diferentes escenarios. Para una muestra completa, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> Este artículo es aplicable a las aplicaciones dirigidas a Windows 10, versión 1607 o posterior. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulta [Compras desde la aplicación y pruebas con el espacio de nombres Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Requisitos previos

Estos ejemplos cumplen los siguientes requisitos:
* Un proyecto de Visual Studio para una aplicación de la Plataforma universal de Windows (UWP) destinado a Windows 10, versión 1607 o posterior.
* Has creado una aplicación en el panel del Centro de desarrollo de Windows, y esta aplicación está publicada y disponible en la Tienda. Esta puede ser una aplicación que quieres publicar para los clientes o puede ser una aplicación básica que cumpla con los requisitos mínimos del [Kit para la certificación de aplicaciones en Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) que se usa solo con fines de prueba. Para obtener más información, consulta la [guía para prueba](in-app-purchases-and-trials.md#testing).

El código de estos ejemplos supone que:
* El código se ejecuta en el contexto de una [página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contiene un elemento [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` y un elemento [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una aplicación de usuario único que se ejecuta solamente en el contexto del usuario que inició la aplicación. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

Para una aplicación de ejemplo completa, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> Si tienes una aplicación de escritorio que usa el [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop), puede que tengas que agregar código adicional que no se muestra en estos ejemplos para configurar el objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

## <a name="get-info-for-the-current-app"></a>Obtener información de la aplicación actual

Para obtener información de producto de la Tienda sobre la aplicación actual, usa el método [GetStoreProductForCurrentAppAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getstoreproductforcurrentappasync.aspx). Este es un método asincrónico que devuelve un objeto [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que puedes usar para obtener información como el precio.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs#GetAppInfo)]

## <a name="get-info-for-products-with-known-store-ids"></a>Obtener información de productos con id. de la Tienda conocidos

Para obtener información de productos de la Tienda para las aplicaciones o complementos para los que ya conoces el [id. de la Tienda](in-app-purchases-and-trials.md#store_ids), usa el método [GetStoreProductsAsync](https://msdn.microsoft.com/library/windows/apps/mt706579.aspx). Este es un método asincrónico que devuelve una colección de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representan cada una de las aplicaciones o los complementos. Además de los id. de la Tienda, debes pasar una lista de cadenas a este método que identifican los tipos de complementos. Para obtener una lista de los valores de cadena compatibles, consulta la propiedad [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

El siguiente ejemplo recupera información para complementos duraderos con los id. de la Tienda especificados.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetProductInfoPage.xaml.cs#GetProductInfo)]

## <a name="get-info-for-add-ons-that-are-available-for-the-current-app"></a>Obtener información sobre los complementos que están disponibles para la aplicación actual

Para obtener información de producto de la Tienda para los complementos que están disponibles para la aplicación actual, usa el método [GetAssociatedStoreProductsAsync](https://msdn.microsoft.com/library/windows/apps/mt706571.aspx). Este es un método asincrónico que devuelve una colección de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representan cada uno de los complementos disponibles. Debes pasar una lista de cadenas a este método que identifiquen los tipos de complementos que quieras recuperar. Para una lista de los valores de cadena compatibles, consulta la propiedad [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

> [!NOTE]
> Si la aplicación tiene muchos complementos, puedes usar de forma alternativa el método [GetAssociatedStoreProductsWithPagingAsync](https://msdn.microsoft.com/library/windows/apps/mt706572.aspx) para emplear la paginación con el fin de devolver los resultados de complementos.

En el siguiente ejemplo, se recupera información respecto a todos los complementos duraderos, complementos consumibles administrados por la Tienda y complementos consumibles administrados por el desarrollador.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAddOnInfoPage.xaml.cs#GetAddOnInfo)]


## <a name="get-info-for-add-ons-for-the-current-app-that-the-current-user-is-entitled-to-use"></a>Obtén información respecto a los complementos de la aplicación actual que el usuario actual tenga derecho a usar

Para obtener información de producto de la Tienda para los complementos que el usuario actual tenga derecho a usar, usa el método [GetUserCollectionAsync](https://msdn.microsoft.com/library/windows/apps/mt706580.aspx). Este es un método asincrónico que devuelve una colección de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representan cada uno de los complementos. Debes pasar una lista de cadenas a este método que identifiquen los tipos de complementos que quieras recuperar. Para una lista de los valores de cadena compatibles, consulta la propiedad [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

> [!NOTE]
> Si la aplicación tiene muchos complementos, puedes usar de forma alternativa el método [GetUserCollectionWithPagingAsync](https://msdn.microsoft.com/library/windows/apps/mt706581.aspx) para emplear la paginación con el fin de devolver los resultados de complementos.

El siguiente ejemplo recupera información sobre complementos duraderos con los id. de la Tienda especificados.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetUserCollectionPage.xaml.cs#GetUserCollection)]

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
