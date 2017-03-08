---
author: shawjohn
Description: "Además de crear una campaña publicitaria para tu aplicación, que se ejecutará en aplicaciones de Windows, también puedes promocionar tu aplicación en otros canales."
title: "Crear una campaña de promoción de la aplicación personalizada"
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, personalizada, aplicación, promoción, campaña"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7920e2ba2fd4222ba012a98751e35133b36ac334
ms.lasthandoff: 02/07/2017

---

# <a name="create-a-custom-app-promotion-campaign"></a>Crear una campaña de promoción de la aplicación personalizada



Además de crear una [campaña publicitaria para tu aplicación](create-an-ad-campaign-for-your-app.md), que se ejecutará en aplicaciones de Windows, también puedes promocionar tu aplicación en otros canales. Por ejemplo, puedes promocionarla mediante proveedores de marketing para aplicaciones, o publicar vínculos a la aplicación en sitios de redes sociales. Estas actividades se denominan *campañas personalizadas*.

Si realizas campañas personalizadas para tu aplicación, puedes hacer un seguimiento del rendimiento relativo de cada una creando una dirección URL para cada campaña. Cada URL de la aplicación de la Tienda Windows contendrá un identificador de campaña distinto. Cuando un cliente que ejecuta Windows 10 hace clic en una dirección URL que contiene un identificador de campaña, Microsoft asocia el clic a la campaña personalizada correspondiente y tendrás los datos disponibles.

Hay dos tipos principales de datos asociados a las campañas personalizadas: las vista de página de tu aplicación y las *conversiones*. Una conversión es una adquisición de la aplicación producida porque el cliente ha hecho clic en la página de la aplicación en la Tienda Windows desde una dirección URL promocionada mediante una campaña personalizada. Para obtener más información acerca de las conversiones, consulta [Qué adquisiciones de la aplicación se califican como conversiones](#understanding-how-app-acquisitions-qualify-as-conversions) en este tema.

Puedes recuperar los datos de rendimiento de una campaña personalizada para tu aplicación de las siguientes maneras:

-   Si es una aplicación de la Plataforma universal de Windows (UWP), puedes usar el método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) para recuperar mediante programación el identificador de campaña personalizada que resultó en una conversión.
-   Puedes ver los datos sobre las vistas de página y las conversiones para tu aplicación o complemento desde el [Informe sobre canales y conversiones](channels-and-conversions-report.md), en el panel del Centro de desarrollo.

> **Importante**   Solo se realizará un seguimiento de estos datos para identificar qué clientes ejecutan Windows 10. Los clientes que usen otros sistemas operativos pueden seguir el vínculo a la descripción de la aplicación, pero no se incluirán los datos acerca de sus actividades.

 

## <a name="example-custom-campaign-scenario"></a>Ejemplo de escenario de campaña personalizada


Imagina a una desarrolladora de juegos que ha terminado su última creación y quiere promocionarla entre los usuarios de sus juegos ya publicados. Publica el anuncio del nuevo lanzamiento en su página de Facebook e incluye un vínculo a la página del juego en la Tienda Windows. Muchos de sus jugadores también la siguen en Twitter, por lo que también tuitea el anuncio con el vínculo a la página del juego en la Tienda Windows.

Para realizar un seguimiento del éxito de cada uno de estos canales de promoción, la desarrolladora crea dos variantes de la dirección URL de la página del juego en la Tienda Windows:

-   La dirección URL que publica en la página de Facebook incluye el identificador de campaña personalizada `my-facebook-campaign`.
-   La dirección URL que publica en Twitter incluye el identificador de campaña personalizada `my-twitter-campaign`.

A medida que sus seguidores en Facebook y Twitter usen las direcciones URL, Microsoft irá registrando cada clic y lo asociará a la campaña personalizada correspondiente. Si cumplen las condiciones, las posteriores adquisiciones del juego y de cualquier complemento se asociarán a las campañas personalizadas y se notificarán como conversiones.

## <a name="understanding-how-app-acquisitions-qualify-as-conversions"></a>Qué adquisiciones de la aplicación se califican como conversiones


Una *conversión* es una adquisición de la aplicación producida porque el cliente ha hecho clic en la página de la aplicación en la Tienda Windows desde una dirección URL promocionada mediante una campaña personalizada. Existen distintas situaciones para que el [informe Canales y conversiones](channels-and-conversions-report.md) del panel del Centro de desarrollo y para que [recuperar mediante programación el identificador de campaña](#programmatically) califiquen una adquisición como conversión.

Son posibles las situaciones siguientes para que exista una conversión según el [informe Canales y conversiones](channels-and-conversions-report.md):

-   Un cliente con o sin una cuenta Microsoft reconocida hace clic en la dirección URL de una aplicación, dirección que contiene el identificador de una campaña personalizada, y se le redirige a la página de la aplicación en la Tienda Windows. El mismo cliente adquiere la aplicación en el plazo de 24 horas a partir de haber hecho clic en la dirección URL de la Tienda Windows con el identificador de campaña personalizada.
-   Si el cliente adquiere la aplicación en un equipo o dispositivo distintos desde el que hizo clic en la dirección URL de la Tienda Windows con el identificador de campaña personalizada, la conversión solo se contará si el cliente tiene una cuenta Microsoft reconocida.

> **Nota**  Para las adquisiciones de aplicaciones que se cuentan como conversiones para una campaña personalizada, toda compra de complementos en esa aplicación también se cuenta como una conversión para la misma campaña personalizada.
     

Para considerar que existe conversión al recuperar mediante programación el identificador de campaña asociado con la aplicación, deben cumplirse las siguientes condiciones:

-   Un cliente con o sin una cuenta Microsoft reconocida hace clic en la dirección URL de una aplicación, dirección que contiene el identificador de una campaña personalizada, y se le redirige a la página de la aplicación en la Tienda Windows. Después de hacer clic en la dirección URL, el cliente adquiere la aplicación durante la misma vista de página de la Tienda Windows.
-   Si el cliente abandona la página y luego vuelve (ya sea en el mismo equipo o dispositivo o en otro diferente si tiene una cuenta Microsoft reconocida) dentro de las 24 horas siguientes y adquiere la aplicación, esto se califica como conversión para el [informe Canales y conversión](channels-and-conversions-report.md). Sin embargo, no se calificará como conversión si recuperas mediante programación el identificador de campaña.

## <a name="embed-a-custom-campaign-id-to-your-apps-windows-store-page-url"></a>Incrustar un identificador de campaña personalizada en la dirección URL de la página de tu aplicación en la Tienda Windows


Para crear una dirección URL de la página de tu aplicación en la Tienda Windows con un identificador de campaña personalizada:

1.  Crea una cadena de identificador para la campaña personalizada. Esta cadena puede contener hasta 100 caracteres, aunque recomendamos definir identificadores de campaña cortos para que sean fáciles de identificar. Ten en cuenta que la cadena del identificador de campaña puede ser visible para otros desarrolladores dentro de un informe Canales y conversiones. Esto puede ocurrir cuando un cliente hace clic en el identificador de campaña para acceder a la Tienda, pero acaba comprando la aplicación de otro desarrollador dentro de la misma sesión. Aunque en este caso otro desarrollador podrá ver tu nombre del identificador de campaña, ese desarrollador solo verá cuántas conversiones de su propia aplicación provienen de un clic inicial en tu identificador de campaña. No podrán ver los datos sobre el número de usuarios que compraron tu aplicación a partir de hacer clic en tu identificador de campaña.
2.  Obtén la dirección URL de la página de tu aplicación en la Tienda Windows en formato HTML o de protocolo. La dirección URL en formato HTML está disponible en la página [**Identidad de la aplicación** en el panel del Centro de desarrollo](link-to-your-app.md).
    -   Usa el formato HTTP si quieres que los clientes naveguen a la página de la Tienda Windows de tu aplicación en un explorador (esta dirección URL también iniciará la aplicación de la Tienda Windows en la descripción de la aplicación, en caso de que la aplicación de la Tienda Windows esté instalada). Esta dirección URL tiene el formato **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**. Por ejemplo, la dirección URL HTTP para Skype es `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`. Ten en cuenta que las direcciones URL en formato HTTP se pueden usar para navegar a la Tienda Windows en un explorador en equipos y tabletas que ejecuten Windows 7 y versiones posteriores, así como en teléfonos con Windows Phone 8 y posterior.
    -   Usa el formato de protocolo si la promoción de la aplicación es a través de otras aplicaciones de Windows que se ejecutan en un equipo o dispositivo con la aplicación de la Tienda Windows instalada y quieres que los clientes abran la página de la aplicación en la aplicación de la Tienda Windows. Esta dirección URL tiene el formato **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**. Por ejemplo, es la dirección URL de protocolo de Skype es `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.
3.  Anexa la siguiente cadena al final de la dirección URL de la aplicación:
    -   Para una dirección URL en formato HTTP, anexa **`?cid=*my custom campaign ID*`**. Por ejemplo, si Skype presenta un identificador de campaña con el valor **custom\_campaign**, la nueva dirección URL HTTP que incluye el identificador de campaña sería: `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`.
    -   Para una dirección URL en formato de protocolo, anexa **`&cid=*my custom campaign ID*`**. Por ejemplo, si Skype presenta un identificador de campaña con el valor **custom\_campaign**, la nueva dirección URL de protocolo que incluye el identificador de campaña sería: `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`.

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>Recuperar mediante programación el identificador de campaña personalizada de una aplicación


Si la tuya es una aplicación para UWP, puedes recuperar mediante programación el identificador de campaña personalizada que tiene asociado. Para ello, usa el método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445). Este método hace posibles muchos escenarios de análisis y monetización. Por ejemplo, puedes descubrir si el usuario actual compró la aplicación tras descubrirla gracias a tu campaña en Facebook y personalizar la experiencia en consecuencia. Si estás usando un proveedor de marketing de aplicaciones de terceros, puedes enviar datos de vuelta al proveedor.

> **Nota**  El método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) solo devuelve una cadena con el identificador de campaña si el cliente hace clic en la dirección URL con el identificador de campaña incrustado, se redirige a la página de tu aplicación en la Tienda Windows y luego instala la adquiere sin salir de esta página. Si el usuario abandona la página y más tarde vuelve e adquirir la aplicación, no se calificará como conversión al usar **GetAppPurchaseCampaignIdAsync**. Para obtener más información, consulta [Qué son las conversiones](#conversions) en este tema.

 

El siguiente ejemplo muestra cómo usar el método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) para recuperar el identificador de campaña personalizada de la aplicación. Si la aplicación no se ha adquirido como parte de una conversión correcta, este método devuelve una cadena vacía.

``` csharp
string campaignId = await CurrentApp.GetAppPurchaseCampaignIdAsync();
```

``` cpp
HString campaignId;
HRESULT hr = CurrentApp::GetAppPurchaseCampaignIdAsync(campaignId.GetAddressOf());
```

El método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) accede a los datos de la Tienda Windows. Sigue estas instrucciones cuando uses este método:

-   Encapsula esta llamada al método en una operación asincrónica para permitir que la llamada se complete.
-   Si la aplicación todavía no está publicada en la Tienda Windows y estás probando la campaña personalizada, usa el método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) de la clase [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) en lugar de la clase [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765). Sigue estas instrucciones:
    -   Agrega un elemento **AppPurchaseCampaignId** al archivo WindowsStoreProxy.xml, como se muestra en el siguiente ejemplo. Asigna el valor del elemento al identificador de campaña personalizada. Cuando ejecutes la aplicación, el método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) siempre devolverá este valor. Para obtener más información sobre el archivo WindowsStoreProxy.xml, consulta la documentación de [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766).

    ```        XML
    <CurrentApp>
        ....
        <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
    </CurrentApp>
    ```

    -   Para cambiar con facilidad entre el uso de [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) y [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766), recomendamos agregar las siguientes instrucciones al código para definir un alias de **Store** y luego califica las llamadas a [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) con el alias de **Store**.

    ```        CSharp
    #if DEBUG
            using Store = Windows.ApplicationMode.Store.CurrentAppSimulator;
            #else
            using Store = Windows.ApplicationMode.Store.CurrentApp;
            #endif   
    ```

## <a name="test-your-custom-campaign"></a>Prueba tu campaña personalizada


Antes de que promociones la dirección URL de una campaña personalizada, te recomendamos que pruebes la campaña del siguiente modo:

1.  Inicia sesión con tu cuenta de Microsoft en el equipo o dispositivo que uses para las pruebas.
2.  Haz clic en la dirección URL de la campaña personalizada. Asegúrate de que la Tienda Windows carga la página de la aplicación correctamente y después cierra la aplicación de la Tienda Windows o la página del explorador.
3.  Haz clic en la dirección URL varias veces más y cierra la aplicación de la Tienda Windows o la página del explorador después de cada visita. Durante una de las visitas, adquiere la aplicación para generar una conversión. Cuenta el número total de veces que has hecho clic en la dirección URL.
4.  Si estás recuperando mediante programación el identificador de campaña personalizada de tu aplicación, prueba el funcionamiento de la operación con el método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) de la clase [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) en lugar de la clase [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765).

 

 

