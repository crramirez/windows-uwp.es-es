---
description: Use este URI de REST para obtener bloques de datos para una aplicación de escritorio durante un intervalo de fechas determinado y los demás filtros opcionales.
title: Obtener bloques de actualización de la aplicación de escritorio
ms.date: 07/11/2018
ms.topic: article
keywords: Windows 10, bloques de aplicación de escritorio, programa de aplicación de escritorio de Windows
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4bd57d61a3d0ee942742ab1688849ab181c8c71d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372104"
---
# <a name="get-upgrade-blocks-for-your-desktop-application"></a>Obtener bloques de actualización de la aplicación de escritorio

Use este URI de REST para obtener información acerca de los dispositivos Windows 10 en el que la aplicación de escritorio está bloqueando la actualización a Windows 10 que no se ejecuten. Puede usar este URI solo para aplicaciones de escritorio que se ha agregado a la [programa de aplicación de escritorio de Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Esta información también está disponible en el [aplicación bloquea informe](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report) para aplicaciones de escritorio en el centro de partners.

Para obtener detalles acerca de los bloques de dispositivo para un archivo ejecutable específico en su aplicación de escritorio, consulte [Get actualización bloque los detalles de la aplicación de escritorio](get-desktop-block-data-details.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits```|


### <a name="request-header"></a>Encabezado de la solicitud

| Header        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El identificador de producto de la aplicación de escritorio para el que desea recuperar datos del bloque. Para obtener el identificador de producto de una aplicación de escritorio, abra cualquier [análisis de informes para su aplicación de escritorio en el centro de partners](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) (como el **bloquea informe**) y recuperar el identificador de producto de la dirección URL. |  Sí  |
| startDate | date | La fecha de inicio del intervalo de fechas de bloques de datos para recuperar. El valor predeterminado es 90 días antes de la fecha actual. |  No  |
| endDate | date | La fecha de finalización del intervalo de fechas de bloques de datos para recuperar. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter | string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>fileName</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>productName</strong></li><li><strong>targetOs</strong></li></ul> | No   |
| orderby | string | Una instrucción que ordena el resultado de los valores de datos para cada bloque. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser uno de los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>date</strong><li><strong>deviceType</strong></li><li><strong>fileName</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>productName</strong></li><li><strong>targetOs</strong></li><li><strong>deviceCount</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>fileName</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>targetOs</strong></li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>productName</strong></li><li><strong>deviceCount</strong></li></ul></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

El ejemplo siguiente muestra varias solicitudes para obtener datos del bloque de aplicación de escritorio. Reemplace el *applicationId* valor con el identificador de producto para su aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits?applicationId=5126873772241846776&startDate=2018-05-01&endDate=2018-06-07&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits?applicationId=5126873772241846776&startDate=2018-05-01&endDate=2018-06-07&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Una matriz de objetos que contienen datos agregados de bloque. Para más información sobre los datos de cada objeto, consulta la tabla siguiente. |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, este valor se devuelve si el **superior** parámetro de la solicitud se establece en 10000, pero hay más de 10000 filas de datos de bloque para la consulta. |
| TotalCount | entero    | El número total de filas del resultado de datos de la consulta. |


Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | El identificador de producto de la aplicación de escritorio para el que se recuperaron datos de bloque. |
| date                | string | La fecha asociada con el valor de aciertos de bloque. |
| productName         | string | El nombre de la aplicación de escritorio derivado de los metadatos de su(s) ejecutable(s) asociado(s). |
| fileName            | string | El archivo ejecutable que se ha bloqueado. |
| applicationVersion  | string | La versión de la aplicación ejecutable que se ha bloqueado. |
| osVersion           | string | Una de las siguientes cadenas que especifica la versión del sistema operativo en el que se está ejecutando la aplicación de escritorio:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unknown</strong></li></ul>   |
| osRelease           | string | Una de las siguientes cadenas que especifica la versión de sistema operativo o distribución de paquetes piloto anillo (como un subpoblación dentro de la versión del sistema operativo) en que la aplicación de escritorio se está ejecutando actualmente.<p/><p>Para Windows 10:</p><ul><li><strong>Versión 1507</strong></li><li><strong>Versión 1511</strong></li><li><strong>Versión 1607</strong></li><li><strong>Versión 1703</strong></li><li><strong>Versión 1709</strong></li><li><strong>Versión preliminar</strong></li><li><strong>Insider rápido</strong></li><li><strong>Insider lenta</strong></li></ul><p/><p>Para Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Para Windows Server 2016:</p><ul><li><strong>Versión 1607</strong></li></ul><p>Para Windows 8.1:</p><ul><li><strong>Update 1</strong></li></ul><p>Para Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si se desconoce la versión del sistema operativo o el canal de actualizaciones, este campo tiene el valor <strong>Unknown</strong>.</p> |
| market              | string | El código de país ISO 3166 del mercado en el que se bloquea la aplicación de escritorio. |
| deviceType          | string | Una de las siguientes cadenas que especifica el tipo de dispositivo en el que se bloquea la aplicación de escritorio:<p/><ul><li><strong>PC</strong></li><li><strong>Server</strong></li><li><strong>Tablet</strong></li><li><strong>Unknown</strong></li></ul> |
| blockType            | string | Una de las siguientes cadenas que especifica el tipo de bloque que se encuentra en el dispositivo:<p/><ul><li><strong>Sedimentos posibles</strong></li><li><strong>Sedimentos temporal</strong></li><li><strong>Notificación en tiempo de ejecución</strong></li></ul><p/><p/>Para obtener más información acerca de estos tipos de bloques y su significado para los desarrolladores y usuarios, vea la descripción de la [aplicación bloquea informe](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report).  |
| architecture        | string | La arquitectura del dispositivo en el que existe el bloque: <p/><ul><li><strong>ARM64</strong></li><li><strong>X86</strong></li></ul> |
| targetOs            | string | Una de las siguientes cadenas que especifica la versión del sistema operativo de Windows 10 en el que se bloquea la aplicación de escritorio se ejecute: <p/><ul><li><strong>Versión 1709</strong></li><li><strong>Versión 1803</strong></li></ul> |
| deviceCount         | número | El número de distintos dispositivos que tienen bloques en el nivel de agregación especificado. |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
     "applicationId": "10238467886765136388",
     "date": "2018-06-03",
     "productName": "Contoso Demo",
     "fileName": "contosodemo.exe",
     "applicationVersion": "2.2.2.0",
     "osVersion": "Windows 8.1",
     "osRelease": "Update 1",
     "market": "ZA",
     "deviceType": "All",
     "blockType": "Runtime Notification",
     "architecture": "X86",
     "targetOs": "RS4",
     "deviceCount": 120
    }
  ],
  "@nextLink": "desktop/blockhits?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>Temas relacionados

* [Programa de aplicación de escritorio de Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [Obtener detalles del bloqueo de actualización para su aplicación de escritorio](get-desktop-block-data-details.md)
