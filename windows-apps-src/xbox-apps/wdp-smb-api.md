---
title: Referencia de API de SMB de Device Portal
description: Obtén información sobre cómo tener acceso a las API de SMB mediante programación.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1f0eb76e-fe3e-4674-a27e-229beec7e63d
ms.localizationpriority: medium
ms.openlocfilehash: a1040ec91af767d9472842b5ba656d347e7782d0
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244071"
---
# <a name="developer-folder-api-reference"></a>Referencia de API de carpeta de desarrollador

Puedes obtener acceso a archivos relacionados con el desarrollo en la Xbox One mediante un explorador de archivos estándar. Esto te permite ver y reemplazar archivos fácilmente desde el equipo a la consola.

**Solicitud**

Puedes obtener acceso a la carpeta de desarrollador mediante la siguiente solicitud. La solicitud devolverá:

* La ubicación del recurso compartido de archivos. Esta ubicación puede introducirse en la barra de direcciones de un explorador de archivos.
* El nombre de usuario para acceder al recurso compartido de archivos.
* La contraseña para acceder al recurso compartido de archivos.

Método      | URI de la solicitud
:------     | :-----
GET | /ext/smb/developerfolder

**Parámetros de URI**

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

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Se concedió la solicitud para acceder a las credenciales para el recurso compartido de archivos.
4XX | Códigos de error
5XX | Códigos de error

**Familias de dispositivos disponibles**

* Windows Xbox
