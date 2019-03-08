---
title: Leer un blob JSON
description: Obtenga información sobre cómo leer un blob JSON en el almacenamiento de título de Xbox Live.
ms.assetid: 3697af16-d054-4835-af7f-7fee8c628345
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 30d2b9f6539e73df1c5ea5ae18b3d1a6ca061d60
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613490"
---
# <a name="reading-a-json-blob-in-xbox-live-title-storage"></a>Leer un blob JSON en el almacenamiento de título de Xbox Live

1.  Enviar una solicitud mediante el *obtener* método para leer los datos del almacenamiento de título. Este ejemplo usa almacenamiento global del título.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/surprise.json,json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive

-   El usuario debe estar en la sesión para actualizarlo.

-   STSTokenString es un marcador de posición para mayor brevedad y debe reemplazarse por el token devuelto por la solicitud de autenticación.

#### <a name="reference"></a>Referencia

**/global/scids/{scid}/data/{pathAndFileName},{type}**
