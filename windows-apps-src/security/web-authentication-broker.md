---
title: Agente de autenticación web
description: En este artículo se explica cómo conectar tu aplicación de la Plataforma universal de Windows (UWP) a un proveedor de identidad en línea que usa protocolos de autenticación como OpenID u OAuth, como Facebook, Twitter, Flickr, Instagram, etc.
ms.assetid: 05F06961-1768-44A7-B185-BCDB74488F85
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, security
ms.localizationpriority: medium
ms.openlocfilehash: 52dc4364689ac04910c5b42cfe2dbcfd3b895b21
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172829"
---
# <a name="web-authentication-broker"></a>Agente de autenticación web




En este artículo se explica cómo conectar tu aplicación de la Plataforma universal de Windows (UWP) a un proveedor de identidad en línea que usa protocolos de autenticación como OpenID u OAuth, como Facebook, Twitter, Flickr, Instagram, etc. El método [**AuthenticateAsync**](/uwp/api/windows.security.authentication.web.webauthenticationbroker.authenticateasync) envía una solicitud al proveedor de identidad en línea y obtiene un token de acceso que describe los recursos del proveedor a los que tiene acceso la aplicación.

>[!NOTE]
>Para obtener un ejemplo de código de trabajo completo, Clone el [repositorio de WebAuthenticationBroker en github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker).

 

## <a name="register-your-app-with-your-online-provider"></a>Registrar la aplicación con un proveedor en línea


Debes registrar tu aplicación con el proveedor de identidad en línea con el cual quieres conectarte. Puedes averiguar cómo registrar tu aplicación con el proveedor de identidad. Después de registrarte, el proveedor en línea generalmente te concede un id. o clave secreta para tu aplicación.

## <a name="build-the-authentication-request-uri"></a>Crear el URI de solicitud de autenticación


El URI de solicitud consta de la dirección desde la que envías la solicitud de autenticación a tu proveedor en línea anexada con otra información obligatoria, como un secreto o identificador de la aplicación, un URI de redirección donde se envía al usuario después de completar la autenticación, y el tipo de respuesta esperado. Puedes preguntarle al proveedor qué parámetros se requieren.

El URI de solicitud se envía como el parámetro *requestUri* del método [**AuthenticateAsync**](/uwp/api/windows.security.authentication.web.webauthenticationbroker.authenticateasync). Debe ser una dirección segura (debe comenzar con `https://` )

En el siguiente ejemplo se muestra cómo crear el URI de solicitud.

```cs
string startURL = "https://<providerendpoint>?client_id=<clientid>&scope=<scopes>&response_type=token";
string endURL = "http://<appendpoint>";

System.Uri startURI = new System.Uri(startURL);
System.Uri endURI = new System.Uri(endURL);
```

## <a name="connect-to-the-online-provider"></a>Conectarse al proveedor en línea


Llama al método [**AuthenticateAsync**](/uwp/api/windows.security.authentication.web.webauthenticationbroker.authenticateasync) para conectarte al proveedor de identidad en línea y obtener un token de acceso. El método toma el URI creado en el paso anterior como el parámetro *requestUri* y un URI al que quieres que se redireccione el usuario como el parámetro *callbackUri*.

```cs
string result;

try
{
    var webAuthenticationResult = 
        await Windows.Security.Authentication.Web.WebAuthenticationBroker.AuthenticateAsync( 
        Windows.Security.Authentication.Web.WebAuthenticationOptions.None, 
        startURI, 
        endURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.Success:
            // Successful authentication. 
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            result = webAuthenticationResult.ResponseErrorDetail.ToString(); 
            break;
        default:
            // Other error.
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
    } 
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and Network Unavailable errors here. 
    result = ex.Message;
}
```

>[!WARNING]
>Además de [**AuthenticateAsync**](/uwp/api/windows.security.authentication.web.webauthenticationbroker.authenticateasync), el espacio de nombres [**Windows.Security.Authentication.Web**](/uwp/api/Windows.Security.Authentication.Web) contiene un método [**AuthenticateAndContinue**](/uwp/api/Windows.Security.Authentication.Web.WebAuthenticationBroker#methods). No llame a este método. Está diseñado solo para aplicaciones destinadas a Windows Phone 8.1 y está en desuso a partir de Windows 10.

## <a name="connecting-with-single-sign-on-sso"></a>Conéctate con inicio de sesión único (SSO).


De forma predeterminada, el agente de autenticación web no permite que haya cookies. Por esto, aunque el usuario de la aplicación indique que quiere mantener la sesión iniciada (por ejemplo, al activar una casilla en el cuadro de diálogo de inicio de sesión del proveedor), deberá iniciar sesión cada vez que quiera tener acceso a los recursos de dicho proveedor. Para iniciar sesión con inicio de sesión único (SSO), el proveedor de identidad en línea debe haber habilitado SSO para el agente de autenticación web y la aplicación debe llamar la sobrecarga de [**AuthenticateAsync**](/uwp/api/windows.security.authentication.web.webauthenticationbroker.authenticateasync), que no toma un parámetro *callbackUri*. Esto permitirá que el agente de autenticación Web almacene las cookies persistentes, de modo que las llamadas de autenticación futuras por la misma aplicación no requieran que el usuario inicie sesión de forma repetida (el usuario se "ha iniciado sesión" hasta que expire el token de acceso).

Para admitir el inicio de sesión único, el proveedor en línea debe permitirle registrar un URI de redirección en el formulario `ms-app://<appSID>` , donde `<appSID>` es el SID de la aplicación. Puedes encontrar el SID de tu aplicación en la página del desarrollador de la aplicación, o llamando al método [**GetCurrentApplicationCallbackUri**](/uwp/api/windows.security.authentication.web.webauthenticationbroker.getcurrentapplicationcallbackuri).

```cs
string result;

try
{
    var webAuthenticationResult = 
        await Windows.Security.Authentication.Web.WebAuthenticationBroker.AuthenticateAsync( 
        Windows.Security.Authentication.Web.WebAuthenticationOptions.None, 
        startURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.Success:
            // Successful authentication. 
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            result = webAuthenticationResult.ResponseErrorDetail.ToString(); 
            break;
        default:
            // Other error.
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
    } 
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and Network Unavailable errors here. 
    result = ex.Message;
}
```

## <a name="debugging"></a>Depuración


Hay varias maneras de solucionar los problemas de las API del agente de autenticación, entre ellas, revisar los registros operativos y revisar las solicitudes web y las respuestas usando Fiddler.

### <a name="operational-logs"></a>Registros operativos

Con frecuencia, los registros operativos ayudan a determinar qué no está funcionando. Hay un canal de registro de eventos dedicado Microsoft-Windows-webauth \\ operativo que permite a los desarrolladores de sitios web comprender cómo el agente de autenticación Web está procesando sus páginas Web. Para habilitarlo, inicie eventvwr.exe y habilite el registro operativo en la aplicación y los servicios \\ Microsoft \\ Windows \\ webauth. Asimismo, el agente de autenticación web anexa una cadena única a la cadena de agente de usuario para identificarse en el servidor web. La cadena es "MSAuthHost/1.0". Ten en cuenta que el número de versión podría cambiar en el futuro, por lo que no debes depender de dicho número de versión en tu código. Este es un ejemplo de la cadena de agente de usuario completa, seguida de los pasos completos de depuración.

`User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Win64; x64; Trident/6.0; MSAuthHost/1.0)`

1.  Habilita los registros operativos.
2.  Ejecuta la aplicación social de Contoso. ![visor de eventos que muestra los registros operativos webauth](images/wab-event-viewer-1.png)
3.  Las entradas de los registros generados se pueden usar para comprender el comportamiento del agente de autenticación web con más detalle. En este caso, pueden incluir:
    -   Navegación - iniciar: registra cuándo se inicia AuthHost y contiene información sobre las direcciones URL de inicio y terminación.
    -   ![ilustra los detalles de Inicio de navegación](images/wab-event-viewer-2.png)
    -   Navegación - completa: registra la finalización de la carga de una página web.
    -   Etiqueta meta: registra cuándo se encuentra una etiqueta meta, incluidos los detalles.
    -   Navegación - finalizar: navegación terminada por el usuario.
    -   Navegación - error: AuthHost encuentra un error de navegación en una dirección URL e incluye HttpStatusCode.
    -   Navegación - fin: se ha encontrado la dirección URL de terminación.

### <a name="fiddler"></a>Fiddler

El depurador web Fiddler puede usarse con aplicaciones.

1.  Dado que AuthHost se ejecuta en su propio contenedor de aplicaciones, para darle la funcionalidad de red privada debe establecer una clave del registro: Windows Registry Editor version 5,00

    **HKEY \_ SOFTWARE de \_ equipo local** \\ **SOFTWARE** \\ **Microsoft** \\ **Windows NT** \\ **CurrentVersion** \\ **Image File Execution Options** \\ **authhost.exe** \\ **EnablePrivateNetwork** = 00000001

    Si no tiene esta clave del registro, puede crearla en un símbolo del sistema con privilegios de administrador.

    ```cmd 
    REG ADD "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\authhost.exe" /v EnablePrivateNetwork /t REG_DWORD /d 1 /f
    ```

2.  Agrega una regla para AuthHost, ya que esto es lo que genera el tráfico saliente.
    ```syntax
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.a.p_8wekyb3d8bbwe
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.sso.p_8wekyb3d8bbwe
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.sso.c_8wekyb3d8bbwe
    D:\Windows\System32>CheckNetIsolation.exe LoopbackExempt -s
    List Loopback Exempted AppContainers
    [1] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.sso.c_8wekyb3d8bbwe
        SID:  S-1-15-2-1973105767-3975693666-32999980-3747492175-1074076486-3102532000-500629349
    [2] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.sso.p_8wekyb3d8bbwe
        SID:  S-1-15-2-166260-4150837609-3669066492-3071230600-3743290616-3683681078-2492089544
    [3] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.a.p_8wekyb3d8bbwe
        SID:  S-1-15-2-3506084497-1208594716-3384433646-2514033508-1838198150-1980605558-3480344935
    ```

3.  Agrega una regla de firewall para el tráfico entrante a Fiddler.