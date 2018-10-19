---
author: Xansky
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: Aprende a usar el espacio de nombres Windows.Services.Store para obtener información de licencia para la aplicación actual y sus complementos.
title: Obtener información sobre licencias de la aplicación y los complementos
ms.author: mhopkins
ms.date: 12/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, licencias, licenses, aplicaciones, apps, complementos, add-ons, compras desde la aplicación, in-app purchases, IAP, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: 83889dce2959a3d373081808864a6b7913fb142b
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "4945592"
---
# <a name="get-license-info-for-apps-and-add-ons"></a>Obtener información sobre licencias de las aplicaciones y los complementos

En este artículo se demuestra cómo usar métodos de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) en el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para obtener información de licencia para la aplicación actual y sus complementos. Por ejemplo, puedes usar esta información para determinar si las licencias de la aplicación o sus complementos están activas o si son licencias de prueba.

> [!NOTE]
> El espacio de nombres **Windows.Services.Store** se introdujo en Windows 10, versión 1607 y solo se puede usar en proyectos destinados a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulta [este artículo](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Requisitos previos

Este ejemplo tiene los siguientes requisitos previos:
* Un proyecto de Visual Studio para una aplicación de la Plataforma universal de Windows (UWP) destinado a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o un versión posterior.
* Has [creado un envío de aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) en el panel del Centro de desarrollo de Windows, y esta aplicación está publicada y disponible en la Store. De manera opcional, puedes configurar la aplicación para que no se pueda descubrir en la Store mientras la pruebas. Para obtener más información, consulta nuestra [guía para prueba](in-app-purchases-and-trials.md#testing).
* Si quieres obtener información de licencia de un complemento de la aplicación, también debes [crear el complemento en el panel del Centro de desarrollo](../publish/add-on-submissions.md).

El código de este ejemplo supone que:
* El código se ejecuta en el contexto de una [página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contiene un elemento [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` y un elemento [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una aplicación de usuario único que se ejecuta solamente en el contexto del usuario que inició la aplicación. Para más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si tienes una aplicación de escritorio que usa el [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop), puede que tengas que agregar código adicional que no se muestra en este ejemplo para configurar el objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Ejemplo de código

Para obtener información de licencia para la aplicación actual, usa el método [GetAppLicenseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getapplicenseasync). Este es un método asincrónico que devuelve un objeto [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) que proporciona la información de licencia para la aplicación, incluidas las propiedades que indican si el usuario actualmente tiene una licencia válida para usar la aplicación ([IsActive](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.isactive)) y si la licencia se destina a una versión de prueba ([IsTrial](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.istrial)).

Para acceder a las licencias de complementos duraderos de la aplicación actual para que el usuario tiene derecho de uso, usa la propiedad [AddOnLicenses](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.addonlicenses) del objeto [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx). Esta propiedad devuelve objetos de una colección de [StoreLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.aspx) que representan las licencias de complemento.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetLicenseInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs#GetLicenseInfo)]

Para una aplicación de ejemplo completa, consulta la [muestra de Microsoft Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Muestra de la Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
