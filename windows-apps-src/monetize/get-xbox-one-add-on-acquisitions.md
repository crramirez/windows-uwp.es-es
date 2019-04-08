---
description: Utilice este método en la API de análisis de Microsoft Store para obtener datos de adquisición de complemento agregado.
title: Obtener adquisiciones de complementos de Xbox One
ms.date: 10/18/2018
ms.topic: article
keywords: Windows 10, uwp, servicios de Store, API, Xbox One adquisiciones de complemento de análisis de Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: f102d2d692a2307c25dcb95e66d612fc561dec70
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633300"
---
# <a name="get-xbox-one-add-on-acquisitions"></a>Obtener adquisiciones de complementos de Xbox One

Use este método en la Microsoft Store API analytics para obtener datos de adquisición de complemento agregado en formato JSON para una Xbox One juegos que estaba introducidos mediante el Portal para desarrolladores de Xbox (XDP) y está disponible en el panel del centro de partners XDP Analytics.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción          |
|---------------|--------|--------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

El *applicationId* o *addonProductId* el parámetro es obligatorio. Para recuperar los datos de compra de todos los complementos registrados en la aplicación, especifica el parámetro *applicationId*. Para recuperar datos de adquisición de un único complemento, especifique el *addonProductId* parámetro. Si especificas ambos, el parámetro *applicationId* se ignorará.

| Parámetro        | Tipo   |  Descripción      |  Requerido  |
|---------------|--------|---------------|------|
| applicationId | string | El *productId* del juego de Xbox One que va a recuperar datos de adquisición. Para obtener el *productId* de su juego, vaya a su juego en el programa de análisis XDP y recuperar el *productId* desde la dirección URL. Como alternativa, si descarga los datos de adquisiciones desde el informe de análisis de centro de partners, el *productId* se incluye en el archivo TSV. |  Sí  |
| addonProductId | string | El *productId* del complemento para el que desea recuperar datos de adquisición.  | Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos de compra del complemento que se recuperarán. El valor predeterminado es la fecha actual. |  No  |
| endDate | fecha | La fecha de finalización del intervalo de fechas de los datos de compra del complemento que recuperarán. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter |string  | <p>Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo desde el cuerpo de respuesta y el valor que están asociados con los operadores eq o ne, y las instrucciones se pueden combinar con y o. Los valores de cadena deben estar entre comillas simples en el parámetro de filtro. Por ejemplo, filtrar = mercado eq eq 'US' y el sexo estoy '.</p> <p>Puedes especificar los campos siguientes del cuerpo de respuesta:</p> <ul><li><strong>acquisitionType</strong></li><li><strong>Edad</strong></li><li><strong>cliente de la tienda</strong></li><li><strong>sexo</strong></li><li><strong>mercado</strong></li><li><strong>versión del SO</strong></li><li><strong>tipo de dispositivo</strong></li><li><strong>SandboxId</strong></li></ul>| No   |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. | No |
| orderby | string | Instrucción que ordena los valores de datos resultantes de cada compra de complemento. La sintaxis es <em>orderby = campo [order], [order],...</em> El parámetro <em>field</em> puede ser una de las siguientes cadenas:<ul><li><strong>Fecha</strong></li><li><strong>acquisitionType</strong></li><li><strong>Edad</strong></li><li><strong>cliente de la tienda</strong></li><li><strong>sexo</strong></li><li><strong>mercado</strong></li><li><strong>versión del SO</strong></li><li><strong>tipo de dispositivo</strong></li><li><strong>orderName</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li><strong>Fecha</strong></li><li><strong>ApplicationName</strong></li><li><strong>addonProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>Edad</strong></li><li><strong>cliente de la tienda</strong></li><li><strong>sexo</strong></li><li><strong>mercado</strong></li><li><strong>versión del SO</strong></li><li><strong>tipo de dispositivo</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>SandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>Fecha</strong></li><li><strong>applicationId</strong></li><li><strong>addonProductId</strong></li><li><strong>cantidad de adquisición</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo:  <em>&amp;groupby = age, mercado&amp;aggregationLevel = semana</em></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En los ejemplos siguientes se muestran varias solicitudes para obtener datos de compra de complementos. Reemplace el *addonProductId* y *applicationId* valores con el identificador de Store apropiado para su aplicación o complemento.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?applicationId=BRRT4NJ9B3D1&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and age ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción         |
|------------|--------|------------------|
| Valor      | array  | Matriz de objetos que contienen los datos de compra agregados del complemento. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de compra del complemento](#add-on-acquisition-values) que encontrarás a continuación.                                                                                                              |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero hay más de 10000 filas de datos de compra de complementos para la solicitud. |
| TotalCount | entero    | El número total de filas del resultado de datos de la consulta.    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>Valores de compra del complemento

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo    | Descripción        |
|---------------------|---------|---------------------|
| fecha                | string  | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| addonProductId      | string  | El *productId* del complemento para el que va a recuperar datos de adquisición.                                                                                                                                                                 |
| addonProductName    | string  | Nombre del complemento que quieres que se muestre. Este valor solo aparece en los datos de respuesta si la *aggregationLevel* parámetro está establecido en **día**, a menos que especifique el **addonProductName** campo el *groupby* parámetro.                                                                                                                                                                                                            |
| applicationId       | string  | El *productId* de la aplicación para la que desea recuperar los datos de adquisición de complemento.                                                                                                                                                           |
| applicationName     | string  | El nombre para mostrar del juego.                                                                                                                                                                                                             |
| deviceType          | string  | <p>Una de las cadenas siguientes que especifica el tipo de dispositivo que ha completado la adquisición:</p> <ul><li>"PC"</li><li>"Teléfono"</li><li>"Consola"</li><li>"IoT"</li><li>"Server"</li><li>"Tablet PC"</li><li>"Holographic"</li><li>"Desconocido"</li></ul>                                                                                                  |
| storeClient         | string  | <p>Una de las cadenas siguientes que indica la versión de la Store donde se produce la adquisición:</p> <ul><li>"Windows Phone Store (cliente)"</li><li>"Microsoft Store (cliente)" (o "Windows Store (cliente)" si la consulta de datos antes del 23 de marzo de 2018)</li><li>"Microsoft Store (web)" (o "Windows Store (web)" si la consulta de datos antes del 23 de marzo de 2018)</li><li>"Compras de volumen por organizaciones"</li><li>"Otros"</li></ul>                                                                                            |
| osVersion           | string  | Versión del sistema operativo en el que se realizó la compra. Este método, este valor siempre es "Windows 10".                                                                                                   |
| market              | string  | Código de país ISO 3166 del mercado donde se realizó la compra.                                                                                                                                                                  |
| gender              | string  | <p>Una de las cadenas siguientes que especifica el sexo del usuario que ha realizado la adquisición:</p> <ul><li>"m"</li><li>"f"</li><li>"Desconocido"</li></ul>                                                                                                    |
| age            | string  | <p>Una de las cadenas siguientes que indica el grupo de edad del usuario que ha realizado la adquisición:</p> <ul><li>"menos de 13"</li><li>"13-17"</li><li>"18 a 24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>"mayor que 55"</li><li>"Desconocido"</li></ul>                                                                                                 |
| acquisitionType     | string  | <p>Una de las siguientes cadenas que indica el tipo de adquisición:</p> <ul><li>"Gratuito"</li><li>"Trial"</li><li>"Pagado"</li><li>"Código de promoción"</li><li>"Iap"</li><li>"Suscripción Iap"</li><li>"Audience privada"</li><li>"Pre Order"</li><li>"Xbox Game Pass" (o "Juego pasar" si la consulta de datos antes del 23 de marzo de 2018)</li><li>"Disco"</li><li>"Código de prepago"</li><li>"Cobra orden anterior a"</li><li>"Cancelar pedido Pre"</li><li>"Error de orden anterior a"</li></ul>                                                                                                    |
| acquisitionQuantity | número entero | Número de compras que se realizaron.                        |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2018-10-18",
      "addonProductId": " BRRT4NJ9B3D2",
      "addonProductName": "Contoso add-on 7",
      "applicationId": "BRRT4NJ9B3D1",
      "applicationName": "Contoso Demo",
      "deviceType": "Console",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows 10",
      "market": "GB",
      "gender": "m",
      "age": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "addonacquisitions?applicationId=BRRT4NJ9B3D1&addonProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```
