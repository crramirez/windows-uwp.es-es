---
author: mcleanbyron
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: "Aprende a usar el espacio de nombres Windows.Services.Store para obtener información de licencia para la aplicación actual y sus complementos."
title: "Obtener información de licencia para tu aplicación y los complementos"
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: 0482cc192eeff4d3633898b6fa677805c635c6e1

---

# <a name="get-license-info-for-apps-and-add-ons"></a>Obtener información de licencia para aplicaciones y complementos

Las aplicaciones diseñadas para Windows 10, versión 1607 o posterior, pueden usar métodos de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) en el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para obtener información de licencia para la aplicación actual o sus complementos (también conocidos como productos desde la aplicación o IAP). Por ejemplo, puedes usar esta información para determinar si las licencias de la aplicación o sus complementos están activas o si son licencias de prueba.

>**Nota**&nbsp;&nbsp;Este artículo es aplicable a las aplicaciones diseñadas para Windows 10, versión 1607 o posterior. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulta [Compras desde la aplicación y pruebas con el espacio de nombres Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

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

Para obtener información de licencia para la aplicación actual, usa el método [GetAppLicenseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getapplicenseasync.aspx). Este es un método asincrónico que devuelve un objeto [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) que proporciona la información de licencia para la aplicación, incluidas las propiedades que indican si el usuario tiene una licencia para usar la aplicación ([IsActive](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.isactive.aspx)) y si la licencia se destina a una versión de prueba ([IsTrial](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.istrial.aspx)).

Para recuperar las licencias de complemento de la aplicación, usa la propiedad [AddOnLicenses](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.addonlicenses.aspx) del objeto [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx). Esta propiedad devuelve objetos de una colección [StoreLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.aspx) que representan las licencias de complemento de la aplicación. Para determinar si el usuario tiene una licencia para usar un complemento, utiliza la propiedad [IsActive](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.isactive.aspx).

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetLicenseInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs#GetLicenseInfo)]

Para obtener una aplicación de ejemplo completa, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Dec16_HO1-->


