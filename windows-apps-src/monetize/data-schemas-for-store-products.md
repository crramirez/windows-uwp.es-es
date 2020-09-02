---
description: Describe el esquema de datos JSON extendido para los productos de la tienda en el espacio de nombres Windows. Services. Store.
title: Esquemas de datos para productos de Store
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP, ExtendedJsonData, productos de la tienda, esquema
ms.localizationpriority: medium
ms.openlocfilehash: e09e02d12afc436f5d22d11fad85fb0bad3507f6
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363188"
---
# <a name="data-schemas-for-store-products"></a>Esquemas de datos para productos de Store

Cuando se envía un producto, como una aplicación o un complemento a la tienda, el almacén mantiene un conjunto completo de datos para el producto y sus licencias. En el código de la aplicación, puede acceder mediante programación a algunos de estos datos mediante las propiedades del espacio de nombres [Windows. Services. Store](/uwp/api/windows.services.store) . Por ejemplo, puede recuperar la descripción y el precio de la aplicación actual o un complemento de la aplicación actual mediante las propiedades [StoreProduct. Description](/uwp/api/windows.services.store.storeproduct.Description) y [StoreProduct. Price](/uwp/api/windows.services.store.storeproduct.Price) .

Sin embargo, gran parte de los datos de los productos de la tienda no se exponen mediante propiedades predefinidas en el espacio de nombres [Windows. Services. Store](/uwp/api/windows.services.store) . Para tener acceso a los datos completos de un producto en el código, puede usar en su lugar las siguientes propiedades generales:

* [StoreProduct.ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)
* [StoreSku.ExtendedJsonData](/uwp/api/windows.services.store.storesku.ExtendedJsonData)
* [StoreAvailability.ExtendedJsonData](/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)
*   [StoreCollectionData.ExtendedJsonData](/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData)
*   [StoreAppLicense.ExtendedJsonData](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)
* [StoreLicense.ExtendedJsonData](/uwp/api/windows.services.store.storelicense.ExtendedJsonData)
*   [StorePurchaseProperties.ExtendedJsonData](/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData)

Estas propiedades devuelven los datos completos del objeto correspondiente como una cadena con formato JSON. En este artículo se proporciona el esquema completo de los datos devueltos por estas propiedades.

> [!NOTE]
> Los productos del almacén se organizan en una jerarquía de objetos de [producto](/uwp/api/windows.services.store.storeproduct), [SKU](/uwp/api/windows.services.store.storesku)y [disponibilidad](/uwp/api/windows.services.store.storeavailability) . Para obtener más información, consulte [productos, SKU y disponibilidad](in-app-purchases-and-trials.md#products-skus).

## <a name="schema-for-storeproduct-storesku-storeavailability-and-storecollectiondata"></a>Esquema para StoreProduct, StoreSku, StoreAvailability y StoreCollectionData

En el esquema siguiente se describe la cadena con formato JSON devuelta por [StoreProduct. ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData). Las propiedades [StoreSku. ExtendedJsonData](/uwp/api/windows.services.store.storesku.ExtendedJsonData), [StoreAvailability. ExtendedJsonData](/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)y [StoreCollectionData. ExtendedJsonData](/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData) solo devuelven las partes del esquema definidas bajo las rutas de acceso `DisplaySkuAvailabilities\Sku` , `DisplaySkuAvailabilities\Availabilities` y `DisplaySkuAvailabilities\Sku\CollectionData` , respectivamente.

Para obtener un ejemplo de una cadena con formato JSON devuelta por [StoreProduct. ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData), vea [esta sección](#product-example).

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json" range="1-729":::

<span id="product-example" />

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra una cadena con formato JSON devuelta por la propiedad [StoreProduct. ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) para la aplicación.

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json" range="1-268":::

## <a name="schema-for-storeapplicense-and-storelicense"></a>Esquema para StoreAppLicense y StoreLicense

En el esquema siguiente se describe la cadena con formato JSON devuelta por [StoreAppLicense. ExtendedJsonData](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData). La propiedad [StoreLicense. ExtendedJsonData](/uwp/api/windows.services.store.storelicense.ExtendedJsonData) devuelve solo las partes del esquema definidas bajo la `productAddOns` ruta de acceso.

Para obtener un ejemplo de una cadena con formato JSON devuelta por [StoreAppLicense. ExtendedJsonData](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData), vea [esta sección](#license-example).

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json" range="1-80":::

<span id="license-example" />

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra una cadena con formato JSON devuelta por la propiedad [StoreAppLicense. ExtendedJsonData](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) para la aplicación.

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json" range="1-28":::

## <a name="schema-for-storepurchaseproperties"></a>Esquema para StorePurchaseProperties

En el esquema siguiente se describe la cadena con formato JSON devuelta por [StorePurchaseProperties. ExtendedJsonData](/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData).

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json" range="1-12":::

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
