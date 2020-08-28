---
title: Referencia de API de SSH PIN del portal de dispositivos
description: Aprenda a quitar todos los pin de Secure Shell de confianza (SSH) mediante programación con la API de REST de/ext/App/sshpins Xbox Device portal.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 307af4cdd4e998832f4a2fe7a8f874615fe10ad7
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043537"
---
# <a name="ssh-pins-api-reference"></a>Referencia de API de PIN de SSH
Puede quitar todos los pin SSH de confianza de su DevKit mediante esta API de REST.

## <a name="remove-trusted-ssh-pins"></a>Quitar PIN SSH de confianza

**Solicitud**

Método      | URI de solicitud
:------     | :-----
Delete | /ext/app/sshpins
<br />
**Parámetros de URI**

- None

**Encabezados de solicitud**

- None

**Cuerpo de la solicitud**   

- None

**Respuesta**   

- None 

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
204 | La solicitud para borrar los PIN se realizó correctamente.
4XX | Códigos de error
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox

