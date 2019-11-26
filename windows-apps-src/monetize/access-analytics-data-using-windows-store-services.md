---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Use la API de análisis de Microsoft Store para recuperar mediante programación los datos de análisis de las aplicaciones que están registradas en la cuenta del centro de Partners de Windows de la organización.
title: Acceder a los datos de análisis mediante los servicios de la Store
ms.date: 03/06/2019
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Store, Microsoft Store analytics API, API de análisis de Microsoft Store
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 71c59049b76219d6f9360748e9ca11ea84542e47
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259315"
---
# <a name="access-analytics-data-using-store-services"></a>Acceder a los datos de análisis mediante los servicios de la Store

Use la *API de análisis de Microsoft Store* para recuperar mediante programación los datos de análisis de las aplicaciones que están registradas en su cuenta del centro de Partners de Windows de su organización. Esta API permite recuperar los datos respecto a las adquisiciones de aplicaciones y de complementos (conocidas también como producto desde la aplicación o IAP), errores, valoraciones de la aplicación y opiniones. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas provenientes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
2.  Antes de llamar a un método en la API de análisis de Microsoft Store [obtén un token de acceso de Azure AD](#obtain-an-azure-ad-access-token). Después de obtener un token, tienes 60 minutos para utilizar dicho token en llamadas a la API de análisis de Microsoft Store antes de que expire. Después de que el token expire, puedes generar uno nuevo.
3.  [Llama a la API de análisis de Microsoft Store](#call-the-windows-store-analytics-api).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>Paso 1: Completar los requisitos previos para usar la API de análisis de Microsoft Store

Antes de empezar a escribir código para llamar a la API de análisis de Microsoft Store, asegúrate de que has completado los siguientes requisitos previos.

* Tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para el directorio. Si ya usas Office 365 u otros servicios empresariales de Microsoft, ya tienes un directorio de Azure AD. De lo contrario, puede [crear un nuevo Azure ad en el centro de Partners](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sin cargo adicional.

* Debe asociar una aplicación Azure AD a la cuenta del centro de Partners, recuperar el identificador de inquilino y el identificador de cliente de la aplicación y generar una clave. La aplicación de Azure AD representa la aplicación o el servicio desde donde quieres originar la llamada a la API de análisis de Microsoft Store. Necesitas el identificador de inquilino, de cliente y la clave para obtener un token de acceso de Azure AD que se pasa a la API.
    > [!NOTE]
    > Solo debes realizar esta tarea una vez. Una vez que tengas el identificador de inquilino, de cliente y la clave, puedes volver a usarlos cuando necesites crear un nuevo token de acceso de Azure AD.

Para asociar una aplicación Azure AD a la cuenta del centro de Partners y recuperar los valores necesarios:

1.  En el centro de Partners, [asocie la cuenta del centro de Partners de su organización con el directorio de Azure ad de su organización](../publish/associate-azure-ad-with-partner-center.md).

2.  A continuación, en la página **usuarios** de la sección **configuración** de la cuenta del centro de partners, [agregue la Azure ad aplicación](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) que representa la aplicación o el servicio que usará para acceder a los datos de análisis de la cuenta del centro de Partners. Asegúrate de que se asignas a esta aplicación el rol de **Administrador**. Si la aplicación aún no existe en el directorio de Azure AD, puede [crear una nueva aplicación de Azure ad en el centro de Partners](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).

3.  Vuelve a la página **Usuarios**, haz clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y copia los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haz clic en **Agregar nueva clave**. En la siguiente pantalla, copia el valor **Clave**. No podrás acceder a esta información de nuevo después de salir de la página. Para obtener más información, consulta [Administrar claves para una aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtención de un token de acceso de Azure AD

Antes de llamar a cualquiera de los métodos en la API de análisis de Microsoft Store, debes obtener un token de acceso de Azure AD para pasarlo al encabezado **Authorization** de cada método en la API. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizar el token para que puedas continuar usándolo en llamadas adicionales a la API.

Para obtener el token de acceso, sigue las instrucciones en [Llamadas de servicio a servicio utilizando las credenciales del cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar un HTTP POST al punto de conexión ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Este es un ejemplo de solicitud.

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Para el valor de *identificador de\_de inquilino* en el URI de entrada y los parámetros de cliente *\_ID.* y *cliente\_secreto* , especifique el identificador de inquilino, el identificador de cliente y la clave de la aplicación que recuperó del centro de Partners en la sección anterior. Para el parámetro *resource*, debes especificar ```https://manage.devcenter.microsoft.com```.

Una vez que expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>Paso 3: Llama a la API de análisis de Microsoft Store

Cuando tengas un token de acceso de Azure AD, podrás llamar a la API de análisis de Microsoft Store. Debes pasar el token de acceso al encabezado **Authorization** de cada método.

### <a name="methods-for-uwp-apps-and-games"></a>Métodos para aplicaciones y juegos para UWP
Los siguientes métodos están disponibles para las adquisiciones de aplicaciones y juegos y las adquisiciones de complementos: 

* [Obtención de datos de adquisiciones para juegos y aplicaciones](acquisitions-data.md)
* [Obtención de datos de adquisiciones de complementos para juegos y aplicaciones](add-on-acquisitions-data.md)

### <a name="methods-for-uwp-apps"></a>Métodos para las aplicaciones para UWP 

Los siguientes métodos de análisis están disponibles para las aplicaciones UWP del centro de Partners.

| Escenario       | Métodos      |
|---------------|--------------------|
| Adquisiciones, conversiones, instalaciones y uso |  <ul><li>[Obtener adquisiciones de aplicaciones](get-app-acquisitions.md) (heredado)</li><li>[Obtener datos de embudo de adquisición](get-acquisition-funnel-data.md) de la aplicación (heredado)</li><li>[Obtención de conversiones de aplicaciones por canal](get-app-conversions-by-channel.md)</li><li>[Obtención de adquisiciones de complementos](get-in-app-acquisitions.md)</li><li>[Obtención de adquisiciones del complemento de suscripción](get-subscription-acquisitions.md)</li><li>[Obtener conversiones de complementos por canal](get-add-on-conversions-by-channel.md)</li><li>[Obtener instalaciones de la aplicación](get-app-installs.md)</li><li>[Obtención diaria de uso de la aplicación](get-app-usage-daily.md)</li><li>[Obtener uso mensual de la aplicación](get-app-usage-monthly.md)</li></ul> |
| Errores de la aplicación | <ul><li>[Obtener datos de informes de errores](get-error-reporting-data.md)</li><li>[Obtención de los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md)</li><li>[Obtención del seguimiento de la pila para un error en la aplicación](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[Descargar el archivo. CAB para un error en la aplicación](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Insights | <ul><li>[Obtención de datos de insights para la aplicación](get-insights-data-for-your-app.md)</li></ul>  |
| Calificaciones y opiniones | <ul><li>[Obtener clasificaciones de aplicaciones](get-app-ratings.md)</li><li>[Obtener revisiones de la aplicación](get-app-reviews.md)</li></ul> |
| Anuncios en la aplicación y campañas de anuncios | <ul><li>[Obtención de datos de rendimiento de ad](get-ad-performance-data.md)</li><li>[Obtención de datos de rendimiento de campaña de ad](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>Métodos para aplicaciones de escritorio

Los siguientes métodos de análisis están disponibles para que las cuentas de desarrollador que pertenecen al [Programa de aplicaciones de escritorio de Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) puedan usarlos.

| Escenario       | Métodos      |
|---------------|--------------------|
| Instalaciones |  <ul><li>[Obtener instalaciones de aplicaciones de escritorio](get-desktop-app-installs.md)</li></ul> |
| Catch |  <ul><li>[Obtener los bloques de actualización de la aplicación de escritorio](get-desktop-block-data.md)</li><li>[Obtención de detalles del bloque de actualización para la aplicación de escritorio](get-desktop-block-data-details.md)</li></ul> |
| Errores de aplicaciones |  <ul><li>[Obtención de datos de informes de errores para la aplicación de escritorio](get-desktop-application-error-reporting-data.md)</li><li>[Obtener detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md)</li><li>[Obtención del seguimiento de la pila para un error en la aplicación de escritorio](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[Descargar el archivo. CAB para un error en la aplicación de escritorio](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Insights | <ul><li>[Obtención de datos de insights para la aplicación de escritorio](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Métodos para los servicios de Xbox Live

Los siguientes métodos adicionales están disponibles para que las usen las cuentas de desarrollador con los juegos que usan [servicios de Xbox Live ](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md).

| Escenario       | Métodos      |
|---------------|--------------------|
| Análisis general |  <ul><li>[Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)</li><li>[Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)</li><li>[Obtener datos de uso simultáneo de Xbox Live](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| Análisis de estado |  <ul><li>[Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)</li></ul> |
| Análisis de la comunidad |  <ul><li>[Obtener datos del concentrador de juegos de Xbox Live](get-xbox-live-game-hub-data.md)</li><li>[Obtener datos del Club de Xbox Live](get-xbox-live-club-data.md)</li><li>[Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-xbox-one-games"></a>Métodos para juegos de Xbox One

Los siguientes métodos adicionales están disponibles para su uso por parte de las cuentas de desarrollador con juegos de Xbox One que se han ingerido a través del portal para desarrolladores de Xbox (XDP) y están disponibles en el panel de XDP Analytics.

| Escenario       | Métodos      |
|---------------|--------------------|
| Adquisiciones |  <ul><li>[Obtener adquisiciones de juegos de Xbox One](get-xbox-one-game-acquisitions.md)</li><li>[Obtener adquisiciones de complementos de Xbox One](get-xbox-one-add-on-acquisitions.md)</li></ul> |
| Errores |  <ul><li>[Obtener datos de informes de errores para el juego Xbox One](get-error-reporting-data-for-your-xbox-one-game.md)</li><li>[Obtener detalles de un error en el juego Xbox One](get-details-for-an-error-in-your-xbox-one-game.md)</li><li>[Obtención del seguimiento de la pila para un error en el juego Xbox One](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)</li><li>[Descargar el archivo. CAB para un error en el juego Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)</li></ul> |

### <a name="methods-for-hardware-and-drivers"></a>Métodos para hardware y controladores

Las cuentas de desarrollador que pertenecen al [programa del panel de hardware de Windows](https://docs.microsoft.com/windows-hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) tienen acceso a un conjunto adicional de métodos para recuperar datos de análisis del hardware y los controladores. Para obtener más información, consulte [API del panel de hardware](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api).

## <a name="code-example"></a>Ejemplo de código

El siguiente ejemplo de código muestra cómo obtener un token de acceso de Azure AD y llamar a la API de análisis de Microsoft Store desde una aplicación de consola C#. Para usar este ejemplo de código, asigna las variables *tenantId*, *clientId*, *clientSecret* y *appID* a los valores adecuados de tu escenario. En este ejemplo se necesita el [paquete Json.NET](https://www.newtonsoft.com/json) de Newtonsoft para deserializar los datos de JSON devueltos por la API de análisis de Microsoft Store.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>Respuestas de error

La API de análisis de Microsoft Store devuelve respuestas de error en un objeto JSON que contiene mensajes y códigos de error. En el ejemplo siguiente se muestra una respuesta de error producida por un parámetro no válido.

```json
{
    "code":"BadRequest",
    "data":[],
    "details":[],
    "innererror":{
        "code":"InvalidQueryParameters",
        "data":[
            "top parameter cannot be more than 10000"
        ],
        "details":[],
        "message":"One or More Query Parameters has invalid values.",
        "source":"AnalyticsAPI"
    },
    "message":"The calling client sent a bad request to the service.",
    "source":"AnalyticsAPI"
}
```
