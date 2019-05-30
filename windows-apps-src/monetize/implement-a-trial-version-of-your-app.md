---
ms.assetid: 571697B7-6064-4C50-9A68-1374F2C3F931
description: Aprende a usar el espacio de nombres Windows.Services.Store para implementar una versión de prueba de tu aplicación.
title: Implementar una versión de prueba de la aplicación
keywords: Windows 10, uwp, prueba, compras desde la aplicación, Windows.Services.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 47affd7e54bcaad21949cb56916de27dd3bf260b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371064"
---
# <a name="implement-a-trial-version-of-your-app"></a>Implementar una versión de prueba de la aplicación

Si se [configuración de la aplicación como una evaluación gratuita en el centro de partners](../publish/set-app-pricing-and-availability.md#free-trial) para que los clientes pueden usar la aplicación de forma gratuita durante un período de prueba, puede conseguir que sus clientes para actualizar a la versión completa de la aplicación mediante la exclusión o limitación de algunas características durante el período de prueba. Determina las funciones que quieres restringir antes de empezar a codificar y luego asegúrate de que tu aplicación solo permita que funcionen una vez comprada la licencia completa. Asimismo, puedes habilitar características tales como banners o marcas de agua que solo se muestren durante la prueba antes de que el cliente compre la aplicación.

En este artículo se muestra cómo usar miembros de la clase [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) en el espacio de nombres [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) para determinar si el usuario tiene una licencia de prueba para tu aplicación y recibir una notificación si cambia el estado de la licencia mientras se ejecuta la aplicación. 

> [!NOTE]
> El espacio de nombres **Windows.Services.Store** se introdujo en Windows 10, versión 1607 y solo se puede usar en proyectos destinados a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio. Si la aplicación está destinada a una versión anterior de Windows 10, debes usar el espacio de nombres [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) en lugar del espacio de nombres **Windows.Services.Store**. Para obtener más información, consulta [este artículo](exclude-or-limit-features-in-a-trial-version-of-your-app.md).

## <a name="guidelines-for-implementing-a-trial-version"></a>Directrices para implementar una versión de prueba

El estado de licencia actual de la aplicación se almacena en forma de propiedades de la clase [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense). Por lo general, tendrás que incluir las funciones que dependen del estado de la licencia en un bloque condicional, tal y como describimos en el siguiente paso. Cuando uses estas características, asegúrate de implementarlas de manera que funcionen en todos los estados de la licencia.

Además, debes decidir cómo quieres tratar los cambios en la licencia de la aplicación cuando se está ejecutando la aplicación. La aplicación de prueba puede tener todas las características, pero también puede incorporar anuncios publicitarios que la versión de pago no tiene. O bien, la aplicación de prueba puede tener determinadas características deshabilitadas o mostrar mensajes regulares que indiquen al usuario que compre la aplicación.

Piensa en el tipo de aplicación que estás haciendo y cuál sería la mejor estrategia de prueba o expiración. Para la versión de prueba de un juego, una buena estrategia es limitar la cantidad de contenido del juego que puede utilizar el usuario. Para la versión de prueba de una utilidad, te recomendamos que establezcas una fecha de caducidad o que limites las características que podría usar un posible comprador.

Para la mayoría de las aplicaciones que no sean juegos, definir una fecha de expiración funciona bien, porque los usuarios pueden familiarizarse con la aplicación completa. Estos son algunos escenarios de expiración habituales y las opciones para tratarlos.

-   **Licencia de evaluación expira mientras se está ejecutando la aplicación**

    Si la prueba expira mientras se está ejecutando la aplicación, esta puede:

    -   No hacer nada.
    -   Mostrar un mensaje al cliente.
    -   Cierre.
    -   Pedir al cliente que compre la aplicación.

    El procedimiento recomendado es mostrar un mensaje pidiendo que se compre la aplicación y, si el cliente la compra, permitirle usarla con todas las características habilitadas. Si el usuario decide no comprar la aplicación, ciérrela o recuérdeles que compren la aplicación a intervalos regulares.

-   **Licencia de evaluación expira antes de inicia la aplicación**

    Si la prueba expira antes de que el usuario inicie la aplicación, esta no se iniciará. En lugar de ello, aparecerá un cuadro de diálogo y los usuarios tendrán la posibilidad de comprar tu aplicación en la Tienda Windows.

-   **El cliente compra la aplicación mientras se está ejecutando**

    Si el cliente compra la aplicación mientras la está ejecutando, estas son algunas de las acciones que la aplicación puede realizar.

    -   No hacer nada y dejar que el cliente siga en el modo de prueba hasta que reinicie la aplicación.
    -   Darle las gracias por la compra o mostrar un mensaje.
    -   Habilitar de forma silenciosa las características que están disponibles con una licencia completa (o deshabilitar los avisos exclusivos de la prueba).

Explica cómo se comportará la aplicación durante el período de prueba gratuito y después de él, para que el comportamiento de la aplicación no sorprenda a los clientes. Para obtener más información sobre cómo describir tu aplicación, consulta [Crear descripciones de la aplicación](https://docs.microsoft.com/windows/uwp/publish/create-app-descriptions).

## <a name="prerequisites"></a>Requisitos previos

Este ejemplo tiene los siguientes requisitos previos:
* Un proyecto de Visual Studio para una aplicación de la Plataforma universal de Windows (UWP) destinado a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o un versión posterior.
* Ha creado una aplicación en el centro de partners está configurado como un [gratuita](https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) sin límite de tiempo y esta aplicación se publica en el Store. De manera opcional, puedes configurar la aplicación para que no se pueda descubrir en la Tienda mientras la pruebas. Para obtener más información, consulta nuestra [guía para prueba](in-app-purchases-and-trials.md#testing).

El código de este ejemplo supone que:
* El código se ejecuta en el contexto de una [página](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) que contiene un elemento [ProgressRing](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring) denominado ```workingProgressRing``` y un elemento [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock) denominado ```textBlock```. Estos objetos se usan para indicar que se está produciendo una operación asincrónica y para mostrar mensajes de salida, respectivamente.
* El archivo de código tiene una instrucción **using** para el espacio de nombres **Windows.Services.Store**.
* La aplicación es una de usuario único que se ejecuta solamente en el contexto del usuario que la inició. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si tienes una aplicación de escritorio que usa el [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop), puede que tengas que agregar código adicional que no se muestra en este ejemplo para configurar el objeto [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext). Para obtener más información, consulta [Uso de la clase StoreContext en una aplicación de escritorio que usa el Puente de escritorio](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Ejemplo de código

Si tu aplicación se está inicializando, obtén el objeto [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense) para esta y controla el evento [OfflineLicensesChanged](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.offlinelicenseschanged) para recibir notificaciones si cambia la licencia mientras la aplicación se está ejecutando. Por ejemplo, la licencia de la aplicación puede cambiar si expira el período de prueba o si el cliente compra la aplicación a través de una Tienda. Si la licencia cambia, obtén la nueva licencia y habilita o deshabilita una característica de tu aplicación en consecuencia.

En este punto, si el usuario compró la aplicación, se recomienda proporcionar información al usuario sobre los cambios de estado de licencia. Es posible que necesites pedirle al usuario que reinicie la aplicación, si así la has codificado. Esta transición debe ser lo más sencilla y fácil posible.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ImplementTrial](./code/InAppPurchasesAndLicenses_RS1/cs/ImplementTrialPage.xaml.cs#ImplementTrial)]

Para obtener una aplicación de ejemplo completa, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener la información de producto para las aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener la información de licencia para las aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar la adquisición de la aplicación de las aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar complemento consumibles compras](enable-consumable-add-on-purchases.md)
* [Ejemplo de Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
