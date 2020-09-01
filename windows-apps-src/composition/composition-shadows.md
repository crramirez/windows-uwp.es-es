---
title: Sombras de composición
description: Las API de Shadow permiten agregar sombras dinámicas personalizables al contenido de la interfaz de usuario.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 29ca04ac3faf3a2884bcc2346177f49222cf786e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166429"
---
# <a name="shadows-in-windows-ui"></a>Sombras en la interfaz de usuario de Windows

La clase de modo [de sombra permite](/uwp/api/Windows.UI.Composition.DropShadow) crear una sombra configurable que se puede aplicar a un método [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) o [LayerVisual](/uwp/api/windows.ui.composition.layervisual) (subárbol de objetos visuales). Como es el caso de los objetos de la capa visual, todas las propiedades de la sombra se pueden animar mediante CompositionAnimations.

## <a name="basic-drop-shadow"></a>Sombra paralela básica

Para crear una sombra básica, basta con crear una nueva sombra y asociarla a su visual. La sombra es rectangular de forma predeterminada. Hay disponible un conjunto estándar de propiedades para ajustar la apariencia y el funcionamiento de la sombra.

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

![Objetos visuales rectangulares con la sombra básica](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>Dar forma a la sombra

Hay varias maneras de definir la forma de la sombra:

- **Usar el valor predeterminado** : de forma predeterminada, la forma de la sombra está definida por el modo ' default ' en CompositionDropShadowSourcePolicy. Para SpriteVisual, el valor predeterminado es rectangular a menos que se proporcione una máscara. En el caso de LayerVisual, el valor predeterminado es heredar una máscara mediante el alfa del pincel del visual.
- **Establecer una máscara** : puede establecer la propiedad [máscara](/uwp/api/windows.ui.composition.dropshadow.mask) para definir una máscara de opacidad para la sombra.
- **Especifique el uso de la máscara heredada** : establezca la propiedad [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) para usar [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy). InheritFromVisualContent para usar la máscara generada a partir del alfa del pincel del visual.

## <a name="masking-to-match-your-content"></a>Enmascaramiento para que coincida con su contenido

Si desea que la sombra coincida con el contenido del visual, puede usar el pincel del control visual para la propiedad de máscara de sombra o establecer la sombra para que herede automáticamente la máscara del contenido. Si se usa un LayerVisual, la sombra heredará la máscara de forma predeterminada.

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

![Imagen web conectada con sombra paralela enmascarada](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>Usar una máscara alternativa

En algunos casos, puede que desee dar forma a la sombra para que no coincida con el contenido de su visual. Para lograr este efecto, debe establecer explícitamente la propiedad Mask mediante un pincel con alfa.

En el ejemplo siguiente, se cargan dos superficies: una para el contenido visual y otra para la máscara de sombra:

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

![Imagen web conectada con sombra paralela con máscara de círculo](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>Anima

Como es estándar en la capa visual, las propiedades de sombra se pueden animar mediante animaciones de composición. A continuación, se modifica el código del ejemplo de rocias anterior para animar el radio de desenfoque de la sombra.

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

Si desea agregar una sombra a elementos de marco más complejos, hay un par de formas de interoperar con las sombras entre XAML y la composición:

1. Use el [DropShadowPanel](https://github.com/windows-toolkit/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) disponible en el kit de herramientas de la comunidad de Windows. Consulte la [documentación de DropShadowPanel](/windows/uwpcommunitytoolkit/controls/DropShadowPanel) para obtener más información sobre cómo usarlo.
1. Cree un visual para usarlo como el host de instantánea & enlazarlo al visual de documentos XAML.
1. Use el control CompositionShadow personalizado [SamplesCommon](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SamplesCommon/SamplesCommon) de la galería de ejemplos de composición. Vea el ejemplo aquí para ver el uso.

## <a name="performance"></a>Rendimiento

Aunque la capa visual tiene muchas optimizaciones para que los efectos sean eficientes y utilizables, la generación de sombras puede ser una operación relativamente costosa en función de las opciones que establezca. A continuación se muestran los "costos" de alto nivel para los diferentes tipos de sombras. Tenga en cuenta que, aunque algunas sombras pueden ser costosas, pueden seguir siendo adecuadas para usarse con moderación en ciertos escenarios.

Características de la sombra| Coste
------------- | -------------
Rectangular    | Bajo
Shadow. Mask      | Alto
CompositionDropShadowSourcePolicy.InheritFromVisualContent | Alto
Radio de desenfoque estático | Bajo
Animar el radio de desenfoque | Alto

## <a name="additional-resources"></a>Recursos adicionales

- [API de composición de la sombra](/uwp/api/Windows.UI.Composition.DropShadow)
- [Repositorio de GitHub de WindowsUIDevLabs](https://github.com/microsoft/WindowsCompositionSamples)