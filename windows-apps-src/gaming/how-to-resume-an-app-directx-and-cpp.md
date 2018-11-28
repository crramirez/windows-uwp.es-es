---
title: Cómo reanudar una aplicación (DirectX y C++)
description: En este tema se muestra cómo restaurar datos importantes de la aplicación cuando el sistema reanuda la aplicación DirectX para la Plataforma universal de Windows (UWP).
ms.assetid: 5e6bb673-6874-ace5-05eb-f88c045f2178
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, reanudar, resuming, directx
ms.localizationpriority: medium
ms.openlocfilehash: f0aa60061ae9fc14392bfe4beb0693ba50fda0df
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7843171"
---
# <a name="how-to-resume-an-app-directx-and-c"></a>Cómo reanudar una aplicación (DirectX y C++)



En este tema se muestra cómo restaurar datos importantes de la aplicación cuando el sistema reanude la aplicación DirectX para la Plataforma universal de Windows (UWP).

## <a name="register-the-resuming-event-handler"></a>Registrar el controlador de eventos de reanudación


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

## <a name="refresh-displayed-content-after-suspension"></a>Actualizar el contenido mostrado tras la suspensión


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

## <a name="remarks"></a>Observaciones


El sistema suspende la aplicación cuando el usuario cambia a otra aplicación o al escritorio. El sistema reanuda la aplicación cuando el usuario vuelve a cambiar a ella. Cuando el sistema reanuda la aplicación, el contenido de las variables y las estructuras de datos es el mismo que antes de que el sistema la suspendiera. El sistema restaura la aplicación en el punto exacto en el que estaba, para que parezca al usuario que se ejecutaba en segundo plano No obstante, es posible que la aplicación haya estado suspendida durante un período de tiempo largo. Por ello, debes actualizar el contenido mostrado que puede haber cambiado mientras la aplicación estaba suspendida, y reiniciar cualquier subproceso de representación o de procesamiento de audio. Si guardaste datos de estado del juego durante un evento de suspensión anterior, restáuralos ahora.

## <a name="related-topics"></a>Temas relacionados

* [Cómo suspender una aplicación (DirectX y C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [Cómo activar una aplicación (DirectX y C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 




