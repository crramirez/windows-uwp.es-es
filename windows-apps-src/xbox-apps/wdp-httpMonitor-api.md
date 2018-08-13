---
author: mathy
title: Referencia de API del supervisor de HTTP del Portal de dispositivos
description: Obtén información sobre cómo acceder al tráfico HTTP desde la aplicación centrada en una consola Xbox.
ms.localizationpriority: medium
ms.openlocfilehash: e7ce2ba75eb24f3963818935a7172b25114fd144
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
ms.locfileid: "1397298"
---
# <a name="http-monitor-api-reference"></a>Referencia de API del supervisor de HTTP   
Puede acceder al tráfico de HTTP en tiempo real para la aplicación centrada mediante esta API si se ha habilitado el supervisor de HTTP en la consola Xbox, activando la casilla en Dev Home.

## <a name="get-if-the-http-monitor-is-enabled"></a>Obtener si el supervisor de HTTP está habilitado

**Solicitud**

Puedes obtener si se ha habilitado el supervisor de HTTP en Dev Home.

Método      | URI de solicitud
:------     | :-----
GET | /ext/httpmonitor/sessions
<br />
**Parámetros del URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**   
Un objeto JSON con los siguientes campos:

* Habilitado - (booleano) Si el supervisor de HTTP se ha habilitado en la consola Xbox activando la casilla en Dev Home.

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
5XX | Códigos de error

## <a name="get-http-traffic-from-the-focused-app"></a>Obtener el tráfico de HTTP desde la aplicación centrada
**Solicitud**

Obtén en tiempo real el tráfico de HTTP desde la aplicación centrada en Xbox, siempre que no sea una aplicación de sistema, si se ha habilitado el supervisor de HTTP desde Dev Home.

Método      | URI de solicitud
:------     | :-----
Websocket | /ext/httpmonitor/sessions
<br />
**Parámetros del URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**   
Un objeto JSON con los siguientes campos:

* Sesiones
    * RequestHeaders - (objeto JSON), Los encabezados desde la solicitud de HTTP.
    * RequestContentHeaders - (objeto JSON), Los encabezados de contenido de la solicitud desde la solicitud de HTTP.
    * RequestURL - (cadena) La dirección URL de la solicitud.
    * RequestMethod - (cadena) El método de la solicitud.
    * RequestMessage - (cadena) El mensaje de la solicitud, actualmente solo admite el contenido de JSON y de texto.
    * ResponseHeaders - (objeto JSON), Los encabezados de respuesta desde la respuesta de HTTP.
    * ResponseContentHeaders - (objeto JSON), Los encabezados del contenido de respuesta desde la respuesta de HTTP.
    * StatusCode - (número) El código de estado dela respuesta.
    * ReasponsePhrase - (cadena) La frase del motivo de la respuesta.
    * ResponseMessage - (cadena) El mensaje de respuesta, actualmente solo admite el contenido de JSON y de texto.

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
403 | Supervisor de HTTP deshabilitado, debe habilitarse en Dev Home
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox