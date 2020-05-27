---
Description: Puede usar el método SendRequestAsync para enviar solicitudes al Microsoft Store para las operaciones que aún no tienen una API disponible en el Windows SDK.
title: Enviar solicitudes al Microsoft Store
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, StoreRequestHelper, SendRequestAsync
ms.localizationpriority: medium
ms.openlocfilehash: 810c546eb0ee0263dcb50b3ce58e593ad294435c
ms.sourcegitcommit: 577a54d36145f91c8ade8e4509d4edddd8319137
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83867335"
---
# <a name="send-requests-to-the-microsoft-store"></a>Enviar solicitudes al Microsoft Store

A partir de Windows 10, versión 1607, el Windows SDK proporciona las API para las operaciones relacionadas con el almacén (como compras desde la aplicación) en el espacio de nombres [Windows. Services. Store](https://docs.microsoft.com/uwp/api/windows.services.store) . Sin embargo, aunque los servicios que admiten la tienda se actualizan, se expanden y mejoran constantemente entre versiones del sistema operativo, las nuevas API normalmente se agregan a la Windows SDK solo durante las versiones principales del sistema operativo.

Proporcionamos el método [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync) como una manera flexible de poner nuevas operaciones de almacenamiento a disposición de las aplicaciones plataforma universal de Windows (UWP) antes de que se publique una nueva versión de la Windows SDK. Puede usar este método para enviar solicitudes al almacén de nuevas operaciones que aún no tienen una API correspondiente disponible en la versión más reciente de la Windows SDK.

> [!NOTE]
> El método **SendRequestAsync** solo está disponible para las aplicaciones que tienen como destino Windows 10, versión 1607 o posterior. Algunas de las solicitudes admitidas por este método solo se admiten en versiones posteriores a Windows 10, versión 1607.

**SendRequestAsync** es un método estático de la clase [StoreRequestHelper](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper) . Para llamar a este método, debe pasar la información siguiente al método:
* Objeto [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) que proporciona información sobre el usuario para el que desea realizar la operación. Para obtener más información sobre este objeto, consulte Introducción [a la clase StoreContext](in-app-purchases-and-trials.md#get-started-with-the-storecontext-class).
* Un entero que identifica la solicitud que desea enviar al almacén.
* Si la solicitud admite argumentos, también puede pasar una cadena con formato JSON que contenga los argumentos que se van a pasar junto con la solicitud.

En el siguiente ejemplo se muestra cómo llamar a este método. Este ejemplo requiere el uso de instrucciones para los espacios de nombres **Windows. Services. Store** y **System. Threading. Tasks** .

```csharp
public async Task<bool> AddUserToFlightGroup()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 8,
        "{ \"type\": \"AddToFlightGroup\", \"parameters\": { \"flightGroupId\": \"your group ID\" } }");

    if (result.ExtendedError == null)
    {
        return true;
    }

    return false;
}
```

Vea las siguientes secciones para obtener información sobre las solicitudes que están disponibles actualmente a través del método **SendRequestAsync** . Este artículo se actualizará cuando se agregue la compatibilidad con nuevas solicitudes.

## <a name="request-for-in-app-ratings-and-reviews"></a>Solicitud de revisiones y clasificaciones en la aplicación

Puede iniciar mediante programación un cuadro de diálogo desde la aplicación que solicita al cliente que califique la aplicación y envíe una revisión pasando el entero de solicitud 16 al método **SendRequestAsync** . Para obtener más información, consulte [Mostrar un cuadro de diálogo de clasificación y revisión en la aplicación](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app).

## <a name="requests-for-flight-group-scenarios"></a>Solicitudes de escenarios de grupo de vuelos

> [!IMPORTANT]
> Todas las solicitudes de grupo de vuelos descritas en esta sección actualmente no están disponibles para la mayoría de las cuentas de desarrollador. Se producirá un error en estas solicitudes a menos que Microsoft aprovisione especialmente su cuenta de desarrollador.

El método **SendRequestAsync** admite un conjunto de solicitudes para escenarios de grupo de vuelos, como agregar un usuario o un dispositivo a un grupo de vuelos. Para enviar estas solicitudes, pase el valor 7 u 8 al parámetro *requestKind* junto con una cadena con formato JSON al parámetro *parametersAsJson* que indica la solicitud que desea enviar junto con los argumentos relacionados. Estos valores de *requestKind* difieren de las siguientes maneras.

|  Valor de la clase de solicitud  |  Descripción  |
|----------------------|---------------|
|  7                   |  Las solicitudes se realizan en el contexto del dispositivo actual. Este valor solo se puede usar en Windows 10, versión 1703 o posterior.  |
|  8                   |  Las solicitudes se realizan en el contexto del usuario que ha iniciado sesión actualmente en el almacén. Este valor se puede usar en Windows 10, versión 1607 o posterior.  |

Actualmente se implementan las siguientes solicitudes de grupo de vuelos.

### <a name="retrieve-remote-variables-for-the-highest-ranked-flight-group"></a>Recuperación de variables remotas para el grupo de vuelos con mayor clasificación

> [!IMPORTANT]
> Actualmente, esta solicitud no está disponible para la mayoría de las cuentas de desarrollador. Se producirá un error en esta solicitud a menos que Microsoft aprovisione especialmente su cuenta de desarrollador.

Esta solicitud recupera las variables remotas para el grupo de vuelos de mayor clasificación del usuario o dispositivo actual. Para enviar esta solicitud, pase la siguiente información a los parámetros *requestKind* y *ParametersAsJson* del método **SendRequestAsync** .

|  Parámetro  |  Descripción  |
|----------------------|---------------|
|  *requestKind*                   |  Especifique 7 para devolver el grupo de vuelos de mayor rango para el dispositivo o especifique 8 para devolver el grupo de vuelos de mayor rango para el usuario y el dispositivo actuales. Se recomienda usar el valor 8 para el parámetro *requestKind* , ya que este valor devolverá el grupo de vuelos de mayor rango a través de la pertenencia para el usuario y el dispositivo actuales.  |
|  *parametersAsJson*                   |  Pase una cadena con formato JSON que contenga los datos que se muestran en el ejemplo siguiente.  |

En el ejemplo siguiente se muestra el formato de los datos JSON que se van a pasar a *parametersAsJson*. El campo de *tipo* debe asignarse a la cadena *GetRemoteVariables*. Asigne el campo *Projectid* al identificador del proyecto en el que definió las variables remotas en el centro de Partners.

```json
{ 
    "type": "GetRemoteVariables", 
    "parameters": "{ \"projectId\": \"your project ID\" }" 
}
```

Después de enviar esta solicitud, la propiedad [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.Response) del valor devuelto [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) contiene una cadena con formato JSON con los siguientes campos.

|  Campo  |  Descripción  |
|----------------------|---------------|
|  *anonymous*                   |  Un valor booleano, donde **true** indica que la identidad del usuario o del dispositivo no estaba presente en la solicitud, y **false** indica que la identidad del usuario o del dispositivo estaba presente en la solicitud.  |
|  *name*                   |  Una cadena que contiene el nombre del grupo de vuelos de mayor rango al que pertenece el dispositivo o el usuario.  |
|  *settings*                   |  Un diccionario de pares clave-valor que contienen el nombre y el valor de las variables remotas que el desarrollador ha configurado para el grupo de vuelos.  |

En el ejemplo siguiente se muestra un valor devuelto para esta solicitud.

```json
{ 
  "anonymous": false, 
  "name": "Insider Slow",
  "settings":
  {
      "Audience": "Slow"
      ...
  }
}
```

### <a name="add-the-current-device-or-user-to-a-flight-group"></a>Agregar el dispositivo o el usuario actual a un grupo de vuelos

> [!IMPORTANT]
> Actualmente, esta solicitud no está disponible para la mayoría de las cuentas de desarrollador. Se producirá un error en esta solicitud a menos que Microsoft aprovisione especialmente su cuenta de desarrollador.

Para enviar esta solicitud, pase la siguiente información a los parámetros *requestKind* y *ParametersAsJson* del método **SendRequestAsync** .

|  Parámetro  |  Descripción  |
|----------------------|---------------|
|  *requestKind*                   |  Especifique 7 para agregar el dispositivo a un grupo de vuelos o especifique 8 para agregar el usuario que ha iniciado sesión actualmente en la tienda a un grupo de vuelos.  |
|  *parametersAsJson*                   |  Pase una cadena con formato JSON que contenga los datos que se muestran en el ejemplo siguiente.  |

En el ejemplo siguiente se muestra el formato de los datos JSON que se van a pasar a *parametersAsJson*. El campo de *tipo* debe asignarse a la cadena *AddToFlightGroup*. Asigne el campo *flightGroupId* al ID. del grupo de vuelos al que desea agregar el dispositivo o el usuario.

```json
{ 
    "type": "AddToFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

Si se produce un error con la solicitud, la propiedad [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) del valor devuelto [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) contiene el código de respuesta.

### <a name="remove-the-current-device-or-user-from-a-flight-group"></a>Quitar el dispositivo o el usuario actual de un grupo de vuelos

> [!IMPORTANT]
> Actualmente, esta solicitud no está disponible para la mayoría de las cuentas de desarrollador. Se producirá un error en esta solicitud a menos que Microsoft aprovisione especialmente su cuenta de desarrollador.

Para enviar esta solicitud, pase la siguiente información a los parámetros *requestKind* y *ParametersAsJson* del método **SendRequestAsync** .

|  Parámetro  |  Descripción  |
|----------------------|---------------|
|  *requestKind*                   |  Especifique 7 para quitar el dispositivo de un grupo de vuelos o especifique 8 para quitar el usuario que ha iniciado sesión actualmente en el almacén desde un grupo de vuelos.  |
|  *parametersAsJson*                   |  Pase una cadena con formato JSON que contenga los datos que se muestran en el ejemplo siguiente.  |

En el ejemplo siguiente se muestra el formato de los datos JSON que se van a pasar a *parametersAsJson*. El campo de *tipo* debe asignarse a la cadena *RemoveFromFlightGroup*. Asigne el campo *flightGroupId* al ID. del grupo de vuelos del que desea quitar el dispositivo o el usuario.

```json
{ 
    "type": "RemoveFromFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

Si se produce un error con la solicitud, la propiedad [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) del valor devuelto [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) contiene el código de respuesta.

## <a name="related-topics"></a>Temas relacionados

* [Mostrar un cuadro de diálogo de clasificación y revisión en la aplicación](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app)
* [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync)
