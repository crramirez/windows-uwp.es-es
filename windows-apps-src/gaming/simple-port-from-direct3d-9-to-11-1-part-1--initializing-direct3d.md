---
author: mtoepke
title: Inicializar Direct3D 11
description: Aprende a convertir el código de inicialización de Direct3D 9 a Direct3D 11, a obtener identificadores para el dispositivo Direct3D y el contexto de dispositivo, y a usar DXGI para configurar una cadena de intercambio.
ms.assetid: 1bd5e8b7-fd9d-065c-9ff3-1a9b1c90da29
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, juegos, direct3d 11, inicialización, migrar, direct3d 9
ms.localizationpriority: medium
ms.openlocfilehash: 5f6aa5bca3ecc242e90b42081a0111358afdfa9b
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2018
ms.locfileid: "5702583"
---
# <a name="initialize-direct3d-11"></a>Inicializar Direct3D11



**Resumen**

-   Parte 1: Inicializar Direct3D 11
-   [Parte 2: Convertir el marco de representación](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   [Parte 3: Migrar el bucle del juego](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


Aprende a convertir el código de inicialización de Direct3D 9 a Direct3D 11, a obtener identificadores para el dispositivo Direct3D y el contexto de dispositivo, y a usar DXGI para configurar una cadena de intercambio. Parte 1 del tutorial [Migrar una aplicación simple de Direct3D9 a DirectX11 y la Plataforma universal de Windows (UWP)](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

## <a name="initialize-the-direct3d-device"></a>Inicializar el dispositivo Direct3D


En Direct3D 9, creamos un identificador para el dispositivo Direct3D llamando a [**IDirect3D9::CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174313). Empezamos obteniendo un puntero a [**IDirect3D9 interface**](https://msdn.microsoft.com/library/windows/desktop/bb174300) y especificamos un número de parámetros para controlar la configuración del dispositivo Direct3D y la cadena de intercambio. Antes de hacer esto, llamamos a [**GetDeviceCaps**](https://msdn.microsoft.com/library/windows/desktop/dd144877) para asegurarnos de que no estábamos pidiéndole al dispositivo que hiciera algo que no podía.

Direct3D 9

```cpp
UINT32 AdapterOrdinal = 0;
D3DDEVTYPE DeviceType = D3DDEVTYPE_HAL;
D3DCAPS9 caps;
m_pD3D->GetDeviceCaps(AdapterOrdinal, DeviceType, &caps); // caps bits

D3DPRESENT_PARAMETERS params;
ZeroMemory(&params, sizeof(D3DPRESENT_PARAMETERS));

// Swap chain parameters:
params.hDeviceWindow = m_hWnd;
params.AutoDepthStencilFormat = D3DFMT_D24X8;
params.BackBufferFormat = D3DFMT_X8R8G8B8;
params.MultiSampleQuality = D3DMULTISAMPLE_NONE;
params.MultiSampleType = D3DMULTISAMPLE_NONE;
params.SwapEffect = D3DSWAPEFFECT_DISCARD;
params.Windowed = true;
params.PresentationInterval = 0;
params.BackBufferCount = 2;
params.BackBufferWidth = 0;
params.BackBufferHeight = 0;
params.EnableAutoDepthStencil = true;
params.Flags = 2;

m_pD3D->CreateDevice(
    0,
    D3DDEVTYPE_HAL,
    m_hWnd,
    64,
    &params,
    &m_pd3dDevice
    );
```

En Direct3D 11, el contexto del dispositivo y la infraestructura de gráficos se consideran de manera independiente del dispositivo en sí. La inicialización se divide en varios pasos.

Primero, creamos el dispositivo. Obtenemos una lista de los niveles de característica que el dispositivo admite. Esta lista informa la mayor parte de lo que necesitamos saber sobre la GPU. Además no es necesario que creemos una interfaz solo para acceder a Direct3D. En cambio, usamos la API principal de [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082). Obtenemos un identificador para el dispositivo y su contexto inmediato. El contexto del dispositivo se usa para establecer el estado de canalización y generar comandos de representación.

Después de crear el dispositivo Direct3D 11 y su contexto, podemos aprovechar la funcionalidad del puntero COM para obtener la versión más reciente de las interfaces, lo cual incluye capacidad adicional, siempre recomendable.

> **Nota**  D3D\_FEATURE\_LEVEL\_9\_1 (que corresponde al modelo de sombreador 2.0) es el nivel mínimo tu juego de Microsoft Store es necesarias para admitir. (Los paquetes ARM de tu juego no obtendrán la certificación si no son compatibles con el nivel 9\_1.) Si el juego también incluye una ruta de acceso de representación para las 3 funcionalidades del modelo de sombreador, debes incluir D3D\_FEATURE\_LEVEL\_9\_3 en la matriz

 

Direct3D 11

```cpp
// This flag adds support for surfaces with a different color channel 
// ordering than the API default. It is required for compatibility with
// Direct2D.
UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
// If the project is in a debug build, enable debugging via SDK Layers.
creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

// This example only uses feature level 9.1.
D3D_FEATURE_LEVEL featureLevels[] = 
{
    D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;
D3D11CreateDevice(
    nullptr, // Specify nullptr to use the default adapter.
    D3D_DRIVER_TYPE_HARDWARE,
    nullptr,
    creationFlags,
    featureLevels,
    ARRAYSIZE(featureLevels),
    D3D11_SDK_VERSION, // UWP apps must set this to D3D11_SDK_VERSION.
    &device, // Returns the Direct3D device created.
    nullptr,
    &context // Returns the device immediate context.
    );

// Store pointers to the Direct3D 11.2 API device and immediate context.
device.As(&m_d3dDevice);

context.As(&m_d3dContext);
```

## <a name="create-a-swap-chain"></a>Crear una cadena de intercambio


Direct3D 11 incluye una API de dispositivo denominada infraestructura de gráficos DirectX (DXGI). La interfaz DXGI nos permite, por ejemplo, controlar la configuración de la cadena de intercambio y establecer dispositivos compartidos. En este paso de la inicialización de Direct3D, vamos a usar DXGI para crear una cadena de intercambio. Dado que creamos el dispositivo, podemos seguir una cadena de interfaz de regreso al adaptador DXGI.

El dispositivo Direct3D implementa una interfaz COM para DXGI. Primero necesitamos obtener esa interfaz y usarla para solicitar el adaptador DXGI que hospeda el dispositivo. Luego usamos el adaptador DXGI para crear una fábrica de DXGI.

> **Nota**  son interfaces COM, tu primera respuesta podría ser usar [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521). En cambio, debes usar punteros inteligentes [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx). Luego, simplemente llama al método [**Como()**](https://msdn.microsoft.com/library/windows/apps/br230426.aspx), suministrando un puntero COM vacío del tipo de interfaz correcto.

 

**Direct3D 11**

```cpp
ComPtr<IDXGIDevice2> dxgiDevice;
m_d3dDevice.As(&dxgiDevice);

// Then, the adapter hosting the device;
ComPtr<IDXGIAdapter> dxgiAdapter;
dxgiDevice->GetAdapter(&dxgiAdapter);

// Then, the factory that created the adapter interface:
ComPtr<IDXGIFactory2> dxgiFactory;
dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2),
    &dxgiFactory
    );
```

Ahora que tenemos la fábrica de DXGI, podemos usarla para crear la cadena de intercambio. Definamos los parámetros de la cadena de intercambio. Necesitamos especificar el formato de la superficie. Elegiremos el formato [**DXGI\_FORMAT\_B8G8R8A8\_UNORM**](https://msdn.microsoft.com/library/windows/desktop/bb173059) porque es compatible con Direct2D. Deshabilitaremos el ajuste de escala de la pantalla, el muestreo múltiple y la representación en estéreo, porque no se usarán en este ejemplo. Dado que estamos ejecutando directamente en una clase CoreWindow, podemos dejar el ancho y el alto establecidos en 0 y obtener los valores de pantalla completa de manera automática.

> **Nota**  siempre establece el parámetro *SDKVersion* en D3D11\_SDK\_VERSION para aplicaciones para UWP.

 

**Direct3D 11**

```cpp
ComPtr<IDXGISwapChain1> swapChain;
dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr,
    &swapChain
    );
swapChain.As(&m_swapChain);
```

Para asegurarnos de no estar representando con una frecuencia superior a la que la pantalla puede realmente mostrar, establecemos la latencia del marco en 1 y usamos [**DXGI\_SWAP\_EFFECT\_FLIP\_SEQUENTIAL**](https://msdn.microsoft.com/library/windows/desktop/bb173077). Esto ahorra energía y es un requisito de certificación de almacenamiento. Obtendremos más detalles sobre la presentación en pantalla en la segunda parte de este tutorial.

> **Nota**  se puede usar multithreading (por ejemplo, elementos de trabajo del [**grupo de subprocesos**](https://msdn.microsoft.com/library/windows/apps/br229642) ) para continuar con otro trabajo mientras se bloquea el subproceso de representación.

 

**Direct3D 11**

```cpp
dxgiDevice->SetMaximumFrameLatency(1);
```

Ahora podemos configurar el búfer de reserva para la representación.

## <a name="configure-the-back-buffer-as-a-render-target"></a>Configurar el búfer de reserva como un destino de representación


Primero tenemos que obtener un identificador para el búfer de reserva. (Ten en cuenta que el búfer de reserva es propiedad de la cadena de intercambio DXGI, mientras que en DirectX 9 era propiedad del dispositivo Direct3D). Después, le indicamos al dispositivo que usara Direct3D como el destino de representación mediante la creación de un destino de representación *vista* con el búfer de reserva.

**Direct3D 11**

```cpp
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChain->GetBuffer(
    0,
    __uuidof(ID3D11Texture2D),
    &backBuffer
    );

// Create a render target view on the back buffer.
m_d3dDevice->CreateRenderTargetView(
    backBuffer.Get(),
    nullptr,
    &m_renderTargetView
    );
```

Ahora el contexto del dispositivo entra en juego. Le decimos a Direct3D que use la vista de destino de representación que creamos recién mediante la interfaz del contexto de dispositivo. Recuperaremos el ancho y el alto del búfer de reserva para poder destinar toda la ventana como ventanilla. Ten en cuenta que el búfer de reserva se conecta con la cadena de intercambio, de modo tal que si el tamaño de la ventana cambia (por ejemplo, el usuario arrastra la ventana del juego a otro monitor), será necesario cambiar el tamaño y algunos valores de configuración del búfer de reserva.

**Direct3D 11**

```cpp
D3D11_TEXTURE2D_DESC backBufferDesc = {0};
backBuffer->GetDesc(&backBufferDesc);

CD3D11_VIEWPORT viewport(
    0.0f,
    0.0f,
    static_cast<float>(backBufferDesc.Width),
    static_cast<float>(backBufferDesc.Height)
    );

m_d3dContext->RSSetViewports(1, &viewport);
```

Ahora que tenemos un identificador de dispositivo y un destino de representación de pantalla completa, estamos listos para cargar y dibujar geometría. Continúa con la [Parte 2: Representación](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md).

 

 




