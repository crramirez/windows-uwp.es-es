---
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: Aprende a usar el espacio de nombres Windows.Services.Store para comprar una aplicación o uno de sus complementos.
title: Habilitar compras desde la aplicación para aplicaciones y complementos
keywords: Windows 10, UWP, complementos, compras desde la aplicación, IAPs, Windows. Services. Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3064a651715329a3602c0c3539394d2ce72f90a7
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362818"
---
# <a name="enable-in-app-purchases-of-apps-and-add-ons"></a>Habilitar compras desde la aplicación para aplicaciones y complementos

En este artículo se muestra cómo usar miembros en el espacio de nombres [Windows. Services. Store](/uwp/api/windows.services.store) para solicitar la compra de la aplicación actual o de uno de sus complementos para el usuario. Por ejemplo, si el usuario tiene actualmente una versión de prueba de la aplicación, puedes usar este proceso para adquirir una licencia completa para el usuario. Como alternativa, puedes usar este proceso para comprar un complemento, como un nuevo nivel de juego para el usuario.

Para solicitar la compra de una aplicación o un complemento, el espacio de nombres [Windows. Services. Store](/uwp/api/windows.services.store) proporciona varios métodos diferentes:
* Si conoces el [Id. de la Tienda](in-app-purchases-and-trials.md#store_ids) de la aplicación o complemento, puedes usar el método [RequestPurchaseAsync](/uwp/api/windows.services.store.storecontext.requestpurchaseasync) de la clase [StoreContext](/uwp/api/windows.services.store.storecontext).
* Si ya tiene un objeto [ **StoreProduct**, **StoreSku**o **StoreAvailability** ](in-app-purchases-and-trials.md#products-skus) que representa la aplicación o el complemento, puede usar los métodos **RequestPurchaseAsync** de estos objetos. Para obtener ejemplos de diferentes maneras de recuperar un **StoreProduct** en el código, vea [obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md).

Cada método presenta una interfaz de usuario de compra estándar para el usuario y se finaliza asincrónicamente una vez completada la transacción. El método devuelve un objeto que indica si la transacción se realizó correctamente.

> [!NOTE]
> El espacio de nombres **Windows. Services. Store** se presentó en la versión 1607 de Windows 10 y solo se puede usar en proyectos que tengan como destino la **edición de aniversario de windows 10 (10,0; Compilación 14393)** o una versión posterior en Visual Studio. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulte [este artículo](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Requisitos previos

Este ejemplo tiene los siguientes requisitos previos:
* Un proyecto de Visual Studio para una aplicación Plataforma universal de Windows (UWP) destinada a **Windows 10 aniversario Edition (10,0; Compilación 14393)** o una versión posterior.
* Ha [creado un envío de aplicación](../publish/app-submissions.md) en el centro de Partners y esta aplicación se publica en la tienda. Opcionalmente, puede configurar la aplicación para que no se pueda detectar en el almacén mientras la prueba. Para obtener más información, consulte nuestra [Guía de pruebas](in-app-purchases-and-trials.md#testing).
* Si desea habilitar las compras desde la aplicación para un complemento de la aplicación, también debe [crear el complemento en el centro de Partners](../publish/add-on-submissions.md).

El código de este ejemplo supone que:
* El código se ejecuta en el contexto de una [página](/uwp/api/windows.ui.xaml.controls.page) que contiene un elemento [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) denominado ```workingProgressRing``` y un elemento [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una de usuario único que se ejecuta solamente en el contexto del usuario que la inició. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si tiene una aplicación de escritorio que usa el [puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop), es posible que necesite agregar código adicional que no se muestra en este ejemplo para configurar el objeto [StoreContext](/uwp/api/windows.services.store.storecontext) . Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Ejemplo de código

En este ejemplo se muestra cómo usar el método [RequestPurchaseAsync](/uwp/api/windows.services.store.storecontext.requestpurchaseasync) de la clase [StoreContext](/uwp/api/windows.services.store.storecontext) para comprar una aplicación o complemento con un [Id. de la Tienda](in-app-purchases-and-trials.md#store-ids) conocido. Para obtener una aplicación de ejemplo completa, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/PurchaseAddOnPage.xaml.cs" id="PurchaseAddOn":::

## <a name="video"></a>Vídeo

Vea el vídeo siguiente para obtener información general sobre cómo implementar compras desde la aplicación en la aplicación.
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
