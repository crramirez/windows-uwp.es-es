---
author: payzer
title: "Cómo deshabilitar el modo de mouse"
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 6f4719c98d490cdcac8c799c4c68af55b217cbc5
ms.openlocfilehash: d1ee946693b9f9714b8d570b8ae3718469d2c10d

---

# Cómo deshabilitar el modo de mouse
El modo de mouse está activado de manera predeterminada para todas las aplicaciones. Esto significa que todas las aplicaciones que no lo hayan deshabilitado voluntariamente recibirán un puntero de mouse (similar al que se muestra en el navegador Edge en la consola). Recomendamos encarecidamente desactivarlo y optimizar para la navegación con el mando de dirección.   
   
## HTML   
Para activar la navegación con el mando de dirección en una aplicación para UWP de JavaScript, usa la biblioteca de JavaScript [navegación direccional TVHelpers](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation). Incluye el archivo de navegación direccional de JavaScript en el paquete de la aplicación y agrega una referencia a él en todas las páginas HTML que requieran navegación con el mando de dirección:
```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
Consulta el [wiki de navegación direccional](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation) para obtener más información.

En cambio, si deseas desactivar el modo de mouse y utilizar las API para el mando DOM o WinRT directamente, ejecuta lo siguiente para todas las páginas que lo requieran: 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

De forma predeterminada, esta propiedad es ```'mouse'```, lo que habilita el modo de mouse. Si se establece en ```'keyboard'```, desactiva el modo de mouse y en su lugar la entrada del mando genera eventos de teclado de DOM. Si se configura en ```'gamepad'```, desactiva el modo de mouse. no genera eventos de teclado de DOM y te permite usar solo las API del mando de DOM o WinRT.

## XAML    
Para desactivar el modo de mouse, agrega lo siguiente al constructor de la aplicación:   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## C++/DirectX   
Si estás escribiendo una aplicación C++/DirectX, no se puede hacer nada. El modo de mouse solo se aplica a las aplicaciones HTML y XAML.



<!--HONumber=Jul16_HO1-->


