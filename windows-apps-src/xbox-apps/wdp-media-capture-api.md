---
author: WilliamsJason
title: Referencia de API de captura multimedia
description: "Obtén información sobre cómo tener acceso a la API de captura multimedia mediante programación."
translationtype: Human Translation
ms.sourcegitcommit: 4356bd2cfc7697905ed91d60b5829c06d850e109
ms.openlocfilehash: 2f77c565acf272a1a8f147eb07ac6c7f08bf8ef0

---

# Referencia de API de captura multimedia #

**Solicitud**

Puedes capturar una representación de PNG de la pantalla actual mediante el siguiente formato de solicitud.

| Método        | URI de la solicitud     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |
<br>

**Parámetros del URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:


| Parámetro del URI      | Descripción     | 
| ------------------ |-----------------|
| descargar (opcional)| Un valor booleano que indica si los encabezados de respuesta HTTP deben configurarse para indicar que el explorador del host debe descargar la captura de pantalla como datos adjuntos en lugar de representarla en el explorador.  |
<br>

**Encabezados de la solicitud**

* Ninguno

**Cuerpo de la solicitud**

* Ninguno

###Respuesta###

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

| Código de estado HTTP   | Descripción     | 
| ------------------ |-----------------|
| 200                | Solicitud de captura de pantalla correcta y captura devuelta |
| 5XX                | Códigos de error para errores inesperados |
<br>

**Familias de dispositivos disponibles**

* Windows Xbox




<!--HONumber=Aug16_HO3-->


