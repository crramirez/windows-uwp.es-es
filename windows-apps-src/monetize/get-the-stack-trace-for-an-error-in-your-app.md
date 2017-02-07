---
author: mcleanbyron
ms.assetid: 
description: "Usa este método en la API de análisis de la Tienda Windows para obtener el seguimiento de la pila de un error en la aplicación."
title: "Obtener el seguimiento de la pila de un error en la aplicación"
translationtype: Human Translation
ms.sourcegitcommit: 767097f068630e5ec171415c05d6dc395c8b26b3
ms.openlocfilehash: 90481b5f85d010a142e86ca67ac94c3ec25d89c6

---

# <a name="get-the-stack-trace-for-an-error-in-your-app"></a>Obtener el seguimiento de la pila de un error en la aplicación

Usa este método en la API de análisis de la Tienda Windows para obtener el seguimiento de la pila de un error en la aplicación. Este método solo puede descargar el seguimiento de la pila de un error en la aplicación que se haya producido en los últimos 30 días. Los seguimientos de la pila también están disponibles en la sección **Errores** del [informe de estado](../publish/health-report.md) en el panel del Centro de desarrollo de Windows.

Para poder usar este método, debes emplear antes el método para [obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md) para recuperar el identificador del archivo CAB que está asociado con el error para el que quieres recuperar el seguimiento de la pila.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén el identificador del archivo CAB asociado con el error para el que quieres recuperar el seguimiento de la pila. Para obtener este identificador, usa el método [obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md) para recuperar los detalles de un error específico de tu aplicación y usa el valor **cabId** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace``` |

<span/> 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | cadena | Obligatorio. Token de acceso de Azure AD con el formato **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  |
|---------------|--------|---------------|------|
| applicationId | cadena | El id. de la Tienda de la aplicación para la que quieres recuperar el seguimiento de la pila. El id. de la Tienda está disponible en la [página Identidad de la aplicación](../publish/view-app-identity-details.md) del panel del Centro de desarrollo. Un ejemplo de un id. de la Tienda sería 9WZDNCRFJ3Q8. |  Sí  |
| cabId | cadena | El identificador exclusivo del archivo CAB asociado con el error para el que quieres recuperar el seguimiento de la pila. Para obtener este identificador, usa el método [obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md) para recuperar los detalles de un error específico de tu aplicación y usa el valor **cabId** en el cuerpo de la respuesta de ese método. |  Sí  |

<span/>
 
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
| Valor      | matriz   | Una matriz de objetos, todos los cuales contienen un marco de los datos de seguimiento de la pila. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores del rastreo de la pila](#stack-trace-values) que encontrarás a continuación. |
| @nextLink  | cadena  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de errores de la solicitud. |
| TotalCount | número | El número total de filas del resultado de datos de la consulta.          |

<span/>

### <a name="stack-trace-values"></a>Valores del seguimiento de la pila

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción      |
|-----------------|---------|----------------|
| level            | cadena  |  El número de marco que este elemento representa en la pila de llamadas.  |
| image   | cadena  |   El nombre del archivo ejecutable o la biblioteca de imágenes que contiene la función que se llama en este marco de la pila.           |
| function | cadena  |  El nombre de la función que se llama en este marco de la pila. Está disponible únicamente si la aplicación incluye símbolos para el archivo ejecutable o la biblioteca.              |
| offset     | cadena  |  El desplazamiento de bytes de la instrucción actual en relación con el inicio de la función.      |

<span/> 

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
* [Acceder a los datos de análisis mediante los servicios de la Tienda Windows](access-analytics-data-using-windows-store-services.md)
* [Obtener los datos del informe de errores](get-error-reporting-data.md)
* [Obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md)



<!--HONumber=Dec16_HO1-->

