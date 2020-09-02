---
title: Referencia de API del monitor HTTP del portal de dispositivos
description: Obtenga información sobre cómo acceder al tráfico HTTP en tiempo real desde la aplicación con el foco en una consola Xbox mediante la API de REST del portal de dispositivos Xbox de/ext/httpmonitor/Sessions.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 6ea53f6356aa89a83f3b267a65f65b32aad5749d
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304537"
---
# <a name="http-monitor-api-reference"></a>Referencia de la API del monitor HTTP   
Puede acceder al tráfico HTTP en tiempo real de la aplicación con el foco mediante esta API si el monitor HTTP se ha habilitado en la consola Xbox. para ello, active la casilla en dev Home.

## <a name="get-if-the-http-monitor-is-enabled"></a>Obtener si está habilitado el monitor HTTP

**Solicitud**

Puede obtener si el monitor HTTP se ha habilitado en dev Home.

Método      | URI de solicitud
:------     | :-----
GET | /ext/httpmonitor/sessions

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Ante**   
Un objeto JSON con los siguientes campos:

* Habilitado: (bool) si el monitor HTTP se ha habilitado en la consola Xbox activando la casilla de dev Home.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
5XX | Códigos de error

## <a name="get-http-traffic-from-the-focused-app"></a>Obtención del tráfico HTTP desde la aplicación con el foco

**Solicitud**

Obtiene el tráfico HTTP desde la aplicación con el foco en la Xbox, siempre y cuando no sea una aplicación del sistema, en tiempo real, si el monitor HTTP se ha habilitado desde dev Home.

Método      | URI de solicitud
:------     | :-----
WebSocket | /ext/httpmonitor/sessions

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Ante**   
Un objeto JSON con los siguientes campos:

* Sesiones
    * RequestHeaders: (objeto JSON) los encabezados de solicitud de la solicitud HTTP.
    * RequestContentHeaders: (objeto JSON) los encabezados de contenido de la solicitud de la solicitud HTTP.
    * RequestURL: (cadena) dirección URL de la solicitud.
    * RequestMethod: (cadena) el método de solicitud.
    * RequestMessage: (cadena) el mensaje de solicitud; actualmente solo admite contenido JSON y texto.
    * ResponseHeaders: (objeto JSON) los encabezados de respuesta de la respuesta HTTP.
    * ResponseContentHeaders: (objeto JSON) los encabezados de contenido de respuesta de la respuesta HTTP.
    * StatusCode: (número) código de estado de respuesta.
    * ReasponsePhrase: (cadena) la frase del motivo de la respuesta.
    * ResponseMessage: (cadena) el mensaje de respuesta; actualmente solo admite contenido JSON y texto.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
403 | El monitor HTTP está deshabilitado, debe estar habilitado en dev Home
5XX | Códigos de error


**Familias de dispositivos disponibles**

* Windows Xbox