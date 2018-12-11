---
description: Usa este método en la API de análisis de Microsoft Store para descargar el archivo .cab para un error de la aplicación de escritorio.
title: Descargar el archivo .cab para un error en tu aplicación de escritorio
ms.date: 03/06/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store analytics API, API de análisis de Microsoft Store, download CAB, descargar .cab, desktop application, aplicación de escritorio
ms.localizationpriority: medium
ms.openlocfilehash: 1e3535f18b8127ea18bca234cdcc9b695e89ebfd
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8918493"
---
# <a name="download-the-cab-file-for-an-error-in-your-desktop-application"></a>Descargar el archivo .cab para un error en tu aplicación de escritorio

Usa este método en la API de análisis de Microsoft Store para descargar el archivo .cab que está asociado con un error concreto de una aplicación de escritorio que agregaste al [Programa de aplicaciones de escritorio de Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504). Este método solo puede descargar el archivo .cab para un error en la aplicación que se haya producido en los últimos 30 días. Descargas del archivo .cab también están disponibles en el [informe de estado](https://msdn.microsoft.com/library/windows/desktop/mt826504) para aplicaciones de escritorio en el centro de partners.

Antes de poder usar este método, tienes que emplear primero el método para [obtener los detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md) y recuperar el hash de id. del archivo .cab que quieres descargar.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.
* Obtén el hash de id. del archivo .cab que quieres descargar. Para obtener este valor, usa el método [obtener los detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md) para recuperar los detalles de un error específico de tu aplicación y usa el valor **cabIdHash** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  |
|---------------|--------|---------------|------|
| applicationId | cadena | El identificador de producto de la aplicación de escritorio de la cual quieres descargar un archivo .cab. Para obtener el identificador de producto de una aplicación de escritorio, abra cualquier [informe de análisis del centro de partners para la aplicación de escritorio](https://msdn.microsoft.com/library/windows/desktop/mt826504) (por ejemplo, el **informe de estado**) y recuperar el identificador de producto de la dirección URL. |  Sí  |
| cabIdHash | cadena | El hash de id. exclusivo del archivo .cab que quieres descargar. Para obtener este valor, usa el método [obtener los detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md) para recuperar los detalles de un error específico de tu aplicación de escritorio y usa el valor **cabIdHash** en el cuerpo de la respuesta de ese método. |  Sí  |


### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo descargar un archivo .cab con este método. Reemplaza los parámetros *applicationId* y *cabIdHash* con los valores correspondientes a la aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

Este método devuelve un código de respuesta 302 (redirigir) y el encabezado **Ubicación** de la respuesta se asigna al URI de la firma de acceso compartido (SAS) del archivo .cab. El autor de la llamada se redirige a este URI para descargar automáticamente el archivo .cab.

## <a name="related-topics"></a>Temas relacionados

* [Informe Estado](../publish/health-report.md)
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de informes de errores para la aplicación de escritorio](get-desktop-application-error-reporting-data.md)
* [Obtener los detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md)
* [Obtener el seguimiento de la pila de un error en la aplicación de escritorio](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
