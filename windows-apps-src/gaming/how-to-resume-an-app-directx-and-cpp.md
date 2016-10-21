---
author: mtoepke
title: "Cómo reanudar una aplicación (DirectX y C++)"
description: "En este tema se muestra cómo restaurar datos importantes de la aplicación cuando el sistema reanuda la aplicación DirectX para la Plataforma universal de Windows (UWP)."
ms.assetid: 5e6bb673-6874-ace5-05eb-f88c045f2178
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 978f779eaeb732b549657751c11cd2192728999b

---

# Cómo reanudar una aplicación (DirectX y C++)


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este tema se muestra cómo restaurar datos importantes de la aplicación cuando el sistema reanude la aplicación DirectX para la Plataforma universal de Windows (UWP).

## Registrar el controlador de eventos de reanudación


Haz un registro para controlar el evento [**CoreApplication::Resuming**](https://msdn.microsoft.com/library/windows/apps/br205859), que indica que el usuario abandonó la aplicación y después volvió.

Agrega este código a la implementación del método [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) del proveedor de vistas:

```cpp
// The first method is called when the IFrameworkView is being created.
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);
    
  //...

}
```

## Actualizar el contenido mostrado tras la suspensión


Cuando la aplicación controla el evento Resuming, tiene la oportunidad de actualizar el contenido mostrado. Restaura cualquier aplicación que hayas guardado con el controlador para [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) y reinicia el procesamiento. Nota para desarrolladores de juegos: Si suspendiste el motor de audio, es el momento de reiniciarlo.

```cpp
void App::OnResuming(Platform::Object^ sender, Platform::Object^ args)
{
    // Restore any data or state that was unloaded on suspend. By default, data
    // and state are persisted when resuming from suspend. Note that this event
    // does not occur if the app was previously terminated.

    // Insert your code here.
}
```

Esta devolución de llamada se produce como un mensaje de evento procesado por [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) para el elemento [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) de la aplicación. Esta devolución de llamada no se invocará si no llamas a [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) desde el bucle principal de la aplicación (implementado en el método [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) del proveedor de vistas).

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

## Observaciones


El sistema suspende la aplicación cuando el usuario cambia a otra aplicación o al escritorio. El sistema reanuda la aplicación cuando el usuario vuelve a cambiar a ella. Cuando el sistema reanuda la aplicación, el contenido de las variables y las estructuras de datos es el mismo que antes de que el sistema la suspendiera. El sistema restaura la aplicación en el punto exacto en el que estaba, para que parezca al usuario que se ejecutaba en segundo plano No obstante, es posible que la aplicación haya estado suspendida durante un período de tiempo largo. Por ello, debes actualizar el contenido mostrado que puede haber cambiado mientras la aplicación estaba suspendida, y reiniciar cualquier subproceso de representación o de procesamiento de audio. Si guardaste datos de estado del juego durante un evento de suspensión anterior, restáuralos ahora.

## Temas relacionados

* [Cómo suspender una aplicación (DirectX y C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [Cómo activar una aplicación (DirectX y C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 







<!--HONumber=Aug16_HO3-->


