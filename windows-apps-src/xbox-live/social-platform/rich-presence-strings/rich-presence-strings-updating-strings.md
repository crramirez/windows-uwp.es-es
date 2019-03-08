---
title: Actualizando cadenas de presencia de enriquecido
description: Obtenga información sobre cómo actualizar una cadena de la presencia de Xbox Live enriquecido.
ms.assetid: eb2bb82e-8730-4d74-9b33-95d133360e44
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, presencia enriquecida
ms.localizationpriority: medium
ms.openlocfilehash: ac4549301c60eafb935dab0ac9c5b5028452edfb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610400"
---
# <a name="rich-presence-updating-strings"></a>Actualizando cadenas de presencia de enriquecido

Para actualizar la cadena de presencia enriquecido en su título, puede llamar a la dirección URI escribir título con los parámetros adecuados en un objeto JSON. También se incluye esta llamada restful mediante la API de servicio de Xbox. Consulte **Microsoft.Xbox.Services.Presence Namespace** para obtener información sobre la API relacionada.

El identificador URI tiene este aspecto:

          POST /users/xuid({xuid})/devices/current/titles/current

A continuación se muestran solo los campos de configuración de las cadenas de presencia enriquecida. Hay otros campos opcionales relacionados con la presencia de escritura para un título no aparece aquí.

## <a name="titlerequest-object"></a>Objeto TitleRequest

Propiedad | Tipo | Solicitado | Descripción
---|---|---|---
Actividad|ActivityRequest|N|Registro que describe la información de título (presencia enriquecida y medios de información, si está disponible)

## <a name="activityrequest-object"></a>Objeto ActivityRequest

Propiedad | Tipo | Solicitado | Descripción
---|---|---|---
richPresence|RichPresenceRequest|N|FriendlyName de la cadena de presencia enriquecida que se debe usar.

## <a name="richpresencerequest-object"></a>Objeto RichPresenceRequest

Propiedad | Tipo | Solicitado | Descripción
---|---|---|---
Id|Cadena|esté|FriendlyName de la cadena de presencia enriquecida que se debe usar
¿scid|Cadena|esté|¿Scid que nos indica donde se definen las cadenas de presencia enriquecida.

Por ejemplo, si quisiera actualizar la presencia enriquecida para el usuario cuyo xuid es 12345, mi llamada sería como sigue:

          POST /users/xuid(12345)/devices/current/titles/current


Con el siguiente cuerpo JSON:

```json
          {
            activity:
            {
              richPresence:
              {
                id:"playingMap",
                scid:"0000-0000-0000-0000-01010101"
              }
            }
          }
```

Mediante la API de contenedor, sería una llamada a **PresenceService.SetPresenceAsync (método)**

Si mantiene la plataforma de datos actualizados, no tendrá que restablecer la cadena de presencia enriquecida cada vez los datos para rellenar los cambios en blanco. En el ejemplo anterior se sabe que va a utilizar el objeto map actual. Presencia buscará los datos en la plataforma de datos cuando un usuario intenta leer la cadena para rellenar el valor actual. Por lo que incluso si el Reproductor cambia de mapa a map para asignar, no tiene que restablecer la cadena de presencia enriquecida en el juego, siempre que envía los eventos correspondientes a la plataforma de datos. Tenga en cuenta que puede tardar unos segundos para los datos para encontrar el camino a través de la plataforma de datos.

A continuación, cuando alguien intenta leer la presencia de usuario del 12345 enriquecido, examine qué configuración regional que se está solicitando el servicio y la cadena de formato adecuadamente antes de devolver.

En este caso, supongamos que un usuario desea leer la cadena en-US. Lectura de presencia enriquecida funcionaría como sigue (para obtener más información acerca de esta llamada, vea **GET (/users/xuid({xuid}))**

          GET /users/xuid(12345)?level=all

El contenedor de la API para esto es **PresenceService.GetPresenceAsync (método)**

¿Qué sucede aquí es que se está solicitando el PresenceRecord del usuario, cuyo xuid es 12345. Y que está solicitando que el nivel de detalle sea "all". Si no se especifica "all", no se devolverán presencia enriquecida.

Y eso podría devolver lo siguiente en la respuesta JSON:

```json
          {
            xuid:"12345",
            state:"online",
            devices:
            [
              {
                type:"D",
                titles:
                [
                  {
                    id:"12345",
                    name:"Buckets are Awesome",
                    lastModified:"2012-09-17T07:15:23.4930000",
                    placement: "full",
                    state:"active",
                    activity:
                    {
                      richPresence:"Playing on map:Mountains"
                    }
                  }
                ]
              }
            ]
          }
```
