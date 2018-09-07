---
author: daneuber
title: Sombras de composición
description: La sombra API te permite agregar sombras personalizables dinámicas al contenido de la interfaz de usuario.
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 84e12d6c3e25a18902aaa55011949dd5b5ff97ca
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "3659232"
---
# <a name="shadows-in-windows-ui"></a>Sombras en la interfaz de usuario de Windows

La clase [DropShadow](/uwp/api/Windows.UI.Composition.DropShadow) proporciona medio de creación de una sombra configurable que se puede aplicar a un [objeto SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) o [LayerVisual](/uwp/api/windows.ui.composition.layervisual) (subárbol de elementos visuales). Como es habitual para los objetos de la capa Visual, todas las propiedades de la DropShadow se pueden animar con CompositionAnimations.

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

Hay varias formas de definir la forma de su DropShadow:

- **Usa el valor predeterminado** , de manera predeterminada, la forma de DropShadow se define mediante el modo de 'Predeterminado' en CompositionDropShadowSourcePolicy. SpriteVisual, el valor predeterminado es Rectangular, a menos que se proporciona una máscara. LayerVisual, valor predeterminado es hereden una máscara mediante el valor alfa del pincel del elemento visual.
- **Establecer una máscara** – se puede establecer la propiedad [máscara](/uwp/api/windows.ui.composition.dropshadow.mask) para definir una máscara de opacidad de la sombra.
- **Especificar usar heredado máscara** : establece la propiedad de [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) usar [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy). InheritFromVisualContent use la máscara que se generó desde el valor alfa del pincel del elemento visual.

## <a name="masking-to-match-your-content"></a>Enmascaramiento para que coincida con el contenido

Si quieres que la sombra para que coincida con el contenido del elemento Visual puede usar el pincel del elemento Visual para la propiedad de máscara de sombras, o establecer la sombra automáticamente hereden máscara desde el contenido. Si usas un LayerVisual, la sombra heredarán la máscara de manera predeterminada.

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

![Imagen de web conectada con enmascarada sombra paralela](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>Usar una máscara de alternativa

En algunos casos, puede que quieras la sombra de forma que no coincide con contenido de tu Visual. Para lograr este efecto, debes establecer explícitamente la propiedad de la máscara con un pincel alfa.

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

![Imagen de web conectada con un círculo enmascarada por sombra paralela](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>Animación

Dado que es estándar de la capa Visual, DropShadow propiedades se pueden animar con animaciones de composición. A continuación, se modifica el código de la muestra de fragmentos anterior para animar el radio de desenfoque de la sombra.

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

Si quieres agregar una sombra a elementos de marco más complejos, hay un par de formas para interoperar con sombras entre XAML y la composición:

1. Usa el [DropShadowPanel](https://github.com/Microsoft/UWPCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) disponibles en el Kit de herramientas de comunidad Windows. Consulta la [documentación de DropShadowPanel](https://docs.microsoft.com/windows/uwpcommunitytoolkit/controls/DropShadowPanel) para obtener más información sobre cómo usarla.
1. Crea un objeto Visual para usar como el host de sombras y vincular a la entrega XAML Visual.
1. Usar control de la Galería de ejemplos de composición [SamplesCommon](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SamplesCommon/SamplesCommon) de CompositionShadow personalizado. Vea el ejemplo a continuación para su uso.

## <a name="performance"></a>Rendimiento

Aunque la capa Visual tiene muchas optimizaciones en su lugar para lograr efectos eficiente y utilizable, la generación de sombras puede ser una operación relativamente costosa dependiendo de qué opciones establecidas. A continuación, encontrarás alto nivel 'los costos de' para distintos tipos de sombras. Ten en cuenta que aunque determinadas sombras pueden ser caras, puede apropiados usar con moderación en determinados escenarios.

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