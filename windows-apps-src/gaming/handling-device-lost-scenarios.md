---
title: Controlar escenarios cuando se quitan dispositivos en Direct3D 11
description: En este tema se explica cómo recrear la cadena de la interfaz de dispositivo de Direct3D y DXGI cuando se quita o reinicializa la tarjeta gráfica.
ms.assetid: 8f905acd-08f3-ff6f-85a5-aaa99acb389a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, DirectX 11, dispositivo perdido
ms.localizationpriority: medium
ms.openlocfilehash: 49356b910879f96d607a8bfeb2586b8da0ea3c69
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173159"
---
# <a name="span-iddev_gaminghandling_device-lost_scenariosspanhandle-device-removed-scenarios-in-direct3d-11"></a><span id="dev_gaming.handling_device-lost_scenarios"></span>Controlar escenarios cuando se quitan dispositivos en Direct3D 11



En este tema se explica cómo recrear la cadena de la interfaz de dispositivo de Direct3D y DXGI cuando se quita o reinicializa la tarjeta gráfica.

En DirectX9, las aplicaciones podrían encontrarse con la condición "[dispositivo perdido](/windows/desktop/direct3d9/lost-devices)" por la cual el dispositivo D3D adquiere un estado no operativo. Por ejemplo, cuando una aplicación de Direct3D 9 con pantalla completa pierde el foco, el dispositivo Direct3D pasa a tener la condición de "perdido"; todo intento de dibujar con un dispositivo perdido dará error en forma silenciosa. Direct3D 11 usa interfaces de dispositivo gráfico, permitiendo que varios programas compartan el mismo dispositivo gráfico físico y eliminando las condiciones donde las aplicaciones pierden el control del dispositivo de Direct3D. Sin embargo, aún es posible que la disponibilidad de la tarjeta gráfica cambie. Por ejemplo:

-   El controlador de gráficos se actualiza.
-   El sistema cambia de una tarjeta gráfica de ahorro de energía a una tarjeta gráfica de rendimiento.
-   El dispositivo gráfico deja de responder y se restablece.
-   Se conecta o se quita una tarjeta gráfica físicamente.

Cuando se dan estas circunstancias, DXGI devuelve un código de error que indica que el dispositivo de Direct3D debe reinicializarse y que los recursos de dispositivo deben recrearse. Este tutorial explica de qué manera las aplicaciones y juegos de Direct3D 11 pueden detectar cualquier circunstancia y responder ante esta cuando se restablece, quita o cambia la tarjeta gráfica. Los ejemplos de código proceden de la plantilla Aplicación DirectX 11 (Windows universal) que se incluye con Microsoft Visual Studio 2015.

## <a name="instructions"></a>Instructions

### <a name="spanspanstep-1"></a><span></span>Paso 1:

Agrega una comprobación para el error de dispositivo quitado en el bucle de representación. Presenta el marco con una llamada a [**IDXGISwapChain::Present**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) (o [**Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1), etc.). A continuación, compruebe si devolvió un [**dispositivo de error de dxgi \_ \_ \_ quitado**](/windows/desktop/direct3ddxgi/dxgi-error) o el restablecimiento del **dispositivo de error de dxgi \_ \_ \_ **.

Primero, la plantilla almacena el valor HRESULT que devuelve la cadena de intercambio de DXGI:

```cpp
HRESULT hr = m_swapChain->Present(1, 0);
```

Después de encargarse de todo el trabajo relacionado con la presentación del marco, la plantilla comprueba si hay error de dispositivo quitado. Si es necesario, llama a un método para controlar la condición de dispositivo quitado:

```cpp
// If the device was removed either by a disconnection or a driver upgrade, we
// must recreate all device resources.
if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    HandleDeviceLost();
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-2"></a>Paso 2:

Además, incluye una comprobación para el error de dispositivo quitado cuando responde a cambios en el tamaño de la ventana. Este es un buen lugar para comprobar si se ha [** \_ \_ \_ quitado el dispositivo de errores de dxgi**](/windows/desktop/direct3ddxgi/dxgi-error) o el ** \_ dispositivo de errores \_ \_ de dxgi** por varias razones:

-   Cuando se cambia el tamaño de la cadena de intercambio, es necesaria una llamada al adaptador DXGI subyacente, que puede devolver un error de dispositivo quitado.
-   La aplicación podría haberse movido a un monitor conectado a un dispositivo gráfico diferente.
-   Cuando un dispositivo gráfico se quita o se restablece, la resolución del escritorio suele cambiar y, por lo tanto, cambia el tamaño de la ventana.

La plantilla comprueba el valor HRESULT que devuelve [**ResizeBuffers**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers):

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    0
    );

if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    // If the device was removed for any reason, a new device and swap chain will need to be created.
    HandleDeviceLost();

    // Everything is set up now. Do not continue execution of this method. HandleDeviceLost will reenter this method 
    // and correctly set up the new device.
    return;
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-3"></a>Paso 3:

Cada vez que la aplicación recibe el error de dispositivo de error de DXGI, debe reinicializar el dispositivo Direct3D y volver a crear los recursos dependientes del dispositivo. [** \_ \_ \_ **](/windows/desktop/direct3ddxgi/dxgi-error) Libera todas las referencias a los recursos de dispositivos gráficos que se crearan con el dispositivo anterior de Direct3D (esos recursos ya no tienen validez) y libera todas las referencias a la cadena de intercambio para poder crear una nueva.

El método HandleDeviceLost libera la cadena de intercambio y notifica a los componentes de la aplicación que liberen recursos de dispositivo:

```cpp
m_swapChain = nullptr;

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that device resources need to be released.
    // This ensures all references to the existing swap chain are released so that a new one can be created.
    m_deviceNotify->OnDeviceLost();
}
```

Luego crea una nueva cadena de intercambio y reinicializa los recursos dependientes de dispositivo controlados por la clase de administración de dispositivos:

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();
```

Una vez que el dispositivo y la cadena de intercambio se hayan restablecido, notifica a los componentes de la aplicación que reinicialicen recursos dependientes de dispositivo:

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that resources can now be created again.
    m_deviceNotify->OnDeviceRestored();
}
```

Cuando el método HandleDeviceLost finaliza, el control vuelve al bucle de representación, que continúa dibujando el marco siguiente.

## <a name="remarks"></a>Observaciones


### <a name="investigating-the-cause-of-device-removed-errors"></a>Investigar la causa de los errores de dispositivo quitado

Los problemas repetitivos con errores de dispositivo quitado de DXGI pueden indicar que el código de gráficos está creando condiciones no válidas durante una rutina de dibujo. También puede indicar un error de hardware o un error en el controlador de gráficos. Para investigar la causa de los errores de dispositivo quitado, llama a [**ID3D11Device::GetDeviceRemovedReason**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-getdeviceremovedreason) antes de liberar el dispositivo de Direct3D. Este método devuelve uno de seis códigos de error de DXGI posibles, indicando la razón del error de dispositivo quitado:

-   **DXGI \_ ERROR de \_ dispositivo \_ bloqueado**: el controlador de gráficos dejó de responder debido a una combinación no válida de comandos de gráficos enviados por la aplicación. Si obtienes este error en repetidas ocasiones, puede indicar que la aplicación provocó que el dispositivo dejase de responder y necesita depurarse.
-   **DXGI \_ Dispositivo de ERROR \_ \_ quitado**: el dispositivo de gráficos se ha quitado físicamente, está apagado o se ha realizado una actualización del controlador. Esto sucede de manera ocasional y es normal; la aplicación o el juego debe recrear recursos de dispositivo como se describe en este tema.
-   **DXGI \_ ERROR \_ de \_ restablecimiento del dispositivo**: error en el dispositivo de gráficos debido a un comando con formato incorrecto. Si obtienes este error en repetidas ocasiones, puede significar que el código está enviando comandos de dibujo no válidos.
-   **DXGI \_ Error \_ \_ interno del \_ controlador de errores**: el controlador de gráficos ha detectado un error y ha restablecido el dispositivo.
-   **DXGI \_ ERROR \_ de \_ llamada no válida**: la aplicación proporcionó datos de parámetros no válidos. Si obtienes este error aunque sea una sola vez, significa que el código provocó la condición de dispositivo quitado y debe depurarse.
-   **S \_ OK**: se devuelve cuando se habilita, deshabilita o restablece un dispositivo de gráficos sin invalidar el dispositivo de gráficos actual. Por ejemplo, este código de error puede devolverse si una aplicación está usando [Windows Advanced Rasterization Platform (WARP)](/windows/desktop/direct3darticles/directx-warp) y un adaptador de hardware se vuelve disponible.

El código siguiente recuperará el código de error del dispositivo de error de DXGI y lo imprimirá en la consola de depuración. [** \_ \_ \_ **](/windows/desktop/direct3ddxgi/dxgi-error) Inserta este código al comienzo del método HandleDeviceLost.

```cpp
    HRESULT reason = m_d3dDevice->GetDeviceRemovedReason();

#if defined(_DEBUG)
    wchar_t outString[100];
    size_t size = 100;
    swprintf_s(outString, size, L"Device removed! DXGI_ERROR code: 0x%X\n", reason);
    OutputDebugStringW(outString);
#endif
```

Para obtener más información, consulte [**GetDeviceRemovedReason**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-getdeviceremovedreason) y [**DXGI \_ error**](/windows/desktop/direct3ddxgi/dxgi-error).

### <a name="testing-device-removed-handling"></a>Probar el control de dispositivo quitado

El símbolo del sistema para desarrolladores de Visual Studio admite una herramienta de línea de comandos, 'dxcap', para la captura y reproducción de eventos de Direct3D relacionados con el diagnóstico de gráficos de Visual Studio. Puede usar la opción de línea de comandos "-forcetdr" mientras se ejecuta la aplicación, lo que forzará un evento de detección y recuperación del tiempo de espera de la GPU, lo que desencadenará \_ el dispositivo de errores de DXGI \_ y le \_ permitirá probar el código de control de errores.

> **Nota:** DXCap y sus dll de soporte se instalan en system32/SysWow64 como parte de las herramientas de gráficos para Windows 10 que ya no se distribuyen a través del Windows SDK. En su lugar, se proporcionan a través de la característica de herramientas de gráficos a petición que es un componente opcional del sistema operativo, y que debe instalarse para poder habilitar y usar las herramientas de gráficos en Windows 10. Puedes obtener información sobre cómo instalar las herramientas de gráficos para Windows 10 aquí: <https://msdn.microsoft.com/library/mt125501.aspx#InstallGraphicsTools>