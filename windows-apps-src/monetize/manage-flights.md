---
author: Xansky
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: Usa estos métodos en la API de envío de Microsoft Store para administrar paquetes piloto para las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows.
title: Administrar paquetes piloto
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, flights, pilotos
ms.localizationpriority: medium
ms.openlocfilehash: 41cf0d224dfca4d11bbd1e3fde7da44c5201a601
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "5757675"
---
# <a name="manage-package-flights"></a>Administrar paquetes piloto

Usa los siguientes métodos en la API de envío de Microsoft Store para administrar paquetes piloto para tus aplicaciones. Para obtener una introducción a la API de envío de Microsoft Store, incluidos los requisitos previos para usar la API, consulta [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

Estos métodos solo pueden usarse para obtener, crear o eliminar paquetes piloto. Para crear envíos de paquetes piloto, consulta los métodos de [Administrar envíos de paquetes piloto](manage-flight-submissions.md).

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Método</th>
<th align="left">URI</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="get-a-flight.md">Obtener un paquete piloto</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights</td>
<td align="left"><a href="create-a-flight.md">Crear un paquete piloto</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="delete-a-flight.md">Eliminar un paquete piloto</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>Requisitos previos

Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store antes de intentar usar cualquiera de estos métodos.

## <a name="related-topics"></a>Artículos relacionados

* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de paquetes piloto](manage-flight-submissions.md)
