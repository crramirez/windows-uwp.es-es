---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: Si tiene un catálogo de aplicaciones y complementos, puede usar la API de colección Microsoft Store y Microsoft Store API de compra para acceder a la información de propiedad de estos productos desde sus servicios.
title: Administrar los derechos de producto de un servicio
ms.date: 08/01/2018
ms.topic: article
keywords: Windows 10, UWP, API de colección de Microsoft Store, API de compra Microsoft Store, ver productos, conceder productos
ms.localizationpriority: medium
ms.openlocfilehash: 0e4bfa74b693c9571d9bb2818e0d8527388600a2
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846715"
---
# <a name="manage-product-entitlements-from-a-service"></a>Administrar los derechos de producto de un servicio

Si tiene un catálogo de aplicaciones y complementos, puede usar la *API de recopilación de Microsoft Store* y *Microsoft Store API de compra* para acceder a la información de derechos de estos productos desde sus servicios. Un *derecho* representa el derecho de un cliente de usar una aplicación o un complemento que se publica a través de la Microsoft Store.

Estas API constan de métodos REST diseñados para que los desarrolladores los usen con los catálogos de complementos compatibles con los servicios multiplataforma. Estas API te permiten hacer lo siguiente:

-   API de colección Microsoft Store: [consulte los productos que pertenecen a un usuario](query-for-products.md) y [notifique un producto consumible como completado](report-consumable-products-as-fulfilled.md).
-   Microsoft Store API de compra: [conceder a un usuario un producto gratuito](grant-free-products.md), [obtener suscripciones para un usuario](get-subscriptions-for-a-user.md)y [cambiar el estado de facturación de una suscripción para un usuario](change-the-billing-state-of-a-subscription-for-a-user.md).

> [!NOTE]
> La API de recopilación de Microsoft Store y la API de compra usan la autenticación de Azure Active Directory (Azure AD) para tener acceso a la información de propiedad del cliente. Para usar estas API, usted (o su organización) debe tener un directorio Azure AD y debe tener permisos de [administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para el directorio. Si usa Microsoft 365 u otros servicios empresariales de Microsoft, ya tiene el directorio de Azure AD.

## <a name="overview"></a>Información general

En los pasos siguientes se describe el proceso de un extremo a otro para usar la API de recopilación de Microsoft Store y la API de compra:

1.  [Configurar una aplicación en Azure ad](#step-1).
2.  [Asocie el identificador de la aplicación Azure ad a la aplicación en el centro de Partners](#step-2).
3.  En el servicio, [cree Azure ad tokens de acceso](#step-3) que representen la identidad del publicador.
4.  En la aplicación cliente de Windows, [cree una clave de identificador de Microsoft Store](#step-4) que represente la identidad del usuario actual y vuelva a pasar esta clave a su servicio.
5.  Una vez que tenga el token de acceso Azure AD necesario y la clave de Microsoft Store ID, [llame a la API de colección Microsoft Store o a la API de compra desde el servicio](#step-5).

Este proceso de un extremo a otro implica dos componentes de software que realizan tareas diferentes:

* **Su servicio**. Se trata de una aplicación que se ejecuta de forma segura en el contexto de su entorno empresarial y se puede implementar con cualquier plataforma de desarrollo que elija. El servicio es responsable de la creación de los tokens de acceso Azure AD necesarios para el escenario y para llamar a los URI de REST de la API de recopilación Microsoft Store y de la API de compra.
* **La aplicación cliente de Windows**. Se trata de la aplicación para la que desea obtener acceso y administrar la información de derechos de los clientes (incluidos los complementos de la aplicación). Esta aplicación es responsable de crear las claves de identificador de Microsoft Store que necesita para llamar a la API de colección Microsoft Store y a la API de compra desde el servicio.

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>Paso 1: configurar una aplicación en Azure AD

Antes de poder usar la API de colección Microsoft Store o la API de compra, debe crear una aplicación web Azure AD, recuperar el identificador de inquilino y el identificador de aplicación de la aplicación y generar una clave. La aplicación Web de Azure AD representa el servicio del que desea llamar a la API de colección Microsoft Store o a la API de compra. Necesita el identificador de inquilino, el identificador de la aplicación y la clave para generar Azure AD tokens de acceso que necesita para llamar a la API.

> [!NOTE]
> Solo tiene que realizar las tareas de esta sección una vez. Después de actualizar el manifiesto de aplicación de Azure AD y tener el identificador de inquilino, el identificador de aplicación y el secreto de cliente, puede volver a usar estos valores cada vez que necesite crear un nuevo token de acceso de Azure AD.

1.  Si aún no lo ha hecho, siga las instrucciones de [integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) para registrar una aplicación **Web o** una aplicación de API con Azure ad.
    > [!NOTE]
    > Al registrar la aplicación, debe elegir **aplicación web o API** como tipo de aplicación para que pueda recuperar una clave (también denominada *secreto de cliente*) para la aplicación. Para llamar a la API de colección Microsoft Store o a la API de compra, debe proporcionar un secreto de cliente al solicitar un token de acceso de Azure AD en un paso posterior.

2.  En [Azure portal de administración](https://portal.azure.com/), vaya a **Azure Active Directory**. Seleccione el directorio, haga clic en **registros de aplicaciones** en el panel de navegación izquierdo y, a continuación, seleccione la aplicación.
3.  Se le dirigirá a la página de registro principal de la aplicación. En esta página, copie el valor de ID. de **aplicación** para usarlo más adelante.
4.  Cree una clave que necesitará más adelante (esto se conoce como *secreto de cliente*). En el panel izquierdo, haga clic en **configuración** y luego en **claves**. En esta página, complete los pasos para [crear una clave](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis). Copie esta clave para su uso posterior.
5.  Agregue varios URI de audiencia necesarios al [manifiesto de aplicación](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest). En el panel izquierdo, haga clic en **Manifiesto**. Haga clic en **Editar**, reemplace la `"identifierUris"` sección por el siguiente texto y, a continuación, haga clic en **Guardar**.

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
> Solo tiene que realizar esta tarea una vez.

1.  Inicie sesión en el [centro de Partners](https://partner.microsoft.com/dashboard) y seleccione la aplicación.
2.  Vaya a la **Services** &gt; página **colecciones de productos y compras** de servicios y escriba el identificador de la aplicación Azure ad en uno de los campos ID. de **cliente** disponibles.

<span id="step-3"/>

## <a name="step-3-create-azure-ad-access-tokens"></a>Paso 3: creación de tokens de acceso de Azure AD

Para poder recuperar una clave de identificador de Microsoft Store o llamar a la API de colección Microsoft Store o a la API de compra, el servicio debe crear varios tokens de acceso Azure AD distintos que representen la identidad del publicador. Cada token se usará con una API diferente. La duración de cada uno de estos tokens es de 60 minutos y se pueden actualizar después de su expiración.

> [!IMPORTANT]
> Cree Azure AD tokens de acceso solo en el contexto del servicio, no en la aplicación. El secreto de cliente podría verse comprometido si se envía a la aplicación.

<span id="access-tokens" />

### <a name="understanding-the-different-tokens-and-audience-uris"></a>Descripción de los distintos tokens y URI de audiencia

En función de los métodos que desee llamar en la API de colección Microsoft Store o en la API de compra, debe crear dos o tres tokens diferentes. Cada token de acceso está asociado a un URI de audiencia diferente (estos son los mismos URI que se agregaron previamente a la `"identifierUris"` sección del manifiesto de aplicación Azure ad).

  * En todos los casos, debe crear un token con el `https://onestore.microsoft.com` URI de la audiencia. En un paso posterior, pasará este token al encabezado **Authorization** de los métodos de la API de colección Microsoft Store o de la API de compra.
      > [!IMPORTANT]
      > Use la `https://onestore.microsoft.com` audiencia solo con los tokens de acceso que se almacenan de forma segura en el servicio. La exposición de los tokens de acceso con este público fuera del servicio puede provocar que el servicio sea vulnerable a los ataques de reproducción.

  * Si desea llamar a un método en la API de colección Microsoft Store para [consultar los productos que son propiedad de un usuario](query-for-products.md) o [notificar un producto consumible como completado](report-consumable-products-as-fulfilled.md), también debe crear un token con el `https://onestore.microsoft.com/b2b/keys/create/collections` URI de audiencia. En un paso posterior, pasará este token a un método de cliente en el Windows SDK para solicitar una clave de identificador de Microsoft Store que pueda usar con la API de colección Microsoft Store.

  * Si desea llamar a un método en la API de compra de Microsoft Store para [conceder a un usuario un producto gratuito](grant-free-products.md), [obtener suscripciones para un usuario](get-subscriptions-for-a-user.md)o [cambiar el estado de facturación de una suscripción para un usuario](change-the-billing-state-of-a-subscription-for-a-user.md), también debe crear un token con el `https://onestore.microsoft.com/b2b/keys/create/purchase` URI del público. En un paso posterior, pasará este token a un método de cliente en el Windows SDK para solicitar una clave de identificador de Microsoft Store que pueda usar con la API de compra de Microsoft Store.

<span />

### <a name="create-the-tokens"></a>Creación de los tokens

Para crear los tokens de acceso, use la API OAuth 2,0 en su servicio siguiendo las instrucciones de [llamadas de servicio a servicio con las credenciales de cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar una solicitud HTTP post al ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` punto de conexión. Este es un ejemplo de solicitud.

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

* Para los parámetros de * \_ identificador de cliente* y * \_ secreto de cliente* , especifique el identificador de aplicación y el secreto de cliente de la aplicación que recuperó de la [portal de administración de Azure](https://portal.azure.com/). Ambos parámetros son necesarios para crear un token de acceso con el nivel de autenticación requerido por la API de recopilación de Microsoft Store o la API de compra.

* Para el parámetro de *recurso* , especifique uno de los URI de audiencia enumerados en la [sección anterior](#access-tokens), en función del tipo de token de acceso que esté creando.

Una vez que expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens). Para obtener más detalles sobre la estructura de un token de acceso, consulta [Supported Token and Claim Types (Tipos de notificaciones y tokens admitidos)](https://docs.microsoft.com/azure/active-directory/develop/id-tokens).

<span id="step-4"/>

## <a name="step-4-create-a-microsoft-store-id-key"></a>Paso 4: crear una clave de identificador de Microsoft Store

Antes de poder llamar a cualquier método de la API de colección Microsoft Store o de la API de compra, la aplicación debe crear una clave de identificador de Microsoft Store y enviarla al servicio. Esta clave es un JSON Web Token (JWT) que representa la identidad del usuario cuya información de propiedad del producto desea tener acceso. Para obtener más información sobre las notificaciones de esta clave, consulte [notificaciones en una clave de identificador de Microsoft Store](#claims-in-a-microsoft-store-id-key).

Actualmente, la única manera de crear una clave de identificador de Microsoft Store es llamar a una API de Plataforma universal de Windows (UWP) desde el código de cliente de la aplicación. La clave generada representa la identidad del usuario que ha iniciado sesión actualmente en el Microsoft Store en el dispositivo.

> [!NOTE]
> Cada clave de identificador de Microsoft Store es válida durante 90 días. Puedes [renovar la clave](renew-a-windows-store-id-key.md) cuando expire. Se recomienda renovar las claves de Microsoft Store ID en lugar de crear otras nuevas.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-collection-api"></a>Para crear una clave de identificador de Microsoft Store para la API de colección Microsoft Store

Siga estos pasos para crear una clave de identificador de Microsoft Store que puede usar con la API de Microsoft Store colección para [consultar los productos que son propiedad de un usuario](query-for-products.md) o [notificar un producto consumible como satisfecho](report-consumable-products-as-fulfilled.md).

1.  Pase el Azure AD token de acceso que tiene el valor `https://onestore.microsoft.com/b2b/keys/create/collections` de URI de audiencia de su servicio a la aplicación cliente. Este es uno de los tokens que creó [anteriormente en el paso 3](#step-3).

2.  En el código de la aplicación, llame a uno de estos métodos para recuperar una clave de identificador de Microsoft Store:

  * Si tu aplicación usa la clase [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) en el espacio de nombres [Windows. Services. Store](https://docs.microsoft.com/uwp/api/windows.services.store) para administrar las compras desde la aplicación, usa el método [StoreContext. GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomercollectionsidasync) .

  * Si la aplicación usa la clase [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) en el espacio de nombres [Windows. ApplicationModel. Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) para administrar las compras desde la aplicación, use el método [CurrentApp. GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomercollectionsidasync) .

    Pase el token de acceso Azure AD al parámetro *serviceTicket* del método. Si mantiene identificadores de usuario anónimos en el contexto de los servicios que administra como el publicador de la aplicación actual, también puede pasar un identificador de usuario al parámetro *publisherUserId* para asociar el usuario actual con la nueva clave de identificador de Microsoft Store (el identificador de usuario se insertará en la clave). De lo contrario, si no necesita asociar un identificador de usuario con la clave de identificador de Microsoft Store, puede pasar cualquier valor de cadena al parámetro *publisherUserId* .

3.  Una vez que la aplicación crea correctamente una clave de identificador de Microsoft Store, vuelva a pasar la clave al servicio.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>Para crear una clave de identificador de Microsoft Store para la API de compra de Microsoft Store

Siga estos pasos para crear una clave de identificador de Microsoft Store que puede usar con la API de Microsoft Store Purchase para [conceder un producto gratuito a un usuario](grant-free-products.md), [obtener suscripciones para un usuario](get-subscriptions-for-a-user.md)o [cambiar el estado de facturación de una suscripción para un usuario](change-the-billing-state-of-a-subscription-for-a-user.md).

1.  Pase el Azure AD token de acceso que tiene el valor `https://onestore.microsoft.com/b2b/keys/create/purchase` de URI de audiencia de su servicio a la aplicación cliente. Este es uno de los tokens que creó [anteriormente en el paso 3](#step-3).

2.  En el código de la aplicación, llame a uno de estos métodos para recuperar una clave de identificador de Microsoft Store:

  * Si tu aplicación usa la clase [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) en el espacio de nombres [Windows. Services. Store](https://docs.microsoft.com/uwp/api/windows.services.store) para administrar las compras desde la aplicación, usa el método [StoreContext. GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync) .

  * Si la aplicación usa la clase [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) en el espacio de nombres [Windows. ApplicationModel. Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) para administrar las compras desde la aplicación, use el método [CurrentApp. GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync) .

    Pase el token de acceso Azure AD al parámetro *serviceTicket* del método. Si mantiene identificadores de usuario anónimos en el contexto de los servicios que administra como el publicador de la aplicación actual, también puede pasar un identificador de usuario al parámetro *publisherUserId* para asociar el usuario actual con la nueva clave de identificador de Microsoft Store (el identificador de usuario se insertará en la clave). De lo contrario, si no necesita asociar un identificador de usuario con la clave de identificador de Microsoft Store, puede pasar cualquier valor de cadena al parámetro *publisherUserId* .

3.  Una vez que la aplicación crea correctamente una clave de identificador de Microsoft Store, vuelva a pasar la clave al servicio.

### <a name="diagram"></a>Diagrama

En el diagrama siguiente se ilustra el proceso de creación de una clave de identificador de Microsoft Store.

  ![Crear clave de identificador de la tienda Windows](images/b2b-1.png)

<span id="step-5"/>

## <a name="step-5-call-the-microsoft-store-collection-api-or-purchase-api-from-your-service"></a>Paso 5: llamar a la API de colección Microsoft Store o a la API de compra desde el servicio

Una vez que el servicio tiene una clave de identificador Microsoft Store que le permite tener acceso a la información de propiedad de un usuario específico, el servicio puede llamar a la API de colección Microsoft Store o a la API de compra siguiendo estas instrucciones:

* [Consultar productos](query-for-products.md)
* [Notificar productos consumibles como completados](report-consumable-products-as-fulfilled.md)
* [Conceder productos gratuitos](grant-free-products.md)
* [Obtener suscripciones de un usuario](get-subscriptions-for-a-user.md)
* [Cambiar el estado de facturación de la suscripción de un usuario](change-the-billing-state-of-a-subscription-for-a-user.md)

Para cada escenario, pasa la siguiente información a la API:

-   En el encabezado de solicitud, pase el Azure AD token de acceso que tiene el valor de URI de audiencia `https://onestore.microsoft.com` . Este es uno de los tokens que creó [anteriormente en el paso 3](#step-3). Este token representa tu identidad de publicador.
-   En el cuerpo de la solicitud, pase la clave de identificador de Microsoft Store que recuperó [anteriormente en el paso 4](#step-4) desde el código de cliente de la aplicación. Esta clave representa la identidad del usuario a cuya información de propiedad del producto deseas acceder.

### <a name="diagram"></a>Diagrama

En el diagrama siguiente se describe el proceso de llamar a un método en la API de colección Microsoft Store o la API de compra desde el servicio.

  ![Llamar a colecciones o API de compra](images/b2b-2.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Notificaciones en una clave de identificador de Microsoft Store

Una clave de identificador de Microsoft Store es un JSON Web Token (JWT) que representa la identidad del usuario cuya información de propiedad del producto desea obtener. Cuando se descodifica con base64, una clave de identificador de Microsoft Store contiene las siguientes notificaciones.

* `iat`: &nbsp; &nbsp; &nbsp; Identifica la hora a la que se emitió la clave. Esta notificación puede usarse para determinar la edad del token. Este valor se expresa como la época.
* `iss`: &nbsp; &nbsp; &nbsp; Identifica al emisor. Tiene el mismo valor que la `aud` demanda.
* `aud`: &nbsp; &nbsp; &nbsp; Identifica la audiencia. Deber ser uno de los siguientes valores: `https://collections.mp.microsoft.com/v6.0/keys` o `https://purchase.mp.microsoft.com/v6.0/keys`.
* `exp`: &nbsp; &nbsp; &nbsp; Identifica la hora de expiración en o después de la cual ya no se aceptará la clave para el procesamiento de nada, excepto para renovar las claves. El valor de esta notificación se expresa como el tiempo de la época.
* `nbf`: &nbsp; &nbsp; &nbsp; Identifica la hora a la que se aceptará el token para el procesamiento. El valor de esta notificación se expresa como el tiempo de la época.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`: &nbsp; &nbsp; &nbsp; El identificador de cliente que identifica al desarrollador.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`: &nbsp; &nbsp; &nbsp; Una carga opaca (cifrada y codificada en Base64) que contiene información que solo está destinada a su uso por parte de Microsoft Store Services.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`: &nbsp; &nbsp; &nbsp; Un identificador de usuario que identifica al usuario actual en el contexto de los servicios. Este es el mismo valor que se pasa al parámetro opcional *publisherUserId* del [método que se usa para crear la clave](#step-4).
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`: &nbsp; &nbsp; &nbsp; El URI que puede usar para renovar la clave.

A continuación se muestra un ejemplo de un encabezado de clave de identificador de Microsoft Store descodificado.

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

Este es un ejemplo de un conjunto de notificaciones de clave de Microsoft Store de identificador descodificado.

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
* [Obtener suscripciones de un usuario](get-subscriptions-for-a-user.md)
* [Cambiar el estado de facturación de la suscripción de un usuario](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Renovar una clave de identificador de Microsoft Store](renew-a-windows-store-id-key.md)
* [Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app)
* [Descripción del manifiesto de aplicación de Azure Active Directory]( https://go.microsoft.com/fwlink/?LinkId=722500)
* [Tipos de notificaciones y tokens admitidos](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
