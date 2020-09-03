---
Description: El reproductor multimedia tiene controles de transporte en XAML personalizados para administrar el control de contenido de audio y vídeo
title: Crear controles de transporte de contenido multimedia personalizados
ms.assetid: 6643A108-A6EB-42BC-B800-22EABD7B731B
label: Create custom media transport controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f5dd1a27cb02a33a8d760f4d902a42c6619ad796
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160419"
---
# <a name="create-custom-transport-controls"></a>Crear controles de transporte personalizados



La clase MediaPlayerElement tiene controles de transporte de XAML personalizables para administrar el control del contenido de audio y vídeo dentro de una aplicación de Windows. Aquí te mostramos cómo personalizar la platilla MediaTransportControls. Te mostraremos cómo trabajar con el menú de desbordamiento, agregar un botón personalizado y modificar el control deslizante.

> **API importantes**: [MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement), [MediaPlayerElement.AreTransportControlsEnabled](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.aretransportcontrolsenabled), [MediaTransportControls](/uwp/api/Windows.Media.SystemMediaTransportControls)

Antes de empezar, debes estar familiarizado con las clases MediaPlayerElement y MediaTransportControls. Para obtener más información, consulta la guía de control de MediaPlayerElement.

> [!TIP]
> Los ejemplos de este tema se basan en la [muestra de controles de transporte de contenido multimedia](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCustomMediaTransportControls). Puedes descargar la muestra para ver y ejecutar el código completo.

> [!NOTE]
> **MediaPlayerElement** solo está disponible en Windows 10, versión 1607 y posteriores. Si vas a desarrollar una aplicación para una versión anterior de Windows 10, tendrás que usar [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) en su lugar. Todos los ejemplos de esta página también funcionan con **MediaElement**.

## <a name="when-should-you-customize-the-template"></a>¿Cuándo deberías personalizar la plantilla?

**MediaPlayerElement** tiene controles de transporte integrados que se han diseñado para funcionar adecuadamente sin modificaciones con la mayoría de las aplicaciones de reproducción de audio y vídeo. La clase [**MediaTransportControls**](/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) proporciona estos controles e incluyen botones para reproducir y detener contenido multimedia, así como navegar por él, ajustar el volumen, alternar entre pantalla completa, difundir a un segundo dispositivo, habilitar los subtítulos, cambiar las pistas de audio y ajustar la velocidad de reproducción. MediaTransportControls cuenta con propiedades que permiten controlar si cada botón se muestra y se habilita. También puedes establecer la propiedad [**IsCompact**](/uwp/api/windows.ui.xaml.controls.mediatransportcontrols.iscompact) para especificar si los controles se muestran en una o dos filas.

Sin embargo, puede haber escenarios donde tendrás que personalizar aún más personalizar el aspecto del control o cambiar su comportamiento. A continuación, se muestran algunos ejemplos:
- Cambiar los iconos, el comportamiento de los controles deslizantes y los colores.
- Mover los botones de comando que se usan con menos frecuencia a un menú de desbordamiento.
- Cambiar el orden en que los comandos desaparecen cuando el control cambia de tamaño.
- Proporcionar un botón de comando que no forma parte del conjunto predeterminado.

> [!NOTE]
> Los botones que se muestran en la pantalla desaparecerán de los controles de transporte integrados en un orden predefinido si no hay suficiente espacio en la pantalla. Para cambiar este orden o colocar los comandos que no caben en un menú de desbordamiento, tienes que personalizar los controles.

Puedes personalizar el aspecto del control al modificar la plantilla predeterminada. Para modificar el comportamiento del control o agregar comandos nuevos, puedes crear un control personalizado derivado de MediaTransportControls.

> [!TIP]
> Las plantillas de control personalizables son una característica eficaz de la plataforma XAML, pero también hay consecuencias que deberías tener en cuenta. Al personalizar una plantilla, esta se convierte en un elemento estático de la aplicación y, por tanto, no recibirá ninguna actualización de plataforma que Microsoft realice en la plantilla. Si Microsoft realiza alguna actualización de la plantilla, se debería tomar la nueva plantilla y volver a modificarla para obtener las ventajas de la plantilla actualizada.

## <a name="template-structure"></a>Estructura de la plantilla

[  **ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) forma parte del estilo predeterminado. Puedes copiar este estilo predeterminado en el proyecto para modificarlo. ControlTemplate se divide en secciones similares a otras plantillas de control XAML.
- La primera sección de la plantilla contiene las definiciones de [**Style**](/uwp/api/Windows.UI.Xaml.Style) para los distintos componentes de MediaTransportControls.
- La segunda define los diversos estados visuales que usa MediaTransportControls.
- En la tercera sección se incluye [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid), que contiene los distintos elementos MediaTransportControls juntos y define la distribución de los componentes.

> [!NOTE]
> Para más información sobre cómo modificar las plantillas de control, consulta [Plantillas de control](./control-templates.md). Puedes usar un editor de texto o un editor similar en el IDE para abrir los archivos XAML en \(*Program Files*)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\\(*SDK version*)\Generic. El estilo predeterminado y la plantilla para cada control se definen en el archivo **generic.xaml**. Puedes encontrar la plantilla MediaTransportControls en generic.xaml si buscas "MediaTransportControls".

En las siguientes secciones se muestra cómo personalizar muchos de los elementos principales de los controles de transporte:
- [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider): permite que un usuario arrastre a través de sus elementos multimedia y también muestra el progreso.
- [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.CommandBar): contiene todos los botones.
Para obtener más información, consulta la sección de anatomía del tema de referencia MediaTransportControls.

## <a name="customize-the-transport-controls"></a>Personalizar los controles de transporte

Si solo quieres modificar la apariencia de MediaTransportControls, también puedes crear una copia del estilo de control predeterminado y la plantilla, y modificarlas. Sin embargo, si también quieres agregar o modificar la funcionalidad del control, debes crear una nueva clase que derive de MediaTransportControls.

### <a name="re-template-the-control"></a>Crear una nueva plantilla del control

**Para personalizar la plantilla y el estilo predeterminados de MediaTransportControls**
1. Copia el estilo predeterminado de los estilos y plantillas de MediaTransportControls en un ResourceDictionary del proyecto.
2. Asigna un valor x:Key a Style para identificarlo del modo siguiente.

```xaml
<Style TargetType="MediaTransportControls" x:Key="myTransportControlsStyle">
    <!-- Style content ... -->
</Style>
```

3. Agrega una clase MediaPlayerElement con MediaTransportControls a la interfaz de usuario.
4. Establece la propiedad Style del elemento MediaTransportControls al recurso personalizado Style, tal como se muestra aquí.

```xaml
<MediaPlayerElement AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls Style="{StaticResource myTransportControlsStyle}"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```

Para obtener más información sobre cómo modificar estilos y plantillas, consulta [Controles de estilos](./xaml-styles.md) y [Plantillas de control](./control-templates.md).

### <a name="create-a-derived-control"></a>Crear un control derivado

Para agregar o modificar la funcionalidad de los controles de transporte, debes crear una nueva clase derivada de MediaTransportControls. Una clase derivada llamada `CustomMediaTransportControls` se muestra en la [muestra de controles de transporte multimedia](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCustomMediaTransportControls) y los ejemplos restantes en esta página.

**Para crear una nueva clase derivada de MediaTransportControls**
1. Agrega un nuevo archivo de clase al proyecto.
    - En Visual Studio, selecciona Proyecto > Agregar clase. Se abre el cuadro de diálogo Agregar nuevo elemento.
    - En el cuadro de diálogo Agregar nuevo elemento, escribe un nombre para el archivo de clase y haz clic en Agregar. (En la muestra de controles de transporte multimedia, la clase se denomina `CustomMediaTransportControls`).
2. Modifica el código de clase que se debe derivar de la clase MediaTransportControls.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
}
```

3. Copia el estilo predeterminado de [**MediaTransportControls**](/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) en [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) del proyecto. Este es el estilo y la plantilla que modificas.
(En la muestra de controles de transporte multimedia, se crea una nueva carpeta denominada "Themes" y se le agrega un archivo ResourceDictionary denominado generic.xaml).
4. Cambia el [**TargetType**](/uwp/api/windows.ui.xaml.style.targettype) del estilo al nuevo tipo de control personalizado. (En el ejemplo, TargetType se cambia a `local:CustomMediaTransportControls`).

```xaml
xmlns:local="using:CustomMediaTransportControls">
...
<Style TargetType="local:CustomMediaTransportControls">
```

5. Establece el valor [**DefaultStyleKey**](/uwp/api/windows.ui.xaml.controls.control.defaultstylekey) de la clase personalizada. Esto indica a la clase personalizada que debe usar Style con un TargetType de `local:CustomMediaTransportControls`.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }
}
```

6. Agrega una clase [**MediaPlayerElement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) al marcado XAML y agrégale los controles de transporte personalizados. Algo a tener en cuenta es que las API para ocultar, mostrar, deshabilitar y habilitar los botones predeterminados funcionan con una plantilla personalizada.

```xaml
<MediaPlayerElement Name="MediaPlayerElement1" AreTransportControlsEnabled="True" Source="video.mp4">
    <MediaPlayerElement.TransportControls>
        <local:CustomMediaTransportControls x:Name="customMTC"
                                            IsFastForwardButtonVisible="True"
                                            IsFastForwardEnabled="True"
                                            IsFastRewindButtonVisible="True"
                                            IsFastRewindEnabled="True"
                                            IsPlaybackRateButtonVisible="True"
                                            IsPlaybackRateEnabled="True"
                                            IsCompact="False">
        </local:CustomMediaTransportControls>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```

Ahora puedes modificar el estilo y la plantilla de control para actualizar el aspecto del control personalizado y el código de control para actualizar su comportamiento.

### <a name="working-with-the-overflow-menu"></a>Trabajar con el menú de desbordamiento

Puedes mover los botones de comando de MediaTransportControls en un menú de desbordamiento para que los comandos menos usados queden ocultos hasta que el usuario los necesite.

En la plantilla MediaTransportControls, los botones de comando se encuentran en un elemento [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.CommandBar). La barra de comandos tiene el concepto de comandos principales y secundarios. Los comandos principales son los botones que aparecen en el control de manera predeterminada y están siempre visibles (a menos que deshabilites u ocultes el botón o no haya suficiente lugar). Los comandos secundarios se muestran en un menú de desbordamiento que aparece cuando un usuario hace clic en el botón de puntos suspensivos (…). Para obtener más información, consulta el artículo sobre [barras de la aplicación y barras de comandos](app-bars.md).

Para mover un elemento de los comandos principales de la barra de comandos al menú de desbordamiento, tienes que editar la plantilla de control XAML.

**Para mover un comando al menú de desbordamiento:**
1. En la plantilla de control, busca el elemento CommandBar denominado `MediaControlsCommandBar`.
2. Agrega una sección [**SecondaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) al código XAML para CommandBar. Colócala después de la etiqueta de cierre [**PrimaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands).

```xaml
<CommandBar x:Name="MediaControlsCommandBar" ... >  
  <CommandBar.PrimaryCommands>
...
    <AppBarButton x:Name='PlaybackRateButton'
                    Style='{StaticResource AppBarButtonStyle}'
                    MediaTransportControlsHelper.DropoutOrder='4'
                    Visibility='Collapsed'>
      <AppBarButton.Icon>
        <FontIcon Glyph="&#xEC57;"/>
      </AppBarButton.Icon>
    </AppBarButton>
...
  </CommandBar.PrimaryCommands>
<!-- Add secondary commands (overflow menu) here -->
  <CommandBar.SecondaryCommands>
    ...
  </CommandBar.SecondaryCommands>
</CommandBar>
```

3. Para rellenar el menú con los comandos, corta y pega el código XAML de los objetos [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) deseados de PrimaryCommands a SecondaryCommands. En este ejemplo, movemos el `PlaybackRateButton` al menú de desbordamiento.

4. Agrega una etiqueta al botón y quita la información de estilo, tal como se muestra aquí.
Dado que el menú de desbordamiento se compone de botones de texto, tienes que agregar una etiqueta de texto al botón y también quitar el estilo que establece el alto y ancho del botón. De lo contrario, no aparecerá correctamente en el menú de desbordamiento.

```xaml
<CommandBar.SecondaryCommands>
    <AppBarButton x:Name='PlaybackRateButton'
                  Label='Playback Rate'>
    </AppBarButton>
</CommandBar.SecondaryCommands>
```

> [!IMPORTANT]
> Aún puedes hacer que el botón sea visible y se pueda habilitar para poder usarlo en el menú de desbordamiento. En este ejemplo, el elemento PlaybackRateButton no está visible en el menú de desbordamiento, a menos que la propiedad IsPlaybackRateButtonVisible sea true. No se habilita, a menos que la propiedad IsPlaybackRateEnabled sea true. El establecimiento de estas propiedades se muestra en la sección anterior.

### <a name="adding-a-custom-button"></a>Agregar un botón personalizado

Una razón por la que quizás quieras personalizar MediaTransportControls es para agregar un comando personalizado al control. Ya sea para agregarlo como un comando principal o secundario, el procedimiento para crear el botón de comando y modificar su comportamiento es el mismo. En la [muestra de controles de transporte multimedia](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCustomMediaTransportControls), se agrega un botón "rating" a los comandos principales.

**Para agregar un botón de comando personalizado**
1. Crea un objeto AppBarButton y agrégalo a CommandBar de la plantilla de control.

```xaml
<AppBarButton x:Name="LikeButton"
              Icon="Like"
              Style="{StaticResource AppBarButtonStyle}"
              MediaTransportControlsHelper.DropoutOrder="3"
              VerticalAlignment="Center" />
```

Debes agregarlo a CommandBar en la ubicación adecuada. (Para obtener más información, consulta la sección Trabajar con el menú de desbordamiento). La posición que ocupa en la interfaz de usuario la determina el lugar donde se encuentra el botón en el marcado. Por ejemplo, si quieres que este botón aparezca como el último elemento en los comandos principales, agrégalo al final de la lista de comandos principal.

También puedes personalizar el icono del botón. Para obtener más información, consulta la referencia <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton"><b>AppBarButton</b></a>.
    

2. En la invalidación de [**OnApplyTemplate**](/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate), obtén el botón a partir de la plantilla y registra un controlador para su evento [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Este código se coloca en la clase `CustomMediaTransportControls`.

```csharp
public sealed class CustomMediaTransportControls :  MediaTransportControls
{
    // ...

    protected override void OnApplyTemplate()
    {
        // Find the custom button and create an event handler for its Click event.
        var likeButton = GetTemplateChild("LikeButton") as Button;
        likeButton.Click += LikeButton_Click;
        base.OnApplyTemplate();
    }

    //...
}
```

3. Agrega código al controlador de eventos "Click" para realizar la acción que ocurre cuando se hace clic en el botón.
Este es el código completo para la clase.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public event EventHandler< EventArgs> Liked;

    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }

    protected override void OnApplyTemplate()
    {
        // Find the custom button and create an event handler for its Click event.
        var likeButton = GetTemplateChild("LikeButton") as Button;
        likeButton.Click += LikeButton_Click;
        base.OnApplyTemplate();
    }

    private void LikeButton_Click(object sender, RoutedEventArgs e)
    {
        // Raise an event on the custom control when 'like' is clicked.
        var handler = Liked;
        if (handler != null)
        {
            handler(this, EventArgs.Empty);
        }
    }
}
```

**Controles de transporte de contenido multimedia personalizados con un botón "Me gusta" agregado**
![Control de transporte de contenido multimedia personalizado con un botón Me gusta adicional](images/controls/mtc_double_custom_inprod.png)

### <a name="modifying-the-slider"></a>Modificar el control deslizante

Un elemento [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider) proporciona el control "seek" de MediaTransportControls. Una manera de personalizarlo es cambiando la granularidad del comportamiento de búsqueda.

El control deslizante de búsqueda predeterminado se divide en 100 partes, por lo que el comportamiento de búsqueda se limita a ese número de secciones. Puedes cambiar la granularidad del control deslizante de búsqueda al obtener el elemento Slider del árbol visual de XAML de tu controlador de eventos [**MediaOpened**](/uwp/api/windows.media.playback.mediaplayer.mediaopened) en [**MediaPlayerElement.MediaPlayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement). En este ejemplo se muestra cómo usar [**VisualTreeHelper**](/uwp/api/Windows.UI.Xaml.Media.VisualTreeHelper) para obtener una referencia a Slider y luego cambiar la frecuencia de paso predeterminado del control deslizante del 1 % al 0,1 % (1000 pasos) si el contenido multimedia tiene más de 120 minutos. MediaPlayerElement se denomina `MediaPlayerElement1`.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
  MediaPlayerElement1.MediaPlayer.MediaOpened += MediaPlayerElement_MediaPlayer_MediaOpened;
  base.OnNavigatedTo(e);
}

private void MediaPlayerElement_MediaPlayer_MediaOpened(object sender, RoutedEventArgs e)
{
  FrameworkElement transportControlsTemplateRoot = (FrameworkElement)VisualTreeHelper.GetChild(MediaPlayerElement1.TransportControls, 0);
  Slider sliderControl = (Slider)transportControlsTemplateRoot.FindName("ProgressSlider");
  if (sliderControl != null && MediaPlayerElement1.NaturalDuration.TimeSpan.TotalMinutes > 120)
  {
    // Default is 1%. Change to 0.1% for more granular seeking.
    sliderControl.StepFrequency = 0.1;
  }
}
```
## <a name="related-articles"></a>Artículos relacionados

- [Reproducción de multimedia](media-playback.md)