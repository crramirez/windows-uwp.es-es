---
title: BatchRequest (JSON)
assetID: 2ca34506-8801-efc5-7c83-3c9ec5572276
permalink: en-us/docs/xboxlive/rest/json-batchrequest.html
description: " BatchRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51073c71700613edcd7c22e18cc0c00a9222d7e5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654160"
---
# <a name="batchrequest-json"></a>BatchRequest (JSON)
Una matriz de propiedades con el que se va a filtrar la información de presencia, como usuarios, dispositivos y los títulos.
<a id="ID4EN"></a>


## <a name="batchrequest"></a>BatchRequest

El objeto BatchRequest tiene la especificación siguiente.

| Miembro| Tipo| Descripción|
| --- | --- | --- |
| usuarios| matriz de cadena| XUIDs de lista de usuarios cuya presencia desea aprender, con un máximo de XUIDs 1100 a la vez.|
| deviceTypes| matriz de cadena| Lista de tipos de dispositivos utilizado por los usuarios que desea conocer. Si la matriz se deja vacía, el valor predeterminado es todos los posibles tipos de dispositivos (es decir, no se filtran).|
| titles| matriz de enteros de 32 bits sin signo| Lista de dispositivos cuyos usuarios desea saber acerca de los tipos. Si la matriz se deja vacía, el valor predeterminado es todos los títulos de posibles (es decir, no se filtran).|
| level| string| Posibles valores: <ul><li>usuario - get usuario nodos</li><li>dispositivo - usuario get y nodos de dispositivo</li><li>título: obtener información de nivel básico de título</li><li>todo: obtener información de presencia enriquecida, la información multimedia o ambos</li></ul>El valor predeterminado es "title".| 
| onlineOnly| Valor booleano| Si esta propiedad es true, la operación por lotes filtrará los registros para los usuarios sin conexión (incluidos los ocultos). Si no se proporciona, se devolverán los usuarios en línea y sin conexión.|

<a id="ID4EAD"></a>


## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo


```json
{
  users:
  [
    "1234567890",
    "4567890123",
    "7890123456"
  ]
}


```


<a id="ID4EJD"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4ELD"></a>


##### <a name="parent"></a>Primario

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>Referencia   
