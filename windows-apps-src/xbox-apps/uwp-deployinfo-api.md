---
title: Referencia de API de información de implementación de Device Portal
description: Obtén información sobre cómo acceder a la API de información de implementación mediante programación.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 7543b41c6ee1d9c07f4540012f84dccc10bb4d76
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638010"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>Solicita la información de implementación para uno o más paquetes instalados.

**Solicitud**

Método      | URI de la solicitud
:------     | :------
POST | /ext/app/deployinfo
<br />
**Parámetros de URI**

 - Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

Una matriz JSON en el siguiente formato:

* DeployInfo
  * PackageFullName: Nombre del paquete del que solicitamos información.
  * OverlayFolder: ruta de acceso opcional a una ruta de acceso de carpeta de superposición si se usa esta característica.

###<a name="response"></a>Respuesta

**Cuerpo de respuesta**

Una matriz JSON con el siguiente formato (algunos campos son opcionales):

* DeployInfo
  * PackageFullName: Nombre del paquete del que recibimos información.
  * DeployType: El tipo de implementación.
  * DeployPathOrSpecifiers: Una ruta de implementación para implementaciones sueltas o especificadores instalados para implementaciones empaquetadas.
  * DeployDrive: La unidad en la que se implementa el paquete para tipos de implementación aplicables.
  * DeploySizeInBytes: El tamaño en bytes del paquete para los tipos de implementación aplicables.
  * OverlayFolder: La carpeta de superposición para implementaciones que admiten esta característica.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Correcto
4XX | Códigos de error
5XX | Códigos de error
<br />

**Familias de dispositivos disponibles**

* Windows Xbox