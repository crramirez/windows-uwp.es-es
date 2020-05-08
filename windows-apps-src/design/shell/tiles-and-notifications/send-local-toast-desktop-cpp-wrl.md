---
Description: Obtenga información sobre cómo las aplicaciones de la WRL de Win32 C++ pueden enviar notificaciones del sistema local y controlar el usuario al hacer clic en la notificación del sistema.
title: Enviar una notificación del sistema local desde las aplicaciones de la WRL de C++ de escritorio
label: Send a local toast notification from desktop C++ WRL apps
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: Windows 10, UWP, Win32, escritorio, notificaciones del sistema, enviar una notificación del sistema, enviar notificaciones locales, puente de escritorio, msix, paquete disperso, C++, CPP, CPlusPlus, WRL
ms.localizationpriority: medium
ms.openlocfilehash: cc87f9281b9623c1f1b46def8f886cfebeb0438f
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968300"
---
# <a name="send-a-local-toast-notification-from-desktop-c-wrl-apps"></a>Enviar una notificación del sistema local desde las aplicaciones de la WRL de C++ de escritorio

Las aplicaciones de escritorio (incluidas las aplicaciones empaquetadas de [MSIX](https://docs.microsoft.com/windows/msix/desktop/source-code-overview) , las aplicaciones que usan [paquetes dispersos](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) para obtener la identidad del paquete y las aplicaciones Win32 clásicas no empaquetadas) pueden enviar notificaciones del sistema interactivas, al igual que las aplicaciones de aplicaciones de Windows. Sin embargo, hay algunos pasos especiales para las aplicaciones de escritorio debido a los diferentes esquemas de activación y a la falta de identidad del paquete si no está usando MSIX o un paquete disperso.

> [!IMPORTANT]
> Si va a escribir una aplicación para UWP, consulte la [documentación de UWP](send-local-toast.md). En el caso de otros idiomas del escritorio, vea [escritorio C#](send-local-toast-desktop.md).


## <a name="step-1-enable-the-windows-10-sdk"></a>Paso 1: habilitar el SDK de Windows 10

Si no ha habilitado el SDK de Windows 10 para su aplicación Win32, debe hacerlo primero. Hay algunos pasos clave...

1. Agregar `runtimeobject.lib` a **dependencias adicionales**
2. Usar el SDK de Windows 10 como destino

Haga clic con el botón derecho en el proyecto y seleccione **propiedades**.

En el menú de **configuración** superior, seleccione **todas las configuraciones** para que se aplique el siguiente cambio tanto a la depuración como a la versión.

En enlazador **: > entrada**, agregue `runtimeobject.lib` las **dependencias adicionales**.

En **General**, asegúrese de que la **versión de Windows SDK** está establecida en algo 10,0 o superior (no Windows 8.1).


## <a name="step-2-copy-compat-library-code"></a>Paso 2: copiar el código de la biblioteca de compatibilidad

Copie el archivo [DesktopNotificationManagerCompat. h](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.h) y [DesktopNotificationManagerCompat. cpp](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.cpp) de github en el proyecto. La biblioteca de compatibilidad abstrae gran parte de la complejidad de las notificaciones del escritorio. Las instrucciones siguientes requieren la biblioteca de compatibilidad.

Si usa encabezados precompilados, `#include "stdafx.h"` Asegúrese de que es la primera línea del archivo DesktopNotificationManagerCompat. cpp.


## <a name="step-3-include-the-header-files-and-namespaces"></a>Paso 3: incluir los archivos de encabezado y los espacios de nombres

Incluya el archivo de encabezado de la biblioteca de compatibilidad y los archivos de encabezado y espacios de nombres relacionados con el uso de las API del sistema de Windows.

```cpp
#include "DesktopNotificationManagerCompat.h"
#include <NotificationActivationCallback.h>
#include <windows.ui.notifications.h>

using namespace ABI::Windows::Data::Xml::Dom;
using namespace ABI::Windows::UI::Notifications;
using namespace Microsoft::WRL;
```


## <a name="step-4-implement-the-activator"></a>Paso 4: implementar el activador

Debe implementar un controlador para la activación del sistema, de modo que cuando el usuario haga clic en la notificación del sistema, la aplicación pueda hacer algo. Esto es necesario para que las notificaciones del sistema se conserven en el centro de actividades (dado que se puede hacer clic en los días después de cerrar la aplicación). Esta clase se puede colocar en cualquier parte del proyecto.

Implemente la interfaz **INotificationActivationCallback** como se muestra a continuación, incluido un UUID, y llame a **CoCreatableClass** para marcar la clase como com que se puede crear. Para el UUID, cree un GUID único con uno de los muchos generadores de GUID en línea. Este GUID CLSID (identificador de clase) es la forma en que el centro de actividades sabe qué clase se debe activar a través de COM.

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


## <a name="step-5-register-with-notification-platform"></a>Paso 5: registro con la plataforma de notificación

A continuación, debe registrarse con la plataforma de notificación. Hay pasos diferentes en función de si usa paquetes MSIX/dispersos o Win32 clásico. Si admite ambos, debe realizar ambos pasos (sin embargo, no es necesario bifurcar el código, nuestra biblioteca lo controla automáticamente).


### <a name="msixsparse-package"></a>MSIX/paquete disperso

Si usa [MSIX](https://docs.microsoft.com/windows/msix/desktop/source-code-overview) o un [paquete disperso](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) (o si admite ambos), en el **paquete. appxmanifest**, agregue:

1. Declaración de **xmlns: com**
2. Declaración de **xmlns: Desktop**
3. En el atributo **IgnorableNamespaces** , **com** y **Desktop**
4. **com: extensión** para el activador com con el GUID del paso #4. No olvide incluir el `Arguments="-ToastActivated"` para saber que el lanzamiento proviene de una notificación del sistema
5. **Desktop: Extension** para **Windows. toastNotificationActivation** para declarar el CLSID del activador del sistema (el GUID del paso #4).

**Paquete. appxmanifest**

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

Si usa Win32 clásico (o si admite ambos), tendrá que declarar el identificador de modelo de usuario de la aplicación (AUMID) y el activador de notificaciones del sistema (GUID del paso #4) en el acceso directo de la aplicación en Inicio.

Seleccione un AUMID único que identifique la aplicación Win32. Normalmente tiene el formato [CompanyName]. [AppName], pero desea asegurarse de que es único en todas las aplicaciones (no dude en agregar algunos dígitos al final).

#### <a name="step-51-wix-installer"></a>Paso 5,1: instalador de WiX

Si usa WiX para el instalador, edite el archivo **product. WXS** para agregar las dos propiedades de acceso directo al acceso directo del menú Inicio, tal como se muestra a continuación. Asegúrese de que el GUID del paso #4 se incluye `{}` como se muestra a continuación.

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


#### <a name="step-52-register-aumid-and-com-server"></a>Paso 5,2: registrar el servidor AUMID y COM

A continuación, independientemente del instalador, en el código de inicio de la aplicación (antes de llamar a las API de notificación), llame al método **RegisterAumidAndComServer** , especificando la clase de activador de notificaciones del paso #4 y el AUMID usado anteriormente.

```cpp
// Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"YourCompany.YourApp", __uuidof(NotificationActivator));
```

Si es compatible con el paquete MSIX o disperso y con Win32 clásico, puede llamar a este método sin importar. Si está ejecutando en un paquete MSIX o disperso, este método simplemente volverá inmediatamente. No es necesario bifurcar el código.

Este método le permite llamar a las API de compatibilidad para enviar y administrar notificaciones sin tener que proporcionar constantemente su AUMID. Y inserta la clave del registro LocalServer32 para el servidor COM.


## <a name="step-6-register-com-activator"></a>Paso 6: registrar el activador COM

En el caso de los paquetes MSIX y dispersos y de las aplicaciones Win32 clásicas, debe registrar el tipo de activador de notificaciones para que pueda controlar las activaciones del sistema.

En el código de inicio de la aplicación, llame al siguiente método **RegisterActivator** . Se debe llamar a esta para que reciba cualquier activación del sistema.

```cpp
// Register activator type
hr = DesktopNotificationManagerCompat::RegisterActivator();
```


## <a name="step-7-send-a-notification"></a>Paso 7: envío de una notificación

El envío de una notificación es idéntico a las aplicaciones para UWP, salvo que usará **DesktopNotificationManagerCompat** para crear un **ToastNotifier**. La biblioteca de compatibilidad controla automáticamente la diferencia entre el paquete MSIX/disperso y el modo Win32 clásico, por lo que no tiene que bifurcar el código. En el caso de Win32 clásico, la biblioteca de compatibilidad almacena en caché el AUMID que proporcionó al llamar a **RegisterAumidAndComServer** para que no tenga que preocuparse de Cuándo proporcionar o no el AUMID.

Asegúrese de usar el enlace **ToastGeneric** como se muestra a continuación, ya que las plantillas de notificación del sistema de Windows 8.1 heredadas no activarán el activador de notificación com que creó en el paso #4.

> [!IMPORTANT]
> Las imágenes http solo se admiten en aplicaciones de paquetes MSIX/dispersos que tienen la capacidad de Internet en su manifiesto. Las aplicaciones Win32 clásicas no admiten imágenes http; debe descargar la imagen en los datos de la aplicación local y hacer referencia a ella localmente.

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
        hr = DesktopNotificationManagerCompat::CreateToastNotification(doc.Get(), &toast);
        if (SUCCEEDED(hr))
        {
            // And show it!
            hr = notifier->Show(toast.Get());
        }
    }
}
```

> [!IMPORTANT]
> Las aplicaciones Win32 clásicas no pueden usar plantillas de notificación de sistema heredadas (como ToastText02). Se producirá un error en la activación de las plantillas heredadas cuando se especifique el CLSID COM. Debe usar las plantillas de ToastGeneric de Windows 10 como se ha indicado anteriormente.


## <a name="step-8-handling-activation"></a>Paso 8: control de la activación

Cuando el usuario hace clic en la notificación del sistema o en los botones de la notificación del sistema, se invoca el método **Activate** de la clase **NotificationActivator** .

Dentro del método activar, puede analizar los argumentos que especificó en el sistema y obtener la entrada del usuario que el usuario ha escrito o seleccionado y, a continuación, activar la aplicación en consecuencia.

> [!NOTE]
> Se llama al método **Activate** en un subproceso de separte desde el subproceso principal.

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

Para permitir que se inicie correctamente mientras se cierra la aplicación, en la función WinMain, querrá determinar si se está iniciando desde un sistema o no. Si se inicia desde una notificación del sistema, habrá un argumento Launch de "-ToastActivated". Cuando lo vea, debe dejar de realizar cualquier código de activación de inicio normal y permitir que el **NotificationActivator** controle el inicio de Windows, si es necesario.

```cpp
// Main function
int WINAPI wWinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE, _In_ LPWSTR cmdLineArgs, _In_ int)
{
    RoInitializeWrapper winRtInitializer(RO_INIT_MULTITHREADED);

    HRESULT hr = winRtInitializer;
    if (SUCCEEDED(hr))
    {
        // Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
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

Si la aplicación ya se está ejecutando:

1. Se llama a **Activar** en el **NotificationActivator**

Si la aplicación no se está ejecutando:

1. La aplicación se inicia con EXE, se obtiene un args de línea de comandos de "-ToastActivated".
2. Se llama a **Activar** en el **NotificationActivator**


### <a name="foreground-vs-background-activation"></a>Activación en segundo plano y en segundo plano
En el caso de las aplicaciones de escritorio, la activación en primer plano y en segundo plano se administra de forma idéntica: se llama al activador de COM. Depende del código de la aplicación decidir si mostrar una ventana o simplemente realizar algún trabajo y, a continuación, salir. Por lo tanto, la especificación de un **activationType** de **fondo** en el contenido del sistema no cambia el comportamiento.


## <a name="step-9-remove-and-manage-notifications"></a>Paso 9: eliminación y administración de notificaciones

Quitar y administrar notificaciones es idéntico a las aplicaciones para UWP. Sin embargo, se recomienda usar nuestra biblioteca de compatibilidad para obtener un **DesktopNotificationHistoryCompat** , por lo que no tiene que preocuparse por proporcionar el AUMID si usa Win32 clásico.

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


## <a name="step-10-deploying-and-debugging"></a>Paso 10: implementación y depuración

Para implementar y depurar la aplicación de paquete MSIX/Sparse, vea [Ejecutar, depurar y probar una aplicación de escritorio empaquetada](/windows/uwp/porting/desktop-to-uwp-debug).

Para implementar y depurar la aplicación clásica de Win32, debe instalar la aplicación a través del instalador una vez antes de la depuración, para que el acceso directo de inicio con el AUMID y el CLSID estén presentes. Después de que el acceso directo de inicio esté presente, puede depurar con F5 desde Visual Studio.

Si las notificaciones simplemente no aparecen en la aplicación de Win32 clásica (y no se inicia ninguna excepción), lo más probable es que el acceso directo de inicio no esté presente (Instale la aplicación mediante el instalador) o que el AUMID que usó en el código no coincida con AUMID en el acceso directo de inicio.

Si las notificaciones aparecen pero no se conservan en el centro de actividades (desaparecen después de descartar el elemento emergente), significa que no ha implementado correctamente el activador de COM.

Si ha instalado el paquete MSIX o disperso y la aplicación de Win32 clásica, tenga en cuenta que la aplicación de paquete MSIX/Sparse sustituirá a la aplicación de Win32 clásica al controlar las activaciones del sistema. Esto significa que las notificaciones del sistema de la aplicación Win32 clásica seguirán iniciando la aplicación del paquete MSIX/Sparse cuando se haga clic en ella. Al desinstalar la aplicación de paquete MSIX/Sparse, se revertirán las activaciones de nuevo a la aplicación de Win32 clásica.

Si recibe `HRESULT 0x800401f0 CoInitialize has not been called.`, asegúrese de llamar `CoInitialize(nullptr)` a en la aplicación antes de llamar a las API.

Si recibe `HRESULT 0x8000000e A method was called at an unexpected time.` mientras llama a las API de compatibilidad, lo más probable es que no pueda llamar a los métodos de registro necesarios (o bien, si una aplicación de paquete MSIX o disperso no está ejecutando actualmente la aplicación en el contexto MSIX/Sparse).

Si obtiene numerosos `unresolved external symbol` errores de compilación, probablemente olvidó agregar `runtimeobject.lib` las **dependencias adicionales** en el paso #1 (o solo lo agregó a la configuración de depuración y no a la configuración de versión).


## <a name="handling-older-versions-of-windows"></a>Administrar versiones anteriores de Windows

Si admite Windows 8.1 o inferior, querrá comprobar en tiempo de ejecución si está ejecutando en Windows 10 antes de llamar a cualquier API de **DesktopNotificationManagerCompat** o enviar cualquier notificación del sistema de ToastGeneric.

Windows 8 presentó notificaciones del sistema, pero usó las plantillas del sistema de notificaciones [heredadas](https://docs.microsoft.com/previous-versions/windows/apps/hh761494(v=win.10)), como ToastText01. La activación se controló mediante el evento **activado** en memoria en la clase **ToastNotification** , ya que las notificaciones del sistema solo eran elementos emergentes breves que no se guardaron. Windows 10 presentó notificaciones de [ToastGeneric interactivas](adaptive-interactive-toasts.md)y también incorporó el centro de actividades en el que las notificaciones se conservan durante varios días. La introducción del centro de actividades requirió la introducción de un activador COM, de modo que la notificación del sistema se pueda activar los días después de su creación.

| SO | ToastGeneric | Activador COM | Plantillas del sistema de notificaciones heredadas |
| -- | ------------ | ------------- | ---------------------- |
| Windows 10 | Compatible. | Compatible. | Compatible (pero no activará el servidor COM) |
| Windows 8.1/8 | N/A | N/D | Compatible |
| Windows 7 y versiones inferiores | N/A | N/A | N/D |

Para comprobar si se está ejecutando en Windows 10, incluya el `<VersionHelpers.h>` encabezado y compruebe el método **IsWindows10OrGreater** . Si devuelve true, continúe llamando a todos los métodos descritos en esta documentación. 

```cpp
#include <VersionHelpers.h>

if (IsWindows10OrGreater())
{
    // Running Windows 10, continue with sending Windows 10 toasts!
}
```


## <a name="known-issues"></a>Problemas conocidos

Problema **corregido: la aplicación no se centra después de hacer clic**en la notificación del sistema: en las compilaciones 15063 y anteriores, los derechos de primer plano no se transferían a la aplicación cuando se activa el servidor com. Por lo tanto, la aplicación simplemente parpadearía al intentar moverla a primer plano. No había ninguna solución para este problema. Este problema se ha corregido en las compilaciones 16299 y posteriores.


## <a name="resources"></a>Recursos

* [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/desktop-toasts)
* [Notificaciones del sistema de aplicaciones de escritorio](toast-desktop-apps.md)
* [Documentación del contenido del sistema](adaptive-interactive-toasts.md)
