---
author: mcleanbyron
Description: "Independientemente de que la aplicación sea gratuita o no, puedes vender contenido, otras aplicaciones o nuevas funcionalidades de la aplicación (como el desbloqueo del nivel siguiente de un juego) desde la misma aplicación. Aquí te mostramos cómo habilitar estos productos en la aplicación."
title: "Habilitar compras de productos desde la aplicación"
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: "uwp, complementos, add-ons, compras desde la aplicación, in-app purchases, IAP, Windows.ApplicationModel.Store"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b7cd3f5d2c566958aaf83b8f633418ce444a2eaa
ms.lasthandoff: 02/07/2017

---

# <a name="enable-in-app-product-purchases"></a>Habilitar compras de productos desde la aplicación

>**Nota**&nbsp;&nbsp;En este artículo se muestra cómo usar miembros del espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx). Si la aplicación está orientada a Windows 10, versión 1607, o posterior, te recomendamos que uses miembros del espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para administrar los complementos (también conocidos como productos dentro de la aplicación o IAP), en lugar del espacio de nombres **Windows.ApplicationModel.Store**. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md).

Independientemente de que la aplicación sea gratuita o no, puedes vender contenido, otras aplicaciones o nuevas funcionalidades de la aplicación (como el desbloqueo del nivel siguiente de un juego) desde la misma aplicación. Aquí te mostramos cómo habilitar estos productos en la aplicación.

> **Nota**&nbsp;&nbsp;Los productos desde la aplicación no pueden ofrecerse en una versión de prueba de una aplicación. Los clientes que usan una versión de prueba de la aplicación solamente pueden comprar un producto desde la aplicación si compran una versión completa de la misma.

## <a name="prerequisites"></a>Requisitos previos

-   Una aplicación de Windows en la que se puedan agregar características que los clientes pueden comprar.
-   Cuando codificas y pruebas nuevos productos desde la aplicación por primera vez, debes usar el objeto [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) en lugar del objeto [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765). De esta manera, puedes comprobar la lógica de la licencia con llamadas simuladas al servidor de licencias, en lugar de llamar al servidor activo. Para ello, debes personalizar el archivo llamado WindowsStoreProxy.xml en %perfil_usuario%\\AppData\\local\\packages\\&lt;nombre_paquete&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. El simulador de Microsoft Visual Studio crea este archivo la primera vez que se ejecuta la aplicación, aunque también puedes cargar un archivo personalizado en el tiempo de ejecución. Para obtener más información, consulta [Uso del archivo WindowsStoreProxy.xml con CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Este tema también hace referencia a los ejemplos de código que se proporcionan en la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Esta muestra es ideal para conseguir experiencia práctica con las diferentes opciones de monetización que se proporcionan para las aplicaciones para la Plataforma universal de Windows (UWP).

## <a name="step-1-initialize-the-license-info-for-your-app"></a>Paso 1: Inicializa la información de licencia para la aplicación

Cuando se esté iniciando la aplicación, obtén el objeto [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157) de la aplicación mediante la inicialización de la clase [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) o [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) para habilitar las compras de un producto desde la aplicación.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#InitializeLicenseTest)]

## <a name="step-2-add-the-in-app-offers-to-your-app"></a>Paso 2: Agrega las ofertas desde la aplicación

Para cada función que quieras tener disponible a través de un producto desde la aplicación, crea una oferta y agrégala a la aplicación.

> **Importante:**&nbsp;&nbsp;Debes agregar a la aplicación todos los productos desde la aplicación que quieras presentar a tus clientes antes de enviarla a la Tienda. Si deseas agregar nuevos productos en la aplicación más adelante, tendrás que actualizar la aplicación y enviar una nueva versión.

1.  **Crea un token de oferta desde la aplicación**

    En la aplicación, puedes identificar cada producto desde la aplicación con un token. Este token es una cadena que debes definir y usar en la aplicación y en la Tienda para identificar un producto concreto desde la aplicación. Debes darle un nombre único y significativo (en la aplicación) para que puedas identificar rápidamente la función correcta que representa mientras la estás codificando. Estos son algunos ejemplos de nombres:

    -   "SpaceMissionLevel4"

    -   "ContosoCloudSave"

    -   "RainbowThemePack"

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

En el panel del Centro de desarrollo, define el identificador de producto, el tipo, el precio y otras propiedades del producto desde la aplicación. Asegúrate de que la configuración es idéntica a la configuración que estableciste en WindowsStoreProxy.xml durante las pruebas. Para más información, consulta [IAP submissions](https://msdn.microsoft.com/library/windows/apps/mt148551).

## <a name="remarks"></a>Comentarios

Si te interesa que los clientes cuenten con varios productos consumibles desde la aplicación (elementos que se puedan comprar, usar y después volver a comprar si se desea), consulta el tema [Habilitar compras de productos consumibles desde la aplicación](enable-consumable-in-app-product-purchases.md).

Si necesitas usar recibos para comprobar que el usuario ha realizado una compra desde la aplicación, asegúrate de revisar [Usar recibos para comprobar compras de productos](use-receipts-to-verify-product-purchases.md).

## <a name="related-topics"></a>Temas relacionados


* [Habilitar compras de productos consumibles desde la aplicación](enable-consumable-in-app-product-purchases.md)
* [Administrar un catálogo extenso de productos desde la aplicación](manage-a-large-catalog-of-in-app-products.md)
* [Usar recibos para comprobar compras de productos](use-receipts-to-verify-product-purchases.md)
* [Muestra de la Tienda (muestra pruebas y compras desde la aplicación)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)

