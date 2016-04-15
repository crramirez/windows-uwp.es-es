---
ms.assetid: 6e9b9ff2-234b-6f63-0975-1afb2d86ba1a
title: Efectos de composición
description: Las API de efectos permiten a los desarrolladores personalizar la representación de su interfaz de usuario.
---
# Efectos de composición

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

La API [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878) de WinRT permite aplicar efectos a las imágenes en tiempo real y a la interfaz de usuario con propiedades de efectos que se pueden animar. En esta introducción, veremos las funciones disponibles que permiten aplicar efectos a una composición visual.

Para respaldar la coherencia de la [Plataforma universal de Windows (UWP)](https://msdn.microsoft.com/library/windows/apps/dn726767.aspx) para los desarrolladores que describen efectos en sus aplicaciones, los efectos de composición aprovechan la interfaz IGraphicsEffect de Win2D para usar las descripciones de efectos a través del espacio de nombre [Microsoft.Graphics.Canvas.Effects](http://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.md).

Los efectos de pincel se usan para pintar áreas de una aplicación mediante efectos en un conjunto de imágenes existentes. Las API de efectos de composición de Windows 10 se centran en las clases SpriteVisual. La clase SpriteVisual permite la flexibilidad e interacción en cuanto a la creación de colores, imágenes y efectos. La clase SpriteVisual es un tipo visual de composición que puede rellenar un rectángulo 2D con un pincel. El objeto visual define los límites del rectángulo y el pincel define los píxeles usados para pintar el rectángulo.

Los pinceles de efecto se usan en objetos visuales de árbol de composición cuyo contenido procede de la salida de un gráfico de efecto. Los efectos pueden hacer referencia a superficies o texturas existentes, pero no a la salida de otros árboles de composición.

## Características de los efectos

-   [Biblioteca de efectos](./composition-effects.md#effect-library)
-   [Encadenamiento de efectos](./composition-effects.md#chaining-effects)
-   [Compatibilidad con animaciones](./composition-effects.md#animation-support)
-   [Propiedades de efectos: constante frente a animado](./composition-effects.md#effect-properties-constant-vs-animated)
-   [Varias instancias de efecto con propiedades independientes](./composition-effects.md#multiple-effect-instances-with-independent-properties)

### Biblioteca de efectos

La composición admite actualmente los siguientes efectos:

| Efecto               | Descripción                                                                                                                                                                                                                |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Transformación afín 2D  | Aplica una matriz de transformación afín 2D a una imagen. Este efecto se usa para animar la máscara alfa en las [muestras](https://github.com/Microsoft/composition/tree/master/SDK10240_WIN10_RTM/BasicCompositonEffects) de efectos.       |
| Compuesta aritmética | Combina dos imágenes mediante una ecuación flexible. La compuesta aritmética se usa para crear un efecto de encadenado en las [muestras](https://github.com/Microsoft/composition/tree/master/SDK10240_WIN10_RTM/BasicCompositonEffects). |
| Efecto de fusión         | Crea un efecto de fusión que combina dos imágenes. La composición proporciona 21 de los 26 [modos de fusión](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_Effects_BlendEffectMode.md) que se admiten en Win2D.        |
| Fuente de color         | Genera una imagen con un color sólido.                                                                                                                                                                               |
| Compuesta            | Combina dos imágenes. La composición proporciona los 13 [modos compuestos](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasComposite.md) que se admiten en Win2D.                                              |
| Contraste             | Aumenta o disminuye el contraste de una imagen.                                                                                                                                                                           |
| Exposición             | Aumenta o disminuye la exposición de una imagen.                                                                                                                                                                           |
| Escala de grises            | Convierte una imagen a gris monocromático.                                                                                                                                                                                   |
| Transferencia gama       | Modifica los colores de una imagen mediante la aplicación de una función de transferencia gama por canal.                                                                                                                                           |
| Giro de matiz           | Modifica el color de una imagen mediante la rotación de sus valores de matiz.                                                                                                                                                                   |
| Invertir               | Invierte los colores de una imagen.                                                                                                                                                                                            |
| Saturar             | Altera la saturación de una imagen.                                                                                                                                                                                         |
| Sepia                | Convierte una imagen a tonos sepia.                                                                                                                                                                                          |
| Temperatura y tono | Ajusta la temperatura y/o el tono de una imagen.                                                                                                                                                                           |

 

Consulta el espacio de nombres [Microsoft.Graphics.Canvas.Effects](http://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.md) de Win2D para obtener información más detallada. Los efectos que no se admiten en la composición se indican como \[NoComposition\].

### Encadenamiento de efectos

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

### Compatibilidad con animaciones

Las propiedades de los efectos admiten la animación. Durante la compilación del efecto se pueden especificar las propiedades del efecto, que pueden animarse y se pueden incorporar como constantes. Las propiedades que se pueden animar se especifican a través de las cadenas con formato "nombre de efecto.nombre de propiedad". Estas propiedades pueden animarse de forma independiente en varias instancias del efecto.

### Propiedades de efectos: constante frente a animado

Durante la compilación de efectos puedes especificar propiedades de efectos como dinámicas o como propiedades que se "incorporan" como constantes. Las propiedades dinámicas se especifican a través de las cadenas del formulario "<effect name>.<property name>”. Las propiedades dinámicas se puede establecer en un valor específico o se pueden animar con el sistema de animación de la composición.

Al compilar la descripción del efecto anterior, tienes la flexibilidad de preparar la saturación para que sea igual a 0,5 o hacerla dinámica y configurarla dinámicamente o animarla.

Compilación de un efecto con la saturación incorporada:

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);              
```

Compilación de un efecto con saturación dinámica:

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect, new[]{SaturationEffect.Saturation});
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

Consulta la [Muestra Desaturación - Animación](https://github.com/Microsoft/composition/tree/master/SDK10586_NOV_UPDATE_RTM/BasicCompositonEffects/Desaturation%20-%20Animation) para las propiedades de efectos animados con fotogramas clave y la [muestra AlphaMask](https://github.com/Microsoft/composition/tree/master/SDK10586_NOV_UPDATE_RTM/BasicCompositonEffects/AlphaMask) para el uso de efectos y expresiones.

### Varias instancias de efecto con propiedades independientes

Al especificar que un parámetro debe ser dinámico durante la compilación del efecto, el parámetro se puede cambiar para cada instancia del efecto. Esto permite que dos objetos visuales usen el mismo efecto, pero se representen con propiedades de efecto diferentes. Consulta la [muestra](https://github.com/Microsoft/composition/tree/master/SDK10586_NOV_UPDATE_RTM/BasicCompositonEffects/ColorSource%20and%20Blend) ColorSource and Blend para obtener más información.

## Introducción a los efectos de composición

Este tutorial de inicio rápido muestra cómo usar algunas de las funciones básicas de los efectos.

-   [Instalación de Visual Studio](./composition-effects.md#installing-visual-studio)
-   [Creación de un nuevo proyecto](./composition-effects.md#creating-a-new-project)
-   [Instalación de Win2D](./composition-effects.md#installing-win2d)
-   [Configuración de los conceptos básicos de la composición](./composition-effects.md#setting-your-composition-basics)
-   [Creación de un pincel CompositionSurface](./composition-effects.md#creating-a-compositionsurface-brush)
-   [Creación, compilación y aplicación de efectos](./composition-effects.md#creating,-compiling-and-applying-effects)

### Instalación de Visual Studio

-   Si no tienes una versión compatible de Visual Studio instalada, dirígete a la página de descargas de Visual Studio [aquí](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).

### Creación de un nuevo proyecto

-   Ve a Archivo -> Nuevo -> Proyecto...
-   Selecciona "Visual C#".
-   Crea una "aplicación vacía (universal de Windows)" (Visual Studio 2015).
-   Escribe el nombre que quieras asignar al proyecto.
-   Haz clic en "Aceptar".

### Instalación de Win2D

Win2D se suministra como un paquete Nuget.org y debe instalarse para poder usar los efectos.

Existen dos versiones del paquete, una para Windows 10 y otra para Windows 8.1. Para los efectos de composición se usará la versión de Windows 10.

-   Inicia el Administrador de paquetes de NuGet. Para ello, ve a Herramientas → Administrador de paquetes NuGet → Administrar paquetes NuGet para la solución.
-   Busca "Win2D" y selecciona el paquete adecuado para tu versión de Windows de destino. Dado que la API Windows.UI.Composition es compatible con Windows 10 (no 8.1), seleccione Win2D.uwp.
-   Acepta el acuerdo de licencia.
-   Haz clic en "Cerrar".

En los siguientes pasos usaremos las API de composición para aplicar un efecto de saturación a esta imagen de un gato, que eliminará todo saturación. En este modelo, se crea el efecto y luego se aplica a una imagen.

![Imagen de origen](images/composition-cat-source.png)
### Configuración de los conceptos básicos de la composición

Consulta la [muestra del árbol de objetos visuales de composición](https://github.com/Microsoft/composition/tree/master/SDK10586_NOV_UPDATE_RTM/CompositionVisual) en GitHub para ver un ejemplo de cómo configurar el compositor Windows.UI.Composition, del elemento raíz ContainerVisual y de cómo asociarlo a la ventana principal.

```cs
_compositor = new Compositor();
_root = _compositor.CreateContainerVisual();
_target = _compositor.CreateTargetForCurrentView();
_target.Root = _root;
_imageFactory = new CompositionImageFactory(_compositor)
Desaturate();
```

### Creación de un pincel CompositionSurface

```cs
CompositionSurfaceBrush surfaceBrush = _compositor.CreateSurfaceBrush();
LoadImage(surfaceBrush); 
```

### Creación, compilación y aplicación de efectos

1.) Crear el efecto de gráficos.
```cs
var graphicsEffect = new SaturationEffect
{
  Saturation = 0.0f,
  Source = new CompositionEffectSourceParameter("mySource")
};
```

2.) Compilar el efecto y crear el pincel de efecto
```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);

var catEffect = effectFactory.CreateBrush();
catEffect.SetSourceParameter("mySource", surfaceBrush);
```

3.) Crear una clase SpriteVIsual en el árbol de composición y aplicar el efecto
```cs
var catVisual = _compositor.CreateSpriteVisual();
  catVisual.Brush = catEffect;
  catVisual.Size = new Vector2(219, 300);
  _root.Children.InsertAtBottom(catVisual);
}
```

4.) Crear el origen de la imagen que se va a cargar.
```cs
CompositionImage imageSource = _imageFactory.CreateImageFromUri(new Uri("ms-appx:///Assets/cat.png"));
CompositionImageLoadResult result = await imageSource.CompleteLoadAsync();
if (result.Status == CompositionImageLoadStatus.Success)
```

5.) Tamaño y pincel de la superficie en la clase SpriteVisual
```cs
brush.Surface = imageSource.Surface;
```

6.) Ejecutar la aplicación. El resultado debe ser un gato sin saturación:

![Imagen sin saturación](images/composition-cat-desaturated.png)
## Más información

-   [Microsoft: tema sobre la composición en GitHub](https://github.com/Microsoft/composition)
-   [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878)
-   [Equipo de composición de Windows en Twitter](https://twitter.com/wincomposition)
-   [Información general de la composición](https://blogs.windows.com/buildingapps/2015/12/08/awaken-your-creativity-with-the-new-windows-ui-composition/)
-   [Conceptos básicos del árbol visual](composition-visual-tree.md)
-   [Pinceles de composición](composition-brushes.md)
-   [Información general sobre animaciones](composition-animation.md)
-   [Interoperación DirectX y Direct2D nativa de composición con BeginDraw y EndDraw](composition-native-interop.md)

 

 






<!--HONumber=Mar16_HO1-->


