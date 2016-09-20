---
author: mtoepke
title: "Cómo activar una aplicación (DirectX y C++)"
description: "En este tema se muestra cómo definir la experiencia de activación de una aplicación DirectX para la Plataforma universal de Windows (UWP)."
ms.assetid: b07c7da1-8a5e-5b57-6f77-6439bf653a53
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 14859d03c7af45a17772c76f8c79b3c1bc56272c

---

# Cómo activar una aplicación (DirectX y C++)


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este tema se muestra cómo definir la experiencia de activación de una aplicación DirectX para la Plataforma universal de Windows (UWP).

## Registrar el controlador de eventos de activación de la aplicación


Primero, haz un registro para controlar el evento [**CoreApplicationView::Activated**](https://msdn.microsoft.com/library/windows/apps/br225018), que se activa cuando el sistema operativo inicia e inicializa la aplicación.

Agrega este código a la implementación del método [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) del proveedor de vistas (denominado **MyViewProvider** en el ejemplo):

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

## Activar la instancia de CoreWindow para la aplicación


Cuando se inicia la aplicación, debes obtener una referencia a [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) para la aplicación. 
            **CoreWindow** contiene el distribuidor de mensajes de eventos de ventana que la aplicación usa para procesar eventos de ventana. Obtén esta referencia en la devolución de llamada del evento de activación de la aplicación con una llamada a [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589). Después de obtener esta referencia, activa la ventana de aplicación principal con una llamada a [**CoreWindow::Activate**](https://msdn.microsoft.com/library/windows/apps/br208254).

```cpp
void App::OnActivated(CoreApplicationView^ applicationView, IActivatedEventArgs^ args)
{
    // Run() won't start until the CoreWindow is activated.
    CoreWindow::GetForCurrentThread()->Activate();
}
```

## Iniciar el mensaje de evento de procesamiento para la ventana de aplicación principal


Las devoluciones de llamada se producen a medida que [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) procesa los mensajes de eventos para el elemento [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) de la aplicación. Esta devolución de llamada no se invocará si no llamas a [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) desde el bucle principal de la aplicación (implementado en el método [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) del proveedor de vistas).

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

## Temas relacionados


* [Cómo suspender una aplicación (DirectX y C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [Cómo reanudar una aplicación (DirectX y C++)](how-to-resume-an-app-directx-and-cpp.md)

 

 







<!--HONumber=Jun16_HO4-->


