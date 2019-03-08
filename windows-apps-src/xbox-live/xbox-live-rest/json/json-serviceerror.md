---
title: ServiceError (JSON)
assetID: 81c43f6e-bfff-c4b5-d25c-eace22649f01
permalink: en-us/docs/xboxlive/rest/json-serviceerror.html
description: " ServiceError (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: da3d682a1b66d25a12f21a93e9596d13afae7f90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631710"
---
# <a name="serviceerror-json"></a>ServiceError (JSON)
Contiene información sobre un error devuelto cuando el error en la llamada al servicio. 
<a id="ID4EN"></a>

 
## <a name="serviceerror"></a>ServiceError
 
El objeto depuracuión puede contener tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| code| entero de 32 bits con signo | El tipo de error. Consulte la tabla siguiente para los valores posibles. | 
| Código fuente| string | El nombre del servicio que ha provocado el error. Por ejemplo, un valor de <code>ReputationFD</code> indica que el error se encontraba en el servicio de reputación. | 
| description| string| Una descripción del error. | 
 
<a id="ID4EBC"></a>

 
### <a name="error-codes"></a>Códigos de error
 
| Valor| Descripción| 
| --- | --- | --- | --- | --- | 
| 0| Éxito sin errores| 
| 4000| Documento de cuerpo de la solicitud JSON enviada con una validación de error en la solicitud POST no válido. Consulte el campo de descripción para obtener más información. | 
| 4100| Usuario Does no existe el XUID contenidos en el URI de solicitud no representa un usuario válido en XBOX Live.| 
| 4500| Error de autorización el llamador no está autorizado para realizar la operación solicitada.| 
| 5000| Servicio Error se produjo un error interno en el servicio| 
| 5300| Servicio no disponible el servicio no está disponible.| 
   
<a id="ID4EQE"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
    
```

  
<a id="ID4EZE"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E2E"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   