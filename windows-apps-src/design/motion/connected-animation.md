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
ms.openlocfilehash: f448f481b2b55a42cbaa158cc4b07261e2d7717b
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317133"
---
# <a name="connected-animation-for-uwp-apps"></a>Animación conectada para aplicaciones para UWP

Las animaciones conectadas te permiten crear una experiencia de navegación dinámica y atractiva al animar la transición de un elemento entre dos vistas distintas. Esto ayuda al usuario a mantener el contexto y ofrece continuidad entre las vistas.

En una animación conectada, aparece un elemento en "Continuar" entre las dos vistas durante un cambio en el contenido de la interfaz de usuario, volando en la pantalla de su ubicación en la vista del origen a su destino en la nueva vista. Esto enfatiza el contenido comunes entre las vistas y crea un efecto atractivo y dinámico como parte de una transición.

> **API importantes**:  [Clase ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation), [ConnectedAnimationService clase](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)


## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tiene la <strong style="font-weight: semi-bold">Galería de controles XAML</strong> aplicación instalada, haga clic aquí para <a href="xamlcontrolsgallery:/item/ConnectedAnimation">abra la aplicación y ver la animación conectada en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

En este breve vídeo, una aplicación usa una animación conectada para animar una imagen de elemento como "sigue" pase a formar parte del encabezado de la página siguiente. El efecto te ayuda a mantener al usuario en contexto a través de la transición.

![Animación conectada](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>Resumen en vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>Animación conectada y Fluent Design System

 Fluent Design System te ayuda a crear interfaces de usuario modernas y claras que incorporan luz, profundidad, movimiento, materiales y escala. La animación conectada es un componente de Fluent Design System que agrega movimiento a tu aplicación. Para obtener más información, consulta la [Introducción a Fluent Design para UWP](/windows/apps/fluent-design-system).

## <a name="why-connected-animation"></a>¿Por qué usar una animación conectada?

Al navegar entre páginas, es importante que el usuario pueda comprender el nuevo contenido que se le presenta después del desplazamiento y cómo se relaciona con su intención al navegar. Las animaciones conectadas proporcionan una metáfora visual eficaz que destaca la relación entre dos vistas al llevar el foco del usuario al contenido compartido por ellas. Además, las animaciones conectadas agregan un interés y un orden visuales a la navegación por la página, lo que puede ayudar a diferenciar el diseño de movimiento de la aplicación.

## <a name="when-to-use-connected-animation"></a>Cuándo usar animaciones conectadas

En general, las animaciones conectadas se usan al cambiar de página, aunque pueden aplicarse a cualquier experiencia donde cambie el contenido de una interfaz de usuario y quieras que el usuario se mantenga en contexto. Debes pensar en usar una animación conectada en lugar de una [transición de navegación de profundización](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) siempre que haya una imagen u otro elemento de la interfaz de usuario que se comparta entre las vistas de origen y de destino.

## <a name="configure-connected-animation"></a>Configurar la animación conectada

> [!IMPORTANT]
> Esta característica requiere que la versión de destino de la aplicación ser Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o una versión posterior. La propiedad de configuración no está disponible en SDK anteriores. Puede tener como destino una versión mínima inferior a SDK 17763 mediante código adaptable o XAML condicional. Para obtener más información, consulte [aplicaciones adaptables versión](/windows/uwp/debug-test-perf/version-adaptive-apps).

A partir de Windows 10, versión 1809, aún más las animaciones conectadas representan diseño Fluent proporcionando animación configuraciones adaptadas específicamente para hacia delante y hacia atrás navegación de páginas.

Especifique una configuración de la animación estableciendo la propiedad de configuración en el elemento ConnectedAnimation. (Mostraremos algunos ejemplos de esto en la sección siguiente).

Esta tabla describen las configuraciones disponibles. Para obtener más información sobre los principios de movimiento que se aplican en estas animaciones, consulte [direccionalidad y gravedad](index.md).

| [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) |
| - |
| Esta es la configuración predeterminada y se recomienda para la navegación hacia delante. |
Cuando el usuario navega hacia delante en la aplicación (A B), aparece el elemento conectado físicamente "extraerlos fuera de la página". De esta manera, el elemento aparece para desplazarse hacia delante en el espacio de la z y quita un poco como un efecto de gravedad teniendo la suspensión. Para superar los efectos de gravedad, el elemento positivas velocidad y acelera en su posición final. El resultado es una animación de "escalado y dip". |

| [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) |
| - |
| Cuando el usuario navega hacia atrás en la aplicación (B a), la animación es más directa. El elemento conectado linealmente traduce del B al uso de una función de aceleración decelerate cúbica Bézier. La prestación visual hacia atrás devuelve al usuario a su estado anterior tan rápido como sea posible sin perder el contexto del flujo de navegación. |

| [BasicConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration) |
| - |
| Este es el valor predeterminado (y único) animación usada en las versiones anteriores de Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)). |

### <a name="connectedanimationservice-configuration"></a>Configuración de ConnectedAnimationService

El [ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) clase tiene dos propiedades que se aplican a las animaciones individuales en lugar de con el servicio global.

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

Para lograr los distintos efectos, algunas configuraciones omitir estas propiedades en ConnectedAnimationService y utilizan sus propios valores en su lugar, como se describe en esta tabla.

| Configuración | ¿Aspectos DefaultDuration? | ¿Aspectos DefaultEasingFunction? |
| - | - | - |
| Gravedad | Sí | Sí* <br/> **La traducción básica de la A B utiliza esta función de aceleración, pero la dip"gravedad" tiene su propia función de aceleración.*  |
| Directo | No <br/> *Más de 150 MS se anima.*| No <br/> *Usa la función de aceleración de Decelerate.* |
| Básico | Sí | Sí |

## <a name="how-to-implement-connected-animation"></a>Cómo implementar la animación conectada

La configuración de una animación conectada consta de dos pasos:

1. *Preparar* un objeto de animación en la página de origen, que indica al sistema que participará el elemento de origen en la animación conectada.
1. *Iniciar* la animación en la página de destino, pasando una referencia al elemento de destino.

Al navegar desde la página de origen, llame a [ConnectedAnimationService.GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) para obtener una instancia de ConnectedAnimationService. Para preparar una animación, llame a [PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) en esta instancia y pasar una clave única y el elemento de interfaz de usuario que desea utilizar en la transición. La clave única permite recuperar la animación posteriormente en la página de destino.

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

Cuando se produce la navegación, iniciar la animación en la página de destino. Para iniciar la animación, llama a [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart). Puedes recuperar la instancia de animación correcta llamando a [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation) con la clave única que proporcionaste cuando creaste la animación.

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>Navegación hacia delante

Este ejemplo muestra cómo usar ConnectedAnimationService para crear una transición para la navegación hacia delante entre las dos páginas (Page_A a Page_B).

La configuración de animación recomendados para la navegación hacia delante es [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration). Esta es la predeterminada, por lo que no es necesario establecer la [configuración](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) propiedad a menos que desee especificar una configuración diferente.

Configure la animación en la página de origen.

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

Iniciar la animación en la página de destino.

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

Para la navegación hacia atrás (Page_B a Page_A), siga los mismos pasos, pero se invierten las páginas de origen y destino.

Cuando el usuario navega de vuelta, esperan que la aplicación va a devolver al estado anterior tan pronto como sea posible. Por lo tanto, la configuración recomendada es [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration). Esta animación es más rápido y más directa y usa la aceleración decelerate.

Configure la animación en la página de origen.

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimationService.GetForCurrentView()
            .PrepareToAnimate("backAnimation", DestinationImage);

        // Use the recommended configuration for back animation.
        animation.Configuration = new DirectConnectedAnimationConfiguration();
    }
}
```

Iniciar la animación en la página de destino.

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

Entre el momento en que se configura la animación y cuando se inicia, el elemento de origen parece estar inmovilizado por encima de otras interfaces de usuario en la aplicación. Esto le permite realizar cualquier otra animación de transición al mismo tiempo. Por este motivo, no debe esperar más de 250 milisegundos entre los dos pasos porque la presencia del elemento de origen puede volverse confuso. Si preparas una animación y no se inicia en el plazo de tres segundos, el sistema desechará la animación y todas las subsiguientes llamadas a [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) generarán un error.

## <a name="connected-animation-in-list-and-grid-experiences"></a>Animación conectado en experiencias de lista y cuadrícula

A menudo, querrás crear una animación conectada desde un control de lista o cuadrícula o hacia él. Puede usar los dos métodos en [ListView](/uwp/api/windows.ui.xaml.controls.listview) y [GridView](/uwp/api/windows.ui.xaml.controls.gridview), [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) y [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync), Para simplificar este proceso.

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

Para preparar una animación conectada con la elipse correspondiente a un elemento de lista determinado, llame a la [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) método con una clave única, el elemento y el nombre "PortraitEllipse".

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

Para iniciar una animación con este elemento como el destino, por ejemplo, al volver de una vista detallada, use [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync). Si acaba de cargar el origen de datos para el ListView, TryStartConnectedAnimationAsync esperará a que la animación se inicia hasta que se ha creado el contenedor del elemento correspondiente.

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

Un *coordinada animación* es un tipo especial de animación de entrada donde aparece un elemento junto con el destino de animación conectada, animar conjuntamente con el elemento de animación conectada mientras se desplaza por la pantalla. Las animaciones coordinadas pueden sumar interés visual a una transición y llamar aún más la atención del usuario al contexto que comparten las vistas de origen y de destino. En estas imágenes, la interfaz de usuario del título para el elemento ser anima con una animación coordinada.

Cuando una animación coordinada usa la configuración de gravedad, gravedad se aplica a los dos elementos coordinados y el elemento de animación conectada. Los elementos coordinados se "swoop" junto con el elemento conectado por lo que los elementos permanecen realmente coordinados.

Usa la sobrecarga de dos parámetros de **TryStart** para agregar elementos coordinados a una animación conectada. En este ejemplo se muestra una animación de coordinadas de un diseño de cuadrícula denominado "DescriptionRoot" que entra en tándem con un elemento de animación conectada denominado "CoverImage".

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
- Use [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) para realizar una exploración.
- No esperar a las solicitudes de red u otras operaciones asincrónicas de larga ejecución entre la preparación y el inicio de una animación conectada. Es posible que tengas que cargar previamente la información necesaria para ejecutar la transición con antelación o usar una imagen de marcador de posición de baja resolución mientras se carga la imagen de alta resolución en la vista de destino.
- Use [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) para evitar que una animación de transición en un **marco** si usas **ConnectedAnimationService**, desde las animaciones conectadas no están diseñados para usarse simultáneamente con las transiciones de exploración predeterminada. Para obtener más información sobre cómo usar las transiciones de navegación, consulta [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition).

## <a name="related-articles"></a>Artículos relacionados

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
