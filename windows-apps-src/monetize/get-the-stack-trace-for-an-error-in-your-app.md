---
ms.assetid: b556a245-6359-4ddc-a4bd-76f9873ab694
description: Usa este método en la API de análisis de Microsoft Store para obtener el seguimiento de la pila de un error en la aplicación.
title: Obtener el seguimiento de la pila de un error en la aplicación
ms.date: 06/05/2018
ms.topic: article
keywords: Windows 10, uwp, servicios de Microsoft Store, Store services, API de análisis de Microsoft Store, Microsoft Store analytics API, seguimiento de la pila, stack trace, error
ms.localizationpriority: medium
ms.openlocfilehash: ceffc7622f756eb17c8475208852e013df814554
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627460"
---
# <a name="get-the-stack-trace-for-an-error-in-your-app"></a>Obtener el seguimiento de la pila de un error en la aplicación

Usa este método en la API de análisis de Microsoft Store para obtener el seguimiento de la pila de un error en la aplicación. Este método solo puede descargar el seguimiento de la pila de un error en la aplicación que se haya producido en los últimos 30 días. Los seguimientos de pila también están disponibles en el **errores** sección de la [informe de mantenimiento](../publish/health-report.md) en el centro de partners.

Para poder usar este método, debes emplear antes el método para [obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md) para recuperar el identificador del archivo CAB que está asociado con el error para el que quieres recuperar el seguimiento de la pila.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén el identificador del archivo CAB asociado con el error para el que quieres recuperar el seguimiento de la pila. Para obtener este identificador, usa el método [obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md) para recuperar los detalles de un error específico de tu aplicación y usa el valor **cabId** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  |
|---------------|--------|---------------|------|
| applicationId | string | El id. de la Tienda de la aplicación para la que quieres recuperar el seguimiento de la pila. El identificador de Store está disponible en el [página identidad de aplicación](../publish/view-app-identity-details.md) en el centro de partners. Un ejemplo de un id. de la Tienda sería 9WZDNCRFJ3Q8. |  Sí  |
| cabId | string | El identificador exclusivo del archivo CAB asociado con el error para el que quieres recuperar el seguimiento de la pila. Para obtener este identificador, usa el método [obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md) para recuperar los detalles de un error específico de tu aplicación y usa el valor **cabId** en el cuerpo de la respuesta de ese método. |  Sí  |

 
### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo obtener un seguimiento de la pila con este método. Sustituye el valor *applicationId* por el id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo    | Descripción                  |
|------------|---------|--------------------------------|
| Valor      | array   | Una matriz de objetos, todos los cuales contienen un marco de los datos de seguimiento de la pila. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores del rastreo de la pila](#stack-trace-values) que encontrarás a continuación. |
| @nextLink  | string  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de errores de la solicitud. |
| TotalCount | número entero | El número total de filas del resultado de datos de la consulta.          |


### <a name="stack-trace-values"></a>Valores del seguimiento de la pila

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción      |
|-----------------|---------|----------------|
| level            | string  |  El número de marco que este elemento representa en la pila de llamadas.  |
| image   | string  |   El nombre del archivo ejecutable o la biblioteca de imágenes que contiene la función que se llama en este marco de la pila.           |
| function | string  |  El nombre de la función que se llama en este marco de la pila. Está disponible únicamente si la aplicación incluye símbolos para el archivo ejecutable o la biblioteca.              |
| offset     | string  |  El desplazamiento de bytes de la instrucción actual en relación con el inicio de la función.      |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "level": "0",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.DoWork",
      "offset": "0x25C"
    }
    {
      "level": "1",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.Initialize",
      "offset": "0x26"
    }
    {
      "level": "2",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.Start",
      "offset": "0x66"
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}

```

## <a name="related-topics"></a>Temas relacionados

* [Informe de mantenimiento](../publish/health-report.md)
* [Acceder a los datos de análisis con servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de informes de errores](get-error-reporting-data.md)
* [Obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md)
* [Descargue el archivo CAB para un error en la aplicación](download-the-cab-file-for-an-error-in-your-app.md)
