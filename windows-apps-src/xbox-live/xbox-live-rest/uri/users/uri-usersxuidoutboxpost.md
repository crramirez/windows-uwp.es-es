---
title: POST (/users/xuid({xuid})/outbox)
assetID: de991d88-efe0-04f2-f6b2-0bc3e68bfd46
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutboxpost.html
description: " POST (/users/xuid({xuid})/outbox)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2b225e8441fee3d499f172e2e5701096cdbc161a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594290"
---
# <a name="post-usersxuidxuidoutbox"></a>POST (/users/xuid({xuid})/outbox)
Envía un mensaje especificado a una lista de destinatarios.
Es el dominio para estos URI `msg.xboxlive.com`.

  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EAB)
  * [Autorización](#ID4ENB)
  * [Efecto de la configuración de privacidad en recurso](#ID4EYB)
  * [Cuerpo de la solicitud](#ID4E3F)
  * [Códigos de estado HTTP](#ID4ETCAC)
  * [Cuerpo de respuesta](#ID4E1EAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Observaciones

El tipo de contenido solo es compatible con esta API es "application/json", que es necesario en los encabezados HTTP de cada llamada.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| xuid | entero de 64 bits sin signo | La consola Xbox usuario ID (XUID) del Reproductor que realiza la solicitud. |

<a id="ID4ENB"></a>


## <a name="authorization"></a>Autorización

Debe tener su propia notificación de usuario y una suscripción válida a gold para enviar un mensaje de usuario.

<a id="ID4EYB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Efecto de la configuración de privacidad en recurso

Enviar un mensaje de usuario correctamente a un Reproductor, si ese jugador es de confianza o no, da como resultado un código de resultado de 200. Sin embargo, si envía un mensaje a alguien que le ha bloqueado, el destinatario no recibirá el mensaje y no recibirá ninguna indicación de que el mensaje no era correcta.

También hay límites en cuántos mensajes pueden enviarse al día y a cuántos de sus amigos y que no sean tus amigos, como se indica a continuación.

   * 20 extraños por mensaje
   * 200 extraños por 24 horas
   * total de 250 mensajes por 24 horas
   * 2500 destinatarios en total por cada 24 horas

| Solicitud de usuario| Configuración de privacidad del usuario de destino| Comportamiento|
| --- | --- | --- | --- | --- | --- |
| Me| -| Como se describe.|
| Friend| Todo el mundo| 200 correctos|
| Friend| solo los amigos| 200 correctos|
| Friend| bloqueado| 200 correctos|
| usuario de no confianza| Todo el mundo| 200 correctos|
| usuario de no confianza| solo los amigos| 200 correctos|
| usuario de no confianza| bloqueado| 200 correctos|
| sitio de terceros| Todo el mundo| 200 correctos|
| sitio de terceros| solo los amigos| 200 correctos|
| sitio de terceros| bloqueado| 200 correctos|

<a id="ID4E3F"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

| Propiedad| Tipo| Longitud máxima| Consumidores| Observaciones|
| --- | --- | --- | --- | --- |
| header| Encabezado|  | Todos| Encabezado del mensaje de usuario|
| messageText| string| 250| Todas las plataformas excepto Windows 8| Texto del mensaje de usuario (UTF-8)|

#### <a name="header"></a>Encabezado

| Propiedad| Tipo| Longitud máxima| Consumidores| Observaciones|
| --- | --- | --- | --- | --- |
| destinatarios| User[]| 20| Todos| Lista de los destinatarios del mensaje|

#### <a name="user"></a>Usuario

| Propiedad| Tipo| Longitud máxima| Consumidores| Observaciones|
| --- | --- | --- | --- | --- |
| xuid| ulong|  | Todos| XUID del destinatario. No se utiliza si se envía el nombre de jugador.|
| gamertag| string| 15| Todos| Nombre de jugador del destinatario. No se utiliza si se envía XUID.|

#### <a name="sample-request-body"></a>Cuerpo de la solicitud de ejemplo 

```cpp
{
          "header":
          {
            "recipients":
            [{"gamertag":"GoTeamEmily"},
            {"gamertag":"Longstreet360"}]
          },
          "messageText":"random user text"
        }

```


<a id="ID4ETCAC"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP

El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).

| Código| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| Proceso completado correctamente.|
| 400| Lista de destinatarios está vacío o supera la longitud máxima; o se especificaron el nombre de jugador y XUID; o messageText es demasiado largo.|
| 403| No se puede convertir XUID.|
| 404| Nombre de jugador no es válido o no se encuentra el usuario.|
| 409| Usuario ha alcanzado el límite diario de impuesto por el sistema.|
| 500| Error general al servidor.|

<a id="ID4E1EAC"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

Ningún objeto que se envía en el cuerpo de la respuesta.

<a id="ID4EJFAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4ELFAC"></a>


##### <a name="parent"></a>Primario  

[/users/xuid({xuid})/outbox](uri-usersxuidoutbox.md)


<a id="ID4EZFAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Referencia [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md)
