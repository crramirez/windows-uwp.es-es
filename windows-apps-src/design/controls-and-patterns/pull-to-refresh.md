---
Description: Use the pull-to-refresh control to get new content into a list.
title: Extraer para actualizar
label: Pull-to-refresh
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: aaeb1e74-b795-4015-bf41-02cb1d6f467e
pm-contact: predavid
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 2efd091d90a856e45d76c0b1357f30417812160a
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "8351622"
---
# <a name="pull-to-refresh"></a>Extraer para actualizar

Extraer para actualizar permite al usuario desplegar una lista de datos con la entrada táctil para recuperar más datos. Se usa ampliamente Extraer para actualizar en dispositivos con pantalla táctil. Puedes controlar los API que se muestran aquí para implementar Extraer para actualizar en tu aplicación.

> **API importantes**: [RefreshContainer](/uwp/api/windows.ui.xaml.controls.refreshcontainer), [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer)

![gif de Extraer para actualizar](images/Pull-To-Refresh.gif)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa Extraer para actualizar cuando tengas una lista o cuadrícula de datos que el usuario quizá quiera actualizar con regularidad y es posible que tu aplicación se ejecute en dispositivos móviles y táctiles.

También puedes usar la [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer) para crear una experiencia coherente de actualización que invoca de otras maneras, como un botón de actualización.

## <a name="refresh-controls"></a>Controles de actualización

Dos controles habilitan la función Extraer para actualizar.

- **RefreshContainer**: un ContentControl que proporciona un contenedor para la experiencia de extraer para actualizar. Controla las interacciones táctiles y administra el estado de su visualizador de actualización interna.
- **RefreshVisualizer**: encapsula la visualización de actualización que se explica en la siguiente sección.

El control principal es **RefreshContainer**, que se coloca como un contenedor alrededor del contenido que el usuario agrupa para activar una actualización. RefreshContainer solo funciona con entrada táctil, por lo que recomendamos que también tengas disponible un botón de actualización para los usuarios que no dispongan de interfaz táctil. Puedes colocar el botón Actualizar en una ubicación adecuada en la aplicación, en una barra de comandos o cerca de la superficie que se está actualizando.

## <a name="refresh-visualization"></a>Visualización de actualización

La visualización de actualización predeterminada es un control de giro de progreso circular que se usa para comunicarse cuando se va a producir una actualización y durante el progreso de esta una vez iniciada. El visualizador de actualización tiene 5 estados.

 La distancia que el usuario necesita para desplegar una lista e iniciar así una actualización se denomina _umbral_. El visualizador [State](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.State) viene determinado por el estado de extracción correspondiente a este umbral. Los valores posibles se encuentran en la enumeración [RefreshVisualizerState](/uwp/api/windows.ui.xaml.controls.refreshvisualizerstate).

### <a name="idle"></a>Inactivo

El estado predeterminado del visualizador es **Idle**. El usuario no está interactuando con RefreshContainer a través de la entrada táctil, y no hay una actualización en curso.

Visualmente, no hay ninguna prueba del visualizador de actualización.

### <a name="interacting"></a>Interactuar

Cuando el usuario extrae la lista en la dirección especificada por la propiedad PullDirection y antes de que se alcance el umbral, el visualizador está en el estado **Interacting**.

- Si el usuario libera el control en este estado, el control vuelve a **Idle**.

    ![extraer para actualizar antes del umbral](images/ptr-prethreshold.png)

    Visualmente, el icono aparece como deshabilitado (60% de opacidad). Además, el icono hace una rotación completa con la acción de desplazamiento.

- Si el usuario extrae la lista más allá del umbral, el visualizador pasa de **Interacting** a **Pending**.

    ![extraer para actualizar en el umbral](images/ptr-atthreshold.png)

    Visualmente, el icono cambia a un 100% de opacidad y su tamaño se amplía hasta un 150% para volver al 100% durante la transición.

### <a name="pending"></a>Pending

Cuando el usuario ha extraído la lista más allá del umbral, el visualizador se encuentra en el estado **Pending**.

- Si el usuario vuelve la lista atrás por encima del umbral sin liberarla, vuelve al estado **Interacting**.
- Si el usuario suelta la lista, se inicia una solicitud de actualización y pasa al estado **Refreshing**.

![extraer para actualizar después del umbral](images/ptr-postthreshold.png)

Visualmente, el icono tiene un 100% de tamaño y opacidad. En este estado, el icono sigue hacia abajo con la acción de desplazamiento, pero ya no gira.

### <a name="refreshing"></a>Actualizando

Cuando el usuario suelta el visualizador más allá del umbral, está en el estado **Refreshing**.

Cuando se especifica este estado, se genera el evento **RefreshRequested**. Se trata de la señal para iniciar la actualización del contenido de la aplicación. Los argumentos del evento ([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs)) contienen un objeto [Deferral](/uwp/api/windows.foundation.deferral), que debe tener un manipulador del controlador de eventos. A continuación, debes marcar el aplazamiento como completado cuando se haya completado tu código para realizar la actualización.

Una vez completada la actualización, el visualizador vuelve al estado **Idle**.

Visualmente, el icono vuelve a la ubicación de umbral y gira durante la actualización. Este giro se usa para mostrar el progreso de la actualización y se reemplaza por la animación del contenido entrante.

### <a name="peeking"></a>Inspección

Cuando el usuario extrae la dirección de la actualización desde una posición de inicio, donde no se permite una actualización, el visualizador entra en el estado **Peeking**. Esto suele ocurrir cuando ScrollViewer no está en la posición 0 cuando el usuario inicia la extracción.

- Si el usuario libera el control en este estado, el control vuelve a **Idle**.

## <a name="pull-direction"></a>Dirección de extracción

De manera predeterminada, el usuario extrae una lista de arriba hacia abajo para iniciar una actualización. Si tienes una lista o cuadrícula con una orientación diferente, debes cambiar la dirección de extracción del contenedor de actualización para que coincida.

La propiedad [PullDirection](/uwp/api/windows.ui.xaml.controls.refreshcontainer.PullDirection) tiene uno de estos valores [RefreshPullDirection](/uwp/api/windows.ui.xaml.controls.refreshpulldirection): **BottomToTop**, **TopToBottom**, **RightToLeft** o **LeftToRight**.

Cuando se cambia la dirección de extracción, la posición inicial de giro de progreso del visualizador rota de manera automática, para que la flecha empiece en la posición adecuada para la dirección de la extracción. Si es necesario, puedes cambiar la propiedad [RefreshVisualizer.Orientation](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.Orientation) para invalidar el comportamiento automático. En la mayoría de los casos, se recomienda dejar el valor predeterminado **Auto**.

## <a name="implement-pull-to-refresh"></a>Implementar extraer para actualizar

Para agregar a una lista la función de extraer para actualizar, se necesitan solo unos pocos pasos.

1. Ajusta la lista en un control **RefreshContainer**.
1. Manipula el evento **RefreshRequested** para actualizar el contenido.
1. Otra opción es iniciar una actualización llamando a **RequestRefresh** (por ejemplo, haciendo clic en un botón).

> [!NOTE]
> Puedes crear instancias de un RefreshVisualizer por sí solo. Sin embargo, te recomendamos que ajustes el contenido en un RefreshContainer y uses el RefreshVisualizer que proporciona la propiedad RefreshContainer.Visualizer, incluso en escenarios no táctiles. En este artículo, se supone que el visualizador siempre se obtiene desde el contenedor de la actualización.

> Además, se puede usar a miembros de RequestRefresh y RefreshRequested del contenedor de la actualización para mayor comodidad. `refreshContainer.RequestRefresh()` es equivalente a `refreshContainer.Visualizer.RequestRefresh()` y generará los eventos RefreshContainer.RefreshRequested y RefreshVisualizer.RefreshRequested.

### <a name="request-a-refresh"></a>Solicitar una actualización

El contenedor de actualización controla las interacciones táctiles para permitir al usuario actualizar el contenido a través de la entrada táctil. Te recomendamos que proporciones otras prestaciones para las interfaces no táctiles, como un botón Actualizar o un control de voz.

Para iniciar una actualización, llama al método [RequestRefresh](/uwp/api/windows.ui.xaml.controls.refreshcontainer.RequestRefresh).

```csharp
// See the Examples section for the full code.
private void RefreshButtonClick(object sender, RoutedEventArgs e)
{
    RefreshContainer.RequestRefresh();
}
```

Cuando se llama a RequestRefresh, el estado del visualizador pasa directamente de **Idle** a **Refreshing**.

### <a name="handle-a-refresh-request"></a>Manipula una solicitud de actualización

Para obtener contenido actualizado cuando sea necesario, manipula el evento RefreshRequested. En el controlador de eventos, necesitarás un código específico de tu aplicación para obtener el contenido actualizado.

Los argumentos del evento ([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs)) contienen un objeto [Deferral](/uwp/api/windows.foundation.deferral). Familiarízate con el aplazamiento en el controlador de eventos. Luego marca el aplazamiento como completado cuando se haya completado tu código para realizar la actualización.

```csharp
// See the Examples section for the full code.
private async void RefreshContainer_RefreshRequested(RefreshContainer sender, RefreshRequestedEventArgs args)
{
    // Respond to a request by performing a refresh and using the deferral object.
    using (var RefreshCompletionDeferral = args.GetDeferral())
    {
        // Do some async operation to refresh the content

         await FetchAndInsertItemsAsync(3);

        // The 'using' statement ensures the deferral is marked as complete.
        // Otherwise, you'd call
        // RefreshCompletionDeferral.Complete();
        // RefreshCompletionDeferral.Dispose();
    }
}
```

### <a name="respond-to-state-changes"></a>Responde a los cambios de estado

Puedes responder a los cambios de estado del visualizador, si es necesario. Por ejemplo, para evitar varias de las solicitudes de actualización, puedes deshabilitar un botón Actualizar mientras se está actualizando el visualizador.

```csharp
// See the Examples section for the full code.
private void Visualizer_RefreshStateChanged(RefreshVisualizer sender, RefreshStateChangedEventArgs args)
{
    // Respond to visualizer state changes.
    // Disable the refresh button if the visualizer is refreshing.
    if (args.NewState == RefreshVisualizerState.Refreshing)
    {
        RefreshButton.IsEnabled = false;
    }
    else
    {
        RefreshButton.IsEnabled = true;
    }
}
```

## <a name="examples"></a>Ejemplos

### <a name="using-a-scrollviewer-in-a-refreshcontainer"></a>Usar un ScrollViewer en un RefreshContainer

Este ejemplo muestra cómo usar la función Extraer para actualizar con un visor de desplazamiento.

```xaml
<RefreshContainer>
    <ScrollViewer VerticalScrollMode="Enabled"
                  VerticalScrollBarVisibility="Auto"
                  HorizontalScrollBarVisibility="Auto">
 
        <!-- Scrollviewer content -->

    </ScrollViewer>
</RefreshContainer>
```

### <a name="adding-pull-to-refresh-to-a-listview"></a>Añadir Extraer para actualizar a ListView

Este ejemplo muestra cómo usar la función Extraer para actualizar con una vista de lista.

```xaml
<StackPanel Margin="0,40" Width="280">
    <CommandBar OverflowButtonVisibility="Collapsed">
        <AppBarButton x:Name="RefreshButton" Click="RefreshButtonClick"
                      Icon="Refresh" Label="Refresh"/>
        <CommandBar.Content>
            <TextBlock Text="List of items" 
                       Style="{StaticResource TitleTextBlockStyle}"
                       Margin="12,8"/>
        </CommandBar.Content>
    </CommandBar>

    <RefreshContainer x:Name="RefreshContainer">
        <ListView x:Name="ListView1" Height="400">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <Grid Height="80">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <TextBlock Text="{x:Bind Path=Header}"
                                   Style="{StaticResource SubtitleTextBlockStyle}"
                                   Grid.Row="0"/>
                        <TextBlock Text="{x:Bind Path=Date}"
                                   Style="{StaticResource CaptionTextBlockStyle}"
                                   Grid.Row="1"/>
                        <TextBlock Text="{x:Bind Path=Body}"
                                   Style="{StaticResource BodyTextBlockStyle}"
                                   Grid.Row="2"
                                   Margin="0,4,0,0" />
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </RefreshContainer>
</StackPanel>
```

```csharp
public sealed partial class MainPage : Page
{
    public ObservableCollection<ListItemData> Items { get; set; } 
        = new ObservableCollection<ListItemData>();

    public MainPage()
    {
        this.InitializeComponent();

        Loaded += MainPage_Loaded;
        ListView1.ItemsSource = Items;
    }

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        Loaded -= MainPage_Loaded;
        RefreshContainer.RefreshRequested += RefreshContainer_RefreshRequested;
        RefreshContainer.Visualizer.RefreshStateChanged += Visualizer_RefreshStateChanged;

        // Add some initial content to the list.
        await FetchAndInsertItemsAsync(2);
    }

    private void RefreshButtonClick(object sender, RoutedEventArgs e)
    {
        RefreshContainer.RequestRefresh();
    }

    private async void RefreshContainer_RefreshRequested(RefreshContainer sender, RefreshRequestedEventArgs args)
    {
        // Respond to a request by performing a refresh and using the deferral object.
        using (var RefreshCompletionDeferral = args.GetDeferral())
        {
            // Do some async operation to refresh the content

            await FetchAndInsertItemsAsync(3);

            // The 'using' statement ensures the deferral is marked as complete.
            // Otherwise, you'd call
            // RefreshCompletionDeferral.Complete();
            // RefreshCompletionDeferral.Dispose();
        }
    }

    private void Visualizer_RefreshStateChanged(RefreshVisualizer sender, RefreshStateChangedEventArgs args)
    {
        // Respond to visualizer state changes.
        // Disable the refresh button if the visualizer is refreshing.
        if (args.NewState == RefreshVisualizerState.Refreshing)
        {
            RefreshButton.IsEnabled = false;
        }
        else
        {
            RefreshButton.IsEnabled = true;
        }
    }

    // App specific code to get fresh data.
    private async Task FetchAndInsertItemsAsync(int updateCount)
    {
        for (int i = 0; i < updateCount; ++i)
        {
            // Simulate delay while we go fetch new items.
            await Task.Delay(1000);
            Items.Insert(0, GetNextItem());
        }
    }

    private ListItemData GetNextItem()
    {
        return new ListItemData()
        {
            Header = "Header " + DateTime.Now.Second.ToString(),
            Date = DateTime.Now.ToLongDateString(),
            Body = DateTime.Now.ToLongTimeString()
        };
    }
}

public class ListItemData
{
    public string Header { get; set; }
    public string Date { get; set; }
    public string Body { get; set; }
}
```

## <a name="related-articles"></a>Artículos relacionados

- [Interacciones táctiles](../input/touch-interactions.md)
- [Vista de lista y vista de cuadrícula](listview-and-gridview.md)
- [Plantillas y contenedores de elementos](item-containers-templates.md)
- [Animaciones de expresión](../../composition/composition-animation.md)
