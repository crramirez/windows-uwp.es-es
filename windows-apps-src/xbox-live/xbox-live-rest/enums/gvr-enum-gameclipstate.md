---
title: Enumeración de GameClipState
assetID: 97fe5c1e-f7b5-537e-69eb-8284b69cd3e1
permalink: en-us/docs/xboxlive/rest/gvr-enum-gameclipstate.html
description: " Enumeración de GameClipState"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f7b20224eeab1b98c7c80f0e4b551420b5a15e7d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630720"
---
# <a name="gameclipstate-enumeration"></a>Enumeración de GameClipState
Detalles de la enumeración GameClipState. 
<a id="ID4ET"></a>

 
## <a name="gameclipstate"></a>GameClipState
 
| <b>Enumerator</b>| <b>Descripción</b>| 
| --- | --- | 
| Ninguno | Estado del servicio de clip de juego es desconocido o no configurada.| 
| PendingUpload | Servicio de clip de juego está esperando la carga de activos.| 
| PendingDelete | Clip de juego está en la cola para su eliminación. (Significa es "eliminar").| 
| Procesado | Clip de juego ha terminado todo el procesamiento.| 
| En proceso| Se está procesando el clip de juego (codificación, miniaturas, etcetera).| 
| Publishing| Activos de juego clip se van a publicar.| 
| Published| Se publican los activos de juego clip: este estado indica que está todo preparado para verlo.| 
| Marcados| Clip de juego se ha marcado para la aplicación.| 
| No permitidas| Clip de juego se ha prohibido pero aún no se ha eliminado.| 
| Uploaded| Clip de juego ha completado la carga.| 
| Eliminado| Se ha eliminado el clip de juego.| 
| Error| Clip de juego es un error de estado e inutilizables.| 
  