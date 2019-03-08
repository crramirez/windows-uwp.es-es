---
title: Autenticación para los proyectos de UWP
author: aablackm
description: Obtenga información sobre cómo iniciar sesión en Xbox Live, los usuarios en un título de la plataforma Universal de Windows (UWP).
ms.assetid: e54c98ce-e049-4189-a50d-bb1cb319697c
ms.author: aablackm
ms.date: 03/19/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, autenticación, inicio de sesión
ms.localizationpriority: medium
ms.openlocfilehash: 5473b7ede7731d7d07b7e5bfd72857fdb64f1c89
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628390"
---
# <a name="authentication-for-uwp-projects"></a>Autenticación para los proyectos de UWP

Para aprovechar las características de Xbox Live en los juegos, un usuario debe crear un perfil de Xbox Live para identificarse en la Comunidad de Xbox Live.  Servicios de Xbox Live realizar un seguimiento de juego relacionados con actividades mediante ese perfil de Xbox Live, como nombre de jugador y la imagen jugador, que son de amigos de juegos del usuario, el usuario lo que el usuario de juegos ha desempeñado, qué logros que el usuario ha desbloqueado, donde el usuario se encuentra en el marcador para un determinado etcetera juego.

Cuando un usuario desea tener acceso a servicios de Xbox Live en un juego en particular en un dispositivo concreto, el usuario debe autenticarse primero.  El juego puede llamar a la API de Xbox Live para iniciar el proceso de autenticación.  En algunos casos, el usuario se presentará una interfaz para proporcionar información adicional, como escribir el nombre de usuario y la contraseña de la Microsoft Account desea usar, dar el consentimiento de permiso para el juego, resolver problemas de cuentas, aceptando nuevas condiciones de uso, etcetera.

Una vez autenticado, el usuario está asociado con ese dispositivo hasta que cierren explícitamente de Xbox Live desde la aplicación de Xbox.  Se permite solo uno de los jugadores se autentique en un dispositivo de consola a la vez (para todos los Xbox Live juegos);  un nuevo jugador se autentique en un dispositivo de consola, el Reproductor autenticado existente debe iniciar sesión por primera vez.

## <a name="steps-to-sign-in"></a>Pasos para iniciar sesión

En un nivel alto, usa las API de Xbox Live siguiendo estos pasos:

1. Crear un objeto XboxLiveUser para representar al usuario
2. Inicie sesión en modo silencioso Xbox Live en el inicio
3. Intenta iniciar sesión con experiencia de usuario si es necesario
4. Crear un contexto de Xbox Live en función de la interacción de usuario
5. Use el contexto de Xbox Live para tener acceso a servicios de Xbox Live
6. Cuando el usuario o el programa sale del juego signos horizontal, el objeto XboxLiveUser y el objeto de XboxLiveContext de versiones si se establecen en null

### <a name="creating-an-xboxliveuser-object"></a>Creación de un objeto XboxLiveUser

La mayoría de las actividades de Xbox Live está relacionados con el usuario de Xbox Live.  Como desarrollador de juegos, deberá crear primero un objeto XboxLiveUser para representar al usuario local.

C++:

```cpp
auto xboxUser = std::make_shared<xbox_live_user>(Windows::System::User^ windowsSystemUser);
```

C++ / c++ / CX (WinRT):

```cpp
XboxLiveUser xboxUser = ref new XboxLiveUser(Windows::System::User^ windowsSystemUser);
```

C#(WinRT):

```csharp
XboxLiveUser xboxUser = new XboxLiveUser(Windows.System.User windowsSystemUser);
```

* **windowsSystemUser** el objeto de usuario del sistema de windows que se usará para asociar el usuario de xbox live. Podría ser nullptr si la aplicación es un application(SUA) de usuario único.
  * Para obtener más información sobre Application(SUA) de usuario único y múltiple Application(MUA) de usuario, consulte [Introducción a las aplicaciones multiusuario](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/multi-user-applications#single-user-applications)
  * Para obtener más información sobre cómo obtener Windows::System::User ^ desde Windows, compruebe [recuperación de usuario del sistema de windows en UWP](retrieving-windows-system-user-on-UWP.md)

### <a name="sign-in-silently-to-xbox-live-at-startup"></a>Inicie sesión en modo silencioso Xbox Live en el inicio ###

Debe iniciar su juego autenticar al usuario a Xbox Live tan pronto como sea posible después de iniciar, incluso antes de presentar la interfaz de usuario para capturar previamente los datos de servicios de Xbox Live.

Para autenticar al usuario local en modo silencioso, llame a

C++:

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin_silently(Platform::Object^ coreDispatcher)
```

C++ / c++ / CX (WinRT):

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInSilentlyAsync(Platform::Object^ coreDispatcher)
```

C#(WinRT):

```csharp
Microsoft.Xbox.Services.System.SignInResult XboxLiveUser.SignInSilentlyAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  Subproceso de distribuidor se usa para la comunicación entre subprocesos. Aunque la API de inicio de sesión silenciosa no va a mostrar ninguna interfaz de usuario, XSAPI sigue necesitando el distribuidor del subproceso de interfaz de usuario para obtener información sobre la configuración regional de su appx. Puede obtener estático distribuidor del subproceso de interfaz de usuario mediante una llamada a Windows::UI::Core::CoreWindow::GetForCurrentThread() -> distribuidor en el subproceso de interfaz de usuario. O bien, si está seguro de que esta API se llama en el subproceso de interfaz de usuario, puede pasar en nullptr (por ejemplo, en JS UWA).


Hay 3 posibles resultados desde el intento de inicio de sesión silencioso

* **Éxito**

  Si el dispositivo está en línea, esto significa que el usuario que se autenticó correctamente a Xbox Live y fuimos capaces de obtener un token válido.

  Si el dispositivo está desconectado, esto significa que el usuario ha autenticado previamente en Xbox Live correctamente y no tiene explícitamente firmado horizontal de este título.  Tenga en cuenta en este caso no hay ninguna garantía de que el título tiene acceso a un token válido, solo se garantiza que la identidad del usuario se conoce y se ha comprobado.    Se conoce la identidad del usuario en el título a través de su identificador de usuario de xbox (xuid) y el nombre de jugador.

* **UserInteractionRequired**

  Esto significa que el tiempo de ejecución no pudo iniciar sesión en el usuario en modo silencioso.  El juego debe llamar a `xbox_live_user::sign_in` que invoca el proveedor de identidades de Xbox para mostrar el flujo de experiencia de usuario necesario para el usuario para el inicio de sesión de registro/Inicio de sesión.  Problemas comunes son:

  * Usuario no tiene una Account de Microsoft
  * Usuario no ha establecido un Account Microsoft preferida para juegos
  * La Account Microsoft seleccionada no tiene un perfil de Xbox Live
  * Usuario debe aceptar el consentimiento Account de Microsoft

* **Otros errores**

  El tiempo de ejecución no pudo iniciar sesión debido a otros motivos.  Estos problemas no suelen procesables el juego o el usuario. Cuando se usa la API de c ++, necesitaría comprobar error activando xbox_live_result <> .err(); en WinRT, necesitaría catch Platform:: Exception ^.

### <a name="attempt-to-sign-in-with-ux-if-required"></a>Intenta iniciar sesión con experiencia de usuario si es necesario ###

El juego debe autenticar al usuario a Xbox Live con la experiencia de usuario habilitada cuando el inicio de sesión silenciosa no tuvo éxito y esté listo para presentar la interfaz de usuario.

Para autenticar al usuario local con la experiencia de usuario, llame a

C++:

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin(Platform::Object^ coreDispatcher)
```


C++ / c++ / CX (WinRT):

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInAsync(Platform::Object^ coreDispatcher)
```

C#(WinRT):

```csharp
Microsoft.Xbox.Services.System.SignInResult  XboxLiveUser.SignInAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  Subproceso de distribuidor se usa para la comunicación entre subprocesos. Inicio de sesión en la API requiere que el distribuidor de la interfaz de usuario para que puede mostrar el inicio de sesión en la interfaz de usuario y obtener la información sobre la configuración regional de su appx. Puede obtener estático distribuidor del subproceso de interfaz de usuario mediante una llamada a Windows::UI::Core::CoreWindow::GetForCurrentThread() -> distribuidor en el subproceso de interfaz de usuario. O bien, si está seguro de que esta API se llama en el subproceso de interfaz de usuario, puede pasar en nullptr (por ejemplo, en JS UWA).

Hay 3 posibles resultados del intento de inicio de sesión con la experiencia de usuario:

* **Éxito**

  Si el dispositivo está en línea, esto significa que el usuario que se autenticó correctamente a Xbox Live y fuimos capaces de obtener un token válido.

  Si el dispositivo está desconectado, esto significa que el usuario ha autenticado previamente en Xbox Live correctamente y no tiene explícitamente firmado horizontal de este título.  Tenga en cuenta en este caso no hay ninguna garantía de que el título tiene acceso a un token válido, solo se garantiza que la identidad del usuario se conoce y se ha comprobado.    Se conoce la identidad del usuario para el identificador de usuario de xbox de título (xuid) y el nombre de jugador.

* **UserCancel**

  Esto significa que el usuario canceló la operación de inicio de sesión antes de completarse.  Cuando esto sucede, el juego no debería reintentar automáticamente inicie sesión con la experiencia de usuario.  En su lugar, debe presentar experiencia del usuario en el juego que permite al usuario que vuelva a intentar la operación de inicio de sesión.  (Por ejemplo, un botón de inicio de sesión)

* **Otros errores**

  El tiempo de ejecución no pudo iniciar sesión debido a otros motivos.  Estos problemas no suelen procesables el juego o el usuario. Cuando se usa la API de c ++, necesitaría comprobar error activando xbox_live_result <> .err(); en WinRT, necesitaría catch Platform:: Exception ^.

## <a name="sign-in-code-examples"></a>Ejemplos de código de inicio de sesión

### <a name="c"></a>C++

```cpp

#include "xsapi\services.h" // contains the xbox::services::system namespace

using namespace xbox::services::system; // contains definitions necessary for sign-in

void SignInSample::SignIn()
{
    //1. Create an xbox_live_user object
    m_user = std::make_shared<xbox::services::system::xbox_live_user>(); // m_user declared in header file

    //2. Sign-In silently to Xbox Live at startup
    m_user->signin_silently()
        .then([this](xbox::services::xbox_live_result<xbox::services::system::sign_in_result> result)
    {
        if (!result.err())
        {
            auto rPayload = result.payload();
            switch (rPayload.status())
            {
            case xbox::services::system::sign_in_status::success:
                // sign-in successful
                signIn = true;
                break;
            case xbox::services::system::sign_in_status::user_interaction_required:
                // 3. Attempt to sign-in with UX if required
                m_user->signin(Windows::UI::Core::CoreWindow::GetForCurrentThread()->Dispatcher)
                    .then([this](xbox::services::xbox_live_result<xbox::services::system::sign_in_result> loudResult) // use task_continuation_context::use_current() to make the continuation task running in current apartment 
                {
                    if (!loudResult.err())
                    {
                        auto resPayload = loudResult.payload();
                        switch (resPayload.status())
                        {
                        case xbox::services::system::sign_in_status::success:
                            // sign-in successful
                            signIn = true;
                            break;
                        case xbox::services::system::sign_in_status::user_cancel:
                            // user cancelled sign in 
                            // present in-game UX that allows the user to retry the sign-in operation. (For example, a sign-in button)
                            break;
                        }
                    }
                    else
                    {
                        //login has failed at this point
                    }
                }, concurrency::task_continuation_context::use_current());
                break;
            }
        }
    });
    if (signIn)
    {
        // 4. Create an Xbox Live context based on the interacting user
        m_xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(m_user); // m_xboxLiveContext declared in header file

        // add sign out event handler
        AddSignOut();
    }
}

void SignInSample::AddSignOut()
{
    xbox::services::system::xbox_live_user::add_sign_out_completed_handler(
        [this](const xbox::services::system::sign_out_completed_event_args&)

    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        m_user = NULL;
        m_xboxLiveContext = NULL;
    });
}

```

### <a name="c-winrt"></a>C#(WinRT)

```csharp

using System.Diagnostics;
using Microsoft.Xbox.Services.System;
using Microsoft.Xbox.Services;

public async Task SignIn()
{
    bool signedIn = false;

    // Get a list of the active Windows users.
    IReadOnlyList<Windows.System.User> users = await Windows.System.User.FindAllAsync();

    // Acquire the CoreDispatcher which will be required for SignInSilentlyAsync and SignInAsync.
    Windows.UI.Core.CoreDispatcher UIDispatcher = Windows.UI.Xaml.Window.Current.CoreWindow.Dispatcher; 

    try
    {
        // 1. Create an XboxLiveUser object to represent the user
        XboxLiveUser primaryUser = new XboxLiveUser(users[0]);

        // 2. Sign-in silently to Xbox Live
        SignInResult signInSilentResult = await primaryUser.SignInSilentlyAsync(UIDispatcher);
        switch (signInSilentResult.Status)
        {
            case SignInStatus.Success:
                signedIn = true;
                break;
            case SignInStatus.UserInteractionRequired:
                //3. Attempt to sign-in with UX if required
                SignInResult signInLoud = await primaryUser.SignInAsync(UIDispatcher);
                switch(signInLoud.Status)
                {
                    case SignInStatus.Success:
                        signedIn = true;
                        break;
                    case SignInStatus.UserCancel:
                        // present in-game UX that allows the user to retry the sign-in operation. (For example, a sign-in button)
                        break;
                    default:
                        break;
                }
                break;
            default:
                break;
        }
        if(signedIn)
        {
            // 4. Create an Xbox Live context based on the interacting user
            Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(user);

            //add the sign out event handler
            XboxLiveUser.SignOutCompleted += OnSignOut;
        }
    }
    catch (Exception e)
    {
        Debug.WriteLine(e.Message);
    }

}

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        primaryUser = null;
        xboxLiveContext = null;
    }
```

## <a name="sign-out"></a>Cierre la sesión

### <a name="handling-user-sign-out-completed-event"></a>Evento de cierre de sesión completado de control de usuario

Verá el usuario en el cierre de sesión de un título si se produce uno de los siguientes:

1. Los usuarios inicien sesión horizontal desde la consola Xbox App (Windows 10) o el shell de consola (Xbox One). Cierre de sesión afectará a todas las aplicaciones Xbox Live habilitado instaladas para este usuario.
2. El usuario ha cambiado a otra Microsoft Account
3. El usuario ha iniciado sesión en el mismo título desde otro dispositivo

En todos estos casos, el título recibirán un evento desde el `xbox_live_user::add_sign_out_completed_handler` o `XboxLiveUser::SignOutCompleted` controladores.  El juego debe controlar el evento completado de cierre de sesión correctamente:

1. El juego debe mostrar indicación visual clara para el usuario que ver ha firmado horizontal de Xbox Live.
2. El juego no puede llamar a cualquier servicio de Xbox Live API en el controlador de eventos, porque el usuario ha ya firmado horizontal y no hay ningún token de autorización.

## <a name="sign-out-handler-code-samples"></a>Cerrar sesión ejemplos de código de controlador

### <a name="c"></a>C++

```cpp

xbox::services::system::xbox_live_user::add_sign_out_completed_handler(
        [this](const xbox::services::system::sign_out_completed_event_args&)

    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        m_user = NULL;
        m_xboxLiveContext = NULL;
    });

```

### <a name="c-winrt"></a>C#(WinRT)

```csharp
XboxLiveUser.SignOutCompleted += OnUserSignOut;

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
        {
            // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
            primaryUser = null;
            xboxLiveContext = null;
        }
```

## <a name="determining-if-the-device-is-offline"></a>Determinar si el dispositivo está desconectado

Inicio de sesión en las API todavía será correcta cuando sin conexión si el usuario ha iniciado sesión una vez y se devolverá el último inicien sesión en la cuenta.  

Si ningún usuario se ha iniciado antes, el inicio de sesión sin conexión no se podrá alcanzar.

Si el título se puede reproducir sin conexión (campaña modo, etc.), el título puede permitir al usuario jugar y progreso juego registro a través de API WriteInGameEvent y API de almacenamiento conectado, ambos funcionan correctamente mientras el dispositivo está sin conexión.

Si no se puede reproducir el título sin conexión (de varios jugadores o juego en función de servidor, etc.) el título debe llamar a la API GetNetworkConnectivityLevel para averiguar si el dispositivo está sin conexión e informar al usuario sobre el estado y las soluciones posibles (por ejemplo, ' deberá conexión a Internet para continuar...').

## <a name="online-status-code-samples"></a>Ejemplos de código de estado en línea

### <a name="c"></a>C++

```cpp

using namespace Windows::Networking::Connectivity;

//Retrieve the ConnectionProfile
ConnectionProfile^ InternetConnectionProfile = NetworkInformation::GetInternetConnectionProfile();

NetworkConnectivityLevel connectionLevel = InternetConnectionProfile->GetNetworkConnectivityLevel();

switch (connectionLevel)
{
case NetworkConnectivityLevel::InternetAccess:
    // User is connected to the internet.
    break;
case NetworkConnectivityLevel::ConstrainedInternetAccess: //Limited Internet Access Possible Authentication Required
     // display error message for user.
    LogConnectivityLine("Game Offline: Limited internet access, browser authentication may be required. "); //function writes to UI
    break;
default:
    LogConnectivityLine("Game Offline: No internet access.");
    break;
}

```

### <a name="c-winrt"></a>C#(WinRT)

```csharp
using Windows.Networking.Connectivity;

//Retrieve the ConnectionProfile
string connectionProfileInfo = string.Empty;
ConnectionProfile InternetConnectionProfile = NetworkInformation.GetInternetConnectionProfile();

NetworkConnectivityLevel connectionLevel = InternetConnectionProfile.GetNetworkConnectivityLevel();

switch(connectionLevel)
    {
        case NetworkConnectivityLevel.InternetAccess:
            // User is connected to the internet.
            break;
        case NetworkConnectivityLevel.ConstrainedInternetAccess: //Limited Internet Access Possible Authentication Required
            // display error message for user.
            LogConnectivityLine("Game Offline: Limited internet access, browser authentication may be required. "); //function writes to UI
            break;
        default:
            LogConnectivityLine("Game Offline: No internet access.");
            break;
    }
```