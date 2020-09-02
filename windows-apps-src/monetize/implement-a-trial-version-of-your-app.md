---
ms.assetid: 571697B7-6064-4C50-9A68-1374F2C3F931
description: Aprende a usar el espacio de nombres Windows.Services.Store para implementar una versión de prueba de tu aplicación.
title: Implementar una versión de prueba de la aplicación
keywords: Windows 10, UWP, evaluación, compras desde la aplicación, Windows. Services. Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c07be51de312ab5a8483cb67537e809d3a55e14a
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363648"
---
# <a name="implement-a-trial-version-of-your-app"></a>Implementar una versión de prueba de la aplicación

Si [configura la aplicación como una evaluación gratuita en el centro de Partners](../publish/set-app-pricing-and-availability.md#free-trial) para que los clientes puedan usar su aplicación de forma gratuita durante un período de prueba, puede persuadir a los clientes para que actualicen a la versión completa de la aplicación excluyendo o limitando algunas características durante el período de prueba. Determina las funciones que quieres restringir antes de empezar a codificar y luego asegúrate de que tu aplicación solo permita que funcionen una vez comprada la licencia completa. Asimismo, puedes habilitar características tales como banners o marcas de agua que solo se muestren durante la prueba antes de que el cliente compre la aplicación.

En este artículo se muestra cómo usar los miembros de la clase [StoreContext](/uwp/api/windows.services.store.storecontext) en el espacio de nombres [Windows. Services. Store](/uwp/api/windows.services.store) para determinar si el usuario tiene una licencia de prueba para la aplicación y recibir una notificación si el estado de la licencia cambia mientras se ejecuta la aplicación. 

> [!NOTE]
> El espacio de nombres **Windows. Services. Store** se presentó en la versión 1607 de Windows 10 y solo se puede usar en proyectos que tengan como destino la **edición de aniversario de windows 10 (10,0; Compilación 14393)** o una versión posterior en Visual Studio. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulte [este artículo](exclude-or-limit-features-in-a-trial-version-of-your-app.md).

## <a name="guidelines-for-implementing-a-trial-version"></a>Directrices para implementar una versión de prueba

El estado de licencia actual de la aplicación se almacena en forma de propiedades de la clase [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense). Por lo general, tendrás que incluir las funciones que dependen del estado de la licencia en un bloque condicional, tal y como describimos en el siguiente paso. Cuando uses estas características, asegúrate de implementarlas de manera que funcionen en todos los estados de la licencia.

Además, debes decidir cómo quieres tratar los cambios en la licencia de la aplicación cuando se está ejecutando la aplicación. La aplicación de prueba puede tener todas las características, pero también puede incorporar anuncios publicitarios que la versión de pago no tiene. O bien, la aplicación de prueba puede tener determinadas características deshabilitadas o mostrar mensajes regulares que indiquen al usuario que compre la aplicación.

Piensa en el tipo de aplicación que estás haciendo y cuál sería la mejor estrategia de prueba o expiración. Para la versión de prueba de un juego, una buena estrategia es limitar la cantidad de contenido del juego que puede utilizar el usuario. Para la versión de prueba de una utilidad, te recomendamos que establezcas una fecha de caducidad o que limites las características que podría usar un posible comprador.

Para la mayoría de las aplicaciones que no sean juegos, definir una fecha de expiración funciona bien, porque los usuarios pueden familiarizarse con la aplicación completa. Estos son algunos escenarios de expiración habituales y las opciones para tratarlos.

-   **La licencia de prueba expira cuando se está ejecutando la aplicación**

    Si la prueba expira mientras se está ejecutando la aplicación, esta puede:

    -   No haga nada.
    -   Mostrar un mensaje al cliente.
    -   Casi.
    -   Pedir al cliente que compre la aplicación.

    El procedimiento recomendado es mostrar un mensaje pidiendo que se compre la aplicación y, si el cliente la compra, permitirle usarla con todas las características habilitadas. Si el usuario decide no comprar la aplicación, ciérrela o recuérdeles que compren la aplicación a intervalos regulares.

-   **La licencia de prueba expira antes de que se inicie la aplicación**

    Si la prueba expira antes de que el usuario inicie la aplicación, esta no se iniciará. En lugar de ello, aparecerá un cuadro de diálogo y los usuarios tendrán la posibilidad de comprar tu aplicación en la Tienda Windows.

-   **El cliente compra la aplicación mientras se está ejecutando**

    Si el cliente compra la aplicación mientras la está ejecutando, estas son algunas de las acciones que la aplicación puede realizar.

    -   No hacer nada y dejar que el cliente siga en el modo de prueba hasta que reinicie la aplicación.
    -   Darle las gracias por la compra o mostrar un mensaje.
    -   Habilitar de forma silenciosa las características que están disponibles con una licencia completa (o deshabilitar los avisos exclusivos de la prueba).

Explica cómo se comportará la aplicación durante el período de prueba gratuito y después de él, para que el comportamiento de la aplicación no sorprenda a los clientes. Para obtener más información sobre cómo describir tu aplicación, consulta [Crear descripciones de la aplicación](../publish/create-app-store-listings.md).

## <a name="prerequisites"></a>Requisitos previos

Este ejemplo tiene los siguientes requisitos previos:
* Un proyecto de Visual Studio para una aplicación Plataforma universal de Windows (UWP) destinada a **Windows 10 aniversario Edition (10,0; Compilación 14393)** o una versión posterior.
* Ha creado una aplicación en el centro de partners que está configurada como una [evaluación gratuita](../publish/set-app-pricing-and-availability.md) sin límite de tiempo y esta aplicación se publica en la tienda. Opcionalmente, puede configurar la aplicación para que no se pueda detectar en el almacén mientras la prueba. Para obtener más información, consulte nuestra [Guía de pruebas](in-app-purchases-and-trials.md#testing).

El código de este ejemplo supone que:
* El código se ejecuta en el contexto de una [página](/uwp/api/windows.ui.xaml.controls.page) que contiene un elemento [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) denominado ```workingProgressRing``` y un elemento [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una de usuario único que se ejecuta solamente en el contexto del usuario que la inició. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si tiene una aplicación de escritorio que usa el [puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop), es posible que necesite agregar código adicional que no se muestra en este ejemplo para configurar el objeto [StoreContext](/uwp/api/windows.services.store.storecontext) . Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Ejemplo de código

Si tu aplicación se está inicializando, obtén el objeto [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense) para esta y controla el evento [OfflineLicensesChanged](/uwp/api/windows.services.store.storecontext.offlinelicenseschanged) para recibir notificaciones si cambia la licencia mientras la aplicación se está ejecutando. Por ejemplo, la licencia de la aplicación puede cambiar si expira el período de prueba o si el cliente compra la aplicación a través de una Tienda. Si la licencia cambia, obtén la nueva licencia y habilita o deshabilita una característica de tu aplicación en consecuencia.

En este punto, si el usuario compró la aplicación, se recomienda proporcionar información al usuario sobre los cambios de estado de licencia. Es posible que necesites pedirle al usuario que reinicie la aplicación, si así la has codificado. Esta transición debe ser lo más sencilla y fácil posible.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/ImplementTrialPage.xaml.cs" id="ImplementTrial":::

Para obtener una aplicación de ejemplo completa, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
