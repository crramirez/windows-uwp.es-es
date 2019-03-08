---
title: POST (/system/strings/validate)
assetID: 6a59bc0b-8edd-87bf-efaf-f16efa3bedf7
permalink: en-us/docs/xboxlive/rest/uri-systemstringsvalidatepost.html
description: " POST (/system/strings/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 70e86567f449674c7a046e072437d9ee715dc6d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595510"
---
# <a name="post-systemstringsvalidate"></a>POST (/system/strings/validate)
Acepta una matriz de cadenas para la validación y devuelve una matriz de resultados del mismo tamaño. Es el dominio para estos URI `client-strings.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Encabezados de solicitud necesarios](#ID4EIB)
  * [Cuerpo de la solicitud](#ID4ELC)
  * [Códigos de estado HTTP](#ID4E4C)
  * [Cuerpo de respuesta](#ID4ETF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
Cada resultado indica si la cadena correspondiente es aceptable en Xbox LIVE y contiene la cadena infractora si es aplicable.
 
Las cadenas idénticas siempre dará resultados idénticos. Si recibe un resultado que no es correcto, analizar el resultado y modifique la cadena en consecuencia.
 
 

> [!NOTE] 
> Resultante <b>VerifyStringResult</b> notificará solo la primera palabra incorrecta en la cadena. Puede haber más palabras dentro de la cadena que provoca el error. Si va a reemplazar las palabras infractor para poder usar la cadena, debe reemplazar la palabra o subcadena incorrecto y, a continuación, volver a comprobar la cadena para buscar subcadenas infractor adicionales.  

 
  
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Descripción| 
| --- | --- | --- | 
| Autorización| Token de autenticación. Por ejemplo: XBL3.0 x = [hash]; [símbolo (token)].| 
| x-xbl-contract-version| Versión del contrato de API de entero. Debe ser 1 o 2 para esta API.| 
  
<a id="ID4ELC"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
El cuerpo de solicitud es una matriz de cadenas con ningún límite en el tamaño de la matriz y 512 caracteres por la cadena.
 
<a id="ID4ETC"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo
 

```cpp
{
    "stringstoVerify":
    [
        "Poppycock",
        "The quick brown fox jumped over the lazy dog.",
        "Hey, want to hang out?",
        "oh noes"
    ]
}
      
```

   
<a id="ID4E4C"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| 200| Aceptar| Todas las cadenas se han procesado correctamente. Esto no significa necesariamente que todas las cadenas tenían HResults positivos.| 
| 401| Sin autorización| La solicitud requiere autenticación del usuario.| 
| 403| prohibido| No se permite la solicitud para el usuario o servicio.| 
| 406| No es aceptable| Falta <b>Content-type: application/json</b> encabezado.| 
| 408| Tiempo de espera de solicitud| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
  
<a id="ID4ETF"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
Devuelve una matriz de [VerifyStringResult (JSON)](../../json/json-verifystringresult.md), del mismo tamaño que la matriz de la solicitud.
  
<a id="ID4EAG"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ECG"></a>

 
##### <a name="parent"></a>Primario 

[/system/strings/validate](uri-systemstringsvalidate.md)

   