---
title: Referencia de API de espacio aislado de Xbox Live de Device Portal
description: Obtén información sobre cómo tener acceso al espacio aislado de Xbox Live mediante programación.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 72c7459c-420a-4da9-8afa-191a846185a5
ms.localizationpriority: medium
ms.openlocfilehash: 8f04514962cf0684daa99ee75d4c4da73c785735
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244091"
---
# <a name="xbox-live-sandbox-api-reference"></a>Referencia de API de espacio aislado de Xbox Live   
Puedes obtener y configurar el espacio aislado de Xbox Live con esta API de REST.

## <a name="get-the-xbox-live-sandbox"></a>Obtener el espacio aislado de Xbox Live

**Solicitud**

Puedes leer el valor actual del espacio aislado de Xbox Live del dispositivo con la siguiente solicitud:

Método      | URI de la solicitud
:------     | :-----
GET | /ext/xboxlive/sandbox

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**   
Espacio aislado: (cadena) el espacio aislado actual en el que se encuentra el dispositivo.   

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Se concedió la solicitud para acceder a las credenciales para el recurso compartido de archivos.
4XX | Códigos de error
5XX | Códigos de error

## <a name="set-the-xbox-live-sandbox"></a>Configurar el espacio aislado de Xbox Live
Puedes cambiar el espacio aislado de Xbox Live del dispositivo con la siguiente solicitud. Ten en cuenta que en Xbox One, el dispositivo debe reiniciarse para que la configuración surta efecto.

**Solicitud**

Puedes configurar el valor actual del espacio aislado de Xbox Live del dispositivo con la siguiente solicitud:

Método      | URI de la solicitud
:------     | :-----
PUT | /ext/xboxlive/sandbox

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**   
El cuerpo de la solicitud es un objeto JSON que contiene el campo siguiente:   
Espacio aislado: (cadena) el nuevo valor en el que configurar el espacio aislado del dispositivo.

**Respuesta**   
Espacio aislado: (cadena) el espacio aislado actual en el que se encuentra el dispositivo.   

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Se concedió la solicitud para acceder a las credenciales para el recurso compartido de archivos.
4XX | Códigos de error
5XX | Códigos de error

**Familias de dispositivos disponibles**

* Windows Xbox

