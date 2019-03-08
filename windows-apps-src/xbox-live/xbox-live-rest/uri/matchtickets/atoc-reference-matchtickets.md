---
title: URI concordantes
assetID: 667b02a9-6f34-8165-001b-ee8782575202
permalink: en-us/docs/xboxlive/rest/atoc-reference-matchtickets.html
description: " URI concordantes"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c52abca7ed49d4a5e14520095ae944938b86f093
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590470"
---
# <a name="matchmaking-uris"></a>URI concordantes
 
Esta sección proporciona detalles sobre las direcciones de identificador de recursos Universal (URI) y los métodos de protocolo de transporte de hipertexto (HTTP) asociados de servicios de Xbox Live para el servicio. 
 
<a id="ID4E6"></a>

 
## <a name="domain"></a>Dominio
momatch.xboxlive.com  
<a id="ID4EEB"></a>

 
## <a name="service-version"></a>Versión del servicio
 
Los llamadores de estos URI de HTTP/REST deben pasar el valor 103 o posterior para X-Xbl-contrato-Version, el encabezado HTTP que especifica la versión del servicio de servicios de detección de entretenimiento (EDS). 
  
<a id="ID4ELB"></a>

 
## <a name="system-objects-and-properties"></a>Las propiedades y los objetos del sistema
 
Actualmente, toda la configuración del servicio de contactos se produce de forma manual, usa la parte de la configuración de servicio de la [Xbox Developer Portal (XDP)](https://xdp.xboxlive.com) o [centro de partners](https://partner.microsoft.com/dashboard). Alguna información de contactos también se refleja en los objetos definidos para el MPSD. 
 
Los objetos principales de JSON se utiliza para configurar los contactos se definen en [MatchTicket (JSON)](../../json/json-matchticket.md) y [HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md). Tenga en cuenta que todos coinciden con los vales debe definir un **ticketSessionRef** objeto para proporcionar una referencia a una sesión de varios jugadores que contiene el Reproductor o reproductores que desean que se debe coincidir con otras personas. 
  
<a id="ID4EBC"></a>

 
## <a name="in-this-section"></a>En esta sección

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)

&nbsp;&nbsp;Admite una operación POST para crear vales de coincidencia. 

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)

&nbsp;&nbsp;Admite una operación GET para recuperar las estadísticas para un hopper.

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)

&nbsp;&nbsp;Admite una operación de eliminación de un vale de coincidencia.
 
<a id="ID4ENC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EPC"></a>

   [MatchTicket (JSON)](../../json/json-matchticket.md)

 [HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md)

 [URI del directorio de sesión](../sessiondirectory/atoc-reference-sessiondirectory.md)

  
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)

   