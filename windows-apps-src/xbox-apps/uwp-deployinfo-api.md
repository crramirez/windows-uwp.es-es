---
title: Referencia de API de información de implementación de portal de dispositivos
description: Obtenga información sobre cómo usar la API de REST del portal de dispositivos Xbox deployinfo para solicitar información de implementación de uno o más paquetes instalados.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 5260125625ced6c258a683bcfb9b552e57d07f06
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943005"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>Solicita información de implementación de uno o más paquetes instalados.

**Solicitud**

Método      | URI de solicitud
:------     | :------
POST | /ext/app/deployinfo
<br />
**Parámetros de URI**

 - Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

Una matriz JSON con el siguiente formato:

* DeployInfo
  * PackageFullName: nombre del paquete al que se solicita información.
  * OverlayFolder: ruta de acceso opcional a una ruta de acceso de la carpeta de superposición si se usa esta característica.

###<a name="response"></a>Respuesta

**Cuerpo de respuesta**

Una matriz JSON en el formato siguiente (algunos campos son opcionales):

* DeployInfo
  * PackageFullName: nombre del paquete sobre el que se va a recibir información.
  * DeployType: el tipo de implementación.
  * DeployPathOrSpecifiers: una ruta de acceso de implementación para implementaciones sueltas o especificadores instalados para implementaciones empaquetadas.
  * DeployDrive: la unidad en la que se implementa el paquete para los tipos de implementación aplicables.
  * DeploySizeInBytes: el tamaño en bytes del paquete para los tipos de implementación aplicables.
  * OverlayFolder: la carpeta de superposición para implementaciones que admiten esta característica.

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