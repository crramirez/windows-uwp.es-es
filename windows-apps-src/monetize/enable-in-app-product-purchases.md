---
author: Xansky
Description: Whether your app is free or not, you can sell content, other apps, or new app functionality (such as unlocking the next level of a game) from right within the app. Here we show you how to enable these products in your app.
title: Habilitar compras de productos desde la aplicación
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: uwp, UWP, add-ons, complementos, in-app purchases, compras desde la aplicación, IAPs, IAP, Windows.ApplicationModel.Store, Windows.ApplicationModel.Store
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: dcdedda655011cf700df2548140b312f4b0f817d
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "5471959"
---
# <a name="enable-in-app-product-purchases"></a>Habilitar compras de productos desde la aplicación

Independientemente de que la aplicación sea gratuita o no, puedes vender contenido, otras aplicaciones o nuevas funcionalidades de la aplicación (como el desbloqueo del nivel siguiente de un juego) desde la misma aplicación. Aquí te mostramos cómo habilitar estos productos en la aplicación.

> [!IMPORTANT]
> En este artículo se muestra cómo usar los miembros del espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) para habilitar las compras de productos desde la aplicación. Este espacio de nombres ya no se actualiza con las nuevas características por lo que te recomendamos que uses el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) en su lugar. El espacio de nombres **Windows.Services.Store** admite los tipos de complemento más recientes, como las suscripciones y los complementos de consumibles administrados por Microsoft Store, y está diseñado para ser compatible con futuros tipos de productos y características compatibles con el Centro de desarrollo de Windows y Microsoft Store. El espacio de nombres **Windows.Services.Store** se introdujo en Windows 10, versión 1607 y solo se puede usar en proyectos destinados a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio. Para obtener más información acerca de cómo habilitar las compras de productos en la aplicación con el espacio de nombres **Windows.Services.Store** , consulta [este artículo](enable-in-app-purchases-of-apps-and-add-ons.md).

> [!NOTE]
> Los productos desde la aplicación no pueden ofrecerse durante la versión de prueba de una aplicación. Los clientes que usan una versión de prueba de la aplicación solamente pueden comprar un producto desde la aplicación si compran una versión completa de la misma.

## <a name="prerequisites"></a>Requisitos previos

-   Una aplicación de Windows en la que se puedan agregar características que los clientes pueden comprar.
-   Cuando codificas y pruebas nuevos productos desde la aplicación por primera vez, debes usar el objeto [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) en lugar del objeto [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765). De esta manera, puedes comprobar la lógica de la licencia con llamadas simuladas al servidor de licencias, en lugar de llamar al servidor activo. Para ello, debes personalizar el archivo llamado WindowsStoreProxy.xml en %perfil_usuario%\\AppData\\local\\packages\\&lt;nombre_paquete&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. El simulador de Microsoft Visual Studio crea este archivo la primera vez que se ejecuta la aplicación, aunque también puedes cargar un archivo personalizado en el tiempo de ejecución. Para obtener más información, consulta [Uso del archivo WindowsStoreProxy.xml con CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Este tema también hace referencia a los ejemplos de código que se proporcionan en la [muestra de la Store](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Esta muestra es ideal para conseguir experiencia práctica con las diferentes opciones de monetización que se proporcionan para las aplicaciones para la Plataforma universal de Windows (UWP).

## <a name="step-1-initialize-the-license-info-for-your-app"></a>Paso 1: Inicializa la información de licencia para la aplicación

Cuando se esté iniciando la aplicación, obtén el objeto [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157) de la aplicación mediante la inicialización de la clase [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) o [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) para habilitar las compras de un producto desde la aplicación.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#InitializeLicenseTest)]

## <a name="step-2-add-the-in-app-offers-to-your-app"></a>Paso 2: Agrega las ofertas desde la aplicación

Para cada función que quieras tener disponible a través de un producto desde la aplicación, crea una oferta y agrégala a la aplicación.

> [!IMPORTANT]
> Debes agregar a la aplicación todos los productos desde la aplicación que quieres presentar a tus clientes antes de enviarla Tienda. Si deseas agregar nuevos productos en la aplicación más adelante, tendrás que actualizar la aplicación y enviar una nueva versión.

1.  **Crea un token de oferta desde la aplicación**

    En la aplicación, puedes identificar cada producto desde la aplicación con un token. Este token es una cadena que debes definir y usar en la aplicación y en la Tienda para identificar un producto concreto desde la aplicación. Debes darle un nombre único y significativo (en la aplicación) para que puedas identificar rápidamente la función correcta que representa mientras la estás codificando. Estos son algunos ejemplos de nombres:

    * "SpaceMissionLevel4"
    * "ContosoCloudSave"
    * "RainbowThemePack"

  > [!NOTE]
  > El token de oferta desde la aplicación que usas en tu código debe coincidir con el valor del [id. del producto](../publish/set-your-add-on-product-id.md#product-id) que especificas al [definir el complemento correspondiente de la aplicación en el panel del Centro de desarrollo](../publish/add-on-submissions.md).

2.  **Codifica la característica en un bloque condicional**

    Debes situar el código de cada función asociada con un producto desde la aplicación en un bloque condicional que compruebe si el cliente tiene licencia para usar esa función.

    En este ejemplo se muestra cómo puedes codificar una función de producto llamada **featureName** en un bloqueo condicional específico de una licencia. La cadena **featureName** es el token que identifica este producto de manera única en la aplicación. También se usa para identificarlo en la Tienda.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#CodeFeature)]

3.  **Agrega la interfaz de usuario de compra para esta característica**

    La aplicación también debe proporcionar una manera de que los clientes puedan comprar el producto o la función que ofrece el producto desde la aplicación. No pueden obtenerlos a través de la Tienda de la misma manera que obtuvieron la aplicación completa.

    A continuación te mostramos cómo comprobar si el cliente ya tiene un producto desde la aplicación y, si no la tiene, cómo mostrar el cuadro de diálogo de compra para que puedan comprarla. Reemplaza el comentario "mostrar el cuadro de diálogo de compra" por el código personalizado del cuadro de diálogo de compra (como, por ejemplo, una página con un alegre botón que diga: “¡Comprar esta aplicación!” ).

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#BuyFeature)]

## <a name="step-3-change-the-test-code-to-the-final-calls"></a>Paso 3: Cambia el código de prueba para las llamadas finales

Este es un paso fácil: cambia todas las referencias a [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) por [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) en el código de la aplicación. Ya no es necesario que proporciones el archivo WindowsStoreProxy.xml, por lo tanto, elimínalo de la ruta de acceso de la aplicación (aunque es posible que quieras guardarlo como referencia cuando configures la oferta desde la aplicación en el próximo paso).

## <a name="step-4-configure-the-in-app-product-offer-in-the-store"></a>Paso 4: Configura la oferta del producto desde la aplicación en la Tienda

En el panel del Centro de desarrollo, ve a la aplicación y [crea un complemento](../publish/add-on-submissions.md) que coincida con la oferta del producto desde la aplicación. Define el id. del producto, el tipo, el precio y otras propiedades para el complemento. Asegúrate de que la configuración es idéntica a la configuración que estableciste en WindowsStoreProxy.xml durante las pruebas.

  > [!NOTE]
  > El token de oferta desde la aplicación que usas en tu código debe coincidir con el valor del [id. del producto](../publish/set-your-add-on-product-id.md#product-id) que especificas para el complemento correspondiente en el panel.

## <a name="remarks"></a>Observaciones

Si te interesa que los clientes cuenten con varios productos consumibles desde la aplicación (elementos que se puedan comprar, usar y después volver a comprar si se desea), consulta el tema [Habilitar compras de productos consumibles desde la aplicación](enable-consumable-in-app-product-purchases.md).

Si necesitas usar recibos para comprobar que el usuario ha realizado una compra desde la aplicación, asegúrate de revisar [Usar recibos para comprobar compras de productos](use-receipts-to-verify-product-purchases.md).

## <a name="related-topics"></a>Temas relacionados


* [Habilitar compras de productos consumibles desde la aplicación](enable-consumable-in-app-product-purchases.md)
* [Administrar un catálogo extenso de productos desde la aplicación](manage-a-large-catalog-of-in-app-products.md)
* [Usar recibos para comprobar compras de productos](use-receipts-to-verify-product-purchases.md)
* [Muestra de la Tienda (muestra pruebas y compras desde la aplicación)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
