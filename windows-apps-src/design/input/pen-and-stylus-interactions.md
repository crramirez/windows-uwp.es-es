---
Description: Crea aplicaciones para la Plataforma universal de Windows (UWP) que admitan interacciones personalizadas con los dispositivos de lápiz y pluma, incluida la entrada de lápiz digital, para ofrecer experiencias de escritura y dibujo naturales.
title: Interacciones de lápiz y Windows Ink en aplicaciones para UWP
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen interactions and Windows Ink in UWP apps
template: detail.hbs
keywords: Windows Ink, entrada manuscrita de Windows, DirectInk, InkPresenter, InkCanvas, reconocimiento de escritura a mano, interacción del usuario, entrada
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 031ac1ebc8d164c99240969ce77813f36a311858
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258297"
---
# <a name="pen-interactions-and-windows-ink-in-uwp-apps"></a>Interacciones de lápiz y Windows Ink en aplicaciones para UWP

![el lápiz de Surface](images/ink/hero-small.png)  
*Lápiz para Surface* (disponible para su compra en [Microsoft Store](https://www.microsoft.com/p/surface-pen/8zl5c82qmg6b)).

## <a name="overview"></a>Introducción

Optimiza tu aplicación para la Plataforma universal de Windows (UWP) para que las entradas realizadas con la pluma proporcionen tanto la funcionalidad del [**dispositivo señalador**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.PointerDevice) como la mejor experiencia de Windows Ink para tus usuarios.

> [!NOTE]
> Este tema se centra en la plataforma Windows Ink. Para poder controlar las entradas generales del puntero (es algo similar al mouse, a la función táctil y al panel táctil), consulta el artículo [Controlar la entrada de puntero](handle-pointer-input.md).

| Vídeos |   |
| --- | --- |
| <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Ink-in-Your-UWP-App/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Events/Ignite/2016/BRK2060/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> |
| *Usar la entrada manuscrita en la aplicación para UWP* | *Usar el lápiz y la tinta de Windows para crear aplicaciones empresariales más atractivas* |

La plataforma de Windows Ink, junto con un dispositivo de lápiz, te ofrece una forma natural de crear notas, dibujos y anotaciones manuscritas, todo ello de forma digital. Asimismo, la plataforma te permite capturar los datos de entrada del digitalizador a modo de datos de entrada de lápiz, generar datos de entrada de lápiz, administrarlos, representarlos como trazos de lápiz en el dispositivo de salida y convertirlos en texto a través del reconocimiento de escritura a mano.

Además de capturar la posición básica y el movimiento del lápiz a medida que el usuario escribe o dibuja, la aplicación también puede realizar un seguimiento y recopilar los distintos niveles de presión que se usan en un trazo. Esta información, junto con la configuración de la forma de la punta del lápiz, el tamaño y la rotación, el color de la entrada de lápiz y el propósito (como la entrada de lápiz sin formato y las acciones de borrar, resaltar y seleccionar), te permite proporcionar experiencias de usuario que se asemejan mucho a escribir o dibujar sobre papel con un lápiz, un bolígrafo o un pincel.

> [!NOTE]
> La aplicación también es compatible con la entrada de lápiz de otros dispositivos señaladores, como digitalizadores táctiles y dispositivos de mouse. 

La plataforma de entrada de lápiz es muy flexible. Está diseñada para admitir varios niveles de funcionalidad, dependiendo de los requisitos.

Para obtener directrices sobre la experiencia del usuario de Windows Ink, consulta [Controles de entrada de lápiz](../controls-and-patterns/inking-controls.md).

## <a name="components-of-the-windows-ink-platform"></a>Componentes de la plataforma Windows Ink

| Componente | Descripción |
| --- | --- |
| [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) | Control de plataforma de interfaz de usuario XAML que, de forma predeterminada, recibe y muestra todas las entradas de un lápiz como trazo de tinta o trazo de borrado.<br/>Para obtener más información sobre cómo usar InkCanvas, consulta [Reconocer trazos de Windows Ink como texto](convert-ink-to-text.md) y [Almacenar y recuperar datos de trazos de lápiz de Windows Ink](save-and-load-ink.md). |
| [**Objeto**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) | Un objeto de código subyacente, cuya instancia se creó con un control [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) (expuesto a través de la propiedad [**InkCanvas.InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)). Este objeto proporciona todas las funcionalidades de entrada manuscrita predeterminadas y que expuso **InkCanvas**, junto con un completo conjunto de API para la personalización adicional.<br/>Para obtener más información sobre cómo usar InkPresenter, consulta [Reconocer trazos de Windows Ink como texto](convert-ink-to-text.md) y [Almacenar y recuperar datos de trazos de lápiz de Windows Ink](save-and-load-ink.md). |
| [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) | Control de plataforma de interfaz de usuario XAML que contiene una colección personalizable y extensible de botones que activan las características relacionadas con la tinta en un [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)asociado.<br/>Para obtener más información sobre cómo usar InkToolbar, consulta [Agregar un control InkToolbar a una aplicación de entrada manuscrita para la Plataforma universal de Windows (UWP)](ink-toolbar.md). |
| [**IInkD2DRenderer**](https://docs.microsoft.com/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer) | Habilita la representación de trazos de lápiz en el contexto de dispositivo de Direct2D designado de una aplicación universal de Windows, en lugar del control [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) predeterminado. Esto permite la personalización completa de la experiencia de entrada manuscrita.<br/>Para obtener más información, consulta [Complex ink sample (Muestra de entrada de lápiz compleja)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk). |

## <a name="basic-inking-with-inkcanvas"></a>Entrada manuscrita básica con InkCanvas

Para agregar la funcionalidad de entrada de lápiz básica, solo debes colocar un control [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) de plataforma UWP en la página adecuada de la aplicación.

De manera predeterminada, el control [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) solo admite la entrada de lápiz que se haya realizado con un lápiz. La entrada se representa como un trazo de lápiz mediante la configuración predeterminada de color y espesor (como un bolígrafo negro con un grosor de 2 píxeles) o se trata como si fuera un borrador de trazos (cuando la entrada se realiza desde un extremo del borrador o cuando la punta del lápiz se modifica con un botón de borrador).

> [!NOTE]
> Si no hay ningún botón o extremo borrador, InkCanvas puede configurarse para procesar la entrada de la punta del lápiz como trazos de borrado.

En este ejemplo, un control [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) se superpone a una imagen en segundo plano.

> [!NOTE]
> Un control InkCanvas tiene las propiedades [**height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Height) y [**width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Width) predeterminadas de cero, a menos que sea el elemento secundario de un elemento que ajusta automáticamente el tamaño de sus elementos secundarios, como los controles [StackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel) o [Grid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) .

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

Esta serie de imágenes muestra cómo se representa la entrada manuscrita mediante el control [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

| ![Control InkCanvas en blanco con una imagen de fondo](images/ink_basic_1_small.png) | ![Control InkCanvas con trazos de lápiz](images/ink_basic_2_small.png) | ![Control InkCanvas con un trazo borrado](images/ink_basic_3_small.png) |
| --- | --- | ---|
| Control [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) en blanco con una imagen de fondo. | Control [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) con trazos de lápiz. | Control [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) con un trazo borrado (ten en cuenta que la opción de borrado actúa en todo el trazo, no en una parte). |

La funcionalidad de entrada manuscrita admitida por el control [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) la proporciona un objeto de código subyacente denominado [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter).

Para realizar entradas manuscritas básicas, no es necesario preocuparse por [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter). Sin embargo, para personalizar y configurar el comportamiento de la entrada manuscrita en la clase [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas), debe acceder al objeto correspondiente **InkPresenter**.

## <a name="basic-customization-with-inkpresenter"></a>Personalización básica con InkPresenter

Se crea una instancia de un objeto [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) con cada control [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

> [!NOTE]
> No se pueden crear instancias de la propiedad [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) directamente. En su lugar, debes acceder a ellas a través de la propiedad [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) de la clase [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas). 

Junto con el suministro de todos los comportamientos de entrada de lápiz de su control [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) correspondiente, [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) proporciona un completo conjunto de API para la personalización del trazo adicional y administración más detallada de la entrada de lápiz (estándar y modificada). Se incluyen las propiedades de trazo, los tipos de dispositivo de entrada admitidos y si el objeto procesa la entrada o esta se pasa a la aplicación para su procesamiento.

> [!NOTE]
> La entrada de lápiz estándar (de punta de lápiz o punta y botón del borrador) no se modifica con una prestación hardware secundaria, como un botón del lápiz, el botón derecho del ratón o un mecanismo similar. 

De manera predeterminada, la entrada de lápiz solo es compatible con el lápiz. En este apartado, configuraremos [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) para que interprete los datos de entrada del lápiz y el mouse como trazos de lápiz. Asimismo, también estableceremos algunos atributos de trazo de lápiz iniciales que se usarán en la representación de trazos de [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

Para habilitar la entrada de lápiz con el ratón y la función táctil, establece la propiedad [**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) de [**InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter) en la combinación de valores de [**CoreInputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.core.coreinputdevicetypes) que quieras.

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

Estas imágenes muestran cómo se procesa y personaliza la entrada manuscrita mediante [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter).

| ![control inkcanvas con trazos de lápiz negro predeterminados](images/ink-basic-custom-1-small.png) | ![control inkcanvas con trazos de lápiz rojo seleccionados por el usuario](images/ink-basic-custom-2-small.png) |
| --- | --- |
| [  **InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) con trazos de lápiz negro predeterminados. | [  **InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) con trazos de lápiz rojo seleccionados por el usuario. | 

Para proporcionar funcionalidades que vayan más allá de la entrada manuscrita y el borrado (como la selección de trazo), la aplicación debe identificar la entrada específica de [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter), la cual debe pasarse sin procesar para que la aplicación la controle.

## <a name="pass-through-input-for-advanced-processing"></a>Entrada de paso a través para el procesamiento avanzado

De manera predeterminada, [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) procesa todas las entradas como trazos de lápiz o trazos de borrado, incluidas aquellas modificadas por una prestación de hardware secundaria como, por ejemplo, un botón de menú contextual del lápiz, un botón secundario del mouse o similar. Sin embargo, los usuarios suelen esperar algunas funciones adicionales o un comportamiento modificado con estas prestaciones secundarias.

En algunos casos, es posible que también necesites permitir una funcionalidad adicional para lápices sin prestaciones secundarias (esta funcionalidad generalmente no está asociada con la punta del lápiz), otros tipos de dispositivos de entrada o algún tipo de comportamiento modificado; todo ello según la selección que el usuario haga en la interfaz de usuario de la aplicación.

Para que esta acción sea compatible, puedes configurar [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) para que deje una entrada específica sin procesar. A continuación, esta entrada sin procesar se pasa a través de la aplicación para su procesamiento.

### <a name="example---use-unprocessed-input-to-implement-stroke-selection"></a>Ejemplo: Usa la entrada sin procesar para implementar la selección de trazo. 

La plataforma Windows Ink no proporciona compatibilidad integrada con las acciones que requieren la entrada modificada, como la selección de trazo. Para admitir características como esta, debes proporcionar una solución personalizada en tus aplicaciones. 

En el siguiente ejemplo de código (todo el código está en los archivos MainPage.xaml y MainPage.xaml.cs) se indica cómo habilitar la selección de trazo al modificarse la entrada con un botón de menú contextual del lápiz (o botón secundario del mouse).

1.  En primer lugar, debemos configurar la interfaz de usuario en MainPage.xaml.

    Una vez hecho esto, agregamos un lienzo (debajo de [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)) para dibujar el trazo de selección. Si usas una capa independiente para dibujar el trazo de selección, **InkCanvas** y su contenido no se modifican.

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

3.  A continuación, configuramos [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) para que interprete los datos de entrada del lápiz y el mouse como trazos de lápiz, y establecemos algunos atributos de trazo iniciales que se usan para la representación de trazos en [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

    Recuerda usar la propiedad [**InputProcessingConfiguration**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputprocessingconfiguration) de [InkPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) para indicar que la aplicación debe procesar cualquier entrada modificada. La entrada modificada se especifica asignando a **InputProcessingConfiguration.RightDragAction** un valor de [**InkInputRightDragAction.LeaveUnprocessed**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkInputRightDragAction). Al establecerse este valor, [InkPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) pasa a través de la clase [InkUnprocessedInput](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput), un conjunto de eventos de puntero para que los controles.

    Asignamos escuchas para los eventos [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerpressed), [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointermoved) y [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerreleased) sin procesar que la propiedad [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) ha pasado. Todas las funcionalidades de selección se implementan en los controladores de estos eventos.

    Por último, asignamos agentes de escucha para los eventos [**StrokeStarted**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted) y [**StrokesErased**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokeserased) de la propiedad [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter). Los controladores de estos eventos se usan para limpiar la interfaz de usuario de selección si se inicia un nuevo trazo o se borra un trazo existente.

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

4.  Una vez hecho todo esto, definimos los controladores de los eventos [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerpressed), [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointermoved) y [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerreleased) sin procesar que la propiedad [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) ha pasado.

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

6.  Por último, definimos los controladores de los eventos [**StrokeStarted**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted) y [**StrokesErased**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokeserased) de la propiedad InkPresenter.

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

De manera predeterminada, la entrada de lápiz se procesa en un subproceso en segundo plano de baja latencia y se representa como en curso o "húmeda" mientras se dibuja. Cuando se completa el trazo (se levanta el lápiz o el dedo o se libera el botón del mouse), el trazo se procesa en el subproceso de la interfaz de usuario y se representa como "seco" en la capa de [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) (lo verás sobre el contenido de la aplicación y reemplazando la entrada de lápiz húmeda).

Puedes invalidar este comportamiento predeterminado y controlar completamente la experiencia de entrada manuscrita "secando de forma personalizada" los trazos de lápiz húmedos. Aunque el comportamiento predeterminado suele ser suficiente para la mayoría de las aplicaciones, hay algunos casos en los que puede requerirse un secado personalizado, que incluyen lo siguiente:
- Administración más eficiente de colecciones de trazos de lápiz grandes o complejas
- Compatibilidad más eficiente con movimiento panorámico y zoom en lienzos de entrada de lápiz grandes
- Intercalación de tinta y otros objetos, como formas o texto, mientras se mantiene un orden Z 
- Secado y conversión de la entrada de lápiz sincrónicamente en una forma DirectX (por ejemplo, una línea recta o forma rasterizada e integrada en el contenido de la aplicación en lugar de hacerlo como una capa de **InkCanvas** diferente).

El secado personalizado requiere un objeto [**IInkD2DRenderer**](https://docs.microsoft.com/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer) para administrar la entrada de lápiz y representarla en el contexto de dispositivo Direct2D de la aplicación universal de Windows, en lugar del control [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) predeterminado.

Al llamar a [**ActivateCustomDrying**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.activatecustomdrying) (antes de cargar [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)), una aplicación crea un objeto [**InkSynchronizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkSynchronizer) para personalizar la representación de un trazo de lápiz seco en la clase [**SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) o [**VirtualSurfaceImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource). 

Ambos objetos [**SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) y [**VirtualSurfaceImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource) proporcionan una superficie compartida de DirectX para que tu aplicación dibuje en ella y componen en el contenido de tu aplicación, aunque VSIS proporciona una superficie virtual que es más grande que la pantalla para un movimiento panorámico y un zoom eficaces. Como las actualizaciones visuales de estas superficies se sincronizan con el subproceso de interfaz de usuario XAML, cuando la entrada de lápiz se representa en alguna de ellas, la entrada de lápiz húmeda se puede quitar de InkCanvas de forma simultánea. 

También puedes secar de forma personalizada la entrada de lápiz para [SwapChainPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel), pero la sincronización con el subproceso de interfaz de usuario no se garantiza y puede haber un retraso entre el momento en el que la entrada de lápiz se representa en SwapChainPanel y el momento en el que la entrada de lápiz se quita de InkCanvas.

Para obtener un ejemplo completo de esta funcionalidad, consulta la [muestra de entrada de lápiz compleja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk).

> [!NOTE]
> El secado personalizado y la clase [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)  
> Si tu aplicación invalida el comportamiento predeterminado de representación de entrada de lápiz de la clase [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) con una implementación de secado personalizado, los trazos de lápiz representados ya no están disponibles para InkToolbar y los comandos de borrado integrados de InkToolbar no funcionan según lo previsto. Para proporcionar la funcionalidad de borrado, debes controlar todos los eventos de puntero, realizar la prueba de posicionamiento en cada trazo e invalidar el comando "Borrar todas las entradas de lápiz" integrado.

## <a name="other-articles-in-this-section"></a>Otros artículos de esta sección

| Tema | Descripción |
| --- | --- |
| [Reconocer trazos de lápiz](convert-ink-to-text.md) | Conversión de trazos de lápiz en texto mediante el reconocimiento de escritura a mano o en formas mediante el reconocimiento personalizado |
| [Almacenar y recuperar trazos de entrada de lápiz](save-and-load-ink.md) | Almacena los datos de trazo de lápiz en un archivo de formato de intercambio de elementos gráficos (GIF) mediante los metadatos Ink Serialized Format (ISF) insertados. |
| [Agregar un control InkToolbar a una aplicación de entrada manuscrita de UWP](ink-toolbar.md) | Agrega un valor predeterminado InkToolbar a una aplicación de entrada de lápiz de la Plataforma universal de Windows (UWP), agrega un botón de lápiz personalizado a InkToolbar y enlaza el botón de lápiz personalizado con una definición de lápiz personalizado. |

## <a name="related-articles"></a>Artículos relacionados

* [Introducción: compatibilidad con la entrada manuscrita en la aplicación para UWP](../../get-started/ink-walkthrough.md)
* [Control de la entrada con puntero](handle-pointer-input.md)
* [Identificación de dispositivos de entrada](identify-input-devices.md)

**Enumera**

* [**Windows. Devices. Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input)
* [**Windows. UI. Input. inking**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking)
* [**Windows. UI. Input. inking. Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.Core)

**Ejemplos**
* [Tutorial de introducción: compatibilidad con la entrada manuscrita en la aplicación para UWP](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
* [Ejemplo de entrada deC#lápizC++simple (/)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
* [Ejemplo de tinta complejaC++()](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
* [Ejemplo de entrada de lápiz (JavaScript)](https://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Ejemplo de libro de color](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [Ejemplo de notas de la familia](https://github.com/Microsoft/Windows-appsample-familynotes)
* [Ejemplo de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Ejemplo de entrada de baja latencia](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [Ejemplo de modo de interacción del usuario](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**Ejemplos de archivo**
* [Entrada: ejemplo de funcionalidades del dispositivo](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [Entrada: ejemplo de eventos de entrada de usuario de XAML](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [Ejemplo de desplazamiento, panorámica y zoom de XAML](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [Entrada: gestos y manipulaciones con GestureRecognizer](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
