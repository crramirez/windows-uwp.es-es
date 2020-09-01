---
Description: Use los comentarios visuales para mostrar a los usuarios Cuándo se detectan, interpretan y controlan sus interacciones con una aplicación de Windows.
title: Información visual
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
keywords: información visual,información de foco,información táctil,información de función táctil,visualización de contacto,entrada,interacción
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9ced83ca771f4954f8e42dc42e0882d1a5b7c6b1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172459"
---
# <a name="guidelines-for-visual-feedback"></a>Directrices para información visual

Usa la información visual para mostrar a los usuarios cuándo se detectan, se interpretan y se controlan sus interacciones. La información visual puede ayudar a los usuarios al promover la interacción. Indica si una interacción se ha realizado correctamente, lo que mejora la sensación de control del usuario. También transmite los estados del sistema y reduce los errores.

> **API importantes**:  [**Windows. Devices. Input**](/uwp/api/Windows.Devices.Input), [**Windows. UI. Input**](/uwp/api/Windows.UI.Input), [**Windows. UI. Core**](/uwp/api/Windows.UI.Core)

## <a name="recommendations"></a>Recomendaciones

- Intente limitar el Modifcations de una plantilla de control a los que estén directamente relacionados con su intención de diseño, ya que los cambios extensos pueden afectar al rendimiento y la accesibilidad del control y de la aplicación. 
    - Vea [estilos XAML](../controls-and-patterns/xaml-styles.md) para obtener más información sobre cómo personalizar las propiedades de un control, incluidas las propiedades de estado visual.
    - Vea la [clase UserControl](/uwp/api/windows.ui.xaml.controls.usercontrol) para obtener más información sobre cómo realizar cambios en una plantilla de control.
    - Considere la posibilidad de crear su propio control con plantilla personalizado si necesita realizar cambios significativos en una plantilla de control. Para obtener un ejemplo de un control con plantilla personalizado, vea el [ejemplo de control personalizado de edición](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).
- No uses visualizaciones táctiles en situaciones en que podrían interferir con el uso de la aplicación. Para más información, consulta [**ShowGestureFeedback**](/uwp/api/windows.ui.input.gesturerecognizer.showgesturefeedback).
- No muestres información a menos que sea absolutamente necesario. No muestres información visual a menos que esta sirva para añadir valor que no se encuentra disponible en ningún otro sitio. De este modo, mantendrás la interfaz de usuario ordenada y organizada.
- Intenta no personalizar drásticamente los comportamientos de la información visual de los gestos integrados de Windows, ya que esto podría generar una experiencia del usuario confusa e incoherente.

## <a name="additional-usage-guidance"></a>Instrucciones de uso adicionales

Las visualizaciones de contacto son especialmente importantes para las interacciones táctiles que requieren exactitud y precisión. Por ejemplo, tu aplicación debe indicar de manera clara la ubicación de una pulsación para que el usuario sepa si se equivocó al pulsar el objetivo, por cuánto se equivocó y qué ajustes debe realizar.

Al usar los controles de plataforma XAML disponibles, te aseguras de que la aplicación funcione correctamente en todos los dispositivos y en todas las situaciones de entrada. Si tu aplicación presenta interacciones personalizadas que necesitan de información personalizada, debes asegurarte de que esta información sea adecuada, abarque los dispositivos de entrada y no distraiga al usuario de su tarea. Esto es especialmente importante en las aplicaciones de dibujo o de juegos, donde la información visual podría entrar en conflicto con áreas críticas de la interfaz de usuario u ocultarlas.

> [!Important]
> Te recomendamos que no cambies el comportamiento de interacción de los gestos integrados.

**Comentarios a través de los dispositivos**

Los comentarios visuales dependen por lo general del dispositivo de entrada (táctil, panel táctil, mouse, lápiz o pluma, teclado, etc.). Por ejemplo, la información integrada para un mouse por lo general implica mover y cambiar el cursor, mientras que la entrada táctil o de lápiz requiere visualizaciones de contacto, y la navegación y entrada de teclado usa rectángulos de foco y resaltado.

Usa [**ShowGestureFeedback**](/uwp/api/windows.ui.input.gesturerecognizer.showgesturefeedback) para establecer el comportamiento de la información para los gestos de la plataforma.

Si personalizas la interfaz de usuario de información, asegúrate de proporcionar información que sea adecuada y admita todos los modos de entrada.

Estos son algunos ejemplos de visualizaciones de contacto integradas en Windows.

| ![Información táctil](images/TouchFeedback.png) | ![Comentarios del mouse](images/MouseFeedback.png) | ![Comentarios del lápiz](images/PenFeedback.png) | ![Comentarios del teclado](images/KeyboardFeedback.png) |
| --- | --- | --- | --- |
| Visualización táctil | Visualización de mouse y panel táctil | Visualización de pluma | Visualización de teclado |

## <a name="high-visibility-focus-visuals"></a>Elementos visuales de foco de alta visibilidad

Todas las aplicaciones de Windows admiten elementos visuales de foco alrededor de controles interactivos más definidos en la aplicación. Estos nuevos elementos visuales de foco son totalmente personalizables y se pueden deshabilitar cuando sea necesario.

En el caso de la **experiencia de 10 pies** típica del uso de Xbox y TV, Windows admite **revelar el foco**, un efecto de iluminación que anima el borde de los elementos que pueden recibir el foco, como un botón, cuando se centra a través de la entrada del teclado o del controlador de juegos. Para obtener más información, consulte [diseñar para Xbox y TV](../devices/designing-for-tv.md#reveal-focus).

## <a name="color-branding--customizing"></a>Personalización de marca de color y personalización

### <a name="border-properties"></a>Propiedades de borde

Los elementos visuales de enfoque de alta visibilidad constan de dos partes: el borde principal y el borde secundario. El borde principal es de **2px** de espesor y se ejecuta alrededor de la parte de *fuera* del borde secundario. El borde secundario es de **1px** de espesor y se ejecuta alrededor de la parte de *dentro* del borde primario.
![Contornos visuales de foco de alta visibilidad](images/FocusRectRedlines.png)

Para cambiar el grosor del tipo de borde (principal o secundario) usa **FocusVisualPrimaryThickness** o **FocusVisualSecondaryThickness**, respectivamente:
```XAML
<Slider Width="200" FocusVisualPrimaryThickness="5" FocusVisualSecondaryThickness="2"/>
```
![Grosores de los márgenes visuales de foco de alta visibilidad](images/FocusMargin.png)

El margen es una propiedad de tipo [**Grosor**](/dotnet/api/system.windows.thickness)y, por lo tanto, el margen se puede personalizar para que aparezca únicamente en determinadas partes del control. Vea a continuación: el ![ foco visual del margen visual de ancho de la visibilidad solo inferior](images/FocusThicknessSide.png)

El margen es el espacio entre los límites visuales del control y el inicio del *borde secundario*de los objetos visuales de foco. El margen predeterminado se **1px** fuera de los límites del control. Puede editar este margen para cada control, cambiando la propiedad **FocusVisualMargin** :
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

### <a name="color-properties"></a>Propiedades de color

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

### <a name="for-designers"></a>Para diseñadores

- [Directrices sobre movimiento panorámico](guidelines-for-panning.md)

### <a name="for-developers"></a>Para desarrolladores

- [Interacciones del usuario personalizadas](../layout/index.md)

### <a name="samples"></a>Ejemplos

- [Ejemplo de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Ejemplo de entrada de latencia baja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Ejemplo de modo de interacción del usuario](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Ejemplos de archivo

- [Entrada: muestra de eventos de entrada de usuario de XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrada: muestra de funcionalidades del dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrada: muestra de prueba de acceso táctil](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Ejemplo de desplazamiento, panorámica y zoom de XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrada: ejemplo de entrada de lápiz simplificada](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Entrada: muestra de gestos de Windows 8](/samples/browse/?redirectedfrom=MSDN-samples)
- [Entrada: ejemplo de manipulaciones y gestos](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Muestra de entrada táctil de DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
 

 