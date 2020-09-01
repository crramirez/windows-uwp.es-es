---
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: Aprende a usar el espacio de nombres Windows.Services.Store para obtener información de licencia para la aplicación actual y sus complementos.
title: Obtener información de licencia para tu aplicación y los complementos
ms.date: 12/04/2017
ms.topic: article
keywords: Windows 10, UWP, licencias, aplicaciones, complementos, compras desde la aplicación, IAPs, Windows. Services. Store
ms.localizationpriority: medium
ms.openlocfilehash: 2c83ecc7b5bfc8158d80469eab12710719fa0834
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164679"
---
# <a name="get-license-info-for-apps-and-add-ons"></a>Obtener información de licencia para aplicaciones y complementos

En este artículo se muestra cómo usar los métodos de la clase [StoreContext](/uwp/api/windows.services.store.storecontext) en el espacio de nombres [Windows. Services. Store](/uwp/api/windows.services.store) para obtener información de licencia de la aplicación actual y sus complementos. Por ejemplo, puedes usar esta información para determinar si las licencias de la aplicación o sus complementos están activas o si son licencias de prueba.

> [!NOTE]
> El espacio de nombres **Windows. Services. Store** se presentó en la versión 1607 de Windows 10 y solo se puede usar en proyectos que tengan como destino la **edición de aniversario de windows 10 (10,0; Compilación 14393)** o una versión posterior en Visual Studio. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulte [este artículo](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Requisitos previos

Este ejemplo tiene los siguientes requisitos previos:
* Un proyecto de Visual Studio para una aplicación Plataforma universal de Windows (UWP) destinada a **Windows 10 aniversario Edition (10,0; Compilación 14393)** o una versión posterior.
* Ha [creado un envío de aplicación](../publish/app-submissions.md) en el centro de Partners y esta aplicación se publica en la tienda. Opcionalmente, puede configurar la aplicación para que no se pueda detectar en el almacén mientras la prueba. Para obtener más información, consulte nuestra [Guía de pruebas](in-app-purchases-and-trials.md#testing).
* Si desea obtener información de licencia para un complemento de la aplicación, también debe [crear el complemento en el centro de Partners](../publish/add-on-submissions.md).

El código de este ejemplo supone que:
* El código se ejecuta en el contexto de una [página](/uwp/api/windows.ui.xaml.controls.page) que contiene un elemento [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) denominado ```workingProgressRing``` y un elemento [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una de usuario único que se ejecuta solamente en el contexto del usuario que la inició. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si tiene una aplicación de escritorio que usa el [puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop), es posible que necesite agregar código adicional que no se muestra en este ejemplo para configurar el objeto [StoreContext](/uwp/api/windows.services.store.storecontext) . Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Ejemplo de código

Para obtener información de licencia para la aplicación actual, usa el método [GetAppLicenseAsync](/uwp/api/windows.services.store.storecontext.getapplicenseasync). Se trata de un método asincrónico que devuelve un objeto [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense) que proporciona información de licencia de la aplicación, incluidas las propiedades que indican si el usuario tiene actualmente una licencia válida para usar la aplicación ([isActive](/uwp/api/windows.services.store.storeapplicense.isactive)) y si la licencia es para una versión de prueba ([IsTrial](/uwp/api/windows.services.store.storeapplicense.istrial)).

Para acceder a las licencias de complementos duraderos de la aplicación actual para la que el usuario tiene derecho a usar, use la propiedad [AddOnLicenses](/uwp/api/windows.services.store.storeapplicense.addonlicenses) del objeto [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense) . Esta propiedad devuelve una colección de objetos [StoreLicense](/uwp/api/windows.services.store.storelicense) que representan las licencias del complemento.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetLicenseInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs#GetLicenseInfo)]

Para obtener una aplicación de ejemplo completa, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)