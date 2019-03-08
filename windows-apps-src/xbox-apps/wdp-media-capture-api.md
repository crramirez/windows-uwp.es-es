---
title: Referencia de API de captura multimedia
description: Obtén información sobre cómo tener acceso a la API de captura multimedia mediante programación.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: 7a27d13f7ceedd14a84d5b4b4aa1233445037a1f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640770"
---
# <a name="media-capture-api-reference"></a>Referencia de API de captura multimedia #

**Request**

Puedes capturar una representación de PNG de la pantalla actual mediante el siguiente formato de solicitud.

| Método        | URI de la solicitud     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |
<br>

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:


| Parámetro de URI      | Descripción     | 
| ------------------ |-----------------|
| descargar (opcional)| Un valor booleano que indica si los encabezados de respuesta HTTP deben configurarse para indicar que el explorador del host debe descargar la captura de pantalla como datos adjuntos en lugar de representarla en el explorador.  |
<br>

**Encabezados de solicitud**

* Ninguno

**Cuerpo de la solicitud**

* Ninguno

###<a name="response"></a>Respuesta ###

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP   | Descripción     | 
| ------------------ |-----------------|
| 200                | Solicitud de captura de pantalla correcta y captura devuelta |
| 5XX                | Códigos de error para errores inesperados |
<br>

**Familias de dispositivos disponibles**

* Windows Xbox

