---
title: Referencia de API de Administración de usuarios de prueba de Xbox Live
description: Obtén información sobre cómo tener acceso a las API de Administración de usuarios mediante programación.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 70876ab6-8222-4940-b4fb-65b581a77d6a
ms.openlocfilehash: c934a88dd1825fb0111083d71eb25e477956d79c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627370"
---
#<a name="xbox-live-user-management"></a>Administración # de Xbox Live del usuario

**Solicitud**

Puedes obtener la lista de los usuarios en la consola o actualizar la lista al agregar, quitar, iniciar sesión, cerrar sesión o modificar los usuarios existentes.

| Método        | URI de la solicitud     | 
| ------------- |-----------------|
| GET           | /ext/user |
| PUT           | /ext/user |
<br>

**Parámetros de URI**

* Ninguno

**Encabezados de solicitud**

* Ninguno

**Cuerpo de la solicitud**

Las llamadas a PUT deben incluir una matriz JSON con la estructura siguiente:

* Usuarios
  * AutoSignIn (opcional): bool que habilita o deshabilita el inicio de sesión automático para la cuenta que especifica EmailAddress o UserId.
  * Dirección de correo electrónico (opcional: se debe proporcionar si el identificador de usuario no se proporciona a menos que el inicio de sesión en un usuario patrocinado): Especifica el usuario para modificar, agregar o eliminar la dirección de correo electrónico.
  * Contraseña (opcional: se debe proporcionar si el usuario no está actualmente en la consola): Contraseña usada para agregar un nuevo usuario en la consola.
  * SignedIn (opcional): bool que especifica si la cuenta proporcionada debe iniciar o cerrar sesión.
  * Identificador de usuario (opcional: se debe proporcionar si no se proporciona la dirección de correo electrónico, a menos que el inicio de sesión en un usuario patrocinado): Identificador de usuario especificando el usuario para modificar, agregar o eliminar.
  * SponsoredUser (opcional): bool que especifica si agregar un usuario patrocinado.
  * Delete (opcional): valor booleano que especifica para eliminar este usuario de la consola

###<a name="response"></a>Respuesta ###

**Cuerpo de respuesta**

Las llamadas a GET devolverán una matriz JSON con las propiedades siguientes:

* Usuarios
  * AutoSignIn (opcional)
  * EmailAddress (opcional)
  * Gamertag
  * SignedIn
  * UserId
  * XboxUserId
  * SponsoredUser (opcional)
  
**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP   | Descripción     | 
| ------------------ |-----------------|
| 200                | La llamada a GET se realizó correctamente y la matriz JSON de usuarios devueltos en el cuerpo de la respuesta |
| 204                | La llamada a PUT se realizó correctamente y se han actualizado los usuarios de la consola |
| 4XX                | Varios errores de formato o datos de solicitud no válidos |
| 5XX                | Códigos de error para errores inesperados |
<br>


