---
title: POST (/serviceconfigs/{scid}/hoppers/{hoppername})
assetID: 8cbf62aa-d639-e920-1e39-099133af17f8
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamepost.html
description: " POST (/serviceconfigs/{scid}/hoppers/{hoppername})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7682b92ec61c98679904825e360d73318e9fee90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659840"
---
# <a name="post-serviceconfigsscidhoppershoppername"></a>POST (/serviceconfigs/{scid}/hoppers/{hoppername})

Crea el vale de coincidencia especificados.

> [!IMPORTANT]
> Este método está pensado para su uso con el contrato 103 o posterior y requiere un elemento de encabezado de X-Xbl-contrato-Version: 103 o posterior en cada solicitud.

  * [Comentarios](#ID4ET)
  * [Parámetros de URI](#ID4E5)
  * [Autorización](#ID4EJB)
  * [Códigos de estado HTTP](#ID4E3C)
  * [Cuerpo de la solicitud](#ID4EFD)
  * [Cuerpo de respuesta](#ID4E3G)

<a id="ID4ET"></a>


## <a name="remarks"></a>Observaciones

Este método HTTP/REST, crea un vale de coincidencia para un hopper con un nombre determinado en la configuración del servicio de nivel de Id. (¿SCID). Este método se puede ajustar mediante el **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.CreateMatchTicketAsync** método.  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| scid| GUID| El identificador de configuración de servicio (¿SCID) para la sesión.|
| hoppername | string | El nombre de la hopper. |

<a id="ID4EJB"></a>


## <a name="authorization"></a>Autorización

| Tipo| Requerido| Descripción| Si falta de respuesta|
| --- | --- | --- | --- | --- | --- | --- | --- |
| Privilegios y el tipo de dispositivo| sí| Cuando se establece el tipo de dispositivo del usuario a la consola, se permiten solo los usuarios con el privilegio para varios jugadores en sus notificaciones para realizar llamadas para el servicio. | 403|
| Tipo de dispositivo| sí| Al tipo de dispositivo del usuario no está presente o establecido en la consola, el título que se va a hacer coincidir para no debe ser un título sólo para consola. | 403|
| Id. de título y prueba del tipo de compra/dispositivo| sí| El título que se buscan en debe permitir que los contactos de la notificación de título especificado, la combinación del tipo de dispositivo. | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP
El servicio devuelve un código de estado HTTP tal como se aplica a MPSD.  
<a id="ID4EFD"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

<a id="ID4ELD"></a>


### <a name="required-members"></a>Miembros requeridos

| Miembro| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| serviceConfig| GUID| ¿SCID para la sesión.|
| hopperName| string| Nombre de la hopper.|
| giveUpDuration| entero de 32 bits con signo| Tiempo de espera máximo (número entero de segundos).|
| preserveSession| enumeración| Un valor que indica si la sesión se vuelve a usar como la sesión en la que buscar la coincidencia. Los valores posibles son "siempre" y "nunca". |
| ticketSessionRef| MultiplayerSessionReference| El objeto MultiplayerSessionReference para la sesión en el que el Reproductor o el grupo se está reproduciendo. |
| ticketAttributes| colección de objetos| Los atributos y valores proporcionados por el usuario sobre el grupo de los jugadores.|

<a id="ID4EXF"></a>


### <a name="prohibited-members"></a>Miembros prohibidos

Todos los demás miembros están prohibidas en una solicitud.

<a id="ID4ECG"></a>


### <a name="sample-request"></a>Solicitud de ejemplo

La sesión al que hace referencia el **ticketSessionRef** objeto debe crearse antes de que se puede crear una incidencia de coincidencia y la sesión debe contener los jugadores deben coincidir, junto con sus atributos específicos del Reproductor. Cada jugador debe crear o unirse a la sesión en el MPSD, agregar atributos de coincidencia asociada a la sesión. Los atributos de coincidencia se colocan en un campo de propiedad personalizada denominado matchAttrs en cada jugador.

Se envía una solicitud de creación o unión a **https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templatename}/sessions/{sessionname}** y podría parecerse a esto:


```cpp
{
   "members": {
     "me": {
       "constants": {
         "system": {
           "xuid": 2535285330879750
         }
      },
      "properties": {
         "custom": {
           "matchAttrs": {
             "skill": 5,
             "ageRange": "teenager"
           }
         }
      }
    }
  }
}

```


Una vez creada la sesión, el título puede llamar el servicio para crear el vale para esa sesión.


> [!NOTE] 
> Un título puede permitir que un usuario vuelva a intentar esta llamada, pero debe no vuelva a intentar automáticamente si se produce un error en los datos.  



```cpp
POST /serviceconfigs/{scid}/hoppers/{hoppername}

{
  "giveUpDuration":10,
  "preserveSession": "never",
  "ticketSessionRef": {
     "scid": "ABBACDDC-0000-0000-0000-000000000001",  
     "templateName": "TestTemplate",
     "name": "5E55104-0000-0000-0000-000000000001"
  },
  "ticketAttributes": {
    "desiredMap": "Test Map",
    "desiredGameType": "Test GameType"
  }
}

```


<a id="ID4E3G"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

| Miembro| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ticketId| GUID| El identificador para el vale que se está creando.|
| waitTime| entero de 32 bits con signo| El promedio de tiempo de espera para la hopper (número entero de segundos).|


```cpp
{
  "ticketId":"0584338f-a2ff-4eb9-b167-c0e8ddecae72",
  "waitTime":60
}

```


<a id="ID4EHAAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EJAAC"></a>


##### <a name="parent"></a>Primario  

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)
