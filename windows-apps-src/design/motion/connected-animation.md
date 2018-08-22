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
ms.openlocfilehash: a789a8f082192b79b3e96990827f9a4f6a0eacbc
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "2801189"
---
# <a name="connected-animation-for-uwp-apps"></a>Animación conectada para aplicaciones para UWP

## <a name="what-is-connected-animation"></a>¿Qué es una animación conectada?

Las animaciones conectadas te permiten crear una experiencia de navegación dinámica y atractiva al animar la transición de un elemento entre dos vistas distintas. Esto ayuda al usuario a mantener el contexto y ofrece continuidad entre las vistas.
En una animación conectada, un elemento parece "continuar" entre dos vistas durante un cambio en el contenido de la interfaz de usuario, volando por la pantalla desde su ubicación en la vista de origen hasta su destino en la nueva vista. Esto enfatiza el contenido común entre las vistas y crea un efecto atractivo y dinámico como parte de una transición.

## <a name="see-it-in-action"></a>Vela en acción

En este breve vídeo, una aplicación usa una animación conectada para animar la imagen de un elemento en la medida en que "continúa" para formar parte del encabezado de la página siguiente. El efecto te ayuda a mantener al usuario en contexto a través de la transición.

![Ejemplo de interfaz de usuario de movimiento continuo](images/continuous3.gif)

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

## <a name="how-to-implement"></a>Cómo se implementa

La configuración de una animación conectada consta de dos pasos:

1.  *Preparar* un objeto de animación en la página de origen, lo que indica al sistema que el elemento de origen participará en la animación conectada. 
2.  *Iniciar* la animación en la página de destino, pasando una referencia al elemento de destino.

Entre los dos pasos, el elemento de origen aparecerá congelado por encima del resto de la interfaz de usuario en la aplicación, lo que te permite realizar cualquier otra animación de transición al mismo tiempo. Por este motivo, no debes esperar más de unos 250milisegundos entre los dos pasos, ya que la presencia del elemento de origen puede volverse una distracción. Si preparas una animación y no se inicia en el plazo de tres segundos, el sistema desechará la animación y todas las subsiguientes llamadas a **TryStart** generarán un error.

Puedes acceder a la instancia actual de ConnectedAnimationService llamando a **ConnectedAnimationService.GetForCurrentView**. Para preparar una animación, llama a **PrepareToAnimate** en esta instancia, pasando una referencia a una clave única y el elemento de la interfaz de usuario que quieres usar en la transición. La clave única te permite recuperar la animación más adelante, por ejemplo, en otra página.

```csharp
ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("image", SourceImage);
```

Para iniciar la animación, llama a **ConnectedAnimation.TryStart**. Puedes recuperar la instancia de animación correcta llamando a **ConnectedAnimationService.GetAnimation** con la clave única que proporcionaste cuando creaste la animación.

```csharp
ConnectedAnimation imageAnimation = 
    ConnectedAnimationService.GetForCurrentView().GetAnimation("image");
if (imageAnimation != null)
{
    imageAnimation.TryStart(DestinationImage);
}
```

Aquí tienes un ejemplo completo del uso de ConnectedAnimationService para crear una transición entre dos páginas.

*SourcePage.xaml*

```xaml
<Image x:Name="SourceImage"
       Width="200"
       Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

*SourcePage.xaml.cs*

```csharp
private void NavigateToDestinationPage()
{
    ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("image", SourceImage);
    Frame.Navigate(typeof(DestinationPage));
}
```

*DestinationPage.xaml*

```xaml
<Image x:Name="DestinationImage"
       Width="400"
       Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

*DestinationPage.xaml.cs*

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation imageAnimation = 
        ConnectedAnimationService.GetForCurrentView().GetAnimation("image");
    if (imageAnimation != null)
    {
        imageAnimation.TryStart(DestinationImage);
    }
}
```

## <a name="connected-animation-in-list-and-grid-experiences"></a>Animación conectado en experiencias de lista y cuadrícula

A menudo, querrás crear una animación conectada desde un control de lista o cuadrícula o hacia él. Para simplificar este proceso, puedes usar dos nuevos métodos de **ListView** y **GridView**: **PrepareConnectedAnimation** y **TryStartConnectedAnimationAsync**.
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

Para preparar una animación conectada con la elipse correspondiente a un elemento de lista determinado, llama al método **PrepareConnectedAnimation** con una clave única, el elemento y el nombre "PortraitEllipse".

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

Como alternativa, para iniciar una animación con este elemento como destino, por ejemplo, cuando navegas de vuelta de una vista detallada, usa **TryStartConnectedAnimationAsync**. Si acabas de cargar el origen de datos para la clase **ListView**, **TryStartConnectedAnimationAsync** esperará para iniciar la animación hasta que se haya creado el contenedor de elementos correspondiente.

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

Una *animación coordinada* es un tipo especial de animación de entrada, en la que un elemento aparecerá junto con el destino de la animación conectada, que se anima a la par del elemento de la animación conectada a medida que se mueve por la pantalla. Las animaciones coordinadas pueden sumar interés visual a una transición y llamar aún más la atención del usuario al contexto que comparten las vistas de origen y de destino. En estas imágenes, la interfaz de usuario del título para el elemento ser anima con una animación coordinada.

Usa la sobrecarga de dos parámetros de **TryStart** para agregar elementos coordinados a una animación conectada. En este ejemplo se muestra una animación coordinada de un diseño de clase Grid denominado "DescriptionRoot" que entrará junto con un elemento de animación conectada denominado "CoverImage".

*DestinationPage.xaml*

```xaml
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

*DestinationPage.xaml.cs*

```csharp
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
- No esperes en las solicitudes de red u otras operaciones asincrónicas de larga ejecución entre la preparación y el inicio de una animación conectada. Es posible que tengas que cargar previamente la información necesaria para ejecutar la transición con antelación o usar una imagen de marcador de posición de baja resolución mientras se carga la imagen de alta resolución en la vista de destino.
- Usa [SuppressNavigationTransitionInfo](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) para evitar una animación de transición en un **Frame** si estás usando **ConnectedAnimationService**, ya que las animaciones conectadas no están diseñadas para usarse simultáneamente con las transiciones de navegación predeterminadas. Para obtener más información sobre cómo usar las transiciones de navegación, consulta [NavigationThemeTransition](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx).


## <a name="download-the-code-samples"></a>Descargar las muestras de código

Consulta la [muestra de animaciones conectadas](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/ConnectedAnimationSample) en la galería de muestras de [WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs).

## <a name="related-articles"></a>Artículos relacionados

- [ConnectedAnimation](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimation)
- [ConnectedAnimationService](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.connectedanimation.aspx)
- [NavigationThemeTransition](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx)
