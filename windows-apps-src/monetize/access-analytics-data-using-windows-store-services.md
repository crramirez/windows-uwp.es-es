---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Use la API de análisis de Microsoft Store para recuperar mediante programación los datos de análisis de las aplicaciones que están registradas en la cuenta del centro de Partners de Windows de la organización.
title: Acceder a datos de análisis mediante servicios de almacenamiento
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de Analytics
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 51b0180d6b550cda082b5ee10530194824a48e33
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846805"
---
# <a name="access-analytics-data-using-store-services"></a>Acceder a datos de análisis mediante servicios de almacenamiento

Use la *API de análisis de Microsoft Store* para recuperar mediante programación los datos de análisis de las aplicaciones que están registradas en su cuenta del centro de Partners de Windows de su organización. Esta API permite recuperar los datos respecto a las adquisiciones de aplicaciones y de complementos (conocidas también como producto desde la aplicación o IAP), errores, valoraciones de la aplicación y opiniones. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas provenientes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
2.  Antes de llamar a un método en la API de Microsoft Store Analytics, [obtenga un token de acceso Azure ad](#obtain-an-azure-ad-access-token). Después de obtener un token, tiene 60 minutos para usar este token en las llamadas a la API de Microsoft Store Analytics antes de que expire el token. Después de que el token expire, puedes generar uno nuevo.
3.  [Llame a la API de análisis de Microsoft Store](#call-the-windows-store-analytics-api).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>Paso 1: completar los requisitos previos para usar la API de Microsoft Store Analytics

Antes de empezar a escribir código para llamar a la API de Microsoft Store Analytics, asegúrese de que ha completado los requisitos previos siguientes.

* Usted (o su organización) tiene que tener un directorio de Azure AD y el permiso de [Administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para el directorio. Si usa Microsoft 365 u otros servicios empresariales de Microsoft, ya tiene el directorio de Azure AD. De lo contrario, puede [crear un nuevo Azure ad en el centro de Partners](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sin cargo adicional.

* Debe asociar una aplicación Azure AD a la cuenta del centro de Partners, recuperar el identificador de inquilino y el identificador de cliente de la aplicación y generar una clave. La aplicación Azure AD representa la aplicación o el servicio del que desea llamar a la API de Microsoft Store Analytics. Necesita el identificador de inquilino, el identificador de cliente y la clave para obtener un token de acceso de Azure AD para pasar a la API.
    > [!NOTE]
    > Solo tiene que realizar esta tarea una vez. Una vez que tenga el identificador de inquilino, el identificador de cliente y la clave, puede volver a usarlos cada vez que tenga que crear un nuevo token de acceso de Azure AD.

Para asociar una aplicación Azure AD a la cuenta del centro de Partners y recuperar los valores necesarios:

1.  En el Centro de partners, [asocie la cuenta del Centro de partners de la organización con el directorio de Azure AD de la organización](../publish/associate-azure-ad-with-partner-center.md).

2.  A continuación, en la página **usuarios** de la sección **configuración** de la cuenta del centro de partners, [agregue la Azure ad aplicación](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) que representa la aplicación o el servicio que usará para acceder a los datos de análisis de la cuenta del centro de Partners. Asegúrese de asignar a esta aplicación el rol **Administrador**. Si la aplicación aún no existe en el directorio de Azure AD, puede [crear una nueva aplicación de Azure AD en el Centro de partners](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).

3.  Vuelva a la página **Usuarios**, haga clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y, a continuación, copie los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haga clic en **Agregar nueva clave**. En la pantalla siguiente, copie el valor de **Clave**. Después de salir de esta página no podrá tener acceso de nuevo a esta información. Para más información, consulte [Administrar claves para una aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtención de un token de acceso de Azure AD

Antes de llamar a cualquiera de los métodos de la API de Microsoft Store Analytics, primero debe obtener un token de acceso Azure AD que pase al encabezado **Authorization** de cada método de la API. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizar el token para que puedas continuar usándolo en llamadas adicionales a la API.

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

Para el valor de * \_ identificador de inquilino* en el URI de post y los parámetros de * \_ identificador de cliente* y * \_ secreto de cliente* , especifique el identificador de inquilino, el identificador de cliente y la clave de la aplicación que recuperó del centro de Partners en la sección anterior. Para el parámetro *resource*, tiene que especificar ```https://manage.devcenter.microsoft.com```.

Una vez que expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>Paso 3: llamar a la API de análisis de Microsoft Store

Una vez que tenga un Azure AD token de acceso, estará listo para llamar a la API de Microsoft Store Analytics. Debes pasar el token de acceso al encabezado **Authorization** de cada método.

### <a name="methods-for-uwp-apps-and-games"></a>Métodos para aplicaciones y juegos para UWP
Los siguientes métodos están disponibles para las adquisiciones de aplicaciones y juegos y las adquisiciones de complementos: 

* [Obtener datos de adquisiciones para sus aplicaciones y juegos](acquisitions-data.md)
* [Obtener datos de adquisiciones de complementos para sus aplicaciones y juegos](add-on-acquisitions-data.md)

### <a name="methods-for-uwp-apps"></a>Métodos para aplicaciones para UWP 

Los siguientes métodos de análisis están disponibles para las aplicaciones UWP del centro de Partners.

| Escenario       | Métodos      |
|---------------|--------------------|
| Adquisiciones, conversiones, instalaciones y uso |  <ul><li>[Obtener adquisiciones de aplicaciones](get-app-acquisitions.md) (heredado)</li><li>[Obtener datos de embudo de adquisición](get-acquisition-funnel-data.md) de la aplicación (heredado)</li><li>[Obtener conversiones de aplicaciones por canal](get-app-conversions-by-channel.md)</li><li>[Obtener los datos de las adquisiciones de complementos](get-in-app-acquisitions.md)</li><li>[Obtener adquisiciones de complementos de suscripción](get-subscription-acquisitions.md)</li><li>[Obtener conversiones de complementos por canal](get-add-on-conversions-by-channel.md)</li><li>[Obtener instalaciones de aplicaciones](get-app-installs.md)</li><li>[Obtener el uso diario de la aplicación](get-app-usage-daily.md)</li><li>[Obtener el uso mensual de la aplicación](get-app-usage-monthly.md)</li></ul> |
| Errores de aplicación | <ul><li>[Obtener los datos del informe de errores](get-error-reporting-data.md)</li><li>[Obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md)</li><li>[Obtener el seguimiento de la pila de un error en la aplicación](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[Descargar el archivo CAB asociado a un error en la aplicación](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Información detallada | <ul><li>[Obtener datos de información de la aplicación](get-insights-data-for-your-app.md)</li></ul>  |
| Calificaciones y reseñas | <ul><li>[Obtener las valoraciones de la aplicación](get-app-ratings.md)</li><li>[Obtener las opiniones de la aplicación](get-app-reviews.md)</li></ul> |
| Anuncios en la aplicación y campañas de anuncios | <ul><li>[Obtener los datos de rendimiento de los anuncios](get-ad-performance-data.md)</li><li>[Obtener los datos de rendimiento de la campaña de anuncios](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>Métodos para aplicaciones de escritorio

Los siguientes métodos de análisis están disponibles para su uso por parte de las cuentas de desarrollador que pertenecen al [programa de aplicación de escritorio de Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program).

| Escenario       | Métodos      |
|---------------|--------------------|
| Instala . |  <ul><li>[Obtener instalaciones de aplicaciones de escritorio](get-desktop-app-installs.md)</li></ul> |
| Blocks |  <ul><li>[Obtener bloques de actualización de la aplicación de escritorio](get-desktop-block-data.md)</li><li>[Obtener detalles de bloque de actualización de la aplicación de escritorio](get-desktop-block-data-details.md)</li></ul> |
| Errores de aplicación |  <ul><li>[Obtener datos de informes de errores de la aplicación de escritorio](get-desktop-application-error-reporting-data.md)</li><li>[Obtener los detalles asociados a un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md)</li><li>[Obtener el seguimiento de pila asociado a un error en la aplicación de escritorio](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[Descargar el archivo CAB asociado a un error en la aplicación de escritorio](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Información detallada | <ul><li>[Obtener información detallada de la aplicación de escritorio](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Métodos para los servicios de Xbox Live

Los siguientes métodos adicionales están disponibles para su uso por parte de las cuentas de desarrollador con juegos que usan los [servicios de Xbox Live](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md).

| Escenario       | Métodos      |
|---------------|--------------------|
| Análisis general |  <ul><li>[Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)</li><li>[Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)</li><li>[Obtener datos de uso simultáneo de Xbox Live](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| Análisis de estado |  <ul><li>[Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)</li></ul> |
| Análisis de la comunidad |  <ul><li>[Obtener datos del hub de juegos de Xbox Live](get-xbox-live-game-hub-data.md)</li><li>[Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)</li><li>[Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-hardware-and-drivers"></a>Métodos para hardware y controladores

Las cuentas de desarrollador que pertenecen al [programa del panel de hardware de Windows](https://docs.microsoft.com/windows-hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) tienen acceso a un conjunto adicional de métodos para recuperar datos de análisis del hardware y los controladores. Para obtener más información, consulte [API del panel de hardware](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api).

## <a name="code-example"></a>Ejemplo de código

En el ejemplo de código siguiente se muestra cómo obtener un token de acceso Azure AD y cómo llamar a la API de Microsoft Store Analytics desde una aplicación de consola de C#. Para usar este ejemplo de código, asigna las variables *tenantId*, *clientId*, *clientSecret* y *appID* a los valores adecuados de tu escenario. Este ejemplo requiere el [paquete JSON.net](https://www.newtonsoft.com/json) de Newtonsoft para deserializar los datos JSON devueltos por la API de Microsoft Store Analytics.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>Respuestas de errores

Microsoft Store Analytics API devuelve respuestas de error en un objeto JSON que contiene códigos y mensajes de error. En el ejemplo siguiente se muestra una respuesta de error producida por un parámetro no válido.

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
