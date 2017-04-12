---
author: payzer
title: Referencia de API de espacio aislado de Xbox Live de Device Portal
description: "Obtén información sobre cómo tener acceso al espacio aislado de Xbox Live mediante programación."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 72c7459c-420a-4da9-8afa-191a846185a5
ms.openlocfilehash: d31c943336b36c325218b0b2a8830daf54ed25ca
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="xbox-live-sandbox-api-reference"></a>Referencia de API de espacio aislado de Xbox Live   
Puedes obtener y configurar el espacio aislado de Xbox Live con esta API de REST.

## <a name="get-the-xbox-live-sandbox"></a>Obtener el espacio aislado de Xbox Live

**Solicitud**

Puedes leer el valor actual del espacio aislado de Xbox Live del dispositivo con la siguiente solicitud:

Método      | URI de solicitud
:------     | :-----
GET | /ext/xboxlive/sandbox
<br />
**Parámetros del URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**   
Espacio aislado: (cadena) el espacio aislado actual en el que se encuentra el dispositivo.   

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

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
<br />
**Parámetros del URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**   
El cuerpo de la solicitud es un objeto JSON que contiene el campo siguiente:   
Espacio aislado: (cadena) el nuevo valor en el que configurar el espacio aislado del dispositivo.

**Respuesta**   
Espacio aislado: (cadena) el espacio aislado actual en el que se encuentra el dispositivo.   

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Se concedió la solicitud para acceder a las credenciales para el recurso compartido de archivos.
4XX | Códigos de error
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox

