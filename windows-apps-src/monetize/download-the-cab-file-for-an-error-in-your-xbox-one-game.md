---
description: Use este método en la API de análisis de Microsoft Store para descargar el archivo .cab de un error en el juego de Xbox One.
title: Descargar el archivo CAB de un error de tu juego de Xbox One
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store analytics API, API de análisis de Microsoft Store, download CAB, descargar .cab
ms.localizationpriority: medium
ms.openlocfilehash: 736219533a254e6380c10600e97f707f15e37de6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604330"
---
# <a name="download-the-cab-file-for-an-error-in-your-xbox-one-game"></a>Descargar el archivo CAB de un error de tu juego de Xbox One

Use este método en la API de análisis de Microsoft Store para descargar el archivo .cab que está asociado a un error determinado en el juego de Xbox One que se ingiere mediante el Portal para desarrolladores de Xbox (XDP) y disponible en el panel del centro de partners XDP Analytics. Este método solo puede descargar el archivo .cab de un error que se produjeron en los últimos 30 días.

Para poder usar este método, primero debe usar el [obtener los detalles de un error en Xbox One juego](get-details-for-an-error-in-your-xbox-one-game.md) método para recuperar el identificador del archivo .cab que desea descargar.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén el id. del archivo .cab que quieres descargar. Para obtener este identificador, utilice el [obtener los detalles de un error en Xbox One juego](get-details-for-an-error-in-your-xbox-one-game.md) método para recuperar los detalles de un error específico en la aplicación y usar el **cabId** valor en el cuerpo de respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  |
|---------------|--------|---------------|------|
| applicationId | string | El identificador de producto del juego de Xbox One que va a descargar el archivo CAB. Para obtener el id. del producto de tu juego, ve a tu juego en el Portal de desarrollador de Xbox (XDP) y recupera el id. del producto desde la dirección URL. Como alternativa, si descarga los datos de estado desde el informe de análisis de centro de partners de Windows, el identificador de producto se incluye en el archivo TSV. |  Sí  |
| cabId | string | El id. exclusivo del archivo .cab que quieres descargar. Para obtener este identificador, utilice el [obtener los detalles de un error en Xbox One juego](get-details-for-an-error-in-your-xbox-one-game.md) método para recuperar los detalles de un error específico en la aplicación y usar el **cabId** valor en el cuerpo de respuesta de ese método. |  Sí  |

 
### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo descargar un archivo .cab con este método. Reemplaza los parámetros *applicationId* y *cabId* con los valores correspondientes a la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

Este método devuelve un código de respuesta 302 (redirigir) y el encabezado **Ubicación** de la respuesta se asigna al URI de la firma de acceso compartido (SAS) del archivo .cab. El autor de la llamada se redirige a este URI para descargar automáticamente el archivo .cab.

## <a name="related-topics"></a>Temas relacionados

* [Acceder a los datos de análisis con servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener juegos para Xbox One datos de informe de errores](get-error-reporting-data-for-your-xbox-one-game.md)
* [Obtener los detalles de un error en Xbox One juegos](get-details-for-an-error-in-your-xbox-one-game.md)
* [Obtener el seguimiento de pila para un error en Xbox One juegos](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
