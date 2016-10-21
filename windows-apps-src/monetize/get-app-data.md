---
author: mcleanbyron
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: "Usa estos métodos en la API de envío de la Tienda Windows para recuperar los datos de las aplicaciones registradas en tu cuenta del Centro de desarrollo de Windows."
title: "Obtener datos de aplicación mediante la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 99d609decc8c38952961deac5bb8ec6926d91c88

---

# Obtener datos de aplicación mediante la API de envío de la Tienda Windows

Usa los métodos siguientes en la API de envío de la Tienda Windows para obtener datos para aplicaciones registradas en tu cuenta del Centro de desarrollo de Windows. Para obtener una introducción a la API de envío de la Tienda Windows, consulta [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md).

>**Nota**&nbsp;&nbsp;Estos métodos solo pueden usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado. Estos métodos solo se pueden usar para obtener datos de aplicaciones. Para crear o administrar envíos de aplicaciones, consulta los métodos de [Administrar envíos de aplicación](manage-app-submissions.md).

| Método        | URI    | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications``` | Recupera los datos de todas las aplicaciones registradas en tu cuenta del Centro de desarrollo de Windows. Para obtener más información, consulta [Obtener todas las aplicaciones](get-all-apps.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}``` | Recupera datos de una aplicación específica registrada en tu cuenta del Centro de desarrollo de Windows. Para obtener más información, consulta [Obtener una aplicación](get-an-app.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts``` | Enumera los complementos (también conocidos como productos desde la aplicación o IAP) de una aplicación registrada en tu cuenta del Centro de desarrollo de Windows. Para obtener más información, consulta [Obtener complementos para una aplicación](get-add-ons-for-an-app.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights``` | Enumera los paquetes piloto de una aplicación registrada en tu cuenta del Centro de desarrollo de Windows. Para obtener más información, consulta [Obtener paquetes piloto para una aplicación](get-flights-for-an-app.md). |

<span/>

## Requisitos previos

Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de la Tienda Windows antes de intentar usar cualquiera de estos métodos.

## Recursos

Estos métodos usan los siguientes recursos para dar formato a los datos.

<span id="application_object" />
### Aplicación

Este recurso representa una aplicación registrada en tu cuenta. En el siguiente ejemplo se muestra el formato de este recurso.

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
  }
}
```

Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | cadena  | Id. de la Tienda de la aplicación. Para obtener más información sobre el Id. de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).   |
| primaryName   | cadena  | Nombre principal de la aplicación.                                                                                                                                                   |
| packageFamilyName | cadena  | El nombre de familia de paquete de la aplicación.                                                                                                                                                                                                         |
| packageIdentityName          | cadena  | El nombre de identidad de paquete de la aplicación.                                                                                                                                                              |
| publisherName       | cadena  | El identificador del editor de Windows asociado con la aplicación. Esto corresponde al valor **paquete/identidad/editor** que aparece en la página de [identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details) de la aplicación en el panel del Centro de desarrollo de Windows.                                                                                             |
| firstPublishedDate      | cadena  | La fecha en que se publicó la aplicación por primera vez, en formato ISO 8601.                                                                                         |
| lastPublishedApplicationSubmission       | objeto | Objeto que proporciona información sobre el último envío publicado para la aplicación. Si quieres obtener más información, consulta la sección [Envío](#submission_object) a continuación.                                                                                                                                                          |
| pendingApplicationSubmission        | objeto  |  Objeto que proporciona información sobre el envío pendiente actualmente para la aplicación. Si quieres obtener más información, consulta la sección [Envío](#submission_object) a continuación.  |   |


<span id="add-on-object" />
### Complemento

Este recurso proporciona información sobre un complemento. En el siguiente ejemplo se muestra el formato de este recurso.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inAppProductId            | cadena  | Id. de la Tienda del complemento. Este valor lo proporciona la Tienda. Un ejemplo de un Id. de la Tienda sería 9NBLGGH4TNMP.   |


<span id="flight-object" />
### Piloto

Este recurso proporciona información sobre un paquete piloto para una aplicación. En el siguiente ejemplo se muestra el formato de este recurso.

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

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | cadena  | El identificador del paquete piloto. Proporciona este valor el Centro de desarrollo.  |
| friendlyName           | cadena  | El nombre del paquete piloto, según lo especifica el desarrollador.   |
| lastPublishedFlightSubmission       | objeto | Un objeto que proporciona información sobre el último envío publicado para el paquete piloto. Si quieres obtener más información, consulta la sección [Envío](#submission_object) a continuación.  |
| pendingFlightSubmission        | objeto  |  Un objeto que proporciona información sobre el envío pendiente actualmente para el paquete piloto. Si quieres obtener más información, consulta la sección [Envío](#submission_object) a continuación.  |    
| groupIds           | matriz  | Una matriz de cadenas que contienen los identificadores de los grupos piloto asociados con el paquete piloto. Para obtener más información acerca de los grupos piloto, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | cadena  | El nombre descriptivo del paquete piloto que está clasificado inmediatamente por debajo del paquete piloto actual. Para obtener más información acerca de la clasificación de grupos piloto, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |


<span id="submission_object" />
### Envío

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

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | cadena  | Identificador del envío.    |
| resourceLocation   | cadena  | Ruta de acceso relativa que se puede anexar al URI de la solicitud de base ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar los datos completos para el envío.                                                                                                                                               |
 
<span/>

## Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de aplicaciones con la API de envío de la Tienda Windows](manage-app-submissions.md)
* [Obtener todas las aplicaciones](get-all-apps.md)
* [Obtener una aplicación](get-an-app.md)
* [Obtener complementos para una aplicación](get-add-ons-for-an-app.md)
* [Obtener paquetes piloto para una aplicación](get-flights-for-an-app.md)



<!--HONumber=Aug16_HO5-->


