---
title: Referencia de API de controladoras del Portal de dispositivos
description: Aprende cómo obtener el número de controladoras físicas conectadas y desactivarlas mediante programación.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8b5061f9193d78d4ff23f5fa707b0bea67a10f98
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657010"
---
# <a name="controller-api-reference"></a>Referencia de API de controladora   
Puedes obtener el número de controladoras físicas conectadas y desactivarlas con esta API REST.

## <a name="determine-the-number-of-attached-physical-controllers"></a>Determinar el número de controladoras físicas conectadas

**Solicitud**

Puedes comprobar el número de controladoras físicas conectadas en el dispositivo con la siguiente solicitud.

Método      | URI de la solicitud
:------     | :-----
GET | /ext/remoteinput/controllers
<br />
**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**   

- Ninguno

**Respuesta**   

- Propiedad de número JSON ConnectedControllerCount que especifica el número de dispositivos físicos conectados.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Correcto
4XX | Códigos de error
5XX | Códigos de error

## <a name="disconnect-all-physical-controllers-on-the-devkit"></a>Desconectar todos los controladores físicos del kit de desarrollo

**Solicitud**

Puedes desconectar todos las controladoras del dispositivo con la siguiente solicitud.

Método      | URI de la solicitud
:------     | :-----
DELETE | /ext/remoteinput/controllers
<br />
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
204 | La solicitud de desconectar controladoras fue correcta.
4XX | Códigos de error
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox
