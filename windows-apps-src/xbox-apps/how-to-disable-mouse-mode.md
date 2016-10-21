---
author: payzer
title: "Cómo deshabilitar el modo de mouse"
description: Instrucciones para deshabilitar el modo de mouse predeterminado.
translationtype: Human Translation
ms.sourcegitcommit: b4df1f944d909640791e4ed7e3bcf8d8bdf7a0d1
ms.openlocfilehash: 91e530a3313d53c4e693b88a64b849f3188a72de

---

# Cómo deshabilitar el modo de mouse
El modo de mouse está activado de manera predeterminada para todas las aplicaciones, lo que significa que todas las aplicaciones que no lo hayan deshabilitado voluntariamente tendrán un puntero de mouse (similar al que se muestra en el navegador Edge en la consola). Te recomendamos encarecidamente que lo desactives y optimices la navegación con el mando de dirección.   
   
## HTML   
Para activar la navegación con el mando de dirección en una aplicación para la Plataforma universal de Windows (UWP) de JavaScript, usa la biblioteca de JavaScript [navegación direccional TVHelpers](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation). Incluye el archivo de navegación direccional de JavaScript en el paquete de la aplicación y agrega una referencia a él en todas las páginas HTML que requieran navegación con el mando de dirección:

```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
Consulta la [wiki de navegación direccional](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation) para obtener más información.

En cambio, si quieres desactivar el modo de mouse y utilizar las API para el mando DOM o WinRT directamente, ejecuta lo siguiente para todas las páginas que lo requieran: 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

   De forma predeterminada, esta propiedad es `mouse`, lo que habilita el modo de mouse. Si se establece en `keyboard`, desactiva el modo de mouse y, en su lugar, la entrada del mando genera eventos de teclado de DOM. Si se configura en `gamepad`, desactiva el modo de mouse, no genera eventos de teclado de DOM y te permite usar solo las API del mando de DOM o WinRT.

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
Si estás escribiendo una aplicación C++ o DirectX, no tienes que hacer nada. El modo de mouse solo se aplica a las aplicaciones HTML y XAML.

## Consulta también
- [Procedimientos recomendados para Xbox](tailoring-for-xbox.md)
- [UWP en Xbox One](index.md)




<!--HONumber=Aug16_HO3-->


