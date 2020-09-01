---
title: Cómo suspender una aplicación (DirectX y C++)
description: En este tema verás cómo guardar datos importantes de la aplicación y del estado del sistema cuando el sistema suspende la aplicación DirectX para la Plataforma universal de Windows (UWP).
ms.assetid: 5dd435e5-ec7e-9445-fed4-9c0d872a239e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, suspensión, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 28c93228f149b25ba129b632d23f9798712ee80d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163119"
---
# <a name="how-to-suspend-an-app-directx-and-c"></a>Cómo suspender una aplicación (DirectX y C++)



En este tema verás cómo guardar datos importantes de la aplicación y del estado del sistema cuando el sistema suspende la aplicación DirectX para la Plataforma universal de Windows (UWP).

## <a name="register-the-suspending-event-handler"></a>Registrar el controlador de eventos de suspensión


Primero, realiza un registro para administrar el evento [**CoreApplication::Suspending**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending), que se desencadena cuando la aplicación cambia a un estado de suspensión debido a la acción del usuario o del sistema.

Agrega este código a la implementación del método [**IFrameworkView::Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) del proveedor de vistas:

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

  //...
}
```

## <a name="save-any-app-data-before-suspending"></a>Guarda los datos de la aplicación antes de la suspensión


Cuando la aplicación controla el evento [**CoreApplication::Suspending**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending), tiene la oportunidad de guardar sus datos de aplicación importantes en la función de controlador. La aplicación debe usar la API de almacenamiento [**LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings) para guardar los datos de aplicación simples de manera sincrónica. Si vas a desarrollar un juego, guarda toda la información de estado sobre el juego que sea importante. No olvides suspender el procesamiento de audio.

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

Esta devolución de llamada debe completarse en 5 segundos. Durante esta devolución de llamada, debes solicitar un aplazamiento mediante una llamada a [**SuspendingOperation::GetDeferral**](/uwp/api/windows.applicationmodel.suspendingoperation.getdeferral), que inicia la cuenta regresiva. Cuando la aplicación complete la operación de guardar, llama a [**SuspendingDeferral::Complete**](/uwp/api/windows.applicationmodel.suspendingdeferral.complete) para indicar al sistema que la aplicación está lista para suspenderse. Si no solicitas un aplazamiento, o si la aplicación tarda más de 5 segundos en guardar los datos, la aplicación se suspenderá automáticamente.

Esta devolución de llamada se produce como un mensaje de evento procesado por [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) para el elemento [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) de la aplicación. Esta devolución de llamada no se invocará si no llamas a [**CoreDispatcher::ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) desde el bucle principal de la aplicación (implementado en el método [**IFrameworkView::Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) del proveedor de vistas).

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

## <a name="call-trim"></a>Llamar a Trim()


A partir de Windows 8.1, todas las aplicaciones UWP de DirectX deben llamar a [**IDXGIDevice3:: Trim**](/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgidevice3-trim) al suspender. Esta llamada la indica al controlador gráfico que libere todos los búferes temporales asignados para la aplicación, lo que reduce la posibilidad de que se cierre la aplicación para reclamar recursos de memoria mientras esté en estado de suspensión. Este es un requisito de certificación para Windows 8.1.

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

## <a name="release-any-exclusive-resources-and-file-handles"></a>Liberar los recursos exclusivos y los identificadores de archivos


Cuando la aplicación controla el evento [**CoreApplication::Suspending**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending), también tiene la oportunidad de liberar recursos exclusivos e identificadores de archivos. Liberar recursos exclusivos e identificadores de archivos sirve para que otras aplicaciones puedan acceder a ellos cuando la aplicación no los está usando. Cuando la aplicación se active después de haberla finalizado, deberá abrir sus recursos exclusivos e identificadores de archivos.

## <a name="remarks"></a>Observaciones


El sistema suspende la aplicación cuando el usuario cambia a otra aplicación o al escritorio. El sistema reanuda la aplicación cuando el usuario vuelve a cambiar a ella. Cuando el sistema reanuda la aplicación, el contenido de las variables y las estructuras de datos es el mismo que antes de que el sistema la suspendiera. El sistema restaura la aplicación en el punto exacto en el que estaba, para que parezca al usuario que se ejecutaba en segundo plano

El sistema intenta mantener la aplicación y sus datos en la memoria mientras está suspendida. No obstante, si el sistema no tiene los recursos necesarios para mantener la aplicación en memoria, finalizará su ejecución. Cuando el usuario vuelve a una aplicación suspendida que se ha cerrado, el sistema envía un evento [**Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) y debe restaurar sus datos de aplicación en su controlador para el evento **CoreApplicationView::Activated**.

El sistema no notifica a una aplicación cuando se cierra, con lo cual la aplicación deberá guardar sus datos de aplicación y liberar los recursos exclusivos y los identificadores de archivos cuando se suspenda y restaurarlos cuando vuelva a activarse.

## <a name="related-topics"></a>Temas relacionados

* [Cómo reanudar una aplicación (DirectX y C++)](how-to-resume-an-app-directx-and-cpp.md)
* [Cómo activar una aplicación (DirectX y C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 