---
Description: Obtenga información sobre las distintas formas en las que puede permitir que los clientes evalúen y revisen su aplicación mediante programación.
title: Solicitar calificaciones y opiniones de tu aplicación
ms.date: 01/22/2019
ms.topic: article
keywords: windows 10, uwp, calificaciones, opiniones
ms.localizationpriority: medium
ms.openlocfilehash: b167f4cc40ee72e6405436bacee28f2f20b4623c
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853042"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>Solicitar calificaciones y opiniones de tu aplicación

Puedes agregar código a la aplicación Plataforma universal de Windows (UWP) para solicitar mediante programación a los clientes que califiquen u opinen sobre tu aplicación. Existen varias formas de hacerlo:
* Puedes mostrar un diálogo de calificaciones y opiniones directamente en el contexto de la aplicación.
* Puedes abrir mediante programación la página de calificaciones y opiniones para tu aplicación en Microsoft Store.

Cuando esté preparado para analizar los datos de clasificación y revisión, puede ver los datos en el centro de Partners o usar la API de Microsoft Store Analytics para recuperar estos datos mediante programación.

> [!IMPORTANT]
> Al agregar una función de clasificación dentro de la aplicación, todas las revisiones deben enviar al usuario a los mecanismos de clasificación de la tienda, con independencia de la clasificación por estrellas elegida. Si recopila comentarios o comentarios de los usuarios, debe estar claro que no están relacionados con la clasificación o las revisiones de la aplicación en la tienda, sino que se envían directamente al desarrollador de la aplicación. Consulte el código de conducta para desarrolladores para obtener más información relacionada con [actividades fraudulentas o deshonestos](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities).

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>Mostrar un diálogo de clasificación y reseña en la aplicación

Para mostrar mediante programación un cuadro de diálogo de la aplicación que solicita al cliente que califique la aplicación y envíe una revisión, llame al método [RequestRateAndReviewAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestrateandreviewappasync) en el espacio de nombres [Windows. Services. Store](https://docs.microsoft.com/uwp/api/windows.services.store) . 

> [!IMPORTANT]
> La solicitud para mostrar el diálogo de clasificación y opiniones se debe llamar en el subproceso de la interfaz de usuario en tu aplicación.

```csharp
using Windows.ApplicationModel.Store;

private StoreContext _storeContext;

public async Task Initialize()
{
    if (App.IsMultiUserApp) // pseudo-code
    {
        IReadOnlyList<User> users = await User.FindAllAsync();
        User firstUser = users[0];
        _storeContext = StoreContext.GetForUser(firstUser);
    }
    else
    {
        _storeContext = StoreContext.GetDefault();
    }
}

private async Task PromptUserToRateApp()
{
    // Check if we’ve recently prompted user to review, we don’t want to bother user too often and only between version changes
    if (HaveWePromptedUserInPastThreeMonths())  // pseudo-code
    {
        return;
    }

    StoreRateAndReviewResult result = await 
        _storeContext.RequestRateAndReviewAppAsync();

    // Check status
    switch (result.Status)
    { 
        case StoreRateAndReviewStatus.Succeeded:
            // Was this an updated review or a new review, if Updated is false it means it was a users first time reviewing
            if (result.UpdatedExistingRatingOrReview)
            {
                // This was an updated review thank user
                ThankUserForReview(); // pseudo-code
            }
            else
            {
                // This was a new review, thank user for reviewing and give some free in app tokens
                ThankUserForReviewAndGrantTokens(); // pseudo-code
            }
            // Keep track that we prompted user and don’t do it again for a while
            SetUserHasBeenPrompted(); // pseudo-code
            break;

        case StoreRateAndReviewStatus.CanceledByUser:
            // Keep track that we prompted user and don’t prompt again for a while
            SetUserHasBeenPrompted(); // pseudo-code

            break;

        case StoreRateAndReviewStatus.NetworkError:
            // User is probably not connected, so we’ll try again, but keep track so we don’t try too often
            SetUserHasBeenPromptedButHadNetworkError(); // pseudo-code

            break;

        // Something else went wrong
        case StoreRateAndReviewStatus.OtherError:
        default:
            // Log error, passing in ExtendedJsonData however it will be empty for now
            LogError(result.ExtendedError, result.ExtendedJsonData); // pseudo-code
            break;
    }
}
```

El método **RequestRateAndReviewAppAsync** se presentó en la versión 1809 de Windows 10 y solo se puede usar en proyectos que tengan como destino la **actualización 2018 de octubre de windows 10 (10,0; Compilación 17763)** o una versión posterior en Visual Studio.

### <a name="response-data-for-the-rating-and-review-request"></a>Datos de respuesta para la solicitud de calificaciones y opiniones

Después de enviar la solicitud para mostrar el cuadro de diálogo clasificación y revisión, la propiedad [ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult.extendedjsondata) de la clase [StoreRateAndReviewResult](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult) contiene una cadena con formato JSON que indica si la solicitud se realizó correctamente.

El siguiente ejemplo muestra el valor devuelto para esta solicitud después de que el cliente envíe correctamente una calificación o reseña.

```json
{ 
  "status": "success", 
  "data": {
    "updated": false
  },
  "errorDetails": "Success"
}
```

El siguiente ejemplo muestra el valor devuelto para esta solicitud después de que el cliente no elija enviar una calificación o reseña.

```json
{ 
  "status": "aborted", 
  "errorDetails": "Navigation was unsuccessful"
}
```

En la siguiente tabla se describen los campos en la cadena de datos con formato JSON.

| Campo          | Descripción                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| *estatus*       | Una cadena que indica si el cliente envió correctamente una calificación o reseña. Los valores admitidos son **success** y **aborted**. |
| *Data*         | Un objeto que contiene un valor booleano único denominado *updated*. Este valor indica si el cliente ha actualizado una calificación o reseña existente. El objeto *data* objeto solo se incluye en respuestas realizadas correctamente. |
| *errorDetails* | Una cadena que contiene la información de errores de la solicitud.                                                                                     |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>Iniciar la página de calificaciones y opiniones de la aplicación en la Store

Si deseas abrir mediante programación la página de calificaciones y opiniones para tu aplicación en la Store, puedes usar el método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) con el esquema URI ```ms-windows-store://review``` como se muestra en este ejemplo de código.

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

Para obtener más información, consulta [Iniciar la aplicación Microsoft Store](../launch-resume/launch-store-app.md).

## <a name="analyze-your-ratings-and-reviews-data"></a>Analizar los datos de calificaciones y opiniones

Para analizar los datos de calificaciones y opiniones de los clientes, tienes varias opciones:
* Puede usar el informe de [revisiones](../publish/reviews-report.md) del centro de partners para ver las clasificaciones y las revisiones de sus clientes. También puedes descargar este informe para verlo sin conexión.
* Puedes usar los métodos [Obtener la clasificación de la aplicación](get-app-ratings.md) y [Obtener opiniones de la aplicación](get-app-reviews.md) en la API de análisis de la Store para recuperar mediante programación las calificaciones y opiniones de los clientes en formato JSON.

## <a name="related-topics"></a>Temas relacionados

* [Enviar solicitudes a la tienda](send-requests-to-the-store.md)
* [Iniciar la aplicación de Microsoft Store](../launch-resume/launch-store-app.md)
* [Informe Críticas](../publish/reviews-report.md)
