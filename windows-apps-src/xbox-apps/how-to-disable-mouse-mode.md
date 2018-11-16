---
author: payzer
title: Cómo deshabilitar el modo de mouse
description: Instrucciones para deshabilitar el modo de mouse predeterminado.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e57ee4e6-7807-4943-a933-c2b4dc80fc01
ms.localizationpriority: medium
ms.openlocfilehash: 9cc6989103442deb364654ec006c0fe4b1455511
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6839457"
---
# <a name="how-to-disable-mouse-mode"></a>Cómo deshabilitar el modo de mouse
El modo de mouse está activado de manera predeterminada para todas las aplicaciones, lo que significa que todas las aplicaciones que no lo hayan deshabilitado voluntariamente tendrán un puntero de mouse (similar al que se muestra en el navegador Edge en la consola). Te recomendamos encarecidamente que lo desactives y optimices la navegación con el mando de dirección.   
   
## <a name="html"></a>HTML   
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

## <a name="xaml"></a>XAML    
Para desactivar el modo de mouse, agrega lo siguiente al constructor de la aplicación:   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## <a name="cdirectx"></a>C++/DirectX   
Si estás escribiendo una aplicación C++ o DirectX, no tienes que hacer nada. El modo de mouse solo se aplica a las aplicaciones HTML y XAML.

## <a name="see-also"></a>Consulta también
- [Procedimientos recomendados para Xbox](tailoring-for-xbox.md)
- [UWP en Xbox One](index.md)

