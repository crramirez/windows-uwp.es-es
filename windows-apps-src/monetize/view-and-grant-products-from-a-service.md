---
author: mcleanbyron
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: Si tienes un catálogo de aplicaciones y complementos, puedes usar la API de colecciones de Microsoft Store y la API de compras de Microsoft Store para acceder a la información de propiedad de estos productos desde tus servicios.
title: Administrar los derechos de producto de un servicio
ms.author: mcleans
ms.date: 08/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de colección de Microsoft Store, API de compra de Microsoft Store, ver productos, conceder productos
ms.localizationpriority: medium
ms.openlocfilehash: 3a0766830bc2110dffcf5baf886e8ccb98ac6446
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "4086064"
---
# <a name="manage-product-entitlements-from-a-service"></a>Administrar los derechos de producto de un servicio

Si tienes un catálogo de aplicaciones y complementos, puedes usar la *API de colecciones de Microsoft Store* y la *API de compras de Microsoft Store* para obtener acceso a la información de derechos de estos productos desde tus servicios. Un *derecho* representa el derecho de un cliente de usar una aplicación o complemento que se publica a través de Microsoft Store.

Estas API constan de métodos REST diseñados para que los desarrolladores los usen con los catálogos de complementos compatibles con los servicios multiplataforma. Estas API te permiten hacer lo siguiente:

-   API de colecciones de Microsoft Store: [Consulta de productos pertenecientes a un usuario](query-for-products.md) y [notificar un producto como cumplido](report-consumable-products-as-fulfilled.md).
-   API de compras de Microsoft Store: [Concede un producto gratuito a un usuario](grant-free-products.md), [obtén las suscripciones de un usuario](get-subscriptions-for-a-user.md) y [cambia el estado de facturación de la suscripción de un usuario](change-the-billing-state-of-a-subscription-for-a-user.md).

> [!NOTE]
> La API de colecciones y la API de compras de Microsoft Store usan autenticación de Azure Active Directory (Azure AD) para acceder a la información de propiedad del cliente. Para usar estas API, tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para el directorio. Si ya usas Office365 u otros servicios empresariales de Microsoft, ya tienes un directorio de AzureAD.

## <a name="overview"></a>Introducción

Los siguientes pasos describen el proceso de principio a fin para usar la API de colecciones y la API de compras de Microsoft Store:

1.  [Configurar una aplicación en Azure AD](#step-1).
2.  [Asocia tu identificador de aplicación de Azure AD con tu aplicación en el panel del centro de desarrollo de Windows](#step-2).
3.  En el servicio, [genera tokens de acceso de Azure AD](#step-3) que representen tu identidad de publicador.
4.  En la aplicación de Windows de cliente, [crear una clave de Id. de Microsoft Store](#step-4) que representa la identidad del usuario actual y pasa esta clave de nuevo al servicio.
5.  Cuando tengas el token de acceso de Azure AD y la clave del id. de Microsoft Store, [llama a la API de colecciones o la API de compras de Microsoft Store, o compra la API desde tu servicio](#step-5).

Este proceso de principio a fin implica dos componentes de software que realizan diferentes tareas:

* **El servicio**. Se trata de una aplicación que se ejecuta de forma segura en el contexto de su entorno empresarial, y pueden implementarse mediante cualquier plataforma de desarrollo que elijas. El servicio es responsable de crear los tokens de acceso de Azure AD necesaria para el escenario y para llamar a los URI de REST de colecciones de Microsoft Store API y la API.
* **La aplicación de cliente de Windows**. Se trata de la aplicación para la que quieres obtener acceso a y administrar la información de derecho de cliente (incluidos los complementos de la aplicación). Esta aplicación es responsable de crear las claves de Id. de Microsoft Store que necesitas para llamar a la API de colecciones de Microsoft Store y la API de compras desde el servicio.

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>Paso 1: Configurar una aplicación en Azure AD

Antes de poder usar la API de colecciones de Microsoft Store o la API de compras, debes crear una aplicación Web de Azure AD, recuperar el identificador de inquilino e Id. de aplicación para la aplicación y generar una clave. La aplicación Web de Azure AD representa el servicio desde el que quieres llamar a la API de colecciones de Microsoft Store o la API de compras. Necesitas el identificador de inquilino, Id. de aplicación y la clave para generar tokens de acceso de Azure AD que necesitas para llamar a la API.

> [!NOTE]
> Solo tienes que realizar las tareas en esta sección una vez. Después de actualizar el manifiesto de aplicación de Azure AD y tienes tu identificador de inquilino, el secreto de cliente y el Id. de aplicación, puedes volver a estos valores siempre que necesites para crear un nuevo token de acceso de Azure AD.

1.  Si no has no lo ha hecho, sigue las instrucciones de [Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) para registrar un **aplicación Web / API** aplicaciones con Azure AD.
    > [!NOTE]
    > Al registrar la aplicación, debes elegir **aplicación Web / API** como tipo de la aplicación para que puedes recuperar una clave (también denominada un *secreto de cliente*) para la aplicación. Para llamar a la API de colecciones o la API de compras de Microsoft Store, debes proporcionar un secreto de cliente cuando solicites un token de acceso de Azure AD en un paso posterior.

2.  En el [Portal de administración de Azure](https://portal.azure.com/), ve a **Azure Active Directory**. Selecciona el directorio, haz clic en los **registros de aplicaciones** en el panel de navegación izquierdo y, a continuación, selecciona la aplicación.
3.  Redirige a la página de registro principal de la aplicación. En esta página, copia el valor de **Id. de aplicación** para su uso posterior.
4.  Crear una clave que tendrás que más adelante (Esto es todo denomina un *secreto de cliente*). En el panel izquierdo, haz clic en **configuración** y, a continuación, **las claves**. En esta página, realiza los pasos para [crear una clave](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis). Copia esta clave para su uso posterior.
5.  Agrega los URI de público necesario varios el [manifiesto de la aplicación](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest). En el panel izquierdo, haz clic en el **manifiesto**. Haz clic en **Editar**, reemplaza el `"identifierUris"` sección con el texto siguiente y, a continuación, haz clic en **Guardar**.

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    Estas cadenas representan los públicos que admite tu aplicación. En un paso posterior, crearás tokens de acceso de Azure AD asociados a cada uno de estos valores de público.

<span id="step-2"/>

## <a name="step-2-associate-your-azure-ad-application-id-with-your-client-app-in-windows-dev-center"></a>Paso 2: Asocia tu identificador de aplicación de Azure AD con tu aplicación de cliente en el centro de desarrollo de Windows

Antes de poder usar la API de colecciones de Microsoft Store o la API para configurar la propiedad y las compras de la aplicación o complemento de compras, tienes que asociar tu identificador de aplicación de Azure AD con la aplicación (o la aplicación que contiene el complemento) en el panel del centro de desarrollo.

> [!NOTE]
> Solo debes realizar esta tarea una vez.

1.  Inicia sesión en el [panel del Centro de desarrollo](https://dev.windows.com/overview) y selecciona tu aplicación.
2.  Ve a los **Servicios** &gt; **colecciones de producto y compras desde la** página y escribe tu identificador de aplicación de Azure AD en uno de los campos de **Id. de cliente** disponibles.

<span id="step-3"/>

## <a name="step-3-create-azure-ad-access-tokens"></a>Paso 3: Crea tokens de acceso de Azure AD

Para poder recuperar una clave de id. de Microsoft Store o llamar a la API de colecciones o la API de compras de Microsoft Store, tu servicio debe crear diferentes tokens de acceso de Azure AD que representen tu identidad de publicador. Cada token se usará con una API diferente. La duración de cada uno de estos tokens es de 60 minutos y se pueden actualizar después de su expiración.

> [!IMPORTANT]
> Crea tokens de acceso de Azure AD solamente en el contexto del servicio, no en la aplicación. El secreto de cliente podría verse comprometido si se envía a la aplicación.

<span id="access-tokens" />

### <a name="understanding-the-different-tokens-and-audience-uris"></a>Descripción de los diferentes tokens y los URI de audiencia

Dependiendo de los métodos con los que quieras llamar en la API de colecciones o la API de compras de Microsoft Store, debes crear dos o tres tokens diferentes. Cada token de acceso está asociado con un URI de público diferente (estos son los mismos URI que agregaste anteriormente a la sección `"identifierUris"` del manifiesto de la aplicación de Azure AD).

  * En todos los casos, debes crear un token con la URI de público de `https://onestore.microsoft.com`. En un paso posterior, pasarás este token al encabezado **Autorización** de métodos en la API de colecciones o la API de compras de Microsoft Store.
      > [!IMPORTANT]
      > Usa la audiencia de `https://onestore.microsoft.com` solo con los tokens de acceso almacenados de forma segura en tu servicio. La exposición de los tokens de acceso con esta audiencia fuera del servicio puede provocar que el servicio sea vulnerable a los ataques de reproducción.

  * Si quieres llamar un método de la API de colecciones de Microsoft Store para [consultar los productos pertenecientes a un usuario](query-for-products.md) o [notificar un producto consumible como completado](report-consumable-products-as-fulfilled.md), debes crear también un token con la URI de audiencia `https://onestore.microsoft.com/b2b/keys/create/collections`. En un paso posterior, pasarás este token a un método de cliente en el Windows SDK para solicitar una clave de id. de Microsoft Store que puedas usar con la API de colecciones de Microsoft Store.

  * Si quieres llamar a un método de la API de compras de Microsoft Store para [conceder un producto gratuito a un usuario](grant-free-products.md), [obtener suscripciones para un usuario](get-subscriptions-for-a-user.md) o [cambiar el estado de facturación de la suscripción de un usuario](change-the-billing-state-of-a-subscription-for-a-user.md), también debes crear un token con el URI de audiencia `https://onestore.microsoft.com/b2b/keys/create/purchase`. En un paso posterior, pasarás este token a un método de cliente del Windows SDK para solicitar una clave de id. de Microsoft Store que se pueda usar con la API de compras de Microsoft Store.

<span />

### <a name="create-the-tokens"></a>Crear los tokens

Para crear los tokens de acceso, usa la API de OAuth 2.0 en tu servicio siguiendo las instrucciones de [Llamadas entre servicios mediante las credenciales del cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service) para enviar un HTTP POST al punto de conexión ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Este es un ejemplo de solicitud.

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

* Para los parámetros *client\_id* y *client\_secret* , especifica el identificador de aplicación y el secreto de cliente para la aplicación que has recuperado desde el [Portal de administración de Azure](http://manage.windowsazure.com). Ambos parámetros se necesitan para crear un token de acceso con el nivel de autenticación que requieren la API de colecciones o la API de compras de Microsoft Store.

* Para el parámetro *recursos*, especifica uno de los URI de público enumerados en la [sección anterior](#access-tokens), según el tipo de token de acceso que está creando.

Una vez que expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens). Para obtener más detalles sobre la estructura de un token de acceso, consulta [Tipos de notificaciones y tokens admitidos](http://go.microsoft.com/fwlink/?LinkId=722501).

<span id="step-4"/>

## <a name="step-4-create-a-microsoft-store-id-key"></a>Paso 4: Crea una clave de id. de Microsoft Store

Antes de llamar a un método en la API de colecciones o la API de compras de Microsoft Store, la aplicación tiene que crear una clave de id. de Microsoft Store y enviarla al servicio. Esta clave es un token web JSON (JWT) que representa la identidad del usuario a cuya información de propiedad del producto quieres acceder. Para obtener más información acerca de las notificaciones de esta clave, consulta [Notificaciones de una clave de id. de Microsoft Store](#claims-in-a-microsoft-store-id-key).

Actualmente, la única manera de crear una clave de id. de Microsoft Store es mediante una llamada a una API de la Plataforma universal de Windows (UWP) desde el código de cliente de la aplicación. La clave generada representa la identidad del usuario que actualmente tiene iniciada sesión en Microsoft Store en el dispositivo.

> [!NOTE]
> Cada clave de id. de Microsoft Store es válida durante 90 días. Puedes [renovar las claves](renew-a-windows-store-id-key.md) cuando expiren. Te recomendamos que renueves tus claves de id. de Microsoft Store en lugar de crear claves nuevas.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-collection-api"></a>Crear una clave de id. de Microsoft Store para la API de colecciones de Microsoft Store

Sigue estos pasos para crear una clave de id. de Microsoft Store que puedas usar con la API de colecciones de Microsoft Store para [consultar productos pertenecientes a un usuario](query-for-products.md) o [notificar un producto consumible como completado](report-consumable-products-as-fulfilled.md).

1.  Pasa el token de acceso de Azure AD que tenga el valor `https://onestore.microsoft.com/b2b/keys/create/collections` del URI de audiencia desde el servicio a tu aplicación cliente. Este es uno de los tokens que creaste [anteriormente en el paso 3](#step-3).

2.  En el código de la aplicación, llama a uno de estos métodos para recuperar una clave de id. de Microsoft Store.

  * Si la aplicación usa la clase [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) en el espacio de nombres [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) para administrar las compras desde la aplicación, usa el método [StoreContext.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomercollectionsidasync).

  * Si la aplicación usa la clase [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) en el espacio de nombres [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) para administrar las compras desde la aplicación, usa el método [CurrentApp.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomercollectionsidasync).

    Pasa el token de acceso de Azure AD al parámetro *serviceTicket* del método. Si mantienes Id. de usuario anónimo en el contexto de los servicios que administrar como el publicador de la aplicación actual, también puedes pasar un identificador de usuario para el parámetro *publisherUserId* para asociar el usuario actual con la nueva clave de Id. de Microsoft Store (el identificador de usuario será em encajados en la clave). De lo contrario, si no es necesario asociar un identificador de usuario con la clave de Id. de Microsoft Store, puedes pasar cualquier valor de cadena para el parámetro *publisherUserId* .

3.  Después de que la aplicación cree correctamente una clave de id. de Microsoft Store, devuelve la clave a tu servicio.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>Crear una clave de id. de Microsoft Store para la API de compras de Microsoft Store

Sigue estos pasos para crear una clave de id. de Microsoft Store que puedas usar con la API de compras de Microsoft Store para [conceder un producto gratuito a un usuario](grant-free-products.md), [obtener suscripciones para un usuario](get-subscriptions-for-a-user.md) o [cambiar el estado de facturación de la suscripción de un usuario](change-the-billing-state-of-a-subscription-for-a-user.md).

1.  Pasa el token de acceso de Azure AD que tenga el valor `https://onestore.microsoft.com/b2b/keys/create/purchase` del URI de audiencia desde el servicio a tu aplicación cliente. Este es uno de los tokens que creaste [anteriormente en el paso 3](#step-3).

2.  En el código de la aplicación, llama a uno de estos métodos para recuperar una clave de id. de Microsoft Store.

  * Si la aplicación usa la clase [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) en el espacio de nombres [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) para administrar las compras desde la aplicación, usa el método [StoreContext.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync).

  * Si la aplicación usa la clase [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) en el espacio de nombres [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) para administrar las compras desde la aplicación, usa el método [CurrentApp.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync).

    Pasa el token de acceso de Azure AD al parámetro *serviceTicket* del método. Si mantienes Id. de usuario anónimo en el contexto de los servicios que administrar como el publicador de la aplicación actual, también puedes pasar un identificador de usuario para el parámetro *publisherUserId* para asociar el usuario actual con la nueva clave de Id. de Microsoft Store (el identificador de usuario será em encajados en la clave). De lo contrario, si no es necesario asociar un identificador de usuario con la clave de Id. de Microsoft Store, puedes pasar cualquier valor de cadena para el parámetro *publisherUserId* .

3.  Después de que la aplicación cree correctamente una clave de id. de Microsoft Store, devuelve la clave a tu servicio.

### <a name="diagram"></a>Diagrama

El siguiente diagrama ilustra el proceso de creación de una clave de Id. de Microsoft Store.

  ![Crear la clave de Id. de Store de Windows](images/b2b-1.png)

<span id="step-5"/>

## <a name="step-5-call-the-microsoft-store-collection-api-or-purchase-api-from-your-service"></a>Paso 5: Llama a la API de colecciones o la API de compras de Microsoft Store desde tu servicio

Cuando el servicio disponga de una clave de id. de Microsoft Store que permita acceder a la información de propiedad de producto de un usuario específico, el servicio puede llamar a la API de colecciones o la API de compras de Microsoft Store siguiendo estas instrucciones:

* [Consultar productos](query-for-products.md)
* [Notificar productos consumibles como completados](report-consumable-products-as-fulfilled.md)
* [Conceder productos gratuitos](grant-free-products.md)
* [Obtener suscripciones para un usuario](get-subscriptions-for-a-user.md)
* [Cambiar el estado de facturación de la suscripción de un usuario](change-the-billing-state-of-a-subscription-for-a-user.md)

Para cada escenario, pasa la siguiente información a la API:

-   En el encabezado de solicitud, pasa el token de acceso de Azure AD que tiene el valor del URI de audiencia `https://onestore.microsoft.com`. Este es uno de los tokens que creaste [anteriormente en el paso 3](#step-3). Este token representa tu identidad de publicador.
-   En el cuerpo de la solicitud, pasa la clave de id. de Microsoft Store que recuperaste [anteriormente en el paso 4](#step-4) desde el código del lado de cliente de la aplicación. Esta clave representa la identidad del usuario a cuya información de propiedad del producto deseas acceder.

### <a name="diagram"></a>Diagrama

El diagrama siguiente describe el proceso de una llamada a un método en la API o la API de colecciones de Microsoft Store desde tu servicio.

  ![Llamar a colecciones o adquirir API](images/b2b-2.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Notificaciones en una clave de id. de Microsoft Store

Una clave de id. de Microsoft Store es un token web JSON (JWT) que representa la identidad del usuario a cuya información de propiedad del producto deseas acceder. Si se descodifica mediante Base64, una clave de id. de Microsoft Store contiene las siguientes notificaciones.

* `iat`:&nbsp;&nbsp;&nbsp;Identifica la hora en que se emitió la clave. Esta notificación puede usarse para determinar la edad del token. Este valor se expresa como la época.
* `iss`:&nbsp;&nbsp;&nbsp;Identifica al emisor. Tiene el mismo valor que la notificación `aud`.
* `aud`:&nbsp;&nbsp;&nbsp;Identifica al público. Deber ser uno de los siguientes valores: `https://collections.mp.microsoft.com/v6.0/keys` o `https://purchase.mp.microsoft.com/v6.0/keys`.
* `exp`:&nbsp;&nbsp;&nbsp;Identifica la hora de expiración en que (o a partir de la cual) la clave ya no se aceptará para procesar nada, excepto para renovar claves. El valor de esta notificación se expresa como el tiempo de la época.
* `nbf`:&nbsp;&nbsp;&nbsp;Identifica la hora a la que el token se aceptará para su procesamiento. El valor de esta notificación se expresa como el tiempo de la época.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`:&nbsp;&nbsp;&nbsp;El identificador de cliente que identifica al desarrollador.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`:&nbsp;&nbsp;&nbsp;Una carga opaca (cifrada y codificada con Base64) que contiene información destinada solo para su uso por parte de los servicios de Microsoft Store.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`:&nbsp;&nbsp;&nbsp;Un Id. de usuario que identifica al usuario actual en el contexto de tus servicios. Es el mismo valor que pasas al parámetro opcional *publisherUserId* del [método que usas para crear la clave](#step-4).
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`:&nbsp;&nbsp;&nbsp;El URI que puedes usar para renovar la clave.

Este es un ejemplo de un encabezado de clave de id. de Microsoft Store descodificado.

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

Este es un ejemplo de un conjunto de notificaciones de clave de id. de Microsoft Store descodificado.

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
* [Obtener suscripciones para un usuario](get-subscriptions-for-a-user.md)
* [Cambiar el estado de facturación de la suscripción de un usuario](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Renovar una clave de id. de Microsoft Store](renew-a-windows-store-id-key.md)
* [Integrar aplicaciones con Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502)
* [Descripción del manifiesto de aplicación de Azure Active Directory]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [Tipos de notificaciones y tokens admitidos](http://go.microsoft.com/fwlink/?LinkId=722501)
