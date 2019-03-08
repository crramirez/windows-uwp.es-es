---
title: Configurar el AppXManifest para varios jugadores
description: Aprenda a configurar su AppXManifest de UWP para permitir invitaciones para varios jugadores de Xbox Live.
ms.assetid: 72f179e7-4705-4161-9b8a-4d6a1a05b8f7
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, activación de protocolos, participan varios jugadores
ms.localizationpriority: medium
ms.openlocfilehash: 13b04a86fdc4e4f661dd1c181dda7d9c9e4c1c8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646030"
---
# <a name="configure-your-appxmanifest-for-multiplayer"></a>Configurar el AppXManifest para varios jugadores

Deberá realizar algunas actualizaciones en el archivo .appxmanifest en el proyecto de Visual Studio si se cumplen las condiciones siguientes:
- Está desarrollando una UWP
- Desea implementar la capacidad para reproductores para invitar a otros usuarios en su título

Si no completa este paso, el título no obtendrá protocolo que se activa cuando un reproductor destinatario acepta una invitación para jugar.

## <a name="open-your-packageappxmanifest"></a>Abra el archivo Package.appxmanifest

Normalmente se encuentra el archivo Package.appxmanifest en el mismo directorio que el archivo de solución del proyecto de Visual Studio.  O bien, puede encontrarlo en el Explorador de soluciones.

![](../../images/multiplayer/multiplayer_open_appxmanifest.png)

## <a name="add-new-entry"></a>Agregar nueva entrada

Deberá agregar lo siguiente a la ```<Extensions>``` elemento bajo ```<Applications>``` en el archivo Package.appxmanifest

```
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

Por ejemplo:

![](../../images/multiplayer/multiplayer_appxmanifest_changes.png)

Guardar y volver a generar su título.  Para obtener información sobre cómo usar el Administrador de varios jugadores para implementar la capacidad para invitar a los jugadores en su título, consulte [reproducir varios jugadores con sus amigos](../multiplayer-manager/play-multiplayer-with-friends.md)
