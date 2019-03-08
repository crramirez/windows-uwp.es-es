---
description: Use este URI de REST para obtener datos de instalación agregado para una aplicación de escritorio durante un intervalo de fechas determinado y los demás filtros opcionales.
title: Obtener instalaciones de aplicaciones de escritorio
ms.date: 03/01/2018
ms.topic: article
keywords: Windows 10, se instala una aplicación de escritorio, programa de aplicación de escritorio de Windows
localizationpriority: medium
ms.openlocfilehash: f9beb73dafa96c489b8a4c9dddecf59d8a047109
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612980"
---
# <a name="get-desktop-application-installs"></a>Obtener instalaciones de aplicaciones de escritorio


Use este URI de REST para obtener datos de instalación agregado en formato JSON para una aplicación de escritorio que se ha agregado a la [programa de aplicación de escritorio de Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504). Este URI le permite obtener datos de instalación durante un intervalo de fechas determinado y los demás filtros opcionales. Esta información también está disponible en el [instala informe](https://msdn.microsoft.com/library/windows/desktop/mt826504) para aplicaciones de escritorio en el centro de partners.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily```|


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El identificador de producto de la aplicación de escritorio para el que desea recuperar los datos de la instalación. Para obtener el identificador de producto de una aplicación de escritorio, abra cualquier [análisis de informes para su aplicación de escritorio en el centro de partners](https://msdn.microsoft.com/library/windows/desktop/mt826504) (como el **instala informe**) y recuperar el identificador de producto de la dirección URL. |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos de instalación que se han de recuperar. El valor predeterminado es 90 días antes de la fecha actual. |  No  |
| endDate | fecha | La fecha de finalización del intervalo de fechas de los datos de instalación que se han de recuperar. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter | string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li></ul> | No   |
| orderby | string | Instrucción que ordena los valores de datos resultantes de cada instalación. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser uno de los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>productName</strong></li><li><strong>date</strong><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>installBase</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>productName</strong></li><li><strong>installBase</strong></li></ul></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

El ejemplo siguiente muestra varias solicitudes para obtener la aplicación de escritorio de instalación los datos. Reemplace el *applicationId* valor con el identificador de producto para su aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily?applicationId=1234567890&startDate=2018-01-01&endDate=2018-02-01&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily?applicationId=1234567890&startDate=2018-01-01&endDate=2018-02-01&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Una matriz de objetos que contienen los datos de instalación agregados. Para más información sobre los datos de cada objeto, consulta la tabla siguiente. |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero hay más de 10 000 filas de datos de instalación para la solicitud. |
| TotalCount | entero    | El número total de filas del resultado de datos de la consulta. |


Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| fecha                | string | La fecha asociada con la instalación del valor base. |
| applicationId       | string | Instalar el identificador de producto de la aplicación de escritorio para el que recuperan los datos de. |
| productName         | string | El nombre de la aplicación de escritorio derivado de los metadatos de su(s) ejecutable(s) asociado(s). |
| applicationVersion  | string | La versión de la aplicación ejecutable que se instaló. |
| deviceType          | string | Una de las siguientes cadenas que especifica el tipo de dispositivo en el que está instalada la aplicación de escritorio:<p/><ul><li><strong>PC</strong></li><li><strong>Server</strong></li><li><strong>Tablet</strong></li><li><strong>Unknown</strong></li></ul> |
| market              | string | El código de país ISO 3166 del mercado en el que se instala la aplicación de escritorio. |
| osVersion           | string | Una de las cadenas siguientes que especifica la versión del sistema operativo en la que se realizó la instalación de la aplicación de escritorio:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unknown</strong></li></ul>   |
| osRelease           | string | Una de las siguientes cadenas que especifica la versión del sistema operativo o el canal de actualizaciones (como una subpoblación dentro de la versión del sistema operativo) en el que se ha instalado la aplicación de escritorio.<p/><p>Para Windows 10:</p><ul><li><strong>Versión 1507</strong></li><li><strong>Versión 1511</strong></li><li><strong>Versión 1607</strong></li><li><strong>Versión 1703</strong></li><li><strong>Versión 1709</strong></li><li><strong>Versión preliminar</strong></li><li><strong>Insider rápido</strong></li><li><strong>Insider lenta</strong></li></ul><p/><p>Para Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Para Windows Server 2016:</p><ul><li><strong>Versión 1607</strong></li></ul><p>Para Windows 8.1:</p><ul><li><strong>Update 1</strong></li></ul><p>Para Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si se desconoce la versión del sistema operativo o el canal de actualizaciones, este campo tiene el valor <strong>Unknown</strong>.</p> |
| installBase         | número | El número de distintos dispositivos que tenían el producto instalado en el nivel de agregación especificado. |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2018-01-24",
      "applicationId": "123456789",
      "productName": "Contoso Demo",
      "applicationVersion": "1.0.0.0",
      "deviceType": "PC",
      "market": "All",
      "osVersion": "Windows 10",
      "osRelease": "Version 1709",
      "installBase": 348218.0
    }
  ],
  "@nextLink": "desktop/installbasedaily?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>Temas relacionados

* [Programa de aplicación de escritorio de Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504)
