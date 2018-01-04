---
author: payzer
title: Referencia de API de SMB de Device Portal
description: "Obtén información sobre cómo tener acceso a las API de SMB mediante programación."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 1f0eb76e-fe3e-4674-a27e-229beec7e63d
ms.openlocfilehash: 1bc02780808d5b9fca09576165f428eca1cce715
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: HT
ms.contentlocale: es-ES
---
# <a name="developer-folder-api-reference"></a>Referencia de API de carpeta de desarrollador   
Puedes obtener acceso a archivos relacionados con el desarrollo en la Xbox One mediante un explorador de archivos estándar. Esto te permite ver y reemplazar archivos fácilmente desde el equipo a la consola.

**Solicitud**

Puedes obtener acceso a la carpeta de desarrollador mediante la siguiente solicitud. La solicitud devolverá:    
* La ubicación del recurso compartido de archivos. Esta ubicación puede introducirse en la barra de direcciones de un explorador de archivos.
* El nombre de usuario para acceder al recurso compartido de archivos.
* La contraseña para acceder al recurso compartido de archivos.

Método      | URI de solicitud
:------     | :-----
GET | /ext/smb/developerfolder
<br />
**Parámetros del URI**

- Ninguno

**Encabezados de solicitud**

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
