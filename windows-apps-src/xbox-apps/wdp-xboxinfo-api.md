---
author: M-Stahl
title: Información del Portal Xbox dispositivo de referencia de la API
description: Obtenga información sobre cómo obtener acceso a información del dispositivo de Xbox.
ms.author: mstahl
ms.date: 11/7/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, xbox, portal de dispositivo
ms.localizationpriority: medium
ms.openlocfilehash: db1df2418a2bb60de4a72f51ad01a0bfd547ec20
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "406261"
---
# <a name="xbox-info-api-reference"></a>Referencia de la API de Info de Xbox   
Puede tener acceso a información del dispositivo Xbox uno con esta API.

## <a name="get-xbox-one-device-information"></a>Obtener información del dispositivo Xbox uno

**Solicitud**

Para obtener información del dispositivo acerca de sus Xbox uno.

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

* OsVersion - (cadena) de la versión del sistema operativo.
* OsEdition - (String) la edición del sistema operativo, como "Marzo de 2017" o "marzo de 2017 QFE 1".
* ConsoleId - identificador (de. de cadena) la consola
* DeviceId - Xbox Live dispositivo de (String) la consola identificador.
* SerialNumber - número de serie de (String) la consola.
* DevMode - modo de programador actual de (String) la consola, como "None" o "Comercial".
* ConsoleType - la consola del tipo String) (, como "Xbox uno" o "Xbox un S".
* DevkitCertificateExpirationTime - (número) la hora UTC tiempo en segundos cuando caduca certificado de kit de desarrollo de la consola.

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