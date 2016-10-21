---
author: payzer
title: "Cómo desactivar el escalado"
description: Instrucciones para desactivar el factor de escala predeterminado.
translationtype: Human Translation
ms.sourcegitcommit: 582f5677c15f7cd62c398103b48743ba4bea6c5b
ms.openlocfilehash: 8079be9685558277565766fa8d0ebbfd4a555904

---

# Cómo desactivar el escalado   
De manera predeterminada, las aplicaciones adoptan una escala de 200% para XAML y de 150% para aplicaciones HTML. Es posible desactivar el factor de escala de forma predeterminado. Esto hará que la aplicación use las dimensiones en píxeles reales del dispositivo (1910 x 1080 píxeles).   
   
## HTML   
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

## XAML
Si quieres deshabilitar el factor de escala, usa el siguiente fragmento de código:   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## DirectX/C++   
Las aplicaciones DirectX/C++ no se escalan. El ajuste de escala automático solo se aplica a las aplicaciones HTML y XAML.  

## Consulta también
- [Procedimientos recomendados para Xbox](tailoring-for-xbox.md)
- [UWP en Xbox One](index.md)



<!--HONumber=Aug16_HO3-->


