---
title: Referencia de API de captura multimedia
description: Obtén información sobre cómo tener acceso a la API de captura multimedia mediante programación.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: 7dcd4c6c39a983ab11bfacd391bfa78942601258
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244061"
---
# <a name="media-capture-api-reference"></a>Referencia de API de captura multimedia #

## <a name="request"></a>Solicitud

Puedes capturar una representación de PNG de la pantalla actual mediante el siguiente formato de solicitud.

| Método        | URI de la solicitud     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:


| Parámetro de URI      | Descripción     | 
| ------------------ |-----------------|
| descargar (opcional)| Un valor booleano que indica si los encabezados de respuesta HTTP deben configurarse para indicar que el explorador del host debe descargar la captura de pantalla como datos adjuntos en lugar de representarla en el explorador.  |

**Encabezados de solicitud**

* Ninguno

**Cuerpo de la solicitud**

* Ninguno

## <a name="response"></a>Respuesta

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP   | Descripción     | 
| ------------------ |-----------------|
| 200                | Solicitud de captura de pantalla correcta y captura devuelta |
| 5XX                | Códigos de error para errores inesperados |
<br>

**Familias de dispositivos disponibles**

* Windows Xbox

