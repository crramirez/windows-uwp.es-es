---
author: mcleanbyron
ms.assetid: 3D6EE7D7-7D75-499D-AA7A-55DA1C485BA6
description: Usa este método en la API de análisis de Microsoft Store para descargar el archivo .cab para un error de controlador de Windows 10. Este método está previsto solo para IHV.
title: Descargar el archivo .cab para un error de controlador de Windows 10
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store analytics API, API de análisis de Microsoft Store, download CAB, descargar .cab
ms.localizationpriority: medium
ms.openlocfilehash: 98d83dd6bbaeb67f4601315dbb92b1d2cf8dfd23
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/17/2018
ms.locfileid: "1664515"
---
# <a name="download-the-cab-file-for-a-windows-10-driver-error"></a>Descargar el archivo .cab para un error de controlador de Windows 10

Usa este método en la API de análisis de Microsoft Store para descargar el archivo .cab que está asociado con un error concreto de controlador de Windows 10. Antes de poder usar este método, tienes que emplear el método para [obtener los detalles de un error de controlador de Windows 10](get-details-for-a-windows-10-driver-error.md) y recuperar el id. del archivo .cab que quieres descargar.

Puedes obtener otra información sobre errores de hardware OEM con los métodos para [obtener datos de informes de errores para controladores de Windows 10](get-error-reporting-data-for-windows-10-drivers.md) y [obtener los detalles de un error de controlador de Windows 10](get-details-for-a-windows-10-driver-error.md) en la API de análisis de Microsoft Store.

> [!NOTE]
> Solo las cuentas de desarrollador que pertenecen al [programa Centro de desarrollo de hardware de Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) pueden usar este método.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.
* Obtén el id. del archivo .cab que quieres descargar. Para obtener este id., usa el método para [obtener los detalles de un error de controlador de Windows 10](get-details-for-a-windows-10-driver-error.md) para recuperar los detalles de un error específico de controlador y usa el valor **cabIdHash** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/cabdownload``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Necesario  |
|---------------|--------|---------------|------|
| applicationId | cadena | Valor del id. del producto del controlador del cual quieres recuperar los datos de errores. |  Sí  |
| cabIdHash | cadena | El id. exclusivo del archivo .cab que quieres descargar. Para obtener este id., usa el método para [obtener los detalles de un error de controlador de Windows 10](get-details-for-a-windows-10-driver-error.md) para recuperar los detalles de un error específico de la aplicación y usa el valor **cabIdHash** en el cuerpo de la respuesta de ese método. |  Sí  |

 
### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo descargar un archivo .cab con este método.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/cabdownload?applicationId=18430592857500421&cabIdHash=c1a51104-d682-4230-be14-7278b18e3555 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

Este método devuelve un código de respuesta 302 (redirigir) y el encabezado **Ubicación** de la respuesta se asigna al URI de la firma de acceso compartido (SAS) del archivo .cab. El autor de la llamada se redirige a este URI para descargar automáticamente el archivo .cab.

## <a name="related-topics"></a>Temas relacionados

* [Obtener datos de informes de errores para controladores de Windows 10](get-error-reporting-data-for-windows-10-drivers.md)
* [Obtener los detalles de un error de controlador de Windows 10](get-details-for-a-windows-10-driver-error.md)
