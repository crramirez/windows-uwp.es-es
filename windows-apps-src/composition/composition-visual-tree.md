---
ms.assetid: f1297b7d-1a10-52ae-dd84-6d1ad2ae2fe6
title: Elemento visual de composición
description: Los elementos visuales de composición conforman la estructura del árbol visual que todas las demás funciones de la API de composición usarán y ampliarán. La API permite a los desarrolladores definir y crear uno o varios objetos visuales que representan un nodo único en un árbol visual.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 70f71265ff763ff8a160705694476e03bf8b6667
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166359"
---
# <a name="composition-visual"></a>Elemento visual de composición

Los elementos visuales de composición conforman la estructura del árbol visual que todas las demás funciones de la API de composición usarán y ampliarán. La API permite a los desarrolladores definir y crear uno o varios objetos visuales que representan un nodo único en un árbol visual.

## <a name="visuals"></a>Objetos visuales

Existen tres tipos de elementos visuales que conforman la estructura del árbol visual, además de una clase de pincel base con varias subclases que afectan al contenido de un elemento visual:

- [**Visual**](/uwp/api/Windows.UI.Composition.Visual): objeto base, la mayoría de las propiedades están aquí y las heredan los otros objetos Visual.
- [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual): se deriva del objeto [**Visual**](/uwp/api/Windows.UI.Composition.Visual) y agrega la capacidad para crear los elementos secundarios.
- [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) : deriva de [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual) y agrega la capacidad de asociar un pincel para que el objeto visual pueda representar píxeles, incluidas imágenes, efectos o un color sólido.

Puede aplicar contenido y efectos a SpriteVisuals mediante [**CompositionBrush**](/uwp/api/Windows.UI.Composition.CompositionBrush) y sus subclases, incluidos [**CompositionColorBrush**](/uwp/api/Windows.UI.Composition.CompositionColorBrush), [**CompositionSurfaceBrush**](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) y [**CompositionEffectBrush**](/uwp/api/Windows.UI.Composition.CompositionEffectBrush). Para obtener más información sobre los pinceles, consulte nuestra [**información general de CompositionBrush**](./composition-brushes.md).

## <a name="the-compositionvisual-sample"></a>Muestra de CompositionVisual

Aquí veremos un código de ejemplo que muestra los tres tipos de objetos visuales diferentes enumerados anteriormente. Si bien en esta muestra no se abarcan conceptos como, por ejemplo, animaciones o efectos más complejos, se incluyen los elementos esenciales que todos los sistemas usan. (El código de ejemplo completo se muestra al final de este artículo).

En el ejemplo, hay un número de cuadrados de color sólido en los que se puede hacer clic y arrastrar sobre la pantalla. Cuando se hace clic en un cuadrado, este se trae al frente, se gira 45 grados y se vuelve opaco cuando se arrastra.

Esto muestra varios de los conceptos básicos para trabajar con la API, entre los que se incluye lo siguiente:

- Crear un compositor
- Crear un SpriteVisual con un CompositionColorBrush
- Recortar el visual
- Girar el visual
- Configurar la opacidad
- Cambiar la posición del elemento visual en la colección

## <a name="creating-a-compositor"></a>Crear un compositor

Crear un [**compositor**](/uwp/api/Windows.UI.Composition.Compositor) y almacenarlo en una variable para usarlo como un generador es una tarea sencilla. En el siguiente fragmento se muestra la creación de un nuevo objeto **Compositor**:

```cs
_compositor = new Compositor();
```

## <a name="creating-a-spritevisual-and-colorbrush"></a>Crear un objeto SpriteVisual y un objeto ColorBrush

Con el objeto [**Compositor**](/uwp/api/Windows.UI.Composition.Compositor), es fácil crear objetos cuando los necesites, tales como un objeto [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) y un objeto [**CompositionColorBrush**](/uwp/api/Windows.UI.Composition.CompositionColorBrush):

```cs
var visual = _compositor.CreateSpriteVisual();
visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
```

Aunque esto es solo unas pocas líneas de código, muestra un importante concepto: los objetos [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) son el corazón del sistema de efectos. El objeto **SpriteVisual** permite mucha flexibilidad e interacción en cuanto a la creación de colores, imágenes y efectos. El **SpriteVisual** es un tipo visual único que puede rellenar un rectángulo 2D con un pincel, en este caso, un color sólido.

## <a name="clipping-a-visual"></a>Recortar un elemento visual

El objeto [**Compositor**](/uwp/api/Windows.UI.Composition.Compositor) también se puede usar para crear recortes en un objeto [**Visual**](/uwp/api/Windows.UI.Composition.Visual). A continuación te mostramos un ejemplo de la muestra en la que se usa el objeto [**InsetClip**](/uwp/api/Windows.UI.Composition.InsetClip) para recortar cada lado del elemento visual:

```cs
var clip = _compositor.CreateInsetClip();
clip.LeftInset = 1.0f;
clip.RightInset = 1.0f;
clip.TopInset = 1.0f;
clip.BottomInset = 1.0f;
_currentVisual.Clip = clip;
```

Al igual que otros objetos de la API, [**InsetClip**](/uwp/api/Windows.UI.Composition.InsetClip) puede tener animaciones aplicadas a sus propiedades.

## <a name="span-idrotating_a_clipspanspan-idrotating_a_clipspanspan-idrotating_a_clipspanrotating-a-clip"></a><span id="Rotating_a_Clip"></span><span id="rotating_a_clip"></span><span id="ROTATING_A_CLIP"></span>Girar un recorte

Un objeto [**Visual**](/uwp/api/Windows.UI.Composition.Visual) se puede transformar con una rotación. Ten en cuenta que el objeto [**RotationAngle**](/uwp/api/windows.ui.composition.visual.rotationangle) admite tanto radianes como grados. El valor predeterminado es radianes, pero es fácil especificar grados, tal como se muestra en el siguiente fragmento de código:

```cs
child.RotationAngleInDegrees = 45.0f;
```

La rotación es tan solo un ejemplo de un conjunto de componentes de transformación que la API proporciona para facilitar estas tareas. Otros incluyen offset, Scale, Orientation, RotationAxis y 4x4 TransformMatrix.

## <a name="setting-opacity"></a>Configurar la opacidad

Configurar la opacidad de un elemento visual es una operación sencilla que usa un valor flotante. Por ejemplo, en la muestra, todos los cuadrados empiezan con una opacidad de 0,8:

```cs
visual.Opacity = 0.8f;
```

Como la rotación, la propiedad [**Opacity**](/uwp/api/windows.ui.composition.visual.opacity) se puede animar.

## <a name="changing-the-visuals-position-in-the-collection"></a>Cambiar la posición del elemento visual en la colección

La API de composición permite cambiar la posición de un visual en una [**VisualCollection**](/uwp/api/windows.ui.composition.visualcollection) de varias maneras. Se puede colocar sobre otro visual con [**InsertAbove**](/uwp/api/windows.ui.composition.visualcollection.insertabove), que se coloca a continuación con [**InsertBelow**](/uwp/api/windows.ui.composition.visualcollection.insertbelow), se mueve a la parte superior con [**InsertAtTop**](/uwp/api/windows.ui.composition.visualcollection.insertattop)o a la parte inferior con [**InsertAtBottom**](/uwp/api/windows.ui.composition.visualcollection.insertatbottom).

En el ejemplo, un [**Visual**](/uwp/api/Windows.UI.Composition.Visual) en el que se ha realizado clic se ordena en la parte superior:

```cs
parent.Children.InsertAtTop(_currentVisual);
```

## <a name="full-example"></a>Ejemplo completo

En la muestra completa, todos los conceptos anteriores se usan en conjunto para construir y recorrer un árbol simple de objetos [**Visual**](/uwp/api/Windows.UI.Composition.Visual) y cambiar la opacidad sin tener que usar XAML, WWA o DirectX. En esta muestra se indica cómo se crean y se agregan los objetos secundarios **Visual**, y cómo se cambian las propiedades.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Numerics;
using System.Text;
using System.Threading.Tasks;
using Windows.ApplicationModel.Core;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Core;

namespace compositionvisual
{
    class VisualProperties : IFrameworkView
    {
        //------------------------------------------------------------------------------
        //
        // VisualProperties.Initialize
        //
        // This method is called during startup to associate the IFrameworkView with the
        // CoreApplicationView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Initialize(CoreApplicationView view)
        {
            _view = view;
            _random = new Random();
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.SetWindow
        //
        // This method is called when the CoreApplication has created a new CoreWindow,
        // allowing the application to configure the window and start producing content
        // to display.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.SetWindow(CoreWindow window)
        {
            _window = window;
            InitNewComposition();
            _window.PointerPressed += OnPointerPressed;
            _window.PointerMoved += OnPointerMoved;
            _window.PointerReleased += OnPointerReleased;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerPressed
        //
        // This method is called when the user touches the screen, taps it with a stylus
        // or clicks the mouse.
        //
        //------------------------------------------------------------------------------

        void OnPointerPressed(CoreWindow window, PointerEventArgs args)
        {
            Point position = args.CurrentPoint.Position;

            //
            // Walk our list of visuals to determine who, if anybody, was selected
            //
            foreach (var child in _root.Children)
            {
                //
                // Did we hit this child?
                //
                Vector3 offset = child.Offset;
                Vector2 size = child.Size;

                if ((position.X >= offset.X) &&
                    (position.X < offset.X + size.X) &&
                    (position.Y >= offset.Y) &&
                    (position.Y < offset.Y + size.Y))
                {
                    //
                    // This child was hit. Since the children are stored back to front,
                    // the last one hit is the front-most one so it wins
                    //
                    _currentVisual = child as ContainerVisual;
                    _offsetBias = new Vector2((float)(offset.X - position.X),
                                              (float)(offset.Y - position.Y));
                }
            }

            //
            // If a visual was hit, bring it to the front of the Z order
            //
            if (_currentVisual != null)
            {
                ContainerVisual parent = _currentVisual.Parent as ContainerVisual;
                parent.Children.Remove(_currentVisual);
                parent.Children.InsertAtTop(_currentVisual);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerMoved
        //
        // This method is called when the user moves their finger, stylus or mouse with
        // a button pressed over the screen.
        //
        //------------------------------------------------------------------------------

        void OnPointerMoved(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual is selected, drag it with the pointer position and
            // make it opaque while we drag it
            //
            if (_currentVisual != null)
            {
                //
                // Set up the properties of the visual the first time it is
                // dragged. This will last for the duration of the drag
                //
                if (!_dragging)
                {
                    _currentVisual.Opacity = 1.0f;

                    //
                    // Transform the first child of the current visual so that
                    // the image is rotated
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngleInDegrees = 45.0f;
                        child.CenterPoint = new Vector3(_currentVisual.Size.X / 2, _currentVisual.Size.Y / 2, 0);
                        break;
                    }

                    //
                    // Clip the visual to its original layout rect by using an inset
                    // clip with a one-pixel margin all around
                    //
                    var clip = _compositor.CreateInsetClip();
                    clip.LeftInset = 1.0f;
                    clip.RightInset = 1.0f;
                    clip.TopInset = 1.0f;
                    clip.BottomInset = 1.0f;
                    _currentVisual.Clip = clip;

                    _dragging = true;
                }

                Point position = args.CurrentPoint.Position;
                _currentVisual.Offset = new Vector3((float)(position.X + _offsetBias.X),
                                                    (float)(position.Y + _offsetBias.Y),
                                                    0.0f);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerReleased
        //
        // This method is called when the user lifts their finger or stylus from the
        // screen, or lifts the mouse button.
        //
        //------------------------------------------------------------------------------

        void OnPointerReleased(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual was selected, make it transparent again when it is
            // released and restore the transform and clip
            //
            if (_currentVisual != null)
            {
                if (_dragging)
                {
                    //
                    // Remove the transform from the first child
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngle = 0.0f;
                        child.CenterPoint = new Vector3(0.0f, 0.0f, 0.0f);
                        break;
                    }

                    _currentVisual.Opacity = 0.8f;
                    _currentVisual.Clip = null;
                    _dragging = false;
                }

                _currentVisual = null;
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Load
        //
        // This method is called when a specific page is being loaded in the
        // application.  It is not used for this application.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Load(string unused)
        {

        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Run
        //
        // This method is called by CoreApplication.Run() to actually run the
        // dispatcher's message pump.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Run()
        {
            _window.Activate();
            _window.Dispatcher.ProcessEvents(CoreProcessEventsOption.ProcessUntilQuit);
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Uninitialize
        //
        // This method is called during shutdown to disconnect the CoreApplicationView,
        // and CoreWindow from the IFrameworkView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Uninitialize()
        {
            _window = null;
            _view = null;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.InitNewComposition
        //
        // This method is called by SetWindow(), where we initialize Composition after
        // the CoreWindow has been created.
        //
        //------------------------------------------------------------------------------

        void InitNewComposition()
        {
            //
            // Set up Windows.UI.Composition Compositor, root ContainerVisual, and associate with
            // the CoreWindow.
            //

            _compositor = new Compositor();

            _root = _compositor.CreateContainerVisual();



            _compositionTarget = _compositor.CreateTargetForCurrentView();
            _compositionTarget.Root = _root;

            //
            // Create a few visuals for our window
            //
            for (int index = 0; index < 20; index++)
            {
                _root.Children.InsertAtTop(CreateChildElement());
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.CreateChildElement
        //
        // Creates a small sub-tree to represent a visible element in our application.
        //
        //------------------------------------------------------------------------------

        Visual CreateChildElement()
        {
            //
            // Each element consists of three visuals, which produce the appearance
            // of a framed rectangle
            //
            var element = _compositor.CreateContainerVisual();
            element.Size = new Vector2(100.0f, 100.0f);

            //
            // Position this visual randomly within our window
            //
            element.Offset = new Vector3((float)(_random.NextDouble() * 400), (float)(_random.NextDouble() * 400), 0.0f);

            //
            // The outer rectangle is always white
            //
            //Note to preview API users - SpriteVisual and Color Brush replace SolidColorVisual
            //for example instead of doing
            //var visual = _compositor.CreateSolidColorVisual() and
            //visual.Color = Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF);
            //we now use the below

            var visual = _compositor.CreateSpriteVisual();
            element.Children.InsertAtTop(visual);
            visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
            visual.Size = new Vector2(100.0f, 100.0f);

            //
            // The inner rectangle is inset from the outer by three pixels all around
            //
            var child = _compositor.CreateSpriteVisual();
            visual.Children.InsertAtTop(child);
            child.Offset = new Vector3(3.0f, 3.0f, 0.0f);
            child.Size = new Vector2(94.0f, 94.0f);

            //
            // Pick a random color for every rectangle
            //
            byte red = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte green = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte blue = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            child.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, red, green, blue));

            //
            // Make the subtree root visual partially transparent. This will cause each visual in the subtree
            // to render partially transparent, since a visual's opacity is multiplied with its parent's
            // opacity
            //
            element.Opacity = 0.8f;

            return element;
        }

        // CoreWindow / CoreApplicationView
        private CoreWindow _window;
        private CoreApplicationView _view;

        // Windows.UI.Composition
        private Compositor _compositor;
        private CompositionTarget _compositionTarget;
        private ContainerVisual _root;
        private ContainerVisual _currentVisual;
        private Vector2 _offsetBias;
        private bool _dragging;

        // Helpers
        private Random _random;
    }


    public sealed class VisualPropertiesFactory : IFrameworkViewSource
    {
        //------------------------------------------------------------------------------
        //
        // VisualPropertiesFactory.CreateView
        //
        // This method is called by CoreApplication to provide a new IFrameworkView for
        // a CoreWindow that is being created.
        //
        //------------------------------------------------------------------------------

        IFrameworkView IFrameworkViewSource.CreateView()
        {
            return new VisualProperties();
        }


        //------------------------------------------------------------------------------
        //
        // main
        //
        //------------------------------------------------------------------------------

        static int Main(string[] args)
        {
            CoreApplication.Run(new VisualPropertiesFactory());

            return 0;
        }
    }
}
```