---
title: VerifyStringResult (JSON)
assetID: 272c688e-179e-c7e9-086b-e76d0d4bcb57
permalink: en-us/docs/xboxlive/rest/json-verifystringresult.html
description: " VerifyStringResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b01793222be80efccdca1f24f5226a2e9ff78064
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599490"
---
# <a name="verifystringresult-json"></a>VerifyStringResult (JSON)
Como resultado de códigos correspondientes a cada cadena enviada a [/system/strings/validate](../uri/stringserver/uri-systemstringsvalidate.md).
<a id="ID4ER"></a>


## <a name="verifystringresult"></a>VerifyStringResult

El objeto VerifyStringResult tiene la especificación siguiente.

| Miembro| Tipo| Descripción|
| --- | --- | --- |
| resultCode| entero de 32 bits sin signo| Obligatorio. Código de HResult correspondiente a enviar la cadena.|
| offendingString| string| Obligatorio. Valor de cadena que ha provocado que se rechazó una cadena.|

<a id="ID4EXB"></a>


## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo


```json
{
    "verifyStringResult":
    [
        {"resultCode": "1", "offendingString": "badword"},
        {"resultCode": "0", "offendingString": ""},
        {"resultCode": "0", "offendingString": ""},
        {"resultCode": "0", "offendingString": ""},
    ]
}

```


#### <a name="common-hresult-values"></a>Valores HResult comunes

| Valor| Nombre del error|
| --- | --- | --- | --- | --- |
| 0| Correcto|
| 1| Cadena ofensivo|
| 2| Cadena demasiado larga|
| 3| Error desconocido|

<a id="ID4ELD"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4END"></a>


##### <a name="parent"></a>Primario

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>Referencia

[POST (/system/strings/validate)](../uri/stringserver/uri-systemstringsvalidatepost.md)
