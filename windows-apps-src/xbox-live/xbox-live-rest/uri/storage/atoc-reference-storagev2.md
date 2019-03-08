---
title: URI de almacenamiento de títulos
assetID: 32bba1e4-0980-785e-c098-a96cd88a8e5f
permalink: en-us/docs/xboxlive/rest/atoc-reference-storagev2.html
description: " URI de almacenamiento de títulos"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e0296eff0937ea5075630db0e049c86e2ea2c8ce
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622680"
---
# <a name="title-storage-uris"></a>URI de almacenamiento de títulos
 
Esta sección proporciona detalles sobre las direcciones de identificador de recursos Universal (URI) y los métodos de protocolo de transporte de hipertexto (HTTP) asociados de servicios de Xbox Live para *título almacenamiento*.
 
Los juegos que se ejecutan en todas las plataformas pueden usar este servicio.
 
El dominio para estos URI es titlestorage.xboxlive.com.
 
<a id="ID4EFB"></a>

 
## <a name="in-this-section"></a>En esta sección

[/global/scids/{scid}](uri-globalscidsscid.md)

&nbsp;&nbsp;Recupera información de cuota de este tipo de almacenamiento.

[/global/scids/{scid}/data/{path}](uri-globalscidssciddatapath.md)

&nbsp;&nbsp;Muestra información de archivo en una ruta de acceso especificada. 

[/global/scids/{scid}/data/{pathAndFileName},{type}](uri-globalscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Descarga un archivo.

[/json/users/batch/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersbatchscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Descarga varios archivos de varios usuarios con el mismo nombre de archivo.

[/json/users/xuid({xuid})/scids/{scid}](uri-jsonusersxuidscidsscid.md)

&nbsp;&nbsp;Recupera información de cuota de este tipo de almacenamiento.

[/json/users/xuid({xuid})/scids/{scid}/data/{path}](uri-jsonusersxuidscidssciddatapath.md)

&nbsp;&nbsp;Muestra información de archivo en una ruta de acceso especificada. 

[/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersxuidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Descarga, carga o elimina un archivo.

[/sessions/{sessionId}/scids/{scid}](uri-sessionssessionidscidsscid.md)

&nbsp;&nbsp;Recupera información de cuota de este tipo de almacenamiento.

[/sessions/{sessionId}/scids/{scid}/data/{path}](uri-sessionssessionidscidssciddatapath.md)

&nbsp;&nbsp;Muestra información de archivo en una ruta de acceso especificada. 

[/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type}](uri-sessionssessionidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Descarga un archivo.

[/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersbatchscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Descarga varios archivos de varios usuarios con el mismo nombre de archivo.

[/trustedplatform/users/xuid({xuid})/scids/{scid}](uri-trustedplatformusersxuidscidsscid.md)

&nbsp;&nbsp;Recupera información de cuota de este tipo de almacenamiento.

[/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}](uri-trustedplatformusersxuidscidssciddatapath.md)

&nbsp;&nbsp;Muestra información de archivo en una ruta de acceso especificada. 

[/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersxuidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Descarga, carga o elimina un archivo.

[/untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}](uri-untrustedplatformusersbatchscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Descarga varios archivos de varios usuarios con el mismo nombre de archivo.

[/untrustedplatform/users/xuid({xuid})/scids/{scid}](uri-untrustedplatformusersxuidscidsscid.md)

&nbsp;&nbsp;Recupera información de cuota de este tipo de almacenamiento.

[/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}](uri-untrustedplatformusersxuidscidssciddatapath.md)

&nbsp;&nbsp;Muestra información de archivo en una ruta de acceso especificada. 

[/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Descarga, carga o elimina un archivo.
 
<a id="ID4E5C"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EAD"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)

   