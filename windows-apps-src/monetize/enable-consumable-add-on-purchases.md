---
author: mcleanbyron
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: Aprende a usar el espacio de nombres Windows.Services.Store para trabajar con complementos consumibles.
title: Habilitar compras de complementos consumibles
keywords: "muestra de código de oferta desde la aplicación"
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: 12191a946ec080c8e386191363617a9c437671c5

---

# <a name="enable-consumable-add-on-purchases"></a>Habilitar compras de complementos consumibles

Las aplicaciones diseñadas para Windows 10, versión 1607 o posterior pueden usar métodos de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) en el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para administrar el suministro de complementos consumibles del usuario en las aplicaciones para UWP (los complementos se conocen también como productos desde la aplicación o IAP). Usar complementos consumibles para los elementos que se puedan comprar, usar y volver a comprar. Esto es especialmente útil para cosas como monedas de juego (oro, monedas, etc.) que se pueden comprar y usar para comprar bonificaciones concretas.

>**Nota**&nbsp;&nbsp;Este artículo es aplicable a las aplicaciones diseñadas para Windows 10, versión 1607 o posterior. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulta [Compras desde la aplicación y pruebas con el espacio de nombres Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="overview-of-consumable-add-ons"></a>Introducción a los complementos consumibles

Las aplicaciones destinadas a Windows 10, versión 1607 o posterior pueden ofrecer dos tipos de complementos consumibles que difieren en la forma en que se administran los suministros:

* **Consumible administrado por el desarrollador**. Para este tipo de consumible, eres responsable de realizar un seguimiento del saldo del usuario de los elementos que representa el complemento, así como de notificar la compra del complemento cuando se complete en la Tienda y una vez que el usuario haya consumido todos los elementos. El usuario no puede comprar el complemento otra vez hasta que la aplicación notifique que la compra del complemento anterior se ha completado.

  Por ejemplo, si el complemento representa 100 monedas en un juego y el usuario consume 10 monedas, la aplicación o el servicio debe mantener el nuevo saldo restante de 90 monedas para el usuario. Cuando el usuario haya consumido las 100 monedas, la aplicación debe notificar el complemento como completado y, a continuación, el usuario puede volver a comprar el complemento de 100 monedas.

* **Consumible administrado por la Tienda**. Para este tipo de consumible, la Tienda realiza un seguimiento del saldo del usuario de los elementos que representa el complemento. Cuando el usuario consume algún elemento, debes notificar dichos elementos como completados en la Tienda, tras lo cual la Tienda actualiza el saldo del usuario. La aplicación puede consultar el saldo actual del usuario en cualquier momento. Cuando el usuario haya consumido todos los elementos, el usuario puede volver a comprar el complemento.

  Por ejemplo, si el complemento representa una cantidad inicial de 100 monedas en un juego y el usuario consume 10 monedas, la aplicación notifica a la Tienda que se han completado 10 unidades del complemento, y la Tienda actualiza el saldo restante. Cuando el usuario ha consumido las 100 monedas, puede volver a comprar el complemento de 100 monedas.

  >**Nota**&nbsp;&nbsp;Los consumibles administrados por la Tienda están disponibles a partir de Windows 10, versión 1607. La capacidad para crear un consumible administrado por la Tienda en el panel del Centro de desarrollo de Windows estará disponible próximamente.

Para ofrecer un complemento consumible a un usuario, sigue este proceso general:

1. Permitir que los usuarios [compren el complemento](enable-in-app-purchases-of-apps-and-add-ons.md) desde la aplicación.
3. Cuando el usuario consume el complemento (por ejemplo, al gastar monedas en un juego), [notifica el complemento como completado](enable-consumable-add-on-purchases.md#report_fulfilled).

En cualquier momento, también puedes [obtener el saldo restante](enable-consumable-add-on-purchases.md#get_balance) para un consumible administrado por la tienda.

## <a name="prerequisites"></a>Requisitos previos

Estos ejemplos cumplen los siguientes requisitos:
* Un proyecto de Visual Studio para una aplicación de la Plataforma universal de Windows (UWP) destinado a Windows 10, versión 1607 o posterior.
* Has creado una aplicación en el panel del Centro de desarrollo de Windows con los complementos consumibles (también conocidos como productos desde la aplicación o IAP) y esta aplicación está publicada y disponible en la Tienda. Esta puede ser una aplicación que quieres publicar para los clientes o puede ser una aplicación básica que cumpla con los requisitos mínimos del [Kit para la certificación de aplicaciones en Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) que se usa solo con fines de prueba. Para obtener más información, consulta la [guía para prueba](in-app-purchases-and-trials.md#testing).

El código de estos ejemplos supone que:
* El código se ejecuta en el contexto de una [página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contiene un elemento [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` y un elemento [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una aplicación de usuario único que se ejecuta solamente en el contexto del usuario que inició la aplicación. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

Para obtener una aplicación de ejemplo completa, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

>**Nota**&nbsp;&nbsp;Si tienes una aplicación de escritorio que usa el [Puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop), tienes que agregar código adicional que no se muestra en estos ejemplos para configurar el objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

<span id="report_fulfilled" />
## <a name="report-a-consumable-add-on-as-fulfilled"></a>Notificar un complemento consumible como completado

Después de que el usuario [compre el complemento](enable-in-app-purchases-of-apps-and-add-ons.md) desde la aplicación y de consumir el complemento, la aplicación debe notificar el complemento como completado llamando al método [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.reportconsumablefulfillmentasync.aspx) de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Debes pasar la siguiente información a este método:

* El [Id. de la tienda](in-app-purchases-and-trials.md#store_ids) del complemento que quieres notificar como completado.
* Las unidades del complemento que quieres notificar como completadas.
  * Para los consumibles administrados por el desarrollador, especifica 1 para el parámetro *cantidad*. Esto alerta a la Tienda de que el consumible se ha suministrado y el cliente puede, a continuación, volver a comprar el consumible. El usuario no puede volver a comprar el consumible hasta que la aplicación haya notificado a la Tienda que se suministró.
  * Para un consumible administrado por la Tienda, especifica el número real de unidades que se hayan consumido. La Tienda actualizará el saldo restante para el consumible.
* El identificador de seguimiento del suministro. Este es un GUID proporcionado por el desarrollador que identifica la transacción específica a la que la operación de suministro está asociada para realizar un seguimiento. Para obtener más información, consulta las observaciones de [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.reportconsumablefulfillmentasync.aspx).

Este ejemplo muestra cómo notificar un consumible administrado por la Tienda como completado.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/ConsumeAddOnPage.xaml.cs#ConsumeAddOn)]

<span id="get_balance" />
## <a name="get-the-remaining-balance-for-a-store-managed-consumable"></a>Obtener el saldo restante para un consumible administrado por la Tienda

En este ejemplo se muestra cómo usar el método [GetConsumableBalanceRemainingAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getconsumablebalanceremainingasync.aspx) de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) para obtener el saldo restante para un complemento consumible administrado por la Tienda.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/GetRemainingAddOnBalancePage.xaml.cs#GetRemainingAddOnBalance)]

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Dec16_HO1-->


