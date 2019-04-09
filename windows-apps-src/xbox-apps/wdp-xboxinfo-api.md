---
title: Referencia de API de información de Xbox de Device Portal
description: Obtén información sobre cómo acceder a la información del dispositivo Xbox.
ms.date: 11/072017
ms.topic: article
keywords: Windows 10, uwp, xbox, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 7aa8b11bc439266d36fbb27a7eaa7b07e924a17c
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244041"
---
# <a name="xbox-info-api-reference"></a>Referencia de API de información de Xbox   
Puede acceder a la información del dispositivo a Xbox One mediante esta API.

## <a name="get-xbox-one-device-information"></a>Obtener información del dispositivo Xbox One

## <a name="request"></a>Solicitud

Puedes obtener información del dispositivo sobre tu Xbox One.

Método      | URI de la solicitud
:------     | :-----
GET | /ext/xbox/info

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

## <a name="response"></a>Respuesta
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

**Familias de dispositivos disponibles**

* Windows Xbox