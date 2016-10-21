---
author: WilliamsJason
title: Referencia de API de Fiddler de Device Portal
description: "Descubre cómo habilitar o deshabilitar el seguimiento de Fiddler mediante programación."
translationtype: Human Translation
ms.sourcegitcommit: 3cc2a4bd1859e46a73f3e806489eac7381fa6c17
ms.openlocfilehash: bd215058c71118d8b3e5ce81e2302ce8b151c3f6

---

# Referencia de API de configuración de Fiddler   
Puedes habilitar y deshabilitar el seguimiento de red de Fiddler en tu kit de desarrollo con esta API de REST.

## Habilitar el seguimiento de Fiddler

**Solicitud**

Puedes habilitar el seguimiento de Fiddler para el kit de desarrollo con la siguiente solicitud.  Ten en cuenta que será necesario reiniciar el dispositivo para que esto surta efecto.

Método      | URI de la solicitud
:------     | :-----
POST | /ext/fiddler
<br />
**Parámetros del URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro del URI      | Descripción     | 
| ------------------ |-----------------|
| proxyAddress       | La dirección IP o el nombre de host del dispositivo que ejecuta Fiddler. |
| proxyPort          | El puerto que Fiddler usa para supervisar el tráfico. El valor predeterminado es 8888. |
| updateCert (opcional)| Un valor booleano que indica si se proporciona el certificado raíz de Fiddler. Debe ser true si Fiddler nunca se ha configurado en este kit de desarrollo o si se ha configurado para un host diferente.  |
<br>

**Encabezados de la solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno si updateCert es false o no se proporciona. En caso contrario, cuerpo de varias partes conforme a HTTP que contiene el archivo FiddlerRoot.cer.

**Respuesta**   

- Ninguna  

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
204 | Se ha aceptado la solicitud para habilitar Fiddler. Fiddler se habilitará la próxima vez que se reinicia el dispositivo.
4XX | Códigos de error
5XX | Códigos de error

## Deshabilitar el seguimiento de Fiddler en el kit de desarrollo

**Solicitud**

Puedes deshabilitar el seguimiento de Fiddler en el dispositivo con la siguiente solicitud. Ten en cuenta que será necesario reiniciar el dispositivo para que esto surta efecto.

Método      | URI de la solicitud
:------     | :-----
DELETE | /ext/fiddler
<br />
**Parámetros del URI**

- Ninguno

**Encabezados de la solicitud**

- Ninguno

**Cuerpo de la solicitud**   

- Ninguno

**Respuesta**   

- Ninguna 

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
204 | La solicitud para deshabilitar el seguimiento de Fiddler se ha realizado correctamente. El seguimiento se deshabilitará en el siguiente reinicio del dispositivo.
4XX | Códigos de error
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox

## Consulta también
- [Configuración de Fiddler para la UWP en Xbox](uwp-fiddler.md)




<!--HONumber=Aug16_HO3-->


