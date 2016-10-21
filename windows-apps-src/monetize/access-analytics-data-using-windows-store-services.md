---
author: mcleanbyron
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: "Usa la API de análisis de la Tienda Windows para recuperar datos de análisis mediante programación para las aplicaciones que se registran en tu cuenta del Centro de desarrollo de Windows o la de tu organización."
title: "Acceder a los datos de análisis mediante los servicios de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 47e0ac11178af98589e75cc562631c6904b40da4
ms.openlocfilehash: 1293bb5beb927425928d832f887129263db5a895

---

# Acceder a los datos de análisis mediante los servicios de la Tienda Windows

Usa la *API de análisis de la Tienda Windows* para recuperar datos de análisis mediante programación referentes a las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows o en la de tu organización. Esta API permite recuperar los datos respecto a las adquisiciones de aplicaciones y de complementos (conocidas también como producto desde la aplicación o IAP), errores, valoraciones de la aplicación y opiniones. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas procedentes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
2.  Antes de llamar a un método en la API de análisis de la Tienda Windows [obtén un token de acceso de Azure AD](#obtain-an-azure-ad-access-token). Después de obtener un token, tienes 60minutos para utilizar dicho token en llamadas a la API de análisis de la Tienda Windows antes de que expire. Después de que el token expire, puedes generar un nuevo token.
3.  [Llama a la API de análisis de la Tienda Windows](#call-the-windows-store-analytics-api).

<span id="prerequisites" />
## Paso 1: Completar los requisitos previos para usar la API de análisis de la Tienda Windows

Antes de empezar a escribir código para llamar a la API de análisis de la Tienda Windows, asegúrate de que has completado los siguientes requisitos previos.

* Tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para el directorio. Si ya usas Office365 u otros servicios empresariales de Microsoft, ya tienes un directorio de AzureAD. De lo contrario, puedes [crear un nuevo Azure AD desde el Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users) sin ningún coste adicional.

* Debes asociar una aplicación de Azure AD con tu cuenta del Centro de desarrollo, recuperar el identificador de inquilino y el identificador de cliente correspondientes a la aplicación y generar la clave. La aplicación de Azure AD representa la aplicación o el servicio desde donde quieres originar la llamada a la API de análisis de la Tienda Windows. Necesitas el identificador de inquilino, el identificador de cliente y la clave para obtener un token de acceso de Azure AD que se pasa a la API.

  >**Nota**&nbsp;&nbsp;Esta tarea solo es necesario realizarla una vez. Una vez que tengas el identificador de inquilino, el identificador de cliente y la clave, puedes volver a usarlos siempre que necesites crear un nuevo token de acceso de Azure AD.

Para asociar una aplicación de Azure AD con la cuenta del Centro de desarrollo y recuperar los valores requeridos:

1.  En el Centro de desarrollo, ve a **Configuración de la cuenta**, haz clic en **Administrar usuarios** y asocia la cuenta del Centro de desarrollo de tu organización al directorio de AzureAD de tu organización. Para obtener instrucciones detalladas, consulta [Administrar usuarios de la cuenta](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

2.  En la página **Administrar usuarios**, haz clic en **Agregar aplicaciones de Azure AD**, agrega la aplicación de Azure AD que representa la aplicación o el servicio que usarás para acceder a los datos de análisis de tu cuenta del Centro de desarrollo y asígnale el rol **Administrador**. Si esta aplicación ya existe en el directorio de Azure AD, puedes seleccionarla en la página **Agregar aplicaciones de Azure AD** para agregarla a tu cuenta del Centro de desarrollo. De lo contrario, puedes crear una nueva aplicación de Azure AD en la página **Agregar aplicaciones de Azure AD**. Para obtener más información, consulta [Agregar y administrar aplicaciones de Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

3.  Vuelve a la página **Administrar usuarios**, haz clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y copia los valores de **Id. de inquilino** e **Id. de cliente**.

4. Haz clic en **Agregar nueva clave**. En la siguiente pantalla, copia el valor **Clave**. No podrás acceder a esta información de nuevo después de salir de la página. Para obtener más información, consulta la información sobre la administración de claves en [Agregar y administrar aplicaciones de Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

<span id="obtain-an-azure-ad-access-token" />
## Paso 2: Obtención de un token de acceso de Azure AD

Antes de llamar a cualquiera de los métodos en la API de análisis de la Tienda Windows, debes obtener un token de acceso de Azure AD para pasarlo al encabezado **Authorization** de cada método en la API. Después de obtener un token de acceso, tienes 60minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizarlo para poder seguir usándolo en llamadas posteriores a la API.

Para obtener el token de acceso, sigue las instrucciones que aparecen en [Llamadas entre servicios mediante las credenciales del cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar una solicitud HTTP POST al punto de conexión ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. A continuación te mostramos un ejemplo de solicitud.

```
POST https://login.microsoftonline.com/<your_tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Para los parámetros *tenant\_id*, *client\_id* y *client\_secret*, especifica el identificador de inquilino, el identificador de cliente y la clave correspondientes a la aplicación que hayas recuperado del Centro de desarrollo en la sección anterior. Para el parámetro *resource*, debes especificar el identificador URI ```https://manage.devcenter.microsoft.com```.

Después de que el token de acceso expire, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-analytics-api" />
## Paso 3: Llama a la API de análisis de la Tienda Windows.

Cuando tengas un token de acceso de Azure AD, podrás llamar a la API de análisis de la Tienda Windows. Para obtener información sobre la sintaxis de cada método, consulta los siguientes artículos. Debes pasar el token de acceso al encabezado **Authorization** de cada método.

-   [Obtener los datos de las adquisiciones de la aplicación](get-app-acquisitions.md)
-   [Obtener los datos de las adquisiciones de complementos](get-in-app-acquisitions.md)
-   [Obtener los datos del informe de errores](get-error-reporting-data.md)
-   [Obtener las valoraciones de la aplicación](get-app-ratings.md)
-   [Obtener las opiniones de la aplicación](get-app-reviews.md)

## Ejemplo de código


El siguiente ejemplo de código muestra cómo obtener un token de acceso de Azure AD y llamar a la API de análisis de la Tienda Windows desde una aplicación de consola C#. Para usar este ejemplo de código, asigna las variables *tenantId*, *clientId*, *clientSecret* y *appID* a los valores adecuados de tu escenario. En este ejemplo se necesita el [paquete Json.NET](http://www.newtonsoft.com/json) de Newtonsoft para deserializar los datos de JSON devueltos por la API de análisis de Tienda Windows.

```CSharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;

namespace TestAnalyticsAPI
{
    class Program
    {
        static void Main(string[] args)
        {
            string tenantId = "<your tenant ID>";
            string clientId = "<your client ID>";
            string clientSecret = "<your secret>";

            string scope = "https://manage.devcenter.microsoft.com";

            // Retrieve an Azure AD access token
            string accessToken = GetClientCredentialAccessToken(
                    tenantId,
                    clientId,
                    clientSecret,
                    scope).Result;

            // This is your app's Store ID. This ID is available on
            // the App identity page of the Dev Center dashboard.
            string appID = "<your app's Store ID>";

            DateTime startDate = DateTime.Parse("08-01-2015");
            DateTime endDate = DateTime.Parse("11-01-2015");
            int pageSize = 1000;
            int startPageIndex = 0;

            // Call the Windows Store analytics API
            CallAnalyticsAPI(accessToken, appID, startDate, endDate, pageSize, startPageIndex);

            Console.Read();
        }

        private static void CallAnalyticsAPI(string accessToken, string appID, DateTime startDate, DateTime endDate, int top, int skip)
        {
            string requestURI;

            // Get app acquisitions
            requestURI = string.Format(
                "https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
                appID, startDate, endDate, top, skip);

            //// Get add-on acquisitions
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app failures
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app ratings
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId={0}&startDate={1}&endDate={2}top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app reviews
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            HttpRequestMessage requestMessage = new HttpRequestMessage(HttpMethod.Get, requestURI);
            requestMessage.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

            WebRequestHandler handler = new WebRequestHandler();
            HttpClient httpClient = new HttpClient(handler);

            HttpResponseMessage response = httpClient.SendAsync(requestMessage).Result;

            Console.WriteLine(response);
            Console.WriteLine(response.Content.ReadAsStringAsync().Result);

            response.Dispose();
        }

        public static async Task<string> GetClientCredentialAccessToken(string tenantId, string clientId, string clientSecret, string scope)
        {
            string tokenEndpointFormat = "https://login.microsoftonline.com/{0}/oauth2/token";
            string tokenEndpoint = string.Format(tokenEndpointFormat, tenantId);

            dynamic result;
            using (HttpClient client = new HttpClient())
            {
                string tokenUrl = tokenEndpoint;
                using (
                    HttpRequestMessage request = new HttpRequestMessage(
                        HttpMethod.Post,
                        tokenUrl))
                {
                    string content =
                        string.Format(
                            "grant_type=client_credentials&client_id={0}&client_secret={1}&resource={2}",
                            clientId,
                            clientSecret,
                            scope);

                    request.Content = new StringContent(content, Encoding.UTF8, "application/x-www-form-urlencoded");

                    using (HttpResponseMessage response = await client.SendAsync(request))
                    {
                        string responseContent = await response.Content.ReadAsStringAsync();
                        result = JsonConvert.DeserializeObject(responseContent);
                    }
                }
            }

            return result.access_token;
        }
    }
}
```

## Respuestas de error


La API análisis de Tienda Windows devuelve respuestas de error en un objeto JSON que contiene mensajes y códigos de error. En el ejemplo siguiente se muestra una respuesta de error producida por un parámetro no válido.

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

## Temas relacionados

* [Obtener los datos de las adquisiciones de la aplicación](get-app-acquisitions.md)
* [Obtener los datos de las adquisiciones de complementos](get-in-app-acquisitions.md)
* [Obtener los datos del informe de errores](get-error-reporting-data.md)
* [Obtener las valoraciones de la aplicación](get-app-ratings.md)
* [Obtener las opiniones de la aplicación](get-app-reviews.md)
 



<!--HONumber=Sep16_HO1-->


