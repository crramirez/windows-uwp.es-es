---
description: Use este URI de REST para obtener datos de bloque para una aplicación de escritorio durante un intervalo de fechas determinado y otros filtros opcionales.
title: Obtener bloques de actualización de la aplicación de escritorio
ms.date: 07/11/2018
ms.topic: article
keywords: Windows 10, bloques de aplicación de escritorio, programa de aplicación de escritorio de Windows
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 65cb156d313f398517e174b12520411d5586e22c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158559"
---
# <a name="get-upgrade-blocks-for-your-desktop-application"></a>Obtener bloques de actualización de la aplicación de escritorio

Use este URI de REST para obtener información acerca de los dispositivos de Windows 10 en los que la aplicación de escritorio está bloqueando la ejecución de una actualización de Windows 10. Puede usar este URI solo para las aplicaciones de escritorio que ha agregado al [programa de aplicación de escritorio de Windows](/windows/desktop/appxpkg/windows-desktop-application-program). Esta información también está disponible en el [Informe de bloques de aplicación](/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report) para aplicaciones de escritorio en el centro de Partners.

Para obtener detalles acerca de los bloques de dispositivo para un archivo ejecutable específico en la aplicación de escritorio, consulte [obtención de detalles de bloque de actualización para la aplicación de escritorio](get-desktop-block-data-details.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits```|


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | string | El ID. del producto de la aplicación de escritorio para la que desea recuperar los datos de bloque. Para obtener el identificador de producto de una aplicación de escritorio, abra el [Informe de análisis de la aplicación de escritorio en el centro de Partners](/windows/desktop/appxpkg/windows-desktop-application-program) (como el **Informe de bloques**) y recupere el identificador de producto de la dirección URL. |  Sí  |
| startDate | date | Fecha de inicio del intervalo de fechas de datos de bloque que se va a recuperar. El valor predeterminado es 90 días antes de la fecha actual. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de los datos de bloque que se van a recuperar. La fecha predeterminada es la actual. |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter | string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y el valor que están asociados a los operadores **EQ** o **ne** , y las instrucciones se pueden combinar con **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puede especificar los siguientes campos del cuerpo de la respuesta:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>fileName</strong></li><li><strong>datamarket</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>NombreDeProducto</strong></li><li><strong>destinos</strong></li></ul> | No   |
| orderby | string | Instrucción que ordena los valores de los datos de resultado de cada bloque. La sintaxis es <em>OrderBy = Field [order], Field [order],...</em>. El parámetro de <em>campo</em> puede ser uno de los siguientes campos del cuerpo de respuesta:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>date</strong><li><strong>deviceType</strong></li><li><strong>fileName</strong></li><li><strong>datamarket</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>NombreDeProducto</strong></li><li><strong>destinos</strong></li><li><strong>deviceCount</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>ASC</strong>.</p><p>Este es un ejemplo de cadena <em>OrderBy</em> : <em>OrderBy = Date, Market</em></p> |  No  |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puede especificar los siguientes campos del cuerpo de la respuesta:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>fileName</strong></li><li><strong>datamarket</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>destinos</strong></li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>NombreDeProducto</strong></li><li><strong>deviceCount</strong></li></ul></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestran varias solicitudes para obtener datos de bloque de aplicaciones de escritorio. Reemplace el valor de *ApplicationID* por el ID. de producto de la aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits?applicationId=5126873772241846776&startDate=2018-05-01&endDate=2018-06-07&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits?applicationId=5126873772241846776&startDate=2018-05-01&endDate=2018-06-07&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Value      | array  | Matriz de objetos que contienen datos de bloque agregado. Para obtener más información acerca de los datos de cada objeto, vea la tabla siguiente. |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, este valor se devuelve si el parámetro **Top** de la solicitud se establece en 10000, pero hay más de 10000 filas de datos de bloque para la consulta. |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta. |


Los elementos de la matriz *Value* contienen los siguientes valores.

| Value               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | El ID. del producto de la aplicación de escritorio para la que recuperó los datos de bloque. |
| date                | string | Fecha asociada al valor de aciertos de bloque. |
| ProductName         | string | Nombre para mostrar de la aplicación de escritorio que se deriva de los metadatos de los archivos ejecutables asociados. |
| fileName            | string | Ejecutable que se ha bloqueado. |
| applicationVersion  | string | Versión del ejecutable de la aplicación que se ha bloqueado. |
| osVersion           | string | Una de las siguientes cadenas que especifica la versión del sistema operativo en la que se está ejecutando actualmente la aplicación de escritorio:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unknown</strong></li></ul>   |
| osRelease           | string | Una de las siguientes cadenas que especifica la versión del sistema operativo o el anillo de vuelo (como un rellenado de la versión del sistema operativo) en el que se está ejecutando actualmente la aplicación de escritorio.<p/><p>Para Windows 10:</p><ul><li><strong>Versión 1507</strong></li><li><strong>Versión 1511</strong></li><li><strong>Versión 1607</strong></li><li><strong>Versión 1703</strong></li><li><strong>Versión 1709</strong></li><li><strong>Versión preliminar</strong></li><li><strong>Insider rápido</strong></li><li><strong>Insider lento</strong></li></ul><p/><p>Para Windows Server 1709:</p><ul><li><strong>VERSIÓN</strong></li></ul><p>Para Windows Server 2016:</p><ul><li><strong>Versión 1607</strong></li></ul><p>Para Windows 8.1:</p><ul><li><strong>Actualización 1</strong></li></ul><p>Para Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si se desconoce la versión del sistema operativo o el anillo de vuelo, este campo tiene el valor <strong>desconocido</strong>.</p> |
| market              | string | El código de país ISO 3166 del mercado en el que se bloquea la aplicación de escritorio. |
| deviceType          | string | Una de las siguientes cadenas que especifica el tipo de dispositivo en el que se bloquea la aplicación de escritorio:<p/><ul><li><strong>PC</strong></li><li><strong>Server</strong></li><li><strong>Tableta</strong></li><li><strong>Unknown</strong></li></ul> |
| blockType            | string | Una de las siguientes cadenas que especifica el tipo de bloque que se encuentra en el dispositivo:<p/><ul><li><strong>Sedimentos potenciales</strong></li><li><strong>Sedimentos temporales</strong></li><li><strong>Notificación en tiempo de ejecución</strong></li></ul><p/><p/>Para obtener más información sobre estos tipos de bloques y su significado para los desarrolladores y usuarios, consulte la descripción del [Informe de bloques de aplicación](/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report).  |
| arquitectura        | string | La arquitectura del dispositivo en el que existe el bloque: <p/><ul><li><strong>ARM64</strong></li><li><strong>Microprocesador</strong></li></ul> |
| destinos            | string | Una de las siguientes cadenas que especifica la versión del sistema operativo Windows 10 en la que se bloquea la ejecución de la aplicación de escritorio: <p/><ul><li><strong>Versión 1709</strong></li><li><strong>Versión 1803</strong></li></ul> |
| deviceCount         | number | El número de dispositivos distintos que tienen bloques en el nivel de agregación especificado. |


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

* [Programa de aplicación de escritorio de Windows](/windows/desktop/appxpkg/windows-desktop-application-program)
* [Get upgrade block details for your desktop application](get-desktop-block-data-details.md) (Obtener detalles de bloques de actualización para la aplicación de escritorio)