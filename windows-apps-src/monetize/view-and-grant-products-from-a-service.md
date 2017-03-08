---
author: mcleanbyron
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: "Si tienes un catálogo de aplicaciones y complementos, puedes usar la API de colecciones de la Tienda Windows y la API de compras de la Tienda Windows para obtener acceso a la información de propiedad de estos productos desde tus servicios."
title: Administrar los derechos de producto de un servicio
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de colección de la Tienda Windows, API de compra de la Tienda Windows, ver productos, conceder productos"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7f4f74c887509e772fd01dbdcfe28d86c583fbc1
ms.lasthandoff: 02/07/2017

---

# <a name="manage-product-entitlements-from-a-service"></a>Administrar los derechos de producto de un servicio

Si tienes un catálogo de aplicaciones y complementos (también conocidos como productos desde la aplicación o IAP), puedes usar la *API de colecciones de la Tienda Windows* y la *API de compras de la Tienda Windows* para obtener acceso a la información de derecho de estos productos desde tus servicios. Un *derecho* representa el derecho de un cliente de usar una aplicación o complemento que se publica a través de la Tienda Windows.

Estas API constan de métodos REST diseñados para que los desarrolladores los usen con los catálogos de complementos compatibles con los servicios multiplataforma. Estas API te permiten hacer lo siguiente:

-   API de colecciones de la Tienda Windows: [Consulta productos pertenecientes a un usuario](query-for-products.md) y [notifica un producto como cumplido](report-consumable-products-as-fulfilled.md).
-   API de compra de la Tienda Windows: [concede un producto gratuito a un usuario](grant-free-products.md).

>**Nota**&nbsp;&nbsp;La API de colecciones y la API de compras de la Tienda Windows usan la autenticación de Azure Active Directory (Azure AD) para acceder a la información de propiedad del cliente. Para usar estas API, tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para el directorio. Si ya usas Office 365 u otros servicios empresariales de Microsoft, ya tienes un directorio de Azure AD.

## <a name="overview"></a>Introducción

Los siguientes pasos describen el proceso de principio a fin para usar la API de colecciones de la Tienda Windows y la API de compra:

1.  [Configura una aplicación web en Azure AD](#step-1).
2.  [Asocia tu identificador de cliente de Azure AD con la aplicación en el panel del Centro de desarrollo de Windows](#step-2).
3.  En el servicio, [genera tokens de acceso de Azure AD](#step-3) que representen tu identidad de publicador.
4.  En el código de cliente de la aplicación de Windows, [crea una clave de id. de la Tienda Windows](#step-4) que represente la identidad del usuario actual y pasa la clave de id. de la Tienda Windows de nuevo al servicio.
5.  Cuando tengas el token de acceso de Azure AD y clave del id. de la Tienda Windows necesarios, [llama a la API de colecciones o la API de compras de la Tienda Windows desde tu servicio](#step-5).

En las siguientes secciones se proporcionan más detalles sobre cada uno de estos pasos.

<span id="step-1"/>
### <a name="step-1-configure-a-web-application-in-azure-ad"></a>Paso 1: Configurar una aplicación web en Azure AD

Antes de poder usar la API de colecciones de la Tienda Windows o la API de compras, debes crear una aplicación Web de Azure AD, recuperar el identificador de inquilino y el identificador de cliente de la aplicación y generar una clave. La aplicación de Azure AD representa la aplicación o el servicio desde donde quieres originar la llamada a la API de colecciones de la Tienda Windows o la API de compras. Necesitas el identificador de inquilino, de cliente y la clave para obtener un token de acceso de Azure AD que se pasa a la API.

>**Nota**&nbsp;&nbsp;Solo tienes que realizar las tareas en esta sección una vez. Después de actualizar el manifiesto de la aplicación de Azure AD y de tener el identificador de inquilino, el identificador de cliente y el secreto de cliente, puedes volver a usar estos valores cuando necesites crear un nuevo token de acceso a Azure AD.

1.  Sigue las instrucciones del artículo [Integración de aplicaciones con Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502) para agregar una aplicación web a Azure AD.

    > **Nota**&nbsp;&nbsp;En la página **Proporciona información sobre tu aplicación**, asegúrate de elegir **Aplicación web y/o API web**. Esto es necesario para que puedas recuperar una clave (también llamada una *secreto de cliente*) de la aplicación. Para llamar a la API de colecciones o la API de compras de la Tienda Windows, debes proporcionar un secreto de cliente cuando solicites un token de acceso de Azure AD en un paso posterior.

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

    Estas cadenas representan los públicos que admite tu aplicación. En un paso posterior, crearás tokens de acceso de Azure AD asociados a cada uno de estos valores de público. Para obtener más información sobre cómo descargar el manifiesto de la aplicación, consulta el tema [Descripción del manifiesto de aplicación de Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722500).

5.  Guarda el manifiesto de la aplicación y cárgalo en la aplicación en el [Portal de administración de Azure](http://manage.windowsazure.com/).

<span id="step-2"/>
### <a name="step-2-associate-your-azure-ad-client-id-with-your-app-in-windows-dev-center"></a>Paso 2: Asocia tu identificador de cliente de Azure AD con tu aplicación en el Centro de desarrollo de Windows

Para que la API de colecciones de la Tienda Windows o la API de compra para funcione en una aplicación o un complemento, tienes que asociar tu identificador de cliente de Azure AD con la aplicación en el panel del Centro de desarrollo de Windows.

>**Nota**&nbsp;&nbsp;Solo debes realizar esta tarea una vez.

1.  Inicia sesión en el [panel del Centro de desarrollo de Windows](https://dev.windows.com/overview) y selecciona tu aplicación.
2.  Ve a la página **Servicios** &gt; **Colecciones y compras de productos** y escribe tu identificador de cliente de Azure AD en uno de los campos disponibles.

<span id="step-3"/>
### <a name="step-3-create-azure-ad-access-tokens"></a>Paso 3: Crea tokens de acceso de Azure AD

Para poder recuperar una clave de id. de la Tienda Windows o llamar a la API de colecciones o la API de compras de la Tienda Windows, el servicio debe crear varios tokens de acceso de Azure AD diferentes que representen tu identidad de publicador. Cada token se usará con una API diferente. La duración de cada uno de estos tokens es de 60 minutos y se pueden actualizar después de su expiración.

<span id="access-tokens" />
#### <a name="understanding-the-different-tokens-and-audience-uris"></a>Descripción de los diferentes tokens y los URI de público

Dependiendo de los métodos que quieres llamar en la API de colecciones de la Tienda Windows o la API de compras, debes crear dos o tres tokens diferentes. Cada token de acceso está asociado con un URI de público diferente (estos son los mismos URI que agregaste anteriormente a la sección `"identifierUris"` del manifiesto de la aplicación de Azure AD).

  * En todos los casos, debes crear un token con la URI de público de `https://onestore.microsoft.com`. En un paso posterior, pasará este token al encabezado **Autorización** de métodos en la API de colecciones de la Tienda Windows o la API de compras.

  > **Importante**&nbsp;&nbsp;Usa el público de `https://onestore.microsoft.com` solo con los token de acceso que tienes almacenados de forma segura en tu servicio. La exposición de los tokens de acceso con este público fuera del servicio puede provocar que el servicio sea vulnerable a los ataques de reproducción.

  * Si quieres llamar un método en la API de colecciones de la Tienda Windows para [consultar productos pertenecientes a un usuario](query-for-products.md) o [notificar un producto consumible como completado](report-consumable-products-as-fulfilled.md), también debes crear un token con la URI de público `https://onestore.microsoft.com/b2b/keys/create/collections`. En un paso posterior, pasarás este token a un método de cliente en el Windows SDK para solicitar una clave de identificador de la Tienda Windows que se pueda usar con la API de colecciones de la Tienda Windows.

  * Si quieres llamar un método en la API de compra de la Tienda Windows para [conceder un producto gratuito a un usuario](grant-free-products.md), también debes crear un token con la URI de público `https://onestore.microsoft.com/b2b/keys/create/purchase`. En un paso posterior, pasarás este token a un método de cliente en el Windows SDK para solicitar una clave de identificador de la Tienda Windows que se pueda usar con la API de compra de la Tienda Windows.

<span />
#### <a name="how-to-create-the-tokens"></a>Cómo crear los tokens

Para crear los tokens de acceso, usa la API de OAuth 2.0 en tu servicio siguiendo las instrucciones de [Llamadas de servicio a servicio utilizando las credenciales del cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service) para enviar un HTTP POST al punto de conexión ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Este es un ejemplo de solicitud.

``` syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://onestore.microsoft.com
```

Para cada token, especifica los siguientes datos de parámetros:

* Para los parámetros *client\_id* y *client\_secret*, especifica el identificador de cliente y la clave secreta de cliente de la aplicación que recuperó del [Portal de administración de Azure](http://manage.windowsazure.com). Ambos parámetros se necesitan para crear un token de acceso con el nivel de autenticación que requieren la API de colecciones o la API de compras de la Tienda Windows.

* Para el parámetro *recursos*, especifica uno de los URI de público enumerados en la [sección anterior](#access-tokens), según el tipo de token de acceso que está creando.

Una vez que expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens). Para obtener más detalles sobre la estructura de un token de acceso, consulta [Supported Token and Claim Types (Tipos de notificaciones y tokens admitidos)](http://go.microsoft.com/fwlink/?LinkId=722501).

> **Importante**&nbsp;&nbsp;Debes crear tokens de acceso de Azure AD solamente en el contexto del servicio, no en la aplicación. El secreto de cliente podría verse comprometido si se envía a la aplicación.

<span id="step-4"/>
### <a name="step-4-create-a-windows-store-id-key"></a>Paso 4: Crea una clave de identificador de la Tienda Windows

Antes de llamar un método en la API de colecciones o la API de compras de la Tienda Windows, el servicio debe crear una clave de identificador de la Tienda Windows. Este es un token web JSON (JWT) que representa la identidad del usuario a cuya información de propiedad del producto deseas acceder. Para obtener más información acerca de las notificaciones de esta clave, consulta [Notificaciones en una clave de identificador de la Tienda Windows](#claims).

Actualmente, la única manera de crear una clave de identificador de la Tienda Windows es mediante una llamada a una API de la Plataforma universal de Windows (UWP) desde el código de la parte del cliente de la aplicación. La clave generada representa la identidad del usuario que actualmente está conectado a la Tienda Windows en el dispositivo.

> **Nota**&nbsp;&nbsp;Cada clave de id. de la Tienda Windows es válida durante 90 días. Puedes [renovar la clave](renew-a-windows-store-id-key.md) cuando expire. Te recomendamos que renueves tus claves de id. de la Tienda Windows en lugar de crear claves nuevas.

<span />
#### <a name="to-create-a-windows-store-id-key-for-the-windows-store-collection-api"></a>Para crear una clave de identificador de la Tienda Windows para la API de colecciones de la Tienda Windows

Sigue estos pasos para crear una clave de identificador de la Tienda Windows que puedas usar con la API de colecciones de la Tienda Windows para [consultar productos pertenecientes a un usuario](query-for-products.md) o [notificar un producto consumible como completado](report-consumable-products-as-fulfilled.md).

1.  Pasa el token de acceso de Azure AD que creaste con el URI de público `https://onestore.microsoft.com/b2b/keys/create/collections` desde el servicio a tu aplicación de cliente.

2.  En el código de la aplicación, llama a uno de estos métodos para recuperar una clave de id. de la Tienda Windows.

  * Si la aplicación usa la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) en el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para administrar las compras desde la aplicación, usa el método [StoreContext.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomercollectionsidasync.aspx).

  * Si la aplicación usa la clase [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) en el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) para administrar las compras desde la aplicación, usa el método [CurrentApp.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608674).

  Pasa el token de acceso de Azure AD al parámetro *serviceTicket* del método. También puedes pasar un identificador al parámetro *publisherUserId* que identifica al usuario actual en el contexto de tus servicios. Si mantienes identificadores de usuario para tus servicios, puedes usar este parámetro para correlacionar estos identificadores de usuario con las llamadas que realices a la API de colecciones de la Tienda Windows.

3.  Después de que la aplicación recupere correctamente una clave de id. de la Tienda Windows, pasa la clave a tu servicio.

<span />
#### <a name="to-create-a-windows-store-id-key-for-the-windows-store-purchase-api"></a>Para crear una clave de identificador de la Tienda Windows para la API de compras de la Tienda Windows

Sigue estos pasos para crear una clave de identificador de la Tienda Windows que se puede usar con la API de compras de la Tienda Windows para [conceder un producto gratuito a un usuario](grant-free-products.md).

1.  Pasa el token de acceso de Azure AD que creaste con el URI de público `https://onestore.microsoft.com/b2b/keys/create/purchase` desde el servicio a tu aplicación de cliente.

2.  En el código de la aplicación, llama a uno de estos métodos para recuperar una clave de id. de la Tienda Windows.

  * Si la aplicación usa la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) en el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para administrar las compras desde la aplicación, usa el método [StoreContext.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomerpurchaseidasync.aspx).

  * Si la aplicación usa la clase [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) en el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) para administrar las compras desde la aplicación, usa el método [CurrentApp.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608675).

  Pasa el token de acceso de Azure AD al parámetro *serviceTicket* del método. También puedes pasar un identificador al parámetro *publisherUserId* que identifica al usuario actual en el contexto de tus servicios. Si mantienes identificadores de usuario para tus servicios, puedes usar este parámetro para correlacionar estos identificadores de usuario con las llamadas que realices a la API de compras de la Tienda Windows.

3.  Después de que la aplicación recupere correctamente una clave de id. de la Tienda Windows, pasa la clave a tu servicio.

<span id="step-5"/>
### <a name="step-5-call-the-windows-store-collection-api-or-purchase-api-from-your-service"></a>Paso 5: Llama a la API de colecciones o la API de compras de la Tienda Windows desde el servicio

Cuando el servicio disponga de una clave de id. de la Tienda Windows que permita acceder a la información de propiedad del producto de un usuario específico, el servicio puede llamar a la API de colecciones o la API de compras de la Tienda Windows siguiendo estas instrucciones:

* [Consultar productos](query-for-products.md)
* [Notificar productos consumibles como completados](report-consumable-products-as-fulfilled.md)
* [Conceder productos gratuitos](grant-free-products.md)

Para cada escenario, pasa la siguiente información a la API:

-   El token de acceso de Azure AD que creaste anteriormente con el URI de público `https://onestore.microsoft.com`. Este token representa tu identidad de publicador. Pasa este token en el encabezado de la solicitud.
-   La clave de id. de la Tienda Windows que has recuperado a partir del código de cliente en la aplicación. Esta clave representa la identidad del usuario a cuya información de propiedad del producto deseas acceder.

<span id="claims"/>
## <a name="claims-in-a-windows-store-id-key"></a>Notificaciones en una clave de id. de la Tienda Windows

Una clave de id. de la Tienda Windows es un token web JSON (JWT) que representa la identidad del usuario a cuya información de propiedad del producto deseas acceder. Si se descodifica mediante Base64, una clave de Id. de la Tienda Windows contiene las siguientes notificaciones.

* `iat`:&nbsp;&nbsp;&nbsp;Identifica la hora en que se emitió la clave. Esta notificación puede usarse para determinar la edad del token. Este valor se expresa como la época.
* `iss`:&nbsp;&nbsp;&nbsp;Identifica al emisor. Tiene el mismo valor que la notificación `aud`.
* `aud`:&nbsp;&nbsp;&nbsp;Identifica al público. Deber ser uno de los siguientes valores: `https://collections.mp.microsoft.com/v6.0/keys` o `https://purchase.mp.microsoft.com/v6.0/keys`.
* `exp`:&nbsp;&nbsp;&nbsp;Identifica la hora de expiración en que (o a partir de la cual) la clave ya no se aceptará para procesar nada, excepto para renovar claves. El valor de esta notificación se expresa como el tiempo de la época.
* `nbf`:&nbsp;&nbsp;&nbsp;Identifica la hora a la que el token se aceptará para su procesamiento. El valor de esta notificación se expresa como el tiempo de la época.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`:&nbsp;&nbsp;&nbsp;El identificador de cliente que identifica al desarrollador.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`:&nbsp;&nbsp;&nbsp;Una carga opaca (cifrada y codificada con Base64) que contiene información destinada solo al uso por parte de servicios de la Tienda Windows.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`:&nbsp;&nbsp;&nbsp;Un identificador de usuario que identifica al usuario actual en el contexto de tus servicios. Es el mismo valor que pasas al parámetro opcional *publisherUserId* del [método que usas para crear la clave](#step-4).
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`:&nbsp;&nbsp;&nbsp;El URI que puedes usar para renovar la clave.

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

## <a name="related-topics"></a>Temas relacionados

* [Consultar productos](query-for-products.md)
* [Notificar productos consumibles como completados](report-consumable-products-as-fulfilled.md)
* [Conceder productos gratuitos](grant-free-products.md)
* [Renovar una clave de id. de la Tienda Windows](renew-a-windows-store-id-key.md)
* [Integrar aplicaciones con Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502)
* [Descripción del manifiesto de aplicación de Azure Active Directory]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [Tipos de notificaciones y tokens admitidos](http://go.microsoft.com/fwlink/?LinkId=722501)

