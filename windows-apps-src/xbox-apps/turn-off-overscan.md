---
author: payzer
title: Cómo dibujar la interfaz de usuario hasta el borde de la pantalla
description: Cómo desactivar el ajuste de escala automático para la zona segura del título.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
ms.localizationpriority: medium
ms.openlocfilehash: 32932845db47ed47b7e80f68cf4e424ba97e85c0
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5763919"
---
# <a name="how-to-draw-ui-to-the-edge-of-the-screen"></a>Cómo dibujar la interfaz de usuario hasta el borde de la pantalla   
De manera predeterminada, las aplicaciones tendrán márgenes situados en los bordes del área de visualización, para que así puedas tener en cuenta la zona segura del televisor (para obtener más información, consulta [Diseño para Xbox y televisión](../design/devices/designing-for-tv.md#tv-safe-area)). 

Te recomendamos desactivar esta función y dibujar hasta el borde de la pantalla. Puedes dibujar hasta el borde de la pantalla si agregas el siguiente código al iniciar la aplicación:
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> Si la aplicación es de tipo C++/DirectX, no tienes que preocuparte de hacer esto. El sistema siempre representará la aplicación hasta el borde de la pantalla.

## <a name="see-also"></a>Consulta también
- [Procedimientos recomendados para Xbox](tailoring-for-xbox.md)
- [UWP en Xbox One](index.md)
