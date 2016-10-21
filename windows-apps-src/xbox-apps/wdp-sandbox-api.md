---
author: payzer
title: Referencia de API de espacio aislado de Xbox Live de Device Portal
description: "Obtén información sobre cómo tener acceso al espacio aislado de Xbox Live mediante programación."
translationtype: Human Translation
ms.sourcegitcommit: a857ba338a971e651653193ff2149f08b1665a36
ms.openlocfilehash: 2a0bfa2eecffb2b0f5ed0bc691cb90bcd7191321

---

# Referencia de API de espacio aislado de Xbox Live   
Puedes obtener y configurar el espacio aislado de Xbox Live con esta API de REST.

## Obtener el espacio aislado de Xbox Live

**Solicitud**

Puedes leer el valor actual del espacio aislado de Xbox Live del dispositivo con la siguiente solicitud:

Método      | URI de la solicitud
:------     | :-----
GET | /ext/xboxlive/sandbox
<br />
**Parámetros del URI**

- Ninguno

**Encabezados de la solicitud**

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

## Configurar el espacio aislado de Xbox Live
Puedes cambiar el espacio aislado de Xbox Live del dispositivo con la siguiente solicitud. Ten en cuenta que en Xbox One, el dispositivo debe reiniciarse para que la configuración surta efecto.

**Solicitud**

Puedes configurar el valor actual del espacio aislado de Xbox Live del dispositivo con la siguiente solicitud:

Método      | URI de la solicitud
:------     | :-----
PUT | /ext/xboxlive/sandbox
<br />
**Parámetros del URI**

- Ninguno

**Encabezados de la solicitud**

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




<!--HONumber=Aug16_HO3-->


