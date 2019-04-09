---
title: Referencia de API de Fiddler de Device Portal
description: Descubre cómo habilitar o deshabilitar el seguimiento de Fiddler mediante programación.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7d4225e-ac2c-41dc-aca7-9b1a95ec590b
ms.localizationpriority: medium
ms.openlocfilehash: 4cbdae1084f96901e90f8237d71bd59bf2d4c592
ms.sourcegitcommit: 681c1e3836d2a51cd3b31d824ece344281932bcd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59240023"
---
# <a name="fiddler-settings-api-reference"></a>Referencia de API de configuración de Fiddler   
Puedes habilitar y deshabilitar el seguimiento de red de Fiddler en tu kit de desarrollo con esta API de REST.

## <a name="determine-if-fiddler-tracing-is-enabled"></a>Determinar si el seguimiento de Fiddler está habilitado

**Solicitud**

Puedes comprobar para ver si el seguimiento de Fiddler está habilitado en el dispositivo con la siguiente solicitud.

Método      | URI de la solicitud
:------     | :-----
GET | /ext/fiddler


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**   

- Ninguno

**Respuesta**   

- Propiedad booleana IsProxyEnabled de JSON que especifica si el proxy está habilitado o no.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Correcto
4XX | Códigos de error
5XX | Códigos de error

## <a name="enable-fiddler-tracing"></a>Habilitar el seguimiento de Fiddler

**Solicitud**

Puedes habilitar el seguimiento de Fiddler para el kit de desarrollo con la siguiente solicitud.  Ten en cuenta que será necesario reiniciar el dispositivo para que esto surta efecto.

Método      | URI de la solicitud
:------     | :-----
EXPONER | /ext/fiddler

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI      | Descripción     | 
| ------------------ |-----------------|
| proxyAddress       | La dirección IP o el nombre de host del dispositivo que ejecuta Fiddler. |
| proxyPort          | El puerto que Fiddler usa para supervisar el tráfico. El valor predeterminado es 8888. |
| updateCert (opcional)| Un valor booleano que indica si se proporciona el certificado raíz de Fiddler. Debe ser true si Fiddler nunca se ha configurado en este kit de desarrollo o si se ha configurado para un host diferente.  |


**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno si updateCert es false o no se proporciona. En caso contrario, cuerpo de varias partes conforme a HTTP que contiene el archivo FiddlerRoot.cer.

**Respuesta**   

- Ninguno  

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
204 | Se ha aceptado la solicitud para habilitar Fiddler. Fiddler se habilitará la próxima vez que se reinicia el dispositivo.
4XX | Códigos de error
5XX | Códigos de error

## <a name="disable-fiddler-tracing-on-the-devkit"></a>Deshabilitar el seguimiento de Fiddler en el kit de desarrollo

**Solicitud**

Puedes deshabilitar el seguimiento de Fiddler en el dispositivo con la siguiente solicitud. Ten en cuenta que será necesario reiniciar el dispositivo para que esto surta efecto.

Método      | URI de la solicitud
:------     | :-----
SUPRIMIR | /ext/fiddler

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**   

- Ninguno

**Respuesta**   

- Ninguno 

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
204 | La solicitud para deshabilitar el seguimiento de Fiddler se ha realizado correctamente. El seguimiento se deshabilitará en el siguiente reinicio del dispositivo.
4XX | Códigos de error
5XX | Códigos de error


**Familias de dispositivos disponibles**

* Windows Xbox

## <a name="see-also"></a>Vea también
- [Configuración de Fiddler para la UWP en Xbox](uwp-fiddler.md)

