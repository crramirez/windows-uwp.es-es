---
title: Referencia de API de credenciales de red de Device Portal
description: Obtén información sobre cómo agregar, quitar o actualizar las credenciales de red mediante programación.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: ac30d8db830c51ee40653feb49b443ed44502617
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659220"
---
# <a name="network-credentials-api-reference"></a>Referencia de API de credenciales de red
Puedes agregar, quitar o actualizar las credenciales de red almacenadas en el kit de desarrollo con esta API de REST.

## <a name="get-existing-credentials"></a>Obtener las credenciales existentes

**Request**

Puedes obtener una lista de los recursos compartidos almacenados junto con el nombre de usuario del usuario que tiene credenciales para ese recurso compartido de red.

Método      | URI de la solicitud
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
  * NetworkPath: La ruta de acceso al recurso compartido de red.
  * Username: El nombre de usuario que tiene credenciales almacenadas.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Correcto
4XX | Códigos de error
5XX | Códigos de error

## <a name="add-or-update-stored-credentials-for-a-user"></a>Agregar o actualizar las credenciales almacenadas para un usuario

**Request**

Método      | URI de la solicitud
:------     | :-----
POST | /ext/networkcredential
<br />
**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI      | Descripción     | 
| ------------------ |-----------------|
| NetworkPath        | La ruta de red al recurso compartido al que vas a agregar credenciales para acceder. |
<br>

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Los siguientes elementos JSON:
* NetworkPath: La ruta de acceso al recurso compartido de red.
* Username: El nombre de usuario donde se almacenarán las credenciales.
* Password: La contraseña nueva o actualizada para este usuario.

**Respuesta**   

- Ninguno  

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
204 | Correcto
4XX | Códigos de error
5XX | Códigos de error

## <a name="remove-stored-credentials-for-a-share"></a>Quita las credenciales almacenadas para un recurso compartido.

**Request**

Método      | URI de la solicitud
:------     | :-----
DELETE | /ext/networkcredential
<br />
**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI      | Descripción     | 
| ------------------ |-----------------|
| NetworkPath        | La ruta de red al recurso compartido del que vas a quitar las credenciales almacenadas. |
<br>

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**   

- Ninguno

**Respuesta**   

- Ninguno 

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
204 | La solicitud de las credenciales fue correcta.
4XX | Códigos de error
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox


