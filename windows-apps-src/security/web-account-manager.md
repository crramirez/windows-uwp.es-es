---
title: Administrador de cuentas web
description: En este artículo se describe cómo usar AccountsSettingsPane para conectar la aplicación Plataforma universal de Windows (UWP) con proveedores de identidades externos, como Microsoft o Facebook, mediante las API del administrador de cuentas web de Windows 10.
ms.date: 12/06/2017
ms.topic: article
keywords: windows 10, uwp, security
ms.assetid: ec9293a1-237d-47b4-bcde-18112586241a
ms.localizationpriority: medium
ms.openlocfilehash: 69e60d8ef919a05493f47f086ee992afe8bfeb4c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172839"
---
# <a name="web-account-manager"></a>Administrador de cuentas web

En este artículo se describe cómo usar **[AccountsSettingsPane](/uwp/api/Windows.UI.ApplicationSettings.AccountsSettingsPane)** para conectar la aplicación plataforma universal de Windows (UWP) con proveedores de identidades externos, como Microsoft o Facebook, mediante las API del administrador de cuentas web de Windows 10. Aprenderá a solicitar el permiso de un usuario para usar sus cuenta de Microsoft, obtener un token de acceso y usarlo para realizar operaciones básicas (como obtener datos de perfil o cargar archivos en su cuenta de OneDrive). Los pasos son similares para obtener permiso del usuario y acceso con cualquier proveedor de identidades que admita el Administrador de cuentas web.

> [!NOTE]
> Para obtener un ejemplo de código completo, vea el [ejemplo de WebAccountManagement en github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAccountManagement).

## <a name="get-set-up"></a>Prepárate

En primer lugar, crea una aplicación vacía en Visual Studio. 

En segundo lugar, para conectar con proveedores de identidades, deberás asociar la aplicación a la Tienda. Para ello, haga clic con el botón derecho en el proyecto, elija **tienda**  >  **asociar aplicación con la tienda**y siga las instrucciones del asistente. 

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

El sistema proporciona una interfaz de usuario integrada para administrar proveedores de identidades y cuentas web llamadas **AccountsSettingsPane**. Se puede mostrar de la siguiente manera:

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    AccountsSettingsPane.Show(); 
}
```

Si ejecutas la aplicación y haces clic en el botón "Iniciar sesión", debería mostrarse una ventana vacía. 

![Panel de configuración de la cuenta](images/tb-1.png)

El panel está vacío porque el sistema solo proporciona un shell de interfaz de usuario. El desarrollador es quien rellena mediante programación el panel con los proveedores de identidades. 

> [!TIP]
> Opcionalmente, puede usar **[ShowAddAccountAsync](/uwp/api/windows.ui.applicationsettings.accountssettingspane.showaddaccountasync)** en lugar de **[Show](/uwp/api/windows.ui.applicationsettings.accountssettingspane.show#Windows_UI_ApplicationSettings_AccountsSettingsPane_Show)**, que devolverá un **[IAsyncAction](/uwp/api/Windows.Foundation.IAsyncAction)** para consultar el estado de la operación. 

## <a name="register-for-accountcommandsrequested"></a>Registrarse para AccountCommandsRequested

Para agregar comandos al panel, empezamos por el registro para el controlador de eventos AccountCommandsRequested. Esto indica al sistema que ejecute la lógica de compilación cuando el usuario le pida ver el panel (por ejemplo, haga clic en nuestro botón XAML). 

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

Se llama al método BuildPaneAsync siempre que se muestra **AccountsSettingsPane** . Aquí es donde colocamos el código para personalizar los comandos que se muestran en el panel. 

Comienza por obtener un aplazamiento. Esto indica al sistema que retrase la visualización del **AccountsSettingsPane** hasta que terminemos de compilarlo.

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    deferral.Complete(); 
}
```

A continuación, obtén un proveedor con el método WebAuthenticationCoreManager.FindAccountProviderAsync. La dirección URL del proveedor varía según el proveedor y puede encontrarse en la documentación del proveedor. En el caso de las cuentas de Microsoft y Azure Active Directory, es "https \: //login.Microsoft.com". 

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

Por último, agregue el proveedor a **AccountsSettingsPane** mediante la creación de un nuevo **[WebAccountProviderCommand](/uwp/api/windows.ui.applicationsettings.webaccountprovidercommand)** similar al siguiente: 

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

El método GetMsaToken que pasamos a nuestro nuevo **WebAccountProviderCommand** todavía no existe (lo compilaremos en el paso siguiente), así que no dude en agregarlo como un método vacío por ahora.

Ejecuta el código anterior y obtendrás un panel con un aspecto similar al siguiente: 

![Panel de configuración de la cuenta](images/tb-2.png)

### <a name="request-a-token"></a>Solicitar un token

Una vez que se muestra la opción cuenta de Microsoft en el **AccountsSettingsPane**, es necesario controlar lo que ocurre cuando el usuario la selecciona. Registramos nuestro método GetMsaToken para que se activara cuando el usuario eligiera iniciar sesión con su cuenta de Microsoft, por lo que obtendremos el token allí. 

Para obtener un token, usa el método RequestTokenAsync de esta manera: 

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

En este ejemplo, pasamos la cadena "WL. Basic" al parámetro de _ámbito_ . El ámbito representa el tipo de información que estás solicitando desde el servicio proveedor en un usuario específico. Ciertos ámbitos proporcionan acceso únicamente a la información básica de un usuario, como el nombre y la dirección de correo electrónico, mientras que otros ámbitos pueden conceder acceso a información confidencial, como las fotos del usuario o la bandeja de entrada de correo electrónico. Por lo general, la aplicación debe usar el ámbito menos permisivo necesario para lograr su función. Los proveedores de servicios proporcionarán documentación sobre qué ámbitos son necesarios para obtener los tokens para su uso con sus servicios. 

* Para los ámbitos Microsoft 365 y Outlook.com, consulte [uso de la API de REST de Outlook (versión 2,0)](/previous-versions/office/office-365-api/api/version-2.0/use-outlook-rest-api). 
* Para los ámbitos de OneDrive, consulte [autenticación e inicio de sesión de onedrive](https://dev.onedrive.com/auth/msa_oauth.htm#authentication-scopes). 

> [!TIP]
> Opcionalmente, si la aplicación usa una sugerencia de inicio de sesión (para rellenar el campo de usuario con una dirección de correo electrónico predeterminada) u otra propiedad especial relacionada con la experiencia de inicio de sesión, escríbala en la propiedad **[WebTokenRequest. AppProperties](/uwp/api/windows.security.authentication.web.core.webtokenrequest.appproperties#Windows_Security_Authentication_Web_Core_WebTokenRequest_AppProperties)** . Esto hará que el sistema omita la propiedad al almacenar en caché la cuenta Web, lo que evita que se produzcan discrepancias de la cuenta en la memoria caché.

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

El resto de este artículo sigue describiendo el escenario de MSA, pero el código de AAD es muy similar. Para obtener más información sobre AAD o Graph, incluida una muestra completa en GitHub, consulta la [documentación de Microsoft Graph](https://developer.microsoft.com/graph).

## <a name="use-the-token"></a>Uso del token

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
> Si recibe un error al solicitar un token, asegúrese de que ha asociado la aplicación a la tienda tal y como se describe en el paso uno. La aplicación no podrá obtener un token si omites este paso. 

Cuando tengas un token, podrás usarlo para llamar a la API de tu proveedor. En el código siguiente, llamaremos a la [información de usuario Microsoft Live API](/office/) para obtener información básica sobre el usuario y mostrarla en la interfaz de usuario. Sin embargo, tenga en cuenta que en la mayoría de los casos se recomienda almacenar el token una vez obtenido y usarlo en un método independiente.

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

## <a name="store-the-account-for-future-use"></a>Almacenar la cuenta para su uso futuro

Los tokens son útiles para obtener información acerca de un usuario de inmediato, pero suelen tener distintas duraciones; los tokens de MSA, por ejemplo, solo son válidos para unas pocas horas. Afortunadamente, no es necesario volver a mostrar el **AccountsSettingsPane** cada vez que expire un token. Una vez que un usuario ha autorizado la aplicación una vez, puedes almacenar la información de la cuenta del usuario para usarla posteriormente. 

Para ello, use la clase **[WebAccount](/uwp/api/windows.security.credentials.webaccount)** . Una **cuenta** webse devuelve mediante el mismo método que se usó para solicitar el token:

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

Una vez que tenga una instancia de **WebAccount** , puede almacenarla fácilmente. En el ejemplo siguiente, usamos LocalSettings. Para obtener más información sobre el uso de LocalSettings y otros métodos para almacenar datos de usuario, vea [almacenar y recuperar la configuración y los datos](../design/app-settings/store-and-retrieve-app-data.md)de la aplicación.

```csharp
private async void StoreWebAccount(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"] = account.WebAccountProvider.Id;
    ApplicationData.Current.LocalSettings.Values["CurrentUserId"] = account.Id; 
}
```

A continuación, podemos usar un método asincrónico como el siguiente para intentar obtener un token en segundo plano con la **cuenta**webalmacenada.

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

Coloque el método anterior justo antes del código que compila el **AccountsSettingsPane**. Si el token se obtiene en segundo plano, no es necesario mostrar el panel. 

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
> En el ejemplo anterior solo se tratan los casos de éxito y error básicos. La aplicación también debe tener en cuenta los escenarios inusuales (por ejemplo, que un usuario revoque el permiso de la aplicación o quite su cuenta de Windows) y controlarlos correctamente.  

## <a name="remove-a-stored-account"></a>Quitar una cuenta almacenada

Si conserva una cuenta Web, puede que desee ofrecer a los usuarios la posibilidad de desasociar su cuenta con la aplicación. De esta manera, pueden "cerrar sesión" de la aplicación: la información de su cuenta ya no se cargará automáticamente al iniciarse. Para ello, quite primero cualquier información de proveedor y cuenta guardada del almacenamiento. Después, llame a **[SignOutAsync](/uwp/api/windows.security.credentials.webaccount.SignOutAsync)** para borrar la memoria caché e invalidar los tokens existentes que pueda tener la aplicación. 

```csharp
private async Task SignOutAccountAsync(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserProviderId");
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserId"); 
    account.SignOutAsync(); 
}
```

## <a name="add-providers-that-dont-support-webaccountmanager"></a>Agregar proveedores que no admitan WebAccountManager

Por ejemplo, si desea integrar la autenticación desde un servicio en la aplicación, pero ese servicio no es compatible con WebAccountManager-Google + o Twitter, puede Agregar manualmente ese proveedor a **AccountsSettingsPane**. Para ello, cree un nuevo objeto WebAccountProvider y proporcione su propio nombre y el icono. png y agréguelo a la lista WebAccountProviderCommands. Este es un ejemplo de código auxiliar: 

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
> Esto solo agrega un icono a **AccountsSettingsPane** y ejecuta el método especificado al hacer clic en el icono (GetTwitterTokenAsync, en este caso). Debes proporcionar el código que controla la autenticación real. Para obtener más información, vea [agente de autenticación Web](web-authentication-broker.md), que proporciona métodos auxiliares para la autenticación mediante servicios REST. 

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

Veamos un ejemplo: 

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

## <a name="see-also"></a>Vea también

[Windows.Security.Authentication.Web.Core namespace (Espacio de nombres Windows.Security.Authentication.Web.Core)](/uwp/api/windows.security.authentication.web.core)

[Windows.Security.Credentials Namespace (Espacio de nombres Windows.Security.Credentials)](/uwp/api/windows.security.credentials)

[Clase AccountsSettingsPane](/uwp/api/windows.ui.applicationsettings.accountssettingspane)

[Agente de autenticación web](web-authentication-broker.md)

[Ejemplo de administración de cuentas web](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAccountManagement)

[Aplicación del programador del almuerzo](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)