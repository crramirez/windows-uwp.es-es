---
title: Cómo activar una aplicación (DirectX y C++)
description: En este tema se muestra cómo definir la experiencia de activación de una aplicación DirectX para la Plataforma universal de Windows (UWP).
ms.assetid: b07c7da1-8a5e-5b57-6f77-6439bf653a53
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, DirectX, activación
ms.localizationpriority: medium
ms.openlocfilehash: 839cfc718e6225beb14df535bc48f9ba6f3f6dc5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173149"
---
# <a name="how-to-activate-an-app-directx-and-c"></a>Cómo activar una aplicación (DirectX y C++)



En este tema se muestra cómo definir la experiencia de activación de una aplicación DirectX para la Plataforma universal de Windows (UWP).

## <a name="register-the-app-activation-event-handler"></a>Registrar el controlador de eventos de activación de la aplicación


Primero, haz un registro para controlar el evento [**CoreApplicationView::Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated), que se activa cuando el sistema operativo inicia e inicializa la aplicación.

Agrega este código a la implementación del método [**IFrameworkView::Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) del proveedor de vistas (denominado **MyViewProvider** en el ejemplo):

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
    // Register event handlers for the app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);
  
  //...

}
```

## <a name="activate-the-corewindow-instance-for-the-app"></a>Activar la instancia de CoreWindow para la aplicación


Cuando se inicia la aplicación, debes obtener una referencia a [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) para la aplicación. **CoreWindow** contiene el distribuidor de mensajes de eventos de ventana que la aplicación usa para procesar eventos de ventana. Obtén esta referencia en la devolución de llamada del evento de activación de la aplicación con una llamada a [**CoreWindow::GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread). Después de obtener esta referencia, activa la ventana de aplicación principal con una llamada a [**CoreWindow::Activate**](/uwp/api/windows.ui.core.corewindow.activate).

```cpp
void App::OnActivated(CoreApplicationView^ applicationView, IActivatedEventArgs^ args)
{
    // Run() won't start until the CoreWindow is activated.
    CoreWindow::GetForCurrentThread()->Activate();
}
```

## <a name="start-processing-event-message-for-the-main-app-window"></a>Iniciar el mensaje de evento de procesamiento para la ventana de aplicación principal


Las devoluciones de llamada se producen a medida que [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) procesa los mensajes de eventos para el elemento [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) de la aplicación. Esta devolución de llamada no se invocará si no llamas a [**CoreDispatcher::ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) desde el bucle principal de la aplicación (implementado en el método [**IFrameworkView::Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) del proveedor de vistas).

``` syntax
// This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

## <a name="related-topics"></a>Temas relacionados


* [Cómo suspender una aplicación (DirectX y C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [Cómo reanudar una aplicación (DirectX y C++)](how-to-resume-an-app-directx-and-cpp.md)

 

 