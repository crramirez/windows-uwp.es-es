---
author: jwmsft
ms.assetid: f1297b7d-1a10-52ae-dd84-6d1ad2ae2fe6
title: Elemento visual de composición
description: Los elementos visuales de composición conforman la estructura del árbol visual que usan todas las demás funciones de la API de composición y sobre la cual construyen. La API permite a los desarrolladores definir y crear uno o varios objetos visuales que representan un nodo único en un árbol visual.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8e43d5697a8fe4536e3574d93bda5836133ec8a8
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2018
ms.locfileid: "6280740"
---
# <a name="composition-visual"></a>Elemento visual de composición

Los elementos visuales de composición conforman la estructura del árbol visual que usan todas las demás funciones de la API de composición y sobre la cual construyen. La API permite a los desarrolladores definir y crear uno o varios objetos visuales que representan un nodo único en un árbol visual.

## <a name="visuals"></a>Elementos visuales

Existen tres tipos de elementos visuales que conforman la estructura del árbol visual, además de una clase de pincel base con varias subclases que afectan al contenido de un elemento visual:

- [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858): objeto base, la mayoría de las propiedades están aquí y las heredan los otros objetos Visual.
- [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810): se deriva del objeto [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) y agrega la capacidad de crear elementos secundarios.
- [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433): se deriva del objeto [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) y agrega la capacidad de asociar un pincel, de modo que el elemento Visual pueda representar píxeles, incluyendo imágenes, efectos o un color sólido.

Puedes aplicar contenido y efectos a SpriteVisuals mediante el objeto [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589398) y sus subclases, incluyendo [**CompositionColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush), [**CompositionSurfaceBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) y [**CompositionEffectBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush). Para más información sobre los pinceles, consulta nuestra [**Introducción a CompositionBrush**](https://docs.microsoft.com/windows/uwp/composition/composition-brushes).

## <a name="the-compositionvisual-sample"></a>Muestra de CompositionVisual

A continuación, veremos algunos códigos de muestra que permiten realizar los tres tipos visuales diferentes enumerados anteriormente. Si bien en esta muestra no se abarcan conceptos como, por ejemplo, animaciones o efectos más complejos, se incluyen los elementos esenciales que todos los sistemas usan. El código completo para este ejemplo se muestra al final de este artículo.

En el ejemplo, hay un número de cuadrados de color sólido en los que se puede hacer clic y que se pueden arrastrar sobre la pantalla. Cuando se hace clic en un cuadrado, este se trae al frente, se gira 45 grados y se vuelve opaco cuando se arrastra.

Esto muestra varios de los conceptos básicos para trabajar con la API, entre los que se incluye lo siguiente:

- Creación de un compositor
- Crear un objeto SpriteVisual con un objeto CompositionColorBrush
- Recortar un elemento visual
- Girar un elemento visual
- Configuración de la opacidad
- Cambiar la posición del elemento visual en la colección

## <a name="creating-a-compositor"></a>Creación de un compositor

Creación de un [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) y almacenamiento del mismo en una variable para usarlo como fábrica es una tarea sencilla. En el siguiente fragmento se muestra la creación de un nuevo objeto **Compositor**:

```cs
_compositor = new Compositor();
```

## <a name="creating-a-spritevisual-and-colorbrush"></a>Crear un objeto SpriteVisual y un objeto ColorBrush

Con el objeto [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789), es fácil crear objetos cuando los necesites, como un objeto [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) o un objeto [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399):

```cs
var visual = _compositor.CreateSpriteVisual();
visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
```

Si bien solo hay unas pocas líneas de código, se muestra un concepto muy eficaz, los objetos [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) son el corazón del sistema de efectos. Los objetos **SpriteVisual** permiten mucha flexibilidad e interacción en cuanto a la creación de colores, imágenes y efectos. El objeto **SpriteVisual** es un tipo visual único que puede rellenar un rectángulo 2D con un pincel; en este caso, un color sólido.

## <a name="clipping-a-visual"></a>Recortar un elemento visual

El objeto [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) también se puede usar para crear recortes en un objeto [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858). A continuación te mostramos un ejemplo de la muestra en la que se usa el objeto [**InsetClip**](https://msdn.microsoft.com/library/windows/apps/Dn706825) para recortar cada lado del elemento visual:

```cs
var clip = _compositor.CreateInsetClip();
clip.LeftInset = 1.0f;
clip.RightInset = 1.0f;
clip.TopInset = 1.0f;
clip.BottomInset = 1.0f;
_currentVisual.Clip = clip;
```

Al igual que otros objetos de la API, se pueden aplicar animaciones a las propiedades del objeto [**InsetClip**](https://msdn.microsoft.com/library/windows/apps/Dn706825).

## <a name="span-idrotatingaclipspanspan-idrotatingaclipspanspan-idrotatingaclipspanrotating-a-clip"></a><span id="Rotating_a_Clip"></span><span id="rotating_a_clip"></span><span id="ROTATING_A_CLIP"></span>Giro de un recorte

Un objeto [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) se puede transformar con una rotación. Ten en cuenta que el objeto [**RotationAngle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.rotationangle) admite tanto radianes como grados. El valor predeterminado es radianes, pero es fácil especificar grados, tal como se muestra en el siguiente fragmento de código:

```cs
child.RotationAngleInDegrees = 45.0f;
```

La rotación es tan solo un ejemplo de un conjunto de componentes de transformación que la API proporciona para facilitar estas tareas. Entre otras se incluyen Offset, Scale, Orientation, RotationAxis y 4x4 TransformMatrix.

## <a name="setting-opacity"></a>Configuración de la opacidad

Configurar la opacidad de un elemento visual es una operación sencilla que usa un valor flotante. Por ejemplo, en la muestra, todos los cuadrados empiezan con una opacidad de 0,8:

```cs
visual.Opacity = 0.8f;
```

Como la rotación, la propiedad [**Opacity**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.opacity) se puede animar.

## <a name="changing-the-visuals-position-in-the-collection"></a>Cambiar la posición del elemento visual en la colección

La API de composición permite cambiar de diferentes maneras la posición de un elemento [**VisualCollection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection). Se puede colocar sobre otro elemento Visual con [**InsertAbove**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertabove), colocar por debajo con [**InsertBelow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertbelow), moverse a la parte superior con [**InsertAtTop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertattop) o a la parte inferior con [**InsertAtBottom**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertatbottom).

En la muestra, un objeto [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) en el que se hizo clic se coloca en la parte superior:

```cs
parent.Children.InsertAtTop(_currentVisual);
```

## <a name="full-example"></a>Ejemplo completo

En la muestra completa, todos los conceptos anteriores se usan en conjunto para construir y recorrer un árbol simple de objetos [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) y cambiar la opacidad sin tener que usar XAML, WWA o DirectX. En esta muestra se indica cómo se crean y se agregan los objetos secundarios **Visual**, y cómo se cambian las propiedades.

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
