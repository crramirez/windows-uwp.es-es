---
author: Karl-Bridge-Microsoft
Description: "Obtén información sobre cómo puede interactuar un usuario con una aplicación en segundo plano mediante el lienzo y la voz de Cortana durante la ejecución de un comando de voz."
title: "Interactuar con una aplicación en segundo plano"
ms.assetid: 6C60F03C-A242-435D-96BB-736892CC1CA6
label: Interact with a background app
template: detail.hbs
ms.sourcegitcommit: 7d9f5eff0f6561b18024658fe99d1e11bbe3309f
ms.openlocfilehash: 675553f5c3954597982360900e965b2a756d7f63

---

# Interactuar con una aplicación en segundo plano en Cortana

Habilita la interacción del usuario con una aplicación en segundo plano, a través de voz y entrada de texto en el lienzo de **Cortana**, mientras se ejecuta un comando de voz.



**API importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Elementos y atributos de Definición de comando de voz (VCD) v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


Cortana es compatible con un flujo de trabajo completo paso a paso de la aplicación. Este flujo de trabajo está definido por la aplicación y puede admitir la funcionalidad, como: 

-   Finalización correcta
-   Entrega
-   Progreso
-   Confirmación
-   Desambiguación
-   Error

**Requisitos previos:**

Este tema se basa en [Iniciar una aplicación en segundo plano con los comandos de voz en Cortana](launch-a-background-app-with-voice-commands-in-cortana.md). A continuación, se muestran las funciones con una aplicación de administración y planificación de viajes llamada **Adventure Works**.

Si acabas de empezar a desarrollar aplicaciones de la Plataforma Universal Windows (UWP), consulta estos temas para familiarizarte con las tecnologías presentadas aquí.

-   [Crear tu primera aplicación](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Encontrarás más información acerca de los eventos, en [Introducción a eventos y eventos enrutados](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Directrices sobre la experiencia del usuario:  **

Consulta [Directrices para el diseño de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233) para obtener información sobre cómo combinar tu aplicación con **Cortana** y [Directrices para el diseño de voz](https://msdn.microsoft.com/library/windows/apps/dn596121) para obtener sugerencias prácticas y diseñar una aplicación habilitada para la voz que sea útil y atractiva.

## <span id="Feedback_strings"></span><span id="feedback_strings"></span><span id="FEEDBACK_STRINGS"></span>Cadenas de comentarios

Componer las cadenas de comentarios, que muestra y dice **Cortana**.

La [Directrices de diseño de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)proporcionan recomendaciones sobre cómo redactar cadenas para **Cortana**.

## <span id="Feedback_strings"></span><span id="feedback_strings"></span><span id="FEEDBACK_STRINGS"></span>Cadenas de comentarios

Las tarjetas de contenido pueden proporcionar contexto adicional para el usuario y ayudar a mantener las cadenas de comentarios concisas.

**Cortana** admite las siguientes plantillas de tarjetas de contenido (solo se puede usar una plantilla en la pantalla de finalización):

    -   Title only
    -   Title with up to three lines of text
    -   Title with image
    -   Title with image and up to three lines of text

La imagen puede ser:

    -   68w x 68h
    -   68w x 92h
    -   280w x 140h

También puedes permitir que los usuarios inicien la aplicación en primer plano haciendo clic en una tarjeta o en el vínculo de texto a tu aplicación.

## <span id="Completion_screen"></span><span id="completion_screen"></span><span id="COMPLETION_SCREEN"></span>Pantalla de finalización

Una pantalla de finalización ofrece información al usuario sobre la tarea de comando de voz completada.

Las tareas a las que la aplicación tarda menos de 500 en responder y no requieren información adicional del usuario, se completan sin más interacción con **Cortana**. Cortana tan solo muestra la pantalla de finalización.

Aquí, usamos la aplicación **Adventure Works** para mostrar la pantalla de finalización de una solicitud de comando de voz para mostrar los próximos viajes a Londres. 

![pantalla de finalización de la aplicación en segundo plano de Cortana](images/cortana-completion-screen-upcomingtrip-small.png)

El comando de voz se define en AdventureWorksCommands.xml:
```
<Command Name="whenIsTripToDestination">
  <Example> When is my trip to Las Vegas?</Example>
  <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
  <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
  <Feedback> Looking for trip to {destination}</Feedback>
  <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
</Command>
```

AdventureWorksVoiceCommandService.cs contiene el método de mensaje de finalización:

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
    // If this operation is expected to take longer than 0.5 seconds, the task must
    // supply a progress response to Cortana before starting the operation, and
    // updates must be provided at least every 5 seconds.
    string loadingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("LoadingTripToDestination", cortanaContext).ValueAsString,
               destination);
    await ShowProgressScreen(loadingTripToDestination);
    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    // Query for the specified trip. 
    // The destination should be in the phrase list. However, there might be  
    // multiple trips to the destination. We pick the first.
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
    if (trips.Count() == 0)
    {
        string foundNoTripToDestination = string.Format(
               cortanaResourceMap.GetValue("FoundNoTripToDestination", cortanaContext).ValueAsString,
               destination);
        userMessage.DisplayMessage = foundNoTripToDestination;
        userMessage.SpokenMessage = foundNoTripToDestination;
    }
    else
    {
        // Set plural or singular title.
        string message = "";
        if (trips.Count() > 1)
        {
            message = cortanaResourceMap.GetValue("PluralUpcomingTrips", cortanaContext).ValueAsString;
        }
        else
        {
            message = cortanaResourceMap.GetValue("SingularUpcomingTrip", cortanaContext).ValueAsString;
        }
        userMessage.DisplayMessage = message;
        userMessage.SpokenMessage = message;

        // Define a tile for each destination.
        foreach (Model.Trip trip in trips)
        {
            int i = 1;
            
            var destinationTile = new VoiceCommandContentTile();

            destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
            destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));

            destinationTile.AppLaunchArgument = trip.Destination;
            destinationTile.Title = trip.Destination;
            if (trip.StartDate != null)
            {
                destinationTile.TextLine1 = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
            }
            else
            {
                destinationTile.TextLine1 = trip.Destination + " " + i;
            }

            destinationsContentTiles.Add(destinationTile);
            i++;
        }
    }

    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```

## <span id="Hand-off_screen"></span><span id="hand-off_screen"></span><span id="HAND-OFF_SCREEN"></span>Pantalla de entrega

Una vez que se reconoce un comando de voz, **Cortana** debe llamar a ReportSuccessAsync y presentar información en aproximadamente 500 ms. Si el servicio de la aplicación no puede completar la acción especificada por el comando de voz en 500 ms, **Cortana** muestra al usuario una pantalla de entrega hasta que la aplicación llame a ReportSuccessAsync o durante un período de hasta 5 segundos.

Si el servicio de la aplicación no llama a ReportSuccessAsync o a algún otro método VoiceCommandServiceConnection, el usuario recibe un mensaje de error y se cancela la llamada del servicio de aplicaciones.

Este es un ejemplo de una pantalla de entrega de la aplicación **Adventure Works**. En este ejemplo, un usuario le ha consultado a **Cortana** los próximos viajes. La pantalla de entrega incluye un mensaje personalizado con el nombre de servicio de la aplicación, un icono y una cadena de **Comentarios**. 

[!NOTE] Puedes declarar una cadena **comentarios** en el archivo VCD. Esta cadena no afecta el texto de la interfaz de usuario que se muestra en el lienzo de Cortana, solo afecta al texto que dice **Cortana**.

![Pantalla de entrega de la aplicación en segundo plano de Cortana](images/cortana-backgroundapp-progress-result.png)


## <span id="Progress_screen"></span><span id="progress_screen"></span><span id="PROGRESS_SCREEN"></span>Pantalla de progreso


Si el servicio de la aplicación tarda más de 500 ms en llamar a ReportSuccessAsync, **Cortana** muestra al usuario una pantalla de progreso. A continuación se muestra el icono de la aplicación y debes proporcionar cadenas de progreso de la interfaz gráfica de usuario y TTS para indicar que la tarea se está controlando activamente.

**Cortana** muestra una pantalla de progreso durante un máximo de 5 segundos. Transcurridos 5 segundos, **Cortana** muestra al usuario un mensaje de error y el servicio de la aplicación se cierra. Si el servicio de la aplicación necesita más de 5 segundos para completar la acción, puede seguir actualizando **Cortana** con pantallas de progreso.

Este es un ejemplo de una pantalla de progreso de la aplicación **Adventure Works**. En este ejemplo, un usuario ha cancelado un viaje a Las Vegas La pantalla de progreso incluye un mensaje personalizado para la acción, un icono y un icono de contenido con información sobre el viaje cancelado.

![Pantalla de progreso de la aplicación en segundo plano de Cortana ](images/cortana-progress-screen.png)

AdventureWorksVoiceCommandService.cs contiene el siguiente método de mensaje de progreso, denominado [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579) para mostrar la pantalla de progreso en **Cortana**.


```    CSharp
/// <summary>
/// Show a progress screen. These should be posted at least every 5 seconds for a 
/// long-running operation.
/// </summary>
/// <param name="message">The message to display, relating to the task being performed.</param>
/// <returns></returns>
private async Task ShowProgressScreen(string message)
{
    var userProgressMessage = new VoiceCommandUserMessage();
    userProgressMessage.DisplayMessage = userProgressMessage.SpokenMessage = message;

    VoiceCommandResponse response = VoiceCommandResponse.CreateResponse(userProgressMessage);
    await voiceServiceConnection.ReportProgressAsync(response);
}
```

## <span id="Confirmation_screen"></span><span id="confirmation_screen"></span><span id="CONFIRMATION_SCREEN"></span>Pantalla de confirmación


Cuando una acción especificada por un comando de voz es irreversible, tiene un impacto significativo o la confianza del reconocimiento no es alta, un servicio de la aplicación puede solicitar confirmación.

Este es un ejemplo de una pantalla de confirmación de la aplicación **Adventure Works**. En este ejemplo, un usuario indicó al servicio de la aplicación que cancelara un viaje a Las Vegas a través de **Cortana**. El servicio de la aplicación ha proporcionado a **Cortana** una pantalla de confirmación que pide al usuario una respuesta de tipo sí o no antes de cancelar el viaje.

Si el usuario dice algo distinto a "Sí" o "No", **Cortana** no puede determinar la respuesta a la pregunta. En este caso, **Cortana** solicita confirmación al usuario con una pregunta similar, que proporciona el servicio de la aplicación.

En la segunda solicitud, si el usuario sigue sin decir "Sí" o "No", **Cortana** solicita confirmación al usuario una tercera vez con la misma pregunta, precedida de una disculpa. Si el usuario sigue sin decir "Sí" o "No", **Cortana** detiene la escucha de la entrada de voz y pide al usuario que, en su lugar, presiona uno de los botones.

La pantalla de confirmación incluye un mensaje personalizado para la acción, un icono y un icono de contenido con información sobre el viaje cancelado.

![Pantalla de confirmación de la aplicación en segundo plano de Cortana](images/cortana-confirmation-screen.png)

AdventureWorksVoiceCommandService.cs contiene el siguiente método de cancelación de viaje, denominado [**RequestConfirmationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706582) para mostrar una pantalla de confirmación en **Cortana**.

```    CSharp
/// <summary>
/// Handle the Trip Cancellation task. This task demonstrates how to prompt a user
/// for confirmation of an operation, show users a progress screen while performing
/// a long-running task, and show a completion screen.
/// </summary>
/// <param name="destination">The name of a destination.</param>
/// <returns></returns>
private async Task SendCompletionMessageForCancellation(string destination)
{
    // Begin loading data to search for the target store. 
    // Consider inserting a progress screen here, in order to prevent Cortana from timing out. 
    string progressScreenString = string.Format(
        cortanaResourceMap.GetValue("ProgressLookingForTripToDest", cortanaContext).ValueAsString,
        destination);
    await ShowProgressScreen(progressScreenString);

    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);
    Model.Trip trip = null;
    if (trips.Count() > 1)
    {
        // If there is more than one trip, provide a disambiguation screen.
        // However, if a significant number of items are returned, you might want to 
        // just display a link to your app and provide a deeper search experience.
        string disambiguationDestinationString = string.Format(
            cortanaResourceMap.GetValue("DisambiguationWhichTripToDest", cortanaContext).ValueAsString,
            destination);
        string disambiguationRepeatString = cortanaResourceMap.GetValue("DisambiguationRepeat", cortanaContext).ValueAsString;
        trip = await DisambiguateTrips(trips, disambiguationDestinationString, disambiguationRepeatString);
    }
    else
    {
        trip = trips.FirstOrDefault();
    }

    var userPrompt = new VoiceCommandUserMessage();
    
    VoiceCommandResponse response;
    if (trip == null)
    {
        var userMessage = new VoiceCommandUserMessage();
        string noSuchTripToDestination = string.Format(
            cortanaResourceMap.GetValue("NoSuchTripToDestination", cortanaContext).ValueAsString,
            destination);
        userMessage.DisplayMessage = userMessage.SpokenMessage = noSuchTripToDestination;

        response = VoiceCommandResponse.CreateResponse(userMessage);
        await voiceServiceConnection.ReportSuccessAsync(response);
    }
    else
    {
        // Prompt the user for confirmation that this is the correct trip to cancel.
        string cancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("CancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userPrompt.DisplayMessage = userPrompt.SpokenMessage = cancelTripToDestination;
        var userReprompt = new VoiceCommandUserMessage();
        string confirmCancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("ConfirmCancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userReprompt.DisplayMessage = userReprompt.SpokenMessage = confirmCancelTripToDestination;
        
        response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt);

        var voiceCommandConfirmation = await voiceServiceConnection.RequestConfirmationAsync(response);

        // If RequestConfirmationAsync returns null, Cortana has likely been dismissed.
        if (voiceCommandConfirmation != null)
        {
            if (voiceCommandConfirmation.Confirmed == true)
            {
                string cancellingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("CancellingTripToDestination", cortanaContext).ValueAsString,
               destination);
                await ShowProgressScreen(cancellingTripToDestination);

                // Perform the operation to remove the trip from app data. 
                // As the background task runs within the app package of the installed app,
                // we can access local files belonging to the app without issue.
                await store.DeleteTrip(trip);

                // Provide a completion message to the user.
                var userMessage = new VoiceCommandUserMessage();
                string cancelledTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("CancelledTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = cancelledTripToDestination;
                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
            else
            {
                // Confirm no action for the user.
                var userMessage = new VoiceCommandUserMessage();
                string keepingTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("KeepingTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = keepingTripToDestination;

                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
        }
    }
}
```

## <span id="Disambiguation_screen"></span><span id="disambiguation_screen"></span><span id="DISAMBIGUATION_SCREEN"></span>Pantalla de desambiguación


Cuando una acción especificada por un comando de voz tiene más de un posible resultado, el servicio de una aplicación puede solicitar más información al usuario.

Este es un ejemplo de una pantalla de desambiguación de la aplicación **Adventure Works**. En este ejemplo, un usuario indicó al servicio de la aplicación que cancelara un viaje a Las Vegas a través de **Cortana**. Sin embargo, el usuario tiene dos viajes a Las Vegas en fechas diferentes y el servicio de la aplicación no puede completar la acción sin que el usuario seleccione el viaje que desea.

El servicio de la aplicación proporciona a **Cortana** una pantalla de desambiguación que pide al usuario que realice una selección de una lista de viajes que coinciden, antes de cancelar ninguno.

En este caso, **Cortana** solicita confirmación al usuario con una pregunta similar, que proporciona el servicio de la aplicación.

En la segunda solicitud, si el usuario aún dice algo que pueda usarse para identificar la selección, **Cortana** pide confirmación al usuario una tercera vez con la misma pregunta, precedida de una disculpa. Si el usuario sigue sin decir algo que pueda usarse para identificar la selección, **Cortana** detiene la escucha de la entrada de voz y pide al usuario que, en su lugar, presiona uno de los botones.

La pantalla de desambiguación incluye un mensaje personalizado para la acción, un icono y un icono de contenido con información sobre el viaje cancelado.

![Pantalla de desambiguación de la aplicación en segundo plano de Cortana ](images/cortana-disambiguation-screen.png)

AdventureWorksVoiceCommandService.cs contiene el siguiente método de cancelación de viaje, denominado [**RequestDisambiguationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706583) para mostrar una pantalla de confirmación en **Cortana**.

```csharp
/// <summary>
/// Provide the user with a way to identify which trip to cancel. 
/// </summary>
/// <param name="trips">The set of trips</param>
/// <param name="disambiguationMessage">The initial disambiguation message</param>
/// <param name="secondDisambiguationMessage">Repeat prompt retry message</param>
private async Task<Model.Trip> DisambiguateTrips(IEnumerable<Model.Trip> trips, string disambiguationMessage, string secondDisambiguationMessage)
{
    // Create the first prompt message.
    var userPrompt = new VoiceCommandUserMessage();
    userPrompt.DisplayMessage =
        userPrompt.SpokenMessage = disambiguationMessage;

    // Create a re-prompt message if the user responds with an out-of-grammar response.
    var userReprompt = new VoiceCommandUserMessage();
    userReprompt.DisplayMessage =
        userReprompt.SpokenMessage = secondDisambiguationMessage;

    // Create card for each item. 
    var destinationContentTiles = new List<VoiceCommandContentTile>();
    int i = 1;
    foreach (Model.Trip trip in trips)
    {
        var destinationTile = new VoiceCommandContentTile();

        destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
        destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
        
        // The AppContext can be any arbitrary object.
        destinationTile.AppContext = trip;
        string dateFormat = "";
        if (trip.StartDate != null)
        {
            dateFormat = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
        }
        else
        {
            // The app allows a trip to have no date.
            // However, the choices must be unique so they can be distinguished.
            // Here, we add a number to identify them.
            dateFormat = string.Format("{0}", i);
        } 

        destinationTile.Title = trip.Destination + " " + dateFormat;
        destinationTile.TextLine1 = trip.Description;

        destinationContentTiles.Add(destinationTile);
        i++;
    }

    // Cortana handles re-prompting if no valid response.
    var response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt, destinationContentTiles);

    // If cortana is dismissed in this operation, null is returned.
    var voiceCommandDisambiguationResult = await
        voiceServiceConnection.RequestDisambiguationAsync(response);
    if (voiceCommandDisambiguationResult != null)
    {
        return (Model.Trip)voiceCommandDisambiguationResult.SelectedItem.AppContext;
    }

    return null;
}
```

## <span id="Error_screen"></span><span id="error_screen"></span><span id="ERROR_SCREEN"></span>Pantalla de error


Cuando no se puede completar una acción especificada por un comando de voz, el servicio de una aplicación puede proporcionar una pantalla de error.

Este es un ejemplo de una pantalla de error de la aplicación **Adventure Works**. En este ejemplo, un usuario indicó al servicio de la aplicación que cancelara un viaje a Las Vegas a través de **Cortana**. Sin embargo, el usuario no tiene ningún viaje programado a Las Vegas.

El servicio de la aplicación proporciona a **Cortana** una pantalla de error que incluye un mensaje personalizado para la acción, un icono y el mensaje de error concreto.

Llama a [**ReportFailureAsync**](https://msdn.microsoft.com/library/windows/apps/dn706578) para mostrar la pantalla de error en **Cortana**.

```csharp
var userMessage = new VoiceCommandUserMessage();
    userMessage.DisplayMessage = userMessage.SpokenMessage = 
      "Sorry, you don't have any trips to Las Vegas";
                
    var response = VoiceCommandResponse.CreateResponse(userMessage);

    response.AppLaunchArgument = "showUpcomingTrips";
    await voiceServiceConnection.ReportFailureAsync(response);
```

## <span id="related_topics"></span>Artículos relacionados


**Desarrolladores**
* [Interacciones de Cortana](cortana-interactions.md)
* [**VCD elements and attributes v1.2 (Elementos y atributos de VCD v1.2)**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Diseñadores**
* [Directrices para el diseño de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Directrices para el diseño de Voz](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Muestras**
* [Muestra de comando de voz de Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 







<!--HONumber=Jun16_HO3-->


