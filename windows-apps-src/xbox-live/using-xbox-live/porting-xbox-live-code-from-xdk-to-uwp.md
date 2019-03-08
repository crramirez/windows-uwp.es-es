---
title: Trasladar el código de Xbox Live de XDK a UWP
description: Obtenga información sobre cómo migrar código de Xbox Live de la plataforma del Kit de desarrollo de Xbox (XDK) a la plataforma Universal de Windows (UWP).
ms.assetid: 69939f95-44ad-4ffd-851f-59b0745907c8
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, xdk, portabilidad
ms.localizationpriority: medium
ms.openlocfilehash: c6e8a6ebe716f1e062940066184e9f734441371b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590820"
---
# <a name="porting-xbox-live-code-from-the-xbox-developer-kit-xdk-to-universal-windows-platform-uwp"></a>Trasladar código Xbox Live desde el Kit para desarrolladores de Xbox (XDK) a la plataforma Universal de Windows (UWP)

## <a name="introduction"></a>Introducción

En este artículo pretende ayudar a los desarrolladores que han usado el XDK Xbox uno para empezar a migrar su código de Xbox Live para Windows 10 Universal Windows Platform (UWP).

Parte de esta migración incluye conmutación desde XSAPI 1.0 (Xbox Live API de servicios incluidos en el XDK una Xbox hasta agosto de 2015) a XSAPI 2.0 (incluido en el XDK una Xbox a partir de noviembre de 2015 y también está disponible en el SDK de Xbox Live. La funcionalidad de estas API son prácticamente idénticos, pero hay algunas diferencias importantes de implementación.

Otros temas que se tratarán en este artículo incluyen la preparación del equipo de desarrollo de Windows y la instalación de otras API suele necesario al usar servicios de Xbox Live, como la API de Sockets seguros, así como la API de almacenamiento conectado para la administración de respaldo en la nube juegos guardados.

<a name="_Setting_up_and"></a>

## <a name="setting-up-and-configuring-your-project-in-partner-center-and-xdp"></a>Cómo instalar y configurar el proyecto en el centro de partners y XDP

Un título UWP que usa Xbox Live services debe estar configurada en [centro de partners](https://partner.microsoft.com/dashboard). Para obtener la información más reciente, consulte [adición de Xbox Live a un proyecto UWP nuevo o existente](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) en la consola Xbox Live Programming Guide incluido con el [Xbox Live SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx).

Los temas en la página incluyen estos pasos para usar servicios de Xbox Live en su título:

-   Crear el proyecto de aplicación para UWP en el centro de partners.

-   Use XDP para configurar el proyecto para el uso de Xbox Live.

-   Vincule su producto del centro de partners a su producto XDP.

-   Cree las cuentas de desarrollador en XDP (requerido cuando ejecutan el título de Xbox Live en su espacio aislado).

Si es compatible con los títulos de jugadores, alguna configuración adicional puede ser necesario en las plantillas de sesión de varios jugadores. Todos los títulos de Windows 10 que usa Xbox Live multijugador y escriben en un MPSD (documento de la sesión de varios jugadores) requieren este nuevo campo en la lista de "capacidades" se encuentran en las plantillas de sesión: ```userAuthorizationStyle: true```.

### <a name="enabling-cross-play"></a>Habilitación de entre-play

Si va a admitir "cross-play" (una Xbox Live configuración compartida entre los juegos de Xbox One y PC, permitir juegos multijugador entre dispositivos), también deberá agregar esta funcionalidad a las plantillas de sesión: **crossPlay: true**.

Para obtener más información sobre el uso de cross-play y sus requisitos de configuración en XDP, vea "Ingesta XDK y UWP Play entre los títulos en XDP" en la Guía de programación de Xbox Live.

Además, para algunas consideraciones de programación, vea la sección posterior [compatibilidad jugadores cruzada entre Xbox One y PC](#_Supporting_multiplayer_cross-play).

## <a name="setting-up-your-windows-development-environment"></a>Configurar el entorno de desarrollo de Windows

1.  [Descargue la última versión **Xbox Live SDK** ](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) y extraer localmente.

2.  [Instalar el **extensiones de SDK de plataforma de Xbox Live** ](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) si necesita la API de Sockets seguros o la API de guardar (también conocido como almacenamiento conectado) de juego para UWP.

3.  Agregar compatibilidad de Xbox Live a su proyecto de aplicación Windows Universal en Visual Studio. Puede agregar la fuente completa o haga referencia a los archivos binarios instalando el paquete de NuGet en el proyecto de Visual Studio. Los paquetes están disponibles para C++ y WinRT. Para obtener más información, consulte [adición de Xbox Live a un proyecto UWP nuevo o existente](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)

4.  Configurar el equipo de desarrollo para usar su espacio aislado. Hay un script de línea de comandos en el directorio de herramientas de Xbox Live SDK que puede usar desde un símbolo del sistema de administrador (por ejemplo: SwitchSandbox.cmd XDKS.1).

  **Tenga en cuenta** para volver al espacio aislado de venta directa, puede eliminar la clave del registro que modifica la secuencia de comandos, o puede cambiar en el espacio aislado llamado MINORISTA.

1.  Agregar una cuenta de desarrollador en el equipo de desarrollo. Una cuenta de desarrollador que creó en XDP es necesaria para interactuar con servicios de Xbox Live en tiempo de ejecución al que está desarrollando en su recinto de seguridad asignado o ejecución de ejemplos. Para agregar una o varias cuentas en Windows:

    1.  Abra **configuración** (acceso directo: Tecla Windows + I).

    2.  Abra **cuentas**.

    3.  En el **su cuenta** , haga clic **agregar una cuenta de Microsoft**.

    4.  Escriba el correo electrónico de la cuenta de desarrollador y la contraseña.

### <a name="appxmanifest-changes"></a>AppxManifest cambios

Los cambios entre las versiones del archivo appxmanifest.xml de Xbox y UWP más comunes son:

1. Identidad del paquete es importante en UWP, incluso durante el desarrollo. El nombre de la identidad y el publicador *debe coincidir con* lo que se definió en el centro de partners para su aplicación para UWP.

1. Se requiere una sección de dependencia del paquete. Por ejemplo:

```xml
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Universal" MinVersion="10.0.10240.0" MaxVersionTested="10.0.10240.0" />
  </Dependencies>
```

1.  Para otras secciones del manifiesto de aplicación que tenga requisitos específicos para consulte un ejemplo de manifiesto de aplicación de UWP (por ejemplo, uno de los ejemplos UWP incluidos con el SDK de Xbox Live o un proyecto de aplicación Windows Universal predeterminado creado en Visual Studio) UWP, como ```<VisualElements>```.

1.  Título y ¿SCID se definen en el archivo xboxservices.config (consulte la [siguiente sección](#_Define_your_title)) en lugar de en la categoría de extensión "xbox.live".

1.  La categoría de extensión "xbox.system.resources" no es necesario.

1.  Sockets seguros se definen en el archivo networkmanifest.xml (consulte [Secure sockets](#_Secure_sockets)) en lugar de en la categoría "windows.xbox.networking".

1.  Se debe definir una categoría de extensión "windows.protocol" para recibir invitaciones de Xbox Live en el título de la UWP (consulte [enviar y recibir invitaciones](#_Sending_and_receiving)).

1.  Si usa la API GameChat, conviene agregar la funcionalidad del dispositivo de micrófono dentro de la ```<Capabilities>``` elemento. Por ejemplo:

  ```<DeviceCapability Name="microphone">```

<a name="_Define_your_title"></a>

### <a name="define-your-title-and-scid-for-the-xbox-live-sdk-in-a-config-file"></a>Definir el título y ¿SCID para el SDK de Xbox Live en un archivo de configuración

El SDK de Xbox Live debe saber el título de la Id. y ¿SCID, que ya no están incluidos en appxmanifest.xml para títulos de UWP. En su lugar, crea un archivo de texto denominado **xboxservices.config** en el proyecto raíz del directorio y agregue los campos siguientes, reemplazando los valores con la información del título:

```
{
  "TitleId": 123456789,
  "PrimaryServiceConfigId": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
}
```

> [!NOTE]
> Todos los valores dentro de xboxservices.config distinguen mayúsculas de minúsculas.

Incluir este archivo de configuración como contenido del proyecto para que esté disponible en la salida de compilación.

**Tenga en cuenta** estos valores estarán disponibles mediante programación dentro de su título mediante el uso de la siguiente API:

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

### <a name="api-namespace-mapping"></a>Asignación de espacio de nombres de API

Tabla 1. Asignación de Namespace de XDK a UWP.

<table>
  <tr>
    <td></td>
    <td><b>Una XDK Xbox</b></td><td><b>UWP</b></td>
    <td><b>Está disponible con la API...</b></td>
  </tr>
  <tr>
    <td>Servicios de Xbox API (XSAPI)</td>
    <td>Microsoft::Xbox::Services</td>
    <td>Microsoft::Xbox::Services (<i>ningún cambio</i>)</td>
    <td>Xbox Live SDK (use NuGet binario u origen)</td>
  </tr>
  <tr>
    <td>GameChat</td>
    <td>
Microsoft::Xbox::GameChat Windows::Xbox::Chat </td>
    <td>
Microsoft::Xbox::GameChat (*ningún cambio*) Microsoft::Xbox::ChatAudio </td>
    <td>
SDK de Xbox Live (use NuGet binario) </td>
  </tr>
  <tr>
    <td>SecureSockets</td>
    <td>Windows::Xbox::Networking</td>
    <td>Windows::Networking::XboxLive</td>
    <td>Xbox Live SDK de extensiones de plataforma</td>
  </tr>
  <tr>
    <td>Almacenamiento conectado</td>
    <td>Windows::Xbox::Storage</td>
    <td>Windows::Gaming::XboxLive::Storage</td>
    <td>Xbox Live SDK de extensiones de plataforma</td>
  </tr>
</table>

### <a name="multiplayer-subscriptions-and-event-handling"></a>Control de eventos y suscripciones multijugador

Uno de los últimos cambios de XSAPI 1.0 a 2.0 XSAPI que se encontrará con varios jugadores más títulos es el traspaso de varios métodos y eventos desde el **RealTimeActivityService** a la **MultiplayerService**.

Por ejemplo:

-   **EnableMultiplayerSubscriptions()\***  (método)

-   **DisableMultiplayerSubscriptions()** (método)

-   **MultiplayerSessionChanged** eventos

-   **MultiplayerSubscriptionLost** eventos

-   **MultiplayerSubscriptionsEnabled** propiedad

**Nota importante implementación** , aunque puede que no esté explícitamente usando cualquier otra cosa en el **RealTimeActivityService** después de mover estos eventos y métodos a la **MultiplayerService**, debe seguir llamando a **xblContext -&gt;RealTimeActivityService -&gt;Activate()** antes de llamar a **EnableMultiplayerSubscriptions()** Dado que las suscripciones de varios jugadores requieren que el servicio de ATR.

## <a name="whats-handled-differently-in-uwp"></a>¿Qué se tratan de forma diferente en UWP

Siguiente es una lista muy alto nivel de secciones de código que probablemente tendrá las diferencias entre el XDK y UWP, como se ha encontrado en el nuevo [NetRumble ejemplo](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx) (que incluye versiones XDK y UWP):

-   Acceda a información de Id. y ¿SCID título

-   Activación de inicio previo (nueva en UWP)

-   Control de suspensiones y reanudaciones PLM

-   Ejecución extendida (nueva en UWP)

-   Xbox **usuario** objeto y las diferencias en el control de usuario

    -   Control de inicio de sesión y cierre de sesión

    -   Controlador de emparejamiento (solo administrado en Xbox)

    -   Control de gamepad

-   Comprobación de privilegios multijugador

-   Compatibilidad con varios jugadores entre Xbox One y PC

-   Enviar invitaciones de juegos

    -   Capacidad de abrir la aplicación de terceros en juego - n/d en UWP

    -   Capacidad para enumerar a los miembros de entidad de juego - n/d en UWP

-   Que muestra el perfil de jugador

-   Proteger los cambios de superficie de API de Socket

-   Iniciación de la medida de calidad de servicio y el control de resultados

-   Escribir eventos de juegos

-   GameChat: objeto ChatUser, configuración y eventos

-   Conectado a los cambios de superficie de API de almacenamiento

-   Eventos de PIX (sólo en Xbox; no se tratan en este artículo técnico)

-   Algunas diferencias de representación

En las secciones siguientes incluyen más detalles en muchas de estas diferencias.

### <a name="accessing-title-id-and-scid-info"></a>Acceda a información de Id. y ¿SCID título

En UWP, el identificador de título y el Id. de configuración de servicio se accede a través de la propiedad de AppConfig en una instancia de un **XboxLiveContext**.

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

**Tenga en cuenta** en XDK, puede obtener estos identificadores con estas nuevas propiedades o las propiedades estáticas antiguas en **Windows::Xbox::Services::XboxLiveConfiguration**.

### <a name="prelaunch-activation"></a>Activación de inicio previo

Es posible que se realizó un inicio previo títulos usados con frecuencia en Windows 10 cuando el usuario inicia sesión. Para manejar esta situación, el título debe tener código que comprueba los argumentos de inicio para **PreLaunchActivated**. Por ejemplo, probablemente no desea cargar todos los recursos durante este tipo de activación. Para obtener más información, consulte el artículo de MSDN [identificador de aplicación de inicio previo](https://msdn.microsoft.com/library/windows/apps/mt593297.ASPx).

### <a name="suspendresume-plm-handling"></a>Control de suspensiones y reanudaciones PLM

Suspender y reanudar y PLM en general, funcionan de forma similar en una aplicación Windows Universal para la forma en que trabajan en Xbox One; Sin embargo, hay algunas diferencias importantes que tener en cuenta:

-   No hay ningún **Constrained** estado en el equipo, este es un concepto exclusivo Xbox One.

-   Suspender comienza inmediatamente cuando se minimiza el título; de alguna forma de hacerlo, consulte la sección [extendido de ejecución](#_Extended_execution).

-   El tiempo es diferente: tiene 5 segundos a suspender en un equipo en lugar de 1 segundo en la consola.

Otra consideración importante si usa almacenamiento conectado es el nuevo **ContainersChangedSinceLastSync** propiedad en la versión UWP de esta API. Al controlar un evento de reanudación, puede comprobar esta propiedad si cualquier contenedor ha cambiado en la nube mientras se suspendió su título. Esto puede ocurrir si el jugador suspende el juego en un equipo, reproduce en otro lugar y, a continuación, se devuelve al primer equipo. Si se hubiera leído datos de estos contenedores en la memoria antes de que haya tenido suspendido, probablemente desee leerlas de nuevo para ver qué cambió y controlar los cambios según corresponda.

Para obtener más información sobre el control de PLM en una aplicación para UWP en Windows 10, consulte el artículo de MSDN [Launching, resuming y tareas en segundo plano](https://msdn.microsoft.com/library/windows/apps/xaml/mt227652.aspx).

También puede encontrar el [PLM para Xbox One](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx) notas del producto en GDN útil porque se escribió con juegos en mente, y la mayoría de los conceptos para controlar el ciclo de vida de la aplicación todavía se aplica en un equipo.

<a name="_Extended_execution"></a>

### <a name="extended-execution"></a>Ejecución extendida

Minimizar una UWP app en un equipo normalmente da como resultado que comenzar inmediatamente a suspender. Mediante el uso extendida ejecución, tiene la oportunidad de retraso de este proceso. Ejemplo de implementación:

```cpp
using namespace Windows::ApplicationModel::ExtendedExecution;
//If this goes out of scope the request is nullified
ExtendedExecutionSession^ session;
void App::RequestExtension()
{
       if (!session)
       {
              session = ref new ExtendedExecutionSession();
       }
       session->Reason = ExtendedExecutionReason::Unspecified;
       session->Description = "foo";
       session->Revoked += ref new TypedEventHandler<Platform::Object^, ExtendedExecutionRevokedEventArgs^>(this, &App::ExtensionRevokedHandler);
       IAsyncOperation<ExtendedExecutionResult>^ request = session->RequestExtensionAsync();
       //At this point the request has been made. When the IAsyncOperation request completes, verify that the ExtendedExecutionResult == Allowed and you will not suspend for up to 10 minutes while minimized.
}

void App::ExtensionRevokedHandler(Platform::Object^ obj, ExtendedExecutionRevokedEventArgs^ args)
{
       if (args->Reason == Windows::ApplicationModel::ExtendedExecutionRevokedReason::Resumed)
       {
              //Request the extension again in preparation for the next suspend.
RequestExtension();
       }
       //The app will either complete suspending if the extension was revoked by system policy or resume running if the user has switched back to the app.
}

```

Después de la **ExtensionRevokedHandler** ha sido llamado, una nueva extensión debe solicitarse para futuras suspensiones posibles. El **ExtensionRevokedHandler** se llama cuando hay presión de memoria en el sistema, han transcurrido 10 minutos o el usuario vuelve a cambiar a la partida mientras se minimiza el juego. Por lo tanto **RequestExtension()** probablemente debe llamarse en estos momentos:

-   Durante el inicio.

-   En el **ExtensionRevokedHandler** cuando args -&gt;motivo == reanudado (es decir, el usuario con pestañas mientras el juego estaba minimizado antes de que caduque el temporizador de 10 minutos).

-   En el **OnResuming** controlador (si el título se ha suspendido debido a presión de memoria o el temporizador de 10 minutos).

### <a name="handling-users-and-controllers"></a>Control de los usuarios y los controladores

En Windows, se trabaja con un usuario que inició sesión a la vez. En el SDK de Xbox Live, cree primero un **XboxLiveUser** objeto, inicie sesión en Xbox Live y, a continuación, crear **XboxLiveContext** objetos desde este usuario.

Antes, en el una XDK Xbox:

1.  Adquirir un usuario (desde la interacción del controlador para juegos, por ejemplo).
2.  Crear un **XboxLiveContext** de dicho usuario:
  ```
  ref new Microsoft::Xbox::Services::XboxLiveContext( Windows::Xbox::System::User^ user )
  ```
1.  Controlar un **SignOut** eventos:
  ```
  Windows::Xbox::System::User::SignOutStarted
  ```
1.  Controlar mediante el emparejamiento de gamepad/controlador:
  ```
  Windows::Xbox::Input::Controller::ControllerRemoved
  Windows::Xbox::Input::Controller::ControllerPairingChanged
  ```

Ahora, para UWP o Xbox Live SDK:

1.  Crear un **XboxLiveUser**:

  ```
  auto xblUser = ref new Microsoft::Xbox::Services::System::XboxLiveUser();
  ```

1.  Intente iniciar sesión con la última cuenta de Microsoft utiliza, sin tener que ellos lidiar con la interfaz de usuario:

  ```
  xblUser->SignInSilentlyAsync();
  ```

1.  Si obtiene **SignInResult::Success** en el resultado de esta operación asincrónica, cree el **XboxLiveContext**, y, a continuación, habrá terminado:

  ```
  auto xblContext = ref new Microsoft::Xbox::Services::XboxLiveContext( xblUser );
  ```

1.  Si en su lugar obtiene **SignInResult::UserInteractionRequired**, deberá llamar al método de inicio de sesión interactivo que abre la interfaz de usuario del sistema:

  ```
  xblUser->SignInAsync();
  ```

1.  Desde aquí puede obtener **SignInResult::UserCancel**, en cuyo caso no tiene un usuario con sesión iniciada y debe considerar que proporciona una opción de menú para que puedan intentar iniciar sesión.

  **Tenga en cuenta** al proporcionar opciones de menú, es una buena idea para darles la opción para cambiar a otra cuenta de Microsoft:

  ```
  xblUser->SwitchAccountAsync( nullptr );
  ```

1.  Una vez que un usuario con sesión iniciada, es posible que desee enlazar el **XboxLiveUser::SignOutCompleted** eventos para que puedan reaccionar al cierre de sesión de usuario:

  ```
  xblUser->SignOutCompleted += ref new Windows::Foundation::EventHandler<Microsoft::Xbox::Services::System::SignOutCompletedEventArgs^>( &OnSignOutCompleted );
  ```

1.  No hay ningún controlador de emparejamiento para controlar en Windows 10.

Esto es un ejemplo simplificado de C++ / WinRT. Para obtener un ejemplo más detallado, vea "Xbox Live autenticación de Windows 10" en la Guía de programación de Xbox Live. También puede encontrar el ejemplo más amplio en "Agregar Xbox Live a un nuevo proyecto UWP" útil.

### <a name="checking-multiplayer-privileges"></a>Comprobación de privilegios multijugador

El equivalente a **CheckPrivilegeAsync()** aún no está disponible en el SDK de Xbox Live. Por ahora, deberá buscar el privilegio que necesita en la lista de cadenas devuelta por la **privilegios** propiedad para un **XboxLiveUser**. Por ejemplo, para comprobar los privilegios para varios jugadores, busque el privilegio "254." Usar la documentación de XDK, puede encontrar una lista de todos los privilegios de Xbox Live en los **Windows::Xbox::ApplicationModel::Store::KnownPrivileges** enumeración.

Para obtener una explicación sobre este tema, vea el comentario del foro [xsapi & usuario privilegios](https://forums.xboxlive.com/questions/48513/xsapi-user-privileges.html).

<a name="_Supporting_multiplayer_cross-play"></a>

### <a name="supporting-multiplayer-cross-play-between-xbox-one-and-pc-uwp"></a>Compatibilidad entre jugadores entre Xbox One y UWP de PC

Además de los requisitos de la plantilla de nueva sesión en XDP (consulte [cómo instalar y configurar el proyecto en el centro de partners y XDP](#_Setting_up_and)), entre-play viene con nuevas restricciones de capacidad de combinación de sesión. Ya no se puede usar "None" como una restricción de combinación de la sesión. Debe usar "Seguidos" o "Local" (la restricción predeterminada es "Local").

Además, las restricciones de lectura y la combinación de forma predeterminada "Local" a causa de los necesarios **userAuthorizationStyle** capacidad para varios jugadores de Windows 10.

En este artículo de foro, [es posible crear una sesión de varios jugadores pública](https://forums.xboxlive.com/questions/46781/is-it-possible-to-create-public-multiplayer-sessio.html), contiene información adicional.

Pueden encontrarse más información y ejemplos en los diagramas de flujo de desarrollador multijugador actualizado, el ejemplo multijugador habilitado entre-play NetRumble, o su administrador de cuenta de desarrollador (DAM).

<a name="_Sending_and_receiving"></a>

### <a name="sending-and-receiving-invites"></a>Enviar y recibir invitaciones

La API para que aparezca la interfaz de usuario para enviar invitaciones es ahora **Microsoft:: Xbox::Services::System::TitleCallableUI::ShowGameInviteUIAsync()**. Se pasa en una sesión -&gt; **SessionReference** objeto desde la sesión de actividad (normalmente en el vestíbulo). También puede pasar en un segundo parámetro que referencias personalizada invitar Id. de cadena que se han definido en la configuración del servicio en XDP. La cadena que defina existe aparecerán en la notificación del sistema que se envían a los encargados de invitado. Tenga en cuenta que lo que se pasa como un parámetro a este método es el número de identificación y debe estar formateado correctamente para el servicio. Por ejemplo, Id. de la cadena "1" debe pasarse como "/ / / 1".

Si desea enviar invitaciones directamente mediante el servicio de varios jugadores (es decir, sin mostrar ninguna interfaz de usuario), todavía puede usar el método de invitación, **Microsoft:: Xbox::Services::Multiplayer::MultiplayerService::SendInvitesAsync()** del usuario **XboxLiveContext**.

Para permitir invitaciones que entran en Windows para el título de protocolo activar, deberá agregar esta extensión a la **&lt;aplicación&gt;** elemento en el appxmanifest:

```xml
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

A continuación, puede controlar la invitación como hizo antes en Xbox One cuando su **CoreApplication** Obtiene un **Activated** eventos y la activación es de tipo una **ActivationKind::Protocol**.

### <a name="showing-the-gamer-profile-card"></a>Mostrando el jugador tarjeta de perfil

Para extraer la tarjeta de perfil de jugadores en UWP, use **Microsoft:: Xbox::Services::System::TitleCallableUI::ShowProfileCardUIAsync()**, pasando el XUID para el usuario de destino.

<a name="_Secure_sockets"></a>

### <a name="secure-sockets"></a>Sockets seguros

La API de Socket seguro se incluye en el separar [extensiones de SDK de plataforma de Xbox Live](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx).

Consulte este foro para el uso de la API: [Configurar SecureDeviceAssociation para todas las plataformas](https://forums.xboxlive.com/answers/45722/view.html).

**Tenga en cuenta** para UWP, la **SocketDescriptions** sección se ha movido el appxmanifest y a su propio [networkmanifest.xml](https://forums.xboxlive.com/storage/attachments/410-networkmanifestxml.txt). El formato dentro del &lt;SocketDescriptions&gt; elemento es prácticamente idéntico, pero sin la **mx:** prefijo.

Cross-play entre Xbox y Windows 10, ser *seguro* que todo lo que se define *exactamente igual* entre los dos tipos diferentes de manifiestos (Package.appxmanifest para Xbox One y networkmanifest.xml para Windows 10). Debe coincidir con el nombre de socket, protocolo, etc. *exactamente*.

También de entre-play, deberá definir los siguientes usos SDA cuatro dentro de la ```<AllowedUsages>``` elemento *ambos* Package.appxmanifest una Xbox y el networkmanifest.xml de Windows 10:

```xml
<SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
<SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
```

### <a name="multiplayer-qos-measurements"></a>Mediciones de varios jugadores QoS

Además del cambio de espacio de nombres de la API de Sockets seguros, algunos de los nombres de objeto y los valores han cambiado, demasiado. La asignación para el estado de la medida que se utiliza normalmente se encuentra en la tabla siguiente.

Tabla 2. Asignación de estado de la medida se utiliza normalmente.

| XDK (Windows::Xbox::Networking::QualityOfServiceMeasurementStatus)  | UWP (Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurementStatus)  |
|------------------------------------|--------------------------------------------|
| HostUnreachable                    | NoCompatibleNetworkPaths                   |
| MeasurementTimedOut                | Tiempo de espera agotado                                   |
| PartialResults                     | InProgressWithProvisionalResults           |
| Correcto                            | Correcto                                  |

Los pasos implicados en *medir* QoS (calidad de servicio) y *procesar los resultados* son en principio iguales al comparar las versiones XDK y UWP de la API. Sin embargo, debido a los cambios de nombre y algunos cambios de diseño, el código resultante tiene un aspecto diferente en algunos lugares.

Para medir la calidad de servicio para el **XDK**, ha creado una colección de direcciones del dispositivo seguro y una colección de métricas y pasa estos elementos en el **MeasureQualityOfServiceAsync()** método.

Para medir la calidad de servicio para **UWP**, se crea un nuevo **XboxLiveQualityOfServiceMeasurement()** de objeto, llame a **Append()** a su **métricas** y **DeviceAddresses** propiedades y, a continuación, llame el objeto **MeasureAsync()** método.

Por ejemplo:

```cpp
auto qosMeasurement = ref new Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurement();
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageInboundBitsPerSecond);
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageOutboundBitsPerSecond);
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageLatencyInMilliseconds);
qosMeasurement->NumberOfProbesToAttempt = myDefaultQosProbeCount;
qosMeasurement->TimeoutInMilliseconds = myDefaultQosMeasurementTimeout;

// Add secure addresses for each session member
for (const auto& member : session->GetMembers())
{
    if (!member->IsCurrentUser)
    {
        auto sda = member->SecureDeviceAddressBase64;

        if (!sda->IsEmpty())
        {
qosMeasurement->DeviceAddresses->Append(Windows::Networking::XboxLive::XboxLiveDeviceAddress::CreateFromSnapshotBase64(sda));
        }
    }
}

if (qosMeasurement->DeviceAddresses->Size > 0)
{
    qosMeasurement->MeasureAsync();
}

```

Para obtener más ejemplos, vea el **MatchmakingSession::MeasureQualityOfService()** y **MatchmakingSession::ProcessQosMeasurements()** funciones en el ejemplo NetRumble.

### <a name="writing-game-events"></a>Escribir eventos de juegos

Envío de eventos de juegos que están configurados en servicio de configuración del título tiene una API diferente en UWP. Usa el SDK de Xbox Live el **EventsService** y un modelo de la bolsa de propiedades.

Por ejemplo:

```cpp
auto properties = ref new Windows::Foundation::Collections::PropertySet();
properties->Insert("RoundId", m_roundId);
properties->Insert("SectionId", safe_cast<Platform::Object^>(0));
properties->Insert("MultiplayerCorrelationId", m_multiplayerCorrelationId);
properties->Insert("GameplayModeId", safe_cast<Platform::Object^>(0));
properties->Insert("MatchTypeId", safe_cast<Platform::Object^>(0));
properties->Insert("DifficultyLevelId", safe_cast<Platform::Object^>(0));

auto measurements = ref new Windows::Foundation::Collections::PropertySet();

xblContext->EventsService->WriteInGameEvent("MultiplayerRoundStart", properties, measurements);

```

Para obtener más información, consulte la documentación del SDK de Xbox Live.

**Sugerencia** puede usar el **xcetool.exe** proporcionado con el SDK de Xbox Live (ubicado en el directorio de herramientas) para convertir el archivo events.man que descargó desde XDP en un archivo .h de encabezado. Utilice el '-x' opción para generar este encabezado de C++ con el nuevo esquema de bolsa de propiedades de v2. Este encabezado contiene las funciones de C++ que se pueden llamar para todos los eventos configurados; Por ejemplo, **EventWriteMultiplayerRoundStart()**. Si prefiere utilizar una interfaz de WinRT, todavía puede hacer referencia a este archivo de encabezado para ver cómo se construyen las propiedades y medidas para cada uno de los eventos.

### <a name="game-chat"></a>Chat de juegos

GameChat en UWP se incluye con el SDK de Xbox Live como un paquete de NuGet binario. Consulte las instrucciones en la Guía de programación Xbox Live para saber cómo agregar este paquete de NuGet al proyecto.

Uso básico es prácticamente idéntica entre el XDK y las versiones UWP. Algunas diferencias en la API incluyen:

1.  El **User::AudioDeviceAdded** evento no necesita enlazarse un título UWP. El dispositivo subyacente de identificadores de biblioteca de chat agrega y quita.

2.  **ChatUser** ahora se denomina **GameChatUser**.

3.  **Microsoft::Xbox::GameChat** espacio de nombres sigue siendo el mismo, pero la **Windows::Xbox::Chat** ha convertido en el espacio de nombres **Microsoft::Xbox::ChatAudio**.

4.  **AddLocalUserToChatChannelAsync()** toma bien un XUID o un **ChatAudio::IChatUser ^** en lugar de un **XboxUser**.

5.  **RemoveLocalUserFromChatChannelAsync()** requiere un **ChatAudio::IChatUser ^** en lugar de un **XboxUser**. Puede obtener un **IChatUser** desde un **GameChatUser**-&gt;**usuario**.

### <a name="connected-storage"></a>Almacenamiento conectado

Se proporciona la API de almacenamiento conectado en el separar [extensiones de SDK de plataforma de Xbox Live](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx). Documentación está incluida en la documentación del SDK de Xbox Live.

El flujo general es el mismo que en Xbox One, con la adición de la **ContainersChangedSinceLastSync** propiedad en la versión de UWP. Esta propiedad se debe comprobar cuando el título de la controla un evento de reanudación, después de llamar a **GetForUserAsync()** nuevo, se ha suspendido ver qué contenedores cambiado en la nube mientras su título. Si tiene datos cargados en memoria desde uno de los contenedores que ha cambiado, probablemente desee leer los datos de nuevo para ver qué cambió y controlar los cambios según corresponda.

Otras diferencias notables en la versión de UWP se incluyen:

1.  Cambio Namespace desde **Windows::Xbox::Storage** a **Windows::Gaming::XboxLive::Storage**.

2.  **ConnectedStorageSpace** se cambia el nombre **GameSaveProvider**.

3.  **Windows::System::User** se usa en **GetForUserAsync()** en lugar de un **XboxUser**, y el ¿SCID ahora es necesario.

4.  No hay almacenamiento local "automático" (es decir, **GetForMachineAsync()** se ha quitado). Considere el uso de **Windows::Storage::ApplicationData** en su lugar para los no móvil, local guardar los datos.

5.  Async resultados se devuelven en una sin excepciones \*el objeto de tipo de resultado (por ejemplo, **GameSaveProviderGetResult**); de esto puede comprobar el **estado** propiedad, y si no se produce ningún error, leer el objeto devuelto desde el **valor** propiedad.

6.  **Enumeración ConnectedStorageErrorStatus** se cambia el nombre **GameSaveErrorStatus** y se devuelve en el **estado** propiedad de un resultado. Existen todos los valores anteriores y se agregaron algunos nuevos:

-   Anular

-   ObjectExpired

-   Aceptar

-   UserHasNoXboxLiveInfo

Consulte el ejemplo GameSave o en el ejemplo NetRumble por ejemplo uso.

**Tenga en cuenta** Gamesaveutil.exe equivale a xbstorage.exe (la utilidad de línea de comandos de desarrollador incluida con el XDK). Después de instalar el SDK de extensiones de Xbox Live plataforma, esta utilidad puede encontrarse aquí: C:\\(x86) de archivos de programa\\Windows Kits\\10\\SDK de extensión\\XboxLive\\1.0\\Bin\\x64

## <a name="summary"></a>Resumen

Los cambios de API y los nuevos requisitos que se describen en este documento son las que es probable que encuentre al migrar código existente de juegos desde el XDK una Xbox a UWP nuevo. Se ha proporcionado el énfasis especial para la aplicación y configuración del entorno, así como las áreas de características relacionadas con servicios de Xbox Live, como el almacenamiento de varios jugadores y está conectado. Para obtener más información, siga los vínculos proporcionados en este artículo y en las siguientes referencias y no olvide visitar la sección "Windows 10" de la [foros para desarrolladores](https://forums.xboxlive.com) para obtener más ayuda, respuestas y noticias.

## <a name="references"></a>Referencias

-   [Portabilidad de Xbox One a Windows 10](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx)

-   [Notas de la Xbox One](https://developer.xboxlive.com/en-us/platform/development/education/Pages/WhitePapers.aspx)

-   [Ejemplos](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx)
