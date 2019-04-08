---
title: Referencia de API de información de Xbox de Device Portal
description: Obtén información sobre cómo acceder a la información del dispositivo Xbox.
ms.date: 11/072017
ms.topic: article
keywords: Windows 10, uwp, xbox, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 85c2c139aa8064e1f0769064b95eeb531086b8c1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617500"
---
# <a name="xbox-info-api-reference"></a>Referencia de API de información de Xbox   
Puede acceder a la información del dispositivo a Xbox One mediante esta API.

## <a name="get-xbox-one-device-information"></a>Obtener información del dispositivo Xbox One

**Solicitud**

Puedes obtener información del dispositivo sobre tu Xbox One.

Método      | URI de la solicitud
:------     | :-----
GET | /ext/xbox/info
<br />
**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**   
Un objeto JSON con los siguientes campos:

* OsVersion: (cadena) la versión del sistema operativo.
* OsEdition: (cadena) la edición del sistema operativo, como "marzo de 2017" o "marzo de 2017 QFE 1".
* ConsoleId: (cadena) el identificador de la consola.
* DeviceId: (cadena) el id. de dispositivo de Xbox Live de la consola.
* SerialNumber: (cadena) el número de serie de la consola.
* DevMode: (cadena) el modo de desarrollador actual de la consola, por ejemplo, "None" o "Retail".
* ConsoleType: (cadena) el tipo de consola, como "Xbox One" o "Xbox One S".
* DevkitCertificateExpirationTime: (número) el tiempo (UTC) en segundos en el que expirará el certificado del kit de desarrollo de la consola.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox