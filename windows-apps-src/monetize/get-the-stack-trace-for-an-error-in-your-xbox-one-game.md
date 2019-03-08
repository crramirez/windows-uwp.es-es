---
description: Use este método en la API de análisis de Microsoft Store para obtener el seguimiento de pila para un error en Xbox One juegos.
title: Obtener el seguimiento de la pila de un error en tu juego de Xbox One
ms.date: 11/06/2018
ms.topic: article
keywords: Windows 10, uwp, servicios de Microsoft Store, Store services, API de análisis de Microsoft Store, Microsoft Store analytics API, seguimiento de la pila, stack trace, error
ms.localizationpriority: medium
ms.openlocfilehash: fd43305c54245c3281a0e840d3df4c5c87ff7ad8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658690"
---
# <a name="get-the-stack-trace-for-an-error-in-your-xbox-one-game"></a>Obtener el seguimiento de la pila de un error en tu juego de Xbox One

Use este método en la Microsoft Store API analytics para obtener el seguimiento de pila para un error en Xbox One juegos que estaba introducidos mediante el Portal para desarrolladores de Xbox (XDP) y está disponible en el panel del centro de partners XDP Analytics. Este método solo puede descargar el seguimiento de la pila de un error producido en los últimos 30 días.

Para poder usar este método, primero debe usar el [obtener los detalles de un error en Xbox One juego](get-details-for-an-error-in-your-xbox-one-game.md) método para recuperar el identificador del archivo .cab que está asociado con el error para el que van a recuperar el seguimiento de pila.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén el identificador del archivo CAB asociado con el error para el que quieres recuperar el seguimiento de la pila. Para obtener este identificador, utilice el [obtener los detalles de un error en Xbox One juego](get-details-for-an-error-in-your-xbox-one-game.md) método para recuperar los detalles de un error específico en la aplicación y usar el **cabId** valor en el cuerpo de respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  |
|---------------|--------|---------------|------|
| applicationId | string | El identificador de producto del juego de Xbox One que va a recuperar un seguimiento de pila. Para obtener el id. del producto de tu juego, ve a tu juego en el Portal de desarrollador de Xbox (XDP) y recupera el id. del producto desde la dirección URL. Como alternativa, si descarga los datos de estado desde el informe de análisis de centro de partners de Windows, el identificador de producto se incluye en el archivo TSV. |  Sí  |
| cabId | string | El identificador exclusivo del archivo CAB asociado con el error para el que quieres recuperar el seguimiento de la pila. Para obtener este identificador, utilice el [obtener los detalles de un error en Xbox One juego](get-details-for-an-error-in-your-xbox-one-game.md) método para recuperar los detalles de un error específico en la aplicación y usar el **cabId** valor en el cuerpo de respuesta de ese método. |  Sí  |

 
### <a name="request-example"></a>Ejemplo de solicitud

El ejemplo siguiente muestra cómo obtener un seguimiento de pila para una Xbox One juegos con este método. Reemplace el *applicationId* valor con el identificador de producto para su juego.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
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
| function | string  |  El nombre de la función que se llama en este marco de la pila. Esto solo está disponible si su juego incluye los símbolos para el archivo ejecutable o biblioteca.              |
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

* [Acceder a los datos de análisis con servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener juegos para Xbox One datos de informe de errores](get-error-reporting-data-for-your-xbox-one-game.md)
* [Obtener los detalles de un error en Xbox One juegos](get-details-for-an-error-in-your-xbox-one-game.md)
* [Descargue el archivo CAB para un error en el juego de Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
