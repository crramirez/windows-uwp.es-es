---
title: Referencia de API de credenciales de red de Device Portal
description: Obtén información sobre cómo agregar, quitar o actualizar las credenciales de red mediante programación.
ms.localizationpriority: medium
ms.openlocfilehash: 2da8dae554a0dcbb84d3d3fc3873e2fb035175dc
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8730976"
---
# <a name="network-credentials-api-reference"></a>Referencia de API de credenciales de red
Puedes agregar, quitar o actualizar las credenciales de red almacenado en el Kit de desarrollo con esta API de REST.

## <a name="get-existing-credentials"></a>Obtener las credenciales existentes

**Solicitud**

Puedes obtener una lista de los recursos compartidos almacenados junto con el nombre de usuario del usuario que tenga credenciales de ese recurso compartido de red.

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
  * Nombre de usuario: el nombre de usuario que se almacena las credenciales.

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
| NetworkPath        | Estás agregando credenciales para acceder a la ruta de acceso de red al recurso compartido. |
<br>

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Los siguientes elementos JSON:
* NetworkPath - la ruta de acceso al recurso compartido de red.
* Nombre de usuario: el nombre de usuario para almacenar las credenciales en.
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

## <a name="remove-stored-credentials-for-a-share"></a>Quitar las credenciales almacenadas para un recurso compartido.

**Solicitud**

Método      | URI de solicitud
:------     | :-----
DELETE | /ext/networkcredential
<br />
**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI      | Descripción     | 
| ------------------ |-----------------|
| NetworkPath        | La ruta de acceso de red al recurso compartido que va a quitar las credenciales almacenadas. |
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
204 | La solicitud de las credenciales fue correcta.
4XX | Códigos de error
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox


