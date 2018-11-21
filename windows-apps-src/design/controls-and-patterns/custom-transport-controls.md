---
author: Jwmsft
Description: The media player has customizable XAML transport controls to manage control of audio and video content.
title: Crea controles de transporte de contenido multimedia personalizados
ms.assetid: 6643A108-A6EB-42BC-B800-22EABD7B731B
label: Create custom media transport controls
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0486fbab829b7c4ce501f9ac20dac97e6abef595
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7430902"
---
# <a name="create-custom-transport-controls"></a>Crear controles de transporte personalizados



La clase MediaPlayerElement tiene controles de transporte de XAML personalizables para administrar el control del contenido de audio y vídeo dentro de una aplicación para Plataforma universal de Windows (UWP). Aquí te mostramos cómo personalizar la platilla MediaTransportControls. Te mostraremos cómo trabajar con el menú de desbordamiento, agregar un botón personalizado y modificar el control deslizante.

> **API importantes**: [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx), [MediaPlayerElement.AreTransportControlsEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aretransportcontrolsenabled.aspx), [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn278677)

Antes de empezar, debes estar familiarizado con las clases MediaPlayerElement y MediaTransportControls. Para obtener más información, consulta la guía de control de MediaPlayerElement.

> [!TIP]
> Los ejemplos de este tema se basan en la [muestra de controles de transporte de contenido multimedia](http://go.microsoft.com/fwlink/p/?LinkId=620023). Puedes descargar la muestra para ver y ejecutar el código completo.

> [!NOTE]
> **MediaPlayerElement** solo está disponible en Windows 10, versión 1607 y posteriores. Si vas a desarrollar una aplicación para una versión anterior de Windows 10, tendrás que usar [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) en su lugar. Todos los ejemplos de esta página también funcionan con **MediaElement**.

## <a name="when-should-you-customize-the-template"></a>¿Cuándo deberías personalizar la plantilla?

**MediaPlayerElement** tiene controles de transporte integrados que se han diseñado para funcionar adecuadamente sin modificaciones con la mayoría de las aplicaciones de reproducción de audio y vídeo. La clase [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) proporciona estos controles e incluyen botones para reproducir y detener contenido multimedia, así como navegar por él, ajustar el volumen, alternar entre pantalla completa, difundir a un segundo dispositivo, habilitar los subtítulos, cambiar las pistas de audio y ajustar la velocidad de reproducción. MediaTransportControls cuenta con propiedades que permiten controlar si cada botón se muestra y se habilita. También puedes establecer la propiedad [**IsCompact**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.iscompact.aspx) para especificar si los controles se muestran en una o dos filas.

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

[**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.controltemplate.aspx) forma parte del estilo predeterminado. El estilo predeterminado del control de transporte se muestra en la página de referencia de clase de [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx). Puedes copiar este estilo predeterminado en el proyecto para modificarlo. ControlTemplate se divide en secciones similares a otras plantillas de control XAML.
- La primera sección de la plantilla contiene las definiciones de [**Style**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx) para los distintos componentes de MediaTransportControls.
- La segunda define los diversos estados visuales que usa MediaTransportControls.
- En la tercera sección se incluye [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx), que contiene los distintos elementos MediaTransportControls juntos y define la distribución de los componentes.

> [!NOTE]
> Para más información sobre cómo modificar las plantillas de control, consulta [Plantillas de control](). Puedes usar un editor de texto o un editor similar en el IDE para abrir los archivos XAML en \(*Program Files*)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\\(*SDK version*)\Generic. El estilo predeterminado y la plantilla para cada control se definen en el archivo **generic.xaml**. Puedes encontrar la plantilla MediaTransportControls en generic.xaml si buscas "MediaTransportControls".

En las siguientes secciones se muestra cómo personalizar muchos de los elementos principales de los controles de transporte:
- [
              Clase **Slider**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx): permite que un usuario arrastre a través de sus elementos multimedia y también muestra el progreso.
- Clase [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx): contiene todos los botones.
Para obtener más información, consulta la sección de anatomía del tema de referencia MediaTransportControls.

## <a name="customize-the-transport-controls"></a>Personalizar los controles de transporte

Si solo quieres modificar la apariencia de MediaTransportControls, también puedes crear una copia del estilo de control predeterminado y la plantilla, y modificarlas. Sin embargo, si también quieres agregar o modificar la funcionalidad del control, debes crear una nueva clase que derive de MediaTransportControls.

### <a name="re-template-the-control"></a>Crear una nueva plantilla del control

**Personalizar la plantilla y el estilo predeterminado de MediaTransportControls**
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

Para obtener más información sobre cómo modificar estilos y plantillas, consulta [Controles de estilos]() y [Plantillas de control]().

### <a name="create-a-derived-control"></a>Crear un control derivado

Para agregar o modificar la funcionalidad de los controles de transporte, debes crear una nueva clase derivada de MediaTransportControls. Una clase derivada llamada `CustomMediaTransportControls` se muestra en la [muestra de controles de transporte multimedia](http://go.microsoft.com/fwlink/p/?LinkId=620023) y los ejemplos restantes en esta página.

**Crear una nueva clase derivada de MediaTransportControls**
1. Agrega un nuevo archivo de clase al proyecto.
    - En Visual Studio, selecciona Proyecto > Agregar clase. Se abre el cuadro de diálogo Agregar nuevo elemento.
    - En el cuadro de diálogo Agregar nuevo elemento, escribe un nombre para el archivo de clase y haz clic en Agregar. (En la muestra de controles de transporte multimedia, la clase se denomina `CustomMediaTransportControls`).
2. Modifica el código de clase que se debe derivar de la clase MediaTransportControls.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
}
```

3. Copia el estilo predeterminado de [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) en [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.resourcedictionary.aspx) del proyecto. Este es el estilo y la plantilla que modificas.
(En la muestra de controles de transporte multimedia, se crea una nueva carpeta denominada "Themes" y se le agrega un archivo ResourceDictionary denominado generic.xaml).
4. Cambia el [**TargetType**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.targettype.aspx) del estilo al nuevo tipo de control personalizado. (En el ejemplo, TargetType se cambia a `local:CustomMediaTransportControls`).

```xaml
xmlns:local="using:CustomMediaTransportControls">
...
<Style TargetType="local:CustomMediaTransportControls">
```

5. Establece el valor [**DefaultStyleKey**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.defaultstylekey.aspx) de la clase personalizada. Esto indica a la clase personalizada que debe usar Style con un TargetType de `local:CustomMediaTransportControls`.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }
}
```

6. Agrega una clase [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) al marcado XAML y agrégale los controles de transporte personalizados. Algo a tener en cuenta es que las API para ocultar, mostrar, deshabilitar y habilitar los botones predeterminados funcionan con una plantilla personalizada.

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

En la plantilla MediaTransportControls, los botones de comando se encuentran en un elemento [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx). La barra de comandos tiene el concepto de comandos principales y secundarios. Los comandos principales son los botones que aparecen en el control de manera predeterminada y están siempre visibles (a menos que deshabilites u ocultes el botón o no haya suficiente lugar). Los comandos secundarios se muestran en un menú de desbordamiento que aparece cuando un usuario hace clic en el botón de puntos suspensivos (…). Para obtener más información, consulta el artículo sobre [barras de la aplicación y barras de comandos](app-bars.md).

Para mover un elemento de los comandos principales de la barra de comandos al menú de desbordamiento, tienes que editar la plantilla de control XAML.

**Mover un comando al menú de desbordamiento:**
1. En la plantilla de control, busca el elemento CommandBar denominado `MediaControlsCommandBar`.
2. Agrega una sección [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) al código XAML para CommandBar. Colócala después de la etiqueta de cierre [**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx).

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

3. Para rellenar el menú con los comandos, corta y pega el código XAML de los objetos [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx) deseados de PrimaryCommands a SecondaryCommands. En este ejemplo, movemos el `PlaybackRateButton` al menú de desbordamiento.

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

Una razón por la que quizás quieras personalizar MediaTransportControls es para agregar un comando personalizado al control. Ya sea para agregarlo como un comando principal o secundario, el procedimiento para crear el botón de comando y modificar su comportamiento es el mismo. En la [muestra de controles de transporte multimedia](http://go.microsoft.com/fwlink/p/?LinkId=620023), se agrega un botón "rating" a los comandos principales.

**Agregar un botón de comando personalizado**
1. Crea un objeto AppBarButton y agrégalo a CommandBar de la plantilla de control.

```xaml
<AppBarButton x:Name="LikeButton"
              Icon="Like"
              Style="{StaticResource AppBarButtonStyle}"
              MediaTransportControlsHelper.DropoutOrder="3"
              VerticalAlignment="Center" />
```

    You must add it to the CommandBar in the appropriate location. (For more info, see the Working with the overflow menu section.) How it's positioned in the UI is determined by where the button is in the markup. For example, if you want this button to appear as the last element in the primary commands, add it at the very end of the primary commands list.

    You can also customize the icon for the button. For more info, see the [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx) reference.

2. En la invalidación de [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.onapplytemplate.aspx), obtén el botón a partir de la plantilla y registra un controlador para su evento [**Click**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.buttonbase.click.aspx). Este código se coloca en la clase `CustomMediaTransportControls`.

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

**Controles de transporte de contenido multimedia personalizados con un botín "Me gusta" agregado**
![Control de transporte de contenido multimedia personalizado con un botón Me gusta adicional](images/controls/mtc_double_custom_inprod.png)

### <a name="modifying-the-slider"></a>Modificar el control deslizante

Un elemento [**Slider**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx) proporciona el control "seek" de MediaTransportControls. Una manera de personalizarlo es cambiando la granularidad del comportamiento de búsqueda.

El control deslizante de búsqueda predeterminado se divide en 100 partes, por lo que el comportamiento de búsqueda se limita a ese número de secciones. Puedes cambiar la granularidad del control deslizante de búsqueda al obtener el elemento Slider del árbol visual de XAML de tu controlador de eventos [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.mediaopened.aspx) en [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx). En este ejemplo se muestra cómo usar [**VisualTreeHelper**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.visualtreehelper.aspx) para obtener una referencia a Slider y luego cambiar la frecuencia de paso predeterminado del control deslizante del 1% al 0,1% (1000 pasos) si el contenido multimedia tiene más de 120minutos. MediaPlayerElement se denomina `MediaPlayerElement1`.

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

- [Reproducción de contenido multimedia](media-playback.md)
