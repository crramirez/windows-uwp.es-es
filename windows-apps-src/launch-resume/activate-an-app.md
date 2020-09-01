---
title: Controlar la activación de aplicaciones
description: Obtén información sobre cómo controlar la activación de aplicaciones mediante la invalidación del método OnLaunched.
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: 0c3aa86fd8eee3724e092799eea6d34f0b9d453b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164999"
---
# <a name="handle-app-activation"></a>Controlar la activación de aplicaciones

Obtenga información sobre cómo controlar la activación de la aplicación mediante la invalidación del método [**Application. onlaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) .

## <a name="override-the-launch-handler"></a>Invalidar el controlador de inicio

Cuando se activa una aplicación, por cualquier motivo, el sistema envía el evento [**CoreApplicationView. Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) . Para ver una lista de los tipos de activación, consulta la enumeración [**ActivationKind**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind).

La clase [**Windows.UI.Xaml.Application**](/uwp/api/Windows.UI.Xaml.Application) define los métodos que puedes invalidar para controlar los distintos tipos de activación. Varios de los tipos de activación tienen un método específico que puedes invalidar. Para los demás tipos de activación, invalida el método [**OnActivated**](/uwp/api/windows.ui.xaml.application.onactivated).

Define la clase de la aplicación.

```xml
<Application
    x:Class="AppName.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
```

Invalida el método [**OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched). Este método se llama cada vez que el usuario inicia la aplicación. El parámetro [**LaunchActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) contiene el estado anterior de la aplicación y los argumentos de activación.

> [!NOTE]
> En Windows, el inicio de una aplicación suspendida desde el icono de inicio o la lista de aplicaciones no llama a este método.

```csharp
using System;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

namespace AppName
{
   public partial class App
   {
      async protected override void OnLaunched(LaunchActivatedEventArgs args)
      {
         EnsurePageCreatedAndActivate();
      }

      // Creates the MainPage if it isn't already created.  Also activates
      // the window so it takes foreground and input focus.
      private MainPage EnsurePageCreatedAndActivate()
      {
         if (Window.Current.Content == null)
         {
             Window.Current.Content = new MainPage();
         }

         Window.Current.Activate();
         return Window.Current.Content as MainPage;
      }
   }
}
```

```vb
Class App
   Protected Overrides Sub OnLaunched(args As LaunchActivatedEventArgs)
      Window.Current.Content = New MainPage()
      Window.Current.Activate()
   End Sub
End Class
```

```cppwinrt
...
#include "MainPage.h"
#include "winrt/Windows.ApplicationModel.Activation.h"
#include "winrt/Windows.UI.Xaml.h"
#include "winrt/Windows.UI.Xaml.Controls.h"
...
using namespace winrt;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    /// <summary>
    /// Invoked when the application is launched normally by the end user.  Other entry points
    /// will be used such as when the application is launched to open a specific file.
    /// </summary>
    /// <param name="e">Details about the launch request and process.</param>
    void OnLaunched(LaunchActivatedEventArgs const& e)
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
        }

        // Do not repeat app initialization when the Window already has content,
        // just ensure that the window is active
        if (rootFrame == nullptr)
        {
            // Create a Frame to act as the navigation context and associate it with
            // a SuspensionManager key
            rootFrame = Frame();

            rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

            if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated)
            {
                // Restore the saved session state only when appropriate, scheduling the
                // final launch steps after the restore is complete
            }

            if (e.PrelaunchActivated() == false)
            {
                if (rootFrame.Content() == nullptr)
                {
                    // When the navigation stack isn't restored navigate to the first page,
                    // configuring the new page by passing required information as a navigation
                    // parameter
                    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), box_value(e.Arguments()));
                }
                // Place the frame in the current Window
                Window::Current().Content(rootFrame);
                // Ensure the current window is active
                Window::Current().Activate();
            }
        }
        else
        {
            if (e.PrelaunchActivated() == false)
            {
                if (rootFrame.Content() == nullptr)
                {
                    // When the navigation stack isn't restored navigate to the first page,
                    // configuring the new page by passing required information as a navigation
                    // parameter
                    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), box_value(e.Arguments()));
                }
                // Ensure the current window is active
                Window::Current().Activate();
            }
        }
    }
};
```

```cpp
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;
void App::OnLaunched(LaunchActivatedEventArgs^ args)
{
   EnsurePageCreatedAndActivate();
}

// Creates the MainPage if it isn't already created.  Also activates
// the window so it takes foreground and input focus.
void App::EnsurePageCreatedAndActivate()
{
    if (_mainPage == nullptr)
    {
        // Save the MainPage for use if we get activated later
        _mainPage = ref new MainPage();
    }
    Window::Current->Content = _mainPage;
    Window::Current->Activate();
}
```

## <a name="restore-application-data-if-app-was-suspended-then-terminated"></a>restaurar los datos de la aplicación si se suspendió y después finalizó

Cuando el usuario cambia a la aplicación finalizada, el sistema envía el evento [**Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated), con el objeto [**Kind**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.kind) establecido en **Launch** y el objeto [**PreviousExecutionState**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.previousexecutionstate) establecido en **Terminated** o **ClosedByUser**. La aplicación debe cargar sus datos de aplicación guardados y actualizar el contenido que muestra.

```csharp
async protected override void OnLaunched(LaunchActivatedEventArgs args)
{
   if (args.PreviousExecutionState == ApplicationExecutionState.Terminated ||
       args.PreviousExecutionState == ApplicationExecutionState.ClosedByUser)
   {
      // TODO: Populate the UI with the previously saved application data
   }
   else
   {
      // TODO: Populate the UI with defaults
   }

   EnsurePageCreatedAndActivate();
}
```

```vb
Protected Overrides Sub OnLaunched(args As Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)
   Dim restoreState As Boolean = False

   Select Case args.PreviousExecutionState
      Case ApplicationExecutionState.Terminated
         ' TODO: Populate the UI with the previously saved application data
         restoreState = True
      Case ApplicationExecutionState.ClosedByUser
         ' TODO: Populate the UI with the previously saved application data
         restoreState = True
      Case Else
         ' TODO: Populate the UI with defaults
   End Select

   Window.Current.Content = New MainPage(restoreState)
   Window.Current.Activate()
End Sub
```

```cppwinrt
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs const& e)
{
    if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated ||
        e.PreviousExecutionState() == ApplicationExecutionState::ClosedByUser)
    {
        // Populate the UI with the previously saved application data.
    }
    else
    {
        // Populate the UI with defaults.
    }
    ...
}
```

```cpp
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ args)
{
   if (args->PreviousExecutionState == ApplicationExecutionState::Terminated ||
       args->PreviousExecutionState == ApplicationExecutionState::ClosedByUser)
   {
      // TODO: Populate the UI with the previously saved application data
   }
   else
   {
      // TODO: Populate the UI with defaults
   }

   EnsurePageCreatedAndActivate();
}
```

Si el valor de [**PreviousExecutionState**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.previousexecutionstate) es **NotRunning**, la aplicación no pudo guardar sus datos de aplicación correctamente y debe iniciarse desde cero, como si se estuviera iniciando por primera vez.

## <a name="remarks"></a>Observaciones

> [!NOTE]
> Las aplicaciones pueden omitir la inicialización si ya hay contenido establecido en la ventana actual. Puede comprobar la propiedad [**LaunchActivatedEventArgs. TileId**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.tileid) para determinar si la aplicación se inició desde un icono principal o secundario y, en función de esa información, decidir si debe presentar una experiencia de aplicación nueva o reanudar.

## <a name="important-apis"></a>API importantes
* [Windows. ApplicationModel. Activation](/uwp/api/Windows.ApplicationModel.Activation)
* [Windows.UI.Xaml.Application](/uwp/api/Windows.UI.Xaml.Application)

## <a name="related-topics"></a>Temas relacionados
* [Controlar la suspensión de aplicaciones](suspend-an-app.md)
* [Controlar la reanudación de aplicaciones](resume-an-app.md)
* [Directrices para suspender y reanudar una aplicación](./index.md)
* [Ciclo de vida de la aplicación](app-lifecycle.md)