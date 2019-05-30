---
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: Utilice estos métodos en la API de envío de Microsoft Store para recuperar los datos para las aplicaciones que están registradas en su cuenta del centro de partners.
title: Obtención de datos de la aplicación
ms.date: 02/28/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, app data, datos de la aplicación
ms.localizationpriority: medium
ms.openlocfilehash: cfbe8df46f51b41ccdd840f609caf2c593735e1f
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372147"
---
# <a name="get-app-data"></a>Obtención de datos de la aplicación

Utilice los métodos siguientes en la API de envío de Microsoft Store para obtener datos para las aplicaciones existentes en la cuenta del centro de partners. Para obtener una introducción a la API de envío de Microsoft Store, incluidos los requisitos previos para usar la API, consulta [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

Para poder usar estos métodos, la aplicación ya debe existir en la cuenta del centro de partners. Para crear o administrar envíos de aplicaciones, consulta los métodos de [Administrar envíos de aplicaciones](manage-app-submissions.md).

| Método | URI                                                                                             | Descripción                                                 |
|------- |------------------------------------------------------------------------------------------------ |------------------------------------------------------------ |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications`                                   | [Obtener datos para todas las aplicaciones](get-all-apps.md)               |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}`                   | [Obtener datos para una aplicación específica](get-an-app.md)                |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts` | [Obtenga complementos para una aplicación](get-add-ons-for-an-app.md)         |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights`       | [Obtener los vuelos de paquete para una aplicación](get-flights-for-an-app.md) |

## <a name="prerequisites"></a>Requisitos previos

Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store antes de intentar usar cualquiera de estos métodos.

## <a name="data-resources"></a>Recursos de datos

Los métodos de la API de envío de Microsoft Store para obtener datos de aplicaciones usan los siguientes recursos de datos de JSON.

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

| Valor           | Tipo    | Descripción       |
|-----------------|---------|---------------------|
| id            | string  | Id. de la Tienda de la aplicación. Para obtener más información sobre el identificador de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).   |
| primaryName   | string  | Nombre principal de la aplicación.      |
| packageFamilyName | string  | El nombre de familia de paquete de la aplicación.      |
| packageIdentityName          | string  | El nombre de identidad de paquete de la aplicación.                       |
| publisherName       | string  | El identificador del editor de Windows asociado con la aplicación. Esto corresponde a la **paquete/identidad/publicador** valor que aparece en el [identidad de aplicación](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details) página de la aplicación en el centro de partners.       |
| firstPublishedDate      | string  | La fecha en que se publicó la aplicación por primera vez, en formato ISO 8601.   |
| lastPublishedApplicationSubmission       | object | Un [recurso de envío](#submission_object) que proporciona información sobre el último envío publicado para la aplicación.    |
| pendingApplicationSubmission        | object  |  Un [recurso de envío](#submission_object) que proporciona información sobre el envío pendiente actual para la aplicación.   |   
| hasAdvancedListingPermission        | boolean  |  Indica si puedes configurar las [gamingOptions](manage-app-submissions.md#gaming-options-object) o los [tráileres](manage-app-submissions.md#trailer-object) para envíos para la aplicación. Este valor es true para envíos creados después de mayo de 2017. |  |


<span id="add-on-object" />

### <a name="add-on-resource"></a>Recurso de complemento

Este recurso proporciona información sobre un complemento.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción         |
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

| Valor           | Tipo    | Descripción           |
|-----------------|---------|------------------------|
| flightId            | string  | El identificador del paquete piloto. Este valor es proporcionado por el centro de partners.  |
| friendlyName           | string  | El nombre del paquete piloto, según lo especifica el desarrollador.   |
| lastPublishedFlightSubmission       | object | Un [recurso de envío](#submission_object) que proporciona información sobre el último envío publicado para el paquete piloto.   |
| pendingFlightSubmission        | object  |  Un [recurso de envío](#submission_object) que proporciona información sobre el envío pendiente actual para el paquete piloto.  |    
| groupIds           | array  | Una matriz de cadenas que contienen los identificadores de los grupos de pilotos asociados con el paquete piloto. Para obtener más información sobre los grupos de pilotos, consulta [Paquetes piloto](https://docs.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | string  | El nombre descriptivo del paquete piloto que está clasificado inmediatamente por debajo del paquete piloto actual. Para obtener más información sobre la clasificación de grupos de pilotos, consulta [Paquetes piloto](https://docs.microsoft.com/windows/uwp/publish/package-flights).  |


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

| Valor              | Tipo   | Descripción               |
|--------------------|--------|---------------------------|
| id                 | string | Identificador del envío. |
| resourceLocation   | string | Ruta de acceso relativa que se puede anexar al URI de la solicitud de base ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar los datos completos para el envío. |

 
## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos de uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de aplicaciones mediante la API de envío de Microsoft Store](manage-app-submissions.md)
* [Obtener todas las aplicaciones](get-all-apps.md)
* [Obtener una aplicación](get-an-app.md)
* [Obtenga complementos para una aplicación](get-add-ons-for-an-app.md)
* [Obtener los vuelos de paquete para una aplicación](get-flights-for-an-app.md)
