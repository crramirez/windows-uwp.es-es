---
author: Xansky
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: Aprende a usar el espacio de nombres Windows.Services.Store para trabajar con complementos consumibles.
title: Habilitar compras de complementos consumibles
keywords: windows 10, uwp, consumibles, consumable, complementos, add-ons, compras desde la aplicación, in-app purchases, IAP, Windows.Services.Store
ms.author: mhopkins
ms.date: 05/09/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e4687833b55f1456d298b552f5cce897f8b4eaa1
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7572934"
---
# <a name="enable-consumable-add-on-purchases"></a>Habilitar compras de complementos consumibles

En este artículo se demuestra cómo usar los métodos de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) en el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para administrar el suministro de complementos consumibles del usuario en las aplicaciones para UWP. Usar complementos consumibles para los elementos que se puedan comprar, usar y volver a comprar. Esto es especialmente útil para cosas como monedas de juego (oro, monedas, etc.) que se pueden comprar y usar para comprar bonificaciones concretas.

> [!NOTE]
> El espacio de nombres **Windows.Services.Store** se introdujo en Windows 10, versión 1607 y solo se puede usar en proyectos destinados a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulta [este artículo](enable-consumable-in-app-product-purchases.md).

## <a name="overview-of-consumable-add-ons"></a>Introducción a los complementos consumibles

Las aplicaciones pueden ofrecer dos tipos de complementos consumibles que difieren en la forma en que se administran los suministros:

* **Consumible administrado por el desarrollador**. Para este tipo de consumible, eres responsable de realizar un seguimiento del saldo del usuario de los elementos que representa el complemento, así como de notificar la compra del complemento cuando se complete en la Tienda y una vez que el usuario haya consumido todos los elementos. El usuario no puede comprar el complemento otra vez hasta que la aplicación notifique que la compra del complemento anterior se ha completado.

  Por ejemplo, si el complemento representa 100 monedas en un juego y el usuario consume 10 monedas, la aplicación o el servicio debe mantener el nuevo saldo restante de 90 monedas para el usuario. Cuando el usuario haya consumido las 100 monedas, la aplicación debe notificar el complemento como completado y, a continuación, el usuario puede volver a comprar el complemento de 100 monedas.

* **Consumible administrado por la Tienda**. Para este tipo de consumible, la Tienda realiza un seguimiento del saldo del usuario de los elementos que representa el complemento. Cuando el usuario consume algún elemento, debes notificar dichos elementos como completados en la Tienda, tras lo cual la Tienda actualiza el saldo del usuario. Los usuarios pueden adquirir el complemento tantas veces como quieran (no necesitan consumir los elementos antes). La aplicación puede consultar en la Store el saldo actual del usuario en cualquier momento.

  Por ejemplo, si el complemento representa una cantidad inicial de 100 monedas en un juego y el usuario consume 50 monedas, la aplicación notifica a la Store que se han completado 50 unidades del complemento y la Store actualiza el saldo restante. Si el usuario vuelve a comprar el complemento para adquirir 100 monedas más, ahora tendrá 150 monedas totales.
    > [!NOTE]
    > Los consumibles administrados por Microsoft Store se introdujeron en Windows 10, versión 1607.

Para ofrecer un complemento consumible a un usuario, sigue este proceso general:

1. Permitir que los usuarios [compren el complemento](enable-in-app-purchases-of-apps-and-add-ons.md) desde la aplicación.
3. Cuando el usuario consume el complemento (por ejemplo, al gastar monedas en un juego), [notifica el complemento como completado](enable-consumable-add-on-purchases.md#report_fulfilled).

En cualquier momento, también puedes [obtener el saldo restante](enable-consumable-add-on-purchases.md#get_balance) para un consumible administrado por la tienda.

## <a name="prerequisites"></a>Requisitos previos

Estos ejemplos cumplen los siguientes requisitos:
* Un proyecto de Visual Studio para una aplicación de la Plataforma universal de Windows (UWP) destinado a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o un versión posterior.
* Ha [creado un envío de aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) en el centro de partners y esta aplicación está publicada en la tienda. De manera opcional, puedes configurar la aplicación para que no se pueda descubrir en Microsoft Store mientras la pruebas. Para obtener más información, consulta nuestra [guía para prueba](in-app-purchases-and-trials.md#testing).
* Has [creado un complemento consumible para la aplicación](../publish/add-on-submissions.md) de centro de partners.

El código de estos ejemplos supone que:
* El código se ejecuta en el contexto de una [página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contiene un elemento [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` y un elemento [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una aplicación de usuario único que se ejecuta solamente en el contexto del usuario que inició la aplicación. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

Para una aplicación de ejemplo completa, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> Si tienes una aplicación de escritorio que usa el [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop), puede que tengas que agregar código adicional que no se muestra en estos ejemplos para configurar el objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

<span id="report_fulfilled" />

## <a name="report-a-consumable-add-on-as-fulfilled"></a>Notificar un complemento consumible como completado

Después de que el usuario [compre el complemento](enable-in-app-purchases-of-apps-and-add-ons.md) desde la aplicación y de consumir el complemento, la aplicación debe notificar el complemento como completado llamando al método [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Debes pasar la siguiente información a este método:

* El [Id. de la tienda](in-app-purchases-and-trials.md#store-ids) del complemento que quieres notificar como completado.
* Las unidades del complemento que quieres notificar como completadas.
  * Para los consumibles administrados por el desarrollador, especifica 1 para el parámetro *cantidad*. Esto alerta a la Tienda de que el consumible se ha suministrado y el cliente puede, a continuación, volver a comprar el consumible. El usuario no puede volver a comprar el consumible hasta que la aplicación haya notificado a la Tienda que se suministró.
  * Para un consumible administrado por la Tienda, especifica el número real de unidades que se hayan consumido. La Tienda actualizará el saldo restante para el consumible.
* El identificador de seguimiento del suministro. Este es un GUID proporcionado por el desarrollador que identifica la transacción específica a la que la operación de suministro está asociada para realizar un seguimiento. Para obtener más información, consulta las observaciones de [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync).

Este ejemplo muestra cómo notificar un consumible administrado por la Store como completado.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/ConsumeAddOnPage.xaml.cs#ConsumeAddOn)]

<span id="get_balance" />

## <a name="get-the-remaining-balance-for-a-store-managed-consumable"></a>Obtener el saldo restante para un consumible administrado por la Tienda

En este ejemplo se muestra cómo usar el método [GetConsumableBalanceRemainingAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getconsumablebalanceremainingasync) de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) para obtener el saldo restante para un complemento consumible administrado por la Tienda.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/GetRemainingAddOnBalancePage.xaml.cs#GetRemainingAddOnBalance)]

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
