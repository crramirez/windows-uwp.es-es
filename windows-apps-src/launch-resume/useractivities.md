---
title: Continuar la actividad del usuario, incluso en diferentes dispositivos
description: En este tema se describe cómo ayudar a los usuarios a reanudar lo que hacían en la aplicación, incluso en varios dispositivos.
keywords: actividad de usuario, actividades de usuario, escala de tiempo, la recogida de Cortana en la que se quedó, la recogida de Cortana en la que se quedó, Project Roma
ms.date: 04/27/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ebaca3b831ae30637a88d01319a89d139dde1cf8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162629"
---
# <a name="continue-user-activity-even-across-devices"></a>Continuar la actividad del usuario, incluso en diferentes dispositivos

En este tema se describe cómo ayudar a los usuarios a reanudar lo que hacían en la aplicación en su equipo y en todos los dispositivos.

## <a name="user-activities-and-timeline"></a>Actividades de usuario y escala de tiempo

Nuestra hora cada día se distribuye entre varios dispositivos. Podríamos usar el teléfono mientras está en el bus, un equipo durante el día, un teléfono o una tableta por la noche. A partir de la compilación 1803 de Windows 10 o superior, la creación de una [actividad de usuario](/uwp/api/windows.applicationmodel.useractivities.useractivity) hace que esa actividad aparezca en la escala de tiempo de Windows y en la recogida de Cortana en la que se dejó la característica. La escala de tiempo es una vista de tareas enriquecida que aprovecha las ventajas de las actividades de usuario para mostrar una vista cronológica de lo que ha estado trabajando. También puede incluir lo que estaba trabajando en todos los dispositivos.

![Imagen de la escala de tiempo de Windows](images/timeline.png)

Del mismo modo, la vinculación del teléfono a su PC Windows le permite continuar lo que estaba haciendo anteriormente en el dispositivo iOS o Android.

Piense en un **UserActivity** como algo específico en el que estaba trabajando el usuario en la aplicación. Por ejemplo, si usa un lector RSS, un **UserActivity** podría ser la fuente que está leyendo. Si juega un juego, el **UserActivity** podría ser el nivel que está reproduciendo. Si está escuchando una aplicación de música, el **UserActivity** podría ser la lista de reproducción a la que está escuchando. Si está trabajando en un documento, el **UserActivity** podría ser el lugar en el que dejó de trabajar en él, etc.  En Resumen, un **UserActivity** representa un destino dentro de la aplicación para que permita al usuario reanudar lo que estaba haciendo.

Al interactuar con un **UserActivity** llamando a [UserActivity. createSession](/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession), el sistema crea un registro de historial que indica la hora de inicio y de finalización de ese **UserActivity**. A medida que vuelva a interactuar con ese **UserActivity** con el tiempo, se registrarán varios registros de historial.

## <a name="add-user-activities-to-your-app"></a>Incorporación de actividades de usuario a la aplicación

Un [UserActivity](/uwp/api/windows.applicationmodel.useractivities.useractivity) es la unidad de compromiso de usuario en Windows. Tiene tres partes: un URI que se usa para activar la aplicación a la que pertenece la actividad, los objetos visuales y los metadatos que describen la actividad.

1. [ActivationUri](/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri#Windows_ApplicationModel_UserActivities_UserActivity_ActivationUri) se usa para reanudar la aplicación con un contexto específico. Normalmente, este vínculo adopta la forma de controlador de protocolo para un esquema (por ejemplo, "My-App://Page2? action = edit") o de un AppUriHandler (por ejemplo, http://constoso.com/page2?action=edit) .
2. [VisualElements](/uwp/api/windows.applicationmodel.useractivities.useractivity.visualelements) expone una clase que permite al usuario identificar visualmente una actividad con un título, una descripción o elementos de tarjeta adaptativa.
3. Por último, el [contenido](/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.content#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_Content) es donde puede almacenar los metadatos de la actividad que se puede usar para agrupar y recuperar las actividades en un contexto específico. A menudo, esto toma la forma de [https://schema.org](https://schema.org) datos.

Para agregar un **UserActivity** a la aplicación:

1. Generar objetos **UserActivity** cuando el contexto del usuario cambie dentro de la aplicación (como la navegación de páginas, el nuevo nivel de juego, etc.)
2. Rellene los objetos **UserActivity** con el conjunto mínimo de campos obligatorios: [ActivityID](/uwp/api/windows.applicationmodel.useractivities.useractivity.activityid#Windows_ApplicationModel_UserActivities_UserActivity_ActivityId), [ActivationUri](/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri)y [UserActivity. VisualElements. DisplayText](/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.displaytext#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_DisplayText).
3. Agregue un controlador de esquema personalizado a la aplicación para que se pueda volver a activar mediante un **UserActivity**.

Un **UserActivity** se puede integrar en una aplicación con solo unas pocas líneas de código. Por ejemplo, Imagine este código en MainPage.xaml.cs, dentro de la clase MainPage (Nota: se supone que `using Windows.ApplicationModel.UserActivities;` ):

```csharp
UserActivitySession _currentActivity;
private async Task GenerateActivityAsync()
{
    // Get the default UserActivityChannel and query it for our UserActivity. If the activity doesn't exist, one is created.
    UserActivityChannel channel = UserActivityChannel.GetDefault();
    UserActivity userActivity = await channel.GetOrCreateUserActivityAsync("MainPage");
 
    // Populate required properties
    userActivity.VisualElements.DisplayText = "Hello Activities";
    userActivity.ActivationUri = new Uri("my-app://page2?action=edit");
     
    //Save
    await userActivity.SaveAsync(); //save the new metadata
 
    // Dispose of any current UserActivitySession, and create a new one.
    _currentActivity?.Dispose();
    _currentActivity = userActivity.CreateSession();
}
```

La primera línea del `GenerateActivityAsync()` método anterior obtiene el [UserActivityChannel](/uwp/api/windows.applicationmodel.useractivities.useractivitychannel)de un usuario. Esta es la fuente en la que se publicarán las actividades de esta aplicación. La línea siguiente consulta el canal de una actividad denominada `MainPage` .

* La aplicación debe nombrar las actividades de tal forma que se genere el mismo identificador cada vez que el usuario esté en una ubicación determinada de la aplicación. Por ejemplo, si la aplicación está basada en páginas, use un identificador de la página; Si su documento se basa, use el nombre del documento (o un hash del nombre).
* Si hay una actividad existente en la fuente con el mismo identificador, esa actividad se devolverá desde el canal con `UserActivity.State` establecido en [publicado](/uwp/api/windows.applicationmodel.useractivities.useractivitystate)). Si no hay ninguna actividad con ese nombre, se devuelve una nueva actividad con `UserActivity.State` establecido en **New**.
* Las actividades se limitan a la aplicación. No es necesario preocuparse por el ID. de actividad en conflicto con los identificadores de otras aplicaciones.

Después de obtener o crear el **UserActivity**, especifique los otros dos campos obligatorios:  `UserActivity.VisualElements.DisplayText` y `UserActivity.ActivationUri` .

A continuación, guarde los metadatos de **UserActivity** llamando a [SaveAsync](/uwp/api/windows.applicationmodel.useractivities.useractivity.saveasync)y, por último, [createSession](/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession), que devuelve una [UserActivitySession](/uwp/api/windows.applicationmodel.useractivities.useractivitysession). **UserActivitySession** es el objeto que se puede usar para administrar cuando el usuario está participando en el **UserActivity**. Por ejemplo, debemos llamar a `Dispose()` en el **UserActivitySession** cuando el usuario abandona la página. En el ejemplo anterior, también llamamos a `Dispose()` en `_currentActivity` antes de llamar a `CreateSession()` . Esto se debe a que se ha hecho `_currentActivity` un campo de miembro de nuestra página y se desea detener cualquier actividad existente antes de iniciar el nuevo (Nota: `?` es el [operador condicional null,](/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-) que comprueba si el valor es NULL antes de realizar el acceso a miembros).

Puesto que, en este caso, `ActivationUri` es un esquema personalizado, también es necesario registrar el protocolo en el manifiesto de aplicación. Esto se hace en el archivo XML package. AppManifest o mediante el diseñador.

Para hacer el cambio con el diseñador, haga doble clic en el archivo package. AppManifest del proyecto para iniciar el diseñador, seleccione la pestaña **declaraciones** y agregue una definición de **Protocolo** . La única propiedad que se debe rellenar, por ahora, es **Name**. Debe coincidir con el URI especificado anteriormente, `my-app` .

Ahora es necesario escribir código para indicar a la aplicación qué hacer cuando se activa mediante un protocolo. Reemplazaremos el `OnActivated` método en App.Xaml.CS para pasar el URI a la Página principal, como por ejemplo:

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        var uriArgs = e as ProtocolActivatedEventArgs;
        if (uriArgs != null)
        {
            if (uriArgs.Uri.Host == "page2")
            {
                // Navigate to the 2nd page of the  app
            }
        }
    }
    Window.Current.Activate();
}
```

Lo que hace este código es detectar si la aplicación se activó a través de un protocolo. En caso de ser así, se busca para ver lo que debe hacer la aplicación para reanudar la tarea para la que se está activando. Al ser una aplicación sencilla, la única actividad que esta aplicación reanuda es colocarlo en la página secundaria cuando la aplicación aparece.

## <a name="use-adaptive-cards-to-improve-the-timeline-experience"></a>Usar tarjetas adaptables para mejorar la experiencia de la escala de tiempo

Las actividades de usuario aparecen en Cortana y escala de tiempo. Cuando las actividades aparecen en la escala de tiempo, se muestran mediante el marco de trabajo de [tarjeta adaptable](https://adaptivecards.io/) . Si no proporciona una tarjeta adaptable para cada actividad, la escala de tiempo creará automáticamente una tarjeta de actividad simple basada en el nombre de la aplicación y el icono, el campo de título y el campo de descripción opcional. A continuación se muestra un ejemplo de carga de tarjeta adaptativa y la tarjeta que genera.

![Una tarjeta adaptable](images/adaptivecard.png)]

Ejemplo de cadena JSON de carga adaptativa de tarjeta:

```json
{ 
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json", 
  "type": "AdaptiveCard", 
  "version": "1.0",
  "backgroundImage": "https://winblogs.azureedge.net/win/2017/11/eb5d872c743f8f54b957ff3f5ef3066b.jpg", 
  "body": [ 
    { 
      "type": "Container", 
      "items": [ 
        { 
          "type": "TextBlock", 
          "text": "Windows Blog", 
          "weight": "bolder", 
          "size": "large", 
          "wrap": true, 
          "maxLines": 3 
        }, 
        { 
          "type": "TextBlock", 
          "text": "Training Haiti’s radiologists: St. Louis doctor takes her teaching global", 
          "size": "default", 
          "wrap": true, 
          "maxLines": 3 
        } 
      ] 
    } 
  ]
}
```

Agregue la carga de tarjetas adaptables como una cadena JSON a **UserActivity** similar a la siguiente:

```csharp
activity.VisualElements.Content = 
Windows.UI.Shell.AdaptiveCardBuilder.CreateAdaptiveCardFromJson(jsonCardText); // where jsonCardText is a JSON string that represents the card
```

## <a name="cross-platform-and-service-to-service-integration"></a>Integración entre plataformas y servicio a servicio

Si la aplicación se ejecuta en varias plataformas (por ejemplo, en Android e iOS) o mantiene el estado del usuario en la nube, puede publicar UserActivities a través de [Microsoft Graph](https://developer.microsoft.com/graph).
Una vez que la aplicación o el servicio se autentica con una cuenta de Microsoft, solo toma dos llamadas REST simples para generar objetos de [actividad](/graph/api/resources/projectrome-activity) e [historial](/graph/api/resources/projectrome-historyitem) , con los mismos datos que los descritos anteriormente.

## <a name="summary"></a>Resumen

Puede usar la API [UserActivity](/uwp/api/windows.applicationmodel.useractivities) para que la aplicación aparezca en la escala de tiempo y Cortana.
* Más información sobre la [API de **UserActivity**](/uwp/api/windows.applicationmodel.useractivities)
* Consulte el [código de ejemplo](https://github.com/Microsoft/project-rome).
* Vea [tarjetas adaptables más sofisticadas](https://adaptivecards.io/).
* Publique un **UserActivity** desde iOS, Android o su servicio Web a través de [Microsoft Graph](https://developer.microsoft.com/graph).
* Obtenga más información sobre [Project Roma en github](https://github.com/Microsoft/project-rome).

## <a name="key-apis"></a>API clave

* [Espacio de nombres UserActivities](/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Temas relacionados

* [Actividades de usuario (documentos de proyecto de Roma)](/windows/project-rome/user-activities/)
* [Tarjetas adaptables](/adaptive-cards/)
* [Visualizador de tarjetas adaptables, ejemplos](https://adaptivecards.io/)
* [Controlar la activación de URI](./handle-uri-activation.md)
* [Interacción con sus clientes en cualquier plataforma mediante el Microsoft Graph, la fuente de actividades y las tarjetas adaptables](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph)