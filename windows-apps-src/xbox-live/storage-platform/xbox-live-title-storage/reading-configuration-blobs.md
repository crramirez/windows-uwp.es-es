---
title: Leer un blob de configuración
description: Obtenga información sobre cómo leer un blob de configuración de almacenamiento de título de Xbox Live.
ms.assetid: ee62d221-69b9-4f52-9b5d-5a44d04de548
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 71607f05f07ee4a98f73a19c28c85dc325041a10
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599400"
---
# <a name="reading-a-configuration-blob-in-xbox-live-title-storage"></a>Leer un blob de configuración en el almacenamiento de título de Xbox Live

*Los blobs de configuración* son archivos en almacenamiento de título de Xbox Live que contienen datos del juego. Los datos son objetos JSON que incluyen los nodos virtuales que se pueden filtrar antes de entregarse al juego. Para obtener más información sobre los blobs de configuración, consulte **título almacenamiento URI**.

### <a name="to-read-a-configuration-blob"></a>Para leer un blob de configuración

1.  Enviar una solicitud mediante el método para leer los datos del almacenamiento de título.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/config.json,config              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive


-   El usuario debe estar en la sesión para actualizarlo.
-   STSTokenString es un marcador de posición para mayor brevedad y debe reemplazarse por el token devuelto por la solicitud de autenticación.

#### <a name="reference"></a>Referencia

**/global/scids/{scid}/data/{pathAndFileName},{type}**
**GET (/global/scids/{scid}/data/{pathAndFileName},{type})**
