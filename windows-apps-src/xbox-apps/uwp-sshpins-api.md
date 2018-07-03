---
author: WilliamsJason
title: Referencia de API de anclas SSH del Portal de dispositivos
description: Obtén información sobre cómo quitar todas las anclas SSH de confianza mediante programación.
ms.localizationpriority: medium
ms.openlocfilehash: 88ba9d3e35650c8c581b9ddb76911636fc18c72e
ms.sourcegitcommit: c104b653601d9b81cfc8bb6032ca434cff8fe9b1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2018
ms.locfileid: "1921287"
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

