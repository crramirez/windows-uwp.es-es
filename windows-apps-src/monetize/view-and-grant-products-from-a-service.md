---
author: mcleanbyron
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: "Si tienes un catálogo de aplicaciones y complementos, puedes usar la API de colecciones y la de compras de la Tienda Windows para obtener acceso a la información de propiedad de estos productos desde tus servicios."
title: Ver y conceder productos desde un servicio
translationtype: Human Translation
ms.sourcegitcommit: 819843c8ba1e4a073f70f7de36fe98dd4087cdc6
ms.openlocfilehash: 86d7383a7288434ec613244e4c2ba610889a45fb

---

# Ver y conceder productos desde un servicio

Si tienes un catálogo de aplicaciones y complementos (también conocidos como productos desde la aplicación o IAP), puedes usar la *API de colecciones de la Tienda Windows* y la *API de compras de la Tienda Windows* para obtener acceso a la información de propiedad de estos productos desde tus servicios.

Estas API constan de métodos REST diseñados para que los desarrolladores los usen con los catálogos de complementos compatibles con los servicios multiplataforma. Estas API te permiten hacer lo siguiente:

-   API de colecciones de la Tienda Windows: consulta aplicaciones y complementos pertenecientes a un usuario determinado o notifica un producto consumible como cumplido.
-   API de compras de la Tienda Windows: concede una aplicación o un complemento gratuitos a un usuario determinado.

## Uso de la API de colecciones de la Tienda Windows y la API de compras de la Tienda Windows


La API de colecciones y la API de compras de la Tienda Windows usan la autenticación de Azure Active Directory (Azure AD) para acceder a la información de propiedad del cliente. Antes de llamar a estas API, debes aplicar los metadatos de Azure AD a la aplicación en el panel del Centro de desarrollo de Windows y generar varios tokens y claves de acceso obligatorios. Los siguientes pasos describen el proceso de principio a fin:

1.  [Configura una aplicación web en Azure AD](#step-1). Esta aplicación representa los servicios multiplataforma en el contexto de Azure AD.
2.  [Asocia tu identificador de cliente de Azure AD con la aplicación en el panel del Centro de desarrollo de Windows](#step-2).
3.  En el servicio ,[genera tokens de acceso de Azure AD](#step-3) que representen tu identidad de publicador.
4.  En el código de cliente de la aplicación de Windows, [genera una clave de id. de la Tienda Windows](#step-4) que represente la identidad del usuario actual y pasa la clave de id. de la Tienda Windows de nuevo al servicio.
5.  Cuando tengas el token de acceso de Azure AD y clave del id. de la Tienda Windows necesarios, [llama a la API de colecciones o la API de compras de la Tienda Windows desde tu servicio](#step-5).

En las siguientes secciones se proporcionan más detalles sobre cada uno de estos pasos.

<span id="step-1"/>
### Paso 1: Configurar una aplicación web en Azure AD

1.  Sigue las instrucciones del artículo [Integración de aplicaciones con Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502) para agregar una aplicación web a Azure AD.

    > **Nota**&nbsp;&nbsp;En la página **Proporcione información sobre su aplicación**, asegúrate de elegir **Aplicación web y/o API web**. Esto es necesario para que puedas obtener una clave (también llamada una *secreto de cliente*) de la aplicación. Para llamar a la API de colecciones o la API de compras de la Tienda Windows, debes proporcionar un secreto de cliente cuando solicites un token de acceso de Azure AD en un paso posterior.

2.  En el [Portal de administración de Azure](http://manage.windowsazure.com/), ve a **Active Directory**. Selecciona el directorio, haz clic en la pestaña **Aplicaciones** de la parte superior y luego selecciona la aplicación.
3.  Haz clic en la pestaña **Configurar**. En esta pestaña, obtén el identificador de cliente de tu aplicación y pide una clave (esto se denomina una *clave secreta de cliente* en pasos posteriores).
4.  En la parte inferior de la pantalla, haz clic en **Administrar manifiesto**. Descarga el manifiesto de la aplicación de Azure AD y reemplaza la sección `"identifierUris"` con el texto siguiente.

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    Estas cadenas representan los públicos que admite tu aplicación. En un paso posterior, crearás tokens de acceso de Azure AD asociados a cada uno de estos valores de público. Para obtener más información sobre cómo descargar el manifiesto de la aplicación, consulta el tema [Descripción del manifiesto de aplicación de Azure Active Directory]( http://go.microsoft.com/fwlink/?LinkId=722500).

5.  Guarda el manifiesto de la aplicación y cárgalo en la aplicación en el [Portal de administración de Azure](http://manage.windowsazure.com/).

<span id="step-2"/>
### Paso 2: Asocia tu identificador de cliente de Azure AD con la aplicación en el panel del Centro de desarrollo de Windows

La API de colecciones y la API de compras de la Tienda Windows solo proporcionan acceso a la información de propiedad de un usuario en las aplicaciones y los complementos que hayas asociado con el identificador de cliente de Azure AD.

1.  Inicia sesión en el [panel del Centro de desarrollo de Windows](https://dev.windows.com/overview) y selecciona tu aplicación.
2.  Ve a la página **Servicios** &gt; **Colecciones y compras de productos** y escribe tu identificador de cliente de Azure AD en uno de los campos disponibles.

<span id="step-3"/>
### Paso 3: Recuperar los tokens de acceso de Azure AD

Para poder recuperar una clave de id. de la Tienda Windows o llamar a la API de colecciones o la API de compras de la Tienda Windows, el servicio debe solicitar primero tres tokens de acceso de Azure AD que representen tu identidad de publicador. Cada uno de estos tokens de acceso está asociado con un URI de público diferente y cada token se usará con una llamada de API diferente. La duración de cada uno de estos tokens es de 60 minutos y se pueden actualizar después de su expiración.

Para crear los tokens de acceso, usa la API de OAuth 2.0 en tu servicio. Para ello, sigue las instrucciones de [Llamadas de servicio a servicio utilizando las credenciales del cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/). Para cada token, especifica los siguientes datos de parámetros:

-   Para los parámetros *client\_id* y *client\_secret*, especifica el identificador de cliente y la clave secreta de cliente de la aplicación, según se obtienen del [Portal de administración de Azure](http://manage.windowsazure.com/). Ambos parámetros se necesitan para generar un token de acceso con el nivel de autenticación que requieren la API de colecciones o la API de compras de la Tienda Windows.
-   Para el parámetro *resource*, especifica uno de los siguientes URI de identificador de aplicación (estos son los mismos URI que agregaste anteriormente a la sección `"identifierUris"` del manifiesto de la aplicación). Al final de este proceso, deberías tener tres tokens de acceso, cada uno de los cuales deberá tener uno de estos URI de identificador de aplicación asociado:
    -   `https://onestore.microsoft.com/b2b/keys/create/collections`: en un paso posterior, usarás el token de acceso que crees con este URI para solicitar una clave de id. de la Tienda Windows que se pueda usar con la API de colecciones de la Tienda Windows.
    -   `https://onestore.microsoft.com/b2b/keys/create/purchase`: en un paso posterior, usarás el token de acceso que crees con este URI para solicitar una clave de id. de la Tienda Windows que se pueda usar con la API de compras de la Tienda Windows.
    -   `https://onestore.microsoft.com`: En un paso posterior, usarás el token de acceso que crees con este URI en llamadas directas a la API de colecciones o la API de compra de la Tienda Windows.

    > **Importante**&nbsp;&nbsp;Usa el público de `https://onestore.microsoft.com` solo con los token de acceso que tienes almacenados de forma segura en tu servicio. La exposición de los tokens de acceso con este público fuera del servicio puede provocar que el servicio sea vulnerable a los ataques de reproducción.

Una vez expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens). Para obtener más detalles sobre la estructura de un token de acceso, consulta [Supported Token and Claim Types (Tipos de notificaciones y tokens admitidos)](http://go.microsoft.com/fwlink/?LinkId=722501).

> **Importante**&nbsp;&nbsp;Debes crear tokens de acceso de Azure AD solamente en el contexto del servicio, no en la aplicación. El secreto de cliente podría verse comprometido si se envía a la aplicación.

<span id="step-4"/>
### Paso 4: Generar una clave de id. de la Tienda Windows a partir del código de cliente en la aplicación

Antes de llamar a la API de colecciones o la API de compras de la Tienda Windows, el servicio debe obtener una clave de id. de la Tienda Windows. Este es un token web JSON (JWT) que representa la identidad del usuario a cuya información de propiedad del producto deseas acceder. Para obtener más información acerca de las notificaciones de esta clave, consulta [Notificaciones en una clave de identificador de la Tienda Windows](#claims).

Actualmente, la única manera de obtener una clave de id. de la Tienda Windows es mediante una llamada a una API de la Plataforma universal de Windows (UWP) desde el código de cliente de la aplicación para recuperar la identidad del usuario que tiene una sesión iniciada actualmente en la Tienda Windows. Para generar una clave de Id. de la Tienda Windows:

1.  Pasa uno de los siguientes tokens de acceso desde el servicio a tu aplicación cliente:

  * Para obtener una clave de id. de la Tienda Windows que se pueda usar con la API de colecciones de la Tienda Windows, pasa el token de acceso de Azure AD que creaste con el URI de público de `https://onestore.microsoft.com/b2b/keys/create/collections`.

  * Para obtener una clave de id. de la Tienda Windows que se pueda usar con la API de compras de la Tienda Windows, pasa el token de acceso de Azure AD que hayas creado con el URI de público de `https://onestore.microsoft.com/b2b/keys/create/purchase`.

2.  En el código de la aplicación, llama a uno de los siguientes métodos para recuperar una clave de id. de la Tienda Windows.

  * Si la aplicación usa la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) en el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para administrar las compras desde la aplicación, usa el método [StoreContext.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomercollectionsidasync.aspx) si tienes previsto usar la API de colecciones de la Tienda Windows, o usa el método [StoreContext.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomerpurchaseidasync.aspx) si tienes previsto usar la API de compras de la Tienda Windows.

  * Si la aplicación usa la clase [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) en el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) para administrar las compras desde la aplicación, usa el método [CurrentApp.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608674) si tienes previsto usar la API de colecciones de la Tienda Windows, o usa el método [CurrentApp.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608675) si tienes previsto usar la API de compras de la Tienda Windows.

    Para cada uno de los métodos, pasa el token de acceso de Azure AD al parámetro *serviceTicket*. También puedes pasar un identificador al parámetro *publisherUserId* que identifica al usuario actual en el contexto de tus servicios. Si mantienes identificadores de usuario para tus servicios, puedes usar este parámetro para correlacionar estos identificadores de usuario con las llamadas que realices a la API de colecciones o la API de compras de la Tienda Windows.

    >**Nota**&nbsp;&nbsp;Para obtener más información sobre las diferencias entre los espacios de nombres **Windows.Services.Store** y **Windows.ApplicationModel.Store**, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md).

3.  Después de que la aplicación recupere correctamente una clave de id. de la Tienda Windows, pasa la clave a tu servicio.

> **Nota**&nbsp;&nbsp;Cada clave de id. de la Tienda Windows es válida durante 90 días. Puedes [renovar la clave](renew-a-windows-store-id-key.md) cuando expire. Te recomendamos que renueves tus claves de id. de la Tienda Windows en lugar de crear claves nuevas.

<span id="step-5"/>
### Paso 5: Llamar a la API de colecciones o la API de compras de la Tienda Windows desde el servicio

Cuando el servicio disponga de una clave de id. de la Tienda Windows que permita acceder a la información de propiedad del producto de un usuario específico, el servicio puede llamar a la API de colecciones o la API de compras de la Tienda Windows. Sigue las instrucciones que se aplican a tu escenario:

-   [Consultar productos](query-for-products.md)
-   [Notificar productos consumibles como completados](report-consumable-products-as-fulfilled.md)
-   [Conceder productos gratuitos](grant-free-products.md)

Para cada escenario, pasa la siguiente información a la API:

-   El token de acceso de Azure AD que creaste anteriormente con el URI de público `https://onestore.microsoft.com`. Este token representa tu identidad de publicador. Pasa este token en el encabezado de la solicitud.
-   La clave de id. de la Tienda Windows que has recuperado a partir del código de cliente en la aplicación. Esta clave representa la identidad del usuario a cuya información de propiedad del producto deseas acceder.

<span id="claims"/>
## Notificaciones en una clave de id. de la Tienda Windows

Una clave de id. de la Tienda Windows es un token web JSON (JWT) que representa la identidad del usuario a cuya información de propiedad del producto deseas acceder. Si se descodifica mediante Base64, una clave de Id. de la Tienda Windows contiene las siguientes notificaciones.

| Nombre de la notificación            | Descripción          |
|---------------|-------------|
| iat                 | Identifica la hora en que se emitió la clave. Esta notificación puede usarse para determinar la edad del token. Este valor se expresa como la época.           |
| iss                    | Identifica al emisor. Tiene el mismo valor que la notificación *aud*.      |
| aud           | Identifica al público. Deber ser uno de los siguientes valores: `https://collections.mp.microsoft.com/v6.0/keys` o `https://purchase.mp.microsoft.com/v6.0/keys`.  |
| exp        | Identifica la hora de expiración en que (o a partir de la cual) la clave ya no se aceptará para procesar nada, excepto para renovar claves. El valor de esta notificación se expresa como la época.       |
| nbf                | Identifica la hora a la que el token se aceptará para su procesamiento. El valor de esta notificación se expresa como el tiempo de la época. |
| `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`   | El identificador de cliente que identifica al desarrollador.     |
| `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`    | Una carga opaca (cifrada y codificada con Base64) que contiene información destinada solo al uso por parte de servicios de la Tienda Windows.   |
| `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`     | Un identificador de usuario que identifica al usuario actual en el contexto de tus servicios. Es el mismo valor que pasas al parámetro opcional *publisherUserId* del [método que usas para generar la clave](view-and-grant-products-from-a-service.md#step-4). |
| `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri` | El URI que puedes usar para renovar la clave.                                                                                                                 
Este es un ejemplo de un encabezado de clave de id. de la Tienda Windows descodificada.

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

Este es un ejemplo de un conjunto de notificaciones de clave de id. de la Tienda Windows descodificada.

```json
{
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId": "1d5773695a3b44928227393bfef1e13d",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload": "ZdcOq0/N2rjytCRzCHSqnfczv3f0343wfSydx7hghfu0snWzMqyoAGy5DSJ5rMSsKoQFAccs1iNlwlGrX+/eIwh/VlUhLrncyP8c18mNAzAGK+lTAd2oiMQWRRAZxPwGrJrwiq2fTq5NOVDnQS9Za6/GdRjeiQrv6c0x+WNKxSQ7LV/uH1x+IEhYVtDu53GiXIwekltwaV6EkQGphYy7tbNsW2GqxgcoLLMUVOsQjI+FYBA3MdQpalV/aFN4UrJDkMWJBnmz3vrxBNGEApLWTS4Bd3cMswXsV9m+VhOEfnv+6PrL2jq8OZFoF3FUUpY8Fet2DfFr6xjZs3CBS1095J2yyNFWKBZxAXXNjn+zkvqqiVRjjkjNajhuaNKJk4MGHfk2rZiMy/aosyaEpCyncdisHVSx/S4JwIuxTnfnlY24vS0OXy7mFiZjjB8qL03cLsBXM4utCyXSIggb90GAx0+EFlVoJD7+ZKlm1M90xO/QSMDlrzFyuqcXXDBOnt7rPynPTrOZLVF+ODI5HhWEqArkVnc5MYnrZD06YEwClmTDkHQcxCvU+XUEvTbEk69qR2sfnuXV4cJRRWseUTfYoGyuxkQ2eWAAI1BXGxYECIaAnWF0W6ThweL5ZZDdadW9Ug5U3fZd4WxiDlB/EZ3aTy8kYXTW4Uo0adTkCmdLibw=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId": "infusQMLaYCrgtC0d/SZWoPB4FqLEwHXgZFuMJ6TuTY=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri": "https://collections.mp.microsoft.com/v6.0/b2b/keys/renew",
    "iat": 1442395542,
    "iss": "https://collections.mp.microsoft.com/v6.0/keys",
    "aud": "https://collections.mp.microsoft.com/v6.0/keys",
    "exp": 1450171541,
    "nbf": 1442391941
}
```

## Temas relacionados

* [Consultar productos](query-for-products.md)
* [Notificar productos consumibles como completados](report-consumable-products-as-fulfilled.md)
* [Conceder productos gratuitos](grant-free-products.md)
* [Renovar una clave de id. de la Tienda Windows](renew-a-windows-store-id-key.md)
* [Integrar aplicaciones con Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502)
* [Descripción del manifiesto de aplicación de Azure Active Directory]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [Tipos de notificaciones y tokens admitidos](http://go.microsoft.com/fwlink/?LinkId=722501)
 

 



<!--HONumber=Nov16_HO1-->


