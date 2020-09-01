---
description: Use este método en la API de Microsoft Store Analytics para obtener el seguimiento de la pila para un error en la aplicación de escritorio.
title: Obtener el seguimiento de pila asociado a un error en la aplicación de escritorio
ms.date: 06/05/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, seguimiento de la pila, error, aplicación de escritorio
ms.localizationpriority: medium
ms.openlocfilehash: 6a960a455260c208b5a38139e24d00076c4fc45d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155589"
---
# <a name="get-the-stack-trace-for-an-error-in-your-desktop-application"></a>Obtener el seguimiento de pila asociado a un error en la aplicación de escritorio

Use este método en la API de Microsoft Store Analytics para obtener el seguimiento de la pila de un error en una aplicación de escritorio que haya agregado al [programa de aplicación de escritorio de Windows](/windows/desktop/appxpkg/windows-desktop-application-program). Este método solo puede descargar el seguimiento de la pila para un error que se ha producido en los últimos 30 días. Los seguimientos de la pila también están disponibles en el [Informe de mantenimiento](/windows/desktop/appxpkg/windows-desktop-application-program) de las aplicaciones de escritorio del centro de Partners.

Para poder usar este método, primero debe usar el método [obtener detalles de un error en su aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md) para recuperar el hash de identificador del archivo. cab asociado al error para el que desea recuperar el seguimiento de la pila.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtenga el hash de identificador del archivo. CAB asociado al error para el que desea recuperar el seguimiento de la pila. Para obtener este valor, use el método [obtener detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md) para recuperar los detalles de un error específico en la aplicación y use el valor **cabIdHash** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |
 

### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  |
|---------------|--------|---------------|------|
| applicationId | string | El ID. del producto de la aplicación de escritorio para la que desea obtener un seguimiento de la pila. Para obtener el identificador de producto de una aplicación de escritorio, abra el [Informe de análisis de la aplicación de escritorio en el centro de Partners](/windows/desktop/appxpkg/windows-desktop-application-program) (como el **Informe de mantenimiento**) y recupere el identificador de producto de la dirección URL. |  Sí  |
| cabIdHash | string | El hash de identificador único del archivo. CAB que está asociado al error para el que desea recuperar el seguimiento de la pila. Para obtener este valor, use el método [obtener detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md) para recuperar los detalles de un error específico de la aplicación y use el valor **cabIdHash** en el cuerpo de la respuesta de ese método. |  Sí  |

 
### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo obtener un seguimiento de la pila con este método. Reemplace los parámetros *ApplicationID* y *cabIdHash* por los valores adecuados para la aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo    | Descripción                  |
|------------|---------|--------------------------------|
| Value      | array   | Una matriz de objetos, todos los cuales contienen un marco de los datos de seguimiento de la pila. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores del rastreo de la pila](#stack-trace-values) que encontrarás a continuación. |
| @nextLink  | string  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de errores de la solicitud. |
| TotalCount | integer | El número total de filas del resultado de datos de la consulta.          |


### <a name="stack-trace-values"></a>Valores del seguimiento de la pila

Los elementos de la matriz *Value* contienen los siguientes valores.

| Value           | Tipo    | Descripción      |
|-----------------|---------|----------------|
| Nivel            | string  |  El número de marco que este elemento representa en la pila de llamadas.  |
| imagen   | string  |   El nombre del archivo ejecutable o la biblioteca de imágenes que contiene la función que se llama en este marco de la pila.           |
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

* [Informe de estado](../publish/health-report.md)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de informes de errores de la aplicación de escritorio](get-desktop-application-error-reporting-data.md)
* [Obtener los detalles asociados a un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md)
* [Descargar el archivo CAB asociado a un error en la aplicación de escritorio](download-the-cab-file-for-an-error-in-your-desktop-application.md)