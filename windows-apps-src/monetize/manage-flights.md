---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: "Usa estos métodos en la API de envío de la Tienda Windows para administrar paquetes piloto para las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows."
title: "Administrar paquetes piloto mediante la API de envío de la Tienda Windows"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, API de envío de la Tienda Windows, paquetes piloto, Windows Store submission API, flights"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 51d7481d0491c85bddcae906a846cb8773f33417
ms.lasthandoff: 02/07/2017

---

# <a name="manage-package-flights-using-the-windows-store-submission-api"></a>Administrar paquetes piloto mediante la API de envío de la Tienda Windows

Usa los siguientes métodos en la API de envío de la Tienda Windows para administrar paquetes piloto para tus aplicaciones. Para obtener una introducción a la API de envío de la Tienda Windows, incluidos los requisitos previos para usar la API, consulta [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md).

>**Nota:**&nbsp;&nbsp;estos métodos solo pueden usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. Este permiso se habilita para cuentas de desarrollador en fases y no todas las cuentas tienen este permiso habilitado en este momento. Para solicitar acceso anterior, inicia sesión en el panel del Centro de desarrollo, haz clic en **Comentarios** en la parte inferior del panel, selecciona **API de envío** para el área de comentarios y envía la solicitud. Recibirás un correo electrónico cuando se habilite este permiso para tu cuenta.

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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}```</td>
<td align="left">[Obtener un paquete piloto](get-a-flight.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights```</td>
<td align="left">[Crear un paquete piloto](create-a-flight.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}```</td>
<td align="left">[Eliminar un paquete piloto](delete-a-flight.md)</td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>Requisitos previos

Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de la Tienda Windows antes de intentar usar cualquiera de estos métodos.

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de paquetes piloto](manage-flight-submissions.md)

