---
author: Karl-Bridge-Microsoft
Description: "Usa la información visual para mostrar a los usuarios cuándo se detectan, se interpretan y se controlan sus interacciones con una aplicación de la Tienda Windows."
title: "Información visual"
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
keywords: "información visual,información de foco,información táctil,información de función táctil,visualización de contacto,entrada,interacción"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 422d396e9810d6ebe083a6346c8ec8f032ef1fb3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="guidelines-for-visual-feedback"></a>Directrices para información visual
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Usa la información visual para mostrar a los usuarios cuándo se detectan, se interpretan y se controlan sus interacciones. La información visual puede ayudar a los usuarios al promover la interacción. Indica si una interacción se ha realizado correctamente, lo que mejora la sensación de control del usuario. También transmite los estados del sistema y reduce los errores.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)</li>
<li>[**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)</li>
<li>[**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)</li>
</ul>
</div>

## <a name="recommendations"></a>Recomendaciones

-   Intenta permanecer lo más cerca posible de la plantilla de control original, para obtener un control óptimo y rendimiento de la aplicación.
-   No uses visualizaciones táctiles en situaciones en que podrían interferir con el uso de la aplicación. Para más información, consulta [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969).
-   No muestres información a menos que sea absolutamente necesario. No muestres información visual a menos que esta sirva para añadir valor que no se encuentra disponible en ningún otro sitio. De este modo, mantendrás la interfaz de usuario ordenada y organizada.
-   Intenta no personalizar drásticamente los comportamientos de la información visual de los gestos integrados de Windows, ya que esto podría generar una experiencia del usuario confusa e incoherente.

## <a name="additional-usage-guidance"></a>Instrucciones de uso adicionales

Las visualizaciones de contacto son especialmente importantes para las interacciones táctiles que requieren exactitud y precisión. Por ejemplo, tu aplicación debe indicar de manera clara la ubicación de una pulsación para que el usuario sepa si se equivocó al pulsar el objetivo, por cuánto se equivocó y qué ajustes debe realizar.

Al usar los controles de plataforma XAML disponibles, te aseguras de que la aplicación funcione correctamente en todos los dispositivos y en todas las situaciones de entrada. Si tu aplicación presenta interacciones personalizadas que necesitan de información personalizada, debes asegurarte de que esta información sea adecuada, abarque los dispositivos de entrada y no distraiga al usuario de su tarea. Esto es especialmente importante en las aplicaciones de dibujo o de juegos, donde la información visual podría entrar en conflicto con áreas críticas de la interfaz de usuario u ocultarlas.

[!IMPORTANT] Te recomendamos que no cambies el comportamiento de interacción de los gestos integrados. 

**Comentarios a través de los dispositivos**

Los comentarios visuales dependen por lo general del dispositivo de entrada (táctil, panel táctil, mouse, lápiz o pluma, teclado, etc.). Por ejemplo, la información integrada para un mouse por lo general implica mover y cambiar el cursor, mientras que la entrada táctil o de lápiz requiere visualizaciones de contacto, y la navegación y entrada de teclado usa rectángulos de foco y resaltado.

Usa [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969) para establecer el comportamiento de la información para los gestos de la plataforma.

Si personalizas la interfaz de usuario de información, asegúrate de proporcionar información que sea adecuada y admita todos los modos de entrada.

Estos son algunos ejemplos de visualizaciones de contacto integradas en Windows.

| ![Información táctil](images/TouchFeedback.png) | ![Comentarios del mouse](images/MouseFeedback.png) | ![Comentarios del lápiz](images/PenFeedback.png) | ![Comentarios del teclado](images/KeyboardFeedback.png) |
| --- | --- | --- | --- |
| Visualización táctil | Visualización de mouse y panel táctil | Visualización de pluma | Visualización de teclado |

## <a name="high-visibility-focus-visuals"></a>Elementos visuales de foco de alta visibilidad

Todas las aplicaciones de Windows admiten elementos visuales de foco alrededor de controles interactivos más definidos en la aplicación. Estos nuevos elementos visuales de foco son totalmente personalizables y se pueden deshabilitar cuando sea necesario.

## <a name="color-branding--customizing"></a>Personalización de marca de color y personalización

**Propiedades de borde**

Los elementos visuales de enfoque de alta visibilidad constan de dos partes: el borde principal y el borde secundario. El borde principal es de **2px** de espesor y se ejecuta alrededor de la parte de *fuera* del borde secundario. El borde secundario es de **1px** de espesor y se ejecuta alrededor de la parte de *dentro* del borde primario.
![Contornos visuales de foco de alta visibilidad](images/FocusRectRedlines.png)

Para cambiar el grosor del tipo de borde (principal o secundario) usa **FocusVisualPrimaryThickness** o **FocusVisualSecondaryThickness**, respectivamente:
```XAML
<Slider Width="200" FocusVisualPrimaryThickness="5" FocusVisualSecondaryThickness="2"/>
```
![Grosores de los márgenes visuales de foco de alta visibilidad](images/FocusMargin.png)

El margen es una propiedad de tipo [**Grosor**](https://msdn.microsoft.com/library/system.windows.thickness)y, por lo tanto, el margen se puede personalizar para que aparezca únicamente en determinadas partes del control. Consulta la siguiente información: ![Grosor de los márgenes visuales de foco de alta visibilidad, solo de la parte inferior](images/FocusThicknessSide.png)

El margen es el espacio entre los límites de los elementos visuales de control y el inicio del *borde secundario* de los elementos visuales de foco. El margen predeterminado está **1px** por encima de los límites del control. Puedes editar este margen según el control, cambiando la propiedad **FocusVisualMargin**:
```XAML
<Slider Width="200" FocusVisualMargin="-5"/>
```
![Diferencias en los márgenes visuales de foco de alta visibilidad](images/FocusPlusMinusMargin.png)

*Un margen negativo alejará el borde del centro del control y un margen positivo acercará el borde hacia el centro del control.*

Para desactivar elementos visuales de foco en el control por completo, simplemente se deshabilitó **UseSystemFocusVisuals**:
```XAML
<Slider Width="200" UseSystemFocusVisuals="False"/>
```

El grosor, el margen o si el desarrollador de aplicaciones desea o no tener los elementos visuales de foco es determinante al regirse por el control.

**Propiedades de color**

Hay solo dos propiedades de color para los elementos visuales de foco: el color del borde principal y el color del borde secundario. Estos colores de borde de los elementos visuales de foco pueden cambiar según el control en el nivel de página y de forma global en el nivel de aplicación:

Para marcar los elementos visuales de foco en toda la aplicación, reemplaza los pinceles del sistema:
```XAML
<SolidColorBrush x:Key="SystemControlFocusVisualPrimaryBrush" Color="DarkRed"/>
<SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="Pink"/>
```
![Cambios de color en los elementos visuales de foco de alta visibilidad](images/FocusRectColorChanges.png)

Para cambiar los colores según el control, modifica las propiedades de los elementos visuales de foco en el control deseado:
```XAML
<Slider Width="200" FocusVisualPrimaryBrush="DarkRed" FocusVisualSecondaryBrush="Pink"/>
```

## <a name="related-articles"></a>Artículos relacionados

**Para diseñadores**
* [Directrices sobre el movimiento panorámico](guidelines-for-panning.md)

**Para desarrolladores**
* [Interacciones del usuario personalizadas](https://msdn.microsoft.com/library/windows/apps/mt185599)

**Muestras**
* [Ejemplo de entrada básica](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Muestra de entrada de latencia baja](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Muestra de modo de interacción del usuario](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Muestra de elementos visuales de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Muestras de archivo**
* [Entrada: muestra de eventos de entrada de usuario de XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: muestra de funcionalidades del dispositivo](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: muestra de prueba de acceso táctil](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Muestra de desplazamiento, movimiento panorámico y zoom XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: ejemplo de entrada de lápiz simplificada](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrada: muestra de gestos de Windows 8](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrada: muestra de manipulaciones y gestos (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Muestra de entrada táctil de DirectX](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 
