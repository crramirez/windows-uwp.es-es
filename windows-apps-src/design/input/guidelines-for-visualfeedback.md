---
Description: Use los comentarios visuales para mostrar a los usuarios Cuándo se detectan, interpretan y controlan sus interacciones con una aplicación de UWP.
title: Información visual
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
keywords: información visual,información de foco,información táctil,información de función táctil,visualización de contacto,entrada,interacción
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bba80403934987569c25b96eced9a610226431b5
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79209981"
---
# <a name="guidelines-for-visual-feedback"></a>Directrices para información visual

Usa la información visual para mostrar a los usuarios cuándo se detectan, se interpretan y se controlan sus interacciones. La información visual puede ayudar a los usuarios al promover la interacción. Indica si una interacción se ha realizado correctamente, lo que mejora la sensación de control del usuario. También transmite los estados del sistema y reduce los errores.

> **API importantes**: [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core)

## <a name="recommendations"></a>Recomendaciones

- Intenta limitar las modifcations de una plantilla de control a las directamente relacionadas con la intención del diseño, dado que muchos cambios pueden afectar al rendimiento y a la accesibilidad tanto del control como de la aplicación. 
    - Consulta [Estilos XAML](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles) para obtener más información sobre cómo personalizar las propiedades de un control, incluidas las propiedades de estado visual.
    - Consulta la [Clase UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) para obtener más información sobre cómo realizar cambios en una plantilla de control
    - Piensa en crear tu propio control con plantilla personalizado si necesitas realizar cambios importantes en una plantilla de control. Para ver un ejemplo de un control con plantilla personalizado, consulta el [ejemplo de control de edición personalizado](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).
- No uses visualizaciones táctiles en situaciones en que podrían interferir con el uso de la aplicación. Para más información, consulta [**ShowGestureFeedback**](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer.showgesturefeedback).
- No muestres información a menos que sea absolutamente necesario. No muestres información visual a menos que esta sirva para añadir valor que no se encuentra disponible en ningún otro sitio. De este modo, mantendrás la interfaz de usuario ordenada y organizada.
- Intenta no personalizar drásticamente los comportamientos de la información visual de los gestos integrados de Windows, ya que esto podría generar una experiencia del usuario confusa e incoherente.

## <a name="additional-usage-guidance"></a>Instrucciones de uso adicionales

Las visualizaciones de contacto son especialmente importantes para las interacciones táctiles que requieren exactitud y precisión. Por ejemplo, tu aplicación debe indicar de manera clara la ubicación de una pulsación para que el usuario sepa si se equivocó al pulsar el objetivo, por cuánto se equivocó y qué ajustes debe realizar.

Al usar los controles de plataforma XAML disponibles, te aseguras de que la aplicación funcione correctamente en todos los dispositivos y en todas las situaciones de entrada. Si tu aplicación presenta interacciones personalizadas que necesitan de información personalizada, debes asegurarte de que esta información sea adecuada, abarque los dispositivos de entrada y no distraiga al usuario de su tarea. Esto es especialmente importante en las aplicaciones de dibujo o de juegos, donde la información visual podría entrar en conflicto con áreas críticas de la interfaz de usuario u ocultarlas.

> [!Important]
> Te recomendamos que no cambies el comportamiento de interacción de los gestos integrados.

**Comentarios entre dispositivos**

Los comentarios visuales dependen por lo general del dispositivo de entrada (táctil, panel táctil, mouse, lápiz o pluma, teclado, etc.). Por ejemplo, la información integrada para un mouse por lo general implica mover y cambiar el cursor, mientras que la entrada táctil o de lápiz requiere visualizaciones de contacto, y la navegación y entrada de teclado usa rectángulos de foco y resaltado.

Usa [**ShowGestureFeedback**](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer.showgesturefeedback) para establecer el comportamiento de la información para los gestos de la plataforma.

Si personalizas la interfaz de usuario de información, asegúrate de proporcionar información que sea adecuada y admita todos los modos de entrada.

Estos son algunos ejemplos de visualizaciones de contacto integradas en Windows.

| ![Información táctil](images/TouchFeedback.png) | ![Comentarios del mouse](images/MouseFeedback.png) | ![Comentarios del lápiz](images/PenFeedback.png) | ![Comentarios del teclado](images/KeyboardFeedback.png) |
| --- | --- | --- | --- |
| Visualización táctil | Visualización de mouse y panel táctil | Visualización de pluma | Visualización de teclado |

## <a name="high-visibility-focus-visuals"></a>Elementos visuales de foco de alta visibilidad

Todas las aplicaciones de Windows admiten elementos visuales de foco alrededor de controles interactivos más definidos en la aplicación. Estos nuevos elementos visuales de foco son totalmente personalizables y se pueden deshabilitar cuando sea necesario.

Para la **experiencia de 10 pies** típica del uso de la Xbox y la televisión, Windows admite **Reveal focus**, un efecto de iluminación que anima el borde de los elementos activables como un botón, cuando se obtiene el foco a través de la entrada del controlador para juegos o del teclado. Para obtener más información, consulta [Diseño para Xbox y televisión](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv#reveal-focus).

## <a name="color-branding--customizing"></a>Personalización de marca de color y personalización

**Propiedades de borde**

Los elementos visuales de enfoque de alta visibilidad constan de dos partes: el borde principal y el borde secundario. El borde principal es de **2px** de espesor y se ejecuta alrededor de la parte de *fuera* del borde secundario. El borde secundario es de **1px** de espesor y se ejecuta alrededor de la parte de *dentro* del borde primario.
![elementos visuales visuales de foco de alta visibilidad](images/FocusRectRedlines.png)

Para cambiar el grosor del tipo de borde (principal o secundario) usa **FocusVisualPrimaryThickness** o **FocusVisualSecondaryThickness**, respectivamente:
```XAML
<Slider Width="200" FocusVisualPrimaryThickness="5" FocusVisualSecondaryThickness="2"/>
```
![Grosores de los márgenes visuales de foco de alta visibilidad](images/FocusMargin.png)

El margen es una propiedad de tipo [**Grosor**](https://docs.microsoft.com/dotnet/api/system.windows.thickness)y, por lo tanto, el margen se puede personalizar para que aparezca únicamente en determinadas partes del control. Vea a continuación: ![enfoque de alta visibilidad el margen visual de grosor del margen solo](images/FocusThicknessSide.png)

El margen es el espacio entre los límites de los elementos visuales de control y el inicio del *borde secundario* de los elementos visuales de foco. El margen predeterminado está **1px** por encima de los límites del control. Puedes editar este margen según el control, cambiando la propiedad **FocusVisualMargin**:
```XAML
<Slider Width="200" FocusVisualMargin="-5"/>
```
![Diferencias en los márgenes visuales de foco de alta visibilidad](images/FocusPlusMinusMargin.png)

*Un margen negativo hará que el borde quede fuera del centro del control y un margen positivo moverá el borde más cerca del centro del control.*

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
* [Directrices sobre movimiento panorámico](guidelines-for-panning.md)

**Para desarrolladores**
* [Interacciones del usuario personalizadas](https://docs.microsoft.com/windows/uwp/design/layout/index)

**Ejemplos**
* [Ejemplo de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Ejemplo de entrada de baja latencia](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [Ejemplo de modo de interacción del usuario](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**Ejemplos de archivo**
* [Entrada: ejemplo de eventos de entrada de usuario de XAML](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [Entrada: ejemplo de funcionalidades del dispositivo](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [Entrada: ejemplo de prueba de posicionamiento táctil](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [Ejemplo de desplazamiento, panorámica y zoom de XAML](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [Entrada: ejemplo de entrada de lápiz simplificada](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [Entrada: ejemplo de gestos de Windows 8](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Entrada: ejemplo de manipulaciones y gestos (C++)](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [Ejemplo de entrada táctil de DirectX](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 
