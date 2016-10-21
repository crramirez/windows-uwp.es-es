---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: "Usa estos métodos en la API de envío de la Tienda Windows para administrar paquetes piloto para las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows."
title: "Administrar paquetes piloto mediante la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: de1c23d721cee67d813520e3a23eb553cd90b7e9

---

# Administrar paquetes piloto mediante la API de envío de la Tienda Windows




Usa los métodos siguientes en la API de envío de la Tienda Windows para administrar paquetes piloto para las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows. Para obtener una introducción a la API de envío de la Tienda Windows, incluidos los requisitos previos para usar la API, consulta [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md).

>**Nota**&nbsp;&nbsp;Estos métodos solo pueden usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado. Estos métodos solo pueden usarse para obtener, crear o eliminar paquetes piloto. Para crear envíos de paquetes piloto, consulta los métodos de [Administrar envíos de paquetes piloto](manage-flight-submissions.md).

| Método        | URI    | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` | Obtiene los datos de un paquete piloto para una aplicación que esté registrada en tu cuenta del Centro de desarrollo de Windows. Para obtener más información, consulta [Obtener un paquete piloto](get-a-flight.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` | Crea un nuevo paquete piloto. Para obtener más información, consulta [Crear un paquete piloto](create-a-flight.md).|
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` | Elimina un paquete piloto. Para obtener más información, consulta [Eliminar un paquete piloto](delete-a-flight.md). |


## Requisitos previos

Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de la Tienda Windows antes de intentar usar cualquiera de estos métodos.

## Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de paquetes piloto](manage-flight-submissions.md)



<!--HONumber=Aug16_HO5-->


