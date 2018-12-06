---
Description: Learn about several ways you can programmatically enable customers to rate and review your app.
title: Solicitar calificaciones y opiniones de tu aplicación
ms.date: 06/15/2018
ms.topic: article
keywords: windows 10, uwp, calificaciones, opiniones
ms.localizationpriority: medium
ms.openlocfilehash: 377b71dba2fb62dfc562b56d40e65e43b0bd49c9
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8749744"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>Solicitar calificaciones y opiniones de tu aplicación

Puedes agregar código a la aplicación Plataforma universal de Windows (UWP) para solicitar mediante programación a los clientes que califiquen u opinen sobre tu aplicación. Existen varias formas de hacerlo:
* Puedes mostrar un diálogo de calificaciones y opiniones directamente en el contexto de la aplicación.
* Puedes abrir mediante programación la página de calificaciones y opiniones para tu aplicación en Microsoft Store.

Cuando estés listo para analizar las clasificaciones y los datos de opiniones, puedes ver los datos en el centro de partners o usar la API de análisis de Microsoft Store para recuperar estos datos mediante programación.

> [!IMPORTANT]
> Al agregar una función de clasificación dentro de la aplicación, todas las revisiones deben enviar al usuario a los mecanismos de clasificación de la tienda, independientemente por estrellas elegida. Si recopilar comentarios o los comentarios de los usuarios, debe quedar claro que no está relacionada con la clasificación de la aplicación o a las críticas de la tienda pero se envía directamente al desarrollador de la aplicación. Consulta el desarrollador de código de conducta para obtener más información relacionada con [Fraudulent o malas intenciones de actividades](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities).

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>Mostrar un diálogo de calificaciones y opiniones en la aplicación

Para mostrar mediante programación un cuadro de diálogo de la aplicación que pida al cliente que califique tu aplicación y envíe una opinión , llama al método [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync) en el espacio de nombres [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). Pasa el entero 16al parámetro *requestKind* y una cadena vacía al parámetro *parametersAsJson*, como se muestra en este ejemplo de código. Este ejemplo requiere la biblioteca [Json.NET](http://www.newtonsoft.com/json) de Newtonsoft y requiere el uso de instrucciones para los espacios de nombres **Windows.Services.Store**, **System.Threading.Tasks** y **Newtonsoft.Json.Linq**.

> [!IMPORTANT]
> La solicitud para mostrar el diálogo de clasificación y opiniones se debe llamar en el subproceso de la interfaz de usuario en tu aplicación.

```csharp
public async Task<bool> ShowRatingReviewDialog()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 16, String.Empty);

    if (result.ExtendedError == null)
    {
        JObject jsonObject = JObject.Parse(result.Response);
        if (jsonObject.SelectToken("status").ToString() == "success")
        {
            // The customer rated or reviewed the app.
            return true;
        }
    }

    // There was an error with the request, or the customer chose not to
    // rate or review the app.
    return false;
}
```

El método **SendRequestAsync** usa un sistema sencillo de solicitud basado en números enteros y parámetros de datos basados en JSON para exponer varias operaciones de la Store para las aplicaciones. Al pasar el entero 16al parámetro *requestKind*, se emite una solicitud para mostrar el diálogo de calificaciones y opiniones y enviar los datos relacionados a la Store. Este método se introdujo en Windows 10, versión 1607, y solo se puede usar en proyectos destinados a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio. Para obtener una visión general de este método, consulta [Enviar solicitudes a Store](send-requests-to-the-store.md).

### <a name="response-data-for-the-rating-and-review-request"></a>Datos de respuesta para la solicitud de calificaciones y opiniones

Una vez que envíes la solicitud para mostrar el diálogo de calificaciones y opiniones, la propiedad [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.Response) del valor devuelto [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) contiene una cadena con formato JSON que indica si la solicitud se realizó correctamente.

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

|  Campo  |  Descripción  |
|----------------------|---------------|
|  *status*                   |  Una cadena que indica si el cliente envió correctamente una calificación o reseña. Los valores admitidos son **success** y **aborted**.   |
|  *data*                   |  Un objeto que contiene un valor booleano único denominado *updated*. Este valor indica si el cliente ha actualizado una calificación o reseña existente. El objeto *data* objeto solo se incluye en respuestas realizadas correctamente.   |
|  *errorDetails*                   |  Una cadena que contiene la información de errores de la solicitud. |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>Iniciar la página de calificaciones y opiniones de la aplicación en la Store

Si deseas abrir mediante programación la página de calificaciones y opiniones para tu aplicación en la Store, puedes usar el método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) con el esquema URI ```ms-windows-store://review``` como se muestra en este ejemplo de código.

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

Para obtener más información, consulta [Iniciar la aplicación Microsoft Store](../launch-resume/launch-store-app.md).

## <a name="analyze-your-ratings-and-reviews-data"></a>Analizar los datos de calificaciones y opiniones

Para analizar los datos de calificaciones y opiniones de los clientes, tienes varias opciones:
* Puedes usar el informe [de opiniones](../publish/reviews-report.md) en el centro de partners para ver las calificaciones y opiniones de los clientes. También puedes descargar este informe para verlo sin conexión.
* Puedes usar los métodos [Obtener la clasificación de la aplicación](get-app-ratings.md) y [Obtener opiniones de la aplicación](get-app-reviews.md) en la API de análisis de la Store para recuperar mediante programación las calificaciones y opiniones de los clientes en formato JSON.

## <a name="related-topics"></a>Artículos relacionados

* [Enviar solicitudes a Store](send-requests-to-the-store.md)
* [Iniciar la aplicación Microsoft Store](../launch-resume/launch-store-app.md)
* [Informe Críticas](../publish/reviews-report.md)
