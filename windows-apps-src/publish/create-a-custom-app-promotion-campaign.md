---
description: Además de crear una campaña de AD para la aplicación que se ejecutará en aplicaciones de Windows, puede promover su aplicación mediante otros canales.
title: Crear una campaña de promoción de la aplicación personalizada
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, personalizado, aplicación, promoción, campaña
ms.localizationpriority: medium
ms.openlocfilehash: 4015ee5372c2068d7a9ead389fe7eb07aa50a466
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171129"
---
# <a name="create-a-custom-app-promotion-campaign"></a>Crear una campaña de promoción de la aplicación personalizada

Además de crear una [campaña de AD para la aplicación](create-an-ad-campaign-for-your-app.md) que se ejecutará en aplicaciones de Windows, también puede promocionar su aplicación mediante otros canales. Por ejemplo, puedes promocionarla mediante proveedores de marketing para aplicaciones, o publicar vínculos a la aplicación en sitios de redes sociales. Estas actividades se denominan *campañas personalizadas*.

Si ejecuta campañas personalizadas para la aplicación, puede realizar un seguimiento del rendimiento relativo de cada campaña mediante la creación de una dirección URL diferente para cada campaña personalizada, donde cada dirección URL contiene un ID. de *campaña*diferente. Cuando un cliente que ejecuta Windows 10 hace clic en una dirección URL que contiene un identificador de campaña, Microsoft asocia el clic con la campaña personalizada correspondiente y hace que estos datos estén disponibles en el [centro de Partners](https://partner.microsoft.com/dashboard).

> [!IMPORTANT]
> Solo se realiza el seguimiento de estos datos para los clientes de Windows 10. Los clientes que usen otros sistemas operativos pueden seguir el vínculo a la descripción de la aplicación, pero no se incluirán los datos acerca de sus actividades.

Hay dos tipos principales de datos asociados a las campañas personalizadas: *vistas de página* para la lista de tiendas de la aplicación y *conversiones*. Una conversión es una adquisición de aplicación que se obtiene de un cliente que ve la página de descripción de la tienda de la aplicación desde una dirección URL que incluye un identificador de campaña personalizado. Para obtener más información sobre las conversiones, consulte [Descripción de cómo se califican las adquisiciones de aplicaciones como conversiones](#understanding-how-acquisitions-qualify-as-conversions) en este tema.

Puedes recuperar los datos de rendimiento de una campaña personalizada para tu aplicación de las siguientes maneras:

* Puede ver los datos acerca de las vistas de página y las conversiones de la aplicación o el complemento desde los gráficos de las vistas de la página de la **aplicación y** las conversiones por ID. de campaña y **total de conversiones de campaña** en el informe de [adquisiciones](acquisitions-report.md).
* Si la aplicación es una aplicación Plataforma universal de Windows (UWP), puede usar las API del Windows SDK para recuperar mediante programación el identificador de la campaña personalizada que dio como resultado una conversión.

## <a name="example-custom-campaign-scenario"></a>Ejemplo de escenario de campaña personalizada

Imagina a una desarrolladora de juegos que ha terminado su última creación y quiere promocionarla entre los usuarios de sus juegos ya publicados. Publica el anuncio del nuevo lanzamiento de juego en su página de Facebook, incluido un vínculo a la lista de tiendas del juego. Muchos de sus jugadores también siguen en Twitter, de modo que también Tweet un anuncio con el vínculo a la lista de tiendas del juego.

Para realizar un seguimiento del éxito de cada uno de estos canales de promoción, el desarrollador crea dos variantes de la dirección URL en la lista de tiendas del juego:

* La dirección URL que publicará en su página de Facebook incluye el identificador de la campaña personalizada. `my-facebook-campaign`

* La dirección URL que publicará en Twitter incluye el identificador de la campaña personalizada `my-twitter-campaign`

Al hacer clic en las direcciones URL de Facebook y Twitter seguidores, Microsoft realiza un seguimiento de cada clic y lo asocia a la campaña personalizada correspondiente. Si cumplen las condiciones, las posteriores adquisiciones del juego y de cualquier complemento se asociarán a las campañas personalizadas y se notificarán como conversiones.

<span id="conversions" />

## <a name="understanding-how-acquisitions-qualify-as-conversions"></a>Descripción de cómo las adquisiciones se califican como conversiones

Una *conversión* de campaña personalizada es una adquisición que se produce al hacer clic en una dirección URL que se promueve a través de una campaña personalizada. Hay diferentes escenarios para la calificación como conversión de las vistas de **Página de la aplicación y las conversiones por ID. de campaña** y gráficos de **conversiones de campaña totales** en el [Informe de adquisiciones](acquisitions-report.md) y para calificar como conversión para [recuperar mediante programación el identificador de la campaña](#programmatically).

### <a name="qualifying-conversions-in-the-acquisitions-report"></a>Conversiones de calificación en el informe de adquisiciones

Los escenarios siguientes se califican como una conversión de las **vistas de página y las conversiones de las páginas de la aplicación por ID. de campaña** y gráficos de **conversiones de campaña totales** en el [Informe de adquisiciones](acquisitions-report.md):

* Un cliente *con o sin un cuenta de Microsoft reconocido* hace clic en una dirección URL de la aplicación que contiene un identificador de campaña personalizado y se redirige a la lista de tiendas de la aplicación. Después, ese mismo cliente adquiere la aplicación en un plazo de 24 horas después de que haga clic por primera vez en la dirección URL del Microsoft Store con el identificador de campaña personalizado.

* Si el cliente adquiere la aplicación en un dispositivo diferente del que hizo clic en la dirección URL con el identificador de campaña personalizado, la conversión solo se contará si el cliente ha iniciado sesión con el mismo cuenta de Microsoft que cuando hizo clic en la dirección URL.

> [!NOTE]
> En el caso de las adquisiciones de aplicaciones que se cuentan como conversiones de una campaña personalizada, todas las compras de complementos en esa aplicación también se cuentan como conversiones de la misma campaña personalizada.

### <a name="qualifying-conversions-when-programmatically-retrieving-the-campaign-id"></a>Conversiones de calificación al recuperar el identificador de campaña mediante programación

Para considerar que existe conversión al recuperar mediante programación el identificador de campaña asociado con la aplicación, deben cumplirse las siguientes condiciones:

* En un dispositivo que ejecuta **Windows 10, versión 1607 o posterior**: un cliente (con la sesión iniciada en un cuenta de Microsoft reconocido o no) hace clic en una dirección URL que contiene un identificador de campaña personalizado y se redirige a la página de lista de la tienda de la aplicación. El cliente adquiere la aplicación mientras ve la lista de la tienda como resultado de hacer clic en la dirección URL.

* En un dispositivo que ejecuta **Windows 10, versión 1511 o anterior**: un cliente (que debe iniciar sesión con una cuenta de Microsoft reconocida) haga clic en una dirección URL que contenga un identificador de campaña personalizado y se redirigirá a la página de la lista de tiendas de la aplicación. El cliente adquiere la aplicación mientras ve la lista de la tienda como resultado de hacer clic en la dirección URL. En estas versiones de Windows 10, el usuario debe haber iniciado sesión con una cuenta de Microsoft reconocida para que la adquisición pueda calificarse como una conversión al recuperar el identificador de la campaña mediante programación.

> [!NOTE]
> Si el cliente deja la página de lista de la tienda, pero vuelve a la página con 24 horas (ya sea en el mismo dispositivo o en un dispositivo diferente cuando se inicia sesión con el mismo cuenta de Microsoft) y adquiere la aplicación, **se** considerará una conversión en los gráficos de las **vistas de la página de la aplicación y** las conversiones de la campaña y [de las](acquisitions-report.md) **conversiones de campaña** Sin embargo, esto **no será** apto como conversión si recupera mediante programación el identificador de la campaña.

## <a name="embed-a-custom-campaign-id-to-your-apps-microsoft-store-page-url"></a>Insertar un identificador de campaña personalizado en la dirección URL de la página de Microsoft Store de la aplicación

Para crear una dirección URL de página de Microsoft Store para la aplicación con un ID. de campaña personalizado:

1.  Crea una cadena de identificador para la campaña personalizada. Esta cadena puede contener hasta 100 caracteres, aunque recomendamos definir identificadores de campaña cortos para que sean fáciles de identificar.

   > [!NOTE]
   > La cadena de identificación de la campaña puede estar visible para otros desarrolladores cuando ven el [Informe de adquisiciones](acquisitions-report.md) para sus aplicaciones. Esto puede ocurrir cuando un cliente hace clic en el identificador de campaña personalizado para entrar en el almacén y compra la aplicación de otro desarrollador dentro de la misma sesión, con lo que se atribuye esa conversión al identificador de campaña. Ese desarrollador verá el número de conversiones de su propia aplicación producidas por un clic inicial en el identificador de la campaña, incluido el nombre del identificador de la campaña, pero no verá ningún dato sobre cuántos usuarios adquirieron sus propias aplicaciones (o aplicaciones de cualquier otro desarrollador) después de hacer clic en el identificador de la campaña.

2.  Obtiene el vínculo de la lista de tiendas de la aplicación en formato HTML o de protocolo.

    * Use la dirección URL HTML Si desea que los clientes naveguen a la lista de tiendas basada en Web de la aplicación en un explorador en cualquier sistema operativo. En los dispositivos Windows, la aplicación de la tienda también iniciará y mostrará la lista de la aplicación. Esta dirección URL tiene el formato **`https://www.microsoft.com/store/apps/*your app ID*`** . Por ejemplo, la dirección URL HTML para Skype es `https://www.microsoft.com/store/apps/9wzdncrfj364` . Puede encontrar esta dirección URL en la página identidad de la [aplicación](view-app-identity-details.md#link-to-your-apps-listing) .

    * Use el formato de Protocolo si va a promover la aplicación desde otras aplicaciones de Windows que se ejecutan en un dispositivo o equipo con la aplicación UWP instalada, o si sabe que los clientes están en un dispositivo que admite el Microsoft Store. Este vínculo irá directamente a la lista de tiendas de la aplicación sin abrir un explorador. Esta dirección URL tiene el formato **`ms-windows-store://pdp/?PRODUCTID=*your app id*`** . Por ejemplo, es la dirección URL de protocolo de Skype es `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.

3.  Anexa la siguiente cadena al final de la dirección URL de la aplicación:

    * Para una dirección URL de formato HTML, anexe **`?cid=*my custom campaign ID*`** . Por ejemplo, si Skype introduce un identificador de campaña con el **valor \_ campaña personalizada**, la nueva dirección URL que incluye el identificador de la campaña sería: `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign` .

    * Para una dirección URL de formato de protocolo, anexe **`&cid=*my custom campaign ID*`** . Por ejemplo, si Skype introduce un identificador de campaña con el **valor \_ campaña personalizada**, la dirección URL del nuevo protocolo incluido el ID. de campaña sería: `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign` .

<span id="programmatically" />

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>Recuperar mediante programación el identificador de campaña personalizada de una aplicación

Si la aplicación es una aplicación para UWP, puede recuperar mediante programación el identificador de campaña personalizado asociado a la adquisición de una aplicación mediante el uso de las API de la Windows SDK. Estas API hacen posibles muchos escenarios de análisis y monetización. Por ejemplo, puedes descubrir si el usuario actual compró la aplicación tras descubrirla gracias a tu campaña en Facebook y personalizar la experiencia en consecuencia. Si estás usando un proveedor de marketing de aplicaciones de terceros, puedes enviar datos de vuelta al proveedor.

Estas API devolverán una cadena de identificador de campaña solo si el cliente hizo clic en su dirección URL con el identificador de campaña incrustado, vio la página de Microsoft Store de la aplicación y, a continuación, adquirirá la aplicación sin cerrar la página de la tienda de tiendas. Si el usuario abandona la página y, después, devuelve y adquiere la aplicación, esto no se [considerará una conversión](#conversions) al usar estas API.

Hay diferentes API que puede usar en función de la versión de Windows 10 a la que se destina la aplicación:

* Windows 10, versión 1607 o posterior: Use la clase [**StoreContext**](/uwp/api/windows.services.store.storecontext) en el espacio de nombres **Windows. Services. Store** . Al usar esta API, puede recuperar los identificadores de campaña personalizados para cualquier [adquisición calificada](#conversions), tanto si el usuario ha iniciado sesión como si no con un cuenta de Microsoft reconocido.

* Windows 10, versión 1511 o versiones anteriores: Use la clase [**CurrentApp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) en el espacio de nombres **Windows. ApplicationModel. Store** . Cuando se usa esta API, solo se pueden recuperar los identificadores de campaña personalizados para las [adquisiciones calificadas](#conversions) en las que el usuario ha iniciado sesión con un cuenta de Microsoft reconocido.

> [!NOTE]
> Aunque el espacio de nombres **Windows. ApplicationModel. Store** está disponible en todas las versiones de Windows 10, se recomienda usar las API en el espacio de nombres **Windows. Services. Store** si su aplicación tiene como destino Windows 10, versión 1607 o posterior. Para obtener más información sobre las diferencias entre estos espacios de nombres, consulte [compras y pruebas desde la aplicación](../monetize/in-app-purchases-and-trials.md#choose-namespace). En el ejemplo de código siguiente se muestra cómo estructurar el código para usar ambas API en el mismo proyecto.

### <a name="code-example"></a>Ejemplo de código

En el ejemplo de código siguiente se muestra cómo recuperar el identificador de la campaña personalizada. En este ejemplo se usan ambos conjuntos de API en los espacios de nombres **Windows. Services. Store** y **Windows. ApplicationModel. Store** mediante el [código adaptable](../debug-test-perf/version-adaptive-code.md)de la versión. Al seguir este proceso, el código puede ejecutarse en cualquier versión de Windows 10. Para usar este código, la versión del sistema operativo de destino del proyecto debe ser **Windows 10 aniversario Edition (10,0; Compilación 14394)** o posterior, aunque la versión mínima del sistema operativo puede ser una versión anterior.

``` csharp
// This example assumes the code file has using statements for
// System.Linq, System.Threading.Tasks, Windows.Data.Json,
// and Windows.Services.Store.
public async Task<string> GetCampaignId()
{
    // Use APIs in the Windows.Services.Store namespace if they are available
    // (the app is running on a device with Windows 10, version 1607, or later).
    if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent(
         "Windows.Services.Store.StoreContext"))
    {
        StoreContext context = StoreContext.GetDefault();

        // Try to get the campaign ID for users with a recognized Microsoft account.
        StoreProductResult result = await context.GetStoreProductForCurrentAppAsync();
        if (result.Product != null)
        {
            StoreSku sku = result.Product.Skus.FirstOrDefault(s => s.IsInUserCollection);

            if (sku != null)
            {
                return sku.CollectionData.CampaignId;
            }
        }

        // Try to get the campaign ID from the license data for users without a
        // recognized Microsoft account.
        StoreAppLicense license = await context.GetAppLicenseAsync();
        JsonObject json = JsonObject.Parse(license.ExtendedJsonData);
        if (json.ContainsKey("customPolicyField1"))
        {
            return json["customPolicyField1"].GetString();
        }

        // No campaign ID was found.
        return String.Empty;
    }
    // Fall back to using APIs in the Windows.ApplicationModel.Store namespace instead
    // (the app is running on a device with Windows 10, version 1577, or earlier).
    else
    {
#if DEBUG
        return await Windows.ApplicationModel.Store.CurrentAppSimulator.GetAppPurchaseCampaignIdAsync();
#else
        return await Windows.ApplicationModel.Store.CurrentApp.GetAppPurchaseCampaignIdAsync() ;
#endif
    }
}
```

Este código hace lo siguiente:

1. En primer lugar, comprueba si la clase [**StoreContext**](/uwp/api/windows.services.store.storecontext) del espacio de nombres **Windows. Services. Store** está disponible en el dispositivo actual (esto significa que el dispositivo ejecuta Windows 10, versión 1607 o posterior). Si es así, el código continúa utilizando esta clase.

2. A continuación, intenta obtener el identificador de la campaña personalizada para el caso en el que el usuario actual tiene un cuenta de Microsoft reconocido. Para ello, el código obtiene un objeto [**StoreSku**](/uwp/api/Windows.Services.Store.StoreSku) que representa la SKU de la aplicación actual y, a continuación, accede a la propiedad [**CampaignId**](/uwp/api/windows.services.store.storecollectiondata.CampaignId) para recuperar el identificador de la campaña, si hay alguno disponible.
3. A continuación, el código intenta recuperar el ID. de campaña para el caso en el que el usuario actual no tiene un cuenta de Microsoft reconocido. En este caso, el identificador de la campaña se inserta en la licencia de la aplicación. El código recupera la licencia mediante el método [**GetAppLicenseAsync**](/uwp/api/windows.services.store.storecontext.GetAppLicenseAsync) y, a continuación, analiza el contenido JSON de la licencia para el valor de una clave denominada *customPolicyField1*. Este valor contiene el identificador de la campaña.

4. Si la clase [**StoreContext**](/uwp/api/windows.services.store.storecontext) del espacio de nombres **Windows. Services. Store** no está disponible, el código recurre al uso del método [**GetAppPurchaseCampaignIdAsync**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_GetAppPurchaseCampaignIdAsync) en el espacio de nombres **Windows. ApplicationModel. Store** para recuperar el identificador de la campaña personalizada (este espacio de nombres está disponible en todas las versiones de Windows 10, incluida la versión 1511 y versiones anteriores). Tenga en cuenta que cuando se usa este método, solo se pueden recuperar los identificadores de campaña personalizados para las [adquisiciones calificadas](#conversions) en las que el usuario tiene un cuenta de Microsoft reconocido.

### <a name="specify-the-campaign-id-in-the-proxy-file-for-the-windowsapplicationmodelstore-namespace"></a>Especifique el identificador de la campaña en el archivo de proxy para el espacio de nombres Windows. ApplicationModel. Store

El espacio de nombres **Windows. ApplicationModel. Store** incluye [**CurrentAppSimulator**](/uwp/api/windows.applicationmodel.store.currentappsimulator), una clase especial que simula las operaciones de almacenamiento para probar el código antes de enviar la aplicación a la tienda. Esta clase recupera datos de [un archivo local denominado Windows.StoreProxy.xml archivo](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator). En el ejemplo de código anterior se muestra cómo incluir el uso de **CurrentApp** y **CurrentAppSimulator** en depuración y código que no es de depuración en el proyecto. Para probar este código en un entorno de depuración, agregue un elemento **AppPurchaseCampaignId** al archivo WindowsStoreProxy.xml en el equipo de desarrollo, como se muestra en el ejemplo siguiente. Cuando ejecutes la aplicación, el método [**GetAppPurchaseCampaignIdAsync**](/uwp/api/windows.applicationmodel.store.currentappsimulator.GetAppPurchaseCampaignIdAsync) siempre devolverá este valor.

``` xml
<CurrentApp>
    ...
    <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
</CurrentApp>
```

El espacio de nombres **Windows.Services.Store** no proporciona ninguna clase que puedas usar para simular la información de licencia durante las pruebas. En su lugar, debes publicar una aplicación en la Tienda y descargar la aplicación en el dispositivo de desarrollo para usar su licencia para las pruebas. Para obtener más información, consulta [Pruebas y compras desde la aplicación](../monetize/in-app-purchases-and-trials.md#testing).

## <a name="test-your-custom-campaign"></a>Prueba tu campaña personalizada

Antes de que promociones la dirección URL de una campaña personalizada, te recomendamos que pruebes la campaña del siguiente modo:

1.  Inicie sesión en un cuenta de Microsoft en el dispositivo que está usando para las pruebas.

2.  Haz clic en la dirección URL de la campaña personalizada. Asegúrese de que se le lleva a la página de la aplicación y, a continuación, cierre la aplicación para UWP o la página del explorador.

3.  Haga clic en la dirección URL varias veces más, cerrando la aplicación de UWP o la página del explorador después de cada visita a la página de la aplicación. Durante **una** de las visitas a la página de la aplicación, adquiera la aplicación para generar una conversión. Cuenta el número total de veces que has hecho clic en la dirección URL.

4. Confirme si las vistas de página y las conversiones esperadas aparecen en las páginas de la **aplicación vistas y conversiones por ID. de campaña** y **total de conversiones de campaña** en el informe de [adquisiciones](acquisitions-report.md), y pruebe el código de la aplicación para confirmar si puede recuperar correctamente el ID. de campaña con las API descritas anteriormente.