---
author: payzer
title: "Cómo deshabilitar el modo de mouse"
description: 
area: Xbox
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: bd7780f105f86d7d292c5db2535ad40af07d6e4f

---

# Cómo deshabilitar el modo de mouse
El modo de mouse está activado de manera predeterminada para todas las aplicaciones. Esto significa que todas las aplicaciones que no lo hayan deshabilitado voluntariamente recibirán un puntero de mouse (similar al que se muestra en el navegador Edge en la consola). Recomendamos encarecidamente desactivarlo y optimizar para la navegación con el mando de dirección.   
   
## HTML   
Para desactivar el modo de mouse, agrega lo siguiente a un archivo de JavaScript de la aplicación:   
   
```code
navigator.gamepadInputEmulation = "keyboard";
```   

## XAML    
Para desactivar el modo de mouse, agrega lo siguiente a un archivo de JavaScript de la aplicación:   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
        }
```

## C++/DirectX   
Si estás escribiendo una aplicación C++/DirectX, no se puede hacer nada. El modo de mouse solo se aplica a las aplicaciones HTML y XAML.



<!--HONumber=Jun16_HO5-->


