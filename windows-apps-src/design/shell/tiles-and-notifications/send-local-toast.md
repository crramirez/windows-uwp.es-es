---
Description: Obtenga información sobre cómo enviar una notificación del sistema local y controlar el usuario que hace clic en la notificación del sistema.
title: Enviar una notificación de icono local
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP, enviar notificaciones del sistema, notificaciones, enviar notificaciones, notificaciones del sistema, procedimientos, Inicio rápido, introducción, ejemplo de código, tutorial
ms.localizationpriority: medium
ms.openlocfilehash: 8e099ae97f67ca2f61a9e771f7ad015305b851d0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174599"
---
# <a name="send-a-local-toast-notification"></a>Enviar una notificación de icono local


Una notificación del sistema es un mensaje que una aplicación puede crear y enviar al usuario mientras no está actualmente dentro de la aplicación. Esta guía de inicio rápido le guiará por los pasos necesarios para crear, enviar y mostrar una notificación del sistema de Windows 10 con las nuevas plantillas adaptables y las acciones interactivas. Estas acciones se muestran a través de una notificación local, que es la notificación más sencilla que se va a implementar.

> [!IMPORTANT]
> Las aplicaciones de escritorio (incluidas las aplicaciones empaquetadas de [MSIX](/windows/msix/desktop/source-code-overview) , las aplicaciones que usan [paquetes dispersos](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) para obtener la identidad del paquete y las aplicaciones Win32 clásicas no empaquetadas) tienen diferentes pasos para enviar notificaciones y controlar la activación. Consulte la documentación de las [aplicaciones de escritorio](toast-desktop-apps.md) para obtener información sobre cómo implementar notificaciones del sistema.

Analizaremos lo siguiente:

### <a name="sending-a-toast"></a>Envío de una notificación del sistema

* Construir la parte visual (texto e imagen) de la notificación
* Agregar acciones a la notificación
* Establecer una fecha de expiración en la notificación del sistema
* Configuración de la etiqueta o el grupo para que pueda reemplazar o quitar la notificación del sistema en otro momento.
* Envío de la notificación del sistema mediante las API locales

### <a name="handling-activation"></a>Controlar la activación

* Controlar la activación cuando se hace clic en el cuerpo o los botones
* Controlar la activación en primer plano
* Controlar la activación en segundo plano

> **API importantes**: [clase ToastNotification](/uwp/api/Windows.UI.Notifications.ToastNotification), [clase ToastNotificationActivatedEventArgs](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)


## <a name="prerequisites"></a>Requisitos previos

Para entender completamente este tema, le resultará útil lo siguiente...

* Conocimiento práctico de los términos y conceptos de las notificaciones del sistema. Para obtener más información, consulte la información [General del sistema y del centro de actividades](/archive/blogs/tiles_and_toasts/toast-notification-and-action-center-overview-for-windows-10).
* Familiaridad con el contenido de las notificaciones del sistema de Windows 10. Para obtener más información, consulte [documentación del contenido del sistema](adaptive-interactive-toasts.md).
* Un proyecto de aplicación para UWP de Windows 10

> [!NOTE]
> A diferencia de Windows 8/8.1, ya no es necesario declarar en el manifiesto de la aplicación que la aplicación es capaz de mostrar las notificaciones del sistema. Todas las aplicaciones pueden enviar y Mostrar notificaciones del sistema.

> [!NOTE]
> **Aplicaciones de Windows 8/8.1**: Use la [documentación archivada](/previous-versions/windows/apps/hh868254(v=win.10)).


## <a name="install-nuget-packages"></a>Instalación de paquetes NuGet

Se recomienda instalar los dos paquetes de NuGet siguientes en el proyecto. Nuestro ejemplo de código usará estos paquetes. Al final del artículo, proporcionaremos los fragmentos de código "vainilla" que no usan ningún paquete NuGet.

* [Microsoft. Toolkit. UWP. notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/): genere cargas del sistema a través de objetos en lugar de XML sin formato.
* [QueryString.net](https://www.nuget.org/packages/QueryString.NET/): generar y analizar cadenas de consulta con C #


## <a name="add-namespace-declarations"></a>Incorporación de declaraciones de espacio de nombres

`Windows.UI.Notifications` incluye las API del sistema.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="send-a-toast"></a>Enviar una notificación del sistema

En Windows 10, el contenido de la notificación del sistema se describe mediante un lenguaje adaptable que permite una gran flexibilidad con el aspecto de la notificación. Consulte la [documentación del contenido del sistema](adaptive-interactive-toasts.md) para obtener más información.

### <a name="constructing-the-visual-part-of-the-content"></a>Construir la parte visual del contenido

Comencemos por crear la parte visual del contenido, que incluye el texto y las imágenes que desea que el usuario vea.

Gracias a la biblioteca de notificaciones, la generación del contenido XML es sencilla. Si no instala la biblioteca de notificaciones desde NuGet, tendrá que construir el XML manualmente, lo que deja espacio para los errores.

> [!NOTE]
> Las imágenes se pueden usar desde el paquete de la aplicación, el almacenamiento local de la aplicación o desde la Web. A partir de la actualización de Fall Creators, las imágenes Web pueden tener hasta 3 MB en las conexiones normales y 1 MB en las conexiones de uso medido. En los dispositivos que aún no ejecuten el Fall Creators Update, las imágenes web no deben tener más de 200 KB.

```csharp
// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "https://picsum.photos/360/202?image=883";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// Construct the visuals of the toast
ToastVisual visual = new ToastVisual()
{
    BindingGeneric = new ToastBindingGeneric()
    {
        Children =
        {
            new AdaptiveText()
            {
                Text = title
            },
 
            new AdaptiveText()
            {
                Text = content
            },
 
            new AdaptiveImage()
            {
                Source = image
            }
        },
 
        AppLogoOverride = new ToastGenericAppLogo()
        {
            Source = logo,
            HintCrop = ToastGenericAppLogoCrop.Circle
        }
    }
};
```


### <a name="constructing-actions-part-of-the-content"></a>Crear acciones parte del contenido

Ahora vamos a agregar acciones al contenido.

En el ejemplo siguiente, se incluye un elemento INPUT que permite al usuario escribir texto, que se devuelve a la aplicación cuando el usuario hace clic en uno de los botones o en el propio sistema.

Después hemos agregado dos botones, cada uno con su propio tipo de activación, contenido y argumentos.
* **ActivationType** se usa para especificar cómo desea que la aplicación se active cuando el usuario realiza esta acción. Puede optar por iniciar la aplicación en primer plano, iniciar una tarea en segundo plano o el protocolo iniciar otra aplicación. Tanto si la aplicación elige en primer plano o en segundo plano, siempre recibirá la entrada del usuario y los argumentos especificados, de modo que la aplicación pueda realizar la acción correcta, como enviar el mensaje o abrir una conversación.

```csharp
// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Construct the actions for the toast (inputs and buttons)
ToastActionsCustom actions = new ToastActionsCustom()
{
    Inputs =
    {
        new ToastTextBox("tbReply")
        {
            PlaceholderContent = "Type a response"
        }
    },
 
    Buttons =
    {
        new ToastButton("Reply", new QueryString()
        {
            { "action", "reply" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background,
            ImageUri = "Assets/Reply.png",
 
            // Reference the text box's ID in order to
            // place this button next to the text box
            TextBoxId = "tbReply"
        },
 
        new ToastButton("Like", new QueryString()
        {
            { "action", "like" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background
        },
 
        new ToastButton("View", new QueryString()
        {
            { "action", "viewImage" },
            { "imageUrl", image }
 
        }.ToString())
    }
};
```


### <a name="combining-the-above-to-construct-the-full-content"></a>Combinación de lo anterior para construir el contenido completo

La construcción del contenido está ahora completa y podemos utilizarlo para crear una instancia del objeto [**ToastNotification**](/uwp/api/Windows.UI.Notifications.ToastNotification) .

**Nota**: también puede proporcionar un tipo de activación dentro del elemento raíz para especificar qué tipo de activación debe producirse cuando el usuario puntea en el cuerpo de la notificación del sistema. Normalmente, al puntear en el cuerpo de la notificación del sistema, se debe iniciar la aplicación en primer plano para crear una experiencia coherente con el usuario, pero puede usar otros tipos de activación para ajustarse a su escenario concreto, donde es más conveniente para el usuario.

Siempre debe establecer la propiedad **Launch** , por lo que cuando el usuario puntea el cuerpo de la notificación del sistema y se inicia la aplicación, la aplicación sabe qué contenido debe mostrar.

```csharp
// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = visual,
    Actions = actions,
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
 
    }.ToString()
};
 
// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());
```


## <a name="set-an-expiration-time"></a>Establecer una fecha de expiración

En Windows 10, todas las notificaciones del sistema entran en el centro de actividades una vez que el usuario las ha descartado o omitido, por lo que los usuarios pueden ver la notificación una vez que el elemento emergente haya desaparecido.

Sin embargo, si el mensaje de la notificación solo es relevante durante un período de tiempo, debe establecer una fecha de expiración en la notificación del sistema para que los usuarios no vean la información obsoleta de la aplicación. Por ejemplo, si una promoción solo es válida durante 12 horas, establezca el tiempo de expiración en 12 horas. En el código siguiente, se establece el tiempo de expiración en 2 días.

> [!NOTE]
> El tiempo de expiración predeterminado y máximo para las notificaciones del sistema local es de 3 días.

```csharp
toast.ExpirationTime = DateTime.Now.AddDays(2);
```


## <a name="provide-a-primary-key-for-your-toast"></a>Proporcionar una clave principal para la notificación del sistema

Si desea quitar o reemplazar la notificación que envía mediante programación, debe usar la propiedad de etiqueta (y, opcionalmente, la propiedad de grupo) para proporcionar una clave principal para la notificación. Después, puede usar esta clave principal en el futuro para quitar o reemplazar la notificación.

Para ver más detalles sobre cómo reemplazar o quitar notificaciones del sistema ya entregadas, consulte [Inicio rápido: administrar notificaciones del sistema en el centro de actividades (XAML)](/previous-versions/windows/apps/dn631260(v=win.10)).

Etiqueta y grupo combinados actúan como clave principal compuesta. Grupo es el identificador más genérico, donde puede asignar grupos como "wallPosts", "Messages", "friendRequests", etc. Y, a continuación, la etiqueta debe identificar de forma única la notificación en el grupo. Mediante el uso de un grupo genérico, puede quitar todas las notificaciones de ese grupo mediante la [API de RemoveGroup](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_).

```csharp
toast.Tag = "18365";
toast.Group = "wallPosts";
```


## <a name="send-the-notification"></a>Enviar la notificación

Una vez que haya inicializado la notificación del sistema, basta con crear un [ToastNotifier](/uwp/api/windows.ui.notifications.toastnotifier) y llamar a Show (), pasando la notificación del sistema.

```csharp
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


## <a name="clear-your-notifications"></a>Borrado de las notificaciones

Las aplicaciones UWP son responsables de quitar y borrar sus propias notificaciones. Cuando se inicia la aplicación, no se borran automáticamente las notificaciones.

Windows solo quitará automáticamente una notificación si el usuario hace clic explícitamente en la notificación.

A continuación se muestra un ejemplo de lo que debe hacer una aplicación de mensajería...

1. El usuario recibe varias notificaciones del sistema sobre nuevos mensajes en una conversación
2. El usuario pulsa una de esas notificaciones del sistema para abrir la conversación
3. La aplicación abre la conversación y borra todas las notificaciones del sistema de esa conversación (mediante [RemoveGroup](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) en el grupo proporcionado por la aplicación para esa conversación).
4. El centro de actividades del usuario ahora refleja correctamente el estado de la notificación, ya que no hay ninguna notificación obsoleta para esa conversación en el centro de actividades.

Para obtener información sobre cómo borrar todas las notificaciones o quitar notificaciones específicas, consulte [Inicio rápido: administrar notificaciones del sistema en el centro de actividades (XAML)](/previous-versions/windows/apps/dn631260(v=win.10)).


## <a name="activation-handling"></a>Control de activación

En Windows 10, cuando el usuario hace clic en la notificación del sistema, puede hacer que el sistema Active la aplicación de dos maneras diferentes...

* Activación en primer plano
* Activación en segundo plano

> [!NOTE]
> Si usa las plantillas del sistema de notificaciones heredadas de Windows 8.1, se llamará a **onlaunched** en lugar de a **onactivated**. La siguiente documentación solo se aplica a las notificaciones modernas de Windows 10 que usan la biblioteca de notificaciones (o la plantilla ToastGeneric si usa XML sin formato).


### <a name="handling-foreground-activation"></a>Controlar la activación en primer plano

En Windows 10, cuando un usuario hace clic en una notificación del sistema moderna (o en un botón de la notificación del sistema), se invoca **onactivated** en lugar de **onlaunched**, con un nuevo tipo de activación: **ToastNotification**. Por lo tanto, el desarrollador puede distinguir fácilmente una activación del sistema y realizar tareas en consecuencia.

En el ejemplo que aparece a continuación, puede recuperar la cadena de argumentos que proporcionó inicialmente en el contenido del sistema. También puede recuperar la entrada proporcionada por el usuario en los cuadros de texto y cuadros de selección.

> [!IMPORTANT]
> Debe inicializar el marco y activar la ventana del mismo modo que el código **onlaunched** . **No se llama a onlaunched si el usuario hace clic en la notificación del sistema**, incluso si la aplicación se cerró y se inicia por primera vez. A menudo se recomienda combinar **onlaunched** y **onactivated** en su propio `OnLaunchedOrActivated` método, ya que la misma inicialización debe realizarse en ambos.

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        var toastActivationArgs = e as ToastNotificationActivatedEventArgs;
                 
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


## <a name="handling-background-activation"></a>Controlar la activación en segundo plano

Cuando se especifica la activación en segundo plano en el sistema (o en un botón dentro de la notificación del sistema), se ejecutará la tarea en segundo plano en lugar de activar la aplicación en primer plano.

Para obtener más información sobre las tareas en segundo plano, consulte [soporte técnico de la aplicación con tareas en segundo plano](../../../launch-resume/support-your-app-with-background-tasks.md).

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


Después, en el App.xaml.cs, invalide el método OnBackgroundActivated puede recuperar los argumentos predefinidos y la entrada del usuario, de forma similar a la activación en primer plano.

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



## <a name="plain-vanilla-code-snippets"></a>Fragmentos de código "vainilla" sin formato

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