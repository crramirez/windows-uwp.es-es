---
author: M-Stahl
title: Información de dispositivo Xbox Portal referencia de la API
description: Obtenga información sobre cómo tener acceso a información del dispositivo de Xbox.
ms.author: mstahl
ms.date: 11/7/2017
ms.topic: article
keywords: Windows 10 uwp, xbox, portal de dispositivo
ms.localizationpriority: medium
ms.openlocfilehash: 4b0e2bab0ce7d5525e8032809954ff656a74a61c
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "5919898"
---
# <a name="xbox-info-api-reference"></a>Referencia de la API de Info de Xbox   
Puede tener acceso a información del dispositivo Xbox uno usando este API.

## <a name="get-xbox-one-device-information"></a>Obtener información del dispositivo uno de Xbox

**Solicitud**

Puede obtener información del dispositivo acerca de tu Xbox uno.

Método      | URI de solicitud
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

* OsVersion - (String) la versión del sistema operativo.
* OsEdition - (String) en la edición del sistema operativo, como "Marzo de 2017" o "marzo de 2017 QFE 1".
* ConsoleId - identificador (de. de cadena) la consola
* DeviceId - dispositivo de (cadena) la consola Xbox Live ID.
* Número de serie: número de serie de (String) la consola.
* DevMode - modo de programador actual de (cadena) la consola, como "Ninguno" o "Comercial".
* ConsoleType - la consola del tipo String) (, como "Uno de Xbox" o "S uno de Xbox".
* DevkitCertificateExpirationTime - el tiempo de UTC (número) en segundos cuando caduca certificado de kit de desarrolladores de la consola.

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox