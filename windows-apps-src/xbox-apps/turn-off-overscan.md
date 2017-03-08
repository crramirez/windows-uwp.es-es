---
author: payzer
title: "Cómo dibujar la interfaz de usuario hasta el borde de la pantalla"
description: 
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 9a221672391dfbfb4af664438448307800020c6f
ms.lasthandoff: 02/08/2017

---

# <a name="how-to-draw-ui-to-the-edge-of-the-screen"></a>Cómo dibujar la interfaz de usuario hasta el borde de la pantalla   
De manera predeterminada, las aplicaciones tendrán márgenes situados en los bordes del área de visualización, para que así puedas tener en cuenta la zona segura del televisor (para obtener más información, consulta [Diseño para Xbox y televisión](../input-and-devices/designing-for-tv.md#tv-safe-area)). 

Te recomendamos desactivar esta función y dibujar hasta el borde de la pantalla. Puedes dibujar hasta el borde de la pantalla si agregas el siguiente código al iniciar la aplicación:
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> Si la aplicación es de tipo C++/DirectX, no tienes que preocuparte de hacer esto. El sistema siempre representará la aplicación hasta el borde de la pantalla.

## <a name="see-also"></a>Consulta también
- [Procedimientos recomendados para Xbox](tailoring-for-xbox.md)
- [UWP en Xbox One](index.md)

