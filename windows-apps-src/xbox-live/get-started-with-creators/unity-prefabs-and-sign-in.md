---
title: XBL en Unity prefabricados e inicio de sesión
description: Se tratan las redes sociales prefabricados y ejemplos de script para los servicios sociales en Xbox Live
ms.date: 01/24/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, unity
ms.openlocfilehash: a893858dac11fa848c2601df2c1bd6292b72ac6d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660040"
---
# <a name="unity-prefabs-and-scripted-sign-in"></a>Unity prefabricados e inicio de sesión generada por script

En este artículo le guiará a través de adición de Xbox Live inicio de sesión a los proyectos de Unity. Hay dos formas de lograr inicio de sesión si ya ha descargado la [complemento de Xbox Live Unity](https://github.com/Microsoft/xbox-live-unity-plugin). Puede usar el prefabricados dentro del complemento o usar los scripts y las bibliotecas incluidas para generar un script Xbox Live inicio de sesión en sus propio GameObjects personalizados.

> [!IMPORTANT]
> En este artículo se aplica a una versión del complemento antes de una actualización realizada en mayo de 2018 (versión 1804). Si instaló el complemento de Xbox Live después de ese momento o, aún no ha descargado, puede tener una versión más reciente que tiene diferencias significativas con cómo se realiza en el inicio de sesión. Además, encontrará que las capturas de pantalla en este complemento no coinciden con los de la versión más reciente. En su lugar, consulte el [artículo para el prefabricado de inicio de sesión actualizada](playerauthentication-prefab-sign-in.md) como [el artículo que detalla los métodos para generar scripts de inicio de sesión actualizados](sign-in-manager.md).

## <a name="before-you-begin"></a>Antes de comenzar

Antes de empezar a agregar servicios de Xbox Live social a su juego Unity hay unos pocos pasos que debe completar antes de profundizar en. En primer lugar, asegúrese de que ha descargado e integra la [complemento de Xbox Live Unity](https://github.com/Microsoft/xbox-live-unity-plugin). En segundo lugar, querrá tener el título reservado y se publican a través de la [Microsoft Development Center](https://developer.microsoft.com/en-us/games/uwp). Lectura [crear un nuevo título de programa de creadores de Xbox Live](../get-started-with-creators/create-and-test-a-new-creators-title.md) para obtener instrucciones sobre la publicación de su título.
Por último, lea [configurar Xbox Live en Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) para configurar el entorno de Unity correctamente y configurar el título para usar servicios de Xbox Live. Una vez que el proyecto de Unity se ha configurado correctamente, es el momento para obtener información sobre las herramientas que puede usar en el título de Xbox Live habilitado, así como las dos formas principales de Xbox Live se puede implementar en Unity: prefabricados y secuencias de comandos.

## <a name="prefabs"></a>Prefabricados

Unity tiene un tipo de recurso prefabricado que le permite almacenar un GameObject junto con los componentes y las propiedades. El recurso prefabricado actúa como una plantilla desde el que puede crear nuevas instancias de objetos de la escena de Unity.
[Más información sobre prefabricados desde el sitio Web de Unity](https://unity3d.com/learn/tutorials/topics/interface-essentials/prefabs-concept-usage).

El complemento de Xbox Live Unity proporciona unos prefabricados que pueden utilizar en el proyecto para utilizar las características de Xbox Live. El prefabricados descritos en este artículo le permitirá iniciar sesión, [agregar compatibilidad con varios usuario](../get-started-with-creators/add-multi-user-support.md) a su título, o para mostrar la lista de amigos de con signo en Xbox Live perfil. Puede encontrar estas y otras prefabricados bajo el **proyecto** pestaña siguiendo la ruta de acceso: **Activos > Xbox Live > prefabricados**.

### <a name="the-userprofile-prefab"></a>El recurso prefabricado UserProfile

El prefabricado sociales primero y más importante es la **UserProfile** prefabricado. El **UserProfile** prefabricado tiene todo lo necesario para permitir un inicio de sesión en Xbox Live. Esto es muy importante, ya que debe iniciar sesión un usuario antes de usar servicios de Xbox Live. El recurso prefabricado contiene el botón de inicio de sesión y un GameObject para representar un registrados en el Reproductor por su nombre de jugador, Gamerpic y puntuación.

> [!NOTE]
> Para poder usar cualquiera de los otros prefabricados Xbox Live, se debe incluir un **UserProfile** prefabricado o invocar manualmente la API de inicio de sesión.

![UserProfile prefabricado en activos y jerarquía](../images/unity/unity-userprofile-views.png)

Si expande el **UserProfile** prefabricado en el **proyecto** panel o en el **jerarquía** después de que se ha agregado a una escena, verá que el **UserProfile**  prefabricado contiene dos GameObjects dentro de él. El primer objeto es el **SignInPanel** que contiene la experiencia de inicio de sesión de botón. El segundo objeto es el **ProfileInfo** que contendrá la información sobre el usuario una vez que inicien sesión. El **UserProfile** prefabricado es lo va a usar para representar la información de cualquier usuario con sesión iniciada localmente en el título que Xbox Live.

### <a name="the-xboxliveuser-prefab"></a>El recurso prefabricado XboxLiveUser

El **UserProfile** prefabricado usa un prefabricado social en segundo lugar, su código llamado el **XboxLiveUser**. Uso de este recurso prefabricado no es inmediatamente evidente que no tenga que se agregarán a la jerarquía de la escena, tal como se puede simplemente crear instancias en el código. El **XboxLiveUser** no tiene ninguna representación visual, simplemente contiene los detalles relativos al usuario de Xbox Live. Necesitará una instancia de la **XboxLiveUser** para cada instancia de la **UserProfile**. Esto es muy importante cuando [agregar compatibilidad con varios usuario](../get-started-with-creators/add-multi-user-support.md) en su título. Además que contiene información sobre el usuario después de iniciar sesión en este recurso prefabricado también es un contenedor para el código usado para el inicio de sesión de un usuario de Xbox Live.

## <a name="sign-in-with-the-userprofile-prefab"></a>Inicie sesión con el recurso prefabricado UserProfile

Existen la prefabricados del complemento de Xbox Live Unity para simplificar ciertas tareas de desarrollo. Para habilitar la Xbox Live inicio de sesión en el proyecto de Unity, simplemente deberá usar el **UserProfile** y **XboxLiveServices** prefabricados junto con un Unity **EventSystem**.

En primer lugar arrastre los **UserProfile** prefabricado de una escena. Lo ideal es que el **UserProfile** debe colocarse en la pantalla inicial del menú del proyecto.

![Arrastre userprofile a jerarquía](../images/unity/drag-userprofile.gif)

Además el **UserProfile** prefabricado también deberá asegurarse de que el **XboxLiveServices** prefabricado está presente en al menos la primera escena del proyecto.
El **XboxLiveServices** prefabricado le permite activar o desactivar si determinados prefabricados registrará la información de depuración. Esto es útil para la comprobación de comportamiento prefabricado.

![Busque xboxliveservices prefabricado](../images/unity/check-for-xboxliveservices.gif)

Por último, el **UserProfile** también requiere una **EventSystem** se ejecute correctamente. Esto se puede agregar el botón secundario en la escena de inicio de sesión, a continuación, haga clic en a través de **GameObject--> interfaz de usuario--> EventSystem**.

![Agregue el sistema de eventos](../images/unity/add_event_system.gif)

Si especifica el modo de juego, el servicio firmará automáticamente en un usuario. En Unity, el SDK de Xbox Live simula las llamadas al servicio de Xbox Live y envía datos falsos atrás para trabajar con. Para ver los datos en directo, deberá compilar el proyecto como una aplicación de UWP y ejecútelo desde Visual Studio. Consulte [configurar Xbox Live en Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) para obtener más información. Al especificar el modo de reproducción en Unity, el recurso prefabricado se rellenará con los datos falsos, lo que simulan información como el nombre de jugador, Gamerpic y puntuación de un reproductor. Esta es la información que se debe mostrar mediante el **UserProfile** prefabricado.

En un inicio de sesión correcta tendrá un aspecto similar al siguiente: ![play userprofile correcta](../images/unity/correct-user-profile-play.gif)

Si no ha configurado el proyecto para conectarse a Xbox Live escribir correctamente el modo de juego deshabilitará el botón de inicio de sesión y mostrar un mensaje de error.

El siguiente es un ejemplo de un inicio de sesión con error debido a una configuración incorrecta de la aplicación Xbox Live.
![no se pudo reproducir userprofile](../images/unity/flawed-user-profile-play.gif)

## <a name="scripting-sign-in"></a>Inicio de sesión de secuencias de comandos

Ahora que sabe cómo usar el **UserProfile** prefabricado que sería mejor buscar la secuencia de comandos subyacente que controla la funcionalidad de la prefabricado. Si observa la **UserProfile** en el **Inspector** , verá que tiene un **UserProfile.cs** script conectadas a ella. Este script tiene todo lo que necesita para un usuario inicie sesión y cargar la información de perfil que desea mostrar en el inicio de sesión. Sin embargo, en lugar de en el prefabricado verá todo (que se puede actualizar con el tiempo) que vamos a mirar unas pocas líneas de ejemplo de código para entender qué hace falta para el inicio de sesión de un usuario de Xbox Live.

### <a name="the-xboxliveuser-class"></a>La clase XboxLiveUser

Se incluyen las llamadas es necesarios que un usuario inicie sesión en el `XboxLiveUserInfo` clase. En el **UserProfile.cs** script verá que hay una instancia de la `XboxLiveUserInfo` clase denominada `XboxLiveUser`. En nuestro ejemplo de código se usará el mismo nombre de variable. El `XboxLiveUserInfo` clase contiene una instancia de la `XboxLiveUser` clase llamada `User` como uno de sus variables miembro. El `XboxLiveUser` clase contiene las funciones de inicio de sesión necesarias para iniciar sesión en el `XboxLiveUser`. Se usará la instancia de la `XboxLiveUser` clase `User` al inicio de sesión de usuarios, así como para obtener información sobre el que se describe como su nombre de jugador, Gamerpic y puntuación. Para ello debe inicializar una instancia de la `XboxLiveUserInfo` clase y usar resultante `XboxLiveUserInfo.User` a llamada de inicio de sesión.

### <a name="initialize-the-xboxliveuser"></a>Inicializar el XboxLiveUser

Inicializando el usuario de Xbox Live es el primer paso antes de realmente iniciar sesión en Xbox Live. Esto se hace muy sencillo en el código mediante el `XboxLiveUserInfo.Initialize()` función.
En nuestro ejemplo de código se usa el `XboxLiveUser` variable miembro como nuestro `XboxLiveUserInfo` instancia e inicializar que desea usar para el inicio de sesión.

```csharp
    void ButtonClickTask()
    {
        this.StartCoroutine(this.InitializeXboxLiveUser());
    }

    public IEnumerator InitializeXboxLiveUserAndCallSignIn()
    {
        // Disable the sign-in button
        SignInButton.interactable = false;

        this.XboxLiveUser.Initialize();

        //Wait until the Xbox User has been initialized to call SignInAsync()
        yield return new WaitUntil(() => this.XboxLiveUser != null && this.XboxLiveUser.User != null);
        this.StartCoroutine(this.SignInAsync());
    }
```

Mirar este código de ejemplo verá que el `XboxLiveUserInfo.Initialize()` función se llama en respuesta a un clic del botón. El completo **UserProfile.cs** prefabricado script tiene código que permite el inicio de sesión automático donde `XboxLiveUserInfo.Initialize()` se llama sin interacción.
El `XboxLiveUserInfo.Initialize()` creará una nueva función `XboxLiveUserInfo.User` que nos permitirá llamar a las funciones de inicio de sesión incluidas en el `XboxLiveUserInfo.User` clase.

### <a name="call-sign-in"></a>Inicio de sesión de llamada

Una vez que se ha inicializado el XboxLiveUser hora a llamada de inicio de sesión. En **UserProfile.cs** inicio de sesión se denomina en el `SignInAsync()` función de UserProfile.cs. En el ejemplo de código anterior se simplemente espere el `XboxLiveUser` que inicializarse antes de llamar a la `SignInAsync()` función.

> [!NOTE]
> Es necesario esperar a que el `XboxLiveUser` que inicializarse antes de llamar al inicio de sesión porque la `XboxLiveUser` contiene el `XboxLiveUser.User` propiedad que se utiliza a llamada de inicio de sesión.

En **UserProfile.cs** el `SignInAsync()` función contiene dos funciones de inicio de sesión que se pueden usar para iniciar sesión el usuario. `XboxLiveUser.User.SignInSilentlyAsync()` y `XboxLiveUser.User.SignInAsync()` éstas son las funciones que inician la sesión del usuario. El `SignInAsync()` función es un buen ejemplo de cómo utilizar adecuadamente estas funciones. Ejemplo de código siguiente muestra un método adecuado para llamar a las dos funciones:

```csharp
SignInStatus signInStatus;
TaskYieldInstruction<SignInResult> signInSilentlyTask = this.XboxLiveUser.User.SignInSilentlyAsync().AsCoroutine();
yield return signInSilentlyTask;

signInStatus = signInSilentlyTask.Result.Status;
if (signInSilentlyTask.Result.Status != SignInStatus.Success)
{
    TaskYieldInstruction<SignInResult> signInTask = this.XboxLiveUser.User.SignInAsync().AsCoroutine();
    yield return signInTask;

    signInStatus = signInTask.Result.Status;
}
```

En este ejemplo, los resultados de las llamadas de inicio de sesión se almacenan en la variable `signInStatus`. Esto nos permite comprobar si el inicio de sesión fue correcto y actúe en consecuencia. En este ejemplo que la función primero intenta iniciar sesión en modo silencioso, a continuación, si se produce un error en el inicio de sesión silenciosa la función llama el inicio de sesión normal en función. Una vez que tenga una llamada correcta a una de las funciones de inicio de sesión que habrá iniciado sesión el usuario. Ahora puede usar el `XboxLiveUser.User` para obtener y mostrar los detalles sobre el usuario con sesión iniciada. Eche un vistazo a la `LoadProfileInfo()` funcionando en **UserProfile.cs** para obtener un ejemplo de cómo usar el `XboxLiveUser.User` para mostrar información sobre un usuario con sesión iniciada.

## <a name="build-and-test-sign-in"></a>Compilar y probar el inicio de sesión

Cuando se ejecuta el título en el editor, verá datos falsos cuando intenta usar la funcionalidad de Xbox Live. Para iniciar sesión con un perfil real y pruebe la funcionalidad de Xbox Live en su título, deberá crear una solución UWP y ejecútelo en Visual Studio.  Puede compilar el proyecto UWP en Unity siguiendo estos pasos:

1. Abra el **configuración de compilación** ventana seleccionando **archivo** > **configuración de compilación**.
2. Agregue todas las escenas que se van a incluir en la compilación en el **escenas en compilación** sección.
3. Cambie a la **Universal Windows Platform** seleccionando **plataforma Universal de Windows** en **plataforma** y haga clic en **Cambiar plataforma**.
4. Establecer **SDK** a **10.0.15063.0** o superior.
5. Para habilitar la comprobación de depuración de script **Unity C# proyectos**.
6. Haga clic en **compilar** y especifique la ubicación del proyecto.

Una vez finalizada la compilación, Unity habrá genera un nuevo archivo de solución UWP que necesitará para ejecutar en Visual Studio:

1. En la carpeta que especificó, abra  **&lt;ProjectName&gt;.sln** en Visual Studio.
2. En la barra de herramientas en la parte superior, seleccione **x64** e implementar en el **máquina Local**.

Si habilitó **depuración de scripts** cuando compile la solución para UWP desde Unity, los scripts se encontrarán en el **ensamblado-CSharp (Windows Universal)** proyecto.

> [!NOTE]
> Antes de usar la compilación de Visual Studio para probar el juego con datos reales, siga [esta lista de comprobación](test-visual-studio-build.md) para ayudar a garantizar su título podrán tener acceso al servicio de Xbox Live.

## <a name="troubleshooting"></a>Solución de problemas

Si tiene problemas para iniciar sesión en Xbox Live intente leer [solución de problemas de Xbox Live inicio de sesión de](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md).
