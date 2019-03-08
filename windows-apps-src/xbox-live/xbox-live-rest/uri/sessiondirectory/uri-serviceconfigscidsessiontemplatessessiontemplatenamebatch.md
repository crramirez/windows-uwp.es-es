---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch
assetID: 4f8e1ece-2ba8-9ea4-e551-2a69c499d7b9
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 93ecaa95488493fa8779a44d76bf7d635d1751ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612600"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamebatch"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch
Admite una operación POST para crear una consulta por lotes en el nivel de la plantilla de sesión.

> [!IMPORTANT]
> Este método usa la multijugador 2015 y se aplica solo a esa versión multijugador y versiones posteriores. Está pensado para su uso con el contrato de la plantilla 104/105 o posterior y requiere un elemento de encabezado de X-Xbl-contrato-Version: 105/104 o posterior en cada solicitud.

<a id="ID4ER"></a>


## <a name="domain"></a>Dominio
sessiondirectory.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| scid| GUID| Identificador de configuración de servicio (¿SCID). Parte 1 de identificador de sesión.|
| sessionTemplateName| string| Nombre de la instancia actual de la plantilla de sesión. Parte 2 del identificador de sesión.|

<a id="ID4E2B"></a>


## <a name="valid-methods"></a>Métodos válidos

[POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatchpost.md)

&nbsp;&nbsp;Crea una consulta por lotes en varios identificadores de usuario de Xbox.

<a id="ID4EFC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EHC"></a>


##### <a name="parent"></a>Primario

[URI del directorio de sesión](atoc-reference-sessiondirectory.md)
