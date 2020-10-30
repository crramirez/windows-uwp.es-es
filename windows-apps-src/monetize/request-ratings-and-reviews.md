---
description: Obtenga información sobre las distintas formas en las que puede permitir que los clientes evalúen y revisen su aplicación mediante programación.
title: Solicitar calificaciones y revisiones para la aplicación
ms.date: 01/22/2019
ms.topic: article
keywords: Windows 10, UWP, clasificaciones y revisiones
ms.localizationpriority: medium
ms.openlocfilehash: 9dbc33eaaf3adcb05a6ad37e2f54ceec4769f530
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034408"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>Solicitar calificaciones y revisiones para la aplicación

Puede agregar código a la aplicación Plataforma universal de Windows (UWP) para pedir a los clientes que califiquen o revisen su aplicación mediante programación. Puede hacer esto de varias maneras:
* Puede mostrar un cuadro de diálogo de clasificación y revisión directamente en el contexto de la aplicación.
* Puede abrir mediante programación la página clasificación y revisión de la aplicación en el Microsoft Store.

Cuando esté preparado para analizar los datos de clasificación y revisión, puede ver los datos en el centro de Partners o usar la API de Microsoft Store Analytics para recuperar estos datos mediante programación.

> [!IMPORTANT]
> Al agregar una función de clasificación dentro de la aplicación, todas las revisiones deben enviar al usuario a los mecanismos de clasificación de la tienda, con independencia de la clasificación por estrellas elegida. Si recopila comentarios o comentarios de los usuarios, debe estar claro que no están relacionados con la clasificación o las revisiones de la aplicación en la tienda, sino que se envían directamente al desarrollador de la aplicación. Consulte el código de conducta para desarrolladores para obtener más información relacionada con [actividades fraudulentas o deshonestos](/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities).

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>Mostrar un cuadro de diálogo de clasificación y revisión en la aplicación

Para mostrar mediante programación un cuadro de diálogo de la aplicación que solicita al cliente que califique la aplicación y envíe una revisión, llame al método [RequestRateAndReviewAppAsync](/uwp/api/windows.services.store.storecontext.requestrateandreviewappasync) en el espacio de nombres [Windows. Services. Store](/uwp/api/windows.services.store) . 

> [!IMPORTANT]
> Se debe llamar a la solicitud para mostrar el cuadro de diálogo clasificación y revisión en el subproceso de la interfaz de usuario de la aplicación.

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

### <a name="response-data-for-the-rating-and-review-request"></a>Datos de respuesta para la solicitud de clasificación y revisión

Después de enviar la solicitud para mostrar el cuadro de diálogo clasificación y revisión, la propiedad [ExtendedJsonData](/uwp/api/windows.services.store.storerateandreviewresult.extendedjsondata) de la clase [StoreRateAndReviewResult](/uwp/api/windows.services.store.storerateandreviewresult) contiene una cadena con formato JSON que indica si la solicitud se realizó correctamente.

En el ejemplo siguiente se muestra el valor devuelto para esta solicitud después de que el cliente envíe correctamente una clasificación o una revisión.

```json
{ 
  "status": "success", 
  "data": {
    "updated": false
  },
  "errorDetails": "Success"
}
```

En el ejemplo siguiente se muestra el valor devuelto para esta solicitud después de que el cliente decida no enviar una clasificación o una revisión.

```json
{ 
  "status": "aborted", 
  "errorDetails": "Navigation was unsuccessful"
}
```

En la tabla siguiente se describen los campos de la cadena de datos con formato JSON.

| Campo          | Descripción                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| *status*       | Cadena que indica si el cliente envió correctamente una clasificación o una revisión. Los valores admitidos son **correcto** y **anulado** . |
| *data*         | Objeto que contiene un valor booleano único denominado *actualizado* . Este valor indica si el cliente actualizó una clasificación o revisión existente. El objeto de *datos* solo se incluye en las respuestas Success. |
| *errorDetails* | Cadena que contiene los detalles del error de la solicitud.                                                                                     |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>Inicio de la página clasificación y revisión de la aplicación en la tienda

Si desea abrir mediante programación la página clasificación y revisión de la aplicación en la tienda, puede usar el método [LaunchUriAsync](/uwp/api/windows.system.launcher.launchuriasync) con el ```ms-windows-store://review``` esquema de URI tal y como se muestra en este ejemplo de código.

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

Para obtener más información, consulte [Inicio de la aplicación Microsoft Store](../launch-resume/launch-store-app.md).

## <a name="analyze-your-ratings-and-reviews-data"></a>Analizar los datos de calificaciones y opiniones

Para analizar las clasificaciones y revisar los datos de los clientes, tiene varias opciones:
* Puede usar el informe de [revisiones](../publish/reviews-report.md) del centro de partners para ver las clasificaciones y las revisiones de sus clientes. También puede descargar este informe para verlo sin conexión.
* Puede usar los métodos [obtener clasificaciones de aplicaciones](get-app-ratings.md) y [obtener revisiones de aplicaciones](get-app-reviews.md) en Store Analytics API para recuperar mediante programación las clasificaciones y las revisiones de sus clientes en formato JSON.

## <a name="related-topics"></a>Temas relacionados

* [Enviar solicitudes a Store](send-requests-to-the-store.md)
* [Iniciar la aplicación de Microsoft Store](../launch-resume/launch-store-app.md)
* [Informe Críticas](../publish/reviews-report.md)
