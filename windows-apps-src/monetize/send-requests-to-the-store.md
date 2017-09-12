---
author: mcleanbyron
Description: "Puedes usar el método SendRequestAsync para enviar solicitudes a la Tienda Windows para operaciones que aún no tienen una API disponible en el WindowsSDK."
title: Enviar solicitudes a la Tienda Windows
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, StoreRequestHelper, SendRequestAsync
ms.openlocfilehash: a949b3c93bb5b4a056f9e1fa4679e8ddca05d899
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/21/2017
---
# <a name="send-requests-to-the-windows-store"></a>Enviar solicitudes a la Tienda Windows

A partir de Windows10, versión1607, el WindowsSDK proporciona API para las operaciones relacionadas con la Tienda (como las compras desde la aplicación) en el espacio de nombres [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). Sin embargo, aunque los servicios compatibles con la Tienda se actualizan, amplían y mejoran constantemente entre publicaciones de sistemas operativos, las nuevas API se suelen agregar en el WindowsSDK únicamente durante las publicaciones principales de sistemas operativos.

Proporcionamos el método [SendRequestAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreRequestHelper#Windows_Services_Store_StoreRequestHelper_SendRequestAsync_Windows_Services_Store_StoreContext_System_UInt32_System_String_) como una forma flexible para que las nuevas operaciones de la Tienda están disponibles para las aplicaciones para la Plataforma universal de Windows(UWP) antes de que se publique una nueva versión del WindowsSDK. Puedes usar este método para enviar solicitudes a la Tienda para nuevas operaciones que aún no tienen una API correspondiente disponible en la versión más reciente del WindowsSDK.

> [!NOTE]
> El método **SendRequestAsync** solo está disponible para aplicaciones dirigidas a Windows10, versión1607 o posterior. Algunas de las solicitudes compatibles con este método solo se admiten en versiones posteriores a Windows10, versión 1607.

**SendRequestAsync** es un método estático de la clase [StoreRequestHelper](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper). Para llamar a este método, debes pasar la siguiente información al método:
* Un objeto [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) que proporciona información sobre el usuario para el que quieres realizar la operación. Para obtener más información sobre este objeto, consulta [Introducción a la clase StoreContext](in-app-purchases-and-trials.md#get-started-with-the-storecontext-class).
* Un entero que identifica la solicitud que quieres enviar a la Tienda.
* Si la solicitud admite argumentos, también puedes pasar una cadena con formato JSON que contenga los argumentos que quieres pasar junto con la solicitud.

En el ejemplo siguiente se muestra cómo llamar a este método. En este ejemplo se requiere el uso de instrucciones para los espacios de nombres **Windows.Services.Store** y **System.Threading.Tasks**.

```csharp
public async Task<bool> AddUserToFlightGroup()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 8,
        "{ \"type\": \"AddToFlightGroup\", \"parameters\": \"{ \"flightGroupId\": \"your group ID\" }\" }");

    if (result.ExtendedError == null)
    {
        return true;
    }

    return false;
}
```

Consulta las siguientes secciones para obtener información sobre las solicitudes que están disponibles actualmente a través del método **SendRequestAsync**. Actualizaremos este artículo cuando se agregue compatibilidad para nuevas solicitudes.

## <a name="requests-for-flight-group-scenarios"></a>Solicitudes para escenarios de grupos piloto

> [!IMPORTANT]
> Todas las solicitudes de grupos piloto descritas en esta sección actualmente no están disponibles para la mayoría de las cuentas de desarrollador. Estas solicitudes generarán errores, a menos que Microsoft haya aprovisionado especialmente tu cuenta de desarrollador.

El método **SendRequestAsync** admite un conjunto de solicitudes para escenarios de grupos piloto, como agregar un usuario o dispositivo a un grupo piloto. Para enviar estas solicitudes, pasa el valor 7 u 8 al parámetro *requestKind* junto con una cadena con formato JSON al parámetro *parametersAsJson* que indica la solicitud que quieres enviar junto con cualquier argumento relacionado. Estos valores de *requestKind* difieren en las siguientes maneras.

|  Valor del tipo de solicitud  |  Descripción  |
|----------------------|---------------|
|  7                   |  Las solicitudes se realizan en el contexto del dispositivo actual. Este valor solo puede usarse en Windows10, versión 1703 o posterior.  |
|  8                   |  Las solicitudes se realizan en el contexto del usuario que actualmente inició sesión en la Tienda. Este valor puede usarse en Windows10, versión 1607 o posterior.  |

Actualmente se implementan las siguientes solicitudes del grupo piloto.

### <a name="retrieve-remote-variables-for-the-highest-ranked-flight-group"></a>Recuperar variables remotas para el grupo piloto de clasificación más alta

> [!IMPORTANT]
> Esta solicitud no está disponible actualmente para la mayoría de las cuentas de desarrollador. Esta solicitud generará errores, a menos que Microsoft haya aprovisionado especialmente tu cuenta de desarrollador.

Esta solicitud recupera las variables remotas para el grupo piloto de clasificación más alta del usuario o el dispositivo actuales. Para enviar esta solicitud, pasa la siguiente información a los parámetros *requestKind* y *parametersAsJson* del método **SendRequestAsync**.

|  Parámetro  |  Descripción  |
|----------------------|---------------|
|  *requestKind*                   |  Especifica 7 para devolver el grupo piloto de clasificación más alta para el dispositivo, o especifica 8 para devolver el grupo piloto de clasificación más alta para el dispositivo y el usuario actuales. Te recomendamos que uses el valor de 8 para el parámetro *requestKind*, porque este valor devolverá el grupo piloto de clasificación más alta en la pertenencia tanto para el dispositivo como para el usuario actual.  |
|  *parametersAsJson*                   |  Pasa una cadena con formato JSON que contiene los datos que se muestran en el siguiente ejemplo.  |

En el siguiente ejemplo se muestra el formato de los datos JSON para pasar a *parametersAsJson*. El campo *type* debe estar asignado a la cadena *GetRemoteVariables*. Asigna el campo *projectId* al identificador del proyecto en el que definiste las variables remotas en el panel del Centro de desarrollo de Windows.

```json
{ 
    "type": "GetRemoteVariables", 
    "parameters": "{ \"projectId\": \"your project ID\" }" 
}
```

Una vez que envíes esta solicitud, la propiedad [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult#Windows_Services_Store_StoreSendRequestResult_Response) del valor devuelto en [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) contiene una cadena con formato JSON con los siguientes campos.

|  Campo  |  Descripción  |
|----------------------|---------------|
|  *anonymous*                   |  Un valor booleano, donde **true** indica que la identidad del usuario o del dispositivo no estaba presente en la solicitud, y **false** indica que la identidad del usuario o del dispositivo estaba presente en la solicitud.  |
|  *name*                   |  Una cadena que contiene el nombre del grupo piloto de clasificación más alta a la que pertenecen el dispositivo o el usuario.  |
|  *settings*                   |  Un diccionario de pares de clave/valor que contiene el nombre y el valor de las variables remotas que el desarrollador ha configurado para el grupo piloto.  |

En el siguiente ejemplo se muestra un valor devuelto para esta solicitud.

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

### <a name="add-the-current-device-or-user-to-a-flight-group"></a>Agregar el dispositivo o el usuario actuales a un grupo piloto

> [!IMPORTANT]
> Esta solicitud no está disponible actualmente para la mayoría de las cuentas de desarrollador. Esta solicitud generará errores, a menos que Microsoft haya aprovisionado especialmente tu cuenta de desarrollador.

Para enviar esta solicitud, pasa la siguiente información a los parámetros *requestKind* y *parametersAsJson* del método **SendRequestAsync**.

|  Parámetro  |  Descripción  |
|----------------------|---------------|
|  *requestKind*                   |  Especifica 7 para agregar el dispositivo a un grupo piloto, o especifica 8 para agregar el usuario que inició sesión actualmente en la Tienda a un grupo piloto.  |
|  *parametersAsJson*                   |  Pasa una cadena con formato JSON que contiene los datos que se muestran en el siguiente ejemplo.  |

En el siguiente ejemplo se muestra el formato de los datos JSON para pasar a *parametersAsJson*. El campo *type* debe estar asignado a la cadena *AddToFlightGroup*. Asigna el campo *flightGroupId* al identificador del grupo piloto al que quieres agregar el dispositivo o el usuario.

```json
{ 
    "type": "AddToFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

Si hay un error con la solicitud, la propiedad [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult#Windows_Services_Store_StoreSendRequestResult_HttpStatusCode) del valor devuelto en [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) contiene el código de respuesta.

### <a name="remove-the-current-device-or-user-from-a-flight-group"></a>Quitar el dispositivo o el usuario actual de un grupo piloto

> [!IMPORTANT]
> Esta solicitud no está disponible actualmente para la mayoría de las cuentas de desarrollador. Esta solicitud generará errores, a menos que Microsoft haya aprovisionado especialmente tu cuenta de desarrollador.

Para enviar esta solicitud, pasa la siguiente información a los parámetros *requestKind* y *parametersAsJson* del método **SendRequestAsync**.

|  Parámetro  |  Descripción  |
|----------------------|---------------|
|  *requestKind*                   |  Especifica 7 para quitar el dispositivo de un grupo piloto, o especifica 8 para quitar el usuario que inició sesión actualmente en la Tienda de un grupo piloto.  |
|  *parametersAsJson*                   |  Pasa una cadena con formato JSON que contiene los datos que se muestran en el siguiente ejemplo.  |

En el siguiente ejemplo se muestra el formato de los datos JSON para pasar a *parametersAsJson*. El campo *type* debe estar asignado a la cadena *RemoveFromFlightGroup*. Asigna el campo *flightGroupId* al identificador del grupo piloto del que quieres quitar el dispositivo o el usuario.

```json
{ 
    "type": "RemoveFromFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

Si hay un error con la solicitud, la propiedad [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult#Windows_Services_Store_StoreSendRequestResult_HttpStatusCode) del valor devuelto en [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) contiene el código de respuesta.

## <a name="related-topics"></a>Temas relacionados

* [SendRequestAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreRequestHelper#Windows_Services_Store_StoreRequestHelper_SendRequestAsync_Windows_Services_Store_StoreContext_System_UInt32_System_String_)
