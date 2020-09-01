---
Description: Cree aplicaciones de Windows que admitan interacciones personalizadas de los dispositivos de lápiz y lápiz, incluida la entrada de lápiz digital para experiencias naturales de escritura y dibujo.
title: Interacciones de lápiz y Windows Ink en aplicaciones de Windows
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen interactions and Windows Ink in Windows apps
template: detail.hbs
keywords: Windows Ink, entrada manuscrita de Windows, DirectInk, InkPresenter, InkCanvas, reconocimiento de escritura a mano, interacción del usuario, entrada
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3733c98c81a23fbc5b369297b45f1c1e5183c198
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173389"
---
# <a name="pen-interactions-and-windows-ink-in-windows-apps"></a>Interacciones de lápiz y Windows Ink en aplicaciones de Windows

![Lápiz para Surface](images/ink/hero-small.png)  
*Lápiz para Surface* (disponible para su compra en [Microsoft Store](https://www.microsoft.com/p/surface-pen/8zl5c82qmg6b)).

## <a name="overview"></a>Información general

Optimice su aplicación de Windows para que la entrada manuscrita proporcione la funcionalidad de [**dispositivo de puntero**](/uwp/api/Windows.Devices.Input.PointerDevice) estándar y la mejor experiencia de Windows Ink para los usuarios.

> [!NOTE]
> Este tema se centra en la plataforma Windows Ink. Para poder controlar las entradas generales del puntero (es algo similar al mouse, a la función táctil y al panel táctil), consulta el artículo [Controlar la entrada de puntero](handle-pointer-input.md).

| Vídeos |   |
| --- | --- |
| <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Ink-in-Your-UWP-App/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Events/Ignite/2016/BRK2060/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> |
| *Uso de entradas manuscritas en la aplicación de Windows* | *Uso del lápiz de Windows y Windows Ink para crear aplicaciones empresariales más atractivas* |

La plataforma de Windows Ink, junto con un dispositivo de lápiz, te ofrece una forma natural de crear notas, dibujos y anotaciones manuscritas, todo ello de forma digital. Asimismo, la plataforma te permite capturar los datos de entrada del digitalizador a modo de datos de entrada de lápiz, generar datos de entrada de lápiz, administrarlos, representarlos como trazos de lápiz en el dispositivo de salida y convertirlos en texto a través del reconocimiento de escritura a mano.

Además de capturar la posición básica y el movimiento del lápiz a medida que el usuario escribe o dibuja, la aplicación también puede realizar un seguimiento y recopilar los distintos niveles de presión que se usan en un trazo. Esta información, junto con la configuración de la forma de la punta del lápiz, el tamaño y la rotación, el color de la entrada de lápiz y el propósito (como la entrada de lápiz sin formato y las acciones de borrar, resaltar y seleccionar), te permite proporcionar experiencias de usuario que se asemejan mucho a escribir o dibujar sobre papel con un lápiz, un bolígrafo o un pincel.

> [!NOTE]
> La aplicación también es compatible con la entrada de lápiz de otros dispositivos señaladores, como digitalizadores táctiles y dispositivos de mouse. 

La plataforma de entrada de lápiz es muy flexible. Está diseñada para admitir varios niveles de funcionalidad, dependiendo de los requisitos.

Para obtener directrices sobre la experiencia del usuario de Windows Ink, consulta [Controles de entrada de lápiz](../controls-and-patterns/inking-controls.md).

## <a name="components-of-the-windows-ink-platform"></a>Componentes de la plataforma Windows Ink

| Componente | Descripción |
| --- | --- |
| [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) | Un control de plataforma de interfaz de usuario XAML que, de manera predeterminada, recibe y muestra todas las entradas de lápiz como un trazo de lápiz o un trazo de borrado.<br/>Para obtener más información sobre cómo usar InkCanvas, consulta [Reconocer trazos de Windows Ink como texto](convert-ink-to-text.md) y [Almacenar y recuperar datos de trazos de lápiz de Windows Ink](save-and-load-ink.md). |
| [**Objeto**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) | Un objeto de código subyacente, cuya instancia se creó con un control [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) (expuesto a través de la propiedad [**InkCanvas.InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)). Este objeto proporciona todas las funcionalidades de entrada manuscrita predeterminadas y que expuso **InkCanvas**, junto con un completo conjunto de API para la personalización adicional.<br/>Para obtener más información sobre cómo usar InkPresenter, consulta [Reconocer trazos de Windows Ink como texto](convert-ink-to-text.md) y [Almacenar y recuperar datos de trazos de lápiz de Windows Ink](save-and-load-ink.md). |
| [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) | Control de plataforma de interfaz de usuario XAML que contiene una colección personalizable y extensible de botones que activan las características relacionadas con la tinta en un [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas)asociado.<br/>Para obtener más información sobre cómo usar el control InkToolbar, vea [Agregar un control inktoolbar a una aplicación de entrada de lápiz de la aplicación Windows](ink-toolbar.md). |
| [**IInkD2DRenderer**](/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer) | Habilita la representación de trazos de lápiz en el contexto de dispositivo de Direct2D designado de una aplicación universal de Windows, en lugar del control [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) predeterminado. Esto permite la personalización completa de la experiencia de entrada manuscrita.<br/>Para obtener más información, consulta [Complex ink sample (Muestra de entrada de lápiz compleja)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk). |

## <a name="basic-inking-with-inkcanvas"></a>Entrada manuscrita básica con InkCanvas

Para agregar la funcionalidad básica de entrada manuscrita, solo tiene que colocar un control de plataforma de UWP de [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) en la página correspondiente de la aplicación.

De manera predeterminada, el control [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) solo admite la entrada de lápiz que se haya realizado con un lápiz. La entrada se representa como un trazo de lápiz mediante la configuración predeterminada de color y espesor (como un bolígrafo negro con un grosor de 2 píxeles) o se trata como si fuera un borrador de trazos (cuando la entrada se realiza desde un extremo del borrador o cuando la punta del lápiz se modifica con un botón de borrador).

> [!NOTE]
> Si no hay ningún botón o extremo borrador, InkCanvas puede configurarse para procesar la entrada de la punta del lápiz como trazos de borrado.

En este ejemplo, un control [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) se superpone a una imagen en segundo plano.

> [!NOTE]
> Un control InkCanvas tiene las propiedades [**height**](/uwp/api/windows.ui.xaml.frameworkelement.Height) y [**width**](/uwp/api/windows.ui.xaml.frameworkelement.Width) predeterminadas de cero, a menos que sea el elemento secundario de un elemento que ajusta automáticamente el tamaño de sus elementos secundarios, como los controles [StackPanel](/uwp/api/windows.ui.xaml.controls.stackpanel) o [Grid](/uwp/api/windows.ui.xaml.controls.grid) .

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />            
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

Esta serie de imágenes muestra cómo se representa la entrada manuscrita mediante el control [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

| ![Control InkCanvas en blanco con una imagen de fondo](images/ink_basic_1_small.png) | ![Control InkCanvas con trazos de lápiz](images/ink_basic_2_small.png) | ![Control InkCanvas con un trazo borrado](images/ink_basic_3_small.png) |
| --- | --- | ---|
| El [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) en blanco con una imagen de fondo. | [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) con trazos de lápiz. | Control [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) con un trazo borrado (ten en cuenta que la opción de borrado actúa en todo el trazo, no en una parte). |

La funcionalidad de entrada manuscrita admitida por el control [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) la proporciona un objeto de código subyacente denominado [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter).

Para realizar entradas manuscritas básicas, no es necesario preocuparse por [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter). Sin embargo, para personalizar y configurar el comportamiento de la entrada manuscrita en la clase [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas), debe acceder al objeto correspondiente **InkPresenter**.

## <a name="basic-customization-with-inkpresenter"></a>Personalización básica con InkPresenter

Se crea una instancia de un objeto [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) con cada control [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

> [!NOTE]
> No se pueden crear instancias de la propiedad [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) directamente. En su lugar, debes acceder a ellas a través de la propiedad [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) de la clase [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas). 

Además de proporcionar todos los comportamientos predeterminados de entrada de lápiz de su control [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) correspondiente, el [**objeto InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) proporciona un conjunto completo de API para la personalización de trazos adicionales y la administración más específica de la entrada manuscrita (estándar y modificado). Esto incluye las propiedades de trazo, los tipos de dispositivo de entrada admitidos y si el objeto procesa la entrada o se pasa a la aplicación para su procesamiento.

> [!NOTE]
> La entrada manuscrita estándar (desde la punta del lápiz o el botón o la punta del borrador) no se modifica con una prestación de hardware secundaria, como un botón de barril del lápiz, un botón secundario del mouse o un mecanismo similar. 

De forma predeterminada, la tinta solo se admite para la entrada manuscrita. En este apartado, configuraremos [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) para que interprete los datos de entrada del lápiz y el mouse como trazos de lápiz. También establecemos algunos atributos de trazo de lápiz iniciales que se usan para representar trazos en el [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

Para habilitar el mouse y la entrada táctil, establezca la propiedad [**InputDeviceTypes**](/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) del [**objeto InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) en la combinación de los valores de [**CoreInputDeviceTypes**](/uwp/api/windows.ui.core.coreinputdevicetypes) que desee.

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Set supported inking device types.
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse |
        Windows.UI.Core.CoreInputDeviceTypes.Pen;

    // Set initial ink stroke attributes.
    InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
    drawingAttributes.Color = Windows.UI.Colors.Black;
    drawingAttributes.IgnorePressure = false;
    drawingAttributes.FitToCurve = true;
    inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
}
```

Los atributos de trazo de lápiz se pueden establecer de forma dinámica para así ajustarlos a las preferencias del usuario o a los requisitos de la aplicación.

Aquí dejamos que un usuario elija en una lista de colores de entrada de lápiz.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink customization sample"
                   VerticalAlignment="Center"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
        <TextBlock Text="Color:"
                   Style="{StaticResource SubheaderTextBlockStyle}"
                   VerticalAlignment="Center"
                   Margin="50,0,10,0"/>
        <ComboBox x:Name="PenColor"
                  VerticalAlignment="Center"
                  SelectedIndex="0"
                  SelectionChanged="OnPenColorChanged">
            <ComboBoxItem Content="Black"/>
            <ComboBoxItem Content="Red"/>
        </ComboBox>
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

A continuación, controlamos los cambios en el color seleccionado y actualizamos los atributos de trazo de lápiz en consecuencia.

```csharp
// Update ink stroke color for new strokes.
private void OnPenColorChanged(object sender, SelectionChangedEventArgs e)
{
    if (inkCanvas != null)
    {
        InkDrawingAttributes drawingAttributes =
            inkCanvas.InkPresenter.CopyDefaultDrawingAttributes();

        string value = ((ComboBoxItem)PenColor.SelectedItem).Content.ToString();

        switch (value)
        {
            case "Black":
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
            case "Red":
                drawingAttributes.Color = Windows.UI.Colors.Red;
                break;
            default:
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
        };

        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
    }
}
```

Estas imágenes muestran cómo se procesa y personaliza la entrada manuscrita mediante [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter).

| ![control inkcanvas con trazos de lápiz negro predeterminados](images/ink-basic-custom-1-small.png) | ![control inkcanvas con trazos de lápiz rojo seleccionados por el usuario](images/ink-basic-custom-2-small.png) |
| --- | --- |
| [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) con trazos de lápiz negros predeterminados. | [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) con trazos de tinta roja seleccionados por el usuario. | 

Para proporcionar funcionalidades que vayan más allá de la entrada manuscrita y el borrado (como la selección de trazo), la aplicación debe identificar la entrada específica de [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter), la cual debe pasarse sin procesar para que la aplicación la controle.

## <a name="pass-through-input-for-advanced-processing"></a>Entrada de paso a través para el procesamiento avanzado

De forma predeterminada, [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) procesa toda la entrada como un trazo de tinta o un trazo de borrado, incluida la entrada modificada por una prestación de hardware secundaria, como un botón de barril del lápiz, un botón secundario del mouse o similar. Sin embargo, los usuarios normalmente esperan alguna funcionalidad adicional o un comportamiento modificado con estas prestaciones secundarias.

En algunos casos, es posible que también necesite exponer funcionalidad adicional para los lápices sin prestaciones secundarias (funcionalidad que normalmente no está asociada a la punta del lápiz), otros tipos de dispositivos de entrada o algún tipo de comportamiento modificado basado en una selección de usuario en la interfaz de usuario de la aplicación.

Para que esta acción sea compatible, puedes configurar [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) para que deje una entrada específica sin procesar. A continuación, esta entrada sin procesar se pasa a través de la aplicación para su procesamiento.

### <a name="example---use-unprocessed-input-to-implement-stroke-selection"></a>Ejemplo: uso de la entrada no procesada para implementar la selección de trazos 

La plataforma de tinta de Windows no proporciona compatibilidad integrada para acciones que requieren una entrada modificada, como la selección de trazo. Para admitir características como esta, debe proporcionar una solución personalizada en sus aplicaciones. 

En el ejemplo de código siguiente (todo el código está en los archivos MainPage. XAML y MainPage.xaml.cs), se explica cómo habilitar la selección de trazos cuando la entrada se modifica con un botón de barril del lápiz (o el botón secundario del mouse).

1.  En primer lugar, debemos configurar la interfaz de usuario en MainPage.xaml.

    Una vez hecho esto, agregamos un lienzo (debajo de [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)) para dibujar el trazo de selección. Si usas una capa independiente para dibujar el trazo de selección, **InkCanvas** y su contenido no se modifican.

    ![inkcanvas en blanco con un lienzo de selección subyacente](images/ink-unprocessed-1-small.png)

      ```xaml
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
          </Grid.RowDefinitions>
          <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header"
              Text="Advanced ink customization sample"
              VerticalAlignment="Center"
              Style="{ThemeResource HeaderTextBlockStyle}"
              Margin="10,0,0,0" />
          </StackPanel>
          <Grid Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
          </Grid>
        </Grid>
      ```

2.  En MainPage.xaml.cs, declaramos un par de variables globales para mantener las referencias a los aspectos de la interfaz de usuario de selección. Usaremos específicamente el trazo de lazo de selección y el rectángulo delimitador que resalta los trazos seleccionados.

      ```csharp
        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;
      ```

3.  A continuación, configuraremos el [**objeto InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) para interpretar los datos de entrada del lápiz y del mouse como trazos de entrada de lápiz, y para establecer algunos atributos de trazo de lápiz iniciales que se usan para representar trazos en el [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

    Recuerda usar la propiedad [**InputProcessingConfiguration**](/uwp/api/windows.ui.input.inking.inkpresenter.inputprocessingconfiguration) de [InkPresenter](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) para indicar que la aplicación debe procesar cualquier entrada modificada. La entrada modificada se especifica asignando **InputProcessingConfiguration. RightDragAction** a [**InkInputRightDragAction. LeaveUnprocessed**](/uwp/api/Windows.UI.Input.Inking.InkInputRightDragAction). Cuando se establece este valor, el [objeto InkPresenter](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) pasa a la clase [InkUnprocessedInput](/uwp/api/windows.ui.input.inking.inkunprocessedinput) , un conjunto de eventos de puntero para que los controle.

    Asignamos agentes de escucha para los eventos [**PointerPressed**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerpressed), [**PointerMoved**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointermoved)y [**PointerReleased**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerreleased) sin procesar que ha pasado el [**objeto InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter). Todas las funcionalidades de selección se implementan en los controladores de estos eventos.

    Por último, asignamos agentes de escucha para los eventos [**StrokeStarted**](/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted) y [**StrokesErased**](/uwp/api/windows.ui.input.inking.inkpresenter.strokeserased) de la propiedad [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter). Los controladores de estos eventos se usan para limpiar la interfaz de usuario de selección si se inicia un nuevo trazo o se borra un trazo existente.

    ![control inkcanvas con trazos de lápiz negro predeterminados](images/ink-unprocessed-2-small.png)

      ```csharp
        public MainPage()
        {
          this.InitializeComponent();

          // Set supported inking device types.
          inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

          // Set initial ink stroke attributes.
          InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
          drawingAttributes.Color = Windows.UI.Colors.Black;
          drawingAttributes.IgnorePressure = false;
          drawingAttributes.FitToCurve = true;
          inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

          // By default, the InkPresenter processes input modified by
          // a secondary affordance (pen barrel button, right mouse
          // button, or similar) as ink.
          // To pass through modified input to the app for custom processing
          // on the app UI thread instead of the background ink thread, set
          // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
          inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
              InkInputRightDragAction.LeaveUnprocessed;

          // Listen for unprocessed pointer events from modified input.
          // The input is used to provide selection functionality.
          inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
              UnprocessedInput_PointerPressed;
          inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
              UnprocessedInput_PointerMoved;
          inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
              UnprocessedInput_PointerReleased;

          // Listen for new ink or erase strokes to clean up selection UI.
          inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
              StrokeInput_StrokeStarted;
          inkCanvas.InkPresenter.StrokesErased +=
              InkPresenter_StrokesErased;
        }
      ```

4.  A continuación, se definen Controladores para los eventos [**PointerPressed**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerpressed), [**PointerMoved**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointermoved)y [**PointerReleased**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerreleased) sin procesar que ha pasado el [**objeto InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter).

    Todas las funcionalidades de selección se implementan en estos controladores, incluido el trazo de lazo y el rectángulo delimitador.

    ![lazo de selección](images/ink-unprocessed-3-small.png)

      ```csharp
        // Handle unprocessed pointer events from modified input.
        // The input is used to provide selection functionality.
        // Selection UI is drawn on a canvas under the InkCanvas.
        private void UnprocessedInput_PointerPressed(
          InkUnprocessedInput sender, PointerEventArgs args)
        {
          // Initialize a selection lasso.
          lasso = new Polyline()
          {
              Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
              StrokeThickness = 1,
              StrokeDashArray = new DoubleCollection() { 5, 2 },
              };

              lasso.Points.Add(args.CurrentPoint.RawPosition);

              selectionCanvas.Children.Add(lasso);
          }

          private void UnprocessedInput_PointerMoved(
            InkUnprocessedInput sender, PointerEventArgs args)
          {
            // Add a point to the lasso Polyline object.
            lasso.Points.Add(args.CurrentPoint.RawPosition);
          }

          private void UnprocessedInput_PointerReleased(
            InkUnprocessedInput sender, PointerEventArgs args)
          {
            // Add the final point to the Polyline object and
            // select strokes within the lasso area.
            // Draw a bounding box on the selection canvas
            // around the selected ink strokes.
            lasso.Points.Add(args.CurrentPoint.RawPosition);

            boundingRect =
              inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                lasso.Points);

            DrawBoundingRect();
          }
      ```

5.  Para concluir el controlador de eventos PointerReleased, borramos la capa de selección de todo el contenido (trazo de lazo) y, a continuación, dibujamos un único rectángulo delimitador alrededor de los trazos de lápiz incluidos en el área del lazo.

    ![rectángulo delimitador de selección](images/ink-unprocessed-4-small.png)

      ```csharp
        // Draw a bounding rectangle, on the selection canvas, encompassing
        // all ink strokes within the lasso area.
        private void DrawBoundingRect()
        {
          // Clear all existing content from the selection canvas.
          selectionCanvas.Children.Clear();

          // Draw a bounding rectangle only if there are ink strokes
          // within the lasso area.
          if (!((boundingRect.Width == 0) ||
            (boundingRect.Height == 0) ||
            boundingRect.IsEmpty))
            {
              var rectangle = new Rectangle()
              {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                  StrokeThickness = 1,
                  StrokeDashArray = new DoubleCollection() { 5, 2 },
                  Width = boundingRect.Width,
                  Height = boundingRect.Height
              };

              Canvas.SetLeft(rectangle, boundingRect.X);
              Canvas.SetTop(rectangle, boundingRect.Y);

              selectionCanvas.Children.Add(rectangle);
            }
          }
      ```

6.  Por último, definimos los controladores de los eventos [**StrokeStarted**](/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted) y [**StrokesErased**](/uwp/api/windows.ui.input.inking.inkpresenter.strokeserased) de la propiedad InkPresenter.

    Ambos llaman a la misma función de limpieza para borrar la selección actual siempre que se detecta un nuevo trazo.

      ```csharp
        // Handle new ink or erase strokes to clean up selection UI.
        private void StrokeInput_StrokeStarted(
          InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
        {
          ClearSelection();
        }

        private void InkPresenter_StrokesErased(
          InkPresenter sender, InkStrokesErasedEventArgs args)
        {
          ClearSelection();
        }
      ```

7.  Esta es la función para quitar toda la interfaz de usuario de selección del lienzo de selección cuando se inicia un nuevo trazo o se borra un trazo existente.

      ```csharp
        // Clean up selection UI.
        private void ClearSelection()
        {
          var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
          foreach (var stroke in strokes)
          {
            stroke.Selected = false;
          }
          ClearDrawnBoundingRect();
         }

        private void ClearDrawnBoundingRect()
        {
          if (selectionCanvas.Children.Any())
          {
            selectionCanvas.Children.Clear();
            boundingRect = Rect.Empty;
          }
        }
      ```

## <a name="custom-ink-rendering"></a>Representación de entrada de lápiz personalizada

De forma predeterminada, la entrada de lápiz se procesa en un subproceso en segundo plano de latencia baja y se representa en curso, o "húmeda", cuando se dibuja. Cuando se completa el trazo (se levanta el lápiz o el dedo o se libera el botón del mouse), el trazo se procesa en el subproceso de la interfaz de usuario y se representa como "seco" en la capa de [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) (lo verás sobre el contenido de la aplicación y reemplazando la entrada de lápiz húmeda).

Puede invalidar este comportamiento predeterminado y controlar por completo la experiencia de entrada manuscrita mediante el "secado personalizado" de los trazos de la tinta húmeda. Aunque el comportamiento predeterminado suele ser suficiente para la mayoría de las aplicaciones, hay algunos casos en los que es posible que se requiera un secado personalizado, entre los que se incluyen:
- Administración más eficaz de colecciones grandes o complejas de trazos de tinta
- Compatibilidad más eficaz de movimiento panorámico y zoom en lienzos de tinta grandes
- Intercalar la entrada de lápiz y otros objetos, como formas o texto, manteniendo el orden z 
- Secado y conversión de la entrada de lápiz sincrónicamente en una forma de DirectX (por ejemplo, una línea o forma recta rasterizada e integrada en el contenido de la aplicación en lugar de como una capa de **InkCanvas** independiente).

El secado personalizado requiere un objeto [**IInkD2DRenderer**](/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer) para administrar la entrada de lápiz y representarla en el contexto de dispositivo Direct2D de la aplicación universal de Windows, en lugar del control [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) predeterminado.

Al llamar a [**ActivateCustomDrying**](/uwp/api/windows.ui.input.inking.inkpresenter.activatecustomdrying) (antes de cargar [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)), una aplicación crea un objeto [**InkSynchronizer**](/uwp/api/Windows.UI.Input.Inking.InkSynchronizer) para personalizar la representación de un trazo de lápiz seco en la clase [**SurfaceImageSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) o [**VirtualSurfaceImageSource**](/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource). 

Los objetos [**SurfaceImageSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) y [**VirtualSurfaceImageSource**](/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource) proporcionan una superficie compartida de DirectX para que la aplicación se dibuje en el contenido de la aplicación y se componga en él, aunque VSI proporciona una superficie virtual más grande que la pantalla para la panorámica y el aumento del rendimiento. Dado que las actualizaciones visuales de estas superficies se sincronizan con el subproceso de la interfaz de usuario de XAML, cuando la entrada de lápiz se representa en cualquiera de ellos, la tinta húmeda se puede quitar del InkCanvas simultáneamente. 

También puede personalizar la tinta seca para un [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel), pero no se garantiza la sincronización con el subproceso de la interfaz de usuario y puede haber un retraso entre el momento en que se representa la tinta en el swapchainpanel y el momento en que se quita la tinta del InkCanvas.

Para obtener un ejemplo completo de esta funcionalidad, consulta la [muestra de entrada de lápiz compleja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk).

> [!NOTE]
> El secado personalizado y la clase [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar)  
> Si tu aplicación invalida el comportamiento predeterminado de representación de entrada de lápiz de la clase [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) con una implementación de secado personalizado, los trazos de lápiz representados ya no están disponibles para InkToolbar y los comandos de borrado integrados de InkToolbar no funcionan según lo previsto. Para proporcionar la funcionalidad de borrado, debes controlar todos los eventos de puntero, realizar la prueba de posicionamiento en cada trazo e invalidar el comando "Borrar todas las entradas de lápiz" integrado.

## <a name="other-articles-in-this-section"></a>Otros artículos de esta sección

| Tema | Descripción |
| --- | --- |
| [Reconocer trazos de lápiz](convert-ink-to-text.md) | Conversión de trazos de lápiz en texto mediante el reconocimiento de escritura a mano o en formas mediante el reconocimiento personalizado |
| [Almacenar y recuperar trazos de lápiz](save-and-load-ink.md) | Almacena los datos de trazo de lápiz en un archivo de formato de intercambio de elementos gráficos (GIF) mediante los metadatos Ink Serialized Format (ISF) insertados. |
| [Agregar un control InkToolbar a una aplicación de entrada manuscrita de Windows](ink-toolbar.md) | Agregue un control InkToolbar predeterminado a una aplicación de entrada manuscrita de la aplicación de Windows, agregue un botón de lápiz personalizado al control InkToolbar y enlace el botón del lápiz personalizado a una definición de lápiz personalizada. |

## <a name="related-articles"></a>Artículos relacionados

- [Introducción: compatibilidad con la entrada manuscrita en la aplicación de Windows](./ink-walkthrough.md)
- [Control de la entrada con puntero](handle-pointer-input.md)
- [Identificación de dispositivos de entrada](identify-input-devices.md)

### <a name="apis"></a>API existentes

- [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input)
- [**Windows.UI.Input.Inking**](/uwp/api/Windows.UI.Input.Inking)
- [**Windows. UI. Input. inking. Core**](/uwp/api/Windows.UI.Input.Inking.Core)

### <a name="samples"></a>Ejemplos

- [Tutorial de introducción: compatibilidad con la entrada de lápiz en la aplicación de Windows](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [Ejemplo de entrada de lápiz simple (C#/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [Ejemplo de tinta compleja (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [Ejemplo de entrada de lápiz (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [Muestra de libro para colorear](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Muestra de notas familiares](https://github.com/Microsoft/Windows-appsample-familynotes)
- [Ejemplo de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Ejemplo de entrada de latencia baja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Ejemplo de modo de interacción del usuario](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Muestras de archivo

- [Entrada: muestra de funcionalidades del dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrada: muestra de eventos de entrada de usuario de XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Ejemplo de desplazamiento, panorámica y zoom de XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrada: gestos y manipulaciones con GestureRecognizer](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)