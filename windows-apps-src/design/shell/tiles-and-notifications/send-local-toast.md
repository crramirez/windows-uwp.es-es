---
Description: Obtenga información sobre cómo enviar una notificación del sistema local desde aplicaciones de UWP y controlar el usuario que hace clic en la notificación del sistema.
title: Envío de una notificación del sistema local desde aplicaciones para UWP
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from UWP apps
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP, enviar notificaciones del sistema, notificaciones, enviar notificaciones, notificaciones del sistema, procedimientos, Inicio rápido, introducción, ejemplo de código, tutorial
ms.localizationpriority: medium
ms.openlocfilehash: 7b669ad3c846fec0b60ae01134b80a6d87586c62
ms.sourcegitcommit: c5df8832e9df8749d0c3eee9e85f4c2d04f8b27b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92100293"
---
# <a name="send-a-local-toast-notification-from-uwp-apps"></a>Envío de una notificación del sistema local desde aplicaciones para UWP


Una notificación del sistema es un mensaje que una aplicación puede crear y enviar al usuario mientras no está actualmente dentro de la aplicación. Esta guía de inicio rápido le guiará por los pasos necesarios para crear, enviar y mostrar una notificación del sistema de Windows 10 con las nuevas plantillas adaptables y las acciones interactivas. Estas acciones se muestran a través de una notificación local, que es la notificación más sencilla que se va a implementar.

> [!IMPORTANT]
> Las aplicaciones de escritorio (incluidas las aplicaciones empaquetadas de [MSIX](/windows/msix/desktop/source-code-overview) , las aplicaciones que usan [paquetes dispersos](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) para obtener la identidad del paquete y las aplicaciones de escritorio clásicas no empaquetadas) tienen diferentes pasos para enviar notificaciones y controlar la activación. Consulte la documentación de las [aplicaciones de escritorio](toast-desktop-apps.md) para obtener información sobre cómo implementar notificaciones del sistema.

> **API importantes**: [clase ToastNotification](/uwp/api/Windows.UI.Notifications.ToastNotification), [clase ToastNotificationActivatedEventArgs](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)



## <a name="step-1-install-nuget-package"></a>Paso 1: instalar el paquete NuGet

Instale el [paquete NuGet Microsoft. Toolkit. UWP. notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/). Nuestro ejemplo de código usará este paquete. Al final del artículo, proporcionaremos los fragmentos de código "sin formato" que no usan ningún paquete NuGet. Este paquete le permite crear notificaciones del sistema sin usar XML.


## <a name="step-2-add-namespace-declarations"></a>Paso 2: agregar declaraciones de espacio de nombres

`Windows.UI.Notifications` incluye las API del sistema.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```


## <a name="step-3-send-a-toast"></a>Paso 3: envío de una notificación del sistema

En Windows 10, el contenido de la notificación del sistema se describe mediante un lenguaje adaptable que permite una gran flexibilidad con el aspecto de la notificación. Consulte la [documentación del contenido del sistema](adaptive-interactive-toasts.md) para obtener más información.

Comenzaremos con una notificación simple basada en texto. Construya el contenido de la notificación (mediante la biblioteca de notificaciones) y muestre la notificación.

<img alt="Simple text notification" src="images/send-toast-01.png" width="364"/>

```csharp
// Construct the content
var content = new ToastContentBuilder()
    .AddToastActivationInfo("picOfHappyCanyon", ToastActivationType.Foreground)
    .AddText("Andrew sent you a picture")
    .AddText("Check this out, Happy Canyon in Utah!")
    .GetToastContent();

// Create the notification
var notif = new ToastNotification(content.GetXml());

// And show it!
ToastNotificationManager.CreateToastNotifier().Show();
```

## <a name="step-4-handling-activation"></a>Paso 4: control de la activación

Cuando el usuario haga clic en la notificación (o en un botón de la notificación con activación en primer plano) **App.xaml.cs** **, se** invocará el App.Xaml.CS de la aplicación.

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle notification activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {
        // Obtain the arguments from the notification
        string args = toastActivationArgs.Argument;

        // Obtain any user input (text boxes, menu selections) from the notification
        ValueSet userInput = toastActivationArgs.UserInput;
 
        // TODO: Show the corresponding content
    }
}
```

> [!IMPORTANT]
> Debe inicializar el marco y activar la ventana del mismo modo que el código **onlaunched** . **No se llama a onlaunched si el usuario hace clic en la notificación del sistema**, incluso si la aplicación se cerró y se inicia por primera vez. A menudo se recomienda combinar **onlaunched** y **onactivated** en su propio `OnLaunchedOrActivated` método, ya que la misma inicialización debe realizarse en ambos.


## <a name="activation-in-depth"></a>Activación en profundidad

El primer paso para hacer que las notificaciones sean accionables es agregar algunos argumentos de inicio a la notificación, de modo que la aplicación pueda saber lo que debe iniciarse cuando el usuario haga clic en la notificación (en este caso, vamos a incluir información que más adelante nos indica que deberíamos abrir una conversación y sabemos qué conversación específica abrir).

Se recomienda instalar el paquete NuGet de [QueryString.net](https://www.nuget.org/packages/QueryString.NET/) para ayudar a construir y analizar cadenas de consulta para los argumentos de notificación, como se muestra a continuación.

```csharp
using Microsoft.QueryStringDotNET; // QueryString.NET

int conversationId = 384928;

// Construct the content
var content = new ToastContentBuilder()

    // Arguments returned when user taps body of notification
    .AddToastActivationInfo(new QueryString() // Using QueryString.NET
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
    }.ToString(), ToastActivationType.Foreground)

    .AddText("Andrew sent you a picture")
    ...
```


Este es un ejemplo más complejo de control de la activación...

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {            
        // Parse the query string (using QueryString.NET)
        QueryString args = QueryString.Parse(toastActivationArgs.Argument);
 
        // See what action is being requested 
        switch (args["action"])
        {
            // Open the image
            case "viewImage":
 
                // The URL retrieved from the toast args
                string imageUrl = args["imageUrl"];
 
                // If we're already viewing that image, do nothing
                if (rootFrame.Content is ImagePage && (rootFrame.Content as ImagePage).ImageUrl.Equals(imageUrl))
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ImagePage), imageUrl);
                break;
                             
 
            // Open the conversation
            case "viewConversation":
 
                // The conversation ID retrieved from the toast args
                int conversationId = int.Parse(args["conversationId"]);
 
                // If we're already viewing that conversation, do nothing
                if (rootFrame.Content is ConversationPage && (rootFrame.Content as ConversationPage).ConversationId == conversationId)
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ConversationPage), conversationId);
                break;
        }
 
        // If we're loading the app for the first time, place the main page on
        // the back stack so that user can go back after they've been
        // navigated to the specific page
        if (rootFrame.BackStack.Count == 0)
            rootFrame.BackStack.Add(new PageStackEntry(typeof(MainPage), null, null));
    }
 
    // TODO: Handle other types of activation
 
    // Ensure the current window is active
    Window.Current.Activate();
}
```


## <a name="adding-images"></a>Agregar imágenes

Puede agregar contenido enriquecido a las notificaciones. Agregaremos una imagen en línea y una imagen de perfil (invalidación del logotipo de la aplicación).

> [!NOTE]
> Las imágenes se pueden usar desde el paquete de la aplicación, el almacenamiento local de la aplicación o desde la Web. A partir de la actualización de Fall Creators, las imágenes Web pueden tener hasta 3 MB en las conexiones normales y 1 MB en las conexiones de uso medido. En los dispositivos que aún no ejecuten el Fall Creators Update, las imágenes web no deben tener más de 200 KB.

> [!IMPORTANT]
> Las imágenes http solo se admiten en aplicaciones UWP/MSIX/dispersas que tienen la capacidad de Internet en su manifiesto. Las aplicaciones de escritorio no MSIX/Sparse no admiten imágenes http; debe descargar la imagen en los datos de la aplicación local y hacer referencia a ella localmente.

<img alt="Toast with images" src="images/send-toast-02.png" width="364"/>

```csharp
// Construct the content
var content = new ToastContentBuilder()
    ...

    // Inline image
    .AddInlineImage(new Uri("https://picsum.photos/360/202?image=883"))

    // Profile (app logo override) image
    .AddAppLogoOverride(new Uri("ms-appdata:///local/Andrew.jpg"), ToastGenericAppLogoCrop.Circle)
    
    .GetToastContent();
    
...
```



## <a name="adding-buttons-and-inputs"></a>Agregar botones y entradas

Puede agregar botones y entradas para que las notificaciones sean interactivas. Los botones pueden iniciar la aplicación en primer plano, un protocolo o la tarea en segundo plano. Agregaremos un cuadro de texto de respuesta, un botón "like" y un botón "ver" que abra la imagen.

<img alt="Toast with images and buttons" src="images/send-toast-03.png" width="364"/>

```csharp
int conversationId = 384928;

// Construct the content
var content = new ToastContentBuilder()
    ...

    // Text box for replying
    .AddInputTextBox("tbReply", placeHolderContent: "Type a response")

    // Reference the text box's ID in order to place this button next to the text box
    .AddButton("tbReply", "Reply", ToastActivationType.Background, new QueryString()
    {
        { "action", "reply" },
        { "conversationId", conversationId.ToString() }
    }.ToString(), imageUri: new Uri("Assets/Reply.png", UriKind.Relative))

    .AddButton("Like", ToastActivationType.Background, new QueryString()
    {
        { "action", "like" },
        { "conversationId", conversationId.ToString() }
    }.ToString())

    .AddButton("View", ToastActivationType.Foreground, new QueryString()
    {
        { "action", "viewImage" },
        { "imageUrl", image.ToString() }
    }.ToString())
    
    .GetToastContent();
    
...
```

La activación de los botones de primer plano se administra de la misma manera que el cuerpo del sistema de notificación principal (se llamará a App.xaml.cs Onactivated).



## <a name="handling-background-activation"></a>Controlar la activación en segundo plano

Cuando se especifica la activación en segundo plano en el sistema (o en un botón dentro de la notificación del sistema), se ejecutará la tarea en segundo plano en lugar de activar la aplicación en primer plano.

Para obtener más información sobre las tareas en segundo plano, consulte [soporte técnico de la aplicación con tareas en segundo plano](/windows/uwp/launch-resume/support-your-app-with-background-tasks).

Si tiene como destino la compilación 14393 o posterior, puede usar tareas en segundo plano en proceso, que simplifican considerablemente las cosas. Tenga en cuenta que las tareas en segundo plano en proceso no podrán ejecutarse en versiones anteriores de Windows. Usaremos una tarea en segundo plano en proceso en este ejemplo de código.

```csharp
const string taskName = "ToastBackgroundTask";

// If background task is already registered, do nothing
if (BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals(taskName)))
    return;

// Otherwise request access
BackgroundAccessStatus status = await BackgroundExecutionManager.RequestAccessAsync();

// Create the background task
BackgroundTaskBuilder builder = new BackgroundTaskBuilder()
{
    Name = taskName
};

// Assign the toast action trigger
builder.SetTrigger(new ToastNotificationActionTrigger());

// And register the task
BackgroundTaskRegistration registration = builder.Register();
```


A continuación, en el App.xaml.cs, invalide el método OnBackgroundActivated. A continuación, puede recuperar los argumentos predefinidos y la entrada del usuario, de forma similar a la activación en primer plano.

**App.xaml.cs**

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "ToastBackgroundTask":
            var details = args.TaskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;
            if (details != null)
            {
                string arguments = details.Argument;
                var userInput = details.UserInput;

                // Perform tasks
            }
            break;
    }
 
    deferral.Complete();
}
```


## <a name="set-an-expiration-time"></a>Establecer una fecha de expiración

En Windows 10, todas las notificaciones del sistema entran en el centro de actividades una vez que el usuario las ha descartado o omitido, por lo que los usuarios pueden ver la notificación una vez que el elemento emergente haya desaparecido.

Sin embargo, si el mensaje de la notificación solo es relevante durante un período de tiempo, debe establecer una fecha de expiración en la notificación del sistema para que los usuarios no vean la información obsoleta de la aplicación. Por ejemplo, si una promoción solo es válida durante 12 horas, establezca el tiempo de expiración en 12 horas. En el código siguiente, se establece el tiempo de expiración en 2 días.

> [!NOTE]
> El tiempo de expiración predeterminado y máximo para las notificaciones del sistema local es de 3 días.

```csharp
// Create toast content
var content = new ToastContentBuilder()
    .AddText("Expires in 2 days...")
    .GetToastContent();

// Set expiration time
var notif = new ToastNotification(content.GetXml())
{
    ExpirationTime = DateTime.Now.AddDays(2)
};

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="provide-a-primary-key-for-your-toast"></a>Proporcionar una clave principal para la notificación del sistema

Si desea quitar o reemplazar la notificación que envía mediante programación, debe usar la propiedad de etiqueta (y, opcionalmente, la propiedad de grupo) para proporcionar una clave principal para la notificación. Después, puede usar esta clave principal en el futuro para quitar o reemplazar la notificación.

Para ver más detalles sobre cómo reemplazar o quitar notificaciones del sistema ya entregadas, consulte [Inicio rápido: administrar notificaciones del sistema en el centro de actividades (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10)).

Etiqueta y grupo combinados actúan como clave principal compuesta. Grupo es el identificador más genérico, donde puede asignar grupos como "wallPosts", "Messages", "friendRequests", etc. Y, a continuación, la etiqueta debe identificar de forma única la notificación en el grupo. Mediante el uso de un grupo genérico, puede quitar todas las notificaciones de ese grupo mediante la [API de RemoveGroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_).

```csharp
// Create toast content
var content = new ToastContentBuilder()
    .AddText("New post on your wall!")
    .GetToastContent();

// Set tag/group
new ToastNotification(content.GetXml())
{
    Tag = "18365",
    Group = "wallPosts"
};

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```



## <a name="clear-your-notifications"></a>Borrado de las notificaciones

Las aplicaciones UWP son responsables de quitar y borrar sus propias notificaciones. Cuando se inicia la aplicación, no se borran automáticamente las notificaciones.

Windows solo quitará automáticamente una notificación si el usuario hace clic explícitamente en la notificación.

A continuación se muestra un ejemplo de lo que debe hacer una aplicación de mensajería...

1. El usuario recibe varias notificaciones del sistema sobre nuevos mensajes en una conversación
2. El usuario pulsa una de esas notificaciones del sistema para abrir la conversación
3. La aplicación abre la conversación y borra todas las notificaciones del sistema de esa conversación (mediante [RemoveGroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) en el grupo proporcionado por la aplicación para esa conversación).
4. El centro de actividades del usuario ahora refleja correctamente el estado de la notificación, ya que no hay ninguna notificación obsoleta para esa conversación en el centro de actividades.

Para obtener información sobre cómo borrar todas las notificaciones o quitar notificaciones específicas, consulte [Inicio rápido: administrar notificaciones del sistema en el centro de actividades (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10)).

```csharp
ToastNotificationManager.History.Clear();
```


## <a name="plain-code-snippets"></a>Fragmentos de código sin formato

Si no está usando la biblioteca de notificaciones de NuGet, puede construir manualmente el XML como se muestra a continuación para crear un [ToastNotification](/uwp/api/Windows.UI.Notifications.ToastNotification).

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;

// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "http://blogs.msdn.com/cfs-filesystemfile.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-01-71-81-permanent/2727.happycanyon1_5B00_1_5D00_.jpg";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// TODO: all values need to be XML escaped
 
// Construct the visuals of the toast
string toastVisual =
$@"<visual>
  <binding template='ToastGeneric'>
    <text>{title}</text>
    <text>{content}</text>
    <image src='{image}'/>
    <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
  </binding>
</visual>";

// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Generate the arguments we'll be passing in the toast
string argsReply = $"action=reply&conversationId={conversationId}";
string argsLike = $"action=like&conversationId={conversationId}";
string argsView = $"action=viewImage&imageUrl={Uri.EscapeDataString(image)}";
 
// TODO: all args need to be XML escaped
 
string toastActions =
$@"<actions>
 
  <input
      type='text'
      id='tbReply'
      placeHolderContent='Type a response'/>
 
  <action
      content='Reply'
      arguments='{argsReply}'
      activationType='background'
      imageUri='Assets/Reply.png'
      hint-inputId='tbReply'/>
 
  <action
      content='Like'
      arguments='{argsLike}'
      activationType='background'/>
 
  <action
      content='View'
      arguments='{argsView}'/>
 
</actions>";

// Now we can construct the final toast content
string argsLaunch = $"action=viewConversation&conversationId={conversationId}";
 
// TODO: all args need to be XML escaped
 
string toastXmlString =
$@"<toast launch='{argsLaunch}'>
    {toastVisual}
    {toastActions}
</toast>";
 
// Parse to XML
XmlDocument toastXml = new XmlDocument();
toastXml.LoadXml(toastXmlString);
 
// Generate toast
var toast = new ToastNotification(toastXml);
```


## <a name="resources"></a>Recursos

* [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [Documentación del contenido del sistema](adaptive-interactive-toasts.md)
* [Clase ToastNotification](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [Clase ToastNotificationActivatedEventArgs](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)