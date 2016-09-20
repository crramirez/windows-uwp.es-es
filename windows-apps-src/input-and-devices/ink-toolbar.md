---
author: Karl-Bridge-Microsoft
Description: "Agrega un valor predeterminado InkToolbar a una aplicación de entrada de lápiz de la Plataforma universal de Windows (UWP), agrega un botón de lápiz personalizado a InkToolbar y enlaza el botón de lápiz personalizado con una definición de lápiz personalizado."
title: "Agregar un control InkToolbar a una aplicación de entrada manuscrita para la Plataforma universal de Windows (UWP)"
label: Add an InkToolbar to a Universal Windows Platform (UWP) inking app
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, InkToolbar, Universal Windows Platform, UWP
translationtype: Human Translation
ms.sourcegitcommit: 743ce43be6220d96d7e9bc295ab72bdec4905edc
ms.openlocfilehash: 4c50c8ded127b2261df901d9da3210ff397b00ca

---

# Agregar un control InkToolbar a una aplicación de entrada manuscrita para la Plataforma universal de Windows (UWP)

Hay dos controles diferentes que facilitan la entrada manuscrita en aplicaciones para la Plataforma universal de Windows (UWP): [**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) e [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

El control [**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) proporciona la funcionalidad básica de Windows Ink. Úsalo para representar la entrada manuscrita como un trazo de lápiz (con la configuración predeterminada de color y espesor) o como un trazo de borrado.

> Para obtener detalles sobre la implementación del control InkCanvas, consulta [Interacciones de pluma y lápiz en aplicaciones para UWP](pen-and-stylus-interactions.md).

Como una superposición completamente transparente, el control InkCanvas no proporciona ninguna interfaz de usuario integrada para establecer propiedades de trazo de lápiz. Si quieres cambiar la experiencia de entrada manuscrita predeterminada, permite que los usuarios establezcan las propiedades de trazo de lápiz y admite otras características de entrada manuscrita personalizadas. Para hacerlo, tienes dos opciones:

- En el código subyacente, usa el objeto subyacente [**InkPresenter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) enlazado al control InkCanvas.

  Las API de InkPresenter admiten una amplia personalización de la experiencia de entrada manuscrita. Para obtener más información, consulta [Interacciones de pluma y lápiz en aplicaciones para UWP](pen-and-stylus-interactions.md).

- Enlazar un control [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) al control InkCanvas. De manera predeterminada, el control InkToolbar proporciona una interfaz de usuario básica para activar las características de entrada de lápiz y establecer sus propiedades, como el tamaño del trazo, el color de la entrada de lápiz y la forma de la punta del lápiz.

  El control InkToolbar se describe en este tema.

## API importantes

  -   [**Clase InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx)
  -   [**Clase InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)
  -   [**Clase InkPresenter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx)
  -   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


## Control InkToolbar predeterminado

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

## Personalización básica

En esta sección, nos centraremos en algunos escenarios básicos de personalización de la barra de herramientas de Windows Ink.

### Especificar el botón seleccionado  
![Botón de lápiz seleccionado durante la inicialización](.\images\ink\ink-tools-default-toolbar.png)  
*Barra de herramientas de Windows Ink con el botón de lápiz seleccionado durante la inicialización*

De manera predeterminada, se selecciona el primer botón (más a la izquierda) cuando se inicia la aplicación y se inicializa la barra de herramientas. En la barra de herramientas de Windows Ink predeterminada, este es el botón de bolígrafo.

Dado que el marco define el orden de los botones integrados, el primer botón podría no ser el lápiz o la herramienta que quieres activar de forma predeterminada.

Puedes invalidar este comportamiento predeterminado y especificar el botón seleccionado en la barra de herramientas.

Para este ejemplo, inicializamos la barra de herramientas predeterminada con el botón de lápiz seleccionado y el lápiz activado (en lugar del bolígrafo).

1. Usa la declaración de XAML para los controles InkCanvas e InkToolbar del ejemplo anterior.
2. En el código subyacente, configura un controlador para el evento [Loaded](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx) del objeto [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

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

3. En el controlador del evento [Loaded](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx):
  1. Obtén una referencia al botón [InkToolbarPencilButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) integrado.

    Al pasar un objeto [InkToolbarTool.Pencil](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbartool.aspx) en el método [GetToolButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.gettoolbutton.aspx), se devuelve un objeto [InkToolbarToolButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbartoolbutton.aspx) para el botón [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx).

  2. Establece [ActiveTool](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.activetool.aspx) en el objeto devuelto en el paso anterior.

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

### Especificar los botones integrados

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
- Agrega un atributo [InitialControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) y establece su valor en "[None](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)". Esta acción borra la colección predeterminada de botones integrados.
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

2. En el código subyacente, configura un controlador para el evento [Loading](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loading.aspx) del objeto [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

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

3. Establece [InitialControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) en "[None](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)".
4. Crea referencias de objetos para los botones que necesita tu aplicación. Aquí, agregamos solo los botones [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx), [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) e [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx).
  > [!NOTE]
  > Los botones se agregan a la barra de herramientas en el orden definido por el marco, no en el orden especificado aquí.

5. Método [Add](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.dependencyobjectcollection.add.aspx) para agregar los botones al control InkToolbar.

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

## Botones personalizados y características de entrada manuscrita

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

### Lápiz personalizado

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
4. Especifica que la clase CalligraphicPen se deriva de [InkToolbarCustomPen](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.aspx).
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```
5. Invalida [CreateInkDrawingAttributesCore](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore.aspx) para especificar tu propio tamaño de trazo y pincel.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```
6. Crea un objeto [InkDrawingAttributes](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.aspx) y establece las propiedades de [forma de la punta del lápiz](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentip.aspx), [rotación de la punta](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentiptransform.aspx), [tamaño del trazo](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.size.aspx) y [color del trazo](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.color.aspx).
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

1. Declaramos un diccionario de recursos de página local que cree una referencia al lápiz personalizado (`CalligraphicPen`) definido en CalligraphicPen.cs y una [colección de pinceles](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.UI.Xaml.Media.BrushCollection.aspx) compatibles con el lápiz personalizado (`CalligraphicPenPalette`).
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
2. A continuación, agregamos un control InkToolbar con un elemento [InkToolbarCustomPenButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompenbutton.aspx) secundario.

  El botón de lápiz personalizado incluye las dos referencias de recursos estáticos declaradas en los recursos de página: `CalligraphicPen` y `CalligraphicPenPalette`.

  También especificamos el alcance del control deslizante de tamaño de trazo ([MinStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth.aspx), [MaxStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth.aspx) y [SelectedStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty.aspx)), el pincel seleccionado ([SelectedBrushIndex](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex.aspx)) y el icono para el botón de lápiz personalizado ([SymbolIcon](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.symbolicon.aspx)).
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

<!--
### Custom toggle

Enable touch inking

>**Note**&nbsp;&nbsp;InkToolbar supports pen and mouse input and can be configured to recognize touch input.
-->


## Artículos relacionados

* [Interacciones de pluma y lápiz](pen-and-stylus-interactions.md)

**Muestras**
* [Muestra de entrada de lápiz](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Muestra de entrada de lápiz simple](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Muestra de entrada de lápiz compleja](http://go.microsoft.com/fwlink/p/?LinkID=620314)



<!--HONumber=Aug16_HO3-->


