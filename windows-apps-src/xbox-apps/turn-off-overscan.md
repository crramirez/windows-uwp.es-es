---
author: payzer
title: "Cómo dibujar la interfaz de usuario hasta el borde de la pantalla"
description: "Cómo desactivar el ajuste de escala automático para la zona segura del título."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
ms.openlocfilehash: 30fc3e357eaea0d36a5deba1b0ea85c2d9bc990e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: HT
ms.contentlocale: es-ES
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
