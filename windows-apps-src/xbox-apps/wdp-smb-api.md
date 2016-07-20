---
author: payzer
title: Referencia de API de SMB de Device Portal
description: "Obtén información sobre cómo tener acceso a las API de SMB mediante programación."
translationtype: Human Translation
ms.sourcegitcommit: 3d76bf181baa9dfd973467d43241230fddf2daf7
ms.openlocfilehash: 5efe2af3524d97e6014c4d6be2a8f1aef22f2e66

---

# Referencia de API de carpeta de desarrollador   
Puedes obtener acceso a archivos relacionados con el desarrollo en la Xbox One mediante un explorador de archivos estándar. Esto te permite ver y reemplazar archivos fácilmente desde el equipo a la consola.

**Solicitud**

Puedes obtener acceso a la carpeta de desarrollador mediante la siguiente solicitud. La solicitud devolverá:    
* La ubicación del recurso compartido de archivos. Esta ubicación puede introducirse en la barra de direcciones de un explorador de archivos.
* El nombre de usuario para acceder al recurso compartido de archivos.
* La contraseña para acceder al recurso compartido de archivos.

Método      | URI de la solicitud
:------     | :-----
GET | /ext/smb/developerfolder
<br />
**Parámetros del URI**

- Ninguno

**Encabezados de la solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**   
Ruta de acceso: la ruta de acceso al recurso compartido de archivos del desarrollador de archivos.   
Nombre de usuario: nombre de usuario necesario para obtener acceso al recurso compartido de archivos del desarrollador.   
Contraseña: la contraseña necesaria para obtener acceso al recurso compartido de archivos del desarrollador.   

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



<!--HONumber=Jul16_HO1-->


