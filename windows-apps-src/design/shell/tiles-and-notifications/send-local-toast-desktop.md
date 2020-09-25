---
Description: Obtenga información sobre cómo las aplicaciones de C# de Win32 pueden enviar notificaciones del sistema local y controlar el usuario que hace clic en la notificación del sistema.
title: Enviar una notificaciones del sistema local desde aplicaciones de C# de escritorio
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from desktop C# apps
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: 'Windows 10, UWP, Win32, escritorio, notificaciones del sistema, enviar una notificación del sistema, enviar un sistema local, un puente de escritorio, msix, paquetes dispersos, C#, C Sharp, notificación del sistema, WPF, enviar notificaciones del sistema WPF, enviar notificaciones del sistema, notificación del sistema #'
ms.localizationpriority: medium
ms.openlocfilehash: 9f4f78d689352f0278f814a2e89db6f92df52b99
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220128"
---
# <a name="send-a-local-toast-notification-from-desktop-c-apps"></a>Enviar una notificaciones del sistema local desde aplicaciones de C# de escritorio

Las aplicaciones de escritorio (incluidas las aplicaciones empaquetadas de [MSIX](/windows/msix/desktop/source-code-overview) , las aplicaciones que usan [paquetes dispersos](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) para obtener la identidad del paquete y las aplicaciones Win32 clásicas no empaquetadas) pueden enviar notificaciones del sistema interactivas como aplicaciones de Windows. Sin embargo, hay algunos pasos especiales para las aplicaciones de escritorio debido a los diferentes esquemas de activación y a la falta de identidad del paquete si no está utilizando paquetes MSIX o dispersos.

> [!IMPORTANT]
> Si va a escribir una aplicación para UWP, consulte la [documentación de UWP](send-local-toast.md). En el caso de otros idiomas del escritorio, consulte [escritorio de C++ WRL](send-local-toast-desktop-cpp-wrl.md).


## <a name="step-1-install-the-notifications-library"></a>Paso 1: instalación de la biblioteca de notificaciones

Instale el `Microsoft.Toolkit.Uwp.Notifications` [paquete NuGet](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) en el proyecto.

Esta [biblioteca de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) agrega código de biblioteca compatible para trabajar con notificaciones del sistema de aplicaciones de escritorio. También hace referencia a los SDK de UWP y permite construir notificaciones mediante C# en lugar de XML sin formato. El resto de esta guía de inicio rápido depende de la biblioteca de notificaciones.


## <a name="step-2-implement-the-activator"></a>Paso 2: implementar el activador

Debe implementar un controlador para la activación del sistema, de modo que cuando el usuario haga clic en la notificación del sistema, la aplicación pueda hacer algo. Esto es necesario para que las notificaciones del sistema se conserven en el centro de actividades (dado que se puede hacer clic en los días después de cerrar la aplicación). Esta clase se puede colocar en cualquier parte del proyecto.

Cree una nueva clase **MyNotificationActivator** y extienda la clase **NotificationActivator** . Agregue los tres atributos que se enumeran a continuación y cree un GUID único para la aplicación mediante uno de los muchos generadores de GUID en línea. Este CLSID (identificador de clase) es la forma en que el centro de actividades sabe qué clase se debe activar a través de COM.

**MyNotificationActivator.CS** (crear este archivo)

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


## <a name="step-3-register-with-notification-platform"></a>Paso 3: registro con la plataforma de notificación

A continuación, debe registrarse con la plataforma de notificación. Hay pasos diferentes en función de si usa paquetes MSIX/dispersos o Win32 clásico. Si admite ambos, debe realizar ambos pasos (sin embargo, no es necesario bifurcar el código, nuestra biblioteca lo controla automáticamente).


#### <a name="msixsparse-packages"></a>[Paquetes MSIX/dispersos](#tab/msix-sparse)

Si usa un paquete [MSIX](/windows/msix/desktop/source-code-overview) o [disperso](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) (o si admite ambos), en el **paquete. appxmanifest**, agregue:

1. Declaración de **xmlns: com**
2. Declaración de **xmlns: Desktop**
3. En el atributo **IgnorableNamespaces** , **com** y **Desktop**
4. **com: extensión** para el activador com con el GUID del paso #2. No olvide incluir el `Arguments="-ToastActivated"` para saber que el lanzamiento proviene de una notificación del sistema
5. **Desktop: Extension** para **Windows. toastNotificationActivation** para declarar el CLSID del activador del sistema (el GUID del paso #2).

**Package.appxmanifest**

```xml
<!--Add these namespaces-->
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


#### <a name="classic-win32"></a>[Win32 clásico](#tab/classic)

Si usa Win32 clásico (o si admite ambos), tendrá que declarar el identificador de modelo de usuario de la aplicación (AUMID) y el activador de notificaciones del sistema (GUID del paso #2) en el acceso directo de la aplicación en Inicio.

Seleccione un AUMID único que identifique la aplicación Win32. Normalmente tiene el formato [CompanyName]. [AppName], pero desea asegurarse de que es único en todas las aplicaciones (no dude en agregar algunos dígitos al final).

### <a name="step-31-wix-installer"></a>Paso 3,1: instalador de WiX

Si usa WiX para el instalador, edite el archivo **product. WXS** para agregar las dos propiedades de acceso directo al acceso directo del menú Inicio, tal como se muestra a continuación. Asegúrese de que el GUID del paso #2 se `{}` incluye como se muestra a continuación.

**Producto. WXS**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> Para usar las notificaciones realmente, debe instalar la aplicación a través del instalador una vez antes de la depuración, para que el acceso directo de inicio con el AUMID y el CLSID estén presentes. Después de que el acceso directo de inicio esté presente, puede depurar con F5 desde Visual Studio.


### <a name="step-32-register-aumid-and-com-server"></a>Paso 3,2: registrar el servidor AUMID y COM

A continuación, independientemente del instalador, en el código de inicio de la aplicación (antes de llamar a las API de notificación), llame al método **RegisterAumidAndComServer** , especificando la clase de activador de notificaciones del paso #2 y el AUMID usado anteriormente.

```csharp
// Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
```

Si es compatible con el paquete MSIX o disperso y con Win32 clásico, puede llamar a este método sin importar. Si está ejecutando en un paquete MSIX/disperso, este método simplemente volverá inmediatamente. No es necesario bifurcar el código.

Este método le permite llamar a las API de compatibilidad para enviar y administrar notificaciones sin tener que proporcionar constantemente su AUMID. Y inserta la clave del registro LocalServer32 para el servidor COM.

---


## <a name="step-4-register-com-activator"></a>Paso 4: registrar el activador COM

En el caso de los paquetes MSIX y dispersos y de las aplicaciones Win32 clásicas, debe registrar el tipo de activador de notificaciones para que pueda controlar las activaciones del sistema.

En el código de inicio de la aplicación, llame al método **RegisterActivator** siguiente, pasando su implementación de la clase **NotificationActivator** que creó en el paso #2. Se debe llamar a esta para que reciba cualquier activación del sistema.

```csharp
// Register COM server and activator type
DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();
```


## <a name="step-5-send-a-notification"></a>Paso 5: envío de una notificación

El envío de una notificación es idéntico a las aplicaciones para UWP, salvo que usará la clase **DesktopNotificationManagerCompat** para crear un **ToastNotifier**. La biblioteca de compatibilidad controla automáticamente la diferencia entre el paquete MSIX/disperso y el modo Win32 clásico, por lo que no tiene que bifurcar el código. En el caso de Win32 clásico, la biblioteca de compatibilidad almacena en caché el AUMID que proporcionó al llamar a **RegisterAumidAndComServer** para que no tenga que preocuparse de Cuándo proporcionar o no el AUMID.

> [!NOTE]
> Instale la [biblioteca de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para que pueda construir notificaciones mediante C#, tal y como se muestra a continuación, en lugar de usar XML sin formato.

Asegúrese de usar el **ToastContent** que se muestra a continuación (o la plantilla ToastGeneric si va a diseñar XML manualmente), ya que las plantillas de notificación del sistema de Windows 8.1 heredadas no activarán el activador de notificación com que creó en el paso #2.

> [!IMPORTANT]
> Las imágenes http solo se admiten en aplicaciones de paquetes MSIX/dispersos que tienen la capacidad de Internet en su manifiesto. Las aplicaciones Win32 clásicas no admiten imágenes http; debe descargar la imagen en los datos de la aplicación local y hacer referencia a ella localmente.

```csharp
// Construct the visuals of the toast (using Notifications library)
ToastContent toastContent = new ToastContentBuilder()
    .AddToastActivationInfo("action=viewConversation&conversationId=5", ToastActivationType.Foreground)
    .AddText("Hello world!")
    .GetToastContent();

// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());

// And then show it
DesktopNotificationManagerCompat.CreateToastNotifier().Show(toast);
```

> [!IMPORTANT]
> Las aplicaciones Win32 clásicas no pueden usar plantillas de notificación de sistema heredadas (como ToastText02). Se producirá un error en la activación de las plantillas heredadas cuando se especifique el CLSID COM. Debe usar las plantillas de ToastGeneric de Windows 10 como se ha indicado anteriormente.


## <a name="step-6-handling-activation"></a>Paso 6: control de la activación

Cuando el usuario hace clic en la notificación del sistema, se invoca el método **onactivated** de la clase **NotificationActivator** .

Dentro del método Onactivated, puede analizar los argumentos que especificó en el sistema y obtener la entrada del usuario que el usuario ha escrito o seleccionado y, a continuación, activar la aplicación en consecuencia.

> [!NOTE]
> No se llama al método **onactivated** en el subproceso de la interfaz de usuario. Si desea realizar operaciones de subproceso de interfaz de usuario, debe llamar a `Application.Current.Dispatcher.Invoke(callback)` .

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

Para permitir que se inicie correctamente mientras se cierra la aplicación, en el `App.xaml.cs` archivo, querrá invalidar el método de **Inicio** (para aplicaciones de WPF) para determinar si se está iniciando desde una notificación del sistema o no. Si se inicia desde una notificación del sistema, habrá un argumento Launch de "-ToastActivated". Cuando lo vea, debe dejar de realizar cualquier código de activación de inicio normal y permitir que el control de código **activado** se inicie.

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

En WPF, la secuencia de activación es la siguiente...

Si la aplicación ya se está ejecutando:

1. Se llama a **onactivated** en el **NotificationActivator**

Si la aplicación no se está ejecutando:

1. Se llama a **alstartup** en `App.xaml.cs` con **args** de "-ToastActivated"
2. Se llama a **onactivated** en el **NotificationActivator**


### <a name="foreground-vs-background-activation"></a>Activación en segundo plano y en segundo plano
En el caso de las aplicaciones de escritorio, la activación en primer plano y en segundo plano se administra de forma idéntica: se llama al activador de COM. Depende del código de la aplicación decidir si mostrar una ventana o simplemente realizar algún trabajo y, a continuación, salir. Por lo tanto, la especificación de un **ActivationType** de **fondo** en el contenido del sistema no cambia el comportamiento.


## <a name="step-7-remove-and-manage-notifications"></a>Paso 7: eliminación y administración de notificaciones

Quitar y administrar notificaciones es idéntico a las aplicaciones para UWP. Sin embargo, se recomienda usar nuestra biblioteca de compatibilidad para obtener un **DesktopNotificationHistoryCompat** , por lo que no tiene que preocuparse por proporcionar el AUMID si usa Win32 clásico.

```csharp
// Remove the toast with tag "Message2"
DesktopNotificationManagerCompat.History.Remove("Message2");

// Clear all toasts
DesktopNotificationManagerCompat.History.Clear();
```


## <a name="step-8-deploying-and-debugging"></a>Paso 8: implementación y depuración

Para implementar y depurar la aplicación MSIX, consulte [Ejecutar, depurar y probar una aplicación de escritorio empaquetada](/windows/msix/desktop/desktop-to-uwp-debug).

Para implementar y depurar la aplicación clásica de Win32, debe instalar la aplicación a través del instalador una vez antes de la depuración, para que el acceso directo de inicio con el AUMID y el CLSID estén presentes. Después de que el acceso directo de inicio esté presente, puede depurar con F5 desde Visual Studio.

Si las notificaciones simplemente no aparecen en la aplicación de Win32 clásica (y no se inicia ninguna excepción), lo más probable es que el acceso directo de inicio no esté presente (Instale la aplicación mediante el instalador) o que el AUMID que usó en el código no coincida con AUMID en el acceso directo de inicio.

Si las notificaciones aparecen pero no se conservan en el centro de actividades (desaparecen después de descartar el elemento emergente), significa que no ha implementado correctamente el activador de COM.

Si ha instalado el paquete MSIX o disperso y la aplicación de Win32 clásica, tenga en cuenta que la aplicación de paquete MSIX/Sparse sustituirá a la aplicación de Win32 clásica al controlar las activaciones del sistema. Esto significa que las notificaciones del sistema de la aplicación Win32 clásica seguirán iniciando la aplicación del paquete MSIX/Sparse cuando se haga clic en ella. Al desinstalar la aplicación de paquete MSIX/Sparse, se revertirán las activaciones de nuevo a la aplicación de Win32 clásica.


## <a name="known-issues"></a>Problemas conocidos

Problema **corregido: la aplicación no se centra después de hacer clic**en la notificación del sistema: en las compilaciones 15063 y anteriores, los derechos de primer plano no se transferían a la aplicación cuando se activa el servidor com. Por lo tanto, la aplicación simplemente parpadearía al intentar moverla a primer plano. No había ninguna solución para este problema. Este problema se ha corregido en las compilaciones 16299 y posteriores.


## <a name="resources"></a>Recursos

* [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/desktop-toasts)
* [Notificaciones del sistema de aplicaciones de escritorio](toast-desktop-apps.md)
* [Documentación del contenido del sistema](adaptive-interactive-toasts.md)
