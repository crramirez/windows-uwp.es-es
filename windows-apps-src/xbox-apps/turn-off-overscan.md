---
title: Cómo dibujar la interfaz de usuario hasta el borde de la pantalla
description: Obtenga información sobre cómo desactivar los bordes predeterminados situados en los bordes de la ventanilla y dibujar la interfaz de usuario en los bordes de la pantalla.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
ms.localizationpriority: medium
ms.openlocfilehash: d34da1bbf129358f4549b3a4a04a7c3f84f872ab
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154849"
---
# <a name="how-to-draw-ui-to-the-edge-of-the-screen"></a>Cómo dibujar la interfaz de usuario hasta el borde de la pantalla   
De manera predeterminada, las aplicaciones tendrán márgenes situados en los bordes del área de visualización, para que así puedas tener en cuenta la zona segura del televisor (para obtener más información, consulta [Diseño para Xbox y televisión](../design/devices/designing-for-tv.md#tv-safe-area)). 

Te recomendamos desactivar esta función y dibujar hasta el borde de la pantalla. Puedes dibujar hasta el borde de la pantalla si agregas el siguiente código al iniciar la aplicación:
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> Si la aplicación es de tipo C++/DirectX, no tienes que preocuparte de hacer esto. El sistema siempre representará la aplicación hasta el borde de la pantalla.

## <a name="see-also"></a>Vea también
- [Procedimientos recomendados para Xbox](tailoring-for-xbox.md)
- [UWP en Xbox One](index.md)
