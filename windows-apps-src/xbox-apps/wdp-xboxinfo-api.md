---
title: Referencia de API de Xbox info del portal de dispositivos
description: Obtenga información sobre cómo acceder a la información de dispositivos de Xbox One mediante el método GET de la API de REST del portal de dispositivos Xbox.
ms.date: 04/18/2019
ms.topic: article
keywords: Windows 10, UWP, Xbox, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: c2a4cecaf3340818b3679dfd3f64b9759ea46752
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174659"
---
# <a name="xbox-info-api-reference"></a>Referencia de la API de información de Xbox   
Puede acceder a la información de dispositivos de Xbox One mediante esta API.

## <a name="get-xbox-one-device-information"></a>Obtención de información de dispositivos de Xbox One

## <a name="request"></a>Solicitud

Puede obtener información del dispositivo sobre su Xbox One.

Método      | URI de solicitud
:------     | :-----
GET | /ext/xbox/info

**Parámetros de URI**

- None

**Encabezados de solicitud**

- None

**Cuerpo de la solicitud**

- None

## <a name="response"></a>Response
Un objeto JSON con los siguientes campos:

* OsVersion-(cadena) versión del sistema operativo.
* OsEdition: (cadena) la edición del sistema operativo, como "marzo 2017" o "QFE de marzo de 2017 1".
* ConsoleId: (cadena) el identificador de la consola.
* DeviceId-(cadena) el ID. de dispositivo de Xbox Live de la consola.
* SerialNumber: (cadena) el número de serie de la consola.
* DevMode: (cadena) el modo de desarrollador actual de la consola, como "ninguno" o "comercial".
* ConsoleType: (cadena) el tipo de la consola, como "Xbox One" o "Xbox One S".
* DevkitCertificateExpirationTime: (número) la hora UTC, en segundos, a la que expirará el certificado del kit para desarrolladores de la consola.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
5XX | Códigos de error

**Familias de dispositivos disponibles**

* Windows Xbox
