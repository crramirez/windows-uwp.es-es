---
title: Códigos de estado de sesión multijugador
description: Describe los códigos de estado devueltos desde el servicio Xbox Live cuando se solicita una sesión de varios jugadores.
ms.assetid: 4ab320d6-8050-41a9-9f00-faaad3b128fd
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, varios jugadores 2015, los códigos de estado, la sesión
ms.localizationpriority: medium
ms.openlocfilehash: 8fbddd0070eb24d6fc050c59fa2a0197f98ee08c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646310"
---
# <a name="multiplayer-session-status-codes"></a>Códigos de estado de sesión multijugador

En este tema se enumera los códigos de estado de varios jugadores relativas a las sesiones.

| Nota                                                                                                         |
|---------------------------------------------------------------------------------------------------------------------------|
| Los códigos de estado 4xx devolver la sesión siempre devuelven toda la sesión, incluso si el URI apunta a un elemento de la sesión. |


| Código de estado | Cadena              | Content-Type     | Cuerpo    | Descripción |
|----|
| 200         | Aceptar                  | application/json | sesión | Read (GET) o actualizó (PUT) correctamente.                                                                                                                                                                                                                                                                                                             |
| 201         | Creación             | application/json | sesión | Se creó correctamente.                                                                                                                                                                                                                                                                                                                                 |
| 202         | Aceptado            | text/plain       | ninguno    | La solicitud se aceptó, pero no se ha completado todavía.                                                                                                                                                                                                                                                                                             |
| 204         | Sin contenido          |                  |         | Cuando se OBTIENE de una sesión, la sesión no existe. Cuando se OBTIENE de un elemento de la sesión, la sesión existe pero no el elemento. En PUT para una sesión, se ha eliminado la sesión como resultado de la operación PUT. En PUT o DELETE para un elemento de la sesión, la sesión existente cuando se inició la operación, pero la sesión o el elemento ya no existe. |
| 304         | No modificado        |                  |         | Cuando se OBTIENE con el encabezado If-None-Match, la sesión no ha cambiado.                                                                                                                                                                                                                                                                                        |
| 400         | Solicitud incorrecta         | text/plain       | mensaje | Se supone que la solicitud no es válido en el primer examen. Falta un campo obligatorio o el archivo JSON es incorrecto. El cuerpo incluye detalles adicionales.                                                                                                                                                                                        |
| 403         | prohibido           | text/plain       | mensaje | La solicitud podría ser válida en algunos contextos, pero no es válida para su contexto. Error de autorización.                                                                                                                                                                                                                                                |
|             |                     | application/json | sesión | La sesión no se puede actualizar el usuario, pero se puede leer.                                                                                                                                                                                                                                                                                           |
| 404         | No se encuentra           | text/plain       | mensaje | No se puede acceder a la sesión porque el URI no es válido; no se encuentra el identificador, ¿SCID o plantilla de sesión; no se encuentra un hopper; no se puede acceder a un elemento de sesión porque no cierra la sesión; o bien, la búsqueda de elemento no es válida para la sesión.                                                                                 |
| 405         | Método no permitido  | text/plain       | mensaje | El URI de solicitud es plausible, pero el verbo es incorrecto. Por ejemplo, la solicitud es para una operación POST cuando se necesita una operación PUT.                                                                                                                                                                                                                 |
| 409         | Conflicto            | text/plain       | mensaje | No se pudo actualizar la sesión porque la solicitud no es compatible con la sesión. Por ejemplo, constantes en la solicitud entra en conflicto con constantes en la sesión o la plantilla de sesión o los miembros que no sea el llamador se han agregado a o quitar de una sesión de gran tamaño.                                                                         |
| 412         | Error de condición previa |                  |         | El encabezado If-Match o el encabezado If-None-Match (para una operación que no sean GET), no se pudo cumplir.                                                                                                                                                                                                                                           |
|             |                     | application/json | sesión | No se pudo satisfacer el encabezado If-Match en una operación PUT o DELETE para una sesión existente. El estado actual de la sesión se devuelve junto con el valor ETag actual.                                                                                                                                                                      |
| 429 | Demasiadas solicitudes | application/json | mensaje | Se limitó la llamada al servicio debido a restricciones de limitación de velocidad específica si se supera. Para obtener más información, consulte [bien más preciso limitación de velocidad](../../using-xbox-live/best-practices/fine-grained-rate-limiting.md). |
| 503         | Servicio no disponible | text/plain       | ninguno    | El servicio está sobrecargado y se debe reintentar la solicitud más tarde. Este código incluye un encabezado Retry-After que se debe respetar el cliente.                                                                                                                                                                                                              |
