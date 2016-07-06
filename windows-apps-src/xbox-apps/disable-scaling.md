---
author: payzer
title: "Cómo desactivar el ajuste de escala"
description: 
area: Xbox
ms.sourcegitcommit: 2fcccb9a045aad268afde615d31f8faa002b8a87
ms.openlocfilehash: 65416dd2b6c8656078b63c316f3972cda9c792fc

---

# Cómo desactivar el ajuste de escala   
De manera predeterminada, las aplicaciones adoptan una escala de 200 % para XAML y de 150 % para aplicaciones HTML. Es posible desactivar el factor de escala de forma predeterminado. Esto hará que la aplicación use las dimensiones en píxeles reales del dispositivo (1910 por 1080 píxeles).   
   
## HTML   
Si quieres deshabilitar el factor de escala, usa el siguiente fragmento de código: 
   
`bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);` 

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



<!--HONumber=Jun16_HO5-->


