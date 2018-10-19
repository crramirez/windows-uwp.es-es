---
author: Xansky
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: Aprende a usar el espacio de nombres Windows.Services.Store para comprar una aplicación o uno de sus complementos.
title: Habilitar compras desde la aplicación de aplicaciones y complementos
keywords: windows 10, uwp, complementos, add-ons, compras desde la aplicación, in-app purchases, IAP, Windows.Services.Store
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7bf04632c4c99f2d58e3cd936e678af168750ff0
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "4948594"
---
# <a name="enable-in-app-purchases-of-apps-and-add-ons"></a>Habilitar compras desde la aplicación de aplicaciones y complementos

En este artículo se demuestra cómo usar miembros del espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para solicitar la compra de la aplicación actual o uno de sus complementos para el usuario. Por ejemplo, si el usuario tiene actualmente una versión de prueba de la aplicación, puedes usar este proceso para adquirir una licencia completa para el usuario. Como alternativa, puedes usar este proceso para comprar un complemento, como un nuevo nivel de juego para el usuario.

Para solicitar la compra de una aplicación o un complemento, el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) proporciona varios métodos diferentes:
* Si conoces el [Id. de la Store](in-app-purchases-and-trials.md#store_ids) de la aplicación o complemento, puedes usar el método [RequestPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestpurchaseasync) de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx).
* Si ya tienes un objeto [**StoreProduct**, **StoreSku** o **StoreAvailability**](in-app-purchases-and-trials.md#products-skus) que representa la aplicación o el complemento, puedes usar los métodos **RequestPurchaseAsync** de estos objetos. Para ver ejemplos de formas diferentes de recuperar un **StoreProduct** en tu código, consulta [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md).

Cada método presenta una interfaz de usuario de compra estándar para el usuario y se finaliza asincrónicamente una vez completada la transacción. El método devuelve un objeto que indica si la transacción se realizó correctamente.

> [!NOTE]
> El espacio de nombres **Windows.Services.Store** se introdujo en Windows 10, versión 1607 y solo se puede usar en proyectos destinados a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulta [este artículo](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Requisitos previos

Este ejemplo tiene los siguientes requisitos previos:
* Un proyecto de Visual Studio para una aplicación de la Plataforma universal de Windows (UWP) destinado a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o un versión posterior.
* Has [creado un envío de aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) en el panel del Centro de desarrollo de Windows, y esta aplicación está publicada y disponible en la Store. De manera opcional, puedes configurar la aplicación para que no se pueda descubrir en la Store mientras la pruebas. Para obtener más información, consulta nuestra [guía para prueba](in-app-purchases-and-trials.md#testing).
* Si quieres habilitar compras desde la aplicación de un complemento de la aplicación, también debes [crear el complemento en el panel del Centro de desarrollo](../publish/add-on-submissions.md).

El código de este ejemplo supone que:
* El código se ejecuta en el contexto de una [página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contiene un elemento [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` y un elemento [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una aplicación de usuario único que se ejecuta solamente en el contexto del usuario que inició la aplicación. Para más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si tienes una aplicación de escritorio que usa el [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop), puede que tengas que agregar código adicional que no se muestra en este ejemplo para configurar el objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Ejemplo de código

En este ejemplo se muestra cómo usar el método [RequestPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestpurchaseasync) de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) para comprar una aplicación o complemento con un [Id. de la Store](in-app-purchases-and-trials.md#store-ids) conocido. Para una aplicación de ejemplo completa, consulta la [muestra de la Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnablePurchases](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseAddOnPage.xaml.cs#PurchaseAddOn)]

## <a name="video"></a>Vídeo

Vea el vídeo siguiente para obtener información general de cómo implementar las compras desde la aplicación en la aplicación.
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Muestra de la Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
