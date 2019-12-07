---
Description: Obtén información sobre cómo implementar la navegación hacia atrás para recorrer el historial de navegación del usuario dentro de una aplicación para UWP.
title: Historial de navegación y navegación hacia atrás
template: detail.hbs
op-migration-status: ready
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 05b435eb6f070634507c143bd028d2cb051c97bc
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735030"
---
# <a name="navigation-history-and-backwards-navigation-for-uwp-apps"></a>Historial de navegación y navegación hacia atrás para las aplicaciones para UWP

> **API importantes**: [evento BackRequested](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested), [clase SystemNavigationManager](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager), [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)

La Plataforma universal de Windows (UWP) proporciona un sistema de navegación hacia atrás coherente para recorrer el historial de navegación del usuario dentro de una aplicación y, según el dispositivo, para pasar de una aplicación a otra.

Para implementar la navegación hacia atrás en tu aplicación, coloca un [botón Atrás](#back-button) en la esquina superior izquierda de la interfaz de usuario de la aplicación. Si la aplicación usa el control [NavigationView](../controls-and-patterns/navigationview.md), puedes usar el [botón Atrás integrado de NavigationView](../controls-and-patterns/navigationview.md#backwards-navigation).

El usuario espera que el botón Atrás vaya a la ubicación anterior en el historial de navegación de la aplicación. Ten en cuenta que tú decides qué acciones de navegación agregar al historial de navegación y cómo responderán al presionar el botón Atrás.

## <a name="back-button"></a>Botón Atrás

Para crear un botón Atrás, usa el control [Button](../controls-and-patterns/buttons.md) con el estilo `NavigationBackButtonNormalStyle` y coloca el botón en la esquina superior izquierda de la interfaz de usuario de la aplicación (para obtener más detalles, consulta los ejemplos de código XAML más abajo).

![Botón Atrás en la parte superior izquierda de la interfaz de usuario de la aplicación](images/back-nav/BackEnabled.png)

```xaml
<Page>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <Button Style="{StaticResource NavigationBackButtonNormalStyle}"/>

    </Grid>
</Page>
```

Si la aplicación tiene un elemento [CommandBar](../controls-and-patterns/app-bars.md) superior, el control Button que tiene 44px de altura no se alineará muy bien con los AppBarButtons de 48px. Sin embargo, para evitar incoherencias, alinea la parte superior del control Button dentro de los límites de 48px.

![Botón Atrás en la barra de comandos superior](images/back-nav/CommandBar.png)

```xaml
<Page>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <CommandBar>
            <CommandBar.Content>
                <Button Style="{StaticResource NavigationBackButtonNormalStyle}" VerticalAlignment="Top"/>
            </CommandBar.Content>
        
            <AppBarButton Icon="Delete" Label="Delete"/>
            <AppBarButton Icon="Save" Label="Save"/>
        </CommandBar>
    </Grid>
</Page>
```

Para minimizar los elementos de la interfaz de usuario que se mueven por la aplicación, muestra un botón Atrás deshabilitado cuando no haya nada en la pila de retroceso (consulta el ejemplo de código siguiente). Sin embargo, si esperas que la aplicación nunca va a tener una pila de retroceso, no es necesario mostrar el botón Atrás para nada.

![Estados del botón Atrás](images/back-nav/BackDisabled.png)

## <a name="code-example"></a>Ejemplo de código

El siguiente ejemplo de código muestra cómo implementar el comportamiento de navegación hacia atrás con un botón Atrás. El código responde al evento Button [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click) y habilita o deshabilita la visibilidad del botón en [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_), al que se llama cuando se navega a una página nueva. El ejemplo de código también controla las entradas de las teclas de retroceso del sistema de hardware y software mediante el registro de un cliente de escucha para el evento [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested).

```xaml
<!-- MainPage.xaml -->
<Page x:Class="AppName.MainPage">
...
<Button x:Name="BackButton" Click="Back_Click" Style="{StaticResource NavigationBackButtonNormalStyle}"/>
...
<Page/>
```

Código subyacente:

```csharp
// MainPage.xaml.cs
public MainPage()
{
    KeyboardAccelerator GoBack = new KeyboardAccelerator();
    GoBack.Key = VirtualKey.GoBack;
    GoBack.Invoked += BackInvoked;
    KeyboardAccelerator AltLeft = new KeyboardAccelerator();
    AltLeft.Key = VirtualKey.Left;
    AltLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(GoBack);
    this.KeyboardAccelerators.Add(AltLeft);
    // ALT routes here
    AltLeft.Modifiers = VirtualKeyModifiers.Menu;
}

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    BackButton.IsEnabled = this.Frame.CanGoBack;
}

private void Back_Click(object sender, RoutedEventArgs e)
{
    On_BackRequested();
}

// Handles system-level BackRequested events and page-level back button Click events
private bool On_BackRequested()
{
    if (this.Frame.CanGoBack)
    {
        this.Frame.GoBack();
        return true;
    }
    return false;
}

private void BackInvoked (KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}
```

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"

#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Xaml.Controls.h"
#include "winrt/Windows.UI.Xaml.Input.h"
#include "winrt/Windows.UI.Xaml.Navigation.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;

namespace winrt::PageNavTest::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();

        Windows::UI::Xaml::Input::KeyboardAccelerator goBack;
        goBack.Key(Windows::System::VirtualKey::GoBack);
        goBack.Invoked({ this, &MainPage::BackInvoked });
        Windows::UI::Xaml::Input::KeyboardAccelerator altLeft;
        altLeft.Key(Windows::System::VirtualKey::Left);
        altLeft.Invoked({ this, &MainPage::BackInvoked });
        KeyboardAccelerators().Append(goBack);
        KeyboardAccelerators().Append(altLeft);
        // ALT routes here.
        altLeft.Modifiers(Windows::System::VirtualKeyModifiers::Menu);
    }

    void MainPage::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
    {
        BackButton().IsEnabled(Frame().CanGoBack());
    }

    void MainPage::Back_Click(IInspectable const&, RoutedEventArgs const&)
    {
        On_BackRequested();
    }

    // Handles system-level BackRequested events and page-level back button Click events.
    bool MainPage::On_BackRequested()
    {
        if (Frame().CanGoBack())
        {
            Frame().GoBack();
            return true;
        }
        return false;
    }

    void MainPage::BackInvoked(Windows::UI::Xaml::Input::KeyboardAccelerator const& sender,
        Windows::UI::Xaml::Input::KeyboardAcceleratorInvokedEventArgs const& args)
    {
        args.Handled(On_BackRequested());
    }
}
```

Arriba, controlamos la navegación hacia atrás para una sola página. Puedes controlar la navegación en cada página si quieres excluir páginas específicas de la navegación del botón Atrás, o si quieres ejecutar código en el nivel de página antes de que se muestre la página.

Para controlar la navegación hacia atrás para una aplicación completa, debes registrar un cliente de escucha global para el evento [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) en el archivo de código subyacente `App.xaml`.

Código subyacente de App.xaml:

```csharp
// App.xaml.cs
Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
Frame rootFrame = Window.Current.Content;
rootFrame.PointerPressed += On_PointerPressed;

private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    e.Handled = On_BackRequested();
}

private void On_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    bool isXButton1Pressed =
        e.GetCurrentPoint(sender as UIElement).Properties.PointerUpdateKind == PointerUpdateKind.XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled = On_BackRequested();
    }
}

private bool On_BackRequested()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}
```

```cppwinrt
// App.cpp
#include <winrt/Windows.UI.Core.h>
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"

#include "App.h"
#include "MainPage.h"

using namespace winrt;
...

    Windows::UI::Core::SystemNavigationManager::GetForCurrentView().BackRequested({ this, &App::App_BackRequested });
    Frame rootFrame{ nullptr };
    auto content = Window::Current().Content();
    if (content)
    {
        rootFrame = content.try_as<Frame>();
    }
    rootFrame.PointerPressed({ this, &App::On_PointerPressed });
...

void App::App_BackRequested(IInspectable const& /* sender */, Windows::UI::Core::BackRequestedEventArgs const& e)
{
    e.Handled(On_BackRequested());
}

void App::On_PointerPressed(IInspectable const& sender, Windows::UI::Xaml::Input::PointerRoutedEventArgs const& e)
{
    bool isXButton1Pressed =
        e.GetCurrentPoint(sender.as<UIElement>()).Properties().PointerUpdateKind() == Windows::UI::Input::PointerUpdateKind::XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled(On_BackRequested());
    }
}

// Handles system-level BackRequested events.
bool App::On_BackRequested()
{
    if (Frame().CanGoBack())
    {
        Frame().GoBack();
        return true;
    }
    return false;
}
```

## <a name="optimizing-for-different-device-and-form-factors"></a>Optimización para diferentes dispositivos y factores de forma

Esta guía de diseño para la navegación hacia atrás es aplicable a todos los dispositivos, pero diferentes dispositivos y factores de forma pueden beneficiarse de una optimización. Esto también depende del botón Atrás de hardware que admiten distintos shells.

- **Teléfono o tableta**: en dispositivos móviles y tabletas siempre está presente un botón Atrás de hardware o software, pero te recomendamos dibujar un botón Atrás en la aplicación para mayor claridad.
- **Escritorio o concentrador**: dibuja el botón Atrás de la aplicación en la esquina superior izquierda de la interfaz de usuario de la aplicación.
- **Xbox o televisión**: no dibujes un botón Atrás, porque esto agregará desorden a la interfaz de usuario de forma innecesaria. En su lugar, usa el botón B del controlador para juegos con el fin de navegar hacia atrás.

Si la aplicación se va a ejecutar en varios dispositivos, [crea un desencadenador visual personalizado para Xbox](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox) con el fin de alternar la visibilidad del botón. El control NavigationView alternará automáticamente la visibilidad del botón Atrás si la aplicación se ejecuta en Xbox. 

Te recomendamos que incluyas compatibilidad para las siguientes entradas para la navegación hacia atrás. (Ten en cuenta que algunas de estas entradas no son compatibles con BackRequested del sistema y deben controlarse mediante eventos separados).

| Entrada | Evento |
| --- | --- |
| Windows: tecla Retroceso | BackRequested |
| Botón Atrás de hardware | BackRequested |
| Botón Atrás del modo tableta de shell | BackRequested |
| VirtualKey.XButton1 | PointerPressed |
| VirtualKey.GoBack | KeyboardAccelerator.BackInvoked |
| Tecla Alt+flecha izquierda | KeyboardAccelerator.BackInvoked |

Los ejemplos de código anteriores muestran cómo controlar todas estas entradas.

## <a name="system-back-behavior-for-backward-compatibilities"></a>Comportamiento hacia atrás del sistema para la compatibilidad con versiones anteriores

Anteriormente, las aplicaciones para UWP usaban [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility) para la navegación hacia atrás. La API se seguirá admitiendo para garantizar la compatibilidad con versiones anteriores, pero ya no se recomienda depender de [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility). En su lugar, tu aplicación debería dibujar su propio botón Atrás en la aplicación.

Si la aplicación continúa usando [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility), el sistema de la interfaz de usuario representará el botón Atrás del sistema en la barra de título. (Las interacciones de apariencia y de usuario para el botón Atrás son iguales que en las compilaciones anteriores).

![Botón Atrás de la barra de título](images/nav-back-pc.png)

## <a name="guidelines-for-custom-back-navigation-behavior"></a>Guía para el comportamiento personalizado de la navegación hacia atrás

Si decides proporcionar tu propia navegación de pila de retroceso, la experiencia debe ser coherente con otras aplicaciones. Te recomendamos que sigas los siguientes patrones de acciones de navegación:

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">Acción de navegación</th>
<th align="left">¿Agregar al historial de navegación?</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><strong>Página a página, grupos diferentes del mismo nivel</strong></td>
<td style="vertical-align:top;"><strong>Sí</strong>
<p>En esta ilustración, el usuario va del nivel 1 de la aplicación al nivel 2, cruzando grupos del mismo nivel, de forma que la navegación se agrega al historial de navegación.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Navigation across peer groups" /></p>
<p>En la siguiente ilustración, el usuario navega entre dos grupos del mismo nivel, cruzando nuevamente grupos del mismo nivel, de modo que la navegación se agrega al historial de navegación.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Navigation across peer groups" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Página a página, mismo grupo del mismo nivel, sin elemento de navegación en pantalla</strong>
<p>El usuario navega de una página a otra con el mismo grupo del mismo nivel. No hay elemento de navegación en pantalla (como <a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>) que proporcione navegación directa a las dos páginas.</p></td>
<td style="vertical-align:top;"><strong>Sí</strong>
<p>En la siguiente ilustración, el usuario navega entre las dos páginas en el mismo grupo del mismo nivel y la navegación se debe agregar al historial de navegación.</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Página a página, mismo grupo del mismo nivel, con un elemento de navegación en pantalla</strong>
<p>El usuario navega de una página a otra en el mismo grupo del mismo nivel. Ambas páginas se muestran en el mismo elemento de navegación, como <a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>.</p></td>
<td style="vertical-align:top;"><strong>Depende</strong>
<p>Sí, se agrega al historial de navegación, con 2 excepciones importantes. Si esperas que los usuarios de tu aplicación cambien de una página a otra en el grupo del mismo nivel con frecuencia, o si quieres conservar la jerarquía de navegación, no agregues al historial de navegación. En este caso, cuando el usuario presiona o pulsa Atrás, vuelve a la página previa antes de que este haya navegado al grupo actual del mismo nivel. </p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Mostrar una interfaz de usuario transitoria </strong>
<p>La aplicación muestra una ventana emergente o secundaria (como un cuadro de diálogo, pantalla de presentación o teclado en pantalla), o bien la aplicación entra en un modo especial, como el modo de selección múltiple.</p></td>
<td style="vertical-align:top;"><strong>No</strong>
<p>Cuando el usuario presiona el botón Atrás, se descarta la interfaz de usuario transitoria (oculta el teclado en pantalla, cancela el cuadro de diálogo, etc.) y vuelve a la página que genera la interfaz de usuario transitoria.</p>
<p><img src="images/back-nav/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Enumerar elementos</strong>
<p>La aplicación muestra el contenido de un elemento en pantalla, como los detalles del elemento seleccionado en la lista maestro y detalles.</p></td>
<td style="vertical-align:top;"><strong>No</strong>
<p>Enumerar elementos es similar a navegar dentro de un grupo del mismo nivel. Cuando el usuario presiona o pulsa Atrás, va a la página que precede a la página actual que contiene la enumeración de elementos.</p>
<p><img src="images/back-nav/nav-enumerate.png" alt="Iterm enumeration" /></p></td>
</tr>
</tbody>
</table>
</div>

### <a name="resuming"></a>Reanudar

Cuando el usuario cambia a otra aplicación y vuelve a tu aplicación, te recomendamos que este vuelva a la última página del historial de navegación.

## <a name="related-articles"></a>Artículos relacionados

- [Conceptos básicos de navegación](navigation-basics.md)
