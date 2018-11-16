---
author: WilliamsJason
title: Referencia de API de captura multimedia
description: Obtén información sobre cómo tener acceso a la API de captura multimedia mediante programación.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: f58fa4c3a9a1abd407f635f27de3a545c3aafc6c
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6983088"
---
# <a name="media-capture-api-reference"></a>Referencia de API de captura multimedia #

**Solicitud**

Puedes capturar una representación de PNG de la pantalla actual mediante el siguiente formato de solicitud.

| Método        | URI de solicitud     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |
<br>

**Parámetros del URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:


| Parámetro de URI      | Descripción     | 
| ------------------ |-----------------|
| descargar (opcional)| Un valor booleano que indica si los encabezados de respuesta HTTP deben configurarse para indicar que el explorador del host debe descargar la captura de pantalla como datos adjuntos en lugar de representarla en el explorador.  |
<br>

**Encabezados de la solicitud**

* Ninguno

**Cuerpo de la solicitud**

* Ninguno

###<a name="response"></a>Respuesta ###

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

| Código de estado HTTP   | Descripción     | 
| ------------------ |-----------------|
| 200                | Solicitud de captura de pantalla correcta y captura devuelta |
| 5XX                | Códigos de error para errores inesperados |
<br>

**Familias de dispositivos disponibles**

* Windows Xbox

