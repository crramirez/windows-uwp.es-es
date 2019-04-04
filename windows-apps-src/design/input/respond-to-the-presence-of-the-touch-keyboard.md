---
Description: Aprende a adaptar la interfaz de usuario de la aplicación al mostrar u ocultar el teclado táctil.
title: Responder a la presencia del teclado táctil
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
keywords: teclado, accesibilidad, navegación, foco, texto, entrada, interacciones del usuario, keyboard, accessibility, navigation, focus, text, input, user interactions
ms.date: 07/13/2018
ms.topic: article
ms.openlocfilehash: e44b7cf5a61a795e52490f6d603aea0bcf87bea2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658300"
---
# <a name="respond-to-the-presence-of-the-touch-keyboard"></a>Responder a la presencia del teclado táctil

Aprende a adaptar la interfaz de usuario de la aplicación al mostrar u ocultar el teclado táctil.

### <a name="important-apis"></a>API importantes

- [AutomationPeer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
- [InputPane](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane)

![teclado táctil en el modo de diseño predeterminado](images/keyboard/default.png)

<sup>La interacción de teclado de forma predeterminada en el modo de diseño</sup>

El teclado táctil permite la entrada de texto para dispositivos que admiten la entrada táctil. Controles de entrada de texto de plataforma de Windows (UWP) universal invocan al teclado táctil de manera predeterminada cuando un usuario pulsa en un campo de entrada editable. El teclado táctil normalmente permanece visible mientras el usuario navega por los controles en cierta forma, pero este comportamiento puede variar en función de los otros tipos de control dentro de la forma.

Para admitir el comportamiento del teclado táctil correspondiente en un control de entrada de texto personalizado que no se deriva de un control de entrada de texto estándar, debe usar el <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer">AutomationPeer</a> clase para exponer sus controles a la automatización de interfaz de usuario de Microsoft y implementar los patrones de control de automatización de interfaz de usuario correctos. Consulta [accesibilidad de teclado](https://docs.microsoft.com/windows/uwp/design/accessibility/keyboard-accessibility) y [automatización del mismo nivel personalizado](https://docs.microsoft.com/windows/uwp/design/accessibility/custom-automation-peers).

Una vez que esta compatibilidad esté agregada a tu control personalizado, puedes responder adecuadamente a la presencia del teclado táctil.

**Requisitos previos:**

Este tema se basa en [interacciones de teclado](keyboard-interactions.md).

Debes tener un conocimiento básico de las interacciones de teclado estándar, el control de la entrada de teclado y los eventos, y la automatización de la interfaz de usuario.

Si acabas de empezar a desarrollar aplicaciones para la Plataforma universal de Windows (UWP), consulta estos temas para familiarizarte con las tecnologías que te presentamos aquí.

- [Crea tu primera aplicación.](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
- Encontrarás más información acerca de los eventos, en [Introducción a eventos y eventos enrutados](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

**Instrucciones para la experiencia de usuario:**

Para obtener sugerencias útiles sobre cómo diseñar una aplicación útil y atractiva optimizada para la entrada de teclado, consulte [interacciones de teclado](https://docs.microsoft.com/windows/uwp/design/input/keyboard-interactions) .

## <a name="touch-keyboard-and-a-custom-ui"></a>Teclado táctil y una interfaz de usuario personalizada

Estas son algunas recomendaciones básicas para los controles de entrada de texto personalizado.

- Muestra el teclado táctil manteniendo la plena interacción con el formulario.

- Asegúrese de que los controles personalizados tienen la automatización de interfaz de usuario adecuada [AutomationControlType](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) para el teclado conservar cuando el foco se desplaza de un campo de entrada de texto en el contexto de entrada de texto. Por ejemplo, si tienes un menú que se abre en medio de una situación de entrada de texto y quieres que se mantenga el teclado, el menú debe tener el menú **AutomationControlType**.

- No manipules las propiedades de automatización de la interfaz de usuario para controlar el teclado táctil. Otras herramientas de accesibilidad dependen de la precisión de las propiedades de automatización de la interfaz de usuario.

- Asegúrate de que los usuarios siempre puedan ver el campo de entrada con el que están interactuando.

    Dado que el teclado táctil tapa una gran parte de la pantalla, el UWP garantiza que el campo de entrada con el foco se desplace en la vista como un usuario navega por los controles del formulario, incluidos los controles que no estén en la vista.

    Al personalizar la interfaz de usuario, proporcionar un comportamiento similar en la apariencia del teclado táctil controlando el [mostrando](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing) y [ocultar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding) eventos expuestos por el [ **InputPane** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane) objeto.

    ![Formulario con y sin el teclado táctil visible](images/touch-keyboard-pan1.png)

    En algunos casos, algunos elementos de la interfaz de usuario deben permanecer en la pantalla todo el tiempo. Diseña la interfaz de usuario de modo que los controles del formulario estén incluidos en una región de movimiento panorámico y los elementos de interfaz de usuario importantes permanezcan estáticos. Por ejemplo:

    ![Un formulario que contiene áreas que deben permanecer siempre visibles](images/touch-keyboard-pan2.png)

## <a name="handling-the-showing-and-hiding-events"></a>Control de eventos Showing y Hiding

Este es un ejemplo de adjuntar controladores de eventos para el [mostrando](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing) y [ocultar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding) eventos del teclado táctil.

```csharp
using Windows.UI.ViewManagement;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Foundation;
using Windows.UI.Xaml.Navigation;

namespace SDKTemplate
{
    /// <summary>
    /// Sample page to subscribe show/hide event of Touch Keyboard.
    /// </summary>
    public sealed partial class Scenario2_ShowHideEvents : Page
    {
        public Scenario2_ShowHideEvents()
        {
            this.InitializeComponent();
        }

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Subscribe to Showing/Hiding events
            currentInputPane.Showing += OnShowing;
            currentInputPane.Hiding += OnHiding;
        }

        protected override void OnNavigatedFrom(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Unsubscribe from Showing/Hiding events
            currentInputPane.Showing -= OnShowing;
            currentInputPane.Hiding -= OnHiding;
        }

        void OnShowing(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Showing";
        }
        
        void OnHiding(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Hiding";
        }
    }
}
```

```cppwinrt
...
#include <winrt/Windows.UI.ViewManagement.h>
...
private:
    winrt::event_token m_showingEventToken;
    winrt::event_token m_hidingEventToken;
...
Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Subscribe to Showing/Hiding events
    m_showingEventToken = inputPane.Showing({ this, &Scenario2_ShowHideEvents::OnShowing });
    m_hidingEventToken = inputPane.Hiding({ this, &Scenario2_ShowHideEvents::OnHiding });
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Unsubscribe from Showing/Hiding events
    inputPane.Showing(m_showingEventToken);
    inputPane.Hiding(m_hidingEventToken);
}

void Scenario2_ShowHideEvents::OnShowing(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Showing");
}

void Scenario2_ShowHideEvents::OnHiding(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Hiding");
}
```

```cpp
#include "pch.h"
#include "Scenario2_ShowHideEvents.xaml.h"

using namespace SDKTemplate;
using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::UI::ViewManagement;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Navigation;

Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(NavigationEventArgs^ e)
{
    auto inputPane = InputPane::GetForCurrentView();
    // Subscribe to Showing/Hiding events
    showingEventToken = inputPane->Showing +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnShowing);
    hidingEventToken = inputPane->Hiding +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnHiding);
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(NavigationEventArgs^ e)
{
    auto inputPane = Windows::UI::ViewManagement::InputPane::GetForCurrentView();
    // Unsubscribe from Showing/Hiding events
    inputPane->Showing -= showingEventToken;
    inputPane->Hiding -= hidingEventToken;
}

void Scenario2_ShowHideEvents::OnShowing(InputPane^ /*sender*/, InputPaneVisibilityEventArgs^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Showing";
}

void Scenario2_ShowHideEvents::OnHiding(InputPane^ /*sender*/, InputPaneVisibilityEventArgs ^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Hiding";
}
```

## <a name="related-articles"></a>Artículos relacionados

- [Interacciones de teclado](keyboard-interactions.md)
- [Accesibilidad de teclado](https://msdn.microsoft.com/library/windows/apps/mt244347)
- [Automatización del mismo nivel personalizado](https://msdn.microsoft.com/library/windows/apps/mt297667)

**Ejemplos**

- [Ejemplo de teclado táctil](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)

**Ejemplos de archivo**

- [Entrada: Ejemplo de teclado táctil](https://go.microsoft.com/fwlink/p/?linkid=246019)
- [Responder a la apariencia del teclado en pantalla de ejemplo](https://go.microsoft.com/fwlink/p/?linkid=231633)
- [Ejemplo de edición de texto XAML](https://go.microsoft.com/fwlink/p/?LinkID=251417)
- [Ejemplo de accesibilidad XAML](https://go.microsoft.com/fwlink/p/?linkid=238570)