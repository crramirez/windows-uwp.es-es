---
author: mcleanbyron
description: Describe el esquema de datos JSON extendido para productos de la Tienda en el espacio de nombres Windows.Services.Store.
title: Esquemas de datos para productos de la Tienda
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: 
ms.openlocfilehash: 2a294dc490a0b7bed5e426ff26dfed948f337dfa
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/21/2017
---
# <a name="data-schemas-for-store-products"></a>Esquemas de datos para productos de la Tienda

Cuando envías un producto como una aplicación o un complemento a la Tienda, esta mantiene un conjunto completo de datos para el producto y sus licencias. En el código de la aplicación, puedes obtener acceso mediante programación a algunos de estos datos con las propiedades del espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx). Por ejemplo, puedes recuperar la descripción y el precio de la aplicación actual o un complemento de la aplicación actual mediante las propiedades [StoreProduct.Description](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_Description) y [StoreProduct.Price](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_Price).

Sin embargo, muchos de los datos de productos de la Tienda no se exponen por las propiedades predefinidas en el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx). Para obtener acceso a los datos completos de un producto en tu código, puedes usar las siguientes propiedades generales en su lugar:

* [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_)
* [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku#Windows_Services_Store_StoreSku_ExtendedJsonData_)
* [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAvailability_ExtendedJsonData_)
*   [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata#Windows_Services_Store_StoreCollectionData_ExtendedJsonData_)
*   [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense#Windows_Services_Store_StoreAppLicense_ExtendedJsonData_)
* [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense#Windows_Services_Store_StoreLicense_ExtendedJsonData_)
*   [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties#Windows_Services_Store_StorePurchaseProperties_ExtendedJsonData_)

Estas propiedades devuelven los datos completos del objeto correspondiente como una cadena con formato JSON. En este artículo se proporciona el esquema completo para los datos devueltos por estas propiedades.

> [!NOTE]
> Los productos de la Tienda se organizan en una jerarquía de objetos de [producto](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct), [SKU](https://docs.microsoft.com/uwp/api/windows.services.store.storesku), y [disponibilidad](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability). Para obtener más información, consulta [Productos, SKU y disponibilidades](in-app-purchases-and-trials.md#products-skus).

## <a name="schema-for-storeproduct-storesku-storeavailability-and-storecollectiondata"></a>Esquema para StoreProduct, StoreSku, StoreAvailability y StoreCollectionData

El esquema siguiente describe la cadena con formato JSON devuelta por [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_). Las propiedades [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku#Windows_Services_Store_StoreSku_ExtendedJsonData_), [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAvailability_ExtendedJsonData_) y [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata#Windows_Services_Store_StoreCollectionData_ExtendedJsonData_) solo devuelven las partes del esquema que se definen en las rutas de acceso ```DisplaySkuAvailabilities\Sku```, ```DisplaySkuAvailabilities\Availabilities``` y ```DisplaySkuAvailabilities\Sku\CollectionData```, respectivamente.

Para ver un ejemplo de una cadena con formato JSON devuelta por [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_), consulta [esta sección](#product-example).

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json#L1-L603)]

<span id="product-example" />
### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra una cadena con formato JSON devuelta por la propiedad [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_) para la aplicación.

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json#L1-L268)]

## <a name="schema-for-storeapplicense-and-storelicense"></a>Esquema para StoreAppLicense y StoreLicense

El esquema siguiente describe la cadena con formato JSON devuelta por [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAppLicense_ExtendedJsonData_). La propiedad [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreLicense_ExtendedJsonData_) devuelve solamente las partes del esquema que se definen en la ruta de acceso ```productAddOns```.

Para ver un ejemplo de una cadena con formato JSON devuelta por [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAppLicense_ExtendedJsonData_), consulta [esta sección](#license-example).

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json#L1-L80)]

<span id="license-example" />
### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra una cadena con formato JSON devuelta por la propiedad [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAppLicense_ExtendedJsonData_) para la aplicación.

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json#L1-L28)]

## <a name="schema-for-storepurchaseproperties"></a>Esquema para StorePurchaseProperties

El esquema siguiente describe la cadena con formato JSON devuelta por [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties#Windows_Services_Store_StorePurchaseProperties_ExtendedJsonData_).

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json#L1-L12)]

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
