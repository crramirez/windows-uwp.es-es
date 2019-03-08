---
title: Sombras de composición
description: La sombra de las API le permite agregar sombras personalizables dinámicas al contenido de la interfaz de usuario.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9541ea1c00d473bc4881a80d8597625592e278f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630840"
---
# <a name="shadows-in-windows-ui"></a>Sombras en la interfaz de usuario de Windows

El [DropShadow](/uwp/api/Windows.UI.Composition.DropShadow) clase proporciona el medio para crear una sombra configurable que se puede aplicar a un [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) o [LayerVisual](/uwp/api/windows.ui.composition.layervisual) (subárbol de objetos visuales). Como es habitual para los objetos en la capa Visual, todas las propiedades de la sombra se pueden animar mediante CompositionAnimations.

## <a name="basic-drop-shadow"></a>Sombra paralela básica

Para crear una sombra básica, crear un control DropShadow nuevo y asociarlo al objeto visual. La sombra es rectangular de forma predeterminada. Un conjunto de propiedades estándar están disponibles para ajustar la apariencia y comportamiento de la sombra.

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

![Objeto Visual rectangular con DropShadow básico](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>Dar forma a la sombra

Hay varias maneras de definir la forma de la sombra:

- **Use el valor predeterminado** : de forma predeterminada, la forma de un objeto DropShadow está definida por el modo 'Default' en CompositionDropShadowSourcePolicy. SpriteVisual, el valor predeterminado es Rectangular a menos que se proporciona una máscara. LayerVisual, valor predeterminado es heredar de una máscara con el canal alfa del pincel del objeto visual.
- **Establece una máscara** : puede establecer el [máscara](/uwp/api/windows.ui.composition.dropshadow.mask) propiedad para definir una máscara de opacidad de la sombra.
- **Especifica que se use la máscara de herencia** : establezca el [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) propiedad que se va a usar [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy). InheritFromVisualContent use la máscara que se genera a partir de la versión alfa de pincel del objeto visual.

## <a name="masking-to-match-your-content"></a>Enmascaramiento para que coincida con el contenido

Si desea que la sombra para que coincida con el contenido del objeto Visual puede usar el pincel del objeto Visual para la propiedad de máscara de instantáneas o establecer la sombra para heredar automáticamente la máscara del contenido. Si usa un LayerVisual, la sombra heredará la máscara de forma predeterminada.

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

![Imagen de web conectados con sombra paralela enmascarado](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>Uso de una máscara alternativa

En algunos casos, es posible que desea la sombra de forma tal que no coincide con el contenido del objeto Visual. Para lograr este efecto, deberá establecer explícitamente la propiedad de máscara con un pincel con alfa.

En el ejemplo siguiente, se cargan dos superficies: uno para el contenido Visual y otro para la máscara de sombra:

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

![Imagen de web conectados con un círculo enmascara la sombra paralela](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>Animar

Como es estándar en la capa Visual, se pueden animar propiedades DropShadow usar animaciones de composición. A continuación, modificaremos el código del ejemplo fragmentos anterior para animar el radio de desenfoque de la sombra.

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

Si desea agregar una sombra a elementos de marco de trabajo más complejo, hay un par de formas para interoperar con sombras entre XAML y la composición:

1. Use la [DropShadowPanel](https://github.com/Microsoft/UWPCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) disponibles en el Kit de herramientas de la Comunidad de Windows. Consulte la [DropShadowPanel documentación](https://docs.microsoft.com/windows/uwpcommunitytoolkit/controls/DropShadowPanel) para obtener más información sobre cómo usarlo.
1. Crear un objeto Visual para usar como el host sombra y asociarla al documento XAML Visual.
1. Use la Galería de ejemplos de composición [SamplesCommon](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SamplesCommon/SamplesCommon) control CompositionShadow personalizado. Observe el siguiente ejemplo de uso.

## <a name="performance"></a>Rendimiento

Aunque la capa Visual tiene muchas optimizaciones para realizar efectos eficientes y utilizables, generar sombras puede ser una operación relativamente costosa dependiendo de qué opciones establecidas. A continuación se muestran el nivel 'desorbitados' para los distintos tipos de sombras. Tenga en cuenta que aunque determinadas sombras pueden ser costosas aún pueden ser adecuados usar con moderación en determinados escenarios.

Características de la sombra| Coste
------------- | -------------
Rectangular    | Bajo
Shadow.Mask      | Alto
CompositionDropShadowSourcePolicy.InheritFromVisualContent | Alto
Radio de desenfoque estático | Bajo
Animar el radio de desenfoque | Alto

## <a name="additional-resources"></a>Recursos adicionales

- [Composición DropShadow API](/uwp/api/Windows.UI.Composition.DropShadow)
- [Repositorio de GitHub WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs)