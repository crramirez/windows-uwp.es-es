---
author: mcleanbyron
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: "Usa la API de análisis de la Tienda Windows para recuperar datos de análisis mediante programación para las aplicaciones que se registran en tu cuenta del Centro de desarrollo de Windows o la de tu organización."
title: "Acceder a los datos de análisis mediante los servicios de la Tienda"
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, Windows 10, uwp, UWP, Store services, servicios de la Tienda, Windows Store analytics API, API de análisis de la Tienda Windows"
ms.openlocfilehash: aa33af63a49d890b3c60ec1bee32528cfc78af93
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="access-analytics-data-using-store-services"></a>Acceder a los datos de análisis mediante los servicios de la Tienda

Usa la *API de análisis de la Tienda Windows* para recuperar datos de análisis mediante programación referentes a las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows o en la de tu organización. Esta API permite recuperar los datos respecto a las adquisiciones de aplicaciones y de complementos (conocidas también como producto desde la aplicación o IAP), errores, valoraciones de la aplicación y opiniones. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas procedentes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
2.  Antes de llamar a un método en la API de análisis de la Tienda Windows [obtén un token de acceso de Azure AD](#obtain-an-azure-ad-access-token). Después de obtener un token, tienes 60minutos para utilizar dicho token en llamadas a la API de análisis de la Tienda Windows antes de que expire. Después de que el token expire, puedes generar un nuevo token.
3.  [Llama a la API de análisis de la Tienda Windows](#call-the-windows-store-analytics-api).

<span id="prerequisites" />
## <a name="step-1-complete-prerequisites-for-using-the-windows-store-analytics-api"></a>Paso 1: Completar los requisitos previos para usar la API de análisis de la Tienda Windows

Antes de empezar a escribir código para llamar a la API de análisis de la Tienda Windows, asegúrate de que has completado los siguientes requisitos previos.

* Tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para el directorio. Si ya usas Office365 u otros servicios empresariales de Microsoft, ya tienes un directorio de AzureAD. De lo contrario, puedes [crear un nuevo Azure AD desde el Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users) sin ningún coste adicional.

* Debes asociar una aplicación de Azure AD con tu cuenta del Centro de desarrollo, recuperar el identificador de inquilino y el identificador de cliente correspondientes a la aplicación y generar la clave. La aplicación de Azure AD representa la aplicación o el servicio desde donde quieres originar la llamada a la API de análisis de la Tienda Windows. Necesitas el identificador de inquilino, el identificador de cliente y la clave para obtener un token de acceso de Azure AD que se pasa a la API.

  >**Nota**&nbsp;&nbsp;Solo debes realizar esta tarea una vez. Una vez que tengas el identificador de inquilino, el identificador de cliente y la clave, puedes volver a usarlos siempre que necesites crear un nuevo token de acceso de Azure AD.

Para asociar una aplicación de Azure AD con la cuenta del Centro de desarrollo y recuperar los valores requeridos:

1.  En el Centro de desarrollo, ve a **Configuración de la cuenta**, haz clic en **Administrar usuarios** y asocia la cuenta del Centro de desarrollo de tu organización al directorio de AzureAD de tu organización. Para obtener instrucciones detalladas, consulta [Administración de usuarios de la cuenta](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

2.  En la página **Administrar usuarios**, haz clic en **Agregar aplicaciones de Azure AD**, agrega la aplicación de Azure AD que representa la aplicación o el servicio que usarás para acceder a los datos de análisis de tu cuenta del Centro de desarrollo y asígnale el rol **Administrador**. Si esta aplicación ya existe en el directorio de Azure AD, puedes seleccionarla en la página **Agregar aplicaciones de Azure AD** para agregarla a tu cuenta del Centro de desarrollo. De lo contrario, puedes crear una nueva aplicación de Azure AD en la página **Adición de aplicaciones de Azure AD**. Para obtener más información, consulta [Adición y administración de aplicaciones de Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

3.  Vuelve a la página **Administración de usuarios**, haz clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y copia los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haz clic en **Agregar nueva clave**. En la siguiente pantalla, copia el valor **Clave**. No podrás acceder a esta información de nuevo después de salir de la página. Para obtener más información, consulta la información sobre la administración de claves en [Adición y administración de aplicaciones de Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

<span id="obtain-an-azure-ad-access-token" />
## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtención de un token de acceso de Azure AD

Antes de llamar a cualquiera de los métodos en la API de análisis de la Tienda Windows, debes obtener un token de acceso de Azure AD para pasarlo al encabezado **Authorization** de cada método en la API. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizar el token para que puedas continuar usándolo en llamadas adicionales a la API.

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
## <a name="step-3-call-the-windows-store-analytics-api"></a>Paso 3: Llama a la API de análisis de la Tienda Windows.

Cuando tengas un token de acceso de Azure AD, podrás llamar a la API de análisis de la Tienda Windows. Para obtener información sobre la sintaxis de cada método, consulta los siguientes artículos. Debes pasar el token de acceso al encabezado **Authorization** de cada método.

| Escenario       | Métodos      |
|---------------|--------------------|
| Adquisiciones e instalaciones |  <ul><li>[Obtener adquisiciones de la aplicación](get-app-acquisitions.md)</li><li>[Obtener adquisiciones de complementos](get-in-app-acquisitions.md)</li><li>[Obtener instalaciones de la aplicación](get-app-installs.md)</li></ul> |
| Errores de la aplicación | <ul><li>[Obtener datos de informes de errores](get-error-reporting-data.md)</li><li>[Obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md)</li><li>[Obtener el seguimiento de la pila de un error de la aplicación](get-the-stack-trace-for-an-error-in-your-app.md)</li></ul> |
| Clasificaciones y opiniones | <ul><li>[Obtener clasificaciones de la aplicación](get-app-ratings.md)</li><li>[Obtener opiniones de la aplicación](get-app-reviews.md)</li></ul> |
| Anuncios en la aplicación y campañas de anuncios | <ul><li>[Obtener los datos de rendimiento de los anuncios](get-ad-performance-data.md)</li><li>[Obtener los datos de rendimiento de la campaña de anuncios](get-ad-campaign-performance-data.md)</li></ul> |

Los siguientes métodos adicionales están disponibles para que las cuentas de desarrollador que pertenecen al [programa Centro de desarrollo de hardware de Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) puedan usarlos.

| Escenario       | Descripción      |
|---------------|--------------------|
| Errores en controladores de Windows 10 (para IHV) |  <ul><li>[Obtener datos de informes de errores para controladores de Windows 10](get-error-reporting-data-for-windows-10-drivers.md)</li><li>[Obtener los detalles de un error de controlador de Windows 10](get-details-for-a-windows-10-driver-error.md)</li><li>[Descargar el archivo .cab para un error de controlador de Windows 10](download-the-cab-file-for-a-windows-10-driver-error.md)</li></ul> |
| Errores en controladores de Windows 7 y Windows 8.x (para IHV) |  <ul><li>[Obtener datos de informes de errores para controladores de Windows 7 y Windows 8.x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)</li><li>[Obtener los detalles para un error de controlador de Windows 7 o Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md)</li><li>[Descargar el archivo .cab para un error de controlador de Windows 7 o Windows 8.x](download-the-cab-file-for-a-windows-7-or-windows-8.x-driver-error.md)</li></ul> |
| Errores de hardware (para OEM) |  <ul><li>[Obtener datos de informes de errores de hardware OEM](get-oem-hardware-error-reporting-data.md)</li><li>[Obtener detalles para un error de hardware OEM](get-details-for-an-oem-hardware-error.md)</li><li>[Descargar el archivo .cab para un error de hardware OEM](download-the-cab-file-for-an-oem-hardware-error.md)</li></ul> |

## <a name="code-example"></a>Ejemplo de código

El siguiente ejemplo de código muestra cómo obtener un token de acceso de Azure AD y llamar a la API de análisis de la Tienda Windows desde una aplicación de consola C#. Para usar este ejemplo de código, asigna las variables *tenantId*, *clientId*, *clientSecret* y *appID* a los valores adecuados de tu escenario. En este ejemplo se necesita el [paquete Json.NET](http://www.newtonsoft.com/json) de Newtonsoft para deserializar los datos de JSON devueltos por la API de análisis de la Tienda Windows.

> [!div class="tabbedCodeSnippets"]
[!code-cs[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>Respuestas de error

La API de análisis de la Tienda Windows devuelve respuestas de error en un objeto JSON que contiene mensajes y códigos de error. En el ejemplo siguiente se muestra una respuesta de error producida por un parámetro no válido.

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
