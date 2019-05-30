---
title: Cambios importantes en Direct3D 11 respecto de Direct3D 9
description: En este tema se explican las diferencias de alto nivel entre DirectX 9 y DirectX 11.
ms.assetid: 35a9e388-b25e-2aac-0534-577b15dae364
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, juegos, directx, direct3d 9, direct3d 11, cambios
ms.localizationpriority: medium
ms.openlocfilehash: e3e3ecfaee8a99522623ee6b021d8e3a2d78ab85
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367543"
---
# <a name="important-changes-from-direct3d-9-to-direct3d-11"></a>Cambios importantes en Direct3D 11 respecto de Direct3D 9



**Resumen**

-   [Planear el puerto de DirectX](plan-your-directx-port.md)
-   Cambios importantes en Direct3D 11 respecto de Direct3D 9
-   [Asignación de función](feature-mapping.md)


En este tema se explican las diferencias de alto nivel entre DirectX 9 y DirectX 11.

Direct3D 11 es esencialmente el mismo tipo de API que Direct3D 9, una interfaz virtualizada de bajo nivel en hardware gráfico. Todavía te permite realizar operaciones de dibujo gráfico en una variedad de implementaciones de hardware. El diseño de la API gráfica cambió desde Direct3D 9. Se expandió el concepto de un contexto de dispositivo y se agregó una API específicamente para infraestructura gráfica. Los recursos almacenados en el dispositivo de Direct3D tienen un mecanismo nuevo para el polimorfismo de datos llamado vista de recursos.

## <a name="core-api-functions"></a>Funciones API clave


En Direct3D 9 tenías que crear una interfaz con la API de Direct3D antes de poder empezar a usarla. En tu juego Direct3D 11 de la Plataforma universal de Windows (UWP), llamas a una función estática llamada [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) para crear el dispositivo y el contexto de dispositivo.

## <a name="devices-and-device-context"></a>Dispositivo y contexto de dispositivo


Un dispositivo Direct3D 11 representa una tarjeta gráfica virtualizada. Se usa para crear recursos en memoria de vídeo, por ejemplo: cargar texturas a la GPU creando vistas sobre recursos de textura y cadenas de intercambio, y creando muestras de texturas. Para obtener una lista completa de para qué se usa una interfaz de dispositivo de Direct3D 11, consulta [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device) y [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1).

Un contexto de dispositivo de Direct3D 11 se usa para establecer el estado de la canalización y generar comandos de representación. Por ejemplo, una cadena de representación de Direct3D 11 usa un contexto de dispositivo para configurar la cadena de representación y dibujar la escena (consulta a continuación). El contexto de dispositivo se usa para acceder (asignar) a la memoria de vídeo usada por los recursos de dispositivo de Direct3D. También se usa para actualizar los datos de los subrecursos, por ejemplo, los datos del búfer de constantes. Para obtener una lista completa de para qué se usa una interfaz de dispositivo de Direct3D 11, consulta [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) y [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). Ten en cuenta que la mayoría de las muestras usan un contexto inmediato para representar directamente en el dispositivo, pero Direct3D 11 también admite contextos de dispositivo diferidos, que se usan principalmente para multithreading.

En Direct3D 11, tanto el controlador de dispositivos como el controlador de contexto de dispositivos se obtienen llamando a [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Este método es también donde pides un conjunto específico de características de hardware y se recupera información sobre los niveles de característica de Direct3D admitidos por la tarjeta gráfica. Consulta el tema de [introducción a un dispositivo en Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-intro) para obtener más información sobre los dispositivos, los contextos de dispositivo y los subprocesos.

## <a name="device-infrastructure-frame-buffers-and-render-target-views"></a>Infraestructura de dispositivo, búferes de cuadros y vistas de destino de representación


En Direct3D 11, la configuración de hardware y adaptador de dispositivo se establece con la API de la infraestructura de gráficos de DirectX (DXGI) mediante el uso de las interfaces COM [**IDXGIAdapter**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) y [**IDXGIDevice1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2). Interfaces de DXGI específicas crean y configuran búferes y demás recursos de ventana (visible o fuera de pantalla). La implementación de factores de fábrica de [**IDXGIFactory2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) adquiere recursos de DXGI como el búfer de cuadros. Dado que DXGI es dueño de la cadena de intercambio, se usa una interfaz de DXGI para presentar los cuadros en la pantalla. Consulta [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1).

Usa [**IDXGIFactory2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) para crear una cadena de intercambio compatible con tu juego. Debes crear una cadena de intercambio para una ventana principal o para composición (interoperación de AML), en lugar de crear una cadena de intercambio para un HWND.

## <a name="device-resources-and-resource-views"></a>Recursos de dispositivo y vistas de recurso


Direct3D 11 admite un nivel adicional de polimorfismo en recursos de memoria de vídeo conocidos como vistas. En esencia, donde tenías un único objeto de Direct3D 9 para textura, ahora tienes dos objetos: el recurso de textura, que contiene los datos, y la vista de recurso, que indica cómo se usa la vista para representación. Una vista basada en un recurso permite que este se use para un fin específico. Por ejemplo, se crea un recurso de textura 2D como [**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d). Después se crea una vista de recurso de sombreador ([**ID3D11ShaderResourceView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview)) en él para que pueda usarse como textura en un sombreador. También puede crearse una vista de destino de representación ([**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview)) en el mismo recurso de textura 2D para que pueda usarse como superficie de dibujo. En otro ejemplo, los mismos datos de píxeles se representan en dos formatos de píxeles diferentes usando dos vistas separadas en un único recurso de textura.

Debe crearse el recurso subyacente con propiedades que son compatibles con el tipo de vistas que se crearán a partir de él. Por ejemplo, si un [ **ID3D11RenderTargetView** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) se aplica a una superficie, que se muestran se crea con el [ **D3D11\_enlazar\_representación\_ DESTINO** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ne-d3d11-d3d11_bind_flag) marca. La superficie también debe tener un formato de superficie DXGI compatible con la representación (consulte [ **DXGI\_formato**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)).

La mayoría de los recursos que usas para la representación se heredan desde la interfaz de [**ID3D11Resource**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11resource), que hereda desde [**ID3D11DeviceChild**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicechild). Los búferes de vértices, de índice y de constantes, y los sombreadores son todos recursos de Direct3D 11. Los estados de muestra y diseños de entrada se heredan directamente de [**ID3D11DeviceChild**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicechild).

Las vistas de recursos usan una DXGI\_valor de enumeración de formato para indicar el formato de píxel. No todos D3DFMT se admite como un DXGI\_formato. Por ejemplo, no hay ningún formato de 24 bpp RGB en DXGI que es equivalente a D3DFMT\_R8G8B8. También no existen BGR equivalentes para cada formato RGB (DXGI\_formato\_R10G10B10A2\_UNORM es equivalente a D3DFMT\_A2B10G10R10, pero no hay ningún equivalente directo a D3DFMT\_A2R10G10B10). Debes planear convertir cualquier contenido en estos formatos heredados a los formatos admitidos en tiempo de compilación. Para ver da formato a una lista completa de DXGI el [ **DXGI\_formato** ](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) enumeración.

Los recursos de dispositivo de Direct3D (y las vistas de recurso) se crean antes de que se represente la escena. Como se explica a continuación, se usan contextos de dispositivo para configurar la cadena de representación.

## <a name="device-context-and-the-rendering-chain"></a>Contexto de dispositivo y la cadena de representación


En Direct3D 9 y Direct3D 10.x, había un único objeto de dispositivo de Direct3D que administraba la creación de recursos, su estado y dibujo. En Direct3D 11, la interfaz del dispositivo de Direct3D sigue administrando la creación de los recursos, pero todas las operaciones de dibujo y estado se controlan mediante el uso de un contexto de dispositivo de Direct3D. He aquí un ejemplo de cómo el contexto de dispositivo (interfaz de [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) se usa para configurar la cadena de representación:

-   Establecer y borrar las vistas de destino de representación (y vista de galería de símbolos de profundidad)
-   Establecer el búfer de vértices, el búfer de índice y un diseño de entrada para la fase de ensamblador de entrada (fase de IA)
-   Enlazar vértices y sombreadores de píxeles a la canalización
-   Enlazar búferes de constantes a los sombreadores
-   Enlazar vistas de textura y muestras al sombreador de píxeles
-   Dibujar la escena

Cuando se llama a uno de los métodos de [**ID3D11DeviceContext::Draw**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw), se dibuja la escena en la vista de destino de representación. Cuando terminas con todos los dibujos, se usa el adaptador de DXGI para presentar el cuadro completado llamando a [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1).

## <a name="state-management"></a>Administración de estado


Direct3D 9 administraba la configuración de estado con un gran grupo de alternancias individuales configuradas con los métodos SetRenderState, SetSamplerState y SetTextureStageState. Dado que Direct3D 11 no admite la canalización de función fija heredada, SetTextureStageState se reemplaza escribiendo sombreadores de píxeles (PS). No existe un equivalente para un bloque de estado de Direct3D 9. En cambio, Direct3D 11 administra el estado a través del uso de 4 tipos de objetos de estado que proporcionan una forma más fluida de agrupar el estado de representación.

Por ejemplo, en lugar de usar SetRenderState con D3DRS\_ZENABLE, cree un objeto DepthStencilState con este y otros relacionados con la configuración de estado y usarlo para cambiar el estado durante la representación.

Cuando migres/ aplicaciones de Direct3D 9 a objetos de estado, ten en cuenta que las diversas combinaciones de estados están representadas como objetos de estado invariables. Deben crearse una sola vez y volver a usarse mientras sean válidos.

## <a name="direct3d-feature-levels"></a>Niveles de característica de Direct3D


Direct3D tiene un nuevo mecanismo para determinar la compatibilidad de hardware llamado niveles de característica. Los niveles de característica simplifican la tarea de determinar qué puede hacer la tarjeta gráfica permitiéndote solicitar un conjunto bien definido de funcionalidades de GPU. Por ejemplo, el 9\_implementa nivel 1 característica la funcionalidad proporcionada por los adaptadores de gráficos de Direct3D 9, incluido el sombreador 2.x del modelo. Desde 9\_1 es el nivel más bajo de la característica, puede esperar todos los dispositivos para admitir un sombreador de vértices y un sombreador de píxeles, que son las fases mismas admitidas por el modelo de sombreador programables de Direct3D 9.

Tu juego usará [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) para crear el contexto de dispositivo y el dispositivo de Direct3D. Cuando llamas a esta función, proporcionas una lista de niveles de características que tu juego puede admitir. Devolverá el nivel de característica admitido más alto desde esa lista. Por ejemplo, si su juego puede usar las texturas BC4/BC5 texturas (una característica de hardware DirectX 10), también debe incluir al menos 9\_1 y 10\_0 en la lista de niveles de características compatibles. Si el juego se ejecuta en hardware de DirectX 9 y no se puede utilizar las texturas BC4/BC5 texturas, a continuación, [ **D3D11CreateDevice** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) devolverá 9\_1. Entonces tu juego puede retroceder a un formato de textura diferente (y texturas menores).

Si decides extender tu juego de Direct3D 9 para admitir niveles de característica de Direct3D más altos, es mejor terminar de migrar primero tu código de gráficos de Direct3D 9. Después de que tu juego esté funcionando en Direct3D 11, es más fácil agregar rutas de representación adicionales con gráficos mejorados.

Consulta [Niveles de característica de Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro) para obtener una explicación detallada de la compatibilidad de niveles de característica. Consulta [Características de Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-features) y [Características de Direct3D 11.1](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-1-features) para obtener una lista completa de características de Direct3D 11.

## <a name="feature-levels-and-the-programmable-pipeline"></a>Niveles de característica y la canalización programable


El hardware ha continuado evolucionando desde Direct3D 9, y se han agregado varios niveles opcionales nuevos a la canalización de gráficos programable. El conjunto de opciones que tienes para la canalización de gráficos varía con el nivel de característica de Direct3D. El nivel de característica 10.0 incluye la fase de sombreador de geometría con salida de secuencia opcional para una representación en la GPU. 11 de nivel de características\_0 incluye el sombreador de casco y el sombreador de dominios para su uso con la teselación de hardware. 11 de nivel de características\_0 también incluye compatibilidad total con los sombreadores de DirectCompute, mientras que los niveles de características 10.x sólo incluyen compatibilidad con una forma limitada de DirectCompute.

Todos los sombreadores se escriben en HLSL mediante el uso de un perfil de sombreador que corresponde a un nivel de característica de Direct3D. Perfiles de sombreador son compatibles hacia arriba, por lo que un sombreador HLSL que se compila utilizando vs\_4\_0\_nivel\_9\_1 o ps\_4\_0\_nivel\_9\_1 funcionará en todos los dispositivos. Perfiles de sombreador no son compatible, de nivel inferior para un sombreador compilado con vs\_4\_1 solo funcionará en el nivel de característica 10\_1, 11\_0 u 11\_1 dispositivos.

Direct3D 9 administraba las constantes para los sombreadores mediante el uso de una matriz compartida con SetVertexShaderConstant y SetPixelShaderConstant. Direct3D 11 usa búferes de constantes, que son recursos como un búfer de vértices o un búfer de índice. Los búferes de constante están diseñados para actualizarse de manera eficiente. En lugar de tener todas las constantes de sombreador organizadas en una única matriz global, organizas tus constantes en grupos lógicos y las administras a través de uno o más búferes de constantes. Cuando migras tu juego de Direct3D 9 a Direct3D 11, planea organizar tus búferes de constantes para que puedas actualizarlos apropiadamente. Por ejemplo, agrupa las constantes de sombreador que no se actualizan en cada cuadro en un búfer de constantes separado para que no tengas que cargar constantemente esos datos en la tarjeta gráfica junto con tus constantes de sombreador más dinámicas.

> **Tenga en cuenta**    más de Direct3D 9 aplicaciones realizan un uso extensivo de sombreadores, pero ocasionalmente mixto en el uso del comportamiento heredado de la función fija. Ten en cuenta que Direct3D 11 solamente usa un modelo de sombreado programable. Las características de función fija heredada de Direct3D 9 dejaron de utilizarse.

 

 

 




