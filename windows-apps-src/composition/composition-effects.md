---
ms.assetid: 6e9b9ff2-234b-6f63-0975-1afb2d86ba1a
title: Efectos de composición
description: Las API de efectos permiten a los desarrolladores personalizar la representación de su interfaz de usuario.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 75af433d80364485b0c12a9540c0d7bb471c4e28
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8690875"
---
# <a name="composition-effects"></a>Efectos de composición

Las API [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878) permiten aplicar efectos a las imágenes en tiempo real y a la interfaz de usuario, con propiedades de efectos que se pueden animar. En esta introducción, veremos las funciones disponibles que permiten aplicar efectos a una composición visual.

Para respaldar la coherencia de la [Plataforma universal de Windows (UWP)](https://msdn.microsoft.com/library/windows/apps/dn726767.aspx) para los desarrolladores que describen efectos en sus aplicaciones, los efectos de composición aprovechan la interfaz IGraphicsEffect de Win2D para usar las descripciones de efectos a través del espacio de nombre [Microsoft.Graphics.Canvas.Effects](http://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.htm).

Los efectos de pincel se usan para pintar áreas de una aplicación mediante efectos en un conjunto de imágenes existentes. Las API de efectos de composición de Windows 10 se centran en las clases SpriteVisual. La clase SpriteVisual permite la flexibilidad e interacción en cuanto a la creación de colores, imágenes y efectos. La clase SpriteVisual es un tipo visual de composición que puede rellenar un rectángulo 2D con un pincel. El objeto visual define los límites del rectángulo y el pincel define los píxeles usados para pintar el rectángulo.

Los pinceles de efecto se usan en objetos visuales de árbol de composición cuyo contenido procede de la salida de un gráfico de efecto. Los efectos pueden hacer referencia a superficies o texturas existentes, pero no a la salida de otros árboles de composición.

Los efectos también se pueden aplicar a objetos UIElements de XAML mediante un pincel de efecto con [**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

## <a name="effect-features"></a>Características de los efectos

- [Biblioteca de efectos](./composition-effects.md#effect-library)
- [Encadenamiento de efectos](./composition-effects.md#chaining-effects)
- [Compatibilidad con animaciones](./composition-effects.md#animation-support)
- [Comparación entre las propiedades de efectos constantes y las animadas](./composition-effects.md#constant-vs-animated-effect-properties)
- [Varias instancias de efecto con propiedades independientes](./composition-effects.md#multiple-effect-instances-with-independent-properties)

### <a name="effect-library"></a>Biblioteca de efectos

La composición admite actualmente los siguientes efectos:

| Efecto               | Descripción                                                                                                                                                                                                                |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Transformación afín 2D  | Aplica una matriz de transformación afín 2D a una imagen. Este efecto se usa para animar la máscara alfa en las [muestras](http://go.microsoft.com/fwlink/?LinkId=785341) de efectos.       |
| Compuesta aritmética | Combina dos imágenes mediante una ecuación flexible. La compuesta aritmética se usa para crear un efecto de encadenado en las [muestras](http://go.microsoft.com/fwlink/?LinkId=785341). |
| Efecto de fusión         | Crea un efecto de fusión que combina dos imágenes. La composición proporciona 21 de los 26 [modos de fusión](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_Effects_BlendEffectMode.htm) que se admiten en Win2D.        |
| Fuente de color         | Genera una imagen con un color sólido.                                                                                                                                                                               |
| Compuesta            | Combina dos imágenes. La composición proporciona los 13 [modos compuestos](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasComposite.htm) que se admiten en Win2D.                                              |
| Contraste             | Aumenta o disminuye el contraste de una imagen.                                                                                                                                                                           |
| Exposición             | Aumenta o disminuye la exposición de una imagen.                                                                                                                                                                           |
| Escala de grises            | Convierte una imagen a gris monocromático.                                                                                                                                                                                   |
| Transferencia gama       | Modifica los colores de una imagen mediante la aplicación de una función de transferencia gama por canal.                                                                                                                                           |
| Giro de matiz           | Modifica el color de una imagen mediante la rotación de sus valores de matiz.                                                                                                                                                                   |
| Invertir               | Invierte los colores de una imagen.                                                                                                                                                                                            |
| Saturar             | Altera la saturación de una imagen.                                                                                                                                                                                         |
| Sepia                | Convierte una imagen a tonos sepia.                                                                                                                                                                                          |
| Temperatura y tono | Ajusta la temperatura y/o el tono de una imagen.                                                                                                                                                                           |

Consulta el espacio de nombres [Microsoft.Graphics.Canvas.Effects](http://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.htm) de Win2D para obtener información más detallada. Los efectos que no se admiten en la composición se indican como \[NoComposition\].

### <a name="chaining-effects"></a>Encadenamiento de efectos

Los efectos se pueden encadenar, lo que permite que una aplicación use simultáneamente varios efectos en una imagen. Los gráficos de efectos pueden admitir varios efectos que pueden hacerse referencia entre sí. Al describir el efecto, basta con agregar un efecto como entrada para el efecto.

```cs
IGraphicsEffect graphicsEffect =
new Microsoft.Graphics.Canvas.Effects.ArithmeticCompositeEffect
{
  Source1 = new CompositionEffectSourceParameter("source1"),
  Source2 = new SaturationEffect
  {
    Saturation = 0,
    Source = new CompositionEffectSourceParameter("source2")
  },
  MultiplyAmount = 0,
  Source1Amount = 0.5f,
  Source2Amount = 0.5f,
  Offset = 0
}
```

En el ejemplo anterior se describe un efecto compuesto aritmético que tiene dos entradas. La segunda entrada tiene un efecto de saturación con una propiedad de saturación 0,5.

### <a name="animation-support"></a>Compatibilidad con animaciones

Las propiedades de los efectos admiten la animación. Durante la compilación del efecto se pueden especificar las propiedades del efecto, que pueden animarse y se pueden incorporar como constantes. Las propiedades que se pueden animar se especifican a través de las cadenas con formato "nombre de efecto.nombre de propiedad". Estas propiedades pueden animarse de forma independiente en varias instancias del efecto.

### <a name="constant-vs-animated-effect-properties"></a>Comparación entre las propiedades de efectos constantes y las animadas

Durante la compilación de efectos, puedes especificar las propiedades de efectos como dinámicas o como propiedades que se "incorporan" como constantes. Las propiedades dinámicas se especifican a través de las cadenas del formulario "<effect name>.<property name>". Las propiedades dinámicas se puede establecer en un valor específico o se pueden animar con el sistema de animación de la composición.

Al compilar la descripción del efecto anterior, tienes la flexibilidad de preparar la saturación para que sea igual a 0,5 o hacerla dinámica y configurarla dinámicamente o animarla.

Compilación de un efecto con la saturación incorporada:

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);
```

Compilación de un efecto con saturación dinámica:

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect, new[]{"SaturationEffect.Saturation"});
_catEffect = effectFactory.CreateBrush();
_catEffect.SetSourceParameter("mySource", surfaceBrush);
_catEffect.Properties.InsertScalar("saturationEffect.Saturation", 0f);
```

La propiedad de saturación del efecto anterior puede establecerse en un valor estático o animado con las animaciones Expression o ScalarKeyFrame.

Puedes crear un ScalarKeyFrame que se usará para animar la propiedad de saturación de un efecto como este:

```cs
ScalarKeyFrameAnimation effectAnimation = _compositor.CreateScalarKeyFrameAnimation();
            effectAnimation.InsertKeyFrame(0f, 0f);
            effectAnimation.InsertKeyFrame(0.50f, 1f);
            effectAnimation.InsertKeyFrame(1.0f, 0f);
            effectAnimation.Duration = TimeSpan.FromMilliseconds(2500);
            effectAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
```

Inicia la animación en la propiedad de saturación del efecto como este:

```cs
catEffect.Properties.StartAnimation("saturationEffect.Saturation", effectAnimation);
```

Consulta la [Muestra Desaturación - Animación](http://go.microsoft.com/fwlink/?LinkId=785342) para las propiedades de efectos animados con fotogramas clave y la [muestra AlphaMask](http://go.microsoft.com/fwlink/?LinkId=785343) para el uso de efectos y expresiones.

### <a name="multiple-effect-instances-with-independent-properties"></a>Varias instancias de efecto con propiedades independientes

Al especificar que un parámetro debe ser dinámico durante la compilación del efecto, el parámetro se puede cambiar para cada instancia del efecto. Esto permite que dos objetos visuales usen el mismo efecto, pero se representen con propiedades de efecto diferentes. Consulta la [muestra](http://go.microsoft.com/fwlink/?LinkId=785344) ColorSource and Blend para obtener más información.

## <a name="getting-started-with-composition-effects"></a>Introducción a los efectos de composición

Este tutorial de inicio rápido muestra cómo usar algunas de las funciones básicas de los efectos.

- [Instalación de Visual Studio](./composition-effects.md#installing-visual-studio)
- [Creación de un nuevo proyecto](./composition-effects.md#creating-a-new-project)
- [Instalación de Win2D](./composition-effects.md#installing-win2d)
- [Configuración de los conceptos básicos de la composición](./composition-effects.md#setting-your-composition-basics)
- [Creación de un pincel CompositionSurface](./composition-effects.md#creating-a-compositionsurface-brush)
- [Creación, compilación y aplicación de efectos](./composition-effects.md#creating-compiling-and-applying-effects)

### <a name="installing-visual-studio"></a>Instalación de Visual Studio

- Si no tienes una versión compatible de Visual Studio instalada, dirígete a la página de descargas de Visual Studio [aquí](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).

### <a name="creating-a-new-project"></a>Creación de un nuevo proyecto

- Ve a Archivo -> Nuevo -> Proyecto...
- Selecciona "Visual C#".
- Crea una "aplicación vacía (universal de Windows)" (Visual Studio 2015).
- Escribe el nombre que quieras asignar al proyecto.
- Haz clic en "Aceptar".

### <a name="installing-win2d"></a>Instalación de Win2D

Win2D se suministra como un paquete Nuget.org y debe instalarse para poder usar los efectos.

Existen dos versiones del paquete, una para Windows 10 y otra para Windows 8.1. Para los efectos de composición se usará la versión de Windows 10.

- Inicia el Administrador de paquetes de NuGet. Para ello, ve a Herramientas → Administrador de paquetes NuGet → Administrar paquetes NuGet para la solución.
- Busca "Win2D" y selecciona el paquete adecuado para tu versión de Windows de destino. Dado que la API Windows.UI.Composition es compatible con Windows 10 (no 8.1), seleccione Win2D.uwp.
- Acepta el acuerdo de licencia.
- Haz clic en "Cerrar".

En los siguientes pasos usaremos las API de composición para aplicar un efecto de saturación a esta imagen de un gato, que eliminará todo saturación. En este modelo, se crea el efecto y luego se aplica a una imagen.

![Imagen de origen](images/composition-cat-source.png)
### <a name="setting-your-composition-basics"></a>Configuración de los conceptos básicos de la composición

Consulta la [muestra del árbol de objetos visuales de composición](http://go.microsoft.com/fwlink/?LinkId=785345) en GitHub para ver un ejemplo de cómo configurar el compositor Windows.UI.Composition, del elemento raíz ContainerVisual y de cómo asociarlo a la ventana principal.

```cs
_compositor = new Compositor();
_root = _compositor.CreateContainerVisual();
_target = _compositor.CreateTargetForCurrentView();
_target.Root = _root;
_imageFactory = new CompositionImageFactory(_compositor)
Desaturate();
```

### <a name="creating-a-compositionsurface-brush"></a>Creación de un pincel CompositionSurface

```cs
CompositionSurfaceBrush surfaceBrush = _compositor.CreateSurfaceBrush();
LoadImage(surfaceBrush);
```

### <a name="creating-compiling-and-applying-effects"></a>Creación, compilación y aplicación de efectos

1. Crear el efecto de gráficos

    ```cs
    var graphicsEffect = new SaturationEffect
    {
      Saturation = 0.0f,
      Source = new CompositionEffectSourceParameter("mySource")
    };
    ```

1. Compilar el efecto y crear el pincel de efecto

    ```cs
    var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);

    var catEffect = effectFactory.CreateBrush();
    catEffect.SetSourceParameter("mySource", surfaceBrush);
    ```

1. Crear una clase SpriteVIsual en el árbol de composición y aplicar el efecto

    ```cs
    var catVisual = _compositor.CreateSpriteVisual();
      catVisual.Brush = catEffect;
      catVisual.Size = new Vector2(219, 300);
      _root.Children.InsertAtBottom(catVisual);
    }
    ```

1. Crear el origen de la imagen que se va a cargar.

    ```cs
    CompositionImage imageSource = _imageFactory.CreateImageFromUri(new Uri("ms-appx:///Assets/cat.png"));
    CompositionImageLoadResult result = await imageSource.CompleteLoadAsync();
    if (result.Status == CompositionImageLoadStatus.Success)
    ```

1. Tamaño y pincel de la superficie en la clase SpriteVisual

    ```cs
    brush.Surface = imageSource.Surface;
    ```

1. Ejecutar la aplicación. El resultado debe ser un gato sin saturación:

![Imagen sin saturación](images/composition-cat-desaturated.png)

## <a name="more-information"></a>Más información

- [Microsoft: tema sobre la composición en GitHub](https://github.com/Microsoft/composition)
- [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878)
- [Equipo de composición de Windows en Twitter](https://twitter.com/wincomposition)
- [Información general de la composición](https://blogs.windows.com/buildingapps/2015/12/08/awaken-your-creativity-with-the-new-windows-ui-composition/)
- [Conceptos básicos del árbol visual](composition-visual-tree.md)
- [Pinceles de composición](composition-brushes.md)
- [XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)
- [Información general sobre animaciones](composition-animation.md)
- [Interoperación DirectX y Direct2D nativa de composición con BeginDraw y EndDraw](composition-native-interop.md)
