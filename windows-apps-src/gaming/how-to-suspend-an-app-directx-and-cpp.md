---
author: mtoepke
title: "Cómo suspender una aplicación (DirectX y C++)"
description: "En este tema verás cómo guardar datos importantes de la aplicación y del estado del sistema cuando el sistema suspende la aplicación DirectX para la Plataforma universal de Windows (UWP)."
ms.assetid: 5dd435e5-ec7e-9445-fed4-9c0d872a239e
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: dd7319b254dcaaa5da7a7055bbde299f5e7e62a3

---

# Cómo suspender una aplicación (DirectX y C++)


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este tema verás cómo guardar datos importantes de la aplicación y del estado del sistema cuando el sistema suspende la aplicación DirectX para la Plataforma universal de Windows (UWP).

## Registrar el controlador de eventos de suspensión


Primero, realiza un registro para administrar el evento [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860), que se desencadena cuando la aplicación cambia a un estado de suspensión debido a la acción del usuario o del sistema.

Agrega este código a la implementación del método [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) del proveedor de vistas:

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

  //...
}
```

## Guarda los datos de la aplicación antes de la suspensión


Cuando la aplicación controla el evento [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860), tiene la oportunidad de guardar sus datos de aplicación importantes en la función de controlador. La aplicación debe utilizar la API de almacenamiento [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) para guardar datos de aplicación simples de manera sincrónica. Si vas a desarrollar un juego, guarda toda la información de estado sobre el juego que sea importante. No olvides suspender el procesamiento de audio.

A continuación, implementa la devolución de llamada. Guarda los datos de la aplicación en este método.

```cpp
void App::OnSuspending(Platform::Object^ sender, SuspendingEventArgs^ args)
{
    // Save app state asynchronously after requesting a deferral. Holding a deferral
    // indicates that the application is busy performing suspending operations. Be
    // aware that a deferral may not be held indefinitely. After about five seconds,
    // the app will be forced to exit.
    SuspendingDeferral^ deferral = args->SuspendingOperation->GetDeferral();

    create_task([this, deferral]()
    {
        m_deviceResources->Trim();

        // Insert your code here.

        deferral->Complete();
    });
}
```

Esta devolución de llamada debe completarse en 5 segundos. Durante esta devolución de llamada, debes solicitar un aplazamiento mediante una llamada a [**SuspendingOperation::GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690), que inicia la cuenta regresiva. Cuando la aplicación complete la operación de guardar, llama a [**SuspendingDeferral::Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) para indicar al sistema que la aplicación está lista para suspenderse. Si no solicitas un aplazamiento, o si la aplicación tarda más de 5 segundos en guardar los datos, la aplicación se suspenderá automáticamente.

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

## Llamar a Trim()


Desde Windows8.1, todas las aplicaciones de la TiendaWindows con DirectX deben llamar a [**IDXGIDevice3::Trim**](https://msdn.microsoft.com/library/windows/desktop/dn280346) al suspenderse. Esta llamada la indica al controlador gráfico que libere todos los búferes temporales asignados para la aplicación, lo que reduce la posibilidad de que se cierre la aplicación para reclamar recursos de memoria mientras esté en estado de suspensión. Este es un requisito de certificación para Windows8.1.

```cpp
void App::OnSuspending(Platform::Object^ sender, SuspendingEventArgs^ args)
{
    // Save app state asynchronously after requesting a deferral. Holding a deferral
    // indicates that the application is busy performing suspending operations. Be
    // aware that a deferral may not be held indefinitely. After about five seconds,
    // the app will be forced to exit.
    SuspendingDeferral^ deferral = args->SuspendingOperation->GetDeferral();

    create_task([this, deferral]()
    {
        m_deviceResources->Trim();

        // Insert your code here.

        deferral->Complete();
    });
}

// Call this method when the app suspends. It provides a hint to the driver that the app 
// is entering an idle state and that temporary buffers can be reclaimed for use by other apps.
void DX::DeviceResources::Trim()
{
    ComPtr<IDXGIDevice3> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    dxgiDevice->Trim();
}
```

## Liberar los recursos exclusivos y los identificadores de archivos


Cuando la aplicación controla el evento [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860), también tiene la oportunidad de liberar recursos exclusivos e identificadores de archivos. Liberar recursos exclusivos e identificadores de archivos sirve para que otras aplicaciones puedan acceder a ellos cuando la aplicación no los está usando. Cuando la aplicación se active después de haberla finalizado, deberá abrir sus recursos exclusivos e identificadores de archivos.

## Observaciones


El sistema suspende la aplicación cuando el usuario cambia a otra aplicación o al escritorio. El sistema reanuda la aplicación cuando el usuario vuelve a cambiar a ella. Cuando el sistema reanuda la aplicación, el contenido de las variables y las estructuras de datos es el mismo que antes de que el sistema la suspendiera. El sistema restaura la aplicación en el punto exacto en el que estaba, para que parezca al usuario que se ejecutaba en segundo plano

El sistema intenta mantener la aplicación y sus datos en memoria mientras está suspendida. No obstante, si el sistema no tiene los recursos necesarios para mantener la aplicación en memoria, finalizará su ejecución. Cuando el usuario vuelve a una aplicación suspendida que se ha cerrado, el sistema envía un evento [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) y debe restaurar sus datos de aplicación en su controlador para el evento **CoreApplicationView::Activated**.

El sistema no notifica a una aplicación cuando se cierra, con lo cual la aplicación deberá guardar sus datos de aplicación y liberar los recursos exclusivos y los identificadores de archivos cuando se suspenda, y restaurarlos cuando vuelva a activarse.

## Temas relacionados

* [Cómo reanudar una aplicación (DirectX y C++)](how-to-resume-an-app-directx-and-cpp.md)
* [Cómo activar una aplicación (DirectX y C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 







<!--HONumber=Aug16_HO3-->


