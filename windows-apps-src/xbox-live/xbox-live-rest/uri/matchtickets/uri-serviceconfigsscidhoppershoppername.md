---
title: /serviceconfigs/{scid}/hoppers/{hoppername}
assetID: ba1e129d-b4c4-6535-46ce-fd184465c85f
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppername.html
description: " /serviceconfigs/{scid}/hoppers/{hoppername}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1db069f59eeba565a257531907d6be0b1dcd189d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598430"
---
# <a name="serviceconfigsscidhoppershoppername"></a>/serviceconfigs/{scid}/hoppers/{hoppername}

Admite una operación POST para crear vales de coincidencia.

> [!IMPORTANT]
> Este identificador URI está pensado para su uso con el contrato 103 o posterior y requiere un elemento de encabezado de X-Xbl-contrato-Version: 103 o posterior en cada solicitud.

<a id="ID4ER"></a>


## <a name="domain"></a>Dominio
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| scid| GUID| El identificador de configuración de servicio (¿SCID) para la sesión.|
| hoppername | string | El nombre de la hopper. |

<a id="ID4E2B"></a>


## <a name="valid-methods"></a>Métodos válidos

[POST (/serviceconfigs/{scid}/hoppers/{hoppername})](uri-serviceconfigsscidhoppershoppernamepost.md)

&nbsp;&nbsp;Crea el vale de coincidencia especificados.

<a id="ID4EFC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EHC"></a>


##### <a name="parent"></a>Primario  

[URI de contactos](atoc-reference-matchtickets.md)
