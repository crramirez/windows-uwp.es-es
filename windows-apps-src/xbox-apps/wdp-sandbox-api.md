---
title: Referencia de API de espacio aislado de Xbox Live de Device Portal
description: Obtenga información sobre cómo obtener y establecer el valor para el espacio aislado de Xbox Live del dispositivo mediante la API de REST del portal de dispositivos Xbox.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 72c7459c-420a-4da9-8afa-191a846185a5
ms.localizationpriority: medium
ms.openlocfilehash: ebe280af4279719109db2e36904b501623956339
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166809"
---
# <a name="xbox-live-sandbox-api-reference"></a>Referencia de API de espacio aislado de Xbox Live   
Puedes obtener y configurar el espacio aislado de Xbox Live con esta API de REST.

## <a name="get-the-xbox-live-sandbox"></a>Obtener el espacio aislado de Xbox Live

**Solicitud**

Puedes leer el valor actual del espacio aislado de Xbox Live del dispositivo con la siguiente solicitud:

Método      | URI de solicitud
:------     | :-----
GET | /ext/xboxlive/sandbox

**Parámetros de URI**

- None

**Encabezados de solicitud**

- None

**Cuerpo de la solicitud**

- None

**Ante**   
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

Método      | URI de solicitud
:------     | :-----
PUT | /ext/xboxlive/sandbox

**Parámetros de URI**

- None

**Encabezados de solicitud**

- None

**Cuerpo de la solicitud**   
El cuerpo de la solicitud es un objeto JSON que contiene el campo siguiente:   
Espacio aislado: (cadena) el nuevo valor en el que configurar el espacio aislado del dispositivo.

**Ante**   
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

