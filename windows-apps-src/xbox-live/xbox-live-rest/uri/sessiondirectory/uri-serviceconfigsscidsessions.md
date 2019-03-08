---
title: /serviceconfigs/{scid}/sessions
assetID: d1932817-dcce-5a5c-d7e4-9fd97e1d287c
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessions.html
description: " /serviceconfigs/{scid}/sessions"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 700575ee9e9afa7bc7a1d34388dc071873d3b9ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612580"
---
# <a name="serviceconfigsscidsessions"></a>/serviceconfigs/{scid}/sessions
Admite una operación GET para recuperar un conjunto de documentos de la sesión. 
<a id="ID4EO"></a>

 
## <a name="domain"></a>Dominio
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| scid| GUID| Identificador de configuración de servicio (¿SCID). Identificador de la parte 1 de la sesión.| 
| Palabra clave| string| Una palabra clave utilizada para filtrar los resultados a solo las sesiones identificados con esa cadena.| 
| xuid| GUID| Identificadores de usuario de Xbox para los usuarios para que se van a recuperar las sesiones. Los usuarios deben estar activos en las sesiones.| 
| reservas de direcciones| string| Valor que indica si la lista de sesiones incluye aquellas que los usuarios no han aceptado. Este parámetro solo se puede establecer en true. Esta configuración requiere que el llamador tenga acceso de nivel de servidor a la sesión, o puede solicitar XUID del llamador para que coincida con el filtro de Id. de usuario de Xbox. | 
| inactivo| string| Valor que indica si la lista de sesiones incluye aquellas que los usuarios han aceptado, pero no se reproducción activamente. Este parámetro solo se puede establecer en true.| 
| privado| string| Valor que indica si la lista de sesiones incluye sesiones privadas. Este parámetro solo se puede establecer en true. Es válido solo cuando se consultan sus propias sesiones o al consultar el servidor a servidor. Si establece este parámetro en true requiere que el llamador tener acceso de nivel de servidor a la sesión, o puede solicitar XUID del llamador para que coincida con el filtro de Id. de usuario de Xbox. | 
| visibility| string| Valor de enumeración que indica el estado de visibilidad que se usa en el filtrado de resultados. Actualmente este parámetro solo puede establecerse a Open para incluir las sesiones abiertas. Consulte <b>MultiplayerSessionVisibility</b>.| 
| version| string| Un entero positivo que indica la versión de sesión principal o inferior de las sesiones que incluyen. El valor debe ser menor o igual a la versión de contrato de la solicitud de módulo 100.| 
| Take| string| Un entero positivo que indica el número máximo de sesiones para recuperar.| 
  
<a id="ID4E1D"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/serviceconfigs/{scid}/sessions)](uri-serviceconfigsscidsessionsget.md)

&nbsp;&nbsp;Recupera información de la sesión el especificado.
 
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>Primario 

[URI del directorio de sesión](atoc-reference-sessiondirectory.md)

   