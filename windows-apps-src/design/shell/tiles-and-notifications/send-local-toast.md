---
author: anbare
Description: Learn how to send a local toast notification and handle the user clicking the toast.
title: Enviar una notificación de icono local
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, enviar notificaciones del sistema, notificaciones, enviar notificaciones, notificaciones del sistema, cómo, inicio rápido, introducción, ejemplo de código, tutorial
ms.localizationpriority: medium
ms.openlocfilehash: 4f76bc94c80a5191cf7bad86b43230f0d03e81b1
ms.sourcegitcommit: f91aa1e402f1bc093b48a03fbae583318fc7e05d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2018
ms.locfileid: "1917714"
---
# <a name="send-a-local-toast-notification"></a>Enviar una notificación de icono local


Una notificación del sistema es un mensaje que una aplicación puede construir y entregar al usuario, mientras que no se encuentre actualmente dentro de la aplicación. Este Inicio rápido le guía por los pasos de crear, entregar y mostrar una notificación del sistema de Windows 10 con las nuevas plantillas adaptables y acciones interactivas. Estas acciones se demuestran mediante una notificación local, que es la notificación más simple de implementar.

> [!IMPORTANT]
> Las aplicaciones de escritorio (Puente de dispositivo de escritorio y Win32 clásico) tienen distintos pasos para enviar notificaciones y controlar la activación. Consulta la documentación de [Aplicaciones de escritorio](toast-desktop-apps.md) para obtener información acerca de cómo implementar las notificaciones del sistema.

Pasaremos por las siguientes acciones:

### <a name="sending-a-toast"></a>Enviar una notificación del sistema

* Construir la parte visual (texto e imagen) de la notificación
* Agregar acciones a la notificación
* Establecer un tiempo de expiración en la notificación del sistema
* Establecer la etiqueta o el grupo para que puedas reemplazar o quitar la notificación del sistema posteriormente
* Enviar la notificación del sistema con las API locales

### <a name="handling-activation"></a>Control de la activación

* Controlar la activación cuando se haga clic en el cuerpo o los botones
* Controlar la activación en primer plano
* Controlar la activación en segundo plano

> **API importantes**: [Clase ToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification), [Clase ToastNotificationActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)


## <a name="prerequisites"></a>Requisitos previos

Para comprender completamente este tema, será útil lo siguiente...

* Conocimientos prácticos sobre los términos y conceptos relacionados con las notificaciones del sistema. Para obtener más información, consulta [Introducción a la notificación del sistema y al centro de notificaciones](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/).
* Estar familiarizado con el contenido de la notificación del sistema de Windows 10. Para obtener más información, consulta la [documentación del contenido de la notificación del sistema](adaptive-interactive-toasts.md).
* Un proyecto de aplicación para UWP de Windows 10

> [!NOTE]
> A diferencia de Windows 8/8.1, ya no necesitas declarar en el manifiesto de la aplicación que la aplicación puede mostrar notificaciones del sistema. Todas las aplicaciones pueden enviar y mostrar notificaciones del sistema.

> [!NOTE]
> **Aplicaciones de Windows 8/8.1**: usa la [documentación archivada](https://msdn.microsoft.com/library/windows/apps/xaml/hh868254.aspx).


## <a name="install-nuget-packages"></a>Instalación de paquetes NuGet

Te recomendamos instalar los dos siguientes paquetes NuGet en tu proyecto. Nuestro ejemplo de código usará estos paquetes. Al final del artículo, proporcionaremos los fragmentos de código estándar que no usan paquetes NuGet.

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/): genera cargas de notificaciones del sistema mediante objetos en lugar de XML sin formato.
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/): genera y analiza las cadenas de consulta con C#.


## <a name="add-namespace-declarations"></a>Agregar declaraciones de espacios de nombres

`Windows.UI.Notifications` Incluye las API de notificación del sistema.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="send-a-toast"></a>Enviar una notificación del sistema

En Windows 10, el contenido de la notificación del sistema se describe con un lenguaje adaptable que permite una gran flexibilidad con el aspecto de la notificación. Consulta la [documentación del contenido de la notificación del sistema](adaptive-interactive-toasts.md) para obtener más información.

### <a name="constructing-the-visual-part-of-the-content"></a>Construir la parte visual del contenido

Empecemos por construir la parte visual del contenido, que incluye el texto y las imágenes que quieres que el usuario vea.

Gracias a la biblioteca de notificaciones, la generación del contenido XML resulta sencilla. Si no instalas la biblioteca de notificaciones desde NuGet, tienes que construir el XML manualmente, lo que deja lugar a errores.

> [!NOTE]
> Pueden usarse imágenes del paquete de la aplicación, del almacenamiento local de la aplicación o de la Web. A partir de la actualización de Fall Creators Update, las imágenes web pueden tener un tamaño de hasta 3MB, en conexiones normales y de 1MB en conexiones de uso medido. En dispositivos que no ejecutan aún la actualización de Fall Creators Update, el tamaño de las imágenes web no debe ser superior a los 200KB.

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

En el ejemplo siguiente, hemos incluido un elemento de entrada que permite al usuario escribir texto, que se devuelve a la aplicación cuando el usuario hace clic en uno de los botones o en la propia notificación del sistema.

A continuación, hemos añadido dos botones, cada uno de ellos con su propio tipo de activación, contenido y argumentos.
* **ActivationType** se usa para especificar la manera en que se quiere activar la aplicación cuando el usuario realiza esta acción. Puedes elegir iniciar la aplicación en primer plano, iniciar una tarea en segundo plano o realizar el inicio de protocolo de otra aplicación. Si la aplicación elige primer o segundo plano, siempre recibirás la entrada del usuario y los argumentos que especificaste para que la aplicación pueda realizar la acción correcta, como enviar el mensaje o abrir una conversación.

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


### <a name="combining-the-above-to-construct-the-full-content"></a>Combinar lo anterior para construir el contenido completo

La construcción del contenido está ahora completa y podemos usarla para crear una instancia del objeto [**ToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification).

**Nota**: También puedes proporcionar un tipo de activación dentro del elemento raíz, para especificar qué tipo de activación debe producirse cuando el usuario pulsa en el cuerpo de la notificación del sistema. Normalmente, al pulsa en el cuerpo de la notificación del sistema debería iniciar la aplicación en primer plano para crear una experiencia de usuario coherente, pero puedes usar otros tipos de activación que se adapten a tu escenario específico donde tenga más sentido para el usuario.

Siempre debes establecer la propiedad **Launch** para que cuando el usuario pulse el cuerpo de la notificación del sistema y se inicie tu aplicación, tu aplicación sepa qué contenido debe mostrar.

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


## <a name="set-an-expiration-time"></a>Establecer un tiempo de expiración

En Windows 10, todas las notificaciones del sistema se encuentran en el centro de actividades después de que se descarten o se omitan por el usuario, de manera que los usuarios puedan echar un vistazo a la notificación después de que el control emergente haya desaparecido.

Sin embargo, si el mensaje de la notificación solo es relevante durante un período de tiempo, debes establecer un tiempo de expiración en la notificación del sistema para que los usuarios no vean información obsoleta de la aplicación. Por ejemplo, si una promoción solo es válida durante 12 horas, establece el tiempo de expiración en 12 horas. En el código siguiente, establecemos el tiempo de expiración en 2 días.

> [!NOTE]
> El tiempo de expiración máximo y predeterminado para notificaciones del sistema local es de 3 días.

```csharp
toast.ExpirationTime = DateTime.Now.AddDays(2);
```


## <a name="provide-a-primary-key-for-your-toast"></a>Proporcionar una clave principal para la notificación del sistema

Si quieres eliminar o reemplazar mediante programación la notificación que envíes, debes usar la propiedad Tag (y, opcionalmente, la propiedad Group) para proporcionar una clave principal para la notificación. A continuación, puedes usar esta clave principal en el futuro para eliminar o reemplazar la notificación.

Para ver más detalles sobre reemplazar o quitar las notificaciones del sistema ya entregadas, consulta [Inicio rápido: administración de notificaciones del sistema en el Centro de actividades (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/dn631260.aspx).

Etiqueta y Grupo combinados actúan como clave principal compuesta. Grupo es el identificador más genérico, donde puedes asignar grupos como "wallPosts", "messages", "friendRequests", messages. Y, a continuación, Tag debe identificar de manera única la propia notificación desde dentro del grupo. Al usar un grupo genérico, puedes quitar todas las notificaciones de ese grupo mediante la [API RemoveGroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_).

```csharp
toast.Tag = "18365";
toast.Group = "wallPosts";
```


## <a name="send-the-notification"></a>Enviar la notificación

Una vez que has inicializado la notificación del sistema, simplemente creas un [ToastNotifier](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier) y llamas a Show(), pasando la notificación del sistema.

```csharp
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


## <a name="clear-your-notifications"></a>Borrar las notificaciones

Las aplicaciones para UWP son responsables de quitar y borrar sus propias notificaciones. Cuando se inicia la aplicación, NO borramos automáticamente las notificaciones.

Windows solo quitará automáticamente una notificación si el usuario hace clic en la notificación de manera explícita.

Este es un ejemplo de lo que debe hacer una aplicación de mensajería...

1. El usuario recibe varias notificaciones del sistema acerca de nuevos mensajes en una conversación
2. El usuario pulsa una de esas notificaciones del sistema para abrir la conversación
3. La aplicación abre la conversación y, a continuación, borra todas las notificaciones del sistema para esa conversación (usando [RemoveGroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) en el grupo suministrado por la aplicación para esa conversación)
4. El centro de actividades del usuario refleja correctamente ahora el estado de la notificación, ya que no queda ninguna notificación obsoleta para esa conversación en el centro de actividades.

Para aprender a borrar todas las notificaciones o quitar notificaciones específicas, consulta [Inicio rápido: administración de notificaciones del sistema en el Centro de actividades (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/dn631260.aspx).


## <a name="handling-activation"></a>Control de la activación

En Windows 10, cuando el usuario hace clic en la notificación del sistema, puede hacer que la notificación del sistema active la aplicación de dos maneras diferentes...

* Activación en primer plano
* Activación en segundo plano

> [!NOTE]
> Si estás usando las plantillas de notificación del sistema heredadas de Windows 8.1, se llamará a **OnLaunched** en lugar de **OnActivated**. La siguiente documentación solo se aplica a las notificaciones de Windows 10 modernas utilizando la biblioteca de notificaciones (o la plantilla ToastGeneric si se usa XML sin formato).


### <a name="handling-foreground-activation"></a>Control de la activación en primer plano

En Windows 10, cuando un usuario hace clic en una notificación del sistema moderna (o un botón en la notificación del sistema), **OnActivated** se invoca en lugar de **OnLaunched**, con un nuevo tipo de activación: **ToastNotification**. Por lo tanto, el desarrollador puede distinguir una activación de la notificación del sistema con facilidad y realizar tareas según corresponda.

En el ejemplo que ves a continuación, puedes recuperar la cadena de argumentos que proporcionaste inicialmente en el contenido de la notificación del sistema. También puedes recuperar la entrada que el usuario proporcionó en los cuadros de texto y los cuadros de selección.

> [!IMPORTANT]
> Debes inicializar el marco y activar la ventana igual que el código de **OnLaunched**. **NO se llama a OnLaunched si el usuario hace clic en la notificación del sistema**, incluso si la aplicación estaba cerrada y se inicia por primera vez. A menudo, se recomienda combinar **OnLaunched** y **OnActivated** en su propio método `OnLaunchedOrActivated` puesto que la misma inicialización debe producirse en ambos.

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

Cuando especifiques la activación en segundo plano en la notificación del sistema (o en un botón dentro de la notificación del sistema), se ejecutará la tarea en segundo plano en lugar de activar la aplicación en primer plano.

Para obtener más información sobre las tareas en segundo plano, consulta [Compatibilidad de la aplicación con tareas en segundo plano](/launch-resume/support-your-app-with-background-tasks.md).

Si tu aplicación se dirige a una versión de 14393 o posterior, puedes usar tareas en segundo plano en proceso, lo que simplifica las cosas enormemente. Ten en cuenta que las tareas en segundo plano en proceso no podrán ejecutarse en versiones antiguas de Windows. Usaremos una tarea en segundo plano en proceso en este ejemplo de código.

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


A continuación, en tu App.xaml.cs, invalida el método OnBackgroundActivated puede recuperar los argumentos predefinidos y la entrada del usuario, similar a la activación en primer plano.

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



## <a name="plain-vanilla-code-snippets"></a>Fragmentos de código estándar sin formato

Si no estás usando la biblioteca de notificaciones de NuGet, puedes construir manualmente el XML tal como se muestra a continuación para crear un [ToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification).

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

* [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-toast)
* [Documentación del contenido de la notificación del sistema](adaptive-interactive-toasts.md)
* [Clase ToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)
* [Clase ToastNotificationActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)
