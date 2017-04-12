---
author: WilliamsJason
title: Referencia de API de captura multimedia
description: "Obtén información sobre cómo tener acceso a la API de captura multimedia mediante programación."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.openlocfilehash: 9236b0cd9ac658a34283e54ba70b7e70d19c6bb3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
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

###<a name="response"></a>Respuesta###

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

| Código de estado HTTP   | Descripción     | 
| ------------------ |-----------------|
| 200                | Solicitud de captura de pantalla correcta y captura devuelta |
| 5XX                | Códigos de error para errores inesperados |
<br>

**Familias de dispositivos disponibles**

* Windows Xbox

