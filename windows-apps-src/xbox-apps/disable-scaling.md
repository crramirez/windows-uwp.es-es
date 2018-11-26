---
title: Cómo desactivar el escalado
description: Instrucciones para desactivar el factor de escala predeterminado.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6e68c1fc-a407-4c0b-b0f4-e445ccb72ff3
ms.localizationpriority: medium
ms.openlocfilehash: 44688ff40792ba2ee72cbd1d96bae1ac59834efa
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7694267"
---
# <a name="how-to-turn-off-scaling"></a>Cómo desactivar el escalado   
De manera predeterminada, las aplicaciones adoptan una escala de 200% para XAML y de 150% para aplicaciones HTML. Es posible desactivar el factor de escala de forma predeterminado. Esto hará que la aplicación use las dimensiones en píxeles reales del dispositivo (1910 x 1080 píxeles).   
   
## <a name="html"></a>HTML   
Si quieres deshabilitar el factor de escala, usa el siguiente fragmento de código: 
   
```
var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);
```

O bien, puedes usar un método adaptado para la web:   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## <a name="xaml"></a>XAML
Si quieres deshabilitar el factor de escala, usa el siguiente fragmento de código:   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## <a name="directxc"></a>DirectX/C++   
Las aplicaciones DirectX/C++ no se escalan. El ajuste de escala automático solo se aplica a las aplicaciones HTML y XAML.  

## <a name="see-also"></a>Consulta también
- [Procedimientos recomendados para Xbox](tailoring-for-xbox.md)
- [UWP en Xbox One](index.md)
