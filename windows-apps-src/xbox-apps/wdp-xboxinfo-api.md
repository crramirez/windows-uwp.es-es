---
author: M-Stahl
title: Información del Portal Xbox de dispositivo de referencia de API
description: Obtén información sobre cómo tener acceso a información de dispositivo de Xbox.
ms.author: mstahl
ms.date: 11/7/2017
ms.topic: article
keywords: Windows 10, uwp, xbox, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 4b0e2bab0ce7d5525e8032809954ff656a74a61c
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "6992140"
---
# <a name="xbox-info-api-reference"></a>Referencia de API de información de Xbox   
Puedes acceder a información del dispositivo Xbox One mediante esta API.

## <a name="get-xbox-one-device-information"></a>Obtener información de dispositivo de Xbox One

**Solicitud**

Para obtener información de dispositivo sobre tu Xbox One.

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

* OsVersion - (cadena) la versión del sistema operativo.
* OsEdition - (cadena) la edición del sistema operativo, por ejemplo, "Marzo de 2017" o "marzo de 2017 QFE 1".
* ConsoleId - Id.. del (cadena) la consola
* DeviceId - Xbox Live dispositivo de (cadena) la consola identificador.
* Número de serie: número de serie del (cadena) la consola.
* DevMode - actual el modo de desarrollador del (cadena) la consola, por ejemplo, "None" o "Comercial".
* ConsoleType - la consola del tipo de cadena) (, como "Xbox One" o "Xbox One S".
* DevkitCertificateExpirationTime - (número) la hora de UTC en segundos expirará un certificado kit de desarrollador de la consola.

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