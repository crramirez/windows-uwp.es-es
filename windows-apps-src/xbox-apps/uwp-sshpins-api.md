---
title: Referencia de API de anclas SSH del Portal de dispositivos
description: Obtén información sobre cómo quitar todas las anclas SSH de confianza mediante programación.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 2c7dc6fab021c11c98276ee53af161bea25601a9
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "8874927"
---
# <a name="ssh-pins-api-reference"></a>Referencia de API de anclas SSH
Puedes quitar todas las anclas SSH de confianza de tu kit de desarrollo con esta API de REST.

## <a name="remove-trusted-ssh-pins"></a>Quitar anclas SSH de confianza

**Solicitud**

Método      | URI de solicitud
:------     | :-----
DELETE | /ext/app/sshpins
<br />
**Parámetros del URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**   

- Ninguno

**Respuesta**   

- Ninguno 

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
204 | La solicitud para borrar las anclas fue correcta.
4XX | Códigos de error
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox

