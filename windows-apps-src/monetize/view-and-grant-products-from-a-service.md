---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: Si tienes un catálogo de aplicaciones y complementos, puedes usar la API de colecciones de Microsoft Store y la API de compras de Microsoft Store para acceder a la información de propiedad de estos productos desde tus servicios.
title: Administrar los derechos de producto desde un servicio
ms.date: 08/01/2018
ms.topic: article
keywords: windows 10, uwp, API de colección de Microsoft Store, API de compra de Microsoft Store, ver productos, conceder productos
ms.localizationpriority: medium
ms.openlocfilehash: 2d0df7780943717d8e01f0efcf1583efe05608af
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259215"
---
# <a name="manage-product-entitlements-from-a-service"></a>Administrar los derechos de producto desde un servicio

Si tienes un catálogo de aplicaciones y complementos, puedes usar la *API de colecciones de Microsoft Store* y la *API de compras de Microsoft Store* para obtener acceso a la información de derechos de estos productos desde tus servicios. Un *derecho* representa el derecho de un cliente de usar una aplicación o complemento que se publica a través de Microsoft Store.

Estas API constan de métodos REST diseñados para que los desarrolladores los usen con los catálogos de complementos compatibles con los servicios multiplataforma. Estas API te permiten hacer lo siguiente:

-   API de colecciones de Microsoft Store: [Consulta de productos pertenecientes a un usuario](query-for-products.md) y [notificar un producto como cumplido](report-consumable-products-as-fulfilled.md).
-   API de compras de Microsoft Store: [Concede un producto gratuito a un usuario](grant-free-products.md), [obtén las suscripciones de un usuario](get-subscriptions-for-a-user.md) y [cambia el estado de facturación de la suscripción de un usuario](change-the-billing-state-of-a-subscription-for-a-user.md).

> [!NOTE]
> La API de colecciones y la API de compras de Microsoft Store usan autenticación de Azure Active Directory (Azure AD) para acceder a la información de propiedad del cliente. Para usar estas API, tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para el directorio. Si ya usas Office 365 u otros servicios empresariales de Microsoft, ya tienes un directorio de Azure AD.

## <a name="overview"></a>Introducción

Los siguientes pasos describen el proceso de principio a fin para usar la API de colecciones y la API de compras de Microsoft Store:

1.  [Configurar una aplicación en Azure ad](#step-1).
2.  [Asocie el identificador de la aplicación Azure ad a la aplicación en el centro de Partners](#step-2).
3.  En el servicio, [genera tokens de acceso de Azure AD](#step-3) que representen tu identidad de publicador.
4.  En la aplicación cliente de Windows, [cree una clave de identificador de Microsoft Store](#step-4) que represente la identidad del usuario actual y vuelva a pasar esta clave a su servicio.
5.  Cuando tengas el token de acceso de Azure AD y la clave del id. de Microsoft Store, [llama a la API de colecciones o la API de compras de Microsoft Store, o compra la API desde tu servicio](#step-5).

Este proceso de un extremo a otro implica dos componentes de software que realizan tareas diferentes:

* **Su servicio**. Se trata de una aplicación que se ejecuta de forma segura en el contexto de su entorno empresarial y se puede implementar con cualquier plataforma de desarrollo que elija. El servicio es responsable de la creación de los tokens de acceso Azure AD necesarios para el escenario y para llamar a los URI de REST de la API de recopilación Microsoft Store y de la API de compra.
* **La aplicación cliente de Windows**. Se trata de la aplicación para la que desea obtener acceso y administrar la información de derechos de los clientes (incluidos los complementos de la aplicación). Esta aplicación es responsable de crear las claves de identificador de Microsoft Store que necesita para llamar a la API de colección Microsoft Store y a la API de compra desde el servicio.

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>Paso 1: configurar una aplicación en Azure AD

Antes de poder usar la API de colección Microsoft Store o la API de compra, debe crear una aplicación web Azure AD, recuperar el identificador de inquilino y el identificador de aplicación de la aplicación y generar una clave. La aplicación Web de Azure AD representa el servicio del que desea llamar a la API de colección Microsoft Store o a la API de compra. Necesita el identificador de inquilino, el identificador de la aplicación y la clave para generar Azure AD tokens de acceso que necesita para llamar a la API.

> [!NOTE]
> Solo tienes que realizar las tareas en esta sección una vez. Después de actualizar el manifiesto de aplicación de Azure AD y tener el identificador de inquilino, el identificador de aplicación y el secreto de cliente, puede volver a usar estos valores cada vez que necesite crear un nuevo token de acceso de Azure AD.

1.  Si aún no lo ha hecho, siga las instrucciones de [integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) para registrar una aplicación **Web o** una aplicación de API con Azure ad.
    > [!NOTE]
    > Al registrar la aplicación, debe elegir **aplicación web o API** como tipo de aplicación para que pueda recuperar una clave (también denominada *secreto de cliente*) para la aplicación. Para llamar a la API de colecciones o la API de compras de Microsoft Store, debes proporcionar un secreto de cliente cuando solicites un token de acceso de Azure AD en un paso posterior.

2.  En [Azure portal de administración](https://portal.azure.com/), vaya a **Azure Active Directory**. Seleccione el directorio, haga clic en **registros de aplicaciones** en el panel de navegación izquierdo y, a continuación, seleccione la aplicación.
3.  Se le dirigirá a la página de registro principal de la aplicación. En esta página, copie el valor de ID. de **aplicación** para usarlo más adelante.
4.  Cree una clave que necesitará más adelante (esto se conoce como *secreto de cliente*). En el panel izquierdo, haga clic en **configuración** y luego en **claves**. En esta página, complete los pasos para [crear una clave](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis). Copie esta clave para su uso posterior.
5.  Agregue varios URI de audiencia necesarios al [manifiesto de aplicación](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest). En el panel izquierdo, haga clic en **manifiesto**. Haga clic en **Editar**, reemplace la sección `"identifierUris"` por el siguiente texto y, a continuación, haga clic en **Guardar**.

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    Estas cadenas representan los públicos que admite tu aplicación. En un paso posterior, crearás tokens de acceso de Azure AD asociados a cada uno de estos valores de público.

<span id="step-2"/>

## <a name="step-2-associate-your-azure-ad-application-id-with-your-client-app-in-partner-center"></a>Paso 2: asociar el identificador de la aplicación de Azure AD a la aplicación cliente en el centro de Partners

Antes de poder usar la API de recopilación de Microsoft Store o la API de compra para configurar la propiedad y las compras de la aplicación o el complemento, debe asociar el identificador de la aplicación de Azure AD a la aplicación (o la aplicación que contiene el complemento) en el centro de Partners.

> [!NOTE]
> Solo debes realizar esta tarea una vez.

1.  Inicie sesión en el [centro de Partners](https://partner.microsoft.com/dashboard) y seleccione la aplicación.
2.  Vaya a la página **servicios** &gt; **recopilaciones de productos y compras** y escriba el ID. de la aplicación Azure ad en uno de los campos ID. de **cliente** disponibles.

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
      > Usa la audiencia de `https://onestore.microsoft.com` solo con los tokens de acceso almacenados de forma segura en tu servicio. La exposición de los tokens de acceso con este público fuera del servicio puede provocar que el servicio sea vulnerable a los ataques de reproducción.

  * Si quieres llamar un método de la API de colecciones de Microsoft Store para [consultar los productos pertenecientes a un usuario](query-for-products.md) o [notificar un producto consumible como completado](report-consumable-products-as-fulfilled.md), debes crear también un token con la URI de audiencia `https://onestore.microsoft.com/b2b/keys/create/collections`. En un paso posterior, pasarás este token a un método de cliente en el Windows SDK para solicitar una clave de id. de Microsoft Store que puedas usar con la API de colecciones de Microsoft Store.

  * Si quieres llamar a un método de la API de compras de Microsoft Store para [conceder un producto gratuito a un usuario](grant-free-products.md), [obtener suscripciones para un usuario](get-subscriptions-for-a-user.md) o [cambiar el estado de facturación de la suscripción de un usuario](change-the-billing-state-of-a-subscription-for-a-user.md), también debes crear un token con el URI de audiencia `https://onestore.microsoft.com/b2b/keys/create/purchase`. En un paso posterior, pasarás este token a un método de cliente del Windows SDK para solicitar una clave de id. de Microsoft Store que se pueda usar con la API de compras de Microsoft Store.

<span />

### <a name="create-the-tokens"></a>Crear los tokens

Para crear los tokens de acceso, usa la API de OAuth 2.0 en tu servicio siguiendo las instrucciones de [Llamadas entre servicios mediante las credenciales del cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar un HTTP POST al punto de conexión ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Este es un ejemplo de solicitud.

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

* En el caso de los parámetros *client\_ID* y *Client\_Secret* , especifique el identificador de aplicación y el secreto de cliente de la aplicación que recuperó de la [portal de administración de Azure](https://portal.azure.com/). Ambos parámetros se necesitan para crear un token de acceso con el nivel de autenticación que requieren la API de colecciones o la API de compras de Microsoft Store.

* Para el parámetro *recursos*, especifica uno de los URI de público enumerados en la [sección anterior](#access-tokens), según el tipo de token de acceso que está creando.

Una vez que expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens). Para obtener más detalles sobre la estructura de un token de acceso, consulta [Supported Token and Claim Types (Tipos de notificaciones y tokens admitidos)](https://docs.microsoft.com/azure/active-directory/develop/id-tokens).

<span id="step-4"/>

## <a name="step-4-create-a-microsoft-store-id-key"></a>Paso 4: Crea una clave de id. de Microsoft Store

Antes de llamar a un método en la API de colecciones o la API de compras de Microsoft Store, la aplicación tiene que crear una clave de id. de Microsoft Store y enviarla al servicio. Esta clave es un token web JSON (JWT) que representa la identidad del usuario a cuya información de propiedad del producto quieres acceder. Para obtener más información acerca de las notificaciones de esta clave, consulta [Notificaciones de una clave de id. de Microsoft Store](#claims-in-a-microsoft-store-id-key).

Actualmente, la única manera de crear una clave de id. de Microsoft Store es mediante una llamada a una API de la Plataforma universal de Windows (UWP) desde el código de cliente de la aplicación. La clave generada representa la identidad del usuario que actualmente tiene iniciada sesión en Microsoft Store en el dispositivo.

> [!NOTE]
> Cada clave de id. de Microsoft Store es válida durante 90 días. Puedes [renovar la clave](renew-a-windows-store-id-key.md) cuando expire. Te recomendamos que renueves tus claves de id. de Microsoft Store en lugar de crear claves nuevas.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-collection-api"></a>Crear una clave de id. de Microsoft Store para la API de colecciones de Microsoft Store

Sigue estos pasos para crear una clave de id. de Microsoft Store que puedas usar con la API de colecciones de Microsoft Store para [consultar productos pertenecientes a un usuario](query-for-products.md) o [notificar un producto consumible como completado](report-consumable-products-as-fulfilled.md).

1.  Pasa el token de acceso de Azure AD que tenga el valor `https://onestore.microsoft.com/b2b/keys/create/collections` del URI de audiencia desde el servicio a tu aplicación cliente. Este es uno de los tokens que creaste [anteriormente en el paso 3](#step-3).

2.  En el código de la aplicación, llama a uno de estos métodos para recuperar una clave de id. de Microsoft Store.

  * Si la aplicación usa la clase [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) en el espacio de nombres [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) para administrar las compras desde la aplicación, usa el método [StoreContext.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomercollectionsidasync).

  * Si la aplicación usa la clase [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) en el espacio de nombres [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) para administrar las compras desde la aplicación, usa el método [CurrentApp.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomercollectionsidasync).

    Pasa el token de acceso de Azure AD al parámetro *serviceTicket* del método. Si mantiene identificadores de usuario anónimos en el contexto de los servicios que administra como el publicador de la aplicación actual, también puede pasar un identificador de usuario al parámetro *publisherUserId* para asociar el usuario actual con la nueva clave de identificador de Microsoft Store (el identificador de usuario se insertará en la clave). De lo contrario, si no necesita asociar un identificador de usuario con la clave de identificador de Microsoft Store, puede pasar cualquier valor de cadena al parámetro *publisherUserId* .

3.  Después de que la aplicación cree correctamente una clave de id. de Microsoft Store, devuelve la clave a tu servicio.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>Crear una clave de id. de Microsoft Store para la API de compras de Microsoft Store

Sigue estos pasos para crear una clave de id. de Microsoft Store que puedas usar con la API de compras de Microsoft Store para [conceder un producto gratuito a un usuario](grant-free-products.md), [obtener suscripciones para un usuario](get-subscriptions-for-a-user.md) o [cambiar el estado de facturación de la suscripción de un usuario](change-the-billing-state-of-a-subscription-for-a-user.md).

1.  Pasa el token de acceso de Azure AD que tenga el valor `https://onestore.microsoft.com/b2b/keys/create/purchase` del URI de audiencia desde el servicio a tu aplicación cliente. Este es uno de los tokens que creaste [anteriormente en el paso 3](#step-3).

2.  En el código de la aplicación, llama a uno de estos métodos para recuperar una clave de id. de Microsoft Store.

  * Si la aplicación usa la clase [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) en el espacio de nombres [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) para administrar las compras desde la aplicación, usa el método [StoreContext.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync).

  * Si la aplicación usa la clase [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) en el espacio de nombres [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) para administrar las compras desde la aplicación, usa el método [CurrentApp.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync).

    Pasa el token de acceso de Azure AD al parámetro *serviceTicket* del método. Si mantiene identificadores de usuario anónimos en el contexto de los servicios que administra como el publicador de la aplicación actual, también puede pasar un identificador de usuario al parámetro *publisherUserId* para asociar el usuario actual con la nueva clave de identificador de Microsoft Store (el identificador de usuario se insertará en la clave). De lo contrario, si no necesita asociar un identificador de usuario con la clave de identificador de Microsoft Store, puede pasar cualquier valor de cadena al parámetro *publisherUserId* .

3.  Después de que la aplicación cree correctamente una clave de id. de Microsoft Store, devuelve la clave a tu servicio.

### <a name="diagram"></a>Diagrama

En el diagrama siguiente se ilustra el proceso de creación de una clave de identificador de Microsoft Store.

  ![Crear clave de identificador de la tienda Windows](images/b2b-1.png)

<span id="step-5"/>

## <a name="step-5-call-the-microsoft-store-collection-api-or-purchase-api-from-your-service"></a>Paso 5: Llama a la API de colecciones o la API de compras de Microsoft Store desde tu servicio

Cuando el servicio disponga de una clave de id. de Microsoft Store que permita acceder a la información de propiedad de producto de un usuario específico, el servicio puede llamar a la API de colecciones o la API de compras de Microsoft Store siguiendo estas instrucciones:

* [Consulta de productos](query-for-products.md)
* [Notificar productos consumibles como entregados](report-consumable-products-as-fulfilled.md)
* [Conceda productos gratis](grant-free-products.md)
* [Obtener suscripciones para un usuario](get-subscriptions-for-a-user.md)
* [Cambiar el estado de facturación de una suscripción para un usuario](change-the-billing-state-of-a-subscription-for-a-user.md)

Para cada escenario, pasa la siguiente información a la API:

-   En el encabezado de solicitud, pasa el token de acceso de Azure AD que tiene el valor del URI de audiencia `https://onestore.microsoft.com`. Este es uno de los tokens que creaste [anteriormente en el paso 3](#step-3). Este token representa tu identidad de publicador.
-   En el cuerpo de la solicitud, pasa la clave de id. de Microsoft Store que recuperaste [anteriormente en el paso 4](#step-4) desde el código del lado de cliente de la aplicación. Esta clave representa la identidad del usuario a cuya información de propiedad del producto deseas acceder.

### <a name="diagram"></a>Diagrama

En el diagrama siguiente se describe el proceso de llamar a un método en la API de colección Microsoft Store o la API de compra desde el servicio.

  ![Llamar a colecciones o API de compra](images/b2b-2.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Notificaciones en una clave de id. de Microsoft Store

Una clave de id. de Microsoft Store es un token web JSON (JWT) que representa la identidad del usuario a cuya información de propiedad del producto deseas acceder. Si se descodifica mediante Base64, una clave de id. de Microsoft Store contiene las siguientes notificaciones.

* `iat`:&nbsp;&nbsp;&nbsp;identifica la hora a la que se emitió la clave. Esta notificación puede usarse para determinar la edad del token. Este valor se expresa como la época.
* `iss`:&nbsp;&nbsp;&nbsp;identifica al emisor. Tiene el mismo valor que la notificación `aud`.
* `aud`:&nbsp;&nbsp;&nbsp;identifica a la audiencia. Deber ser uno de los siguientes valores: `https://collections.mp.microsoft.com/v6.0/keys` o `https://purchase.mp.microsoft.com/v6.0/keys`.
* `exp`:&nbsp;&nbsp;&nbsp;identifica la fecha y hora de expiración en la que ya no se aceptará la clave para el procesamiento de cualquier elemento, salvo para la renovación de claves. El valor de esta notificación se expresa como el tiempo de la época.
* `nbf`:&nbsp;&nbsp;&nbsp;identifica la hora a la que se aceptará el token para el procesamiento. El valor de esta notificación se expresa como el tiempo de la época.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`:&nbsp;&nbsp;&nbsp;el identificador de cliente que identifica al desarrollador.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`:&nbsp;&nbsp;&nbsp;una carga opaca (cifrada y codificada en Base64) que contiene información que solo está diseñada para su uso por parte de Microsoft Store Services.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`:&nbsp;&nbsp;&nbsp;un identificador de usuario que identifica al usuario actual en el contexto de los servicios. Es el mismo valor que pasas al parámetro opcional *publisherUserId* del [método que usas para crear la clave](#step-4).
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`:&nbsp;&nbsp;&nbsp;el URI que puede usar para renovar la clave.

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

* [Consulta de productos](query-for-products.md)
* [Notificar productos consumibles como entregados](report-consumable-products-as-fulfilled.md)
* [Conceda productos gratis](grant-free-products.md)
* [Obtener suscripciones para un usuario](get-subscriptions-for-a-user.md)
* [Cambiar el estado de facturación de una suscripción para un usuario](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Renovar una clave de identificador de Microsoft Store](renew-a-windows-store-id-key.md)
* [Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app)
* [Descripción del manifiesto de aplicación de Azure Active Directory]( https://go.microsoft.com/fwlink/?LinkId=722500)
* [Tipos de notificaciones y tokens admitidos](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
