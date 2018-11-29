---
title: Referencia de API de información de implementación de Device Portal
description: Obtén información sobre cómo acceder a la API de información de implementación mediante programación.
ms.localizationpriority: medium
ms.openlocfilehash: c44089313b100880b419e9b55a26101e877496f3
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "8198467"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>Solicita la información de implementación para uno o más paquetes instalados.

**Solicitud**

Método      | URI de solicitud
:------     | :------
POST | /ext/app/deployinfo
<br />
**Parámetros del URI**

 - Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

Una matriz JSON en el siguiente formato:

* DeployInfo
  * PackageFullName: Nombre del paquete del que solicitamos información.
  * OverlayFolder: ruta de acceso opcional a una ruta de acceso de carpeta de superposición si se usa esta característica.

###<a name="response"></a>Respuesta

**Cuerpo de la respuesta**

Una matriz JSON con el siguiente formato (algunos campos son opcionales):

* DeployInfo
  * PackageFullName: Nombre del paquete del que recibimos información.
  * DeployType: El tipo de implementación.
  * DeployPathOrSpecifiers: Una ruta de implementación para implementaciones sueltas o especificadores instalados para implementaciones empaquetadas.
  * DeployDrive: La unidad en la que se implementa el paquete para tipos de implementación aplicables.
  * DeploySizeInBytes: El tamaño en bytes del paquete para los tipos de implementación aplicables.
  * OverlayFolder: La carpeta de superposición para implementaciones que admiten esta característica.

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Correcto
4XX | Códigos de error
5XX | Códigos de error
<br />

**Familias de dispositivos disponibles**

* Windows Xbox