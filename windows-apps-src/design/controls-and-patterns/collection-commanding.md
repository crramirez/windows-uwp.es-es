---
description: Cómo usar los comandos contextuales para implementar estos tipos de acciones de manera que ofrezcan la mejor experiencia posible para todos los tipos de entrada.
title: Comandos contextuales
ms.assetid: ''
label: Contextual commanding in collections
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 56c845186c9e5351abefd4a6d53175e43b11ae58
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217538"
---
# <a name="contextual-commanding-for-collections-and-lists"></a>Comandos contextuales en colecciones y listas



Muchas aplicaciones contienen colecciones de contenido en forma de listas, cuadrículas y árboles que los usuarios pueden manipular. Por ejemplo, es posible que los usuarios puedan eliminar, cambiar el nombre, marcar o actualizar elementos. En este artículo se muestra cómo usar los comandos contextuales para implementar estos tipos de acciones de manera que ofrezcan la mejor experiencia posible para todos los tipos de entrada.  

> **API importantes**: [Interfaz ICommand](/uwp/api/Windows.UI.Xaml.Input.ICommand), [Propiedad UIElement.ContextFlyout](/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) e [Interfaz INotifyPropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged)

![Uso de diversas entradas para ejecutar el comando Favorito](images/ContextualCommand_AddFavorites.png)

## <a name="creating-commands-for-all-input-types"></a>Creación de comandos para todos los tipos de entrada

Dado que los usuarios pueden interactuar con una aplicación de Windows mediante [una amplia gama de dispositivos y entradas](../devices/index.md), la aplicación debe exponer los comandos tanto a través de menús contextuales independientes de entrada como de aceleradores específicos de entrada. La inclusión de ambas opciones permite al usuario invocar rápidamente comandos en el contenido, independientemente del tipo de entrada o dispositivo.

En esta tabla se muestran algunos comandos de colección típicos y formas de exponer los comandos. 

| Comando          | Independiente de la entrada | Acelerador de mouse | Teclas de aceleración | Acelerador de entrada táctil |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| Eliminar elemento      | Menú contextual   | Botón interactivo      | Tecla Supr              | Deslizar el dedo para eliminar   |
| Marcar elemento        | Menú contextual   | Botón interactivo      | Ctrl + Mayús + G         | Deslizar el dedo para marcar     |
| Actualizar datos     | Menú contextual   | N/A               | Tecla F5               | Extraer para actualizar   |
| Marcar como favorito un elemento | Menú contextual   | Botón interactivo      | F, Ctrl+S            | Deslizar el dedo para favorito |


* **En general, debes hacer que todos los comandos de un elemento estén disponibles en el [menú contextual](menus.md) del elemento** . Los menús contextuales son accesibles para los usuarios independientemente del tipo de entrada y deben contener todos los comandos contextuales que el usuario puede realizar.

* **Para los comandos a los que se accede con frecuencia, piensa en la posibilidad de usar aceleradores de entrada.** Los aceleradores de entrada permiten al usuario realizar acciones rápidamente, en función de su dispositivo de entrada. Entre los aceleradores de entrada se incluyen:
    - Deslizar el dedo para la acción (acelerador de entrada táctil)
    - Extraer para actualizar datos (acelerador de entrada táctil)
    - Métodos abreviados de teclado (acelerador de teclado)
    - Teclas de acceso (acelerador de teclado)
    - Botones interactivos de mouse y lápiz (acelerador de puntero)

> [!NOTE]
> Los usuarios han de poder acceder a todos los comandos desde cualquier tipo de dispositivo. Por ejemplo, si los comandos de la aplicación solo se exponen a través de los aceleradores de puntero de botones interactivos, los usuarios táctiles no podrán obtener acceso a ellos. Como mínimo, usa un menú contextual para proporcionar acceso a todos los comandos.  

## <a name="example-the-podcastobject-data-model"></a>Ejemplo: Modelo de datos de PodcastObject

Para demostrar nuestras recomendaciones de comandos, en este artículo se crea una lista de podcasts para una aplicación de podcasts. El código de ejemplo muestra cómo permitir al usuario marcar como "favorito" un podcast concreto en una lista.

Esta es la definición del objeto de podcast con la que trabajaremos: 

```csharp
public class PodcastObject : INotifyPropertyChanged
{
    // The title of the podcast
    public String Title { get; set; }

    // The podcast's description
    public String Description { get; set; }

    // Describes if the user has set this podcast as a favorite
    public bool IsFavorite
    {
        get
        {
            return _isFavorite;
        }
        set
        {
            _isFavorite = value;
            OnPropertyChanged("IsFavorite");
        }
    }
    private bool _isFavorite = false;

    public event PropertyChangedEventHandler PropertyChanged;

    private void OnPropertyChanged(String property)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(property));
    }
}
```

Ten en cuenta que el PodcastObject implementa [INotifyPropertyChanged](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged) para responder a los cambios de propiedad cuando el usuario alterna la propiedad IsFavorite.

## <a name="defining-commands-with-the-icommand-interface"></a>Definir comandos con la interfaz ICommand

La [interfaz ICommand](/uwp/api/Windows.UI.Xaml.Input.ICommand) te ayuda a definir un comando que está disponible para varios tipos de entrada. Por ejemplo, en lugar de escribir el mismo código para un comando Eliminar en dos controladores de eventos diferentes, uno para cuando el usuario presiona la tecla Supr y otro para cuando el usuario hace clic con el botón derecho en "Eliminar" en un menú contextual, puedes implementar una vez la lógica de eliminar, como [ICommand](/uwp/api/Windows.UI.Xaml.Input.ICommand), y que esté disponible para diferentes tipos de entrada.

Tenemos que definir el ICommand que representa la acción "Favorito". Usaremos el método [Execute](/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute) del comando para marcar un podcast como favorito. El podcast en particular se proporcionará al método de ejecución a través del parámetro del comando, que se puede enlazar con la propiedad CommandParameter.

```csharp
public class FavoriteCommand: ICommand
{
    public event EventHandler CanExecuteChanged;

    public bool CanExecute(object parameter)
    {
        return true;
    }
    public void Execute(object parameter)
    {
        // Perform the logic to "favorite" an item.
        (parameter as PodcastObject).IsFavorite = true;
    }
}
```

Para usar el mismo comando con varios elementos y colecciones, puedes almacenar el comando como recurso en la página o en la aplicación.

```xaml
<Application.Resources>
    <local:FavoriteCommand x:Key="favoriteCommand" />
</Application.Resources>
```

Para ejecutar el comando, llama a su método [Execute](/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute).

```csharp
// Favorite the item using the defined command
var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
favoriteCommand.Execute(PodcastObject);
```


## <a name="creating-a-usercontrol-to-respond-to-a-variety-of-inputs"></a>Crear un objeto UserControl para responder a diversas entradas

Cuando tienes una lista de elementos y cada uno de estos elementos debe responder a varias entradas, puedes simplificar el código mediante la definición de un [UserControl](/uwp/api/Windows.UI.Xaml.Controls.UserControl) para el elemento y usarlo para definir los controladores de eventos y el menú contextual de los elementos. 

Para crear un UserControl en Visual Studio:
1. En el Explorador de soluciones, haz clic con el botón derecho en el proyecto. Aparecerá un menú contextual.
2. Selecciona **Agregar > Nuevo elemento...** <br />Se abrirá el cuadro de diálogo **Agregar nuevo elemento**. 
3. Selecciona UserControl en la lista de elementos. Asígnale el nombre que quieras y haz clic en **Agregar**. Visual Studio generará automáticamente un UserControl de código auxiliar. 

En nuestro ejemplo de podcast, cada podcast se mostrará en una lista, que expondrá diversas formas de marcar como "favorito" un podcast. El usuario podrá realizar las siguientes acciones para marcar como "favorito" el podcast:
- Invocar un menú contextual
- Ejecutar métodos abreviados de teclado
- Mostrar un botón interactivo
- Realizar un gesto de deslizar el dedo

Con el fin de encapsular estos comportamientos y usar el FavoriteCommand, vamos a crear un nuevo [UserControl](/uwp/api/Windows.UI.Xaml.Controls.UserControl) denominado "PodcastUserControl" para representar un podcast en la lista.

El PodcastUserControl muestra los campos del PodcastObject como TextBlocks y responde a distintas interacciones del usuario. Haremos referencia al PodcastUserControl y lo ampliaremos a lo largo de este artículo.

**PodcastUserControl.xaml**
```xaml
<UserControl
    x:Class="ContextCommanding.PodcastUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    IsTabStop="True" UseSystemFocusVisuals="True"
    >
    <Grid Margin="12,0,12,0">
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**PodcastUserControl.xaml.cs**
```csharp
public sealed partial class PodcastUserControl : UserControl
{
    public static readonly DependencyProperty PodcastObjectProperty =
        DependencyProperty.Register(
            "PodcastObject",
            typeof(PodcastObject),
            typeof(PodcastUserControl),
            new PropertyMetadata(null));

    public PodcastObject PodcastObject
    {
        get { return (PodcastObject)GetValue(PodcastObjectProperty); }
        set { SetValue(PodcastObjectProperty, value); }
    }

    public PodcastUserControl()
    {
        this.InitializeComponent();

        // TODO: We will add event handlers here.
    }
}
```

Ten en cuenta que el PodcastUserControl mantiene una referencia al PodcastObject como DependencyProperty. Esto nos permite enlazar PodcastObjects al PodcastUserControl.

Cuando hayas generado algunos PodcastObjects, puedes crear una lista de podcasts enlazando las instancias de PodcastObject a una ListView. Los objetos PodcastUserControl describen la visualización de los PodcastObjects y, por tanto, se establecen con ItemTemplate de ListView.

**MainPage.xaml**
```xaml
<ListView x:Name="ListOfPodcasts"
            ItemsSource="{x:Bind podcasts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:PodcastObject">
            <local:PodcastUserControl PodcastObject="{x:Bind Mode=OneWay}" />
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <!-- The PodcastUserControl will entirely fill the ListView item and handle tabbing within itself. -->
        <Style TargetType="ListViewItem" BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="HorizontalContentAlignment" Value="Stretch" />
            <Setter Property="Padding" Value="0"/>
            <Setter Property="IsTabStop" Value="False"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

## <a name="creating-context-menus"></a>Crear menús contextuales

Los menús contextuales muestran una lista de opciones o comandos cuando el usuario los solicita. Los menús contextuales proporcionan comandos contextuales relacionados con su elemento adjunto y, por lo general, se reservan para acciones secundarias específicas de ese elemento.

![Mostrar un menú contextual en el elemento](images/ContextualCommand_RightClick.png)

El usuario puede invocar menús contextuales con estas "acciones contextuales":

| Entrada    | Acción de contexto                          |
| -------- | --------------------------------------- |
| Mouse    | Hacer clic con el botón derecho                             |
| Keyboard | Mayús + F10, botón de menú                  |
| Tocar    | Presión larga en el elemento                      |
| Lápiz      | Presión en el botón de menú contextual, presión larga en el elemento |
| Controlador para juegos  | Botón de menú                             |

**Dado que el usuario puede abrir un menú contextual independientemente del tipo de entrada, el menú contextual debe contener todos los comandos contextuales disponibles para el elemento de lista.**

### <a name="contextflyout"></a>ContextFlyout

La [propiedad ContextFlyout](/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout), definida por la clase UIElement, facilita la creación de un menú contextual que funciona con todos los tipos de entradas. Proporcionas un control flotante que representa el menú contextual con MenuFlyout y, cuando el usuario realiza una "acción de contexto" como se ha definido anteriormente, se mostrará el MenuFlyout correspondiente al elemento.

Agregaremos un ContextFlyout al PodcastUserControl. El MenuFlyout especificado como ContextFlyout contiene un solo elemento para marcar como favorito un podcast. Ten en cuenta que este MenuFlyoutItem usa el favoriteCommand definido anteriormente, con el CommandParameter enlazado al PodcastObject.

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <MenuFlyout>
            <MenuFlyoutItem Text="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" />
        </MenuFlyout>
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <!-- ... -->
    </Grid>
</UserControl>

```

Ten en cuenta que también puedes usar el [evento ContextRequested](/uwp/api/Windows.UI.Xaml.UIElement.ContextRequested) para responder a acciones de contexto. El evento ContextRequested no se activará si se ha especificado un ContextFlyout.

## <a name="creating-input-accelerators"></a>Crear aceleradores de entrada

Aunque cada elemento de la colección debe tener un menú contextual con todos los comandos contextuales, es posible que quieras permitir que los usuarios puedan ejecutar rápidamente un conjunto más pequeño de comandos ejecutados con mayor frecuencia. Por ejemplo, una aplicación de correo puede tener comandos secundarios como Responder, Archivar, Mover a carpeta, Establecer marca y Eliminar que aparecen en un menú contextual, pero los comandos más habituales son Eliminar y Marcar. Cuando hayas identificado los comandos que son más habituales, puedes usar aceleradores basados en entrada para facilitar la ejecución de estos comandos por parte de un usuario.

En la aplicación de podcasts, el comando que se ejecuta con mayor frecuencia es "Favorito".

### <a name="keyboard-accelerators"></a>Aceleradores de teclado

#### <a name="shortcuts-and-direct-key-handling"></a>Accesos directos y control directo de teclas

![Presionar Ctrl y F para realizar una acción](images/ContextualCommand_Keyboard.png)

Según el tipo de contenido, puedes identificar determinadas combinaciones de teclas que deben realizar una acción. En una aplicación de correo electrónico, por ejemplo, la tecla Supr puede usarse para eliminar el correo electrónico que está seleccionado. En una aplicación de podcasts, las teclas Ctrl + S o F podrían marcar como favorito un podcast para más adelante. Aunque algunos comandos tienen métodos abreviados de teclado comunes y conocidos como Supr para eliminar, otros comandos tienen métodos abreviados específicos de dominio o aplicación. Si es posible, usa accesos directos conocidos o piensa en la posibilidad de proporcionar texto de aviso en una información sobre herramientas para informar al usuario sobre el comando contextual.

La aplicación puede responder cuando el usuario presiona una tecla con el evento [KeyDown](/uwp/api/Windows.UI.Xaml.UIElement.KeyDownEvent). En general, los usuarios esperan que la aplicación responda al presionar la tecla por primera vez, en lugar de esperar hasta que se libere la tecla.

En este ejemplo se analiza cómo agregar el controlador de KeyDown al PodcastUserControl para marcar como favorito un podcast cuando el usuario presiona Ctrl + S o F. Se usa el mismo comando que antes.

**PodcastUserControl.xaml.cs**
```csharp
// Respond to the F and Ctrl+S keys to favorite the focused item.
protected override void OnKeyDown(KeyRoutedEventArgs e)
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    var isCtrlPressed = (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down || (ctrlState & CoreVirtualKeyStates.Locked) == CoreVirtualKeyStates.Locked;

    if (e.Key == Windows.System.VirtualKey.F || (e.Key == Windows.System.VirtualKey.S && isCtrlPressed))
    {
        // Favorite the item using the defined command
        var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
        favoriteCommand.Execute(PodcastObject);
    }
}
```

### <a name="mouse-accelerators"></a>Aceleradores de mouse

![Mantener el cursor del mouse sobre un elemento para mostrar un botón](images/ContextualCommand_HovertoReveal.png)

Los usuarios están familiarizados con los menús contextuales, pero es posible que quieras permitir que los usuarios ejecuten comandos habituales con solo hacer un clic en el mouse. Para habilitar esta experiencia, puedes incluir botones dedicados en el lienzo del elemento de la colección. Tanto para permitir que los usuarios actúen rápidamente con el mouse como para reducir la aglutinación visual, puedes mostrar solo estos botones cuando el usuario tiene el puntero dentro de un elemento de lista concreto.

En este ejemplo, el comando Favorito está representado por un botón definido directamente en el PodcastUserControl. Ten en cuenta que el botón de este ejemplo usa el mismo comando que antes, FavoriteCommand. Para alternar la visibilidad de este botón, puedes usar el VisualStateManager para cambiar entre los estados visuales cuando el puntero entra y sale del control.

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <!-- ... -->
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="HoveringStates">
                <VisualState x:Name="HoverButtonsShown">
                    <VisualState.Setters>
                        <Setter Target="hoverArea.Visibility" Value="Visible" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="HoverButtonsHidden" />
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
        <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
            <AppBarButton Icon="OutlineStar" Label="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" VerticalAlignment="Stretch"  />
        </Grid>
    </Grid>
</UserControl>
```

Los botones interactivos deben aparecer y desaparecer cuando el mouse entra y sale del elemento. Para responder a eventos de mouse, puedes usar los eventos [PointerEntered](/uwp/api/Windows.UI.Xaml.UIElement.PointerEnteredEvent) y [PointerExited](/uwp/api/Windows.UI.Xaml.UIElement.PointerExitedEvent) en el PodcastUserControl.

**PodcastUserControl.xaml.cs**
```csharp
protected override void OnPointerEntered(PointerRoutedEventArgs e)
{
    base.OnPointerEntered(e);

    // Only show hover buttons when the user is using mouse or pen.
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(this, "HoverButtonsShown", true);
    }
}

protected override void OnPointerExited(PointerRoutedEventArgs e)
{
    base.OnPointerExited(e);

    VisualStateManager.GoToState(this, "HoverButtonsHidden", true);
}
```

Los botones que se muestran en el estado de movimiento solo serán accesibles mediante el tipo de entrada de puntero. Dado que estos botones están limitados a la entrada del puntero, puedes minimizar o quitar el relleno alrededor del icono del botón para optimizar la entrada del puntero. Si decides hacerlo, asegúrate de que la superficie del botón sea de al menos 20 x 20 píxeles para que siga siendo utilizable con lápiz y mouse.

### <a name="touch-accelerators"></a>Aceleradores de entrada táctil

#### <a name="swipe"></a>Deslizar rápidamente

![Deslizar el dedo por un elemento para mostrar el comando](images/ContextualCommand_Swipe.png)

El comando de deslizar el dedo es un acelerador táctil que permite a los usuarios de dispositivos táctiles realizar acciones secundarias habituales con la entrada táctil. Deslizar el dedo permite a los usuarios táctiles interactuar de manera rápida y natural con el contenido, con acciones comunes, como Deslizar el dedo para eliminar o Deslizar el dedo para invocar. Consulta el artículo sobre los [comandos de deslizar el dedo](swipe.md) para obtener más información.

Para integrar el gesto de pasar el dedo en la colección, necesitas dos componentes: SwipeItems, que hospeda los comandos; y SwipeControl, que contiene el elemento y permite la interacción con deslizamiento rápido.

Se puede definir a SwipeItems como recurso en el PodcastUserControl. En este ejemplo, el SwipeItems contiene un comando para marcar como favorito un elemento.

```xaml
<UserControl.Resources>
    <SymbolIconSource x:Key="FavoriteIcon" Symbol="Favorite"/>
    <SwipeItems x:Key="RevealOtherCommands" Mode="Reveal">
        <SwipeItem IconSource="{StaticResource FavoriteIcon}" Text="Favorite" Background="Yellow" Invoked="SwipeItem_Invoked"/>
    </SwipeItems>
</UserControl.Resources>
```

El SwipeControl ajusta el elemento y permite al usuario interactuar con él mediante el gesto de deslizar el dedo. Ten en cuenta que el SwipeControl contiene una referencia al SwipeItems como su RightItems. El elemento Favorito se mostrará cuando el usuario desliza el dedo de derecha a izquierda.

```xaml
<SwipeControl x:Name="swipeContainer" RightItems="{StaticResource RevealOtherCommands}">
   <!-- The visual state groups moved from the Grid to the SwipeControl, since the SwipeControl wraps the Grid. -->
   <VisualStateManager.VisualStateGroups>
       <VisualStateGroup x:Name="HoveringStates">
           <VisualState x:Name="HoverButtonsShown">
               <VisualState.Setters>
                   <Setter Target="hoverArea.Visibility" Value="Visible" />
               </VisualState.Setters>
           </VisualState>
           <VisualState x:Name="HoverButtonsHidden" />
       </VisualStateGroup>
   </VisualStateManager.VisualStateGroups>
   <Grid Margin="12,0,12,0">
       <Grid.ColumnDefinitions>
           <ColumnDefinition Width="*" />
           <ColumnDefinition Width="Auto" />
       </Grid.ColumnDefinitions>
       <StackPanel>
           <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
       </StackPanel>
       <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
           <AppBarButton Icon="OutlineStar" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" LabelPosition="Collapsed" VerticalAlignment="Stretch"  />
       </Grid>
   </Grid>
</SwipeControl>
```

Cuando el usuario deslice el dedo para invocar el comando Favorito, se llamará al método invocado.

```csharp
private void SwipeItem_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
{
    // Favorite the item using the defined command
    var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
    favoriteCommand.Execute(PodcastObject);
}
```

#### <a name="pull-to-refresh"></a>Extraer para actualizar

Extraer para actualizar permite al usuario desplegar una colección de datos con la entrada táctil para recuperar más datos. Consulta el artículo [Extraer para actualizar](pull-to-refresh.md) para obtener más información.

### <a name="pen-accelerators"></a>Aceleradores de lápiz

El tipo de entrada de lápiz proporciona la precisión de la entrada de puntero. Los usuarios pueden realizar acciones habituales, como abrir menús contextuales, con aceleradores basados en lápiz. Para abrir un menú contextual, los usuarios pueden pulsar en la pantalla con el botón de menú contextual presionado o presionar de manera prolongada en el contenido. Los usuarios pueden usar también el lápiz para mantener el mouse sobre el contenido con el fin de obtener un conocimiento más profundo de la interfaz de usuario, como para mostrar información sobre herramientas o las acciones de mantener el mouse secundarias, de manera similar al mouse.

Para optimizar la aplicación para la entrada de lápiz, consulta el artículo [Interacción de pluma y lápiz](../input/pen-and-stylus-interactions.md).


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

* Asegúrate de que los usuarios pueden acceder a todos los comandos de todos los tipos de dispositivos Windows.
* Incluye un menú contextual que proporcione acceso a todos los comandos disponibles para un elemento de la colección. 
* Proporciona aceleradores de entrada para los comandos usados con frecuencia. 
* Usa la [interfaz ICommand](/uwp/api/Windows.UI.Xaml.Input.ICommand) para implementar comandos. 

## <a name="related-topics"></a>Temas relacionados
* [Interfaz ICommand](/uwp/api/Windows.UI.Xaml.Input.ICommand)
* [Menús y menús contextuales](menus.md)
* [Deslizar rápidamente](swipe.md)
* [Extraer para actualizar](pull-to-refresh.md)
* [Interacción de pluma y lápiz](../input/pen-and-stylus-interactions.md)
* [Adaptar tu aplicación para el controlador para juegos y Xbox](../devices/designing-for-tv.md)
