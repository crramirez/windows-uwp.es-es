---
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: Aprende a usar el espacio de nombres Windows.Services.Store para obtener información del producto relacionada con la Tienda para la aplicación actual o uno de sus complementos.
title: Obtener información de producto para aplicaciones y complementos
ms.date: 02/08/2018
ms.topic: article
keywords: Windows 10, UWP, compras desde la aplicación, IAPs, complementos, Windows. Services. Store
ms.localizationpriority: medium
ms.openlocfilehash: 63072c243e4528d4625fe300697b1edbff64383a
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363138"
---
# <a name="get-product-info-for-apps-and-add-ons"></a>Obtener información de producto para aplicaciones y complementos

En este artículo se muestra cómo usar los métodos de la clase [StoreContext](/uwp/api/windows.services.store.storecontext) en el espacio de nombres [Windows. Services. Store](/uwp/api/windows.services.store) para acceder a información relacionada con el almacén de la aplicación actual o de uno de sus complementos.

Para obtener una aplicación de ejemplo completa, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> El espacio de nombres **Windows. Services. Store** se presentó en la versión 1607 de Windows 10 y solo se puede usar en proyectos que tengan como destino la **edición de aniversario de windows 10 (10,0; Compilación 14393)** o una versión posterior en Visual Studio. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulte [este artículo](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Requisitos previos

Estos ejemplos cumplen los siguientes requisitos:
* Un proyecto de Visual Studio para una aplicación Plataforma universal de Windows (UWP) destinada a **Windows 10 aniversario Edition (10,0; Compilación 14393)** o una versión posterior.
* Ha [creado un envío de aplicación](../publish/app-submissions.md) en el centro de Partners y esta aplicación se publica en la tienda. Opcionalmente, puede configurar la aplicación para que no se pueda detectar en el almacén mientras la prueba. Para obtener más información, consulte nuestra [Guía de pruebas](in-app-purchases-and-trials.md#testing).
* Si quiere obtener información del producto para un complemento de la aplicación, también debe [crear el complemento en el centro de Partners](../publish/add-on-submissions.md).

El código de estos ejemplos supone que:
* El código se ejecuta en el contexto de una [página](/uwp/api/windows.ui.xaml.controls.page) que contiene un elemento [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) denominado ```workingProgressRing``` y un elemento [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una de usuario único que se ejecuta solamente en el contexto del usuario que la inició. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si tiene una aplicación de escritorio que usa el [puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop), es posible que deba agregar código adicional que no se muestra en estos ejemplos para configurar el objeto [StoreContext](/uwp/api/windows.services.store.storecontext) . Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

## <a name="get-info-for-the-current-app"></a>Obtener información de la aplicación actual

Para obtener información de producto de la Tienda sobre la aplicación actual, usa el método [GetStoreProductForCurrentAppAsync](/uwp/api/windows.services.store.storecontext.getstoreproductforcurrentappasync). Este es un método asincrónico que devuelve un objeto [StoreProduct](/uwp/api/windows.services.store.storeproduct) que puedes usar para obtener información como el precio.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs" id="GetAppInfo":::

## <a name="get-info-for-add-ons-with-known-store-ids-that-are-associated-with-the-current-app"></a>Obtener información de complementos con identificadores de almacén conocidos que están asociados a la aplicación actual

Para obtener información de producto de la tienda para los complementos que están asociados a la aplicación actual y para los que ya conoce los [identificadores de almacén](in-app-purchases-and-trials.md#store_ids), use el método [GetStoreProductsAsync](/uwp/api/windows.services.store.storecontext.getstoreproductsasync) . Se trata de un método asincrónico que devuelve una colección de objetos [StoreProduct](/uwp/api/windows.services.store.storeproduct) que representan cada uno de los complementos. Además de los id. de la Tienda, debes pasar una lista de cadenas a este método que identifican los tipos de complementos. Para obtener una lista de los valores de cadena compatibles, consulta la propiedad [ProductKind](/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> El método **GetStoreProductsAsync** devuelve información del producto para los complementos especificados que están asociados a la aplicación, independientemente de si los complementos están actualmente disponibles para su compra. Para recuperar información de todos los complementos de la aplicación actual que se pueden comprar actualmente, use el método **GetAssociatedStoreProductsAsync** tal como se describe en la [sección siguiente](#get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app) .

En este ejemplo se recupera información para los complementos duraderos con los identificadores de almacén especificados que están asociados a la aplicación actual.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/GetProductInfoPage.xaml.cs" id="GetProductInfo":::

## <a name="get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app"></a>Obtener información sobre los complementos que están disponibles para su compra desde la aplicación actual

Para obtener información del producto de la tienda para los complementos que están actualmente disponibles para su compra en la aplicación actual, use el método [GetAssociatedStoreProductsAsync](/uwp/api/windows.services.store.storecontext.getassociatedstoreproductsasync) . Se trata de un método asincrónico que devuelve una colección de objetos [StoreProduct](/uwp/api/windows.services.store.storeproduct) que representan cada uno de los complementos disponibles. Debes pasar una lista de cadenas a este método que identifiquen los tipos de complementos que quieras recuperar. Para obtener una lista de los valores de cadena compatibles, consulta la propiedad [ProductKind](/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> Si la aplicación tiene muchos complementos que están disponibles para su compra, también puede usar el método [GetAssociatedStoreProductsWithPagingAsync](/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsWithPagingAsync) para usar la paginación para devolver los resultados del complemento.

En el ejemplo siguiente se recupera información de todos los complementos duraderos, los complementos consumibles administrados por el almacén y los complementos consumibles administrados por el desarrollador que están disponibles para su compra en la aplicación actual.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/GetAddOnInfoPage.xaml.cs" id="GetAddOnInfo":::


## <a name="get-info-for-add-ons-for-the-current-app-that-the-user-has-purchased"></a>Obtener información sobre los complementos de la aplicación actual que el usuario ha adquirido

Para obtener información del producto de la tienda para los complementos que ha adquirido el usuario actual, use el método [GetUserCollectionAsync](/uwp/api/windows.services.store.storecontext.getusercollectionasync) . Este es un método asincrónico que devuelve una colección de objetos [StoreProduct](/uwp/api/windows.services.store.storeproduct) que representan cada uno de los complementos. Debes pasar una lista de cadenas a este método que identifiquen los tipos de complementos que quieras recuperar. Para obtener una lista de los valores de cadena compatibles, consulta la propiedad [ProductKind](/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> Si la aplicación tiene muchos complementos, también puede usar el método [GetUserCollectionWithPagingAsync](/uwp/api/windows.services.store.storecontext.getusercollectionwithpagingasync) para usar la paginación para devolver los resultados del complemento.

En el ejemplo siguiente se recupera información de complementos duraderos con los [identificadores de almacén](in-app-purchases-and-trials.md#store_ids)especificados.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/GetUserCollectionPage.xaml.cs" id="GetUserCollection":::

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
