---
author: mijacobs
description: Las animaciones conectadas te permiten crear una experiencia de navegación dinámica y atractiva al animar la transición de un elemento entre dos vistas distintas.
title: Animación conectada
template: detail.hbs
ms.author: jimwalk
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 31e940c87626a05ee6911d3ffda36ab8dfd3fad0
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "4679908"
---
# <a name="connected-animation-for-uwp-apps"></a>Animación conectada para aplicaciones para UWP

Las animaciones conectadas te permiten crear una experiencia de navegación dinámica y atractiva al animar la transición de un elemento entre dos vistas distintas. Esto ayuda al usuario a mantener el contexto y ofrece continuidad entre las vistas.

En una animación conectada, un elemento parece "Continuar" entre dos vistas durante un cambio en el contenido de la interfaz de usuario, volando por la pantalla desde su ubicación en la vista de origen hasta su destino en la nueva vista. Esto enfatiza el contenido común entre las vistas y crea un efecto atractivo y dinámico como parte de una transición.

> **API importantes**: [clase ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation), la [clase de ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

## <a name="see-it-in-action"></a>Vela en acción

En este breve vídeo, una aplicación usa una animación conectada para animar la imagen de un elemento medida que "continúa" para formar parte del encabezado de la página siguiente. El efecto te ayuda a mantener al usuario en contexto a través de la transición.

![Animación conectada](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>Resumen en vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>Animación conectada y Fluent Design System

 Fluent Design System te ayuda a crear interfaces de usuario modernas y claras que incorporan luz, profundidad, movimiento, materiales y escala. La animación conectada es un componente de Fluent Design System que agrega movimiento a tu aplicación. Para obtener más información, consulta la [Introducción a Fluent Design para UWP](../fluent-design-system/index.md).

## <a name="why-connected-animation"></a>¿Por qué usar una animación conectada?

Al navegar entre páginas, es importante que el usuario pueda comprender el nuevo contenido que se le presenta después del desplazamiento y cómo se relaciona con su intención al navegar. Las animaciones conectadas proporcionan una metáfora visual eficaz que destaca la relación entre dos vistas al llevar el foco del usuario al contenido compartido por ellas. Además, las animaciones conectadas agregan un interés y un orden visuales a la navegación por la página, lo que puede ayudar a diferenciar el diseño de movimiento de la aplicación.

## <a name="when-to-use-connected-animation"></a>Cuándo usar animaciones conectadas

En general, las animaciones conectadas se usan al cambiar de página, aunque pueden aplicarse a cualquier experiencia donde cambie el contenido de una interfaz de usuario y quieras que el usuario se mantenga en contexto. Debes pensar en usar una animación conectada en lugar de una [transición de navegación de profundización](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx) siempre que haya una imagen u otro elemento de la interfaz de usuario que se comparta entre las vistas de origen y de destino.

## <a name="configure-connected-animation"></a>Configurar la animación conectada

> [!IMPORTANT]
> Esta característica requiere que la versión de destino de la aplicación sea RS5 (versión del SDK de Windows 10.0.NNNNN.0 (Windows 10, versión aamm) o superior. La propiedad de configuración no está disponible en los SDK anteriores. Puedes elegir una versión mínima menor que RS5 (10.0.NNNNN.0 (Windows 10, versión aamm) de versión de SDK de Windows usando código adaptativo para versiones o XAML condicional. Para obtener más información, consulta [aplicaciones adaptables para versiones](/debug-test-perf/version-adaptive-apps).

A partir de RS5, aún más las animaciones conectadas incluyen Fluent design proporcionando animación configuraciones personalizadas específicamente para hacia delante y hacia atrás navegación de páginas.

Se especifica una configuración de animación estableciendo la propiedad de configuración en el ConnectedAnimation. (Te mostraremos ejemplos de esto en la siguiente sección.)

Esta tabla describen las configuraciones disponibles. Para obtener más información acerca de los principios de movimiento que se aplican en estas animaciones, consulta la [direccionalidad y gravedad](index.md).

| [GravityConnectedAnimationConfiguration]() |
| - |
| Esto es la configuración predeterminada y se recomienda para la navegación hacia delante. |
Como el usuario navega hacia adelante en la aplicación (A B), aparece el elemento conectado físicamente "extraer fuera de la página". Al hacerlo, el elemento aparece a avanzar en el espacio z y quita un poco como un efecto de gravedad teniendo en suspensión. Para superar los efectos de gravedad, el elemento obtiene la velocidad y acelera a su posición final. El resultado es una animación de "escala y dip". |

| [DirectConnectedAnimationConfiguration]() |
| - |
| Como el usuario navega hacia atrás en la aplicación (B al A), la animación es más directa. El elemento conectado se traslada linealmente desde B al uso de una función de aceleración decelerate cubic Bezier. La prestación visual hacia atrás devuelve al usuario a su estado anterior lo más rápido posible que mantienen el contexto del flujo de navegación. |

| [BasicConnectedAnimationConfiguration]() |
| - |
| Este es el valor predeterminado (y solo) animación usada en las versiones SDK anteriores RS5 (versión del SDK de Windows 10.0.NNNNN.0 (Windows 10, versión aamm). |

### <a name="connectedanimationservice-configuration"></a>Configuración de ConnectedAnimationService

La clase de [ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) tiene dos propiedades que se aplican a las animaciones individuales en lugar de con el servicio general.

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

Para lograr los distintos efectos, algunas configuraciones pasar por alto estas propiedades en ConnectedAnimationService y usan sus propios valores en su lugar, como se describe en esta tabla.

| Configuración | ¿Aspectos DefaultDuration? | ¿Aspectos DefaultEasingFunction? |
| - | - | - |
| Gravedad | Sí | Sí* <br/> **La traducción básica de A B utiliza esta función de aceleración, pero el "dip de gravedad" tiene su propia función de aceleración.*  |
| Directo | No <br/> *Anima más de 150 milisegundos.*| No <br/> *Usa la función de aceleración de Decelerate.* |
| Básico | Sí | Sí |

## <a name="how-to-implement-connected-animation"></a>Cómo implementar la animación conectada

La configuración de una animación conectada consta de dos pasos:

1. *Preparar* un objeto de animación en la página de origen, lo que indica al sistema que el elemento de origen participará en la animación conectada.
1. *Iniciar* la animación en la página de destino, pasando una referencia al elemento de destino.

Al navegar desde la página de origen, llama a [ConnectedAnimationService.GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) para obtener una instancia de ConnectedAnimationService. Para preparar una animación, llama a [PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) en esta instancia y pasar una clave única y el elemento de interfaz de usuario que quieras usar en la transición. La clave única te permite recuperar la animación más adelante en la página de destino.

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

Cuando se produce la navegación, inicia la animación en la página de destino. Para iniciar la animación, llama a [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart). Puedes recuperar la instancia de animación correcta llamando a [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation) con la clave única que proporcionaste cuando creaste la animación.

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>Navegación hacia adelante

Este ejemplo muestra cómo usar ConnectedAnimationService para crear una transición de avance de navegación entre dos páginas (Page_A a Page_B).

La configuración de animación recomendada para la navegación hacia delante es [GravityConnectedAnimationConfiguration](). Este es el valor predeterminado, por lo que no es necesario establecer la propiedad de [configuración](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) , a menos que quieras especificar una configuración diferente.

Configurar la animación en la página de origen.

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

Inicia la animación en la página de destino.

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

Para la navegación hacia atrás (Page_B a Page_A), sigue los mismos pasos, pero se invierten las páginas de origen y de destino.

Cuando el usuario navega hacia atrás, esperan que la aplicación de ser devuelto al estado anterior lo antes posible. Por lo tanto, la configuración recomendada es [DirectConnectedAnimationConfiguration](). Esta animación es más rápido, más directo y usa deceleración.

Configurar la animación en la página de origen.

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

Inicia la animación en la página de destino.

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

Entre el momento en que se configura la animación y cuando se inicia, el elemento de origen aparecerá congelado por encima de la otra interfaz de usuario en la aplicación. Esto te permite realizar cualquier otra animación de transición al mismo tiempo. Por este motivo, no debes esperar más de unos 250 milisegundos entre los dos pasos porque la presencia del elemento de origen puede volverse una distracción. Si preparas una animación y no se inicia en el plazo de tres segundos, el sistema desechará la animación y todas las subsiguientes llamadas a [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) generarán un error.

## <a name="connected-animation-in-list-and-grid-experiences"></a>Animación conectado en experiencias de lista y cuadrícula

A menudo, querrás crear una animación conectada desde un control de lista o cuadrícula o hacia él. Puedes usar los dos métodos de [ListView](/uwp/api/windows.ui.xaml.controls.listview) y [GridView](/uwp/api/windows.ui.xaml.controls.gridview), [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) y [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync), para simplificar este proceso.

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

Para preparar una animación conectada con la elipse correspondiente a un elemento de lista determinado, llama al método de [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) con una clave única, el elemento y el nombre "PortraitEllipse".

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

Para iniciar una animación con este elemento como el destino, por ejemplo, cuando copia de navegar desde una vista detallada, usa [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync). Si acabas de cargar el origen de datos para la clase ListView, TryStartConnectedAnimationAsync esperará para iniciar la animación hasta que se haya creado el contenedor de elementos correspondiente.

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

Una *animación coordinada* es un tipo especial de animación de entrada que un elemento parece junto con el destino de animación conectada, anima conjuntamente con el elemento de animación conectada a medida que se mueve por la pantalla. Las animaciones coordinadas pueden sumar interés visual a una transición y llamar aún más la atención del usuario al contexto que comparten las vistas de origen y de destino. En estas imágenes, la interfaz de usuario del título para el elemento ser anima con una animación coordinada.

Cuando una animación coordinada usa la configuración de gravedad, gravedad se aplica el elemento de animación conectada y los elementos coordinados. Los elementos coordinados se "swoop" junto con el elemento conectado para que los elementos de estar coordinados realmente.

Usa la sobrecarga de dos parámetros de **TryStart** para agregar elementos coordinados a una animación conectada. En este ejemplo se muestra una animación coordinada de un diseño de cuadrícula denominado "DescriptionRoot" que entra en junto con un elemento de animación conectada denominado "CoverImage".

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
- Usar [GravityConnectedAnimationConfiguration]() para la navegación hacia delante.
- Usar [DirectConnectedAnimationConfiguration]() para la navegación hacia atrás.
- No esperes en las solicitudes de red u otras operaciones asincrónicas de larga ejecución entre la preparación e iniciando una animación conectada. Es posible que tengas que cargar previamente la información necesaria para ejecutar la transición con antelación o usar una imagen de marcador de posición de baja resolución mientras se carga la imagen de alta resolución en la vista de destino.
- Usar [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) para evitar una animación de transición en un **marco** si estás usando **ConnectedAnimationService**, dado que las animaciones conectadas no están diseñadas para usarse simultáneamente con la navegación predeterminada transiciones. Para obtener más información sobre cómo usar las transiciones de navegación, consulta [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition).

## <a name="download-the-code-samples"></a>Descargar las muestras de código

Consulta la [muestra de animaciones conectadas](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/ConnectedAnimationSample) en la galería de muestras de [WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs).

## <a name="related-articles"></a>Artículos relacionados

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
