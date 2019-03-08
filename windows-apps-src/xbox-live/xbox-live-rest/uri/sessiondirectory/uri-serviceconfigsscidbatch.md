---
title: /serviceconfigs/{scid}/batch
assetID: eb1b510f-d92e-ae9b-a3e6-0edf58b4f075
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidbatch.html
description: " /serviceconfigs/{scid}/batch"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 208ee92106563372dd4d92a8c800cc08f513e8c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589720"
---
# <a name="serviceconfigsscidbatch"></a>/serviceconfigs/{scid}/batch
Admite una operación POST para una consulta por lotes en el nivel de identificador de configuración de servicio.

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

<a id="ID4ESB"></a>


## <a name="valid-methods"></a>Métodos válidos

[POST (/serviceconfigs/{scid}/batch)](uri-serviceconfigsscidbatchpost.md)

&nbsp;&nbsp;Crea una consulta por lotes varios Xbox los identificadores de usuario para la configuración del servicio.

<a id="ID4E3B"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4E5B"></a>


##### <a name="parent"></a>Primario

[URI del directorio de sesión](atoc-reference-sessiondirectory.md)
