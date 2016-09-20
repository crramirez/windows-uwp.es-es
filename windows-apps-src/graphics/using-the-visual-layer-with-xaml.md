---
author: jaster
ms.assetid: 
title: Uso de la capa visual con XAML
description: "Obtén información sobre las técnicas de uso de las API de la capa visual en combinación con contenido XAML ya existente para crear animaciones y efectos avanzados."
translationtype: Human Translation
ms.sourcegitcommit: dfda33c70224f32d9c3e8877eabdfcd965521757
ms.openlocfilehash: 00d663b130202f4513cd1a9d82baed4068d909d3

---

# Uso de la capa visual con XAML

## Introducción

La mayoría de las aplicaciones que usan las funcionalidades de la capa visual usarán XAML para definir el contenido de la interfaz de usuario principal. En la Actualización de aniversario de Windows10 hay nuevas características en el marco XAML y en la capa visual que hacen más fácil combinar estas dos tecnologías para crear experiencias del usuario sorprendentes.
La funcionalidad de "interoperabilidad" entre XAML y la capa visual puede usarse para crear animaciones y efectos avanzados que no están disponibles usando únicamente la API de XAML. Esto incluye:

-              Animaciones controladas por desplazamientos y parallax
-              Animaciones de diseño automático
-              Sombras paralelas con píxeles perfectos
-              Efectos de desenfoque y cristal esmerilado

Estos efectos y animaciones pueden aplicarse a contenido XAML existente, para que no tengas que reestructurar considerablemente la aplicación XAML para aprovechar la nueva funcionalidad.
Las animaciones de diseño, las sombras y los efectos de desenfoque se tratan en la sección Recetas que aparece más adelante. Para obtener un ejemplo de código que implementa el parallax, consulta la [ParallaxingListItems sample](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems) (Muestra de ParallaxingListItems). El [repositorio WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs) (en inglés) también contiene varios ejemplos más para la implementación de animaciones, sombras y efectos.

## La clase **ElementCompositionPreview**

[**ElementCompositionPreview**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.aspx) es una clase estática que proporciona la funcionalidad de interoperabilidad entre XAML y la capa visual. Para obtener información general sobre la capa visual y sus funcionalidades, consulta [Capa visual](https://msdn.microsoft.com/en-us/windows/uwp/graphics/visual-layer). La clase **ElementCompositionPreview** proporciona los siguientes métodos:

-   [**GetElementVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): Obtén un objeto visual de "distribución" que se utiliza para representar este elemento.
-   [**SetElementChildVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual.aspx): Establece un objeto visual de "entrega" como último objeto secundario del árbol visual de este elemento. Este objeto visual se dibujará encima del resto del elemento. 
-   [**GetElementChildVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): Recupera el conjunto visual mediante **SetElementChildVisual**.
-   [**GetScrollViewerManipulationPropertySet**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): Obtén un objeto que puede usarse para crear animaciones de 60fps basadas en una compensación del desplazamiento de un **ScrollViewer**.

## Comentarios sobre **ElementCompositionPreview.GetElementVisual**

**ElementCompositionPreview.GetElementVisual** devuelve un objeto visual de "distribución" que se utiliza para representar el **UIElement** dado. El marco XAML establece propiedades como **Visual.Opacity**, **Visual.Offset** y **Visual.Size** en función del estado del UIElement. Esto permite técnicas como las animaciones de cambio de posición implícitas (consulta *Recetas*).

Ten en cuenta que como las propiedades **Offset** y **Size** se establecen como resultado del diseño del marco XAML, los desarrolladores deben tener cuidado al modificar o animar estas propiedades. Los desarrolladores solo deben modificar o animar la propiedad Offset si la esquina superior izquierda del elemento tiene la misma posición en el diseño que la de su elemento primario. Por lo general, el tamaño no se debe modificar, pero el acceso a la propiedad puede resultar útil. Por ejemplo, las muestras de sombra paralela y cristal esmerilado de más adelante usan la propiedad Size de un objeto visual de distribución como entrada para una animación.

Como advertencia adicional, las propiedades actualizadas de un objeto visual de distribución no se reflejarán en el UIElement correspondiente. Por lo tanto, por ejemplo, establecer **UIElement.Opacity** en 0,5 establecerá la propiedad Opacity del objeto visual de entrega correspondiente en 0,5. Sin embargo, establecer la propiedad **Opacity** del objeto visual de entrega en 0,5 hará que el contenido aparezca con un 50% de opacidad, pero no cambiará el valor de la propiedad Opacity del UIElement correspondiente.

### Ejemplo de animación de la propiedad **Offset**

#### Incorrecto

```xml
<Border>
      <Image x:Name="MyImage" Margin="5" />
</Border>
```

```csharp
// Doesn’t work because Image has a margin!
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

#### Correcto

```xml
<Border>
    <Canvas Margin="5">
        <Image x:Name="MyImage" />
    </Canvas>
</Border>
```

```csharp
// This works because the Canvas parent doesn’t generate a layout offset.
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

## El método **ElementCompositionPreview.SetElementChildVisual**

**ElementCompositionPreview.SetElementChildVisual** permite al desarrollador proporcionar un objeto visual de "entrega" que aparecerá como parte del árbol visual de un elemento. Esto permite a los desarrolladores crear una "isla de composición", en la que el contenido basado en objetos visuales puede aparecer dentro de una interfaz de usuario de XAML. Los desarrolladores deben ser conservadores a la hora de usar esta técnica, ya que el contenido basado en objetos visuales no tendrá las mismas garantías de accesibilidad y experiencia del usuario que el contenido XAML. Por lo tanto, por lo general se recomienda que esta técnica se use solo cuando sea necesario para implementar efectos personalizados, como los que se encuentran en la sección Recetas que aparece más adelante.

## Métodos **GetAlphaMask**

[**Image**](https://msdn.microsoft.com/library/windows/apps/br242752), [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) y [**Shape**](https://msdn.microsoft.com/library/windows/apps/br243377) implementan un método llamado **GetAlphaMask**, el cual devuelve un objeto **CompositionBrush** que representa una imagen en escala de grises con la forma del elemento. Este objeto **CompositionBrush** puede servir como entrada para una composición **DropShadow**, de modo que la sombra pueda reflejar la forma del elemento en lugar de un rectángulo. Esto permite sombras con píxeles perfectos basadas en contornos para texto, imágenes con alfa y formas. Consulta *Sombra paralela* más adelante para obtener un ejemplo de esta API.

## Recetas

### Animación del cambio de posición

Mediante el uso de animaciones implícitas de composición, un desarrollador puede animar automáticamente los cambios en el diseño de un elemento en relación con su elemento primario. Por ejemplo, si cambias la propiedad **Margin** del botón siguiente, este se animará automáticamente a su nueva posición de diseño.

#### Información general sobre la implementación

1.            Obtén el objeto **Visual** de entrega para el elemento de destino.
2.            Crea una **ImplicitAnimationCollection** que anime automáticamente los cambios en la propiedad **Offset**.
3.            Asocia la **ImplicitAnimationCollection** con el objeto visual de respaldo.

#### XAML

```xml
<Button x:Name="RepositionTarget" Content="Click Me" />
```

#### C&#35;

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeRepositionAnimation(RepositionTarget);
}

private void InitializeRepositionAnimation(UIElement repositionTarget)
{
    var targetVisual = ElementCompositionPreview.GetElementVisual(repositionTarget);
    Compositor compositor = targetVisual.Compositor;

    // Create an animation to animate targetVisual's Offset property to its final value
    var repositionAnimation = compositor.CreateVector3KeyFrameAnimation();
    repositionAnimation.Duration = TimeSpan.FromSeconds(0.66);
    repositionAnimation.Target = "Offset";
    repositionAnimation.InsertExpressionKeyFrame(1.0f, "this.FinalValue");

    // Run this animation when the Offset Property is changed
    var repositionAnimations = compositor.CreateImplicitAnimationCollection();
    repositionAnimations["Offset"] = repositionAnimation;

    targetVisual.ImplicitAnimations = repositionAnimations;
}
```

### Sombra paralela

Aplica una sombra paralela con píxeles perfectos a un **UIElement**, por ejemplo, una **elipse** que contenga una imagen. Como la sombra requiere un objeto **SpriteVisual** creado por la aplicación, es necesario crear un elemento "host" que contendrá el objeto **SpriteVisual** mediante **ElementCompositionPreview.SetElementChildVisual**.

#### Información general sobre la implementación

1.            Obtén el objeto **Visual** de entrega para el elemento host.
2.            Crea una Windows.UI.Composition **DropShadow**.
3.            Configura la **DropShadow** para que obtenga su forma del elemento de destino a través de una máscara.
    - De manera predeterminada, la **DropShadow** es rectangular, por lo que esto no es necesario si el destino es rectangular.
4.            Adjunta la sombras a un nuevo objeto **SpriteVisual**y establece dicho objeto **SpriteVisual** como elemento secundario del elemento host.
5.            Enlaza el tamaño del objeto **SpriteVisual** con el tamaño del host mediante una **ExpressionAnimation**.

#### XAML

```xml
<Grid Width="200" Height="200">
    <Canvas x:Name="ShadowHost" />
    <Ellipse x:Name="CircleImage">
        <Ellipse.Fill>
            <ImageBrush ImageSource="Assets/Images/2.jpg" Stretch="UniformToFill" />
        </Ellipse.Fill>
    </Ellipse>
</Grid>
```

#### C&#35;

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost, CircleImage);
}

private void InitializeDropShadow(UIElement shadowHost, Shape shadowTarget)
{
    Visual hostVisual = ElementCompositionPreview.GetElementVisual(shadowHost);
    Compositor compositor = hostVisual.Compositor;

    // Create a drop shadow
    var dropShadow = compositor.CreateDropShadow();
    dropShadow.Color = Color.FromArgb(255, 75, 75, 80);
    dropShadow.BlurRadius = 15.0f;
    dropShadow.Offset = new Vector3(2.5f, 2.5f, 0.0f);
    // Associate the shape of the shadow with the shape of the target element
    dropShadow.Mask = shadowTarget.GetAlphaMask();

    // Create a Visual to hold the shadow
    var shadowVisual = compositor.CreateSpriteVisual();
    shadowVisual.Shadow = dropShadow;

    // Add the shadow as a child of the host in the visual tree
   ElementCompositionPreview.SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    var bindSizeAnimation = compositor.CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation.SetReferenceParameter("hostVisual", hostVisual);

    shadowVisual.StartAnimation("Size", bindSizeAnimation);
}
```

### Cristal esmerilado

Crea un efecto que desenfoque y aporte tonos al contenido en segundo plano. Ten en cuenta que los desarrolladores tienen que instalar el paquete de Win2D NuGet para usar efectos. Consulta la [página principal de Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) para obtener instrucciones de instalación.

#### Información general sobre la implementación

1.            Obtén el objeto **Visual** de entrega para el elemento host.
2.            Crear un árbol de efectos de desenfoque con Win2D y **CompositionEffectSourceParameter**.
3.            Crea un **CompositionEffectBrush** basado en el árbol de efectos.
4.            Establece la entrada del **CompositionEffectBrush** en un **CompositionBackdropBrush**, lo que permite aplicar un efecto al contenido que hay detrás de un objeto **SpriteVisual**.
5.            Establece el **CompositionEffectBrush** como el contenido de un nuevo objeto **SpriteVisual**y establece este **SpriteVisual** como el elemento secundario del elemento host.
6.            Enlaza el tamaño del objeto **SpriteVisual** con el tamaño del host mediante una **ExpressionAnimation**.

#### XAML

```xml
<Grid Width="300" Height="300" Grid.Column="1">
    <Image
        Source="Assets/Images/2.jpg"
        Width="200"
        Height="200" />
    <Canvas
        x:Name="GlassHost"
        Width="150"
        Height="300"
        HorizontalAlignment="Right" />
</Grid>
```

#### C&#35;

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeFrostedGlass(GlassHost);
}

private void InitializedFrostedGlass(UIElement glassHost)
{
    Visual hostVisual = ElementCompositionPreview.GetElementVisual(glassHost);
    Compositor compositor = hostVisual.Compositor;

    // Create a glass effect, requires Win2D NuGet package
    var glassEffect = new GaussianBlurEffect
    { 
        BlurAmount = 15.0f,
        BorderMode = EffectBorderMode.Hard,
        Source = new ArithmeticCompositeEffect
        {
            MultiplyAmount = 0,
            Source1Amount = 0.5f,
            Source2Amount = 0.5f,
            Source1 = new CompositionEffectSourceParameter("backdropBrush"),
            Source2 = new ColorSourceEffect
            {
                Color = Color.FromArgb(255, 245, 245, 245)
            }
        }
    };

    //  Create an instance of the effect and set its source to a CompositionBackdropBrush
    var effectFactory = compositor.CreateEffectFactory(glassEffect);
    var backdropBrush = compositor.CreateBackdropBrush();
    var effectBrush = effectFactory.CreateBrush();

    effectBrush.SetSourceParameter("backdropBrush", backdropBrush);

    // Create a Visual to contain the frosted glass effect
    var glassVisual = compositor.CreateSpriteVisual();
    glassVisual.Brush = effectBrush;

    // Add the blur as a child of the host in the visual tree
    ElementCompositionPreview.SetElementChildVisual(glassHost, glassVisual);

    // Make sure size of glass host and glass visual always stay in sync
    var bindSizeAnimation = compositor.CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation.SetReferenceParameter("hostVisual", hostVisual);

    glassVisual.StartAnimation("Size", bindSizeAnimation);
}
```

## Recursos adicionales:

-   [Información general sobre la capa visual](https://msdn.microsoft.com/en-us/windows/uwp/graphics/visual-layer)
-   [La clase **ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/mt608976)
-   Muestras avanzadas de la interfaz de usuario y la composición en [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs).
-   [Muestra de BasicXamlInterop](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/BasicXamlInterop)
-   [Muestra de ParallaxingListItems](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)



<!--HONumber=Aug16_HO3-->


