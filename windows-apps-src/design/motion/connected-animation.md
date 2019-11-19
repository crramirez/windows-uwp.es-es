---
description: Las animaciones conectadas te permiten crear una experiencia de navegación dinámica y atractiva al animar la transición de un elemento entre dos vistas distintas.
title: Animación conectada
template: detail.hbs
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6e17b1c18fc8e643ac788e5e13ac78cae49a35ef
ms.sourcegitcommit: 6d743cf9c3e09f87ea2879b8e1f2dc4a1b1a16fe
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2019
ms.locfileid: "74166081"
---
# <a name="connected-animation-for-uwp-apps"></a>Animación conectada para aplicaciones para UWP

Las animaciones conectadas te permiten crear una experiencia de navegación dinámica y atractiva al animar la transición de un elemento entre dos vistas distintas. Esto ayuda al usuario a mantener el contexto y ofrece continuidad entre las vistas.

En una animación conectada, un elemento aparece en "continuar" entre dos vistas durante un cambio en el contenido de la interfaz de usuario, volando por la pantalla desde su ubicación en la vista de origen hasta su destino en la nueva vista. Esto hace hincapié en el contenido común entre las vistas y crea un efecto atractivo y dinámico como parte de una transición.

> **API importantes**: [clase ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation), [clase ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)


## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tiene instalada la aplicación de <strong style="font-weight: semi-bold">Galería de controles de XAML</strong> , haga clic aquí para <a href="xamlcontrolsgallery:/item/ConnectedAnimation">abrir la aplicación y ver la animación conectada en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

En este vídeo corto, una aplicación usa una animación conectada para animar una imagen de elemento a medida que "continúa" para formar parte del encabezado de la página siguiente. El efecto te ayuda a mantener al usuario en contexto a través de la transición.

![Animación conectada](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>Resumen en vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>Animación conectada y Fluent Design System

 Fluent Design System te ayuda a crear interfaces de usuario modernas y claras que incorporan luz, profundidad, movimiento, materiales y escala. La animación conectada es un componente de Fluent Design System que agrega movimiento a tu aplicación. Para más información, consulta la [Introducción a Fluent Design para UWP](/windows/apps/fluent-design-system).

## <a name="why-connected-animation"></a>¿Por qué usar una animación conectada?

Al navegar entre páginas, es importante que el usuario pueda comprender el nuevo contenido que se le presenta después del desplazamiento y cómo se relaciona con su intención al navegar. Las animaciones conectadas proporcionan una metáfora visual eficaz que destaca la relación entre dos vistas al llevar el foco del usuario al contenido compartido por ellas. Además, las animaciones conectadas agregan un interés y un orden visuales a la navegación por la página, lo que puede ayudar a diferenciar el diseño de movimiento de la aplicación.

## <a name="when-to-use-connected-animation"></a>Cuándo usar animaciones conectadas

En general, las animaciones conectadas se usan al cambiar de página, aunque pueden aplicarse a cualquier experiencia donde cambie el contenido de una interfaz de usuario y quieras que el usuario se mantenga en contexto. Debes pensar en usar una animación conectada en lugar de una [transición de navegación de profundización](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) siempre que haya una imagen u otro elemento de la interfaz de usuario que se comparta entre las vistas de origen y de destino.

## <a name="configure-connected-animation"></a>Configurar animación conectada

> [!IMPORTANT]
> Esta característica requiere que la versión de destino de la aplicación sea Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior. La propiedad de configuración no está disponible en los SDK anteriores. Puede tener como destino una versión mínima inferior al SDK 17763 con código adaptativo o XAML condicional. Para obtener más información, consulte [versiones adaptables](/windows/uwp/debug-test-perf/version-adaptive-apps)de las aplicaciones.

A partir de la versión 1809 de Windows 10, las animaciones conectadas incluyen un diseño fluido al proporcionar configuraciones de animación adaptadas específicamente a la navegación de páginas hacia delante y hacia atrás.

Especifique una configuración de animación mediante el establecimiento de la propiedad de configuración en ConnectedAnimation. (Se mostrarán ejemplos de esto en la sección siguiente).

En esta tabla se describen las configuraciones disponibles. Para obtener más información sobre los principios de movimiento aplicados en estas animaciones, vea [direccionalidad y gravedad](index.md).

| [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) |
| - |
| Esta es la configuración predeterminada y se recomienda para la navegación hacia delante. |
A medida que el usuario navega hacia delante en la aplicación (a a B), aparece el elemento conectado físicamente "extraer de la página". Al hacerlo, el elemento aparece para avanzar en el espacio z y se coloca un bit como efecto de la gravedad de la suspensión. Para superar los efectos de la gravedad, el elemento obtiene la velocidad y se acelera en su posición final. El resultado es una animación "Scale and DIP". |

| [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) |
| - |
| A medida que el usuario navega hacia atrás en la aplicación (de B a A), la animación es más directa. El elemento conectado se traduce linealmente de B a mediante una función de aceleración de Bézier cúbica decelerada. La rentabilidad visual hacia atrás devuelve el usuario a su estado anterior lo más rápido posible mientras sigue manteniendo el contexto del flujo de navegación. |

| [BasicConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration) |
| - |
| Esta es la animación predeterminada (y solo) utilizada en versiones anteriores a Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)). |

### <a name="connectedanimationservice-configuration"></a>Configuración de ConnectedAnimationService

La clase [ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) tiene dos propiedades que se aplican a las animaciones individuales en lugar del servicio general.

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

Para lograr los distintos efectos, algunas configuraciones omiten estas propiedades en ConnectedAnimationService y usan sus propios valores, como se describe en esta tabla.

| Configuración | ¿Respeta DefaultDuration? | ¿Respeta DefaultEasingFunction? |
| - | - | - |
| Gravedad | Sí | Sí* <br/> **la traducción básica de a a B usa esta función de aceleración, pero la "DIP" tiene su propia función de aceleración.*  |
| Directo | No <br/> *Anima por 150MS.*| No <br/> *Usa la función de aceleración decelerada.* |
| Básico | Sí | Sí |

## <a name="how-to-implement-connected-animation"></a>Cómo implementar la animación conectada

La configuración de una animación conectada consta de dos pasos:

1. *Preparar* un objeto de animación en la página de origen, que indica al sistema que el elemento de origen participará en la animación conectada.
1. *Inicie* la animación en la página de destino, pasando una referencia al elemento de destino.

Al navegar desde la página de origen, llame a [ConnectedAnimationService. GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) para obtener una instancia de ConnectedAnimationService. Para preparar una animación, llame a [PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) en esta instancia y pase una clave única y el elemento de la interfaz de usuario que desea usar en la transición. La clave única permite recuperar la animación más adelante en la página de destino.

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

Cuando se produzca la navegación, inicie la animación en la página de destino. Para iniciar la animación, llama a [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart). Puedes recuperar la instancia de animación correcta llamando a [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation) con la clave única que proporcionaste cuando creaste la animación.

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>Navegación hacia delante

En este ejemplo se muestra cómo usar ConnectedAnimationService para crear una transición para la navegación hacia delante entre dos páginas (Page_A a Page_B).

La configuración de animación recomendada para la navegación hacia delante es [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration). Este es el valor predeterminado, por lo que no es necesario establecer la propiedad de [configuración](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) a menos que desee especificar una configuración diferente.

Configure la animación en la página origen.

```xaml
<!-- Page_A.xaml -->

<Image x:Name="SourceImage"
       HorizontalAlignment="Left" VerticalAlignment="Top"
       Width="200" Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png"
       PointerPressed="SourceImage_PointerPressed"/>
```

```csharp
// Page_A.xaml.cs

private void SourceImage_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Navigate to detail page.
    // Suppress the default animation to avoid conflict with the connected animation.
    Frame.Navigate(typeof(Page_B), null, new SuppressNavigationTransitionInfo());
}

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    ConnectedAnimationService.GetForCurrentView()
        .PrepareToAnimate("forwardAnimation", SourceImage);
    // You don't need to explicitly set the Configuration property because
    // the recommended Gravity configuration is default.
    // For custom animation, use:
    // animation.Configuration = new BasicConnectedAnimationConfiguration();
}
```

Inicie la animación en la página de destino.

```xaml
<!-- Page_B.xaml -->

<Image x:Name="DestinationImage"
       Width="400" Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

```csharp
// Page_B.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
    if (animation != null)
    {
        animation.TryStart(DestinationImage);
    }
}
```

### <a name="back-navigation"></a>Navegación hacia atrás

Para la navegación hacia atrás (Page_B a Page_A), sigue los mismos pasos, pero las páginas de origen y de destino se invierten.

Cuando el usuario se desplaza hacia atrás, espera que la aplicación se devuelva al estado anterior lo antes posible. Por lo tanto, la configuración recomendada es [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration). Esta animación es más rápida, más directa y usa la aceleración de deceleración.

Configure la animación en la página origen.

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimation animation = 
            ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("backAnimation", DestinationImage);

        // Use the recommended configuration for back animation.
        animation.Configuration = new DirectConnectedAnimationConfiguration();
    }
}
```

Inicie la animación en la página de destino.

```csharp
// Page_A.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("backAnimation");
    if (animation != null)
    {
        animation.TryStart(SourceImage);
    }
}
```

Entre el momento en que se configura la animación y el momento en que se inicia, el elemento de origen aparece inmovilizado por encima de otra interfaz de usuario de la aplicación. Esto le permite realizar cualquier otra animación de transición simultáneamente. Por esta razón, no debe esperar más de ~ 250 milisegundos entre los dos pasos porque la presencia del elemento de origen puede ser distraer. Si preparas una animación y no se inicia en el plazo de tres segundos, el sistema desechará la animación y todas las subsiguientes llamadas a [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) generarán un error.

## <a name="connected-animation-in-list-and-grid-experiences"></a>Animación conectado en experiencias de lista y cuadrícula

A menudo, querrás crear una animación conectada desde un control de lista o cuadrícula o hacia él. Puede usar los dos métodos en [ListView](/uwp/api/windows.ui.xaml.controls.listview) y [GridView](/uwp/api/windows.ui.xaml.controls.gridview), [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) y [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)para simplificar este proceso.

Por ejemplo, supongamos que tienes una clase **ListView** que contiene un elemento con el nombre "PortraitEllipse" en la plantilla de datos.

```xaml
<ListView x:Name="ContactsListView" Loaded="ContactsListView_Loaded">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="vm:ContactsItem">
            <Grid>
                …
                <Ellipse x:Name="PortraitEllipse" … />
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Para preparar una animación conectada con la elipse correspondiente a un elemento de lista determinado, llame al método [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) con una clave única, el elemento y el nombre "PortraitEllipse".

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

Para iniciar una animación con este elemento como destino, por ejemplo, al navegar de nuevo desde una vista de detalle, use [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync). Si acaba de cargar el origen de datos para ListView, TryStartConnectedAnimationAsync esperará para iniciar la animación hasta que se haya creado el contenedor de elementos correspondiente.

```csharp
private void ContactsListView_Loaded(object sender, RoutedEventArgs e)
{
    ContactsItem item = GetPersistedItem(); // Get persisted item
    if (item != null)
    {
        ContactsListView.ScrollIntoView(item);
        ConnectedAnimation animation =
            ConnectedAnimationService.GetForCurrentView().GetAnimation("portrait");
        if (animation != null)
        {
            await ContactsListView.TryStartConnectedAnimationAsync(
                animation, item, "PortraitEllipse");
        }
    }
}
```

## <a name="coordinated-animation"></a>Animación coordinada

![Animación coordinada](images/connected-animations/coordinated_example.gif)

<!--
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=9066bbbe%2Dcf58%2D4ab4%2Db274%2D595616f5d0a0&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

Una *animación coordinada* es un tipo especial de animación de entrada en el que aparece un elemento junto con el destino de animación conectado, animando en conjunto con el elemento de animación conectado a medida que se mueve por la pantalla. Las animaciones coordinadas pueden sumar interés visual a una transición y llamar aún más la atención del usuario al contexto que comparten las vistas de origen y de destino. En estas imágenes, la interfaz de usuario del título para el elemento ser anima con una animación coordinada.

Cuando una animación coordinada usa la configuración de gravedad, la gravedad se aplica al elemento de animación conectado y a los elementos coordinados. Los elementos coordinados se "swoop" junto con el elemento conectado para que los elementos permanezcan realmente coordinados.

Usa la sobrecarga de dos parámetros de **TryStart** para agregar elementos coordinados a una animación conectada. En este ejemplo se muestra una animación coordinada de un diseño de cuadrícula denominado "DescriptionRoot" que entra junto con un elemento de animación conectado denominado "CoverImage".

```xaml
<!-- DestinationPage.xaml -->
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

```csharp
// DestinationPage.xaml.cs
void OnNavigatedTo(NavigationEventArgs e)
{
    var animationService = ConnectedAnimationService.GetForCurrentView();
    var animation = animationService.GetAnimation("coverImage");

    if (animation != null)
    {
        // Don’t need to capture the return value as we are not scheduling any subsequent
        // animations
        animation.TryStart(CoverImage, new UIElement[] { DescriptionRoot });
     }
}
```

## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer

- Usa una animación conectada en las transiciones de página en las que un elemento se comparta entre las páginas de origen y de destino.
- Use [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) para la navegación hacia delante.
- Use [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) para la navegación hacia atrás.
- No espere en las solicitudes de red u otras operaciones asincrónicas de ejecución prolongada entre la preparación y el inicio de una animación conectada. Es posible que tengas que cargar previamente la información necesaria para ejecutar la transición con antelación o usar una imagen de marcador de posición de baja resolución mientras se carga la imagen de alta resolución en la vista de destino.
- Use [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) para evitar una animación de transición en un **fotograma** si usa **ConnectedAnimationService**, ya que las animaciones conectadas no están diseñadas para usarse simultáneamente con las transiciones de navegación predeterminadas. Para obtener más información sobre cómo usar las transiciones de navegación, consulta [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition).

## <a name="related-articles"></a>Artículos relacionados

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
