---
author: mcleanbyron
ms.assetid: 87708690-079A-443D-807E-D2BF9F614DDF
description: "Usa este método en la API de envío de la Tienda Windows para obtener datos para un paquete piloto de una aplicación que está registrada en tu cuenta del Centro de desarrollo de Windows."
title: Obtener un paquete piloto
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, Windows 10, uwp, UWP, Windows Store submission API, API de envío de la Tienda de Windows, flight, piloto, package flight, paquete piloto"
ms.openlocfilehash: 921d33c033704de711112c30feae7dbbfebc732b
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="get-a-package-flight"></a>Obtener un paquete piloto




Usa este método en la API de envío de la Tienda Windows para obtener datos para un paquete piloto de una aplicación que está registrada en tu cuenta del Centro de desarrollo de Windows.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` |

<span/>
 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-parameters"></a>Parámetros de solicitud


| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadena | Obligatorio. El Id. de la Tienda de la aplicación que contiene el paquete piloto que quieres obtener. El Id. de la Tienda para la aplicación está disponible en el panel del Centro de desarrollo.  |
| flightId | cadena | Obligatorio. El identificador del paquete piloto que se va a obtener. Este identificador está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta a las solicitudes para [crear un paquete piloto](create-a-flight.md) y [obtener paquetes piloto para una aplicación](get-flights-for-an-app.md).  |

<span/>

### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

<span/>

### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra cómo recuperar información sobre un paquete piloto con el identificador 43e448df-97c9-4a43-a0bc-2a445e736bcd para una aplicación con el Id. de la Tienda 9WZDNCRD91MD.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

El siguiente ejemplo muestra el cuerpo de respuesta JSON para una llamada satisfactoria a este método. Para obtener más información acerca de los valores del cuerpo de respuesta, consulta las secciones siguientes.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "lastPublishedFlightSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621086517"
  },
  "pendingFlightSubmission": {
    "id": "115292150462124364",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243647"
  },
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | cadena  | El identificador para el paquete piloto. Este valor está proporcionado por el Centro de desarrollo.  |
| friendlyName           | cadena  | El nombre del paquete piloto, según lo especifica el desarrollador.   |  
| lastPublishedFlightSubmission       | objeto | Un objeto que proporciona información sobre el último envío publicado para el paquete piloto. Para obtener más información, consulta la sección [Objeto de envío](#submission_object) a continuación.  |
| pendingFlightSubmission        | objeto  |  Un objeto que proporciona información sobre el envío pendiente actualmente para el paquete piloto. Para obtener más información, consulta la sección [Objeto de envío](#submission_object) a continuación.  |   
| groupIds           | matriz  | Una matriz de cadenas que contengan los identificadores de los grupos piloto asociados con el paquete piloto. Para obtener más información acerca de los grupos piloto, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | cadena  | El nombre descriptivo del paquete piloto que está clasificado inmediatamente por debajo del paquete piloto actual. Para obtener más información acerca de la clasificación de grupos piloto, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |

<span id="submission_object" />
### <a name="submission-object"></a>Objeto de envío

Los valores *lastPublishedFlightSubmission* y *pendingFlightSubmission* del cuerpo de respuesta contienen objetos que proporcionan información de recursos sobre un envío para el paquete piloto. Estos objetos tienen los siguientes valores.

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | cadena  | Identificador del envío.    |
| resourceLocation   | cadena  | Ruta de acceso relativa que se puede anexar al URI de la solicitud de base ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar los datos completos para el envío.                                                                                                                                               |
 
<span/>

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción     |
|--------|---------------------  |
| 400  | La solicitud no es válida. |
| 404  | No se pudo encontrar el paquete piloto especificado.   |   
| 409  | La aplicación usa una característica del panel del Centro de desarrollo que [la API de envío de la Tienda Windows no admite actualmente](create-and-manage-submissions-using-windows-store-services.md#not_supported). |                                                                                                 

<span/>

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Crear un paquete piloto](create-a-flight.md)
* [Eliminar un paquete piloto](delete-a-flight.md)
