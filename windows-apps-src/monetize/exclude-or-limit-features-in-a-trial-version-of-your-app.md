---
Description: Si permites que los clientes puedan usar la aplicación gratis durante un período de prueba, puedes animarles a actualizar a la versión completa de la aplicación excluyendo o limitando algunas características durante el período de prueba.
title: Excluir o limitar las características de una versión de prueba
ms.assetid: 1B62318F-9EF5-432A-8593-F3E095CA7056
keywords: Windows 10, UWP, versión de prueba, compra desde la aplicación, IAP, Windows. ApplicationModel. Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a0ecc6a6d219262513508114cee226d72d91e386
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362748"
---
# <a name="exclude-or-limit-features-in-a-trial-version"></a>Excluir o limitar las características de una versión de prueba

Si permites que los clientes puedan usar la aplicación gratis durante un período de prueba, puedes animarles a actualizar a la versión completa de la aplicación excluyendo o limitando algunas características durante el período de prueba. Determina las funciones que quieres restringir antes de empezar a codificar y luego asegúrate de que tu aplicación solo permita que funcionen una vez comprada la licencia completa. Asimismo, puedes habilitar características tales como banners o marcas de agua que solo se muestren durante la prueba antes de que el cliente compre la aplicación.

> [!IMPORTANT]
> En este artículo se muestra cómo usar los miembros del espacio de nombres [Windows. ApplicationModel. Store](/uwp/api/windows.applicationmodel.store) para implementar la funcionalidad de prueba. Este espacio de nombres ya no se actualiza con nuevas características y se recomienda usar el espacio de nombres [Windows. Services. Store](/uwp/api/windows.services.store) en su lugar. El espacio de nombres **Windows. Services. Store** es compatible con los tipos de complementos más recientes, como las suscripciones y los complementos consumibles administrados por el almacén, y está diseñado para ser compatible con los tipos de productos y características futuros admitidos por el centro de Partners y el almacén. El espacio de nombres **Windows. Services. Store** se presentó en la versión 1607 de Windows 10 y solo se puede usar en proyectos que tengan como destino la **edición de aniversario de windows 10 (10,0; Compilación 14393)** o una versión posterior en Visual Studio. Para obtener más información sobre cómo implementar la funcionalidad de prueba mediante el espacio de nombres **Windows. Services. Store** , consulte [este artículo](implement-a-trial-version-of-your-app.md).

## <a name="prerequisites"></a>Requisitos previos

Una aplicación de Windows en la que se puedan agregar características que los clientes pueden comprar.

## <a name="step-1-pick-the-features-you-want-to-enable-or-disable-during-the-trial-period"></a>Paso 1: Elige las características que deseas habilitar o deshabilitar durante el período de prueba

El estado de licencia de la aplicación se almacena como propiedad de la clase [LicenseInformation](/uwp/api/Windows.ApplicationModel.Store.LicenseInformation). Por lo general, tendrás que incluir las funciones que dependen del estado de la licencia en un bloque condicional, tal y como describimos en el siguiente paso. Cuando uses estas características, asegúrate de implementarlas de manera que funcionen en todos los estados de la licencia.

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

Si quieres detectar el cambio de licencia y realizar alguna acción en la aplicación, debes agregar un controlador de eventos como se describe en el siguiente paso.

## <a name="step-2-initialize-the-license-info"></a>Paso 2: Inicializar la información de licencia

Cuando se esté inicializando la aplicación, obtén el objeto [LicenseInformation](/uwp/api/Windows.ApplicationModel.Store.LicenseInformation) para la aplicación, tal y como se muestra en este ejemplo. Se supone que **licenseInformation** es una variable global o un campo del tipo **LicenseInformation**.

Por ahora, obtendrás información de licencia simulada mediante [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) en lugar de [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp). Antes de enviar la versión de lanzamiento de la aplicación a la **Tienda**, debes reemplazar todas las referencias a **CurrentAppSimulator** que aparezcan en el código por **CurrentApp**.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="InitializeLicenseTest":::

A continuación, agrega un controlador de eventos para recibir notificaciones cuando cambie la licencia mientras se esté ejecutando la aplicación. La licencia de la aplicación puede cambiar si expira el período de prueba o si el cliente compra la aplicación a través de una Tienda, por ejemplo.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="InitializeLicenseTestWithEvent":::

## <a name="step-3-code-the-features-in-conditional-blocks"></a>Paso 3: Codificar las características en un bloque condicional

Cuando se genere el evento de cambio de la licencia, la aplicación debe llamar a la API de licencia para determinar si el estado de prueba se ha modificado. En el código de este paso se muestra cómo estructurar el controlador para este evento. En este punto, si el usuario compró la aplicación, se recomienda proporcionar información al usuario sobre los cambios de estado de licencia. Es posible que necesites pedirle al usuario que reinicie la aplicación, si así la has codificado. Esta transición debe ser lo más sencilla y fácil posible.

En este ejemplo se muestra cómo evaluar el estado de la licencia de una aplicación para que puedas habilitar o deshabilitar una característica de la aplicación adecuadamente.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="ReloadLicense":::

## <a name="step-4-get-an-apps-trial-expiration-date"></a>Paso 4: Obtener la fecha de caducidad de la prueba de una aplicación

Incluye el código para determinar la fecha de caducidad de la prueba de la aplicación.

El código de este ejemplo define una función para obtener la fecha de expiración de la licencia de prueba de la aplicación. Si la licencia sigue siendo válida, muestra la fecha de caducidad con el número de días que quedan para que caduque la prueba.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="DisplayTrialVersionExpirationTime":::

## <a name="step-5-test-the-features-using-simulated-calls-to-the-license-api"></a>Paso 5: Probar las características con llamadas simuladas a la API de licencia

Ahora, prueba la aplicación con datos simulados. **CurrentAppSimulator** obtiene información de licencia específica de la prueba de un archivo XML denominado WindowsStoreProxy.xml, que se encuentra en% userprofile% \\ AppData archivos \\ locales \\ nombre de \\ &lt; paquete &gt; \\ LocalState \\ Microsoft \\ Windows Store \\ ApiData. Puedes editar WindowsStoreProxy.xml para cambiar las fechas de caducidad simuladas para la aplicación y para sus características. Prueba todas las configuraciones de licencia y caducidad posibles para asegurarte de que todo funciona correctamente. Para obtener más información, consulta [Uso del archivo WindowsStoreProxy.xml con CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).

Si esta ruta de acceso y este archivo no existen, deberás crearlos durante la instalación o durante el tiempo de ejecución. Si intentas acceder a la propiedad [CurrentAppSimulator.LicenseInformation](/uwp/api/windows.applicationmodel.store.currentappsimulator.licenseinformation) sin el archivo WindowsStoreProxy.xml en esa ubicación específica, obtendrás un mensaje de error.

## <a name="step-6-replace-the-simulated-license-api-methods-with-the-actual-api"></a>Paso 6: Reemplazar los métodos API de licencia simulados por las API reales.

Después de probar tu aplicación con el servidor de licencias simulado, y antes de enviar la aplicación a una Tienda para su certificación, reemplaza **CurrentAppSimulator** por **CurrentApp**, tal y como se muestra en la siguiente muestra de código.

> [!IMPORTANT]
> La aplicación debe usar el objeto **CurrentApp** al enviar la aplicación a una tienda o se producirá un error de certificación.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="InitializeLicenseRetailWithEvent":::

## <a name="step-7-describe-how-the-free-trial-works-to-your-customers"></a>Paso 7: Describir cómo funciona la prueba gratuita a tus clientes

Explica cómo se comportará la aplicación durante el período de prueba gratuito y después de él, para que el comportamiento de la aplicación no sorprenda a los clientes.

Para obtener más información sobre cómo describir tu aplicación, consulta [Crear descripciones de la aplicación](../publish/create-app-store-listings.md).

## <a name="related-topics"></a>Temas relacionados

* [Muestra de la Tienda (muestra pruebas y compras desde la aplicación)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Establecer los precios y la disponibilidad de las aplicaciones](../publish/set-app-pricing-and-availability.md)
* [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp)
* [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator)
 

 
