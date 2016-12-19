---
author: mcleanbyron
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: "Aprende a usar el espacio de nombres Windows.Services.Store para comprar una aplicación o uno de sus complementos."
title: "Habilitar compras desde la aplicación de aplicaciones y complementos"
keywords: "muestra de código de oferta desde la aplicación"
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: 05a93f3124324d7308f5494ad14a15bfd6a4e698

---

# <a name="enable-in-app-purchases-of-apps-and-add-ons"></a>Habilitar compras desde la aplicación de aplicaciones y complementos

Las aplicaciones orientadas a Windows 10, versión 1607 o posterior, pueden usar miembros del espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para solicitar la compra de la aplicación actual o uno de sus complementos (también conocidos como productos desde la aplicación o IAP) para el usuario. Por ejemplo, si el usuario tiene actualmente una versión de prueba de la aplicación, puedes usar este proceso para adquirir una licencia completa para el usuario. Como alternativa, puedes usar este proceso para comprar un complemento, como un nuevo nivel de juego para el usuario.

Para solicitar la compra de una aplicación o un complemento, [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) proporciona varios métodos diferentes:
* Si conoces el [Id. de la Tienda](in-app-purchases-and-trials.md#store_ids) de la aplicación o complemento, puedes usar el método [RequestPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.requestpurchaseasync.aspx) de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx).
* Si ya tienes un objeto [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx), [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) o [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) que representa la aplicación o el complemento, puedes usar los métodos **RequestPurchaseAsync** de estos objetos.

Cada método presenta una interfaz de usuario de compra estándar para el usuario y se finaliza asincrónicamente una vez completada la transacción. El método devuelve un objeto que indica si la transacción se realizó correctamente.

>**Nota**&nbsp;&nbsp;Este artículo es aplicable a las aplicaciones orientadas a Windows 10, versión 1607 o posterior. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulta [Compras desde la aplicación y pruebas con el espacio de nombres Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Requisitos previos

Este ejemplo tiene los siguientes requisitos previos:
* Un proyecto de Visual Studio para una aplicación de la Plataforma universal de Windows (UWP) destinado a Windows 10, versión 1607 o posterior.
* Has creado una aplicación en el panel del Centro de desarrollo de Windows, y esta aplicación está publicada y disponible en la Tienda. Esta puede ser una aplicación que quieres publicar para los clientes o puede ser una aplicación básica que cumpla con los requisitos mínimos del [Kit para la certificación de aplicaciones en Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) que se usa solo con fines de prueba. Para obtener más información, consulta la [guía para prueba](in-app-purchases-and-trials.md#testing).

El código de este ejemplo supone que:
* El código se ejecuta en el contexto de una [página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contiene un elemento [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` y un elemento [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una de usuario único que se ejecuta solamente en el contexto del usuario que la inició. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

>**Nota**&nbsp;&nbsp;Si tienes una aplicación de escritorio que usa el [Puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop), tienes que agregar código adicional que no se muestra en este ejemplo para configurar el objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Ejemplo de código

En este ejemplo se muestra cómo usar el método [RequestPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.requestpurchaseasync.aspx) de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) para comprar una aplicación o complemento con un [Id. de la Tienda](in-app-purchases-and-trials.md#store_ids) conocido.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnablePurchases](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseAddOnPage.xaml.cs#PurchaseAddOn)]

Para obtener una aplicación de ejemplo completa, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Dec16_HO1-->


