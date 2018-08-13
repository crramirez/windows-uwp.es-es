---
author: WilliamsJason
title: Referencia de API de credenciales de red de Portal de dispositivo
description: Obtenga información sobre cómo agregar, quitar o actualizar las credenciales de red mediante programación.
ms.localizationpriority: medium
ms.openlocfilehash: 7e00169f92ee6f0aa48df64ec4a1186f9682b358
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "410184"
---
# <a name="network-credentials-api-reference"></a>Referencia de la API de credenciales de red
Puede agregar, quitar o actualizar las credenciales de red almacenados en su devkit con esta API de REST.

## <a name="get-existing-credentials"></a>Obtener las credenciales existentes

**Solicitud**

Puede obtener una lista de los recursos compartidos almacenados junto con el nombre de usuario del usuario que tiene credenciales para ese recurso compartido de red.

Método      | URI de solicitud
:------     | :-----
GET | /ext/networkcredential
<br />
**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**   

- Ninguno

**Respuesta**   

- Matriz JSON en el siguiente formato:
* Credenciales
  * NetworkPath - la ruta de acceso al recurso compartido de red.
  * Nombre de usuario - el nombre de usuario que se almacena las credenciales.

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Correcto
4XX | Códigos de error
5XX | Códigos de error

## <a name="add-or-update-stored-credentials-for-a-user"></a>Agregar o actualizar las credenciales almacenadas para un usuario

**Solicitud**

Método      | URI de solicitud
:------     | :-----
POST | /ext/networkcredential
<br />
**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI      | Descripción     | 
| ------------------ |-----------------|
| NetworkPath        | La ruta de acceso de red al recurso compartido va a agregar las credenciales para tener acceso a. |
<br>

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Los siguientes elementos JSON:
* NetworkPath - la ruta de acceso al recurso compartido de red.
* Nombre de usuario - el nombre de usuario para almacenar las credenciales en.
* Contraseña: la contraseña nueva o actualizada para este usuario.

**Respuesta**   

- Ninguno  

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
204 | Correcto
4XX | Códigos de error
5XX | Códigos de error

## <a name="remove-stored-credentials-for-a-share"></a>Eliminar credenciales almacenadas para un recurso compartido.

**Solicitud**

Método      | URI de solicitud
:------     | :-----
DELETE | /ext/networkcredential
<br />
**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI      | Descripción     | 
| ------------------ |-----------------|
| NetworkPath        | La ruta de acceso de red al recurso compartido desde el que se van a eliminar credenciales almacenadas. |
<br>

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**   

- Ninguno

**Respuesta**   

- Ninguno 

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
204 | La solicitud a las credenciales es correcta.
4XX | Códigos de error
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox


