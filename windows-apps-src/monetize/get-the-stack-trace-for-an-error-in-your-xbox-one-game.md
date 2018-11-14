---
author: Xansky
description: Usa este método en la API de análisis de Microsoft Store para obtener el seguimiento de la pila de un error en tu Xbox One juego.
title: Obtener el seguimiento de la pila de un error en tu Xbox One juego
ms.author: mhopkins
ms.date: 11/06/2018
ms.topic: article
keywords: Windows 10, uwp, servicios de Microsoft Store, Store services, API de análisis de Microsoft Store, Microsoft Store analytics API, seguimiento de la pila, stack trace, error
ms.localizationpriority: medium
ms.openlocfilehash: df3af90bda9d972a891dce67730f8f320b7607c1
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6464894"
---
# <a name="get-the-stack-trace-for-an-error-in-your-xbox-one-game"></a>Obtener el seguimiento de la pila de un error en tu Xbox One juego

Usa este método en la Microsoft Store analytics API para obtener el seguimiento de la pila de un error en tu Xbox One juego que se ha integrado mediante el Portal de desarrollador de Xbox (XDP) y está disponible en el panel del centro de desarrollo de análisis de XDP. Este método solo puede descargar el seguimiento de la pila de un error producido en los últimos 30 días.

Antes de que puedes usar este método, primero debes usar el método [obtener detalles de un error en tu juego de Xbox One](get-details-for-an-error-in-your-xbox-one-game.md) para recuperar el identificador del archivo .cab que está asociado con el error para el que quieres recuperar el seguimiento de la pila.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén el identificador del archivo CAB asociado con el error para el que quieres recuperar el seguimiento de la pila. Para obtener este Id., usa el método [obtener detalles de un error en tu Xbox One en juegos](get-details-for-an-error-in-your-xbox-one-game.md) para recuperar los detalles de un error específico de la aplicación y usa el valor de **cabId** en el cuerpo de respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  |
|---------------|--------|---------------|------|
| applicationId | string | El identificador de producto del juego de Xbox One para el que quieres recuperar un seguimiento de la pila. Para obtener el id. del producto de tu juego, ve a tu juego en el Portal de desarrollador de Xbox (XDP) y recupera el id. del producto desde la dirección URL. Como alternativa, si descargas los datos de estado del informe de análisis del centro de desarrollo de Windows, el identificador de producto se incluye en el archivo TSV. |  Sí  |
| cabId | cadena | El identificador exclusivo del archivo CAB asociado con el error para el que quieres recuperar el seguimiento de la pila. Para obtener este Id., usa el método [obtener detalles de un error en tu Xbox One en juegos](get-details-for-an-error-in-your-xbox-one-game.md) para recuperar los detalles de un error específico de la aplicación y usa el valor de **cabId** en el cuerpo de respuesta de ese método. |  Sí  |

 
### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo obtener un seguimiento de la pila para una consola Xbox One juego mediante este método. Reemplaza el valor de *applicationId* con el identificador de producto para tu juego.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
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
| function | cadena  |  El nombre de la función que se llama en este marco de la pila. Esto está disponible únicamente si el juego incluye símbolos para el archivo ejecutable o la biblioteca.              |
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

## <a name="related-topics"></a>Artículos relacionados

* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener los datos del informe de la Xbox One de errores de juego](get-error-reporting-data-for-your-xbox-one-game.md)
* [Obtener los detalles de un error en tu Xbox One juego](get-details-for-an-error-in-your-xbox-one-game.md)
* [Descargar el archivo .cab para un error en tu juego de Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
