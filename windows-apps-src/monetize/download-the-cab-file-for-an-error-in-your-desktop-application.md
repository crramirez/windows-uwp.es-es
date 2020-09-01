---
description: Use este método en la API de Microsoft Store Analytics para descargar el archivo. CAB para un error en la aplicación de escritorio.
title: Descargar el archivo CAB asociado a un error en la aplicación de escritorio
ms.date: 03/06/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de análisis, descargar CAB, aplicación de escritorio
ms.localizationpriority: medium
ms.openlocfilehash: 98a0d6931a59e037bb15679a20fae11eb82dae32
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162499"
---
# <a name="download-the-cab-file-for-an-error-in-your-desktop-application"></a>Descargar el archivo CAB asociado a un error en la aplicación de escritorio

Use este método en la API de Microsoft Store Analytics para descargar el archivo. CAB que está asociado a un error determinado para una aplicación de escritorio que ha agregado al [programa de aplicación de escritorio de Windows](/windows/desktop/appxpkg/windows-desktop-application-program). Este método solo puede descargar el archivo. CAB para un error de aplicación que se ha producido en los últimos 30 días. Las descargas de archivos. CAB también están disponibles en el [Informe de mantenimiento](/windows/desktop/appxpkg/windows-desktop-application-program) de las aplicaciones de escritorio del centro de Partners.

Para poder usar este método, primero debe usar el método [obtener detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md) para recuperar el hash de identificador del archivo. cab que desea descargar.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtenga el hash de identificador del archivo. CAB que desea descargar. Para obtener este valor, use el método [obtener detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md) para recuperar los detalles de un error específico en la aplicación y use el valor **cabIdHash** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  |
|---------------|--------|---------------|------|
| applicationId | string | El ID. del producto de la aplicación de escritorio para la que desea descargar un archivo. CAB. Para obtener el identificador de producto de una aplicación de escritorio, abra cualquier [Informe del centro de partners para la aplicación de escritorio](/windows/desktop/appxpkg/windows-desktop-application-program) (como el **Informe de mantenimiento**) y recupere el identificador de producto de la dirección URL. |  Sí  |
| cabIdHash | string | El hash de identificador único del archivo. CAB que desea descargar. Para obtener este valor, use el método [obtener detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md) para recuperar los detalles de un error específico de la aplicación y use el valor **cabIdHash** en el cuerpo de la respuesta de ese método. |  Sí  |


### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra cómo descargar un archivo CAB mediante este método. Reemplace los parámetros *ApplicationID* y *cabIdHash* por los valores adecuados para la aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

Este método devuelve un código de respuesta 302 (redireccionamiento) y el encabezado de **Ubicación** en la respuesta se asigna al URI de la firma de acceso compartido (SAS) del archivo. cab. El autor de la llamada se redirige a este URI para descargar automáticamente el archivo CAB.

## <a name="related-topics"></a>Temas relacionados

* [Informe de estado](../publish/health-report.md)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de informes de errores de la aplicación de escritorio](get-desktop-application-error-reporting-data.md)
* [Obtener los detalles asociados a un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md)
* [Obtener el seguimiento de pila asociado a un error en la aplicación de escritorio](get-the-stack-trace-for-an-error-in-your-desktop-application.md)