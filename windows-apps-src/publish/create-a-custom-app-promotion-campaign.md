---
author: shawjohn
Description: "Además de crear una campaña publicitaria para tu aplicación que se ejecutará en aplicaciones de Windows, también puedes promocionar tu aplicación en otros canales."
title: "Crear una campaña de promoción de la aplicación personalizada"
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, personalizada, aplicación, promoción, campaña"
ms.openlocfilehash: 1e56b1df4b5501ded5db799c3ad67f97c0e1cfcd
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-a-custom-app-promotion-campaign"></a>Crear una campaña de promoción de la aplicación personalizada

Además de crear una [campaña publicitaria para tu aplicación](create-an-ad-campaign-for-your-app.md) que se ejecutará en aplicaciones de Windows, también puedes promocionar tu aplicación en otros canales. Por ejemplo, puedes promocionarla mediante proveedores de marketing para aplicaciones, o publicar vínculos a la aplicación en sitios de redes sociales. Estas actividades se denominan *campañas personalizadas*.

Si realizas campañas personalizadas para tu aplicación, puedes hacer un seguimiento del rendimiento relativo de cada una creando una dirección URL para cada campaña. Cada URL de la aplicación de la Tienda Windows contendrá un identificador de campaña distinto. Cuando un cliente que ejecuta Windows 10 hace clic en una dirección URL que contiene un identificador de campaña, Microsoft asocia el clic a la campaña personalizada correspondiente y tendrás los datos disponibles.

Hay dos tipos principales de datos asociados a las campañas personalizadas: las vistas de página de tu aplicación y las *conversiones*. Una conversión es una adquisición de la aplicación producida porque el cliente ha hecho clic en la página de la aplicación en la Tienda Windows desde una dirección URL promocionada mediante una campaña personalizada. Para obtener más información acerca de las conversiones, consulta [Qué adquisiciones de la aplicación se califican como conversiones](#understanding-how-app-acquisitions-qualify-as-conversions) en este tema.

Puedes recuperar los datos de rendimiento de una campaña personalizada para tu aplicación de las siguientes maneras:

* Puedes ver los datos sobre las vistas de página y las conversiones para tu aplicación o complemento desde el [Informe sobre canales y conversiones](channels-and-conversions-report.md), en el panel del Centro de desarrollo.
* Si es una aplicación de la Plataforma universal de Windows (UWP), puedes usar las API del Windows SDK para recuperar mediante programación el identificador de campaña personalizada que resultó en una conversión.

> **Importante**&nbsp;&nbsp;Solo se realizará un seguimiento de estos datos para identificar qué clientes ejecutan Windows 10. Los clientes que usen otros sistemas operativos pueden seguir el vínculo a la descripción de la aplicación, pero no se incluirán los datos acerca de sus actividades.

## <a name="example-custom-campaign-scenario"></a>Ejemplo de escenario de campaña personalizada

Imagina a una desarrolladora de juegos que ha terminado su última creación y quiere promocionarla entre los usuarios de sus juegos ya publicados. Publica el anuncio del nuevo lanzamiento en su página de Facebook e incluye un vínculo a la página del juego en la Tienda Windows. Muchos de sus jugadores también la siguen en Twitter, por lo que también tuitea el anuncio con el vínculo a la página del juego en la Tienda Windows.

Para realizar un seguimiento del éxito de cada uno de estos canales de promoción, la desarrolladora crea dos variantes de la dirección URL de la página del juego en la Tienda Windows:

* La dirección URL que publica en la página de Facebook incluye el identificador de campaña personalizada `my-facebook-campaign`.
* La dirección URL que publica en Twitter incluye el identificador de campaña personalizada `my-twitter-campaign`.

A medida que sus seguidores en Facebook y Twitter usen las direcciones URL, Microsoft irá registrando cada clic y lo asociará a la campaña personalizada correspondiente. Si cumplen las condiciones, las posteriores adquisiciones del juego y de cualquier complemento se asociarán a las campañas personalizadas y se notificarán como conversiones.

<span id="conversions" />
## <a name="understanding-how-app-acquisitions-qualify-as-conversions"></a>Qué adquisiciones de la aplicación se califican como conversiones

Una *conversión* es una adquisición de la aplicación producida porque el cliente ha hecho clic en la página de la aplicación en la Tienda Windows desde una dirección URL promocionada mediante una campaña personalizada. Existen distintas situaciones para que el [informe Canales y conversiones](channels-and-conversions-report.md) del panel del Centro de desarrollo y para que [recuperar mediante programación el identificador de campaña](#programmatically) califiquen una adquisición como conversión.

### <a name="qualifying-conversions-for-the-dashboard-report"></a>Conversiones calificadas para el informe de panel

Son posibles las situaciones siguientes para que exista una conversión según el [informe Canales y conversiones](channels-and-conversions-report.md):

* Un cliente *con o sin una cuenta Microsoft de reconocida* hace clic en la dirección URL de una aplicación, dirección que contiene el identificador de una campaña personalizada, y se le redirige a la página de la aplicación en la Tienda Windows. El mismo cliente adquiere la aplicación en el plazo de 24 horas a partir de haber hecho clic en la dirección URL de la Tienda Windows con el identificador de campaña personalizada.

* Si el cliente adquiere la aplicación en un equipo o dispositivo distintos desde el que hizo clic en la dirección URL de la Tienda Windows con el identificador de campaña personalizada, la conversión solo se contará si el cliente tiene una cuenta de Microsoft.

> **Nota**&nbsp;&nbsp;Para las adquisiciones de aplicaciones que se cuentan como conversiones para una campaña personalizada, toda compra de complementos en esa aplicación también se cuenta como una conversión para la misma campaña personalizada.

### <a name="qualifying-conversions-for-programmatically-retrieving-the-campaign-id"></a>Conversiones calificadas para la recuperación mediante programación del identificador de campaña

Para considerar que existe conversión al recuperar mediante programación el identificador de campaña asociado con la aplicación, deben cumplirse las siguientes condiciones:

* En un dispositivo que ejecuta **Windows 10, versión 1607 o una versión posterior**: un cliente *con o sin una cuenta de Microsoft reconocida* hace clic en una dirección URL que contiene un identificador de campaña y se le redirige a la página de la Tienda Windows de la aplicación. Después de hacer clic en la dirección URL, el cliente adquiere la aplicación durante la misma vista de página de la Tienda Windows.

* En un dispositivo que ejecuta **Windows 10, versión 1511 o una versión anterior**: un cliente *con una cuenta de Microsoft reconocida* hace clic en una dirección URL que contiene un identificador de campaña y se le redirige a la página de la Tienda Windows de la aplicación. Después de hacer clic en la dirección URL, el cliente adquiere la aplicación durante la misma vista de página de la Tienda Windows. En estas versiones de Windows 10, el usuario debe tener una cuenta de Microsoft reconocida para que la adquisición se certifique como conversión cuando el identificador de campaña se recupere mediante programación.

>**Nota**&nbsp;&nbsp;Si el cliente abandona la página y luego vuelve (ya sea en el mismo equipo o dispositivo o en otro diferente si tiene una cuenta de Microsoft) dentro de las 24 horas siguientes y adquiere la aplicación, esto se califica como conversión para el informe [Canales y conversión](channels-and-conversions-report.md). Sin embargo, no se calificará como conversión si recuperas mediante programación el identificador de campaña.

## <a name="embed-a-custom-campaign-id-to-your-apps-windows-store-page-url"></a>Incrustar un identificador de campaña personalizada en la dirección URL de la página de tu aplicación en la Tienda Windows

Para crear una dirección URL de la página de tu aplicación en la Tienda Windows con un identificador de campaña personalizada:

1.  Crea una cadena de identificador para la campaña personalizada. Esta cadena puede contener hasta 100 caracteres, aunque recomendamos definir identificadores de campaña cortos para que sean fáciles de identificar.

  >**Nota**&nbsp;&nbsp;La cadena del identificador de campaña puede ser visible para otros desarrolladores dentro de un informe Canales y conversiones. Esto puede ocurrir cuando un cliente hace clic en el identificador de campaña para acceder a la Tienda, pero acaba comprando la aplicación de otro desarrollador dentro de la misma sesión. Aunque en este caso otro desarrollador podrá ver tu nombre del identificador de campaña, ese desarrollador solo verá cuántas conversiones de su propia aplicación provienen de un clic inicial en tu identificador de campaña. No podrán ver los datos sobre el número de usuarios que compraron tu aplicación a partir de hacer clic en tu identificador de campaña.

2.  Obtén la dirección URL de la página de tu aplicación en la Tienda Windows en formato HTML o de protocolo.

    * Usa el formato HTTP si quieres que los clientes naveguen a la página de la Tienda Windows de tu aplicación en un explorador (esta dirección URL también iniciará la aplicación de la Tienda Windows en la descripción de la aplicación, en caso de que la aplicación de la Tienda Windows esté instalada). Esta dirección URL tiene el formato **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**. Por ejemplo, la dirección URL HTTP para Skype es `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`. La dirección URL en formato HTML está disponible en la página [**Identidad de la aplicación** en el panel del Centro de desarrollo](link-to-your-app.md). Ten en cuenta que las direcciones URL en formato HTTP se pueden usar para navegar a la Tienda Windows en un explorador en equipos y tabletas que ejecuten Windows7 y versiones posteriores, así como en teléfonos con Windows Phone8 y posterior.

    * Usa el formato de protocolo si la promoción de la aplicación es a través de otras aplicaciones de Windows que se ejecutan en un equipo o dispositivo con la aplicación de la Tienda Windows instalada y quieres que los clientes abran la página de la aplicación en la aplicación de la Tienda Windows. Esta dirección URL tiene el formato **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**. Por ejemplo, es la dirección URL de protocolo de Skype es `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.

3.  Anexa la siguiente cadena al final de la dirección URL de la aplicación:

    * Para una dirección URL en formato HTTP, anexa **`?cid=*my custom campaign ID*`**. Por ejemplo, si Skype presenta un identificador de campaña con el valor **custom\_campaign**, la nueva dirección URL HTTP que incluye el identificador de campaña sería: `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`.

    * Para una dirección URL en formato de protocolo, anexa **`&cid=*my custom campaign ID*`**. Por ejemplo, si Skype presenta un identificador de campaña con el valor **custom\_campaign**, la nueva dirección URL de protocolo que incluye el identificador de campaña sería: `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`.

<span id="programmatically" />
## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>Recuperar mediante programación el identificador de campaña personalizada de una aplicación

Si la tuya es una aplicación para UWP, puedes recuperar mediante programación el identificador de campaña personalizada que está asociada con tu aplicación mediante API en el Windows SDK. Estas API permiten muchos escenarios de análisis y monetización. Por ejemplo, puedes descubrir si el usuario actual compró la aplicación tras descubrirla gracias a tu campaña en Facebook y personalizar la experiencia en consecuencia. Si estás usando un proveedor de marketing de aplicaciones de terceros, puedes enviar datos de vuelta al proveedor.

Estas API solo devuelven una cadena con el identificador de campaña si el cliente hace clic en la dirección URL con el identificador de campaña integrado, se le redirige a la página de tu aplicación en la Tienda Windows y adquiere la aplicación sin salir de esta página. Si el usuario abandona la página y más tarde vuelve a adquirir la aplicación, no se [calificará como conversión](#conversions) al usar estas API.

Puedes usar distintas API en función de la versión de Windows 10 a la que se dirige tu aplicación:

* Windows 10, versión 1607 o posterior: usa la clase [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) del espacio de nombres **Windows.Services.Store**. Al usar esta API, puedes recuperar identificadores de campaña personalizados para todas las [conversiones calificadas](#conversions), independientemente de si el usuario tiene o no una cuenta de Microsoft reconocida.
* Windows 10, versión 1511 o anterior: usa la clase [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx) del espacio de nombres **Windows.ApplicationModel.Store**. Al usar esta API, solo puedes recuperar identificadores de campaña personalizados para las [conversiones calificadas](#conversions) en las que el usuario tiene una cuenta de Microsoft reconocida.

>**Nota**&nbsp;&nbsp;Aunque el espacio de nombres **Windows.ApplicationModel.Store** está disponible en todas las versiones de Windows 10, recomendamos que uses las API del espacio de nombres **Windows.Services.Store** si tu aplicación se dirige a Windows 10, versión 1607 o posterior. Para más información sobre las diferencias entre estos espacios de nombres, consulta [Pruebas y compras desde la aplicación](../monetize/in-app-purchases-and-trials.md#choose-namespace). En el siguiente ejemplo de código se muestra cómo estructurar el código para que use ambas API en el mismo proyecto.

### <a name="code-example"></a>Ejemplo de código

En el siguiente ejemplo de código se muestra cómo recuperar el identificador de campaña personalizado. En este ejemplo se usan ambos conjuntos de API en los espacios de nombres **Windows.Services.Store** y **Windows.ApplicationModel.Store** mediante [código adaptativo para versiones](../debug-test-perf/version-adaptive-code.md). Al seguir este proceso, tu código podrá ejecutarse en cualquier versión de Windows 10. Para usar este código, la versión del SO de destino de tu proyecto debe ser **Windows 10 Anniversary Edition (10.0; compilación 14394)** o posterior, aunque la versión mínima del SO puede ser una versión anterior.

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

2. Luego, intenta obtener el identificador de campaña personalizado para el escenario en el que el usuario actual tiene una cuenta de Microsoft reconocida. Para ello, el código obtiene un objeto [**StoreSku**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreSku) que representa la SKU de la aplicación actual y luego accede a la propiedad [**CampaignId**](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata#Windows_Services_Store_StoreCollectionData_CampaignId) para recuperar el identificador de campaña, si hay un disponible.

2. El código luego intenta recuperar el identificador de campaña para el caso en el que el usuario actual no tiene una cuenta de Microsoft reconocida. En este caso, el identificador de campaña se integra en la licencia de la aplicación. El código recupera la licencia mediante el método [**GetAppLicenseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetAppLicenseAsync) y luego analiza el contenido JSON de la licencia para obtener el valor de una clave llamada *customPolicyField1*. Este valor contiene el identificador de campaña.

3. Si la clase [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) del espacio de nombres **Windows.Services.Store** no está disponible, el código usa el método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.getapppurchasecampaignidasync.aspx) del espacio de nombres **Windows.ApplicationModel.Store** para recuperar el identificador de campaña personalizado (este espacio de nombres está disponible en todas las versiones de Windows 10, incluso la versión 1511 y versiones posteriores). Ten en cuenta que cuando usas este método, solo puedes recuperar identificadores de campaña personalizados para las [conversiones calificadas](#conversions) en las que el usuario tiene una cuenta de Microsoft reconocida.

  >**Nota**&nbsp;&nbsp;El espacio de nombres **Windows.ApplicationModel.Store** incluye [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator), una clase especial que simula las operaciones de la Tienda para probar el código antes de enviar tu aplicación a la Tienda. Esta clase recupera datos de [un archivo local llamado Windows.StoreProxy.xml](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator). En el ejemplo de código anterior se muestra cómo incluir **CurrentApp** y **CurrentAppSimulator** en el código de depuración y no depuración del proyecto. Para probar este código en un entorno de depuración, agrega un elemento **AppPurchaseCampaignId** al archivo WindowsStoreProxy.xml del equipo de desarrollo, tal como se muestra en el siguiente ejemplo. Cuando ejecutes la aplicación, el método [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator#Windows_ApplicationModel_Store_CurrentAppSimulator_GetAppPurchaseCampaignIdAsync) siempre devolverá este valor.

  ``` xml
  <CurrentApp>
      ...
      <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
  </CurrentApp>
  ```

  >**Nota**&nbsp;&nbsp;El espacio de nombres **Windows.Services.Store** no proporciona ninguna clase que puedas usar para simular la información de licencia durante las pruebas. En su lugar, debes publicar una aplicación en la Tienda y descargar la aplicación en el dispositivo de desarrollo para usar su licencia para las pruebas. Para obtener más información, consulta [Pruebas y compras desde la aplicación](../monetize/in-app-purchases-and-trials.md#testing).

## <a name="test-your-custom-campaign"></a>Prueba tu campaña personalizada

Antes de que promociones la dirección URL de una campaña personalizada, te recomendamos que pruebes la campaña del siguiente modo:

1.  Inicia sesión con tu cuenta de Microsoft en el equipo o dispositivo que uses para las pruebas.
2.  Haz clic en la dirección URL de la campaña personalizada. Asegúrate de que la Tienda Windows carga la página de la aplicación correctamente y después cierra la aplicación de la Tienda Windows o la página del explorador.
3.  Haz clic en la dirección URL varias veces más y cierra la aplicación de la Tienda Windows o la página del explorador después de cada visita. Durante una de las visitas, adquiere la aplicación para generar una conversión. Cuenta el número total de veces que has hecho clic en la dirección URL.
4. Confirma si las vistas y conversiones de página esperadas aparecen en el [informe Canales y conversiones](channels-and-conversions-report.md) y prueba el código de tu aplicación para confirmar que puede recuperar correctamente el identificador de campaña.
