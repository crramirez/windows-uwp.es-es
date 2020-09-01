---
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: Use estos métodos en la API de envío de Microsoft Store para recuperar los datos de las aplicaciones que están registradas en su cuenta del centro de Partners.
title: Obtención de datos de la aplicación
ms.date: 02/28/2018
ms.topic: article
keywords: Windows 10, UWP, API de envío de Microsoft Store, datos de la aplicación
ms.localizationpriority: medium
ms.openlocfilehash: 7dfbad9d0aa2bfb69479f168ec262fe67bedb49c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162419"
---
# <a name="get-app-data"></a>Obtención de datos de la aplicación

Use los métodos siguientes en el Microsoft Store API de envío para obtener datos de las aplicaciones existentes en la cuenta del centro de Partners. Para ver una introducción a la API de envío de Microsoft Store, incluidos los requisitos previos para el uso de la API, consulte [crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md).

Antes de poder usar estos métodos, la aplicación ya debe existir en la cuenta del centro de Partners. Para crear o administrar envíos de aplicaciones, consulta los métodos de [Administrar envíos de aplicaciones](manage-app-submissions.md).

| Método | URI                                                                                             | Descripción                                                 |
|------- |------------------------------------------------------------------------------------------------ |------------------------------------------------------------ |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications`                                   | [Obtener datos para todas las aplicaciones](get-all-apps.md)               |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}`                   | [Obtener datos para una aplicación específica](get-an-app.md)                |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts` | [Obtener complementos para una aplicación](get-add-ons-for-an-app.md)         |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights`       | [Obtener paquetes piloto para una aplicación](get-flights-for-an-app.md) |

## <a name="prerequisites"></a>Requisitos previos

Si todavía no lo ha hecho, complete todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de Microsoft Store antes de intentar usar cualquiera de estos métodos.

## <a name="data-resources"></a>Recursos de datos

Los métodos de la API de envío Microsoft Store para obtener datos de aplicación usan los siguientes recursos de datos JSON.

<span id="application_object" />

### <a name="application-resource"></a>Recurso de aplicación

Este recurso representa una aplicación registrada en tu cuenta.

```json
{
  "id": "9NBLGGH4R315",
  "primaryName": "ApiTestApp",
  "packageFamilyName": "30481DevCenterAPITester.ApiTestAppForDevbox_ng6try80pwt52",
  "packageIdentityName": "30481DevCenterAPITester.ApiTestAppForDevbox",
  "publisherName": "CN=…",
  "firstPublishedDate": "1601-01-01T00:00:00Z",
  "lastPublishedApplicationSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621086517"
  },
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621243487"
  },
  "hasAdvancedListingPermission": true
}
```

Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción       |
|-----------------|---------|---------------------|
| id            | string  | Id. de la Tienda de la aplicación. Para obtener más información sobre el identificador de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](../publish/view-app-identity-details.md).   |
| primaryName   | string  | Nombre principal de la aplicación.      |
| packageFamilyName | string  | El nombre de familia de paquete de la aplicación.      |
| packageIdentityName          | string  | El nombre de identidad de paquete de la aplicación.                       |
| publisherName       | string  | El identificador del editor de Windows asociado con la aplicación. Esto corresponde al valor de **paquete/identidad/publicador** que aparece en la página identidad de la [aplicación](../publish/view-app-identity-details.md) de la aplicación en el centro de Partners.       |
| firstPublishedDate      | string  | La fecha en que se publicó la aplicación por primera vez, en formato ISO 8601.   |
| lastPublishedApplicationSubmission       | object | Un [recurso de envío](#submission_object) que proporciona información sobre el último envío publicado para la aplicación.    |
| pendingApplicationSubmission        | object  |  Un [recurso de envío](#submission_object) que proporciona información sobre el envío pendiente actual para la aplicación.   |   
| hasAdvancedListingPermission        | boolean  |  Indica si puede configurar el [gamingOptions](manage-app-submissions.md#gaming-options-object) o los [finalizadores](manage-app-submissions.md#trailer-object) para los envíos de la aplicación. Este valor se aplica a los envíos creados después del 2017 de mayo. |  |


<span id="add-on-object" />

### <a name="add-on-resource"></a>Recurso de complemento

Este recurso proporciona información sobre un complemento.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción         |
|-----------------|---------|----------------------|
| inAppProductId            | string  | Id. de la Tienda del complemento. Este valor lo proporciona la Tienda. Un ejemplo de Id. de la Tienda sería 9NBLGGH4TNMP.   |


<span id="flight-object" />

### <a name="flight-resource"></a>Recursos de paquete piloto

Este recurso proporciona información sobre un paquete piloto para una aplicación.

```json
{
    "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
    "friendlyName": "myflight",
    "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
    },
    "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
    },
    "groupIds": [
        "1152921504606962205"
    ],
    "rankHigherThan": "Non-flighted submission"
}
```

Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción           |
|-----------------|---------|------------------------|
| flightId            | string  | El identificador del paquete piloto. Este valor lo proporciona el centro de Partners.  |
| friendlyName           | string  | El nombre del paquete piloto, según lo especifica el desarrollador.   |
| lastPublishedFlightSubmission       | object | Un [recurso de envío](#submission_object) que proporciona información sobre el último envío publicado para el paquete piloto.   |
| pendingFlightSubmission        | object  |  Un [recurso de envío](#submission_object) que proporciona información sobre el envío pendiente actual para el paquete piloto.  |    
| groupIds           | array  | Una matriz de cadenas que contienen los identificadores de los grupos de pilotos asociados con el paquete piloto. Para obtener más información sobre los grupos de pilotos, consulta [Paquetes piloto](../publish/package-flights.md).   |
| rankHigherThan           | string  | El nombre descriptivo del paquete piloto que está clasificado inmediatamente por debajo del paquete piloto actual. Para obtener más información sobre la clasificación de grupos de pilotos, consulta [Paquetes piloto](../publish/package-flights.md).  |


<span id="submission_object" />

### <a name="submission-resource"></a>Recurso de envío

Este recurso proporciona información acerca de un envío. En el siguiente ejemplo se muestra el formato de este recurso.

```json
{
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9WZDNCRD9MMD/submissions/1152921504621243487"
  }
}
```

Este recurso tiene los siguientes valores.

| Value              | Tipo   | Descripción               |
|--------------------|--------|---------------------------|
| id                 | string | Identificador del envío. |
| resourceLocation   | string | Ruta de acceso relativa que se puede anexar al URI de la solicitud de base ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar los datos completos para el envío. |

 
## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Administración de envíos de aplicaciones mediante la API de envío de Microsoft Store](manage-app-submissions.md)
* [Obtener todas las aplicaciones](get-all-apps.md)
* [Obtener una aplicación](get-an-app.md)
* [Obtener complementos para una aplicación](get-add-ons-for-an-app.md)
* [Obtener paquetes piloto para una aplicación](get-flights-for-an-app.md)