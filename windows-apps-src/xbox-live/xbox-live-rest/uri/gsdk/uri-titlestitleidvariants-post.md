---
title: POST (/titles/{titleId}/variants)
assetID: 84303448-5a11-d96f-907d-77f57f859741
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidvariants-post.html
description: " POST (/titles/{titleId}/variants)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 17974ddf7dec26abac18ccee9fda5249bc9d656f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618500"
---
# <a name="post-titlestitleidvariants"></a>POST (/titles/{titleId}/variants)
URI que se llama a un cliente que recupera una lista de variantes de juegos para el título especificado identificador. Los dominios para estos URI son `gameserverds.xboxlive.com` y `gameserverms.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EZ)
  * [Encabezados de solicitud necesarios](#ID4EIB)
  * [Encabezados de solicitud opcionales](#ID4EED)
  * [Autorización](#ID4E3D)
  * [Cuerpo de la solicitud](#ID4EEE)
  * [Encabezados de respuesta necesaria](#ID4ELF)
  * [Encabezados de respuesta opcional](#ID4EMG)
  * [Cuerpo de respuesta](#ID4EEH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Descripción| 
| --- | --- | 
| titleid| Id. del título que debe operar la solicitud.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Nombre de host

gameserverds.xboxlive.com
 
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
Al realizar una solicitud, los encabezados que se muestra en la tabla siguiente son necesarios.
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | --- | --- | 
| Content-Type| application/json| Tipo de datos que se está enviadas.| 
| Host| gameserverds.xboxlive.com|  | 
| Content-Length|  | Longitud del objeto de solicitud.| 
| x-xbl-contract-version| 1| Versión del contrato de API.| 
| Autorización| XBL3.0 x = [hash]; [token]| Token de autenticación.| 
  
<a id="ID4EED"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
Al realizar una solicitud, los encabezados que se muestra en la tabla siguiente son opcionales.
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| X-XblCorrelationId|  | El tipo mime del cuerpo de la solicitud.| 
  
<a id="ID4E3D"></a>

 
## <a name="authorization"></a>Autorización

La solicitud debe incluir un encabezado de autorización válido de Xbox Live. Si el llamador no tiene permiso para tener acceso a este recurso, el servicio devuelve 403 Prohibido en respuesta. Si el encabezado está ausente o no válido, el servicio devuelve 401 no autorizado en la respuesta.
 
<a id="ID4EEE"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
La solicitud debe contener un objeto JSON con los siguientes miembros.
 
| Miembro| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| configuración regional| Local de variantes a devolver.| 
| maxVariants| El número máximo de variantes a devolver.| 
| publisherOnly|  | 
| Restricción|  | 
 
<a id="ID4EDF"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo
 

```cpp
{
  "locale": "en-us",
  "maxVariants": "100",
  "publisherOnly": "false",
  "restriction": null
}

```

   
<a id="ID4ELF"></a>

 
## <a name="required-response-headers"></a>Encabezados de respuesta necesaria
 
Una respuesta siempre incluirá los encabezados que se muestra en la tabla siguiente.
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| application/json| Tipo de datos en el cuerpo de respuesta.| 
| Content-Length|  | Longitud del cuerpo de respuesta.| 
  
<a id="ID4EMG"></a>

 
## <a name="optional-response-headers"></a>Encabezados de respuesta opcional
 
Una respuesta puede incluir los encabezados que se muestra a continuación.
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-XblCorrelationId|  | El tipo mime del cuerpo de respuesta.| 
  
<a id="ID4EEH"></a>

 
## <a name="response-body"></a>Cuerpo de respuesta
 
Si la llamada se realiza correctamente, el servicio devolverá un objeto JSON con los siguientes miembros.
 
| Miembro| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Variantes| Una matriz de variantes.| 
| variantId| El identificador de una variante.| 
| name| El nombre de una variante.| 
| isPublisher|  | 
| rango|  | 
| gameVariantSchemaId|  | 
| variantSchemas| Una matriz de variantes esquemas.| 
| variantSchemaId| El identificador del esquema.| 
| schemaContent| El contenido de esquema| 
| name| Nombre del esquema| 
| gsiSets| Establece una matriz de GSI.| 
| minRequiredPlayers| El número mínimo de reproductores para la variante.| 
| maxAllowedPlayers| El número máximo de reproductores para la variante.| 
| gsiSetId| El identificador del conjunto GSI.| 
| gsiSetName| El nombre del conjunto GSI.| 
| selectionOrder|  | 
| variantSchemaId| Identificador del esquema Variant utilizado en el GSI establecido.| 
 
<a id="ID4EYBAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
 "variants": [
     { 
       "variantId": "8B6EF8A0-7807-42C4-9CB0-1D9B8B8CE742", 
       "name": "tankWarsV2.0",
       "isPublisher": "true",
       "rank": "1",
       "gameVariantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB"
     }],
  "variantSchemas": [
     {
        "variantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB",
        "schemaContent": "&lt;?xml version=\"1.0\" encoding=\"UTF-8\" ?>&lt;xs:schema xmlns:xs=\"http://www.w3.org/2001/XMLSchema\">&lt;xs:element name=\"root\">&lt;/xs:element>&lt;/xs:schema>"
        "name": "tanksSchema"
     }],
     "gsiSets":
     [{ 
          "minRequiredPlayers": "5", 
          "maxAllowedPlayers": "10", 
          "gsiSetId": "B28047F5-B52F-477E-97C2-4C1C39E31D42",
          "gsiSetName": "TanksGSISet",
          "selectionOrder": "1",
          "variantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB"
     }]
 }

  

```

   
<a id="ID4ERCAC"></a>

 
## <a name="see-also"></a>Consulte también
 [/titles/{titleId}/variants](uri-titlestitleidvariants.md)

  