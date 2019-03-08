---
ms.assetid: ''
description: Usa este método en la API de análisis de Microsoft Store para descargar el archivo .cab para un error de la aplicación.
title: Descargar el archivo CAB de un error en tu aplicación
ms.date: 06/16/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store analytics API, API de análisis de Microsoft Store, download CAB, descargar .cab
ms.localizationpriority: medium
ms.openlocfilehash: a4643f94236e62c46c12fd656ab5ddba5e1e0632
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594540"
---
# <a name="download-the-cab-file-for-an-error-in-your-app"></a>Descargar el archivo CAB de un error en tu aplicación

Use este método en la API de análisis de Microsoft Store para descargar el archivo .cab que está asociado a un error determinado en la aplicación que se ha notificado al centro de partners. Este método solo puede descargar el archivo .cab para un error en la aplicación que se haya producido en los últimos 30 días. Descargas de archivos CAB también están disponibles en el **errores** sección de la [informe de mantenimiento](../publish/health-report.md) en el centro de partners.

Antes de poder usar este método, tienes que emplear primero el método para [obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md) y recuperar el id. del archivo .cab que quieres descargar.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén el id. del archivo .cab que quieres descargar. Para obtener este identificador, usa el método [obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md) para recuperar los detalles de un error específico de tu aplicación y usa el valor **cabId** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  |
|---------------|--------|---------------|------|
| applicationId | string | El Id. de la Store de la aplicación para la que quieres descargar un archivo CAB. El identificador de Store está disponible en el [página identidad de aplicación](../publish/view-app-identity-details.md) del centro de partners. Un ejemplo de un id. de la Tienda sería 9WZDNCRFJ3Q8. |  Sí  |
| cabId | string | El id. exclusivo del archivo .cab que quieres descargar. Para obtener este identificador, usa el método [obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md) para recuperar los detalles de un error específico de tu aplicación y usa el valor **cabId** en el cuerpo de la respuesta de ese método. |  Sí  |

 
### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo descargar un archivo .cab con este método. Reemplaza los parámetros *applicationId* y *cabId* con los valores correspondientes a la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

Este método devuelve un código de respuesta 302 (redirigir) y el encabezado **Ubicación** de la respuesta se asigna al URI de la firma de acceso compartido (SAS) del archivo .cab. El autor de la llamada se redirige a este URI para descargar automáticamente el archivo .cab.

## <a name="related-topics"></a>Temas relacionados

* [Informe de mantenimiento](../publish/health-report.md)
* [Acceder a los datos de análisis con servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de informes de errores](get-error-reporting-data.md)
* [Obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md)
* [Obtener el seguimiento de pila para un error en la aplicación](get-the-stack-trace-for-an-error-in-your-app.md)
