---
title: Referencia de API de captura multimedia
description: Obtenga información sobre cómo capturar una representación PNG de la pantalla actual mediante la API de REST del portal de dispositivos Xbox.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: ee1ccba3fe2a3f83a95c3538cb267730f7770c4c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172739"
---
# <a name="media-capture-api-reference"></a>Referencia de API de captura multimedia #

## <a name="request"></a>Solicitud

Puedes capturar una representación de PNG de la pantalla actual mediante el siguiente formato de solicitud.

| Método        | URI de solicitud     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:


| Parámetro de URI      | Descripción     | 
| ------------------ |-----------------|
| descargar (opcional)| Un valor booleano que indica si los encabezados de respuesta HTTP deben configurarse para indicar que el explorador del host debe descargar la captura de pantalla como datos adjuntos en lugar de representarla en el explorador.  |

**Encabezados de solicitud**

* None

**Cuerpo de la solicitud**

* None

## <a name="response"></a>Response

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP   | Descripción     | 
| ------------------ |-----------------|
| 200                | Solicitud de captura de pantalla correcta y captura devuelta |
| 5XX                | Códigos de error para errores inesperados |
<br>

**Familias de dispositivos disponibles**

* Windows Xbox

