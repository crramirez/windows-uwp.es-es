---
title: Sombras de composición
description: La API de sombra te permite agregar sombras personalizables dinámicas a contenido de la interfaz de usuario.
ms.date: 07/16/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9541ea1c00d473bc4881a80d8597625592e278f9
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "8854876"
---
# <a name="shadows-in-windows-ui"></a>Sombras en la interfaz de usuario de Windows

La clase [DropShadow](/uwp/api/Windows.UI.Composition.DropShadow) proporciona medio de creación de una sombra configurable que se puede aplicar a un [objeto SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) o [LayerVisual](/uwp/api/windows.ui.composition.layervisual) (subárbol de elementos visuales). Como es habitual para los objetos de la capa Visual, todas las propiedades de la sombra se pueden animar con CompositionAnimations.

## <a name="basic-drop-shadow"></a>Sombra paralela básica

Para crear una sombra básica, simplemente crea un nuevo DropShadow y asociarlo a tu visual. La sombra es rectangular de manera predeterminada. Un conjunto estándar de propiedades están disponibles para ajustar la apariencia de la sombra.

```cs
var basicRectVisual = _compositor.CreateSpriteVisual();
basicRectVisual.Brush = _compositor.CreateColorBrush(Colors.Blue);
basicRectVisual.Offset = new Vector3(100, 100, 20);
basicRectVisual.Size = new Vector2(300, 300);

var basicShadow = _compositor.CreateDropShadow();
basicShadow.BlurRadius = 25f;
basicShadow.Offset = new Vector3(20, 20, 20);

basicRectVisual.Shadow = basicShadow;
```

![Rectangular Visual con DropShadow básica](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>Definir la sombra

Hay varias maneras para definir la forma de tu DropShadow:

- **Usa el valor predeterminado** , de manera predeterminada, la forma de DropShadow se define con el modo de 'Predeterminado' en CompositionDropShadowSourcePolicy. SpriteVisual, el valor predeterminado es Rectangular, a menos que se proporciona una máscara. Para LayerVisual, el valor predeterminado es hereden una máscara mediante el valor alfa del pincel del elemento visual.
- **Establecer una máscara** : se puede establecer la [máscara de](/uwp/api/windows.ui.composition.dropshadow.mask) propiedad para definir una máscara de opacidad de la sombra.
- **Especificar usar máscara heredado** : establece la propiedad de [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) usar [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy). InheritFromVisualContent use la máscara que se generó desde el valor alfa del pincel del elemento visual.

## <a name="masking-to-match-your-content"></a>Enmascarar para que coincida con el contenido

Si quieres que la sombra para que coincida con el contenido del elemento Visual puede usar el pincel del elemento Visual para que la propiedad de máscara de sombras o establecer la sombra hereden automáticamente máscara desde el contenido. Si usas un LayerVisual, la sombra heredarán la máscara de manera predeterminada.

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = imageBrush;
// or use shadow.SourcePolicy = CompositionDropShadowSourcePolicy.InheritFromVisualContent;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![Imagen de web conectada con sombra paralela enmascarada](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>Usar una máscara alternativa.

En algunos casos, es posible que desee la sombra de forma que no coincide con el contenido del elemento de Visual. Para lograr este efecto, debes establecer explícitamente la propiedad de la máscara con un pincel alfa.

En el ejemplo siguiente, cargamos dos superficies: uno para el contenido Visual y otro para la máscara de sombra:

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var circleSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myCircleImage.png"));
var customMask = _compositor.CreateSurfaceBrush(circleSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = customMask;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![Imagen de web conectada con un círculo enmascarada por la sombra paralela](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>Animación

Dado que es un estándar de la capa Visual, DropShadow propiedades se pueden animar con animaciones de composición. A continuación, se modifica el código de la muestra de fragmentos anterior para animar el radio de desenfoque de la sombra.

```cs
ScalarKeyFrameAnimation blurAnimation = _compositor.CreateScalarKeyFrameAnimation();
blurAnimation.InsertKeyFrame(0.0f, 25.0f);
blurAnimation.InsertKeyFrame(0.7f, 50.0f);
blurAnimation.InsertKeyFrame(1.0f, 25.0f);
blurAnimation.Duration = TimeSpan.FromSeconds(4);
blurAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
shadow.StartAnimation("BlurRadius", blurAnimation);
```

## <a name="shadows-in-xaml"></a>Sombras en XAML

Si quieres agregar una sombra a los elementos de marco más complejos, hay un par de formas para interoperar con sombras entre XAML y la composición:

1. Usa el [DropShadowPanel](https://github.com/Microsoft/UWPCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) disponible en el Kit de herramientas de comunidad Windows. Consulta la [documentación de DropShadowPanel](https://docs.microsoft.com/windows/uwpcommunitytoolkit/controls/DropShadowPanel) para obtener más información sobre cómo usarla.
1. Crea un elemento Visual para usar como el host de sombras y vincular a la entrega XAML Visual.
1. Usar el control de CompositionShadow personalizado de la Galería de composición muestra [SamplesCommon](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SamplesCommon/SamplesCommon) . Vea el ejemplo aquí para el uso.

## <a name="performance"></a>Rendimiento

Aunque la capa Visual tiene muchas optimizaciones en su lugar para realizar efectos eficaz y utilizable, la generación de sombras puede ser una operación relativamente costosa dependiendo de qué opciones establecidas. A continuación, encontrarás alto nivel 'costos' para diferentes tipos de sombras. Ten en cuenta que aunque determinadas sombras pueden ser caras, puede adecuados para usar con moderación en determinados escenarios.

Características de sombras| Coste
------------- | -------------
Rectangular    | Low
Shadow.Mask      | Alto
CompositionDropShadowSourcePolicy.InheritFromVisualContent | Alto
Radio de desenfoque estático | Low
Animación de desenfoque Radius | Alto

## <a name="additional-resources"></a>Recursos adicionales

- [DropShadow API de composición](/uwp/api/Windows.UI.Composition.DropShadow)
- [Repositorio de GitHub de WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs)