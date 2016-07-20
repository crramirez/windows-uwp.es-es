---
author: payzer
title: "Cómo desactivar el ajuste de escala"
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 192de32bf3afd11cd375655ad92d194ccb09dae1
ms.openlocfilehash: 307606bc290e9c5268fc5a37b72770d6b1ada4da

---

# Cómo desactivar el ajuste de escala   
De manera predeterminada, las aplicaciones adoptan una escala de 200% para XAML y de 150% para aplicaciones HTML. Es posible desactivar el factor de escala de forma predeterminado. Esto hará que la aplicación use las dimensiones en píxeles reales del dispositivo (1910 por 1080 píxeles).   
   
## HTML   
Si quieres deshabilitar el factor de escala, usa el siguiente fragmento de código: 
   
`var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);` 

O bien, puedes usar un método adaptado para la web:   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## XAML
Si quieres deshabilitar el factor de escala, usa el siguiente fragmento de código:   
   
`bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);`   
   
## DirectX/C++   
Las aplicaciones DirectX/C++ no se escalan. El ajuste de escala automático solo se aplica a las aplicaciones HTML y XAML.   



<!--HONumber=Jul16_HO1-->


