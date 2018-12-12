---
title: Administrador de cuentas web
description: En este artículo, se describe cómo usar AccountsSettingsPane para conectar la aplicación para la Plataforma universal de Windows (UWP) a proveedores de identidades externos, como Microsoft o Facebook, con las API de Administrador de cuentas web de Windows 10.
ms.date: 12/6/2017
ms.topic: article
keywords: Windows 10, uwp, seguridad
ms.assetid: ec9293a1-237d-47b4-bcde-18112586241a
ms.localizationpriority: medium
ms.openlocfilehash: b0a052d08c6b0816d977d3e86881540194075818
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8941283"
---
# <a name="web-account-manager"></a>Administrador de cuentas web

En este artículo, se describe cómo usar **[AccountsSettingsPane](https://docs.microsoft.com/uwp/api/Windows.UI.ApplicationSettings.AccountsSettingsPane)** para conectar la aplicación para la Plataforma universal de Windows (UWP) a proveedores de identidades externos, como Microsoft o Facebook, con las API de Administrador de cuentas web de Windows 10. Aprenderás a solicitar permiso al usuario para usar su cuenta de Microsoft, obtener un token de acceso y usarlo para realizar operaciones básicas (como obtener datos de perfil o cargar archivos en su cuenta de OneDrive). Los pasos son similares para obtener permiso del usuario y acceso con cualquier proveedor de identidades que admita el Administrador de cuentas web.

> [!NOTE]
> Para obtener un ejemplo de código completo, consulta [WebAccountManagement sample en GitHub (Muestra de administración de cuentas web en GitHub)](http://go.microsoft.com/fwlink/p/?LinkId=620621).

## <a name="get-set-up"></a>Preparación

En primer lugar, crea una aplicación vacía en Visual Studio. 

En segundo lugar, para conectar con proveedores de identidades, deberás asociar la aplicación a la Tienda. Para ello, haz clic con el botón derecho en el proyecto, elige **Tienda** > **Asociar aplicación con la Tienda** y sigue las instrucciones del asistente. 

En tercer lugar, crea una interfaz de usuario muy básica que conste de un botón XAML simple y dos cuadros de texto.

```XML
<StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
    <Button x:Name="LoginButton" Content="Log in" Click="LoginButton_Click" />
    <TextBlock x:Name="UserIdTextBlock"/>
    <TextBlock x:Name="UserNameTextBlock"/>
</StackPanel>
```

Y un controlador de eventos adjunto al botón en el código subyacente:

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{   
}
```

Por último, agrega los siguientes espacios de nombres para no tener que preocuparte por problemas de referencias más adelante: 

```csharp
using System;
using Windows.Security.Authentication.Web.Core;
using Windows.System;
using Windows.UI.ApplicationSettings;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Data.Json;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Http;
```

## <a name="show-the-accounts-settings-pane"></a>Mostrar el panel de configuración de cuentas

El sistema proporciona una interfaz de usuario integrada para administrar proveedores de identidades y cuentas web denominada **AccountsSettingsPane**. Se puede mostrar de la siguiente manera:

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    AccountsSettingsPane.Show(); 
}
```

Si ejecutas la aplicación y haces clic en el botón "Iniciar sesión", debería mostrarse una ventana vacía. 

![Panel de configuración de la cuenta](images/tb-1.png)

El panel está vacío porque el sistema solo proporciona un shell de interfaz de usuario. El desarrollador es quien rellena mediante programación el panel con los proveedores de identidades. 

## <a name="register-for-accountcommandsrequested"></a>Registrarse para AccountCommandsRequested

Para agregar comandos al panel, empezamos por el registro para el controlador de eventos AccountCommandsRequested. Este indica al sistema que ejecute nuestra lógica de compilación cuando el usuario solicite ver el panel (por ejemplo, hace clic en nuestro botón XAML). 

En el código subyacente, invalida los eventos OnNavigatedTo y OnNavigatedFrom y agrégales el siguiente código: 

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested += BuildPaneAsync; 
}
```

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested -= BuildPaneAsync; 
}
```

Los usuarios no interactúan con las cuentas muy a menudo, por lo que registrar y cancelar el registro del controlador de eventos de esta manera ayuda a evitar pérdidas de memoria. De esta forma, el panel personalizado solo está en la memoria cuando existe una alta probabilidad de que un usuario lo solicite (porque está en una página de "configuración" o "inicio de sesión", por ejemplo). 

## <a name="build-the-account-settings-pane"></a>Crear el panel de configuración de la cuenta

El método BuildPaneAsync se llama siempre que se muestra la interfaz **AccountsSettingsPane**. Aquí es donde colocamos el código para personalizar los comandos que se muestran en el panel. 

Comienza por obtener un aplazamiento. Este indica al sistema que retrase la aparición de la interfaz **AccountsSettingsPane** hasta se complete su creación.

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    deferral.Complete(); 
}
```

A continuación, obtén un proveedor con el método WebAuthenticationCoreManager.FindAccountProviderAsync. La dirección URL del proveedor varía según el proveedor y puede encontrarse en la documentación del proveedor. Para cuentas de Microsoft y Azure Active Directory, es "https://login.microsoft.com". 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers"); 
        
    deferral.Complete(); 
}
```

Ten en cuenta que también pasamos la cadena "consumers" al parámetro *authority* opcional. Esto es porque Microsoft proporciona dos tipos diferentes de autenticación: cuentas Microsoft (MSA) para "consumers" y Azure Active Directory (AAD) para "organizations". La autoridad "consumers" indica que queremos la opción de MSA. Si estás desarrollando una aplicación de empresa, usa en su lugar la cadena "organizations".

Por último, agrega el proveedor a la interfaz **AccountsSettingsPane** mediante la creación de un método **[WebAccountProviderCommand](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings.webaccountprovidercommand)** como este: 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();

    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers");

    var command = new WebAccountProviderCommand(msaProvider, GetMsaTokenAsync);  

    e.WebAccountProviderCommands.Add(command);

    deferral.Complete(); 
}
```

El método GetMsaToken que pasamos a nuestro nuevo método **WebAccountProviderCommand** no existe aún (lo crearemos en el siguiente paso), por lo que deberás agregarlo como un método vacío por ahora.

Ejecuta el código anterior y obtendrás un panel con un aspecto similar al siguiente: 

![Panel de configuración de la cuenta](images/tb-2.png)

### <a name="request-a-token"></a>Solicitar un token

Una vez que la opción Cuenta de Microsoft se muestra en la interfaz **AccountsSettingsPane**, necesitamos controlar qué sucede cuando el usuario la selecciona. Registramos nuestro método GetMsaToken para que se activara cuando el usuario eligiera iniciar sesión con su cuenta de Microsoft, por lo que obtendremos el token allí. 

Para obtener un token, usa el método RequestTokenAsync de esta manera: 

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

En este ejemplo, pasamos la cadena "wl.basic" al parámetro _scope_. El ámbito representa el tipo de información que estás solicitando desde el servicio proveedor en un usuario específico. Ciertos ámbitos proporcionan acceso solo a la información básica de un usuario, como la dirección de correo electrónico y el nombre, mientras que otros ámbitos pueden conceder acceso a información confidencial, como las fotos del usuario o la bandeja de entrada de correo electrónico. Por lo general, la aplicación debe usar el ámbito menos permisivo necesario para lograr su función. Los proveedores de servicios proporcionarán documentación sobre los ámbitos que son necesarios para obtener tokens para usarlos con sus servicios. 

* Para ámbitos de Office 365 y Outlook.com, consulta [Authenticate Office 365 and Outlook.com APIs using the v2.0 authentication endpoint](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) (Autenticar las API de Office 365 y Outlook.com mediante el punto de conexión de autenticación v2.0). 
* Para ámbitos de OneDrive, consulta [OneDrive authentication and sign-in](https://dev.onedrive.com/auth/msa_oauth.htm#authentication-scopes) (Autenticación e inicio de sesión de OneDrive). 

> [!TIP]
> Opcionalmente, si la aplicación usa una sugerencia de inicio de sesión (para rellenar el campo de usuario con una dirección de correo electrónico predeterminado) u otra propiedad especial relacionadas con la experiencia de inicio de sesión, se se enumeran en la propiedad **[WebTokenRequest.AppProperties](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webtokenrequest.appproperties#Windows_Security_Authentication_Web_Core_WebTokenRequest_AppProperties)** . Esto hará que el sistema para omitir la propiedad al almacenar en caché de la cuenta web, lo que impide que las diferencias de la cuenta en la memoria caché.

Si estás desarrollando una aplicación de empresa, probablemente querrás conectarte a una instancia de Azure Active Directory (AAD) y usar la API de Microsoft Graph en lugar de los servicios de MSA habituales. En este escenario, usa el siguiente código en su lugar: 

```csharp
private async void GetAadTokenAsync(WebAccountProviderCommand command)
{
    string clientId = "your_guid_here"; // Obtain your clientId from the Azure Portal
    WebTokenRequest request = new WebTokenRequest(provider, "User.Read", clientId);
    request.Properties.Add("resource", "https://graph.microsoft.com");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

El resto de este artículo sigue describiendo el escenario de MSA, pero el código de AAD es muy similar. Para obtener más información sobre AAD o Graph, incluida una muestra completa en GitHub, consulta la [documentación de Microsoft Graph](https://graph.microsoft.io/docs/platform/get-started).

## <a name="use-the-token"></a>Usar el token

El método RequestTokenAsync devuelve un objeto WebTokenRequestResult, que contiene los resultados de la solicitud. Si la solicitud se realizó correctamente, contendrá un token.  

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
    }
}
```

> [!NOTE]
> Si recibes un error al solicitar un token, asegúrate de que has asociado a tu aplicación a la Tienda, tal y como se describe en el paso 1. La aplicación no podrá obtener un token si omites este paso. 

Cuando tengas un token, podrás usarlo para llamar a la API de tu proveedor. En el siguiente código, llamaremos a las [API de Microsoft Live](https://msdn.microsoft.com/library/hh826533.aspx) para obtener información básica sobre el usuario y mostrarla en nuestra interfaz de usuario. Sin embargo, ten en cuenta que en la mayoría de los casos se recomienda almacenar el token una vez obtenido y, a continuación, usarlo en un método independiente.

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
        
        var restApi = new Uri(@"https://apis.live.net/v5.0/me?access_token=" + token);

        using (var client = new HttpClient())
        {
            var infoResult = await client.GetAsync(restApi);
            string content = await infoResult.Content.ReadAsStringAsync();

            var jsonObject = JsonObject.Parse(content);
            string id = jsonObject["id"].GetString();
            string name = jsonObject["name"].GetString();

            UserIdTextBlock.Text = "Id: " + id; 
            UserNameTextBlock.Text = "Name: " + name;
        }
    }
}
```

El método para llamar a las distintas API de REST varía según el proveedor; consulta la documentación de la API del proveedor para obtener información sobre cómo usar el token. 

## <a name="store-the-account-for-future-use"></a>Guardar la cuenta para el uso futuro

Los tokens son útiles para obtener información acerca de un usuario de inmediato, pero suelen tener distintas duraciones; los tokens de MSA, por ejemplo, solo son válidos para unas pocas horas. Afortunadamente, no es necesario volver a mostrar la interfaz **AccountsSettingsPane** cada vez que un token expira. Una vez que un usuario ha autorizado la aplicación una vez, puedes almacenar la información de la cuenta del usuario para usarla posteriormente. 

Para ello, usa la clase **[WebAccount](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount)**. El mismo método que usaste para solicitar el token devuelve un **objeto WebAccount**:

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        WebAccount account = result.ResponseData[0].WebAccount; 
    }
}
```

Cuando tengas una instancia **WebAccount**, podrás almacenarla fácilmente. En el siguiente ejemplo, se usa LocalSettings. Para obtener más información sobre cómo usar LocalSettings y otros métodos para almacenar datos del usuario, consulta [Store and retrieve app settings and data](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) (Almacenar y recuperar la configuración y los datos de una aplicación).

```csharp
private async void StoreWebAccount(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"] = account.WebAccountProvider.Id;
    ApplicationData.Current.LocalSettings.Values["CurrentUserId"] = account.Id; 
}
```

A continuación, podemos usar un método asincrónico, como el siguiente, para intentar obtener un token en segundo plano con la **WebAccount** almacenada.

```csharp
private async Task<string> GetTokenSilentlyAsync()
{
    string providerId = ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"]?.ToString();
    string accountId = ApplicationData.Current.LocalSettings.Values["CurrentUserId"]?.ToString();

    if (null == providerId || null == accountId)
    {
        return null; 
    }

    WebAccountProvider provider = await WebAuthenticationCoreManager.FindAccountProviderAsync(providerId);
    WebAccount account = await WebAuthenticationCoreManager.FindAccountAsync(provider, accountId);

    WebTokenRequest request = new WebTokenRequest(provider, "wl.basic");

    WebTokenRequestResult result = await WebAuthenticationCoreManager.GetTokenSilentlyAsync(request, account);
    if (result.ResponseStatus == WebTokenRequestStatus.UserInteractionRequired)
    {
        // Unable to get a token silently - you'll need to show the UI
        return null; 
    }
    else if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        // Success
        return result.ResponseData[0].Token;
    }
    else
    {
        // Other error 
        return null; 
    }
}
```

Coloca el método anterior justo antes del código que crea el **AccountsSettingsPane**. Si se obtiene el token en segundo plano, no es necesario mostrar el panel. 

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    string silentToken = await GetMsaTokenSilentlyAsync();

    if (silentToken != null)
    {
        // the token was obtained. store a reference to it or do something with it here.
    }
    else
    {
        // the token could not be obtained silently. Show the AccountsSettingsPane
        AccountsSettingsPane.Show();
    }
}
```

Dado que es muy sencillo obtener un token de forma silenciosa, debes usar este proceso para actualizar el token entre sesiones, en lugar de almacenar en caché un token existente (ya que ese token podría expirar en cualquier momento).

> [!NOTE]
> El ejemplo anterior solo cubre los casos correctos e incorrectos básicos. La aplicación también debe tener en cuenta los escenarios inusuales (por ejemplo, que un usuario revoque el permiso de la aplicación o quite su cuenta de Windows) y controlarlos correctamente.  

## <a name="remove-a-stored-account"></a>Quitar una cuenta almacenada

Si conservas una cuenta web, es recomendable que los usuarios tengan la capacidad de desasociar su cuenta con tu aplicación. De esta forma, puede eficazmente "cerrar sesión" de la aplicación: información de su cuenta ya no se cargará automáticamente al iniciarse. Para ello, quita primero cualquier cuenta guardada, así como la información del proveedor del almacenamiento. A continuación, llama a **[SignOutAsync](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount.SignOutAsync)** para borrar la memoria caché e invalidar los tokens existentes que la aplicación pueda tener. 

```csharp
private async Task SignOutAccountAsync(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserProviderId");
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserId"); 
    account.SignOutAsync(); 
}
```

## <a name="add-providers-that-dont-support-webaccountmanager"></a>Agregar proveedores que no admitan WebAccountManager

Si quieres integrar la autenticación de un servicio en la aplicación, pero ese servicio no admite WebAccountManager (Google+ o Twitter, por ejemplo), puedes agregar manualmente ese proveedor a la interfaz **AccountsSettingsPane**. Para ello, crea un nuevo objeto WebAccountProvider y proporciona tu propio nombre e icono .png. Luego, agrégalo a la lista WebAccountProviderCommands. Este es un ejemplo de código auxiliar: 

 ```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 

    var twitterProvider = new WebAccountProvider("twitter", "Twitter", new Uri(@"ms-appx:///Assets/twitter-auth-icon.png")); 
    var twitterCmd = new WebAccountProviderCommand(twitterProvider, GetTwitterTokenAsync);
    e.WebAccountProviderCommands.Add(twitterCmd);   
    
    // other code here
}

private async void GetTwitterTokenAsync(WebAccountProviderCommand command)
{
    // Manually handle Twitter login here
}

```

> [!NOTE] 
> Esto solo agrega un icono a la interfaz **AccountsSettingsPane** y ejecuta el método que especificas al hacer clic en el icono (en este caso, GetTwitterTokenAsync). Debes proporcionar el código que controla la autenticación real. Para obtener más información, consulta (Agente de autenticación web)[web-authentication-broker], que proporciona métodos auxiliares de autenticación con los servicios REST. 

## <a name="add-a-custom-header"></a>Agregar un encabezado personalizado

Puedes personalizar el panel de configuración de la cuenta mediante la propiedad HeaderText, de esta manera: 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    args.HeaderText = "MyAwesomeApp works best if you're signed in.";   
    
    // other code here
}
```

![Panel de configuración de la cuenta](images/tb-3.png)

No te excedas con el texto del encabezado; mantenlo breve y simple. Si el proceso de inicio de sesión es complicado y necesitas mostrar más información, vincula el usuario a otra página con un vínculo personalizado. 

## <a name="add-custom-links"></a>Agregar vínculos personalizados

Puedes agregar comandos personalizados a la interfaz AccountsSettingsPane, que aparecerán como vínculos debajo de la colección de WebAccountProviders admitidos. Los comandos personalizados son geniales para tareas simples relacionadas con las cuentas de usuario, como mostrar una directiva de privacidad o iniciar una página de soporte técnico para los usuarios que tienen problemas. 

Aquí tienes un ejemplo: 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    var settingsCmd = new SettingsCommand(
        "settings_privacy", 
        "Privacy policy", 
        async (x) => await Launcher.LaunchUriAsync(new Uri(@"https://privacy.microsoft.com/en-US/"))); 

    e.Commands.Add(settingsCmd); 
    
    // other code here
}
```

![Panel de configuración de la cuenta](images/tb-4.png)

En teoría, puedes usar comandos de configuración para cualquier cosa. Sin embargo, se recomienda limitar su uso a escenarios intuitivos relacionados con las cuentas como las descritas anteriormente. 

## <a name="see-also"></a>Consulta también

[Windows.Security.Authentication.Web.Core namespace (Espacio de nombres Windows.Security.Authentication.Web.Core)](https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.core.aspx)

[Espacio de nombres Windows.Security.Credentials](https://msdn.microsoft.com/library/windows/apps/windows.security.credentials.aspx)

[Clase AccountsSettingsPane](https://msdn.microsoft.com/library/windows/apps/windows.ui.applicationsettings.accountssettingspane)

[Agente de autenticación web](web-authentication-broker.md)

[Muestra de administración de cuentas web](http://go.microsoft.com/fwlink/p/?LinkId=620621)

[Aplicación de programador de inicio](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
