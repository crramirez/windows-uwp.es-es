---
author: mcleanbyron
ms.assetid: 571697B7-6064-4C50-9A68-1374F2C3F931
description: "Aprende a usar el espacio de nombres Windows.Services.Store para implementar una versión de prueba de tu aplicación."
title: "Implementar una versión de prueba de la aplicación"
keywords: "muestra de código de prueba gratuita"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 22f355c23f4cc87932563e9885f390e9a5ac4130

---

# Implementar una versión de prueba de la aplicación

Si permites que los clientes puedan usar la aplicación gratis durante un período de prueba, puedes animarles a actualizar a la versión completa de tu aplicación excluyendo o limitando algunas características durante el período de prueba. Determina las funciones que quieres restringir antes de empezar a codificar y luego asegúrate de que tu aplicación solo permita que funcionen una vez comprada la licencia completa. Asimismo, puedes habilitar características tales como banners o marcas de agua, para que solo se muestren durante la prueba, antes de que el cliente compre la aplicación.

Las aplicaciones destinadas a Windows 10, versión 1607 o posterior, pueden usar miembros de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) en el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para determinar si el usuario tiene una licencia de prueba para tu aplicación y recibir una notificación si cambia el estado de la licencia mientras se ejecuta la aplicación.

>**Nota**&nbsp;&nbsp;Este artículo es aplicable a las aplicaciones diseñadas para Windows 10, versión 1607 o posterior. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulta [Compras desde la aplicación y pruebas con el espacio de nombres Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## Directrices para implementar una versión de prueba

El estado de licencia actual de la aplicación se almacena en forma de propiedades de la clase [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx). Por lo general, tendrás que incluir las funciones que dependen del estado de la licencia en un bloque condicional, tal y como describimos en el siguiente paso. Cuando uses estas características, asegúrate de implementarlas de manera que funcionen en todos los estados de la licencia.

Además, debes decidir cómo quieres tratar los cambios en la licencia de la aplicación cuando se está ejecutando la aplicación. La aplicación de prueba puede tener todas las características, pero también puede incorporar anuncios publicitarios que la versión de pago no tiene. O bien, la aplicación de prueba puede tener determinadas características deshabilitadas o mostrar mensajes regulares que indiquen al usuario que compre la aplicación.

Piensa en el tipo de aplicación que estás haciendo y cuál sería la mejor estrategia de prueba o expiración. Para la versión de prueba de un juego, una buena estrategia es limitar la cantidad de contenido del juego que puede utilizar el usuario. Para la versión de prueba de una utilidad, te recomendamos que establezcas una fecha de caducidad o que limites las características que podría usar un posible comprador.

Para la mayoría de las aplicaciones que no sean juegos, definir una fecha de expiración funciona bien, porque los usuarios pueden familiarizarse con la aplicación completa. Estos son algunos escenarios de expiración habituales y las opciones para tratarlos.

-   **La licencia de prueba expira cuando se está ejecutando la aplicación**

    Si la prueba expira mientras se está ejecutando la aplicación, esta puede:

    -   No hacer nada.
    -   Mostrar un mensaje al cliente.
    -   Cerrarse.
    -   Pedir al cliente que compre la aplicación.

    El procedimiento recomendado es mostrar un mensaje pidiendo que se compre la aplicación y, si el cliente la compra, permitirle usarla con todas las características habilitadas. Si el usuario decide no comprar la aplicación, ciérrela o recuérdeles que compren la aplicación a intervalos regulares.

-   **La licencia de prueba expira antes de que se inicie la aplicación**

    Si la prueba expira antes de que el usuario inicie la aplicación, esta no se iniciará. En lugar de ello, aparecerá un cuadro de diálogo y los usuarios tendrán la posibilidad de comprar tu aplicación en la Tienda Windows.

-   **El cliente compra la aplicación mientras se está ejecutando**

    Si el cliente compra la aplicación mientras la está ejecutando, estas son algunas de las acciones que la aplicación puede realizar.

    -   No hacer nada y dejar que el cliente siga en el modo de prueba hasta que reinicie la aplicación.
    -   Darle las gracias por la compra o mostrar un mensaje.
    -   Habilitar de forma silenciosa las características que están disponibles con una licencia completa (o deshabilitar los avisos exclusivos de la prueba).

Explica cómo se comportará la aplicación durante el período de prueba gratuito y después de él, para que el comportamiento de la aplicación no sorprenda a los clientes. Para obtener más información sobre cómo describir tu aplicación, consulta [Crear descripciones de la aplicación](https://msdn.microsoft.com/library/windows/apps/mt148529).

## Requisitos previos

Este ejemplo tiene los siguientes requisitos previos:
* Un proyecto de Visual Studio de una aplicación para la Plataforma universal de Windows (UWP) destinado a Windows 10, versión 1607 o posterior.
* Has creado una aplicación en el panel del Centro de desarrollo de Windows que está configurada como una [prueba gratuita](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) sin límite de tiempo y esta aplicación está publicada y disponible en la Tienda. Esta puede ser una aplicación que quieres publicar para los clientes o puede ser una aplicación básica que cumpla con los requisitos mínimos del [Kit para la certificación de aplicaciones en Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) que se usa solo con fines de prueba. Para obtener más información, consulta la [guía para prueba](in-app-purchases-and-trials.md#testing).

El código de este ejemplo supone que:
* El código se ejecuta en el contexto de una [página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contiene un elemento [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` y un elemento [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una aplicación de usuario único que se ejecuta solamente en el contexto del usuario que inició la aplicación. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

## Ejemplo de código

Si tu aplicación se está inicializando, obtén el objeto [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) para esta y controla el evento [OfflineLicensesChanged](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.offlinelicenseschanged.aspx) para recibir notificaciones si cambia la licencia mientras la aplicación se está ejecutando. Por ejemplo, la licencia de la aplicación puede cambiar si expira el período de prueba o si el cliente compra la aplicación a través de una Tienda. Si la licencia cambia, obtén la nueva licencia y habilita o deshabilita una característica de tu aplicación en consecuencia.

En este punto, si el usuario compró la aplicación, se recomienda proporcionar información al usuario sobre los cambios de estado de licencia. Es posible que necesites pedirle al usuario que reinicie la aplicación, si así la has codificado. Esta transición debe ser lo más sencilla y fácil posible.


```csharp
private StoreContext context = null;
private StoreAppLicense appLicense = null;

// Call this while your app is initializing.
private async void InitializeLicense()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    workingProgressRing.IsActive = true;
    appLicense = await context.GetAppLicenseAsync();
    workingProgressRing.IsActive = false;

    // Register for the licenced changed event.
    context.OfflineLicensesChanged += context_OfflineLicensesChanged;
}

private async void context_OfflineLicensesChanged(StoreContext sender, object args)
{
    // Reload the license.
    workingProgressRing.IsActive = true;
    appLicense = await context.GetAppLicenseAsync();
    workingProgressRing.IsActive = false;

    if (appLicense.IsActive)
    {
        if (appLicense.IsTrial)
        {
            textBlock.Text = $"This is the trial version. Expiration date: {appLicense.ExpirationDate}";

            // Show the features that are available during trial only.
        }
        else
        {
            // Show the features that are available only with a full license.
        }
    }
}
```

Para una aplicación de muestra completa, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Aug16_HO5-->


