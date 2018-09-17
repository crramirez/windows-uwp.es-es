---
author: TylerMSFT
title: Continuar la actividad del usuario, incluso en diferentes dispositivos
description: En este tema, se describe cómo ayudar a los usuarios a reanudar lo que estaban haciendo en tu aplicación, incluso en varios dispositivos.
keywords: actividad del usuario, actividades del usuario, línea de tiempo, cortana continúa donde lo dejaste, cortana continúa desde donde lo dejé, proyecto rome
ms.author: twhitney
ms.date: 04/27/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 53aac2375d60df3cd9493f315b20431961378fe8
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "3987978"
---
# <a name="continue-user-activity-even-across-devices"></a>Continuar la actividad del usuario, incluso en diferentes dispositivos

En este tema, se describe cómo ayudar a los usuarios a reanudar lo que estaban haciendo en tu aplicación, ya sea en su PC o en varios dispositivos.

## <a name="user-activities-and-timeline"></a>Actividades y línea de tiempo de usuarios

Todos los días nuestro tiempo se reparte entre varios dispositivos. Podríamos usar nuestro teléfono mientras estamos en el autobús, un PC durante el día y, a continuación, un teléfono o una tableta por la noche. A partir de la compilación 1803 de Windows 10 o posterior, al crear una [actividad del usuario](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity), esa actividad aparece en la línea de tiempo de Windows y en la característica "continuar donde lo dejaste" de Cortana. La línea de tiempo es una vista de tareas enriquecida que aprovecha las ventajas de las actividades del usuario para mostrar una vista cronológica de aquello en lo que has estado trabajando. También puede incluir aquello en lo que estabas trabajando en diferentes dispositivos.

![Imagen de la línea de tiempo de Windows](images/timeline.png)

De igual modo, vincular el teléfono a tu PC Windows te permite continuar lo que estuvieras haciendo anteriormente en tu dispositivo iOS o Android.

Considera una **UserActivity** como algo específico en lo que ha estado trabajando el usuario en la aplicación. Por ejemplo, si estás usando un lector RSS, una **UserActivity** podría ser la fuente que estás leyendo. Si estás jugando a un juego, la **UserActivity** podría ser el nivel al que estás jugando. Si estás escuchando música con una aplicación de música, la **UserActivity** podría ser la lista de reproducción que estás escuchando. Si estás trabajando en un documento, la **UserActivity** podría ser el punto donde dejaste de trabajar en él, y así sucesivamente.  En resumen, una **UserActivity** representa un destino dentro de la aplicación que permite al usuario reanudar lo que estaba haciendo.

Cuando interactúas con una **UserActivity** llamando a [UserActivity.CreateSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession), el sistema crea un registro del historial que indica el tiempo de inicio y finalización de esa **UserActivity **. Cuando interactúas con esa **UserActivity** a lo largo del tiempo, se registran varios registros de historial para ella.

## <a name="add-user-activities-to-your-app"></a>Agregar actividades de usuario a la aplicación

Una [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) es la unidad de participación del usuario en Windows. Tiene tres partes: un URI que se usa para activar la aplicación a la que pertenece la actividad, elementos visuales y metadatos que describen la actividad.

1. La [ActivationUri](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri#Windows_ApplicationModel_UserActivities_UserActivity_ActivationUri) se usa para reanudar la aplicación con un contexto específico. Por lo general, este vínculo adopta la forma de un controlador de protocolo de un esquema (por ejemplo, "my-app://page2?action=edit") o de un AppUriHandler (por ejemplo, http://constoso.com/page2?action=edit).
2. [VisualElements](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.visualelements) expone una clase que permite al usuario identificar visualmente una actividad con un título, una descripción o elementos de la tarjeta adaptable.
3. Por último, [Content](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.content#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_Content) es donde puedes almacenar metadatos para la actividad que se pueden usar para agrupar y recuperar las actividades en un contexto específico. A menudo, esto adopta la forma de datos de [http://schema.org](http://schema.org).

Para agregar una **UserActivity** a la aplicación:

1. Genera objetos de **UserActivity** cuando cambie el contexto del usuario en la aplicación (por ejemplo, la navegación de una página, un nuevo nivel de juego, etc.)
2. Rellena los objetos de **UserActivity** con el conjunto mínimo de campos obligatorios: [ActivityId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activityid#Windows_ApplicationModel_UserActivities_UserActivity_ActivityId), [ActivationUri](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri) y [UserActivity.VisualElements.DisplayText ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.displaytext#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_DisplayText).
3. Agrega un controlador de esquema personalizado a tu aplicación para que se pueda volver a activar mediante una **UserActivity **.

Una **UserActivity** puede integrarse en una aplicación con tan solo unas pocas líneas de código. Por ejemplo, vamos a suponer que tenemos este código de MainPage.xaml.cs dentro de la clase MainPage (nota: se da por hecho `using Windows.ApplicationModel.UserActivities;`):

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

La primera línea del método `GenerateActivityAsync()`anterior obtiene de un [UserActivityChannel ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitychannel) del usuario. Esta es la fuente en la que se publicarán las actividades de la aplicación. La siguiente línea consulta el canal de una actividad denominada `MainPage`.

* La aplicación debe asignar nombres a las actividades de tal forma que se genere el mismo identificador cada vez que el usuario esté en una ubicación en particular en la aplicación. Por ejemplo, si la aplicación está basada en páginas, usa un identificador para la página. Si está basada en documentos, usa el nombre del documento (o un hash del nombre).
* Si hay una actividad en la fuente con el mismo identificador, se devolverá esa actividad desde el canal con `UserActivity.State` establecido en [Publicado](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitystate)). Si no hay ninguna actividad con ese nombre, se devuelve una nueva actividad con `UserActivity.State` establecido en **Nueva **.
* Las actividades tienen el ámbito de la aplicación. No es necesario preocuparse por que el identificador de la actividad entre en conflicto con los identificadores de otras aplicaciones.

Después de obtener o crear la **UserActivity**, especifica los otros dos campos necesarios: `UserActivity.VisualElements.DisplayText` y `UserActivity.ActivationUri`.

A continuación, guarda los metadatos de **UserActivity** mediante una llamada a [SaveAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.saveasync) y finalmente [CreateSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession), que devuelve una [UserActivitySession ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitysession). La **UserActivitySession** es el objeto que podemos usar para administrar cuando el usuario está realizando la **UserActivity **. Por ejemplo, deberíamos llamar a `Dispose()`en la **UserActivitySession** cuando el usuario abandona la página. En el ejemplo anterior, llamamos también a `Dispose()` en `_currentActivity` antes de llamar a `CreateSession()`. Esto es porque hemos convertido a `_currentActivity` en un campo miembro de nuestra página y queremos detener cualquier actividad existente antes de empezar una nueva (nota: la `?` es el operador [nulo condicional](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-conditional-operators) que comprueba si hay valores nulos antes de realizar el acceso del miembro).

Dado que, en este caso, el `ActivationUri`es un esquema personalizado, también necesitamos registrar el protocolo en el manifiesto de la aplicación. Esto se realiza en el archivo XML Package.appmanifest o mediante el diseñador.

Para realizar el cambio con el diseñador, haz doble clic en el archivo Package.appmanifest en el proyecto para iniciar el diseñador, selecciona la pestaña **Declaraciones** y agrega una definición de **protocolo**. La única propiedad que debe completarse por ahora es **Nombre**. Debe coincidir con el URI que se especificó anteriormente, `my-app`.

Ahora, debemos escribir código para indicar a la aplicación qué hacer cuando se ha activado mediante un protocolo. Invalidamos el método `OnActivated` en App.xaml.cs para pasar el URI en la página principal, de este modo:

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

Lo que hace este código es detectar si la aplicación se ha activado mediante un protocolo. Si es así, comprueba qué debe hacer la aplicación para reanudar la tarea para la que se está activando. Si es una aplicación sencilla, la única actividad que reanuda esta aplicación es llevarte a la página secundaria cuando aparece la aplicación.

## <a name="use-adaptive-cards-to-improve-the-timeline-experience"></a>Usar tarjetas adaptables para mejorar la experiencia de la línea de tiempo

Las actividades del usuario aparecen en Cortana y la línea de tiempo. Cuando las actividades aparecen en la línea de tiempo, las mostramos mediante el marco [Tarjeta adaptable](http://adaptivecards.io/). Si no proporcionas una tarjeta adaptable para cada actividad, la línea de tiempo crea automáticamente una tarjeta de actividad sencilla basada en el nombre y el icono de la aplicación, el campo de título y el campo de descripción opcional. A continuación se incluye un ejemplo de carga de tarjeta adaptable y la tarjeta que produce.

![Una tarjeta adaptable](images/adaptivecard.png)]

Cadena JSON de carga de tarjeta adaptable de ejemplo:

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

Agrega la carga de tarjetas adaptables como una cadena JSON para la **UserActivity**, de esta manera:

```csharp
activity.VisualElements.Content = 
Windows.UI.Shell.AdaptiveCardBuilder.CreateAdaptiveCardFromJson(jsonCardText); // where jsonCardText is a JSON string that represents the card
```

## <a name="cross-platform-and-service-to-service-integration"></a>Integración multiplataforma y servicio a servicio

Si la aplicación se ejecuta para varias plataformas (por ejemplo, en Android y iOS), o mantiene el estado de usuario en la nube, puedes publicar UserActivities a través de [Microsoft Graph](https://developer.microsoft.com/graph/).
Una vez que la aplicación o el servicio se autentica con una cuenta de Microsoft, solo se necesitan dos llamadas sencillas a REST para generar objetos de [actividad](https://developer.microsoft.com/graph/docs/api-reference/beta/api/projectrome_put_activity) e [historial](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/projectrome_historyitem) con los mismos datos que se han descrito anteriormente.

## <a name="summary"></a>Resumen

Puedes usar la API [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities) para que tu aplicación aparezca en la línea de tiempo y Cortana.
* Obtén más información acerca de la API **UserActivity** en el [Centro de desarrollo de Windows](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)
* Echa un vistazo al [código de ejemplo](https://github.com/Microsoft/project-rome).
* Consulta [tarjetas adaptables más sofisticadas](http://adaptivecards.io/).
* Publicar una **UserActivity** de iOS, Android o el servicio web a través de [Microsoft Graph](https://developer.microsoft.com/graph/).
* Más información sobre el [proyecto Rome en GitHub](https://github.com/Microsoft/project-rome).

## <a name="key-apis"></a>API de claves

* [Espacio de nombres UserActivities](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Temas relacionados

* [Actividades del usuario (documentos de Project Rome)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [Tarjetas adaptables](https://docs.microsoft.com/adaptive-cards/)
* [Visualizador de tarjetas adaptables, muestras](http://adaptivecards.io/)
* [Administración de la activación de URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Interactuar con los clientes en cualquier plataforma con Microsoft Graph, fuentes de actividades y tarjetas adaptables](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)