---
author: andrewleader
Description: Learn how Win32 C++ WRL apps can send local toast notifications and handle the user clicking the toast.
title: Enviar una notificación del sistema local desde aplicaciones de C++ (WRL) de escritorio
label: Send a local toast notification from desktop C++ WRL apps
template: detail.hbs
ms.author: mijacobs
ms.date: 03/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, win32, escritorio, notificaciones del sistema, enviar una notificación del sistema, enviar notificación del sistema local, puente de dispositivo de escritorio, C++, cpp, cplusplus, WRL
ms.localizationpriority: medium
ms.openlocfilehash: a6134e65a27563f96c03880dea026bed11f46641
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "4963148"
---
# <a name="send-a-local-toast-notification-from-desktop-c-wrl-apps"></a>Enviar una notificación del sistema local desde aplicaciones de C++ (WRL) de escritorio

Las aplicaciones de escritorio (Puente de dispositivo de escritorio y Win32 clásico) pueden enviar notificaciones del sistema interactivas igual de la misma manera que las aplicaciones de la Plataforma universal de Windows (UWP). Sin embargo, hay algunos pasos especiales para aplicaciones de escritorio debido a los diferentes esquemas de activación y a la posible falta de identidad del paquete si no usas el Puente de dispositivo de escritorio.

> [!IMPORTANT]
> Si estás escribiendo una aplicación para UWP, consulta la [documentación de UWP](send-local-toast.md). Para otros lenguajes de escritorio, consulta [C++ de escritorio](send-local-toast-desktop.md).


## <a name="step-1-enable-the-windows-10-sdk"></a>Paso 1: Habilitar el SDK de Windows 10

Si no has habilitado el SDK de Windows 10 para tu aplicación de Win32, debes hacerlo primero. Hay unos pocos pasos clave...

1. Agrega `runtimeobject.lib` a **Dependencias adicionales**.
2. Establecer como destino el nuevo SDK de Windows 10

Haz clic con el botón derecho en el proyecto y selecciona **Propiedades**.

En la menú de **Configuración** superior, selecciona **Todas las configuraciones** para que se aplique el cambio siguiente tanto para Depuración como para Lanzamiento.

En **Enlazador-> Entrada**, agrega `runtimeobject.lib` a las **Dependencias adicionales**.

Luego, en **General**, debes asegurarte de que la **versión de Windows SDK** está establecida en algo como 10.0 o posterior (no Windows 8.1).


## <a name="step-2-copy-compat-library-code"></a>Paso 2: Copiar código de la biblioteca compat

Copia los archivos [DesktopNotificationManagerCompat.h](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.h) y [DesktopNotificationManagerCompat.cpp](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.cpp) desde GitHub en tu proyecto. La biblioteca compat toma buena parte de la complejidad de las notificaciones de escritorio. Las siguientes instrucciones requieren la biblioteca compat.

Si estás usando encabezados precompilados, asegúrate de `#include "stdafx.h"` como la primera línea del archivo DesktopNotificationManagerCompat.cpp.


## <a name="step-3-include-the-header-files-and-namespaces"></a>Paso 3: Incluir los espacios de nombres y los archivos de encabezado

Incluye el archivo de encabezado de la biblioteca de compatibilidad y los espacios de nombres y los archivos de encabezado relacionados con el uso de las API de notificación del sistema para UWP.

```cpp
#include "DesktopNotificationManagerCompat.h"
#include "NotificationActivationCallback.h"
#include <windows.ui.notifications.h>

using namespace ABI::Windows::Data::Xml::Dom;
using namespace ABI::Windows::UI::Notifications;
using namespace Microsoft::WRL;
```


## <a name="step-4-implement-the-activator"></a>Paso 4: Implementar el activador

Debes implementar un controlador para la activación de la notificación del sistema de modo que, cuando el usuario haga clic en la notificación del sistema, la aplicación pueda hacer algo. Esto es necesario para que la notificación del sistema se conserve en el Centro de actividades (ya que se podría hacer clic en la notificación del sistema cuando se cierre la aplicación). Esta clase se puede colocar en cualquier lugar de tu proyecto.

Implementa la interfaz de **INotificationActivationCallback** como se muestra a continuación, incluido un UUID y también llama a **CoCreatableClass** para marcar la clase como que se puede crear con COM. Para el UUID, crea un GUID único mediante uno de los muchos generadores GUID en línea. Este CLSID (identificador de clase) de GUID es la manera a través de la cual el Centro de actividades sabe qué clase de COM activar.

```cpp
// The UUID CLSID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public:
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        // TODO: Handle activation
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
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

```cpp
// Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"YourCompany.YourApp", __uuidof(NotificationActivator));
```

Si admites tanto el Puente de dispositivo de escritorio como Win32 clásico, puedes llamar a este método. Si estás ejecutando en el Puente de dispositivo de escritorio, este método simplemente regresará inmediatamente. No es necesario que bifurques el código.

Este método te permite llamar a las API de compatibilidad para enviar y administrar notificaciones sin tener que proporcionar constantemente tu AUMID. E inserta la clave del Registro de LocalServer32 para el servidor COM.


## <a name="step-6-register-com-activator"></a>Paso 6: Registrar el activador COM

Tanto para el Puente de dispositivo de escritorio como para las aplicaciones de Win32 clásicas, debes registrar tu tipo de activador de notificación, por lo que puedes controlar las activaciones de notificación del sistema.

En el código de inicio de la aplicación, llama al siguiente método **RegisterActivator**. Debes llamar a él para que puedas recibir las activaciones de notificación del sistema.

```cpp
// Register activator type
hr = DesktopNotificationManagerCompat::RegisterActivator();
```


## <a name="step-7-send-a-notification"></a>Paso 7: Enviar una notificación

La acción de enviar una notificación es idéntica a aplicaciones para UWP, excepto en que usarás **DesktopNotificationManagerCompat** para crear un **ToastNotifier**. La biblioteca de compatibilidad controla automáticamente la diferencia entre el Puente de dispositivo de escritorio y Win32 clásico para que no tengas que bifurcar el código. Para Win32 clásico, la biblioteca de compatibilidad almacena en caché el AUMID que proporcionaste al llamar a **RegisterAumidAndComServer** para que no tengas que preocuparte de cuándo debes proporcionar o no el AUMID.

Asegúrate de usar el enlace **ToastGeneric** como se muestra a continuación puesto que las plantillas de notificación del sistema de Windows 8.1 heredadas no activarán el activador de notificaciones COM que creaste en el paso n.4.

> [!IMPORTANT]
> Las imágenes HTTP solo se admiten en las aplicaciones del Puente de dispositivo de escritorio que tienen la funcionalidad de Internet en su manifiesto. Las aplicaciones de Win32 clásicas no admiten imágenes http; debes descargar la imagen en los datos locales de la aplicación y hacer referencia a ellos de manera local.

```cpp
// Construct XML
ComPtr<IXmlDocument> doc;
hr = DesktopNotificationManagerCompat::CreateXmlDocumentFromString(
    L"<toast><visual><binding template='ToastGeneric'><text>Hello world</text></binding></visual></toast>",
    &doc);
if (SUCCEEDED(hr))
{
    // See full code sample to learn how to inject dynamic text, buttons, and more

    // Create the notifier
    // Classic Win32 apps MUST use the compat method to create the notifier
    ComPtr<IToastNotifier> notifier;
    hr = DesktopNotificationManagerCompat::CreateToastNotifier(&notifier);
    if (SUCCEEDED(hr))
    {
        // Create the notification itself (using helper method from compat library)
        ComPtr<IToastNotification> toast;
        hr = DesktopNotificationManagerCompat::CreateToastNotification(doc, &toast);
        if (SUCCEEDED(hr))
        {
            // And show it!
            hr = notifier->Show(toast.Get());
        }
    }
}
```

> [!IMPORTANT]
> Las aplicaciones de Win32 clásicas no pueden usar plantillas de notificación del sistema heredadas (como ToastText02). Se producirá un error en la activación de las plantillas heredadas cuando se especifique el CLSID COM. Debes usar las plantillas ToastGeneric de Windows 10 como se muestra arriba.


## <a name="step-8-handling-activation"></a>Paso 8: Control de la activación

Cuando el usuario hace clic en tu notificación del sistema o en los botones de la notificación del sistema, se invoca el método **Activar** de tu clase **NotificationActivator**.

En el método Activar, puedes analizar los argumentos que especificaste en la notificación del sistema y obtener la entrada del usuario que el usuario ha escrito o seleccionado y, a continuación, activar la aplicación según corresponda.

> [!NOTE]
> Se llama al método **Activar** en un subproceso independiente del subproceso principal.

```cpp
// The GUID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public: 
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        std::wstring arguments(invokedArgs);
        HRESULT hr = S_OK;

        // Background: Quick reply to the conversation
        if (arguments.find(L"action=reply") == 0)
        {
            // Get the response user typed.
            // We know this is first and only user input since our toasts only have one input
            LPCWSTR response = data[0].Value;

            hr = DesktopToastsApp::SendResponse(response);
        }

        else
        {
            // The remaining scenarios are foreground activations,
            // so we first make sure we have a window open and in foreground
            hr = DesktopToastsApp::GetInstance()->OpenWindowIfNeeded();
            if (SUCCEEDED(hr))
            {
                // Open the image
                if (arguments.find(L"action=viewImage") == 0)
                {
                    hr = DesktopToastsApp::GetInstance()->OpenImage();
                }

                // Open the app itself
                // User might have clicked on app title in Action Center which launches with empty args
                else
                {
                    // Nothing to do, already launched
                }
            }
        }

        if (FAILED(hr))
        {
            // Log failed HRESULT
        }

        return S_OK;
    }

    ~NotificationActivator()
    {
        // If we don't have window open
        if (!DesktopToastsApp::GetInstance()->HasWindow())
        {
            // Exit (this is for background activation scenarios)
            exit(0);
        }
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
```

Para admitir correctamente el inicio mientras se cierra la aplicación, en la función WinMain, querrás determinar si estás iniciando desde una notificación del sistema o no. Si se inicia desde una notificación del sistema, habrá un argumento de inicio de "-ToastActivated". Cuando veas este, debes dejar de ejecutar cualquier código de activación de inicio normal y permitir que tu **NotificationActivator** controle el inicio de ventanas, si es necesario.

```cpp
// Main function
int WINAPI wWinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE, _In_ LPWSTR cmdLineArgs, _In_ int)
{
    RoInitializeWrapper winRtInitializer(RO_INIT_MULTITHREADED);

    HRESULT hr = winRtInitializer;
    if (SUCCEEDED(hr))
    {
        // Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
        hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"WindowsNotifications.DesktopToastsCpp", __uuidof(NotificationActivator));
        if (SUCCEEDED(hr))
        {
            // Register activator type
            hr = DesktopNotificationManagerCompat::RegisterActivator();
            if (SUCCEEDED(hr))
            {
                DesktopToastsApp app;
                app.SetHInstance(hInstance);

                std::wstring cmdLineArgsStr(cmdLineArgs);

                // If launched from toast
                if (cmdLineArgsStr.find(TOAST_ACTIVATED_LAUNCH_ARG) != std::string::npos)
                {
                    // Let our NotificationActivator handle activation
                }

                else
                {
                    // Otherwise launch like normal
                    app.Initialize(hInstance);
                }

                app.RunMessageLoop();
            }
        }
    }

    return SUCCEEDED(hr);
}
```


### <a name="activation-sequence-of-events"></a>Secuencia de activación de eventos

La secuencia de activación es la siguiente...

Si tu aplicación ya se está ejecutando:

1. Se llama a **Activar** en tu **NotificationActivator**.

Si tu aplicación no se está ejecutando:

1. Tu aplicación se inicia mediante EXE, obtienes argumentos de línea de comandos de "-ToastActivated".
2. Se llama a **Activar** en tu **NotificationActivator**.


### <a name="foreground-vs-background-activation"></a>Activación en primer plano frente a activación en segundo plano
Para aplicaciones de escritorio, la activación en primer plano y en segundo plano se controla de forma idéntica: se llama a tu activador de COM. Depende del código de tu aplicación decidir si se mostrará una ventana o simplemente se realizará algún trabajo y después se saldrá. Por lo tanto, al especificar un **activationType** **en segundo plano** en el contenido de la notificación del sistema no se cambia el comportamiento.


## <a name="step-9-remove-and-manage-notifications"></a>Paso 9: Quitar y administrar notificaciones

El proceso de quitar y administrar notificaciones es idéntico a las aplicaciones para UWP. Sin embargo, se recomienda usar nuestra biblioteca de compatibilidad para obtener un valor de **DesktopNotificationHistoryCompat** de manera que no tenga que preocuparse de proporcionar el AUMID si estás usando Win32 clásico.

```cpp
std::unique_ptr<DesktopNotificationHistoryCompat> history;
auto hr = DesktopNotificationManagerCompat::get_History(&history);
if (SUCCEEDED(hr))
{
    // Remove a specific toast
    hr = history->Remove(L"Message2");

    // Clear all toasts
    hr = history->Clear();
}
```


## <a name="step-10-deploying-and-debugging"></a>Paso 10: Implementación y depuración

Para implementar y depurar tu aplicación del Puente de dispositivo de escritorio, consulta [Ejecutar, depurar y probar una aplicación de escritorio empaquetada](/porting/desktop-to-uwp-debug.md).

Para implementar y depurar tu aplicación de Win32 clásica, debes instalar tu aplicación mediante el instalador una vez antes de la depuración normalmente para que el acceso directo de Inicio con tus AUMID y CLSID estén presentes. Después de que el acceso directo de Inicio esté presente, puedes depurar con F5 desde Visual Studio.

Si las notificaciones simplemente no aparecen en tu aplicación de Win32 clásica (y no se generan excepciones), probablemente quiere decir que el acceso directo de Inicio no está presente (instala tu aplicación mediante el instalador) o el AUMID que usaste en el código no coincide con el AUMID del acceso directo de Inicio.

Si las notificaciones aparecen pero no se conservan en el Centro de actividades (desaparecen después de descartar el control emergente), eso significa que no has implementado el activador COM correctamente.

Si has instalado tanto el Puente de dispositivo de escritorio como la aplicación de Win32 clásica, ten en cuenta que la aplicación del Puente de dispositivo de escritorio sustituirá a la aplicación de Win32 clásica al controlar las activaciones de notificación del sistema. Esto significa que las notificaciones del sistema desde la aplicación de Win32 clásica todavía iniciarán la aplicación del Puente de dispositivo de escritorio al hacer clic en ella. La desinstalación de la aplicación del Puente de dispositivo de escritorio revertirá activaciones de nuevo a la aplicación de Win32 clásica.

Si recibes `HRESULT 0x800401f0 CoInitialize has not been called.`, asegúrate de llamar a `CoInitialize(nullptr)` en tu aplicación antes de llamar a las API.

Si recibes `HRESULT 0x8000000e A method was called at an unexpected time.` al llamar a las API de compatibilidad, eso significa que probablemente no pudiste llamar a los métodos de registro necesarios (o si es una aplicación del Puente de dispositivo de escritorio, no estás ejecutando actualmente la aplicación en el contexto del Puente de dispositivo de escritorio).

Si recibes muchos errores de compilación de `unresolved external symbol`, es probable que hayas olvidado agregar `runtimeobject.lib` a las **Dependencias adicionales** del paso n.º1 (o solo lo agregaste a la configuración de depuración y no a la configuración de lanzamiento).


## <a name="handling-older-versions-of-windows"></a>Control de versiones anteriores de Windows

Si admite Windows 8.1 o una versión inferior, querrás comprobar en tiempo de ejecución si estás ejecutando en Windows 10 antes de llamar a cualquier API de **DesktopNotificationManagerCompat** o enviar notificaciones del sistema ToastGeneric.

Windows 8 introdujo notificaciones del sistema, pero usó las [plantillas de notificación del sistema heredadas](https://docs.microsoft.com/en-us/previous-versions/windows/apps/hh761494(v=win.10)), como ToastText01. La activación se controlaba mediante el evento **Activated** en la memoria de la clase **ToastNotification** puesto que las notificaciones del sistema eran solo elementos emergentes breves que no se conservaban. Windows 10 introdujo [notificaciones del sistema ToastGeneric interactivas](adaptive-interactive-toasts.md) y también introdujo el Centro de actividades donde las notificaciones se conservan durante varios días. La introducción del Centro de actividades requirió la introducción de un activador COM, para que la notificación del sistema se pueda activar días después de crearla.

| Sistema operativo | ToastGeneric | Activador COM | Plantillas de notificación del sistema heredadas |
| -- | ------------ | ------------- | ---------------------- |
| Windows 10 | Compatible | Compatible | Compatible (pero no activará el servidor COM) |
| Windows 8.1/8 | N/D | N/D | Compatible |
| Windows 7 y versiones inferiores | N/D | N/C | N/D |

Para comprobar si estás ejecutando en Windows 10, incluye el encabezado `<VersionHelpers.h>` y comprueba el método **IsWindows10OrGreater**. Si esto devuelve true, continúa llamando a todos los métodos descritos en esta documentación. 

```cpp
#include <VersionHelpers.h>

if (IsWindows10OrGreater())
{
    // Running Windows 10, continue with sending Windows 10 toasts!
}
```


## <a name="known-issues"></a>Problemas conocidos

**CORREGIDO: Las aplicación no obtiene el foco tras hacer clic en la notificación del sistema**: en la compilación 15063 y anteriores, los derechos de primer plano no se transfirieron a tu aplicación cuando activamos el servidor COM. Por tanto, tu aplicación simplemente parpadeará al intentar moverla al primer plano. No había ninguna solución para este problema. Corregimos esto en las compilaciones 16299 y posteriores.


## <a name="resources"></a>Recursos

* [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/desktop-toasts)
* [Notificaciones del sistema desde aplicaciones de escritorio](toast-desktop-apps.md)
* [Documentación del contenido de la notificación del sistema](adaptive-interactive-toasts.md)