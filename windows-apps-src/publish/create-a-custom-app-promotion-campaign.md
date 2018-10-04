---
author: JnHs
description: Además de crear una campaña publicitaria para tu aplicación que se ejecutará en aplicaciones de Windows, también puedes promocionar tu aplicación en otros canales.
title: Crear una campaña de promoción de la aplicación personalizada
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.author: wdg-dev-content
ms.date: 09/27/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, personalizada, aplicación, promoción, campaña
ms.localizationpriority: medium
ms.openlocfilehash: 13ee8d7482a2ce0716d4e133af329cd0ea42c184
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "4358492"
---
# <a name="create-a-custom-app-promotion-campaign"></a>Crear una campaña de promoción de la aplicación personalizada

Además de crear una [campaña publicitaria para tu aplicación](create-an-ad-campaign-for-your-app.md) que se ejecutará en aplicaciones de Windows, también puedes promocionar tu aplicación en otros canales. Por ejemplo, puedes promocionarla mediante proveedores de marketing para aplicaciones, o publicar vínculos a la aplicación en sitios de redes sociales. Estas actividades se denominan *campañas personalizadas*.

Si realizas campañas personalizadas para tu aplicación, puedes hacer un seguimiento del rendimiento relativo de cada una creando una dirección URL para cada campaña personalizada, de manera que cada URL contenga un *identificador de campaña* distinto. Cuando un cliente que ejecuta Windows 10 hace clic en una dirección URL que contiene un identificador de campaña, Microsoft asocia el clic a la campaña personalizada correspondiente y tendrás los datos disponibles.

> [!IMPORTANT]
> Solo se realiza un seguimiento de estos datos para los clientes con Windows10. Los clientes que usen otros sistemas operativos pueden seguir el vínculo a la descripción de la aplicación, pero no se incluirán los datos acerca de sus actividades.

Hay dos tipos principales de datos asociados a las campañas personalizadas: las *vistas de página* de la descripción de Store de la aplicación y las *conversiones*. Una conversión es una adquisición de la aplicación producida porque un cliente ha visto la página de la descripción de la Tienda de la aplicación desde una dirección URL que incluye un identificador de campaña personalizada. Para obtener más información acerca de las conversiones, consulta [Qué adquisiciones de la aplicación se califican como conversiones](#understanding-how-acquisitions-qualify-as-conversions) en este tema.

Puedes recuperar los datos de rendimiento de una campaña personalizada para tu aplicación de las siguientes maneras:

* Puedes ver los datos sobre las vistas de página y conversiones de la aplicación o complemento en los gráficos **Vistas de página y conversiones de la aplicación por identificador de campaña** y **Total de conversiones de campaña** del [Informe Adquisiciones](acquisitions-report.md) en el panel de información del Centro de desarrollo.
* Si es una aplicación de la Plataforma universal de Windows (UWP), puedes usar las API del WindowsSDK para recuperar mediante programación el identificador de campaña personalizada que resultó en una conversión.

## <a name="example-custom-campaign-scenario"></a>Ejemplo de escenario de campaña personalizada

Imagina a una desarrolladora de juegos que ha terminado su última creación y quiere promocionarla entre los usuarios de sus juegos ya publicados. Publica el anuncio del nuevo lanzamiento de juego en su página de Facebook e incluye un vínculo a la descripción de Store del juego. Muchos de sus jugadores también la siguen en Twitter, por lo que también tuitea un anuncio con el vínculo a la descripción de Store del juego.

Para realizar un seguimiento del éxito de cada uno de estos canales de promoción, la desarrolladora crea dos variantes de la dirección URL de la descripción de Store del juego.

* La dirección URL que publica en la página de Facebook incluye el id. de campaña personalizada . `my-facebook-campaign`

* La dirección URL que publica en Twitter incluye el id. de campaña personalizada. `my-twitter-campaign`

A medida que sus seguidores en Facebook y Twitter usen las direcciones URL, Microsoft irá registrando cada clic y lo asociará a la campaña personalizada correspondiente. Si cumplen las condiciones, las posteriores adquisiciones del juego y de cualquier complemento se asociarán a las campañas personalizadas y se notificarán como conversiones.

<span id="conversions" />

## <a name="understanding-how-acquisitions-qualify-as-conversions"></a>Qué adquisiciones se califican como conversiones

Una *conversión* de una campaña personalizada es una adquisición producida porque el cliente ha hecho clic en una URL promocionada mediante una campaña personalizada. Existen diferentes situaciones para que una instalación se califique como conversión para los gráficos **Vistas de página y conversiones de la aplicación por identificador de campaña** y **Total de conversiones de campaña** en el [Informe Adquisiciones](acquisitions-report.md) del panel de información del Centro de desarrollo y para que se califique como conversión para la [recuperación mediante programación del identificador de campaña](#programmatically).

### <a name="qualifying-conversions-in-the-dashboard-report"></a>Conversiones calificadas para el informe de panel de información

Las siguientes situaciones califican como conversión para los gráficos **Vistas de página y conversiones de la aplicación por identificador de campaña** y **Total de conversiones de campaña** en el [Informe Adquisiciones](acquisitions-report.md):

* Un cliente *con o sin una cuenta Microsoft de reconocida* hace clic en la dirección URL de una aplicación, dirección que contiene el identificador de una campaña personalizada, y se le redirige a la descripción de la Tienda de la aplicación. Después, el mismo cliente adquiere la aplicación en un plazo de 24 horas a partir de haber hecho clic por primera vez en la dirección URL de Microsoft Store con el id. de campaña personalizada.

* Si el cliente adquiere la aplicación en un dispositivo distinto desde el que hizo clic en la dirección URL con el identificador de campaña personalizada, la conversión solo se contará si el cliente ha iniciado sesión en la misma cuenta de Microsoft cuando hizo clic en la dirección URL.

> [!NOTE]
> Para las adquisiciones de aplicaciones que se cuentan como conversiones para una campaña personalizada, toda compra de complementos en esa aplicación también se cuenta como una conversión para la misma campaña personalizada.

### <a name="qualifying-conversions-when-programmatically-retrieving-the-campaign-id"></a>Conversiones calificadas cuando se realiza la recuperación mediante programación del identificador de campaña

Para considerar que existe conversión al recuperar mediante programación el identificador de campaña asociado con la aplicación, deben cumplirse las siguientes condiciones:

* En un dispositivo que ejecuta **Windows10, versión 1607 o una versión posterior**: un cliente (que inicie sesión o no en una cuenta de Microsoft reconocida) hace clic en una dirección URL que contiene un identificador de campaña y se le redirige a la página de la descripción de la Tienda de la aplicación. El cliente adquiere la aplicación al ver la descripción de la Tienda como resultado de hacer clic en la dirección URL.

* En un dispositivo que ejecuta **Windows10, versión 1511 o una versión anterior**: un cliente (que debe iniciar sesión con una cuenta de Microsoft reconocida) hace clic en una dirección URL que contiene un identificador de campaña y se le redirige a la página de la descripción de la Tienda de la aplicación. El cliente adquiere la aplicación al ver la descripción de la Tienda como resultado de hacer clic en la dirección URL. En estas versiones de Windows10, el usuario debe iniciar sesión con una cuenta de Microsoft reconocida para que la adquisición se certifique como conversión cuando el identificador de campaña se recupere mediante programación.

> [!NOTE]
> Si el cliente abandona la página de la descripción de la Tienda, pero vuelve a la página dentro de las 24 horas siguientes (en el mismo dispositivo o en un dispositivo diferente pero con la misma cuenta de Microsoft) y adquiere la aplicación, esto **calificará** como conversión en los gráficos **Vistas de página y conversiones de la aplicación por identificador de campaña** y **Total de conversiones de campaña** en el [Informe adquisiciones](acquisitions-report.md). Sin embargo, **no** calificará como conversión si recuperas mediante programación el identificador de campaña.

## <a name="embed-a-custom-campaign-id-to-your-apps-microsoft-store-page-url"></a>Incrustar un id. de campaña personalizada en la dirección URL de la página de Microsoft Store de tu aplicación

Para crear una dirección URL de la página de Microsoft Store de tu aplicación con un identificador de campaña personalizada:

1.  Crea una cadena de identificador para la campaña personalizada. Esta cadena puede contener hasta 100 caracteres, aunque recomendamos definir identificadores de campaña cortos para que sean fáciles de identificar.

   > [!NOTE]
   > La cadena con el identificador de campaña puede estar visible para otros desarrolladores cuando vean el [Informe Adquisiciones](acquisitions-report.md) para sus aplicaciones. Esto puede ocurrir cuando un cliente hace clic en tu identificador de campaña personalizado para acceder a la Tienda y compra la aplicación de otro desarrollador en la misma sesión. Por tanto, esa conversión se atribuye a tu identificador de campaña. Este desarrollador verá cuántas conversiones de su propia aplicación se originaron por un clic inicial en tu identificador de campaña, incluido el nombre del identificador de campaña, pero no verá los datos sobre cuántos usuarios han comprado tus aplicaciones (o las aplicaciones de otros desarrolladores) tras hacer clic en tu identificador de campaña.

2.  Obtén el vínculo de la descripción de Store de tu aplicación en formato HTML o de protocolo.

    * Utiliza la dirección URL HTML si quieres que los clientes se dirijan a la descripción de la Tienda basada en web de tu aplicación en un navegador de cualquier sistema operativo. En los dispositivos Windows, la aplicación Tienda también se iniciará y mostrará la descripción de tu aplicación. Esta dirección URL tiene el formato **`https://www.microsoft.com/store/apps/*your app ID*`**. Por ejemplo, la dirección URL HTML para Skype es `https://www.microsoft.com/store/apps/9wzdncrfj364`. Puedes encontrar esta dirección URL en la página [Identidad de la aplicación](view-app-identity-details.md#link-to-your-apps-listing).

    * Usa el formato de protocolo si estás promocionando la aplicación en otras aplicaciones de Windows que se ejecutan en un equipo o dispositivo con la aplicación para UWP instalada, o cuando sabes que los clientes están usando un dispositivo que admite Microsoft Store. Este vínculo te redirigirá directamente a la descripción de la Tienda de la aplicación sin abrir un navegador. Esta dirección URL tiene el formato **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**. Por ejemplo, es la dirección URL de protocolo de Skype es `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.

3.  Anexa la siguiente cadena al final de la dirección URL de la aplicación:

    * Para una dirección URL en formato HTML, anexa **`?cid=*my custom campaign ID*`**. Por ejemplo, si Skype presenta un identificador de campaña con el valor **custom\_campaign**, la nueva dirección URL que incluye el identificador de campaña sería: `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`.

    * Para una dirección URL en formato de protocolo, anexa **`&cid=*my custom campaign ID*`**. Por ejemplo, si Skype presenta un identificador de campaña con el valor **custom\_campaign**, la nueva dirección URL de protocolo que incluye el identificador de campaña sería: `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`.

<span id="programmatically" />

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>Recuperar mediante programación el identificador de campaña personalizada de una aplicación

Si la tuya es una aplicación para UWP, puedes recuperar mediante programación el identificador de campaña personalizada que está asociada a la adquisición de la aplicación mediante API en el Windows SDK. Estas API permiten muchos escenarios de análisis y monetización. Por ejemplo, puedes descubrir si el usuario actual compró la aplicación tras descubrirla gracias a tu campaña en Facebook y personalizar la experiencia en consecuencia. Si estás usando un proveedor de marketing de aplicaciones de terceros, puedes enviar datos de vuelta al proveedor.

Estas API devolverán una cadena con el id. de campaña solo si el cliente hizo clic en la dirección URL con el id. de campaña incrustado, vio la página de Microsoft Store de tu aplicación y luego adquirió la aplicación sin abandonar la página de la descripción de Store. Si el usuario abandona la página y más tarde vuelve a adquirir la aplicación, no se [calificará como conversión](#conversions) al usar estas API.

Puedes usar distintas API en función de la versión de Windows 10 a la que se dirige tu aplicación:

* Windows 10, versión 1607 o posterior: usa la clase [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) del espacio de nombres **Windows.Services.Store**. Al usar esta API, puedes recuperar identificadores de campaña personalizados para todas las [conversiones calificadas](#conversions), independientemente de si el usuario ha iniciado sesión con una cuenta de Microsoft reconocida.

* Windows 10, versión 1511 o anterior: usa la clase [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) del espacio de nombres **Windows.ApplicationModel.Store**. Al usar esta API, solo puedes recuperar identificadores de campaña personalizados para las [conversiones calificadas](#conversions) en las que el usuario ha iniciado sesión en una cuenta de Microsoft reconocida.

> [!NOTE]
> Aunque el espacio de nombres **Windows.ApplicationModel.Store** está disponible en todas las versiones de Windows10, recomendamos que uses las API del espacio de nombres **Windows.Services.Store** si tu aplicación se dirige a Windows10, versión 1607 o versiones posteriores. Para más información sobre las diferencias entre estos espacios de nombres, consulta [Pruebas y compras desde la aplicación](../monetize/in-app-purchases-and-trials.md#choose-namespace). En el siguiente ejemplo de código se muestra cómo estructurar el código para que use ambas API en el mismo proyecto.

### <a name="code-example"></a>Ejemplo de código

En el siguiente ejemplo de código se muestra cómo recuperar el identificador de campaña personalizado. En este ejemplo se usan ambos conjuntos de API en los espacios de nombres **Windows.Services.Store** y **Windows.ApplicationModel.Store** mediante [código adaptativo para versiones](../debug-test-perf/version-adaptive-code.md). Al seguir este proceso, tu código podrá ejecutarse en cualquier versión de Windows 10. Para usar este código, la versión del SO de destino de tu proyecto debe ser **Windows10AnniversaryEdition (10.0; compilación14394)** o posterior, aunque la versión mínima del SO puede ser una versión anterior.

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

Este código realiza lo siguiente:

1. En primer lugar, comprueba si la clase [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) del espacio de nombres **Windows.Services.Store** está disponible en el dispositivo actual (significa que el dispositivo ejecuta Windows 10, versión 1607 o posterior). En caso afirmativo, el código procede con el uso de esta clase.

2. Luego, intenta obtener el identificador de campaña personalizado para el escenario en el que el usuario actual tiene una cuenta de Microsoft reconocida. Para ello, el código obtiene un objeto [**StoreSku**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreSku) que representa la SKU de la aplicación actual y luego accede a la propiedad [**CampaignId**](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.CampaignId) para recuperar el identificador de campaña, si hay un disponible.
3. El código luego intenta recuperar el identificador de campaña para el caso en el que el usuario actual no tiene una cuenta de Microsoft reconocida. En este caso, el identificador de campaña se integra en la licencia de la aplicación. El código recupera la licencia mediante el método [**GetAppLicenseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetAppLicenseAsync) y luego analiza el contenido JSON de la licencia para obtener el valor de una clave llamada *customPolicyField1*. Este valor contiene el identificador de campaña.

4. Si la clase [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) del espacio de nombres **Windows.Services.Store** no está disponible, el código usa entonces el método [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_GetAppPurchaseCampaignIdAsync) del espacio de nombres **Windows.ApplicationModel.Store** para recuperar el id. de campaña personalizado (este espacio de nombres está disponible en todas las versiones de Windows 10, incluyendo la versión 1511 y las anteriores). Ten en cuenta que cuando usas este método, solo puedes recuperar identificadores de campaña personalizados para las [conversiones calificadas](#conversions) en las que el usuario tiene una cuenta de Microsoft reconocida.

### <a name="specify-the-campaign-id-in-the-proxy-file-for-the-windowsapplicationmodelstore-namespace"></a>Especifica el id. de campaña en el archivo proxy del espacio de nombres Windows.ApplicationModel.Store

El espacio de nombres **Windows.ApplicationModel.Store** incluye [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator), una clase especial que simula las operaciones de la Tienda para probar el código antes de enviar tu aplicación a la Tienda. Esta clase recupera datos de [un archivo local llamado Windows.StoreProxy.xml](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator). En el ejemplo de código anterior se muestra cómo incluir **CurrentApp** y **CurrentAppSimulator** en el código de depuración y no depuración del proyecto. Para probar este código en un entorno de depuración, agrega un elemento **AppPurchaseCampaignId** al archivo WindowsStoreProxy.xml del equipo de desarrollo, tal como se muestra en el siguiente ejemplo. Cuando ejecutes la aplicación, el método [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.GetAppPurchaseCampaignIdAsync) siempre devolverá este valor.

``` xml
<CurrentApp>
    ...
    <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
</CurrentApp>
```

El espacio de nombres **Windows.Services.Store** no proporciona ninguna clase que puedas usar para simular la información de licencia durante las pruebas. En su lugar, debes publicar una aplicación en la Tienda y descargar la aplicación en el dispositivo de desarrollo para usar su licencia para las pruebas. Para obtener más información, consulta [Pruebas y compras desde la aplicación](../monetize/in-app-purchases-and-trials.md#testing).

## <a name="test-your-custom-campaign"></a>Prueba tu campaña personalizada

Antes de que promociones la dirección URL de una campaña personalizada, te recomendamos que pruebes la campaña del siguiente modo:

1.  Inicia sesión con una cuenta de Microsoft en el dispositivo que uses para las pruebas.

2.  Haz clic en la dirección URL de la campaña personalizada. Asegúrate de que te redirige a la página de tu aplicación y luego cierra la aplicación para UWP o la página del navegador.

3.  Haz clic en la dirección URL varias veces más y cierra la aplicación para UWP o la página del navegador después de cada visita a la página de tu aplicación. Durante **una** de las visitas a la página de tu aplicación, adquiere la aplicación para generar una conversión. Cuenta el número total de veces que has hecho clic en la dirección URL.

4. Confirma si las conversiones y vistas de página esperadas aparecen en los gráficos **Vistas de página y conversiones de la aplicación por identificador de campaña** y **Total de conversiones de campaña** en el [Informe Adquisiciones](acquisitions-report.md) del panel de información del Centro de desarrollo, y prueba el código de tu aplicación para comprobar si puede recuperar correctamente el identificador de campaña con las API indicadas anteriormente.
