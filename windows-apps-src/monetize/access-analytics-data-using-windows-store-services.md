---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Usa la API de análisis de la Tienda Windows para recuperar datos de análisis mediante programación para las aplicaciones que se registran en tu cuenta del Centro de desarrollo de Windows o la de tu organización.
title: Acceder a los datos de análisis mediante los servicios de la Tienda Windows
---

# Acceder a los datos de análisis mediante los servicios de la Tienda Windows


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Usa la *API de análisis de Tienda Windows* para recuperar datos de análisis mediante programación para las aplicaciones que se registran en tu cuenta del Centro de desarrollo de Windows o la de tu organización. Esta API permite recuperar los datos de adquisiciones de IAP y de la aplicación, errores, revisiones, así como clasificaciones y opiniones de la aplicación. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas provenientes de la aplicación o el servicio.

## Requisitos previos para usar la API de análisis de la Tienda Windows


-   Tú (o tu organización) debéis tener un directorio de Azure AD. Si ya usas Office 365 u otros servicios empresariales de Microsoft, ya tienes un directorio de Azure AD. De lo contrario, puedes [obtener uno de forma gratuita](http://go.microsoft.com/fwlink/p/?LinkId=703757).
-   Debes tener una [cuenta de usuario](https://azure.microsoft.com/documentation/articles/active-directory-create-users/) en el directorio de Azure AD que quieres asociar a tu cuenta del Centro de desarrollo de Windows.

## Usar la API de análisis de la Tienda Windows


Para poder usar la API de análisis de la Tienda Windows, debes asociar una aplicación de Azure AD con tu cuenta del Centro de desarrollo y obtener un token de acceso de Azure AD. La aplicación de Azure AD representa la aplicación o el servicio desde donde quieres originar la llamada a la API de análisis de la Tienda Windows. Una vez que tengas un token de acceso, puedes llamar a la API de análisis de la Tienda Windows desde la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin:

1.  [Asocia una aplicación de Azure AD a tu cuenta del Centro de desarrollo de Windows](#associate-an-azure-ad-application-with-your-windows-dev-center-account).
2.  [Obtén un token de acceso de Azure AD](#obtain-an-azure-ad-access-token).
3.  [Usa la API de análisis de la Tienda Windows](#call-the-windows-store-analytics-api).


### Asociar una aplicación de Azure AD a la cuenta del Centro de desarrollo de Windows

1.  En el Centro de desarrollo, ve a **Configuración de la cuenta**, haz clic en **Administrar usuarios** y asocia la cuenta del Centro de desarrollo de tu organización al directorio de Azure AD de tu organización. Para obtener instrucciones detalladas, consulta [Administrar usuarios de la cuenta](https://msdn.microsoft.com/library/windows/apps/mt489008). Opcionalmente, puedes agregar otros usuarios del directorio de Azure AD de la organización para que también puedan acceder a la cuenta del Centro de desarrollo.

    **Nota**: Se puede asociar una sola cuenta del Centro de desarrollo a un directorio de Azure Active Directory. Del mismo modo, solo un directorio de Azure Active Directory puede asociarse con una cuenta del Centro de desarrollo. Una vez establecida esta asociación, no podrás quitarla sin ponerte en contacto con soporte técnico.

     

2.  En la página **Administrar usuarios**, haz clic en **Agregar aplicaciones de Azure AD**, agrega la aplicación de Azure AD que usarás para acceder a los datos de análisis de tu cuenta del Centro de desarrollo y asígnale el rol **Administrador**. Si esta aplicación ya existe en el directorio de Azure AD, puedes seleccionarla en **Agregar aplicaciones de Azure AD** para agregarla a tu cuenta del Centro de desarrollo. De lo contrario, puedes crear una nueva aplicación de Azure AD en la página **Agregar aplicaciones de Azure AD**. Para obtener más información, consulta la sección sobre administración de aplicaciones de Azure AD en [Administrar usuarios de la cuenta](https://msdn.microsoft.com/library/windows/apps/mt489008).

3.  Vuelve a la página **Administrar usuarios**, haz clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y luego haz clic en **Agregar nueva clave**. En la siguiente pantalla, copia los valores de **Id. de cliente** y **Clave**. Para obtener más información, consulta la sección sobre administración de aplicaciones de Azure AD en [Administrar usuarios de la cuenta](https://msdn.microsoft.com/library/windows/apps/mt489008). Necesitas estos valores de id. de cliente y clave para obtener un token de acceso de Azure AD para usarlo al llamar a la API de análisis de la Tienda Windows. No podrás acceder a esta información de nuevo después de salir de la página.


### Obtener un token de acceso de Azure AD

Después de asociar una aplicación de Azure AD a tu cuenta del Centro de desarrollo y recuperar el id. de cliente y la clave de la aplicación, puedes usar esta información para obtener un token de acceso de Azure AD. Necesitas un token de acceso para poder llamar a cualquiera de los métodos de la API de análisis de la Tienda Windows.

Para obtener el token de acceso, sigue las instrucciones de [Llamadas de servicio a servicio utilizando las credenciales del cliente](https://msdn.microsoft.com/library/azure/dn645543.aspx) para enviar un HTTP POST al punto de conexión de Azure AD.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

-   Para obtener el identificador del inquilino, inicia sesión en el [Portal de administración de Azure](http://manage.windowsazure.com/), ve a **Active Directory** y haz clic en el directorio que vinculaste a tu cuenta del Centro de desarrollo. El id. de inquilino de este directorio está insertado en la dirección URL de esta página, como se muestra en la cadena *your\_tenant\_ID* del siguiente ejemplo.

  ```
  https://manage.windowsazure.com/@<your_tenant_name>#Workspaces/ActiveDirectoryExtension/Directory/<your_tenant_ID>/directoryQuickStart
  ```

-   En el caso de los parámetros *client\_id* y *client\_secret*, especifica el identificador de cliente y la clave de la aplicación que recuperaste anteriormente del Centro de desarrollo.
-   Para el parámetro *resource*, especifica el siguiente URI: ```https://manage.devcenter.microsoft.com```.


### Llamar a la API de análisis de la Tienda Windows

Cuando tengas un token de acceso de Azure AD, podrás llamar a la API de análisis de la Tienda Windows. Para obtener información sobre la sintaxis de cada método, consulta los siguientes artículos. Debes pasar el token de acceso al encabezado **Authorization** de cada método.

-   [Obtener adquisiciones de la aplicación](get-app-acquisitions.md)
-   [Obtener adquisiciones de IAP](get-in-app-acquisitions.md)
-   [Obtener datos de informe de errores](get-error-reporting-data.md)
-   [Obtener clasificaciones de la aplicación](get-app-ratings.md)
-   [Obtener opiniones de la aplicación](get-app-reviews.md)

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

            // This is your app's product ID. This ID is embedded in the app's listing link
            // on the App identity page of the Dev Center dashboard.
            string appID = "<your app's product ID>";

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

            //// Get IAP acquisitions
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

* [Obtener adquisiciones de la aplicación](get-app-acquisitions.md)
* [Obtener adquisiciones de IAP](get-in-app-acquisitions.md)
* [Obtener datos de informe de errores](get-error-reporting-data.md)
* [Obtener clasificaciones de la aplicación](get-app-ratings.md)
* [Get app reviews (Obtener opiniones de la aplicación)](get-app-reviews.md)
 


<!--HONumber=Mar16_HO3-->


