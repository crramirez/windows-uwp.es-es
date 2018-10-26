---
author: Xansky
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Usa la API de análisis de Microsoft Store para recuperar datos de análisis mediante programación para las aplicaciones que se registran en tu cuenta del Centro de desarrollo de Windows o la de tu organización.
title: Acceder a los datos de análisis mediante los servicios de la Store
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Store, Microsoft Store analytics API, API de análisis de Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: ce72846a8f7ea5877e2d8b944a6231d526ea0166
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5611409"
---
# <a name="access-analytics-data-using-store-services"></a>Acceder a los datos de análisis mediante los servicios de la Store

Usa la *API de análisis de Microsoft Store* para recuperar datos de análisis mediante programación para las aplicaciones que se registran en tu cuenta del Centro de desarrollo de Windows o la de tu organización. Esta API permite recuperar los datos respecto a las adquisiciones de aplicaciones y de complementos (conocidas también como producto desde la aplicación o IAP), errores, valoraciones de la aplicación y opiniones. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas procedentes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
2.  Antes de llamar a un método en la API de análisis de Microsoft Store [obtén un token de acceso de Azure AD](#obtain-an-azure-ad-access-token). Después de obtener un token, tienes 60minutos para utilizar dicho token en llamadas a la API de análisis de Microsoft Store antes de que expire. Después de que el token expire, puedes generar un nuevo token.
3.  [Llama a la API de análisis de Microsoft Store](#call-the-windows-store-analytics-api).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>Paso 1: Completar los requisitos previos para usar la API de análisis de Microsoft Store

Antes de empezar a escribir código para llamar a la API de análisis de Microsoft Store, asegúrate de que has completado los siguientes requisitos previos.

* Tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para el directorio. Si ya usas Office365 u otros servicios empresariales de Microsoft, ya tienes un directorio de AzureAD. De lo contrario, puedes [crear un nuevo Azure AD desde el Centro de desarrollo](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account) sin ningún coste adicional.

* Debes asociar una aplicación de Azure AD con tu cuenta del Centro de desarrollo, recuperar el identificador de inquilino y el identificador de cliente correspondientes a la aplicación y generar la clave. La aplicación de Azure AD representa la aplicación o el servicio desde donde quieres originar la llamada a la API de análisis de Microsoft Store. Necesitas el identificador de inquilino, el identificador de cliente y la clave para obtener un token de acceso de Azure AD que se pasa a la API.
    > [!NOTE]
    > Solo debes realizar esta tarea una vez. Una vez que tengas el identificador de inquilino, el identificador de cliente y la clave, puedes volver a usarlos siempre que necesites crear un nuevo token de acceso de Azure AD.

Para asociar una aplicación de Azure AD con la cuenta del Centro de desarrollo y recuperar los valores requeridos:

1.  En el Centro de desarrollo, [asocia tu cuenta del Centro de desarrollo de la organización con tu directorio de Azure AD de la organización](../publish/associate-azure-ad-with-dev-center.md).

2.  A continuación, desde la página **Usuarios** en la sección **Configuración de la cuenta** del Centro de desarrollo, [añada la aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-dev-center-account) que representa la aplicación o el servicio que usarás para acceder a datos de análisis de tu cuenta del Centro de desarrollo. Asegúrate de que se asignas a esta aplicación el rol de **Administrador**. Si la aplicación no existe aún en el directorio de Azure AD, puedes [crear una nueva aplicación de Azure AD en el Centro de desarrollo](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account).

3.  Vuelve a la página **Usuarios**, haz clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y copia los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haz clic en **Agregar nueva clave**. En la siguiente pantalla, copia el valor **Clave**. No podrás acceder a esta información de nuevo después de salir de la página. Para obtener más información, consulta [Administrar claves para una aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtención de un token de acceso de Azure AD

Antes de llamar a cualquiera de los métodos en la API de análisis de Microsoft Store, debes obtener un token de acceso de Azure AD para pasarlo al encabezado **Authorization** de cada método en la API. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizar el token para que puedas continuar usándolo en llamadas adicionales a la API.

Para obtener el token de acceso, sigue las instrucciones en [Llamadas de servicio a servicio utilizando las credenciales del cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar un HTTP POST al punto de conexión ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Este es un ejemplo de solicitud.

```
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Para el valor de *tenant\_id* de POST URI y los parámetros *client\_id* y *client\_secret*, especifica el identificador de inquilino, de cliente y la clave para la aplicación que recuperaste del Centro de desarrollo en la sección anterior. Para el parámetro *resource*, debes especificar ```https://manage.devcenter.microsoft.com```.

Una vez que expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>Paso 3: Llama a la API de análisis de Microsoft Store

Cuando tengas un token de acceso de Azure AD, podrás llamar a la API de análisis de Microsoft Store. Debes pasar el token de acceso al encabezado **Authorization** de cada método.

### <a name="methods-for-uwp-apps"></a>Métodos para las aplicaciones para UWP

Los siguientes métodos de análisis están disponibles para aplicaciones para UWP en el Centro de desarrollo.

| Escenario       | Métodos      |
|---------------|--------------------|
| Adquisiciones, conversiones, instalaciones y uso |  <ul><li>[Obtener adquisiciones de la aplicación](get-app-acquisitions.md)</li><li>[Obtener datos de embudo de adquisiciones de aplicaciones](get-acquisition-funnel-data.md)</li><li>[Obtener conversiones de aplicaciones por canal](get-app-conversions-by-channel.md)</li><li>[Obtener adquisiciones de complementos](get-in-app-acquisitions.md)</li><li>[Obtener adquisiciones de complementos de suscripción](get-subscription-acquisitions.md)</li><li>[Obtener conversiones de complementos por canal](get-add-on-conversions-by-channel.md)</li><li>[Obtener instalaciones de la aplicación](get-app-installs.md)</li><li>[Obtener el uso diario de la aplicación](get-app-usage-daily.md)</li><li>[Obtener el uso mensual de la aplicación](get-app-usage-monthly.md)</li></ul> |
| Errores de la aplicación | <ul><li>[Obtener datos de informes de errores](get-error-reporting-data.md)</li><li>[Obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md)</li><li>[Obtener el seguimiento de la pila de un error en la aplicación](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[Descargar el archivo CAB de un error en tu aplicación](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Perspectivas | <ul><li>[Obtener datos de información de la aplicación](get-insights-data-for-your-app.md)</li></ul>  |
| Calificaciones y opiniones | <ul><li>[Obtener clasificaciones de la aplicación](get-app-ratings.md)</li><li>[Obtener opiniones de la aplicación](get-app-reviews.md)</li></ul> |
| Anuncios en la aplicación y campañas de anuncios | <ul><li>[Obtener los datos de rendimiento de los anuncios](get-ad-performance-data.md)</li><li>[Obtener los datos de rendimiento de la campaña publicitaria](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>Métodos para aplicaciones de escritorio

Los siguientes métodos de análisis están disponibles para que las cuentas de desarrollador que pertenecen al [Programa de aplicaciones de escritorio de Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504) puedan usarlos.

| Escenario       | Métodos      |
|---------------|--------------------|
| Instalaciones |  <ul><li>[Obtener instalaciones de aplicaciones de escritorio](get-desktop-app-installs.md)</li></ul> |
| Bloques |  <ul><li>[Obtener bloques de actualización de la aplicación de escritorio](get-desktop-block-data.md)</li><li>[Obtener detalles de bloque de actualización de la aplicación de escritorio](get-desktop-block-data-details.md)</li></ul> |
| Errores de aplicaciones |  <ul><li>[Obtener datos de informes de errores para la aplicación de escritorio](get-desktop-application-error-reporting-data.md)</li><li>[Obtener los detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md)</li><li>[Obtener el seguimiento de la pila de un error en la aplicación de escritorio](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[Descargar el archivo CAB de un error en tu aplicación de escritorio](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Perspectivas | <ul><li>[Obtener datos de información de la aplicación de escritorio](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Métodos para los servicios de Xbox Live

Los siguientes métodos adicionales están disponibles para que las usen las cuentas de desarrollador con los juegos que usan [servicios de Xbox Live ](../xbox-live/developer-program-overview.md).

| Escenario       | Métodos      |
|---------------|--------------------|
| Análisis general |  <ul><li>[Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)</li><li>[Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)</li><li>[Obtener datos de uso simultáneo de Xbox Live](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| Análisis de estado |  <ul><li>[Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)</li></ul> |
| Análisis de la comunidad |  <ul><li>[Obtener datos del hub de juegos de Xbox Live](get-xbox-live-game-hub-data.md)</li><li>[Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)</li><li>[Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-xbox-one-games"></a>Métodos para juegos de Xbox One

Los siguientes métodos adicionales están disponibles para que los utilicen cuentas de desarrollador de juegos de Xbox One añadidos a través del Portal de desarrollador de Xbox (XDP) y disponibles en el panel del Centro de desarrollo de Análisis de XDP.

| Escenario       | Métodos      |
|---------------|--------------------|
| Adquisiciones |  <ul><li>[Obtener adquisiciones de juegos de Xbox One](get-xbox-one-game-acquisitions.md)</li></ul> |

### <a name="methods-for-hardware-and-drivers"></a>Métodos para hardware y controladores

Las cuentas de desarrollador que pertenecen al [programa centro de desarrollo de Hardware de Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) tienen acceso a un conjunto adicional de métodos para recuperar datos de análisis de hardware y controladores. Para obtener más información, consulta [API del panel de Hardware](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api).

## <a name="code-example"></a>Ejemplo de código

El siguiente ejemplo de código muestra cómo obtener un token de acceso de Azure AD y llamar a la API de análisis de Microsoft Store desde una aplicación de consola C#. Para usar este ejemplo de código, asigna las variables *tenantId*, *clientId*, *clientSecret* y *appID* a los valores adecuados de tu escenario. En este ejemplo se necesita el [paquete Json.NET](http://www.newtonsoft.com/json) de Newtonsoft para deserializar los datos de JSON devueltos por la API de análisis de Microsoft Store.

> [!div class="tabbedCodeSnippets"]
[!code-cs[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

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
