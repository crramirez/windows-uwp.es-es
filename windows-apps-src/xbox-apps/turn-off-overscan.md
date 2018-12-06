---
title: Cómo dibujar la interfaz de usuario hasta el borde de la pantalla
description: Cómo desactivar el ajuste de escala automático para la zona segura del título.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
ms.localizationpriority: medium
ms.openlocfilehash: 1ac49d80f1d99a56eff565a0daa8f2f3e9289636
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8744673"
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
