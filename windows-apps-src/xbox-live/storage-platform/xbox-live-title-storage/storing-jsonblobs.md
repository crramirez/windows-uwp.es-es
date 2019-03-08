---
title: Almacena un blob JSON
description: Obtenga información sobre cómo almacenar un blob JSON en el almacenamiento de título de Xbox Live.
ms.assetid: f424aca1-e671-4e31-84c6-098fda4a9558
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, almacenamiento de título
ms.localizationpriority: medium
ms.openlocfilehash: dabcd0634bb20bc6b82ae302c77338b5bb726a63
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595070"
---
# <a name="storing-a-json-blob-in-xbox-live-title-storage"></a>Almacena un blob JSON en el almacenamiento de título de Xbox Live

1.  Enviar una solicitud mediante el *colocar* método para enviar los datos al almacenamiento de título.

        PUT https://titlestorage.xboxlive.com/json/users/xuid(1245111)/scids/{scid}/data/{pathAndFileName},json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 240
        Connection: Keep-Alive



-   El usuario debe estar en la sesión para actualizarlo.

-   STSTokenString es un marcador de posición para mayor brevedad y debe reemplazarse por el token devuelto por la solicitud de autenticación.

2.  Enviar un objeto JSON.

        {
            "startlevel":"1",
            "expression":"smile"
        }

#### <a name="reference"></a>Referencia

**/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)**
