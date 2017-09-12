---
author: mcleanbyron
ms.assetid: 
description: "Usa este método en la API de análisis de la Tienda Windows para descargar el archivo .cab para un error de la aplicación."
title: "Descargar el archivo .cab para un error de la aplicación"
ms.author: mcleans
ms.date: 06/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, Windows 10, uwp, UWP, Windows Store analytics API, API de análisis de la Tienda Windows, download CAB, descargar .cab"
ms.openlocfilehash: 6c9e3f024a0b222be89ede5cf81a14bc923b7af4
ms.sourcegitcommit: 7aabd2e59d45bbc5512dd4ddd9110ae62b79d552
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2017
---
# <a name="download-the-cab-file-for-an-error-in-your-app"></a>Descargar el archivo .cab para un error de la aplicación

Usa este método en la API de análisis de la Tienda Windows para descargar el archivo .cab que está asociado con un error concreto de la aplicación que se notificó al Centro de desarrollo. Este método solo puede descargar el archivo .cab para un error en la aplicación que se haya producido en los últimos 30 días. Las descargas del archivo .cab también están disponibles en la sección **Errores** del [informe de estado](../publish/health-report.md) en el panel del Centro de desarrollo de Windows.

Antes de poder usar este método, tienes que emplear primero el método para [obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md) y recuperar el id. del archivo .cab que quieres descargar.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.
* Obtén el id. del archivo .cab que quieres descargar. Para obtener este identificador, usa el método [obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md) para recuperar los detalles de un error específico de tu aplicación y usa el valor **cabId** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload``` |

<span/> 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  |
|---------------|--------|---------------|------|
| applicationId | string | El Id. de la Tienda de la aplicación para la que quieres descargar un archivo CAB. El Id. de la Tienda está disponible en la [página Identidad de la aplicación](../publish/view-app-identity-details.md) del panel del Centro de desarrollo. Un ejemplo de un Id. de la Tienda sería 9WZDNCRFJ3Q8. |  Sí  |
| cabId | cadena | El id. exclusivo del archivo .cab que quieres descargar. Para obtener este identificador, usa el método [obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md) para recuperar los detalles de un error específico de tu aplicación y usa el valor **cabId** en el cuerpo de la respuesta de ese método. |  Sí  |

<span/>
 
### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo descargar un archivo .cab con este método. Reemplaza los parámetros *applicationId* y *cabId* con los valores correspondientes a la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

Este método devuelve un código de respuesta 302 (redirigir) y el encabezado **Ubicación** de la respuesta se asigna al URI de la firma de acceso compartido (SAS) del archivo .cab. El autor de la llamada se redirige a este URI para descargar automáticamente el archivo .cab.

## <a name="related-topics"></a>Temas relacionados

* [Informe de estado](../publish/health-report.md)
* [Acceder a los datos de análisis mediante los servicios de la Tienda Windows](access-analytics-data-using-windows-store-services.md)
* [Obtener los datos del informe de errores](get-error-reporting-data.md)
* [Obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md)
* [Obtener el seguimiento de la pila de un error en la aplicación](get-the-stack-trace-for-an-error-in-your-app.md)
