---
title: Solicitud HTTP estándar y títulos de respuesta
assetID: a5f8fd96-9393-5234-04ad-837e5c117c92
permalink: en-us/docs/xboxlive/rest/httpstandardheaders.html
description: " Solicitud HTTP estándar y títulos de respuesta"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ca97e82365eab40266b3ffdd84924f71289eede6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636520"
---
# <a name="standard-http-request-and-response-headers"></a>Solicitud HTTP estándar y títulos de respuesta
 
<a id="ID4ES"></a>

 
## <a name="request-headers"></a>Encabezados de solicitud
 
En la tabla siguiente se enumera los encabezados HTTP estándar que se utiliza al realizar solicitudes de servicios de Xbox Live.
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | 
| x-xbl-contract-version| 1| Versión del contrato de API. Todas las solicitudes de servicios de Xbox Live requieren.| 
| Autorización| STSTokenString| Token de autenticación del STS. El valor de este encabezado se recupera de la <b>GetTokenAndSignatureResult.Token</b> propiedad. | 
| Content-Type| aplicación/xml, application/json, multipart/form-data o application/x--www-form-urlencoded| Especifica el tipo de contenido que se envía con una solicitud.| 
| Content-Length| Valor entero| Especifica la longitud de los datos se envíen en una solicitud POST.| 
| Aceptar idioma | Cadena| Especifica cómo adaptar las cadenas devueltas. Consulte <a href="https://msdn.microsoft.com/en-us/library/bb975829.aspx">programación de Advanced Xbox 360</a> para obtener una lista de combinaciones válidas de idioma o configuración regional.| 
  
<a id="ID4E6C"></a>

 
## <a name="response-headers"></a>Encabezados de respuesta
 
La tabla siguiente muestra el encabezado HTTP estándar utilizado en las respuestas de servicios de Xbox Live.
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| application/xml, application/json| Especifica el tipo de contenido que se devuelve.| 
| Content-Length| Valor entero| Especifica la longitud de los datos devueltos.| 
  
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>Primario  

[Referencia adicional](atoc-xboxlivews-reference-additional.md)

   