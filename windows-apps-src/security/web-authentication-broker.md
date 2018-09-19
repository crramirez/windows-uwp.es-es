---
title: Agente de autenticación web
description: En este artículo se explica cómo conectar tu aplicación de la Plataforma universal de Windows (UWP) a un proveedor de identidad en línea que usa protocolos de autenticación como OpenID u OAuth, como Facebook, Twitter, Flickr, Instagram, etc.
ms.assetid: 05F06961-1768-44A7-B185-BCDB74488F85
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, seguridad
ms.localizationpriority: medium
ms.openlocfilehash: d354f0babec3ec2346c6e76fcae8666f40f3f6be
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "4053313"
---
# <a name="web-authentication-broker"></a>Agente de autenticación web




En este artículo se explica cómo conectar tu aplicación de la Plataforma universal de Windows (UWP) a un proveedor de identidad en línea que usa protocolos de autenticación como OpenID u OAuth, como Facebook, Twitter, Flickr, Instagram, etc. El método [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) envía una solicitud al proveedor de identidad en línea y obtiene un token de acceso que describe los recursos del proveedor a los que tiene acceso la aplicación.

>[!NOTE]
>Para obtener una muestra de código completa que funcione, clona el [Repositorio del agente de autenticación web en GitHub](http://go.microsoft.com/fwlink/p/?LinkId=620622).

 

## <a name="register-your-app-with-your-online-provider"></a>Registrar la aplicación con un proveedor en línea


Debes registrar tu aplicación con el proveedor de identidad en línea con el cual quieres conectarte. Puedes averiguar cómo registrar tu aplicación con el proveedor de identidad. Después de registrarte, el proveedor en línea generalmente te concede un id. o clave secreta para tu aplicación.

## <a name="build-the-authentication-request-uri"></a>Crear el URI de solicitud de autenticación


El URI de solicitud consta de la dirección desde la que envías la solicitud de autenticación a tu proveedor en línea anexada con otra información obligatoria, como un secreto o identificador de la aplicación, un URI de redirección donde se envía al usuario después de completar la autenticación, y el tipo de respuesta esperado. Puedes preguntarle al proveedor qué parámetros se requieren.

El URI de solicitud se envía como el parámetro *requestUri* del método [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066). Debe ser una dirección segura (debe comenzar con `https://`)

En el siguiente ejemplo se muestra cómo crear el URI de solicitud.

```cs
string startURL = "https://<providerendpoint>?client_id=<clientid>&scope=<scopes>&response_type=token";
string endURL = "http://<appendpoint>";

System.Uri startURI = new System.Uri(startURL);
System.Uri endURI = new System.Uri(endURL);
```

## <a name="connect-to-the-online-provider"></a>Conectarse al proveedor en línea


Llama al método [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) para conectarte al proveedor de identidad en línea y obtener un token de acceso. El método toma el URI creado en el paso anterior como el parámetro *requestUri* y un URI al que quieres que se redireccione el usuario como el parámetro *callbackUri*.

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
>Además de [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066), el espacio de nombres [**Windows.Security.Authentication.Web**](https://msdn.microsoft.com/library/windows/apps/br227044) contiene un método [**AuthenticateAndContinue**](https://msdn.microsoft.com/library/windows/apps/dn632425). No llame a este método. Está diseñado solo para aplicaciones destinadas a Windows Phone 8.1 y está en desuso a partir de Windows 10.

## <a name="connecting-with-single-sign-on-sso"></a>Conéctate con inicio de sesión único (SSO).


De forma predeterminada, el agente de autenticación web no permite que haya cookies. Por esto, aunque el usuario de la aplicación indique que quiere mantener la sesión iniciada (por ejemplo, al activar una casilla en el cuadro de diálogo de inicio de sesión del proveedor), deberá iniciar sesión cada vez que quiera tener acceso a los recursos de dicho proveedor. Para iniciar sesión con inicio de sesión único (SSO), el proveedor de identidad en línea debe haber habilitado SSO para el agente de autenticación web y la aplicación debe llamar la sobrecarga de [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212068), que no toma un parámetro *callbackUri*. Esto permitirá que las cookies persistentes se almacenen mediante el agente de autenticación web, para que las llamadas de autenticación futuras realizadas por la misma aplicación no requieran que el usuario inicie sesión varias veces (el usuario "ha iniciado sesión" de forma eficaz hasta que el token de acceso caduque).

Para habilitar SSO, el proveedor en línea debe permitir registrar una URI de redirección en la forma de `ms-app://<appSID>`, donde `<appSID>` es el SID de tu aplicación. Puedes encontrar el SID de tu aplicación en la página del desarrollador de la aplicación, o llamando al método [**GetCurrentApplicationCallbackUri**](https://msdn.microsoft.com/library/windows/apps/br212069).

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

Con frecuencia, los registros operativos ayudan a determinar qué no está funcionando. Existe un canal de registro de eventos dedicado, Microsoft-Windows-WebAuth\\Operational, que permite a los desarrolladores de sitios web comprender cómo el agente de autenticación web procesa sus páginas web. Para habilitarlo, inicia eventvwr.exe y habilita el registro operativo en Application and Services\\Microsoft\\Windows\\WebAuth. Asimismo, el agente de autenticación web anexa una cadena única a la cadena de agente de usuario para identificarse en el servidor web. La cadena es "MSAuthHost/1.0". Ten en cuenta que el número de versión podría cambiar en el futuro, por lo que no debes depender de dicho número de versión en tu código. Este es un ejemplo de la cadena de agente de usuario completa, seguida de los pasos completos de depuración.

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

1.  Dado que AuthHost se ejecuta en su propio contenedor de aplicación, para darle la funcionalidad de red privada debe establecer una clave del registro: Editor del registro de Windows 5.00

    **HKEY\_LOCAL\_MACHINE**\\**SOFTWARE**\\**Microsoft**\\**Windows NT**\\**CurrentVersion**\\**Opciones de ejecución del archivo de imágenes**\\**authhost.exe**\\**EnablePrivateNetwork** = 00000001

    Si no tienes esta clave del registro, puede crear en un símbolo del sistema con privilegios de administrador.

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