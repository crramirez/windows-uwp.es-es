---
title: Inicie sesión con la SignInManager en Unity
description: Información general del Administrador de inicio de sesión de complemento de Unity
ms.date: 05/08/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, unity
ms.openlocfilehash: e6d066fe7792912f8918cb139d45ff05d105feaa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641730"
---
# <a name="scripting-sign-in"></a>Inicio de sesión de secuencias de comandos

Para agregar inicio de sesión a sus propios objetos de juego personalizados que necesita para generar el script en un GameObject. Supongamos que el recurso prefabricado PlayerAuthentication no se ajusta a su juego y que le gustaría tener su propio panel de inicio de sesión, en este artículo le guiará por los pasos básicos de agregar lógica de inicio de sesión en su título.

## <a name="sign-in-with-the-signinmanager"></a>Inicie sesión con la SignInManager

El complemento Xbox Live Unity contiene un script para el `SignInManager` bajo la ruta de acceso de archivo **activos >> XboxLive >> Scripts >> SignInManager.cs**. El administrador es una clase Singleton que puede llamarse formulario en cualquier parte de su título consultando el título *instancia* de la `SignInManager`. Esto *instancia* no se deben inicializar y se puede usar la TI tan pronto como comienza su juego. Puede tener acceso a toda la sus propiedades públicas y funciones haciendo referencia a la *instancia* como `SignInManager.Instance`.

El `SignInManager` contiene todo el código necesario para administrar la autenticación para el título, esto incluye iniciar sesión, cierre de sesión, y obtener información acerca de los usuarios que haya iniciado sesión como qué Reproductor.

### <a name="calls-and-results"></a>Las llamadas y los resultados

El `SignInManager` tiene tres funciones de rutina conjunta async `SignInPlayer(int playerNumber)`, `SignOutPlayer(int playerNumber)`, y `SwitchUser(int playerNumber)`, funciones de ese evento de desencadenador para recopilar los resultados de la llamada y actuar en consecuencia. Puede agregar funciones correspondientes a la secuencia de comandos y asignarlas a los `SignInManager.Instance`de lista de devolución de llamada. Las funciones de eventos son `OnPlayerSignIn(int playerNumber, UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`, `OnPlayerSignOut(int playerNumber, UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`, `OnAnyPlayerSignIn(UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`, y `OnAnyPlayerSignOut(UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`. Cada en una de las funciones de eventos escucha los eventos descritos en su nombre. Puede agregar sus propias funciones a la lista del administrador devolución de llamada en su título `Start()` función con el código siguiente.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

void Start () {
    try
    {
        SignInManager.Instance.OnPlayerSignOut(this.playerNumber, this.OnPlayerSignOut);
        SignInManager.Instance.OnPlayerSignIn(this.playerNumber, this.OnPlayerSignIn);
    }
    catch (Exception ex)
    {
        Debug.LogWarning(ex.Message);
    }

}
```

Este fragmento de código agrega los agentes de escucha de inicio de sesión y cierre de sesión para el Reproductor de playerNumber de este GameObject. Este GameObject `OnPlayerSignIn` función llamará cuando el `SignInManager` detecta un intento de inicio de sesión se ha completado y su `OnPlayerSignOut` función se llamará cuando el SignInManager detecta un cierre de sesión. Las funciones de eventos en el GameObject deben tener un tipo de valor devuelto y parámetros para que coincida con el tipo de función que llamó la SignInManager. Tanto el `OnPlayerSignIn` y `OnPlayerSignOut` son funciones void que necesitan un `XboxLiveUser`, `XboxLiveAuthStatus`y una cadena como sus parámetros. El shell de las funciones puede tener el siguiente aspecto:

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

private void OnPlayerSignIn(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
}

private void OnPlayerSignOut(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
}
```

En ambas funciones, compruebe el `XboxLiveAuthStatus` para asegurarse de que la llamada a la `SignInManager.Instance` fue correcta. En una llamada correcta el `XboxLiveUser` será el `XboxLiveUser`, que se firmó en nuestra fuera por `SignInManager`. Cuando la llamada es incorrecta el `errorMessage` cadena contendrá más información sobre el motivo del error.

Agregar unas pocas líneas de código para comprobar si una llamada correcta daría lugar a código similar al siguiente:

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

private void OnPlayerSignIn(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
    if(authStatus == XboxLiveAuthStatus.Succeeded)
    {
        this.xboxLiveUser = xboxLiveUserParam; //store the xboxLiveUser SignedIn
        this.signedIn = true;
    }
    else
    {
        if (XboxLiveServicesSettings.Instance.DebugLogsOn)
        {
            Debug.LogError(errorMessage); //Log the error message in case of unsuccessful call. 
        }
    }
}

private void OnPlayerSignOut(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
    if (authStatus == XboxLiveAuthStatus.Succeeded)
    {
        this.xboxLiveUser = null;
        this.signedIn = false;
    }
    else
    {
        if (XboxLiveServicesSettings.Instance.DebugLogsOn)
        {
            Debug.LogError(errorMessage);
        }
    }
}
```

Llamando al inicio de sesión y capturar el evento resultante para el resultado, puede controlar el inicio de sesión y cierre de sesión para el título.

## <a name="get-signed-in-player-information"></a>Iniciar sesión en la información sobre el Reproductor

Además de la firma de los jugadores en el servicio la SignInManager realiza un seguimiento de todos los usuarios con sesión iniciada. En PC esto estará limitado a una sola sesión en el Reproductor y, en la consola Xbox está limitado a 16. Puede comprobar cómo cerca del límite están comparando el resultado de `SignInManager.Instance.GetCurrentNumberOfPlayers()` al resultado de `SignInManager.Instance.GetMaximumNumberOfPlayers()`. El SignInManager tiene un diccionario de los jugadores con sesión iniciada indizado por ese jugador *playerNumber*. Se puede usar para recuperar cierta información básica sobre el Reproductor accesible de sus asociados `XboxLiveUser`.

```csharp
if (SignInManager.Instance.GetPlayer(this.playerNumber).IsSignedIn) // If there is a player signed in for this gameObjects player number
            {
                this.displayedGamertag = SignInManager.Instance.GetPlayer(this.playerNumber).Gamertag; // Set that users gamertag to the gamertag displayed
            }
```

Este pequeño fragmento de código comprueba si hay un reproductor que inicien sesión la ranura número Reproductor para este GameObject y, a continuación, almacena ese nombre de jugador de los usuarios que se mostrará si haya iniciado sesión. Mientras el `XboxLiveUser` contiene firmado en el nombre de jugador de los usuarios y el Id. de usuario de Xbox (xuid), deberá llamar a otros servicios, como el `SocialManager` para acceder a información como gamerpic y puntuación.

## <a name="destroying-your-sign-in-gameobject"></a>Destruir el GameObject inicio de sesión

Cuando se destruye un objeto de juego que utiliza uno de los administradores de complemento de Xbox Live, como el `SignInManager` o `SocialManager`, por lo general al cargar una nueva escena, es importante quitar las funciones de agregado a la lista de agentes de escucha de eventos para el administrador. En el código de ejemplo de este artículo se debería quitar las funciones que se agregan a los agentes de escucha de eventos para el inicio de sesión y cierre de sesión. Se recomienda eliminar estas funciones desde la `SignInManager` en el `OnDestroy()` función de nuestro GameObject.

```csharp
private void OnDestroy()
{
    if (SignInManager.Instance != null)
    {
        SignInManager.Instance.RemoveCallbackFromPlayer(this.PlayerNumber, this.OnPlayerSignOut);
        SignInManager.Instance.RemoveCallbackFromPlayer(this.PlayerNumber, this.OnPlayerSignIn);
    }
```

Este código quitará las funciones de devolución de llamada de inicio de sesión y cierre de sesión para el Reproductor asociado con este GameObject.

## <a name="testing-you-code-in-visual-studio"></a>Probar el código en Visual Studio

Además el [pasos necesarios para crear su juego en Visual Studio](configure-xbox-live-in-unity.md#build-and-test-the-project), como se indicó en el [configurar el título de Xbox Live para Unity](configure-xbox-live-in-unity.md) del artículo, hay un paso adicional necesario para probar el juego correctamente en Visual Studio. Necesita actualizar una propiedad del archivo package.appxmanifest.xml. Para ello:

1. Buscar en el Explorador de soluciones para el archivo package.appxmanifest.xml
2. A la derecha, haga clic en el archivo y seleccione Ver código
3. En el `<Properties><\/Properties>` sección, agregue la siguiente línea: ' < uap:SupportedUsers > varios <\/uap:SupportedUsers >.
4. Implementar el juego en Xbox iniciando una compilación de depuración remota desde Visual Studio. Puede encontrar instrucciones para configurar el título de una consola Xbox en el [configurar su UWP en el entorno de desarrollo de Xbox](../../xbox-apps/development-environment-setup.md) artículo.

> [!NOTE]
> El fragmento de configuración ha cambiado puede parecer que se está habilitando a multijugador pero sigue siendo necesario ejecutar el juego en escenarios de un solo jugador.

## <a name="policies-and-limitations"></a>Las directivas y limitaciones

Hay algunas limitaciones del título que desee considerar al desarrollar su experiencia de inicio de sesión y las directivas de inicio de sesión.

- Después de su título inicial inicio de sesión debe tener al menos un reproductor con sesión iniciada. El `SignInManager` se producir un error y se producirá un error en la llamada si se intenta cerrar la sesión la última sesión de usuario. También es importante tener en cuenta que no se puede llamar a `SignInManager.Instance.SwitchUser(int playerNumber)`, en la última inicien sesión en el Reproductor como tratará de cerrar sesión el Reproductor antes de iniciar sesión en un nuevo reproductor.

- PC puede solo inicio de sesión de un usuario a la vez, consola pueda iniciar sesión en hasta 16 jugadores a la vez.

- El título realmente no tiene permiso para cerrar la sesión de un reproductor desde el sistema operativo, debido a este cierre de sesión no funcionen según lo previsto. El SignInManager puede iniciar sesión un usuario de entrada y salida que se refiere el título, pero no puede cerrar la sesión cualquier usuario de la máquina que el título se implementa en.

- Solo está disponible en la consola Xbox un inicio de sesión de varios usuarios.

- Las cuentas de invitado no están disponibles para los títulos del programa de creadores de Xbox Live.