---
author: andrewleader
Description: Learn how Win32 C# apps can send local toast notifications and handle the user clicking the toast.
title: Enviar una notificaciones del sistema local desde aplicaciones de C# de escritorio
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from desktop C# apps
template: detail.hbs
ms.author: mijacobs
ms.date: 01/23/2018
ms.topic: article
keywords: windows10, uwp, win32, escritorio, notificaciones del sistema, enviar una notificación del sistema, enviar notificación del sistema local, puente de dispositivo de escritorio, C#, c sharp
ms.localizationpriority: medium
ms.openlocfilehash: 9e828787a1a342b78cd72d1e7afc3fe9df0f0eea
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "5930281"
---
# <a name="send-a-local-toast-notification-from-desktop-c-apps"></a>Enviar una notificaciones del sistema local desde aplicaciones de C# de escritorio

Las aplicaciones de escritorio (Puente de dispositivo de escritorio y Win32 clásico) pueden enviar notificaciones del sistema interactivas igual de la misma manera que las aplicaciones de la Plataforma universal de Windows (UWP). Sin embargo, hay algunos pasos especiales para aplicaciones de escritorio debido a los diferentes esquemas de activación y a la posible falta de identidad del paquete si no usas el Puente de dispositivo de escritorio.

> [!IMPORTANT]
> Si estás escribiendo una aplicación para UWP, consulta la [documentación de UWP](send-local-toast.md). Para otros lenguajes de escritorio, consulta [WRL de C++ de escritorio](send-local-toast-desktop-cpp-wrl.md).


## <a name="step-1-enable-the-windows-10-sdk"></a>Paso 1: Habilitar el SDK de Windows 10

Si no has habilitado el SDK de Windows 10 para tu aplicación de Win32, debes hacerlo primero.

Haz clic con el botón derecho en el proyecto y selecciona **Descargar el proyecto**.

![Descarga de un proyecto](images/win32-unload-project.png)

A continuación, haz clic con el botón derecho en el proyecto de nuevo y selecciona **Editar [nombreDeProyecto].csproj**.

![Edición de un proyecto](images/win32-edit-project.png)

A continuación, el nodo existente `<TargetFrameworkVersion>`, agrega un nuevo nodo `<TargetPlatformVersion>` especificando la versión mínima de Windows 10 admitida. El SDK real usado será el SDK más reciente que has instalado en tu equipo de desarrollo. Esto simplemente especifica la versión mínima permitida (y te permite hacer referencia al SDK de Windows).

```xml
...
<TargetFrameworkVersion>...</TargetFrameworkVersion>
<TargetPlatformVersion>10.0.10240.0</TargetPlatformVersion>
...
```

Guarda tus cambios y, a continuación, vuelve a cargar tu proyecto.

![Volver a cargar un proyecto](images/win32-reload-project.png)


## <a name="step-2-reference-the-apis"></a>Paso 2: Hacer referencia a las API

Abre el Administrador de referencias (haz clic con el botón derecho en el proyecto, selecciona **Agregar-> Referencia**) y selecciona **Windows-> Core** e incluye las siguientes referencias:

* Windows.Data
* Windows.UI

![Administrador de referencias](images/win32-add-windows-reference.png)


## <a name="step-3-copy-compat-library-code"></a>Paso 3: Copiar código de biblioteca compat

Copia el [archivo DesktopNotificationManagerCompat.cs desde GitHub](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CS/DesktopToastsApp/DesktopNotificationManagerCompat.cs) en tu proyecto. La biblioteca compat toma buena parte de la complejidad de las notificaciones de escritorio. Las siguientes instrucciones requieren la biblioteca compat.


## <a name="step-4-implement-the-activator"></a>Paso 4: Implementar el activador

Debes implementar un controlador para la activación de notificación del sistema, para que cuando el usuario hace clic en la notificación del sistema, la aplicación puede hacer algo. Esto es necesario para que la notificación del sistema se conserve en el Centro de actividades (ya que se podría hacer clic en la notificación del sistema cuando se cierre la aplicación). Esta clase se puede colocar en cualquier lugar de tu proyecto.

Amplía la clase **NotificationActivator** y, a continuación, agrega los tres atributos que se indican a continuación y crea un único CLSID GUID para tu aplicación usando uno de los muchos generadores de GUID en línea. Este CLSID (identificador de clase) es la manera a través de la cual el Centro de actividades sabe qué clase de COM activar.

```csharp
// The GUID CLSID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        // TODO: Handle activation
    }
}
```


## <a name="step-5-register-with-notification-platform"></a>Paso 5: Registrarse con la plataforma de notificaciones

A continuación, debes registrarte con la plataforma de notificaciones. Hay diferentes pasos en función de si usas el Puente de dispositivo de escritorio o Win32 clásico. Si admites ambos, debes hacer los dos pasos (sin embargo, no es necesario bifurcar tu código, ¡nuestra biblioteca lo controla por ti!).


### <a name="desktop-bridge"></a>Puente de dispositivo de escritorio

Si estás usando el Puente de dispositivo de escritorio (o, si admites ambos) en tu **Package.appxmanifest**, agrega:

1. Declaración para **xmlns:com**
2. Declaración para **xmlns:desktop**
3. En el atributo **IgnorableNamespaces**, **com** y **escritorio**.
4. **com:Extension** para el activador COM utilizando el GUID del paso n.º4. Asegúrate de incluir el valor de `Arguments="-ToastActivated"` para que sepas que tu inicio procedía de una notificación del sistema.
5. **desktop:Extension** para **windows.toastNotificationActivation** para declarar el CLSID del activador de la notificación del sistema (el GUID del paso n.º4).

**Package.appxmanifest**

```xml
<Package
  ...
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="... com desktop">
  ...
  <Applications>
    <Application>
      ...
      <Extensions>

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```


### <a name="classic-win32"></a>Win32 clásico

Si estás usando Win32 clásico (o si admites ambos), tienes que declarar tu id. de modelo de usuario de aplicación (AUMID) y CLSID de activador de notificación del sistema (el GUID del paso n.º4) en el método abreviado de la aplicación en Inicio.

Elige un AUMID único que identifica tu aplicación de Win32. Esto suele estar en el formato de [CompanyName].[AppName], pero quieres garantizar que esto es único en todas las aplicaciones (puedes agregar algunos dígitos al final).

#### <a name="step-51-wix-installer"></a>Paso 5.1: Instalar WiX

Si estás usando WiX para tu instalador, edita el archivo **Product.wxs** para agregar las dos propiedades de acceso directo al menú Inicio como se muestra a continuación. Asegúrate de que tu GUID del paso n.º4 esté incluido entre `{}` como se ve a continuación.

**Product.wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> Para usar notificaciones realmente, debes instalar tu aplicación mediante el instalador una vez antes de la depuración normalmente para que el acceso directo de Inicio con tus AUMID y CLSID estén presentes. Después de que el acceso directo de Inicio esté presente, puedes depurar con F5 desde Visual Studio.


#### <a name="step-52-register-aumid-and-com-server"></a>Paso 5.2: Registrar servidor COM y AUMID

A continuación, con independencia de su instalador, en el código de inicio de tu aplicación (antes de llamar a cualquier API de notificación), llama al método **RegisterAumidAndComServer**, especificando la clase de activador de notificación del paso n.º4 y tu AUMID utilizada anteriormente.

```csharp
// Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
```

Si admites tanto el Puente de dispositivo de escritorio como Win32 clásico, puedes llamar a este método. Si estás ejecutando en el Puente de dispositivo de escritorio, este método simplemente regresará inmediatamente. No es necesario que bifurques el código.

Este método te permite llamar a las API de compatibilidad para enviar y administrar notificaciones sin tener que proporcionar constantemente tu AUMID. E inserta la clave del Registro de LocalServer32 para el servidor COM.


## <a name="step-6-register-com-activator"></a>Paso 6: Registrar el activador COM

Tanto para el Puente de dispositivo de escritorio como para las aplicaciones de Win32 clásicas, debes registrar tu tipo de activador de notificación, por lo que puedes controlar las activaciones de notificación del sistema.

En el código de inicio de la aplicación, llama al siguiente método **RegisterActivator**, pasando tu implementación de la clase **NotificationActivator** que creaste en el paso n.º4. Debes llamar a él para que puedas recibir las activaciones de notificación del sistema.

```csharp
// Register COM server and activator type
DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();
```


## <a name="step-7-send-a-notification"></a>Paso 7: Enviar una notificación

La acción de enviar una notificación es idéntica a aplicaciones para UWP, excepto en que usarás la clase **DesktopNotificationManagerCompat** para crear un **ToastNotifier**. La biblioteca de compatibilidad controla automáticamente la diferencia entre el Puente de dispositivo de escritorio y Win32 clásico para que no tengas que bifurcar el código. Para Win32 clásico, la biblioteca de compatibilidad almacena en caché el AUMID que proporcionaste al llamar a **RegisterAumidAndComServer** para que no tengas que preocuparte de cuándo debes proporcionar o no el AUMID.

> [!NOTE]
> Instala la [biblioteca Notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para poder construir notificaciones con C# como se muestra a continuación, en lugar de usar XML sin formato.

Asegúrate de usar el enlace **ToastContent** como se muestra (o la plantilla ToastGeneric si está creando a mano XML) puesto que las plantillas de notificación del sistema de Windows 8.1 heredadas no activarán el activador de notificaciones COM que creaste en el paso n.4.

> [!IMPORTANT]
> Las imágenes HTTP solo se admiten en las aplicaciones del Puente de dispositivo de escritorio que tienen la funcionalidad de Internet en su manifiesto. Las aplicaciones de Win32 clásicas no admiten imágenes http; debes descargar la imagen en los datos locales de la aplicación y hacer referencia a ellos de manera local.

```csharp
// Construct the visuals of the toast (using Notifications library)
ToastContent toastContent = new ToastContent()
{
    // Arguments when the user taps body of toast
    Launch = "action=viewConversation&conversationId=5",

    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Hello world!"
                }
            }
        }
    }
};

// Create the XML document (BE SURE TO REFERENCE WINDOWS.DATA.XML.DOM)
var doc = new XmlDocument();
doc.LoadXml(toastContent.GetContent());

// And create the toast notification
var toast = new ToastNotification(doc);

// And then show it
DesktopNotificationManagerCompat.CreateToastNotifier().Show(toast);
```

> [!IMPORTANT]
> Las aplicaciones de Win32 clásicas no pueden usar plantillas de notificación del sistema heredadas (como ToastText02). Se producirá un error en la activación de las plantillas heredadas cuando se especifique el CLSID COM. Debes usar las plantillas ToastGeneric de Windows 10 como se muestra arriba.


## <a name="step-8-handling-activation"></a>Paso 8: Control de la activación

Cuando el usuario hace clic en tu notificación del sistema, se invoca el método **OnActivated** de tu clase **NotificationActivator**.

En el método OnActivated, puedes analizar los argumentos que especificaste en la notificación del sistema y obtener la entrada del usuario que el usuario ha escrito o seleccionado y, a continuación, activar la aplicación según corresponda.

> [!NOTE]
> Al método **OnActivated** no se llama en el subproceso de interfaz de usuario. Si quieres realizar operaciones de subproceso de interfaz de usuario, debes llamar a `Application.Current.Dispatcher.Invoke(callback)`.

```csharp
// The GUID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        Application.Current.Dispatcher.Invoke(delegate
        {
            // Tapping on the top-level header launches with empty args
            if (arguments.Length == 0)
            {
                // Perform a normal launch
                OpenWindowIfNeeded();
                return;
            }

            // Parse the query string (using NuGet package QueryString.NET)
            QueryString args = QueryString.Parse(invokedArgs);

            // See what action is being requested 
            switch (args["action"])
            {
                // Open the image
                case "viewImage":

                    // The URL retrieved from the toast args
                    string imageUrl = args["imageUrl"];

                    // Make sure we have a window open and in foreground
                    OpenWindowIfNeeded();

                    // And then show the image
                    (App.Current.Windows[0] as MainWindow).ShowImage(imageUrl);

                    break;

                // Background: Quick reply to the conversation
                case "reply":

                    // Get the response the user typed
                    string msg = userInput["tbReply"];

                    // And send this message
                    SendMessage(msg);

                    // If there's no windows open, exit the app
                    if (App.Current.Windows.Count == 0)
                    {
                        Application.Current.Shutdown();
                    }

                    break;
            }
        });
    }

    private void OpenWindowIfNeeded()
    {
        // Make sure we have a window open (in case user clicked toast while app closed)
        if (App.Current.Windows.Count == 0)
        {
            new MainWindow().Show();
        }

        // Activate the window, bringing it to focus
        App.Current.Windows[0].Activate();

        // And make sure to maximize the window too, in case it was currently minimized
        App.Current.Windows[0].WindowState = WindowState.Normal;
    }
}
```

Para admitir correctamente el inicio mientras se cierra la aplicación, en el archivo `App.xaml.cs`, querrás invalidar el método **OnStartup** (para aplicaciones WPF) para determinar si estás iniciando desde una notificación del sistema o no. Si se inicia desde una notificación del sistema, habrá un argumento de inicio de "-ToastActivated". Cuando veas este, debes dejar de ejecutar cualquier código de activación de inicio normal y permitir que tu código **OnActivated** controle el inicio de ventanas.

```csharp
protected override async void OnStartup(StartupEventArgs e)
{
    // Register AUMID, COM server, and activator
    DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
    DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();

    // If launched from a toast
    if (e.Args.Contains("-ToastActivated"))
    {
        // Our NotificationActivator code will run after this completes,
        // and will show a window if necessary.
    }

    else
    {
        // Show the window
        // In App.xaml, be sure to remove the StartupUri so that a window doesn't
        // get created by default, since we're creating windows ourselves (and sometimes we
        // don't want to create a window if handling a background activation).
        new MainWindow().Show();
    }

    base.OnStartup(e);
}
```


### <a name="activation-sequence-of-events"></a>Secuencia de activación de eventos

Para WPF, la secuencia de activación es la siguiente...

Si tu aplicación ya se está ejecutando:

1. Se llama a **OnActivated** en tu **NotificationActivator**.

Si tu aplicación no se está ejecutando:

1. Se llama a **OnStartup** en `App.xaml.cs` con **Args** de "-ToastActivated"
2. Se llama a **OnActivated** en tu **NotificationActivator**.


### <a name="foreground-vs-background-activation"></a>Activación en primer plano frente a activación en segundo plano
Para aplicaciones de escritorio, la activación en primer plano y en segundo plano se controla de forma idéntica: se llama a tu activador de COM. Depende del código de tu aplicación decidir si se mostrará una ventana o simplemente se realizará algún trabajo y después se saldrá. Por lo tanto, al especificar un **ActivationType** **Segundo plano** en el contenido de la notificación del sistema no se cambia el comportamiento.


## <a name="step-9-remove-and-manage-notifications"></a>Paso 9: Quitar y administrar notificaciones

El proceso de quitar y administrar notificaciones es idéntico a las aplicaciones para UWP. Sin embargo, se recomienda usar nuestra biblioteca de compatibilidad para obtener un valor de **DesktopNotificationHistoryCompat** de manera que no tenga que preocuparse de proporcionar el AUMID si estás usando Win32 clásico.

```csharp
// Remove the toast with tag "Message2"
DesktopNotificationManagerCompat.History.Remove("Message2");

// Clear all toasts
DesktopNotificationManagerCompat.History.Clear();
```


## <a name="step-10-deploying-and-debugging"></a>Paso 10: Implementación y depuración

Para implementar y depurar tu aplicación del Puente de dispositivo de escritorio, consulta [Ejecutar, depurar y probar una aplicación de escritorio empaquetada](/porting/desktop-to-uwp-debug.md).

Para implementar y depurar tu aplicación de Win32 clásica, debes instalar tu aplicación mediante el instalador una vez antes de la depuración normalmente para que el acceso directo de Inicio con tus AUMID y CLSID estén presentes. Después de que el acceso directo de Inicio esté presente, puedes depurar con F5 desde Visual Studio.

Si las notificaciones simplemente no aparecen en tu aplicación de Win32 clásica (y no se generan excepciones), probablemente quiere decir que el acceso directo de Inicio no está presente (instala tu aplicación mediante el instalador) o el AUMID que usaste en el código no coincide con el AUMID del acceso directo de Inicio.

Si las notificaciones aparecen pero no se conservan en el Centro de actividades (desaparecen después de descartar el control emergente), eso significa que no has implementado el activador COM correctamente.

Si has instalado tanto el Puente de dispositivo de escritorio como la aplicación de Win32 clásica, ten en cuenta que la aplicación del Puente de dispositivo de escritorio sustituirá a la aplicación de Win32 clásica al controlar las activaciones de notificación del sistema. Esto significa que las notificaciones del sistema desde la aplicación de Win32 clásica todavía iniciarán la aplicación del Puente de dispositivo de escritorio al hacer clic en ella. La desinstalación de la aplicación del Puente de dispositivo de escritorio revertirá activaciones de nuevo a la aplicación de Win32 clásica.


## <a name="known-issues"></a>Problemas conocidos

**CORREGIDO: Las aplicación no obtiene el foco tras hacer clic en la notificación del sistema**: en la compilación 15063 y anteriores, los derechos de primer plano no se transfirieron a tu aplicación cuando activamos el servidor COM. Por tanto, tu aplicación simplemente parpadeará al intentar moverla al primer plano. No había ninguna solución para este problema. Corregimos esto en las compilaciones 16299 y posteriores.


## <a name="resources"></a>Recursos

* [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/desktop-toasts)
* [Notificaciones del sistema desde aplicaciones de escritorio](toast-desktop-apps.md)
* [Documentación del contenido de la notificación del sistema](adaptive-interactive-toasts.md)

