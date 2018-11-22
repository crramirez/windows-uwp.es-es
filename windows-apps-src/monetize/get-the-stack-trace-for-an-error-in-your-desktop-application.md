---
author: Xansky
description: Usa este método en la API de análisis de Microsoft Store para obtener el seguimiento de la pila de un error en la aplicación de escritorio.
title: Obtener el seguimiento de la pila de un error en la aplicación de escritorio
ms.author: mhopkins
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Microsoft Store, Microsoft Store analytics API, API de análisis Microsoft Store, error, error, stack trace, seguimiento de la pila
ms.localizationpriority: medium
ms.openlocfilehash: 024c903ea43d9fabc90b2f6b7891f6de4e92b1d5
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7564371"
---
# <a name="get-the-stack-trace-for-an-error-in-your-desktop-application"></a>Obtener el seguimiento de la pila de un error en la aplicación de escritorio

Usa este método en la API de análisis de Microsoft Store para obtener el seguimiento de la pila de un error en una aplicación de escritorio que agregaste al [Programa de aplicaciones de escritorio de Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504). Este método solo puede descargar el seguimiento de la pila de un error producido en los últimos 30 días. Seguimientos de la pila también están disponibles en el [informe de estado](https://msdn.microsoft.com/library/windows/desktop/mt826504) para aplicaciones de escritorio en el centro de partners.

Para poder usar este método, debes emplear antes el método para [obtener los detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md) para recuperar el hash del identificador del archivo CAB que está asociado con el error para el que quieres recuperar el seguimiento de la pila.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.
* Obtén el hash del identificador del archivo CAB asociado con el error para el que quieres recuperar el seguimiento de la pila. Para obtener este valor, usa el método [obtener los detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md) para recuperar los detalles de un error específico de tu aplicación y usa el valor **cabIdHash** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |
 

### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Necesario  |
|---------------|--------|---------------|------|
| applicationId | cadena | El identificador de producto de la aplicación de escritorio de la cual quieres obtener un seguimiento de la pila. Para obtener el identificador de producto de una aplicación de escritorio, abra cualquier [informe de análisis de la aplicación de escritorio en el centro de partners](https://msdn.microsoft.com/library/windows/desktop/mt826504) (por ejemplo, el **informe de estado**) y recuperar el identificador de producto de la dirección URL. |  Sí  |
| cabIdHash | cadena | El hash del identificador exclusivo del archivo CAB asociado con el error para el que quieres recuperar el seguimiento de la pila. Para obtener este valor, usa el método [obtener los detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md) para recuperar los detalles de un error específico de tu aplicación de escritorio y usa el valor **cabIdHash** en el cuerpo de la respuesta de ese método. |  Sí  |

 
### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo obtener un seguimiento de la pila con este método. Reemplaza los parámetros *applicationId* y *cabIdHash* con los valores correspondientes a la aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo    | Descripción                  |
|------------|---------|--------------------------------|
| Valor      | matriz   | Una matriz de objetos, todos los cuales contienen un marco de los datos de seguimiento de la pila. Para más información sobre los datos de cada objeto, consulta la sección sobre los [valores del rastreo de la pila](#stack-trace-values) que encontrarás a continuación. |
| @nextLink  | cadena  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero resulta que hay más de 10000 filas de errores de la solicitud. |
| TotalCount | entero | El número total de filas en el resultado de datos de la consulta.          |


### <a name="stack-trace-values"></a>Valores del seguimiento de la pila

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción      |
|-----------------|---------|----------------|
| level            | cadena  |  El número de marco que este elemento representa en la pila de llamadas.  |
| image   | cadena  |   El nombre del archivo ejecutable o la biblioteca de imágenes que contiene la función que se llama en este marco de la pila.           |
| function | cadena  |  El nombre de la función que se llama en este marco de la pila. Está disponible únicamente si la aplicación incluye símbolos para el archivo ejecutable o la biblioteca.              |
| offset     | cadena  |  El desplazamiento de bytes de la instrucción actual en relación con el inicio de la función.      |


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

* [Informe Estado](../publish/health-report.md)
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de informes de errores para la aplicación de escritorio](get-desktop-application-error-reporting-data.md)
* [Obtener los detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md)
* [Descargar el archivo .cab para un error en tu aplicación de escritorio](download-the-cab-file-for-an-error-in-your-desktop-application.md)
