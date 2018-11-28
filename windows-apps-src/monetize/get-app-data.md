---
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: Usa estos métodos en la API de envío de Microsoft Store para recuperar los datos de las aplicaciones que están registradas en tu cuenta del centro de partners.
title: Obtener datos de la aplicación
ms.date: 02/28/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, app data, datos de la aplicación
ms.localizationpriority: medium
ms.openlocfilehash: 312729c25d5d9f34471c7154a84273bcbf844da4
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "7829960"
---
# <a name="get-app-data"></a>Obtener datos de la aplicación

Usa los siguientes métodos en la API de envío de Microsoft Store para obtener los datos de aplicaciones existentes en tu cuenta del centro de partners. Para obtener una introducción a la API de envío de Microsoft Store, incluidos los requisitos previos para usar la API, consulta [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

Antes de poder usar estos métodos, la aplicación ya debe existir en tu cuenta del centro de partners. Para crear o administrar envíos de aplicaciones, consulta los métodos de [Administrar envíos de aplicaciones](manage-app-submissions.md).

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications</td>
<td align="left"><a href="get-all-apps.md">Obtener datos para todas las aplicaciones</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}</td>
<td align="left"><a href="get-an-app.md">Obtener datos para una aplicación específica</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts</td>
<td align="left"><a href="get-add-ons-for-an-app.md">Obtener complementos para una aplicación</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights</td>
<td align="left"><a href="get-flights-for-an-app.md">Obtener paquetes piloto para una aplicación</a></td>
</tr>
</tbody>
</table>

<span/>

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
| id            | cadena  | Id. de la Store de la aplicación. Para obtener más información sobre el Id. de la Store, consulta [Ver detalles de identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).   |
| primaryName   | cadena  | Nombre principal de la aplicación.      |
| packageFamilyName | cadena  | El nombre de familia de paquete de la aplicación.      |
| packageIdentityName          | cadena  | El nombre de identidad de paquete de la aplicación.                       |
| publisherName       | cadena  | El identificador del editor de Windows asociado con la aplicación. Esto corresponde al valor de **Paquete/identidad/Editor** que aparece en la página de [identidad de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details) para la aplicación en el centro de partners.       |
| firstPublishedDate      | cadena  | La fecha en que se publicó la aplicación por primera vez, en formato ISO 8601.   |
| lastPublishedApplicationSubmission       | objeto | Un [recurso de envío](#submission_object) que proporciona información sobre el último envío publicado para la aplicación.    |
| pendingApplicationSubmission        | objeto  |  Un [recurso de envío](#submission_object) que proporciona información sobre el envío pendiente actual para la aplicación.   |   
| hasAdvancedListingPermission        | booleano  |  Indica si puedes configurar las [gamingOptions](manage-app-submissions.md#gaming-options-object) o los [tráileres](manage-app-submissions.md#trailer-object) para envíos para la aplicación. Este valor es true para envíos creados después de mayo de 2017. |  |


<span id="add-on-object" />

### <a name="add-on-resouce"></a>Recursos de complemento

Este recurso proporciona información sobre un complemento.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción         |
|-----------------|---------|----------------------|
| inAppProductId            | cadena  | Id. de la Store del complemento. Este valor lo proporciona la Store. Un ejemplo de Id. de la Store sería 9NBLGGH4TNMP.   |


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
| flightId            | cadena  | El identificador para el paquete piloto. Este valor lo proporciona el centro de partners.  |
| friendlyName           | cadena  | El nombre del paquete piloto, según lo especifica el desarrollador.   |
| lastPublishedFlightSubmission       | objeto | Un [recurso de envío](#submission_object) que proporciona información sobre el último envío publicado para el paquete piloto.   |
| pendingFlightSubmission        | objeto  |  Un [recurso de envío](#submission_object) que proporciona información sobre el envío pendiente actual para el paquete piloto.  |    
| groupIds           | matriz  | Una matriz de cadenas que contengan los identificadores de los grupos piloto asociados con el paquete piloto. Para obtener más información acerca de los grupos piloto, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | cadena  | El nombre descriptivo del paquete piloto que está clasificado inmediatamente por debajo del paquete piloto actual. Para obtener más información sobre la clasificación de grupos de pilotos, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |


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

| Valor           | Tipo    | Descripción                 |
|-----------------|---------|------------------------------|
| id            | string  | Identificador del envío.    |
| resourceLocation   | cadena  | Ruta de acceso relativa que se puede anexar al URI de la solicitud de base ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar los datos completos para el envío.            |
 
<span/>

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de aplicaciones con la API de envío de Microsoft Store](manage-app-submissions.md)
* [Obtener todas las aplicaciones](get-all-apps.md)
* [Obtener una aplicación](get-an-app.md)
* [Obtener complementos para una aplicación](get-add-ons-for-an-app.md)
* [Obtener paquetes piloto para una aplicación](get-flights-for-an-app.md)
