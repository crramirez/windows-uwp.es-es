---  
author: WilliamsJason
title: "Referencia de API de Administración de usuarios de prueba de Xbox Live"
description: "Obtén información sobre cómo tener acceso a las API de Administración de usuarios mediante programación."
translationtype: Human Translation
ms.sourcegitcommit: 67f158b1d3d5ece14c36483a2513a2db2f478660
ms.openlocfilehash: 66fe038fdb54ac5cb9086bf9225d0a5d573b39c8

---  

#Administración de usuarios de Xbox Live#

**Solicitud**

Puedes obtener la lista de los usuarios en la consola o actualizar la lista al agregar, quitar, iniciar sesión, cerrar sesión o modificar los usuarios existentes.

| Método        | URI de la solicitud     | 
| ------------- |-----------------|
| GET           | /ext/user |
| PUT           | /ext/user |
<br>

**Parámetros del URI**

* Ninguno

**Encabezados de la solicitud**

* Ninguno

**Cuerpo de la solicitud**

Las llamadas a PUT deben incluir una matriz JSON con la estructura siguiente:

* Usuarios
  * AutoSignIn (opcional): bool que habilita o deshabilita el inicio de sesión automático para la cuenta que especifica EmailAddress o UserId.
  * EmailAddress (opcional: debe proporcionarse si el UserId no se proporciona a menos que inicie sesión un usuario patrocinado): dirección de correo electrónico que especifica el usuario que se debe modificar, agregar o eliminar.
  * Contraseña (opcional: debe proporcionarse si el usuario no está actualmente en la consola): contraseña que se usa para agregar un nuevo usuario a la consola.
  * SignedIn (opcional): bool que especifica si la cuenta proporcionada debe iniciar o cerrar sesión.
  * UserId (opcional: debe proporcionarse si el EmailAddress no se proporciona a menos que inicie sesión un usuario patrocinado): UserId que especifica el usuario que se debe modificar, agregar o eliminar.
  * SponsoredUser (opcional): bool que especifica si agregar un usuario patrocinado.
  * Delete (opcional): bool que especifica eliminar este usuario de la consola

###Respuesta###

**Cuerpo de la respuesta**

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

Esta API tiene los siguientes códigos de estado esperado.

| Código de estado HTTP   | Descripción     | 
| ------------------ |-----------------|
| 200                | La llamada a GET se realizó correctamente y la matriz JSON de usuarios devueltos en el cuerpo de la respuesta |
| 204                | La llamada a PUT se realizó correctamente y se han actualizado los usuarios de la consola |
| 4XX                | Varios errores de formato o datos de solicitud no válidos |
| 5XX                | Códigos de error para errores inesperados |
<br>





<!--HONumber=Aug16_HO3-->


