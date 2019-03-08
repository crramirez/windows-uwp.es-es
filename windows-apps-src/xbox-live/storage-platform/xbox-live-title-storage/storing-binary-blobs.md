---
title: Almacenar un blob binario
description: Obtenga información sobre cómo almacenar un blob binario en el almacenamiento de título de Xbox Live.
ms.assetid: a0da36ef-5a5a-466d-80a8-e055ba7e4cdc
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, almacenamiento de título
ms.localizationpriority: medium
ms.openlocfilehash: 70e620f2e992dfdd240f71b5c73165f13d4ef5cb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660100"
---
# <a name="storing-a-binary-blob-in-xbox-live-title-storage"></a>Almacenar un blob binario en el almacenamiento de título de Xbox Live

1.  Enviar una solicitud mediante el método para enviar los datos al almacenamiento de título.

        PUT https://titlestorage.xboxlive.com/trustedplatform/users/xuid(1245111)/scids/{scid}/data/lastturn.bin,binary              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 40
        Connection: Keep-Alive


-   El usuario debe estar en la sesión para actualizarlo.

-   STSTokenString es un marcador de posición para mayor brevedad y debe reemplazarse por el token devuelto por la solicitud de autenticación.

2.  Enviar los datos binarios. Puesto que los datos se transferirán a través de HTTP, se deben restringir los datos para el juego de caracteres aceptables. Información como datos de audio o de imagen debe estar codificado. Puede seleccionar cualquier método de codificación que genera caracteres compatibles de HTTP.
d
```
  01EAEFBAD05903A4
  1EA2311656677DFF
  CF00
```

#### <a name="reference"></a>Referencia

**/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}**
