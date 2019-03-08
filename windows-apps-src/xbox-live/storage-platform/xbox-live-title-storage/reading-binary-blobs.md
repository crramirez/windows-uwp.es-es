---
title: Leer un blob binario
description: Obtenga información acerca de cómo leer un blob binario en el almacenamiento de título de Xbox Live.
ms.assetid: 9b8e0c35-0cea-4491-bf30-22fad224f11b
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, almacenamiento de título
ms.localizationpriority: medium
ms.openlocfilehash: e2a0be72142a3e11c7c680cfc998287f396c2afc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641660"
---
# <a name="reading-a-binary-blob-in-xbox-live-title-storage"></a>Leer un blob binario en el almacenamiento de título de Xbox Live

1.  Enviar una solicitud mediante el *obtener* método para leer los datos del almacenamiento de título. Este ejemplo usa almacenamiento global del título.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/userinfo.bin,binary
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive



-   El usuario debe estar en la sesión para actualizarlo.

-   STSTokenString es un marcador de posición para mayor brevedad y debe reemplazarse por el token devuelto por la solicitud de autenticación.

#### <a name="reference"></a>Referencia

**/global/scids/{scid}/data/{pathAndFileName},{type}**
