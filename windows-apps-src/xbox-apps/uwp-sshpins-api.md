---
title: Referencia de API de anclas SSH del Portal de dispositivos
description: Obtén información sobre cómo quitar todas las anclas SSH de confianza mediante programación.
ms.localizationpriority: medium
ms.openlocfilehash: 1ddf15d3cdb4089a8ef010a4ae46d247a06a10d7
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8705239"
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

