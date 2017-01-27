---
author: Karl-Bridge-Microsoft
Description: "Agrega un valor predeterminado InkToolbar a una aplicación de entrada de lápiz de la Plataforma universal de Windows (UWP), agrega un botón de lápiz personalizado a InkToolbar y enlaza el botón de lápiz personalizado con una definición de lápiz personalizado."
title: "Agregar un control InkToolbar a una aplicación de entrada manuscrita para la Plataforma universal de Windows (UWP)"
label: Add an InkToolbar to a Universal Windows Platform (UWP) inking app
template: detail.hbs
keywords: "Windows Ink, entrada manuscrita de Windows, DirectInk, InkPresenter, InkCanvas, InkToolbar, Plataforma universal de Windows, UWP, interacción del usuario, entrada"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: b3c9e291c8b22c4b642e87a2caa05b36de519d22
ms.openlocfilehash: 5983209e7e107f8cf0a7d2fe9ce8ef734e3af7d4

---

# <a name="add-an-inktoolbar-to-a-universal-windows-platform-uwp-inking-app"></a>Agregar un control InkToolbar a una aplicación de entrada manuscrita para la Plataforma universal de Windows (UWP)
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">


Hay dos controles diferentes que facilitan la entrada manuscrita en aplicaciones para la Plataforma universal de Windows (UWP): [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) e [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

El control [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) proporciona la funcionalidad básica de Windows Ink. Úsalo para representar la entrada manuscrita como un trazo de lápiz (con la configuración predeterminada de color y espesor) o como un trazo de borrado.

> Para obtener detalles sobre la implementación del control InkCanvas, consulta [Interacciones de pluma y lápiz en aplicaciones para UWP](pen-and-stylus-interactions.md).

Como una superposición completamente transparente, el control InkCanvas no proporciona ninguna interfaz de usuario integrada para establecer propiedades de trazo de lápiz. Si quieres cambiar la experiencia de entrada manuscrita predeterminada, permite que los usuarios establezcan las propiedades de trazo de lápiz y admite otras características de entrada manuscrita personalizadas. Para hacerlo, tienes dos opciones:

- En el código subyacente, usa el objeto subyacente [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) enlazado al control InkCanvas.

  Las API de InkPresenter admiten una amplia personalización de la experiencia de entrada manuscrita. Para obtener más información, consulta [Interacciones de pluma y lápiz en aplicaciones para UWP](pen-and-stylus-interactions.md).

- Enlazar un control [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) al control InkCanvas. De manera predeterminada, el control InkToolbar proporciona una interfaz de usuario básica para activar las características de entrada de lápiz y establecer sus propiedades, como el tamaño del trazo, el color de la entrada de lápiz y la forma de la punta del lápiz.

  El control InkToolbar se describe en este tema.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Clase InkCanvas**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx)</li>
<li>[**Clase InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)</li>
<li>[**Clase InkPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx)</li>
<li>[**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)</li>
</ul>
</div>

## <a name="default-inktoolbar"></a>Control InkToolbar predeterminado

De manera predeterminada, el control InkToolbar incluye botones para dibujar, borrar, resaltar y mostrar una regla. Dependiendo de la característica, en un control flotante se proporcionan otros comandos y opciones de configuración, como los destinados a definir el color y el grosor del trazo o a borrar todas las entradas de lápiz.

![InkToolbar](.\images\ink\ink-tools-invoked-toolbar-small.png)  
*Barra de herramientas de Windows Ink predeterminada*

Para agregar un control InkToolbar predeterminado básico:
1. En MainPage.xaml, declara un objeto contenedor (para este ejemplo, usamos un control Grid) para la superficie de entrada manuscrita.
2. Declara un objeto InkCanvas como elemento secundario del contenedor. (El tamaño del control InkCanvas se hereda del contenedor).
3. Declara un control InkToolbar y usa el atributo TargetInkCanvas para enlazarlo al control InkCanvas.
  Asegúrate de que el control InkToolbar se declare después de InkCanvas. De lo contrario, la superposición de InkCanvas hará que el control InkToolbar sea inaccesible.

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
        <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
    </Grid>
</Grid>
```

## <a name="basic-customization"></a>Personalización básica

En esta sección, nos centraremos en algunos escenarios básicos de personalización de la barra de herramientas de Windows Ink.

### <a name="specify-the-selected-button"></a>Especificar el botón seleccionado  
![Botón de lápiz seleccionado durante la inicialización](.\images\ink\ink-tools-default-toolbar.png)  
*Barra de herramientas de Windows Ink con el botón de lápiz seleccionado durante la inicialización*

De manera predeterminada, se selecciona el primer botón (más a la izquierda) cuando se inicia la aplicación y se inicializa la barra de herramientas. En la barra de herramientas de Windows Ink predeterminada, este es el botón de bolígrafo.

Dado que el marco define el orden de los botones integrados, el primer botón podría no ser el lápiz o la herramienta que quieres activar de forma predeterminada.

Puedes invalidar este comportamiento predeterminado y especificar el botón seleccionado en la barra de herramientas.

Para este ejemplo, inicializamos la barra de herramientas predeterminada con el botón de lápiz seleccionado y el lápiz activado (en lugar del bolígrafo).

1. Usa la declaración de XAML para los controles InkCanvas e InkToolbar del ejemplo anterior.
2. En el código subyacente, configura un controlador para el evento [Loaded](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx) del objeto [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loaded += inkToolbar_Loaded;
  }
  ```

3. En el controlador del evento [Loaded](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx):
  1. Obtén una referencia al botón [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) integrado.

    Al pasar un objeto [InkToolbarTool.Pencil](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbartool.aspx) en el método [GetToolButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.gettoolbutton.aspx), se devuelve un objeto [InkToolbarToolButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbartoolbutton.aspx) para el botón [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx).

  2. Establece [ActiveTool](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.activetool.aspx) en el objeto devuelto en el paso anterior.

```CSharp
/// <summary>
/// Handle the Loaded event of the InkToolbar.
/// By default, the active tool is set to the first tool on the toolbar.
/// Here, we set the active tool to the pencil button.
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void inkToolbar_Loaded(object sender, RoutedEventArgs e)
{
    InkToolbarToolButton pencilButton = inkToolbar.GetToolButton(InkToolbarTool.Pencil);
    inkToolbar.ActiveTool = pencilButton;
}
```

### <a name="specify-the-built-in-buttons"></a>Especificar los botones integrados

![Botones específicos que se incluyen durante la inicialización](.\images\ink\ink-tools-specific.png)  
*Botones específicos que se incluyen durante la inicialización*

Como hemos mencionado, la barra de herramientas de Windows Ink incluye una colección de botones integrados predeterminados. Estos botones aparecen en el siguiente orden (de izquierda a derecha):

- [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx)
- [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx)
- [InkToolbarHighlighterButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarhighlighterbutton.aspx)
- [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)
- [InkToolbarRulerButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarrulerbutton.aspx)

Para este ejemplo, inicializamos la barra de herramientas solo con los botones de bolígrafo, lápiz y borrador integrados.

Puedes hacerlo mediante XAML o código subyacente.

**XAML**

Modifica la declaración de XAML para los controles InkCanvas e InkToolbar del primer ejemplo.
- Agrega un atributo [InitialControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) y establece su valor en "[None](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)". Esta acción borra la colección predeterminada de botones integrados.
- Agrega los botones InkToolbar específicos necesarios para la aplicación. Aquí, agregamos solo los botones [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx), [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) e [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx).
> [!NOTE]
> Los botones se agregan a la barra de herramientas en el orden definido por el marco, no en el orden especificado aquí.

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
        <!-- Clear the default InkToolbar buttons by setting InitialControls to None. -->
        <!-- Set the active tool to the pencil button. -->
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
                    VerticalAlignment="Top"
                    TargetInkCanvas="{x:Bind inkCanvas}"
                    InitialControls="None">
            <!--
             Add only the ballpoint pen, pencil, and eraser.
             Note that the buttons are added to the toolbar in the order
             defined by the framework, not the order we specify here.
            -->
            <InkToolbarEraserButton />
            <InkToolbarBallpointPenButton />
            <InkToolbarPencilButton/>
        </InkToolbar>
    </Grid>
</Grid>
```

**Código subyacente**
1. Usa la declaración de XAML para los controles InkCanvas e InkToolbar del primer ejemplo.

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
          <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
      </Grid>
  </Grid>
  ```

2. En el código subyacente, configura un controlador para el evento [Loading](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.loading.aspx) del objeto [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loading += inkToolbar_Loading;
  }
  ```

3. Establece [InitialControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) en "[None](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)".
4. Crea referencias de objetos para los botones que necesita tu aplicación. Aquí, agregamos solo los botones [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx), [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) e [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx).
  > [!NOTE]
  > Los botones se agregan a la barra de herramientas en el orden definido por el marco, no en el orden especificado aquí.

5. Método [Add](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobjectcollection.add.aspx) para agregar los botones al control InkToolbar.

  ```csharp
  /// <summary>
  /// Handles the Loading event of the InkToolbar.
  /// Here, we identify the buttons to include on the InkToolbar.
  /// </summary>
  /// <param name="sender">The InkToolbar</param>
  /// <param name="args">The InkToolbar event data.
  /// If there is no event data, this parameter is null</param>
  private void inkToolbar_Loading(FrameworkElement sender, object args)
  {
      // Clear all built-in buttons from the InkToolbar.
      inkToolbar.InitialControls = InkToolbarInitialControls.None;

      // Add only the ballpoint pen, pencil, and eraser.
      // Note that the buttons are added to the toolbar in the order
      // defined by the framework, not the order we specify here.
      InkToolbarBallpointPenButton ballpoint = new InkToolbarBallpointPenButton();
      InkToolbarPencilButton pencil = new InkToolbarPencilButton();
      InkToolbarEraserButton eraser = new InkToolbarEraserButton();
      inkToolbar.Children.Add(eraser);
      inkToolbar.Children.Add(ballpoint);
      inkToolbar.Children.Add(pencil);
  }
  ```

<!--
### Support touch input
By default, the InkToolbar supports both pen and mouse input, you have to enable support for touch input.
-->

## <a name="custom-buttons-and-inking-features"></a>Botones personalizados y características de entrada manuscrita

Puedes personalizar y ampliar la colección de botones (y las características de entrada manuscrita asociadas) que se proporcionan a través del control InkToolbar.

El control InkToolbar consta de dos grupos de tipos de botones:

1. Un grupo de botones de "herramienta" que contiene los botones para dibujar, borrar y resaltar integrados. Las herramientas y los lápices personalizados se agregan aquí.
> **Nota**&nbsp;&nbsp;La selección de características es mutuamente exclusiva.

2. Un grupo de botones de "alternancia" que contiene el botón de regla integrado. Las alternancias personalizadas se agregan aquí.
> **Nota**&nbsp;&nbsp;Las características no son mutuamente exclusivas y se pueden usar simultáneamente con otras herramientas activas.

Dependiendo de la aplicación y de la funcionalidad de entrada manuscrita necesaria, puedes agregar cualquiera de los siguientes botones (enlazados a sus características de entrada de lápiz personalizada) al control InkToolbar:

- Lápiz personalizado: lápiz para el cual la aplicación host define las propiedades de punta de lápiz y paleta de colores del trazo, como la forma, la rotación y el tamaño.
- Herramienta personalizada: herramienta que no es un lápiz definida por la aplicación host.
- Alternancia personalizada: establece el estado de una característica definida por la aplicación en activado o desactivado. Cuando se activa, la característica funciona junto con la herramienta activa.

> **Nota**&nbsp;&nbsp;No puedes cambiar el orden de visualización de los botones integrados. El orden de visualización predeterminado es: bolígrafo, lápiz, marcador de resaltado, borrador y regla. Los lápices personalizados se agregan al último lápiz predeterminado, los botones de la herramienta personalizada se agregan entre el último botón del lápiz y el botón de borrador, y los botones de alternancia personalizada se agregan detrás del botón de regla. (Los botones personalizados se agregan en el orden en que se especifican).

### <a name="custom-pen"></a>Lápiz personalizado

Puedes crear un lápiz personalizado (que se activa mediante un botón de lápiz personalizado) con el que defines la paleta de colores de entrada de lápiz y las propiedades de punta de lápiz, como la forma, la rotación y el tamaño.

![Botón del lápiz caligráfico personalizado](.\images\ink\ink-tools-custompen.png)  
*Botón del lápiz caligráfico personalizado*

En este ejemplo, se define un lápiz personalizado con una punta ancha que permite trazos de lápiz caligráficos básicos. También se personaliza la colección de pinceles de la paleta que se muestra en el control flotante de botones.

**Código subyacente**

En primer lugar, definimos nuestro lápiz personalizado y especificamos los atributos de dibujo en el código subyacente. Hacemos referencia este lápiz personalizado de XAML más adelante.

1. Haz clic con el botón secundario en el proyecto en el Explorador de soluciones y selecciona Agregar > Nuevo elemento.
2. En Visual C# -> Código, agrega un nuevo archivo de clase y llámalo CalligraphicPen.cs.
3. En Calligraphic.cs, reemplaza el valor predeterminado mediante un bloque con lo siguiente:
```csharp
using System.Numerics;
using Windows.UI;
using Windows.UI.Input.Inking;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
```

4. Especifica que la clase CalligraphicPen se deriva de [InkToolbarCustomPen](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.aspx).
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```

5. Invalida [CreateInkDrawingAttributesCore](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore.aspx) para especificar tu propio tamaño de trazo y pincel.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```

6. Crea un objeto [InkDrawingAttributes](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.aspx) y establece las propiedades de [forma de la punta del lápiz](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentip.aspx), [rotación de la punta](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentiptransform.aspx), [tamaño del trazo](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.size.aspx) y [color del trazo](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.color.aspx).
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
        InkDrawingAttributes inkDrawingAttributes =
          new InkDrawingAttributes();
        inkDrawingAttributes.PenTip = PenTipShape.Circle;
        inkDrawingAttributes.Size =
          new Windows.Foundation.Size(strokeWidth, strokeWidth * 20);
        SolidColorBrush solidColorBrush = brush as SolidColorBrush;
        if (solidColorBrush != null)
        {
            inkDrawingAttributes.Color = solidColorBrush.Color;
        }
        else
        {
            inkDrawingAttributes.Color = Colors.Black;
        }

        Matrix3x2 matrix = Matrix3x2.CreateRotation(45);
        inkDrawingAttributes.PenTipTransform = matrix;

        return inkDrawingAttributes;
    }
}
```

**XAML**

A continuación, agregamos las referencias necesarias para el lápiz personalizado en MainPage.xaml.

1. Declaramos un diccionario de recursos de página local que cree una referencia al lápiz personalizado (`CalligraphicPen`) definido en CalligraphicPen.cs y una [colección de pinceles](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.BrushCollection.aspx) compatibles con el lápiz personalizado (`CalligraphicPenPalette`).
```xaml
<Page.Resources>
    <!-- Add the custom CalligraphicPen to the page resources. -->
    <local:CalligraphicPen x:Key="CalligraphicPen" />
    <!-- Specify the colors for the palette of the custom pen. -->
    <BrushCollection x:Key="CalligraphicPenPalette">
        <SolidColorBrush Color="Blue" />
        <SolidColorBrush Color="Red" />
    </BrushCollection>
</Page.Resources>
```

2. A continuación, agregamos un control InkToolbar con un elemento [InkToolbarCustomPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompenbutton.aspx) secundario.

  El botón de lápiz personalizado incluye las dos referencias de recursos estáticos declaradas en los recursos de página: `CalligraphicPen` y `CalligraphicPenPalette`.

  También especificamos el alcance del control deslizante de tamaño de trazo ([MinStrokeWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth.aspx), [MaxStrokeWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth.aspx) y [SelectedStrokeWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty.aspx)), el pincel seleccionado ([SelectedBrushIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex.aspx)) y el icono para el botón de lápiz personalizado ([SymbolIcon](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.symbolicon.aspx)).
```xaml
<Grid Grid.Row="1">
    <InkCanvas x:Name="inkCanvas" />
    <InkToolbar x:Name="inkToolbar"
                VerticalAlignment="Top"
                TargetInkCanvas="{x:Bind inkCanvas}">
        <InkToolbarCustomPenButton
            CustomPen="{StaticResource CalligraphicPen}"
            Palette="{StaticResource CalligraphicPenPalette}"
            MinStrokeWidth="1" MaxStrokeWidth="3" SelectedStrokeWidth="2"
            SelectedBrushIndex ="1">
            <SymbolIcon Symbol="Favorite" />
            <InkToolbarCustomPenButton.ConfigurationContent>
                <InkToolbarPenConfigurationControl />
            </InkToolbarCustomPenButton.ConfigurationContent>
        </InkToolbarCustomPenButton>
    </InkToolbar>
</Grid>
```

### <a name="custom-toggle"></a>Alternancia personalizada

Puedes crear una alternancia personalizada (que se activa a través de un botón de alternancia personalizada) para activar o desactivar el estado de una característica definida por la aplicación. Cuando se activa, la característica funciona junto con la herramienta activa.

En este ejemplo, definimos un botón de alternancia personalizada que permite la entrada manuscrita con entrada táctil (de manera predeterminada, la entrada manuscrita táctil no está habilitada).

> [!NOTE]  
> Si necesitas compatibilidad con la entrada manuscrita mediante función táctil, te recomendamos habilitar esta opción mediante un botón de alternancia personalizada, con el icono y la información sobre herramientas especificados en este ejemplo.

Por lo general, la entrada táctil se utiliza para la manipulación directa de un objeto o de la interfaz de usuario de la aplicación. Para demostrar las diferencias de comportamiento cuando la entrada manuscrita táctil está habilitada, colocamos InkCanvas en un contenedor ScrollViewer y establecemos que las dimensiones del elemento ScrollViewer sean menores que las de InkCanvas. 

Cuando se inicia la aplicación, solo se admite la entrada manuscrita con lápiz y la entrada táctil se usa para realizar movimientos panorámicos o de zoom en la superficie de entrada manuscrita. Si la entrada manuscrita táctil está habilitada, no se pueden realizar movimientos panorámicos ni de zoom en la superficie de entrada manuscrita mediante la entrada táctil.

> [!NOTE]
> Consulta [Controles de entrada manuscrita](..\controls-and-patterns\inking-controls.md) para conocer las directrices sobre la experiencia del usuario de [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) y [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar). Las siguientes recomendaciones son pertinentes para este ejemplo:
> - [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar), y la entrada manuscrita en general, funcionan mejor con un lápiz activo. Sin embargo, la entrada manuscrita con el mouse y táctil puede admitirse si tu aplicación lo requiere. 
> - Si admites la entrada manuscrita con la entrada táctil, se recomienda usar el icono de "ED5F" de la fuente "Segoe MLD2 Assets" para el botón de alternancia, con una información sobre herramientas "Escritura táctil". 

**XAML**

1. En primer lugar, declaramos un elemento [**InkToolbarCustomToggleButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToggleButton) (toggleButton) con un agente de escucha de eventos Click que especifica el controlador de eventos (Toggle_Custom).

```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel Grid.Row="0" 
                x:Name="HeaderPanel" 
                Orientation="Horizontal">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10" />
    </StackPanel>

    <ScrollViewer Grid.Row="1" 
                  HorizontalScrollBarVisibility="Auto" 
                  VerticalScrollBarVisibility="Auto">
        
        <Grid HorizontalAlignment="Left" VerticalAlignment="Top">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            
            <InkToolbar Grid.Row="0" 
                        Margin="10"
                        x:Name="inkToolbar" 
                        VerticalAlignment="Top"
                        TargetInkCanvas="{x:Bind inkCanvas}">
                <InkToolbarCustomToggleButton 
                x:Name="toggleButton" 
                Click="CustomToggle_Click" 
                ToolTipService.ToolTip="Touch Writing">
                    <SymbolIcon Symbol="{x:Bind TouchWritingIcon}"/>
                </InkToolbarCustomToggleButton>
            </InkToolbar>
            
            <ScrollViewer Grid.Row="1" 
                          Height="500"
                          Width="500"
                          x:Name="scrollViewer" 
                          ZoomMode="Enabled" 
                          MinZoomFactor=".1" 
                          VerticalScrollMode="Enabled" 
                          VerticalScrollBarVisibility="Auto" 
                          HorizontalScrollMode="Enabled" 
                          HorizontalScrollBarVisibility="Auto">
                
                <Grid x:Name="outputGrid" 
                      Height="1000"
                      Width="1000"
                      Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}">
                    <InkCanvas x:Name="inkCanvas"/>
                </Grid>
                
            </ScrollViewer>
        </Grid>
    </ScrollViewer>
</Grid>
```

**Código subyacente**

2. En el fragmento anterior, declaramos un agente de escucha de eventos Click y un controlador (Toggle_Custom) en el botón de alternancia personalizada para la entrada manuscrita táctil (toggleButton). Este controlador activa o desactiva la compatibilidad con CoreInputDeviceTypes.Touch a través de la propiedad InputDeviceTypes del objeto InkPresenter.

   También hemos especificado un icono para el botón con el elemento SymbolIcon y la extensión de marcado {x: Bind} que lo enlaza a un campo definido en el archivo de código subyacente (TouchWritingIcon).

   El siguiente fragmento de código incluye el controlador de eventos Click y la definición de TouchWritingIcon.

```csharp 
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomToggle : Page
    {
        Symbol TouchWritingIcon = (Symbol)0xED5F;

        public MainPage_AddCustomToggle()
        {
            this.InitializeComponent();
        }

        // Handler for the custom toggle button that enables touch inking.
        private void CustomToggle_Click(object sender, RoutedEventArgs e)
        {
            if (toggleButton.IsChecked == true)
            {
                inkCanvas.InkPresenter.InputDeviceTypes |= CoreInputDeviceTypes.Touch;
            }
            else
            {
                inkCanvas.InkPresenter.InputDeviceTypes &= ~CoreInputDeviceTypes.Touch;
            }
        }
    }
}
```

### <a name="custom-tool"></a>Herramienta personalizada

Puedes crear un botón de herramienta personalizada para invocar una herramienta que no sea lápiz y que esté definida por la aplicación.

De manera predeterminada, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) procesa todas las entradas como trazos de lápiz o trazos de borrado. Esto incluye la entrada que modificó una prestación de hardware secundaria, como el botón de menú contextual del lápiz, el botón secundario del mouse o similares. Sin embargo, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) puede configurarse para dejar una entrada determinada sin procesar que, a continuación, se pasa a la aplicación para procesarla de forma personalizada.

En este ejemplo, definimos un botón de herramienta personalizada que, cuando se activa, hace que los trazos posteriores se procesen y se representen como un lazo de selección (línea discontinua) en lugar de como una entrada manuscrita. Todos los trazos de lápiz que se hagan dentro de los límites del área de selección se establecen en [**Selected**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkStroke.Selected).

> [!NOTE]
> Consulta Controles de entrada manuscrita para conocer las directrices sobre la experiencia del usuario de InkCanvas y InkToolbar. La siguiente recomendación es pertinente para este ejemplo:
> - Si se proporciona selección de trazo, se recomienda usar el icono de EF20 de la fuente Segoe MLD2 Assets para el botón de herramienta, con una información sobre herramientas "Herramienta de selección". 
 
**XAML**

1. En primer lugar, declaramos un elemento [**InkToolbarCustomToolButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) (customToolButton) con un agente de escucha de eventos Click que especifica el controlador de eventos (customToolButton_Click) donde se configura la selección de trazos. (También hemos agregado un conjunto de botones para copiar, cortar y pegar la selección de trazos).

2. También agregamos un elemento Canvas para dibujar el trazo de selección. Si usas una capa independiente para dibujar el trazo de selección, [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) y su contenido no se modifican. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />
    </StackPanel>
    <StackPanel x:Name="ToolPanel" Orientation="Horizontal" Grid.Row="1">
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="Top" 
                    TargetInkCanvas="{x:Bind inkCanvas}">
            <InkToolbarCustomToolButton 
                x:Name="customToolButton" 
                Click="customToolButton_Click" 
                ToolTipService.ToolTip="Selection tool">
                <SymbolIcon Symbol="{x:Bind SelectIcon}"/>
            </InkToolbarCustomToolButton>
        </InkToolbar>
        <Button x:Name="cutButton" 
                Content="Cut" 
                Click="cutButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="copyButton" 
                Content="Copy"  
                Click="copyButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="pasteButton" 
                Content="Paste"  
                Click="pasteButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
    </StackPanel>
    <Grid Grid.Row="2" x:Name="outputGrid" 
              Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}" 
              Height="Auto">
        <!-- Canvas for displaying selection UI. -->
        <Canvas x:Name="selectionCanvas"/>
        <!-- Canvas for displaying ink. -->
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

**Código subyacente**

2. A continuación, controlamos el evento Click para [**InkToolbarCustomToolButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) en el archivo de código subyacente MainPage.xaml.cs.

   Este controlador configura [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) para pasar la entrada sin procesar a la aplicación. 

   Para conocer este código con más detalle, consulta la sección "Entrada de paso a través para el procesamiento avanzado" de [Interacciones de lápiz y Windows Ink en aplicaciones para UWP](pen-and-stylus-interactions.md).

   También hemos especificado un icono para el botón con el elemento SymbolIcon y la extensión de marcado {x: Bind} que lo enlaza a un campo definido en el archivo de código subyacente (SelectIcon).

   El siguiente fragmento de código incluye el controlador de eventos Click y la definición de SelectIcon.

```csharp
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomTool : Page
    {
        // Icon for custom selection tool button.
        Symbol SelectIcon = (Symbol)0xEF20;

        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;

        public MainPage_AddCustomTool()
        {
            this.InitializeComponent();

            // Listen for new ink or erase strokes to clean up selection UI.
            inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
                StrokeInput_StrokeStarted;
            inkCanvas.InkPresenter.StrokesErased +=
                InkPresenter_StrokesErased;
        }

        private void customToolButton_Click(object sender, RoutedEventArgs e)
        {
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
        }

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

        private void cutButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
            inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            ClearSelection();
        }

        private void copyButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        }

        private void pasteButton_Click(object sender, RoutedEventArgs e)
        {
            if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
            {
                inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                    new Point(0, 0));
            }
            else
            {
                // Cannot paste from clipboard.
            }
        }

        // Clean up selection UI.
        private void ClearSelection()
        {
            var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
            foreach (var stroke in strokes)
            {
                stroke.Selected = false;
            }
            ClearBoundingRect();
        }

        private void ClearBoundingRect()
        {
            if (selectionCanvas.Children.Any())
            {
                selectionCanvas.Children.Clear();
                boundingRect = Rect.Empty;
            }
        }

        // Handle unprocessed pointer events from modifed input.
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
    }
}
```



### <a name="custom-ink-rendering"></a>Representación de entrada de lápiz personalizada

De manera predeterminada, la entrada de lápiz se procesa en un subproceso en segundo plano de baja latencia y se representa como "húmeda" mientras se dibuja. Cuando se completa el trazo (se levanta el lápiz o el dedo o se libera el botón del mouse), el trazo se procesa en el subproceso de la interfaz de usuario y se representa como "seco" en la capa de [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (lo verás sobre el contenido de la aplicación y reemplazando la entrada de lápiz húmeda).

La plataforma de entrada de lápiz te permite invalidar este comportamiento y personalizar totalmente la experiencia de entrada de lápiz mediante el secado personalizado de la entrada de lápiz.

Para obtener más información sobre el secado personalizado, consulta [Interacciones de lápiz y Windows Ink en aplicaciones para UWP](https://msdn.microsoft.com/windows/uwp/input-and-devices/pen-and-stylus-interactions#custom-ink-rendering).

> [!NOTE]
> El secado personalizado y la clase [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)  
> Si tu aplicación invalida el comportamiento predeterminado de representación de entrada de lápiz de la clase [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) con una implementación de secado personalizado, los trazos de lápiz representados ya no están disponibles para InkToolbar y los comandos de borrado integrados de InkToolbar no funcionan según lo previsto. Para proporcionar la funcionalidad de borrado, debes controlar todos los eventos de puntero, realizar la prueba de posicionamiento en cada trazo e invalidar el comando "Borrar todas las entradas de lápiz" integrado.

## <a name="related-articles"></a>Artículos relacionados

* [Interacciones de pluma y lápiz](pen-and-stylus-interactions.md)

**Ejemplos**
* [Muestra de entrada de lápiz](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Muestra de entrada de lápiz simple](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Muestra de entrada de lápiz compleja](http://go.microsoft.com/fwlink/p/?LinkID=620314)



<!--HONumber=Dec16_HO3-->


