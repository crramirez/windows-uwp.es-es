---
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: Aprende a usar el espacio de nombres Windows.Services.Store para trabajar con complementos consumibles.
title: Habilitar compras de complementos consumibles
keywords: Windows 10, UWP, consumibles, complementos, compras desde la aplicación, IAPs, Windows. Services. Store
ms.date: 05/09/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 01f1646c05b66d354a403e2621e3b032c22734d3
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363658"
---
# <a name="enable-consumable-add-on-purchases"></a>Habilitar compras de complementos consumibles

En este artículo se muestra cómo usar los métodos de la clase [StoreContext](/uwp/api/windows.services.store.storecontext) en el espacio de nombres [Windows. Services. Store](/uwp/api/windows.services.store) para administrar el cumplimiento del usuario de complementos consumibles en las aplicaciones de UWP. Usar complementos consumibles para los elementos que se puedan comprar, usar y volver a comprar. Esto es especialmente útil para cosas como monedas de juego (oro, monedas, etc.) que se pueden comprar y usar para comprar bonificaciones concretas.

> [!NOTE]
> El espacio de nombres **Windows. Services. Store** se presentó en la versión 1607 de Windows 10 y solo se puede usar en proyectos que tengan como destino la **edición de aniversario de windows 10 (10,0; Compilación 14393)** o una versión posterior en Visual Studio. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulte [este artículo](enable-consumable-in-app-product-purchases.md).

## <a name="overview-of-consumable-add-ons"></a>Introducción a los complementos consumibles

Las aplicaciones pueden ofrecer dos tipos de complementos consumibles que difieren en la forma en que se administran las entregas:

* **Consumible administrado por el desarrollador**. Para este tipo de consumible, eres responsable de realizar un seguimiento del saldo del usuario de los elementos que representa el complemento, así como de notificar la compra del complemento cuando se complete en la Tienda y una vez que el usuario haya consumido todos los elementos. El usuario no puede comprar el complemento otra vez hasta que la aplicación notifique que la compra del complemento anterior se ha completado.

  Por ejemplo, si el complemento representa 100 monedas en un juego y el usuario consume 10 monedas, la aplicación o el servicio debe mantener el nuevo saldo restante de 90 monedas para el usuario. Cuando el usuario haya consumido las 100 monedas, la aplicación debe notificar el complemento como completado y, a continuación, el usuario puede volver a comprar el complemento de 100 monedas.

* **Consumible administrado por la Tienda**. Para este tipo de consumible, la Tienda realiza un seguimiento del saldo del usuario de los elementos que representa el complemento. Cuando el usuario consume algún elemento, debes notificar dichos elementos como completados en la Tienda, tras lo cual la Tienda actualiza el saldo del usuario. El usuario puede adquirir el complemento tantas veces como desee (no es necesario usar los elementos primero). La aplicación puede consultar en la tienda el saldo actual del usuario en cualquier momento.

  Por ejemplo, si el complemento representa una cantidad inicial de 100 monedas en un juego y el usuario consume 50 monedas, la aplicación notifica a la tienda que se han cumplido las 50 unidades del complemento y el almacén actualiza el resto del saldo. Si el usuario vuelve a comprar el complemento para adquirir 100 más monedas, ahora tendrá 150 monedas en total.
    > [!NOTE]
    > Los consumibles administrados por almacenamiento se introdujeron en la versión 1607 de Windows 10.

Para ofrecer un complemento consumible a un usuario, sigue este proceso general:

1. Permitir que los usuarios [compren el complemento](enable-in-app-purchases-of-apps-and-add-ons.md) desde la aplicación.
3. Cuando el usuario consume el complemento (por ejemplo, al gastar monedas en un juego), [notifica el complemento como completado](enable-consumable-add-on-purchases.md#report_fulfilled).

En cualquier momento, también puedes [obtener el saldo restante](enable-consumable-add-on-purchases.md#get_balance) para un consumible administrado por la tienda.

## <a name="prerequisites"></a>Requisitos previos

Estos ejemplos cumplen los siguientes requisitos:
* Un proyecto de Visual Studio para una aplicación Plataforma universal de Windows (UWP) destinada a **Windows 10 aniversario Edition (10,0; Compilación 14393)** o una versión posterior.
* Ha [creado un envío de aplicación](../publish/app-submissions.md) en el centro de Partners y esta aplicación se publica en la tienda. Opcionalmente, puede configurar la aplicación para que no se pueda detectar en el almacén mientras la prueba. Para obtener más información, consulte nuestra [Guía de pruebas](in-app-purchases-and-trials.md#testing).
* Ha [creado un complemento consumible para la aplicación en el](../publish/add-on-submissions.md) centro de Partners.

El código de estos ejemplos supone que:
* El código se ejecuta en el contexto de una [página](/uwp/api/windows.ui.xaml.controls.page) que contiene un elemento [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) denominado ```workingProgressRing``` y un elemento [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una de usuario único que se ejecuta solamente en el contexto del usuario que la inició. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

Para obtener una aplicación de ejemplo completa, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> Si tiene una aplicación de escritorio que usa el [puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop), es posible que deba agregar código adicional que no se muestra en estos ejemplos para configurar el objeto [StoreContext](/uwp/api/windows.services.store.storecontext) . Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

<span id="report_fulfilled" />

## <a name="report-a-consumable-add-on-as-fulfilled"></a>Notificar un complemento consumible como completado

Después de que el usuario [compre el complemento](enable-in-app-purchases-of-apps-and-add-ons.md) desde la aplicación y de consumir el complemento, la aplicación debe notificar el complemento como completado llamando al método [ReportConsumableFulfillmentAsync](/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) de la clase [StoreContext](/uwp/api/windows.services.store.storecontext). Debes pasar la siguiente información a este método:

* El [Id. de la tienda](in-app-purchases-and-trials.md#store-ids) del complemento que quieres notificar como completado.
* Las unidades del complemento que quieres notificar como completadas.
  * Para los consumibles administrados por el desarrollador, especifica 1 para el parámetro *cantidad*. Esto alerta a la Tienda de que el consumible se ha suministrado y el cliente puede, a continuación, volver a comprar el consumible. El usuario no puede volver a comprar el consumible hasta que la aplicación haya notificado a la Tienda que se suministró.
  * Para un consumible administrado por la Tienda, especifica el número real de unidades que se hayan consumido. La Tienda actualizará el saldo restante para el consumible.
* El identificador de seguimiento del suministro. Este es un GUID proporcionado por el desarrollador que identifica la transacción específica a la que la operación de suministro está asociada para realizar un seguimiento. Para obtener más información, consulta las observaciones de [ReportConsumableFulfillmentAsync](/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync).

Este ejemplo muestra cómo notificar un consumible administrado por la Tienda como completado.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/ConsumeAddOnPage.xaml.cs" id="ConsumeAddOn":::

<span id="get_balance" />

## <a name="get-the-remaining-balance-for-a-store-managed-consumable"></a>Obtener el saldo restante para un consumible administrado por la Tienda

En este ejemplo se muestra cómo usar el método [GetConsumableBalanceRemainingAsync](/uwp/api/windows.services.store.storecontext.getconsumablebalanceremainingasync) de la clase [StoreContext](/uwp/api/windows.services.store.storecontext) para obtener el saldo restante para un complemento consumible administrado por la Tienda.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/GetRemainingAddOnBalancePage.xaml.cs" id="GetRemainingAddOnBalance":::

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
