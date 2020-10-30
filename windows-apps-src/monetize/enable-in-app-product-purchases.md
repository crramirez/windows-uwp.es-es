---
description: Independientemente de que la aplicación sea gratuita o no, puedes vender contenido, otras aplicaciones o nuevas funcionalidades de la aplicación (como el desbloqueo del nivel siguiente de un juego) desde la misma aplicación. Aquí te mostramos cómo habilitar estos productos en la aplicación.
title: Habilitar compras de productos desde la aplicación
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: UWP, complementos, compras desde la aplicación, IAPs, Windows. ApplicationModel. Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b24a48034585411af5edfb0950fc4f96b189519f
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033478"
---
# <a name="enable-in-app-product-purchases"></a>Habilitar compras de productos desde la aplicación

Independientemente de que la aplicación sea gratuita o no, puedes vender contenido, otras aplicaciones o nuevas funcionalidades de la aplicación (como el desbloqueo del nivel siguiente de un juego) desde la misma aplicación. Aquí te mostramos cómo habilitar estos productos en la aplicación.

> [!IMPORTANT]
> En este artículo se muestra cómo usar los miembros del espacio de nombres [Windows. ApplicationModel. Store](/uwp/api/windows.applicationmodel.store) para habilitar las compras de productos en la aplicación. Este espacio de nombres ya no se actualiza con nuevas características y se recomienda usar el espacio de nombres [Windows. Services. Store](/uwp/api/windows.services.store) en su lugar. El espacio de nombres **Windows. Services. Store** es compatible con los tipos de complementos más recientes, como las suscripciones y los complementos consumibles administrados por el almacén, y está diseñado para ser compatible con los tipos de productos y características futuros admitidos por el centro de Partners y el almacén. El espacio de nombres **Windows. Services. Store** se presentó en la versión 1607 de Windows 10 y solo se puede usar en proyectos que tengan como destino la **edición de aniversario de windows 10 (10,0; Compilación 14393)** o una versión posterior en Visual Studio. Para obtener más información sobre cómo habilitar las compras de productos en la aplicación con el espacio de nombres **Windows. Services. Store** , consulte [este artículo](enable-in-app-purchases-of-apps-and-add-ons.md).

> [!NOTE]
> No se pueden ofrecer productos en la aplicación desde una versión de prueba de una aplicación. Los clientes que usan una versión de prueba de la aplicación solamente pueden comprar un producto desde la aplicación si compran una versión completa de la misma.

## <a name="prerequisites"></a>Requisitos previos

-   Una aplicación de Windows en la que se puedan agregar características que los clientes pueden comprar.
-   Cuando codificas y pruebas nuevos productos desde la aplicación por primera vez, debes usar el objeto [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) en lugar del objeto [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp). De esta manera, puedes comprobar la lógica de la licencia con llamadas simuladas al servidor de licencias, en lugar de llamar al servidor activo. Para ello, debe personalizar el archivo denominado WindowsStoreProxy.xml en% userprofile% \\ AppData local Packages \\ \\ \\ &lt; name &gt; \\ LocalState \\ Microsoft \\ Windows Store \\ ApiData. El simulador de Microsoft Visual Studio crea este archivo la primera vez que se ejecuta la aplicación, aunque también puedes cargar un archivo personalizado en el tiempo de ejecución. Para obtener más información, consulta [Uso del archivo WindowsStoreProxy.xml con CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Este tema también hace referencia a los ejemplos de código que se proporcionan en la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Esta muestra es ideal para conseguir experiencia práctica con las diferentes opciones de monetización que se proporcionan para las aplicaciones para la Plataforma universal de Windows (UWP).

## <a name="step-1-initialize-the-license-info-for-your-app"></a>Paso 1: Inicializa la información de licencia para la aplicación

Cuando se esté iniciando la aplicación, obtén el objeto [LicenseInformation](/uwp/api/Windows.ApplicationModel.Store.LicenseInformation) de la aplicación mediante la inicialización de la clase [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) o [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) para habilitar las compras de un producto desde la aplicación.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs" id="InitializeLicenseTest":::

## <a name="step-2-add-the-in-app-offers-to-your-app"></a>Paso 2: Agrega las ofertas desde la aplicación

Para cada función que quieras tener disponible a través de un producto desde la aplicación, crea una oferta y agrégala a la aplicación.

> [!IMPORTANT]
> Debe agregar todos los productos de la aplicación que desee presentar a los clientes a la aplicación antes de enviarlos a la tienda. Si deseas agregar nuevos productos en la aplicación más adelante, tendrás que actualizar la aplicación y enviar una nueva versión.

1.  **Crea un token de oferta desde la aplicación**

    En la aplicación, puedes identificar cada producto desde la aplicación con un token. Este token es una cadena que debes definir y usar en la aplicación y en la Tienda para identificar un producto concreto desde la aplicación. Debes darle un nombre único y significativo (en la aplicación) para que puedas identificar rápidamente la función correcta que representa mientras la estás codificando. Estos son algunos ejemplos de nombres:

    * "SpaceMissionLevel4"
    * "ContosoCloudSave"
    * "RainbowThemePack"

  > [!NOTE]
  > El token de la oferta de la aplicación que use en el código debe coincidir con el valor de [ID. de producto](../publish/set-your-add-on-product-id.md#product-id) que especifique al [definir el complemento correspondiente para la aplicación en el centro de Partners](../publish/add-on-submissions.md).

2.  **Codifica la característica en un bloque condicional**

    Debes situar el código de cada función asociada con un producto desde la aplicación en un bloque condicional que compruebe si el cliente tiene licencia para usar esa función.

    En este ejemplo se muestra cómo puedes codificar una función de producto llamada **featureName** en un bloqueo condicional específico de una licencia. La cadena, **featureName** , es el token que identifica de forma única este producto dentro de la aplicación y también se utiliza para identificarlo en el almacén.

    > [!div class="tabbedCodeSnippets"]
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs" id="CodeFeature":::

3.  **Agrega la interfaz de usuario de compra para esta característica**

    La aplicación también debe proporcionar una manera de que los clientes puedan comprar el producto o la función que ofrece el producto desde la aplicación. No pueden obtenerlos a través de la Tienda de la misma manera que obtuvieron la aplicación completa.

    A continuación te mostramos cómo comprobar si el cliente ya tiene un producto desde la aplicación y, si no la tiene, cómo mostrar el cuadro de diálogo de compra para que puedan comprarla. Reemplaza el comentario "mostrar el cuadro de diálogo de compra" por el código personalizado del cuadro de diálogo de compra (como, por ejemplo, una página con un alegre botón que diga: “¡Comprar esta aplicación!” ).

    > [!div class="tabbedCodeSnippets"]
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs" id="BuyFeature":::

## <a name="step-3-change-the-test-code-to-the-final-calls"></a>Paso 3: Cambia el código de prueba para las llamadas finales

Este es un paso fácil: cambia cada referencia a [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) por [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) en el código de la aplicación. Ya no es necesario que proporciones el archivo WindowsStoreProxy.xml, por lo tanto, elimínalo de la ruta de acceso de la aplicación (aunque es posible que quieras guardarlo como referencia cuando configures la oferta desde la aplicación en el próximo paso).

## <a name="step-4-configure-the-in-app-product-offer-in-the-store"></a>Paso 4: Configura la oferta del producto desde la aplicación en la Tienda

En el centro de Partners, vaya a la aplicación y [cree un complemento](../publish/add-on-submissions.md) que coincida con su oferta de producto en la aplicación. Defina el identificador de producto, el tipo, el precio y otras propiedades del complemento. Asegúrate de que la configuración es idéntica a la configuración que estableciste en WindowsStoreProxy.xml durante las pruebas.

  > [!NOTE]
  > El token de la oferta de la aplicación que use en el código debe coincidir con el valor de ID. de [producto](../publish/set-your-add-on-product-id.md#product-id) que especifique para el complemento correspondiente en el centro de Partners.

## <a name="remarks"></a>Comentarios

Si te interesa que los clientes cuenten con varios productos consumibles desde la aplicación (elementos que se puedan comprar, usar y después volver a comprar si se desea), consulta el tema [Habilitar compras de productos consumibles desde la aplicación](enable-consumable-in-app-product-purchases.md).

Si necesitas usar recibos para comprobar que el usuario ha realizado una compra desde la aplicación, asegúrate de revisar [Usar recibos para comprobar compras de productos](use-receipts-to-verify-product-purchases.md).

## <a name="related-topics"></a>Temas relacionados


* [Habilitar compras consumibles de productos en la aplicación](enable-consumable-in-app-product-purchases.md)
* [Administrar un catálogo extenso de productos desde la aplicación](manage-a-large-catalog-of-in-app-products.md)
* [Usar recibos para comprobar la compra de productos](use-receipts-to-verify-product-purchases.md)
* [Muestra de la Tienda (muestra pruebas y compras desde la aplicación)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
