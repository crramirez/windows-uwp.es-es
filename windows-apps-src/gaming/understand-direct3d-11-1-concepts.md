---
title: Cambios importantes en Direct3D 11 respecto de Direct3D 9
description: En este tema se explican las diferencias de alto nivel entre DirectX 9 y DirectX 11.
ms.assetid: 35a9e388-b25e-2aac-0534-577b15dae364
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, juegos, directx, direct3d 9, direct3d 11, cambios
ms.localizationpriority: medium
ms.openlocfilehash: ecdd8591efb3920d2cfe333aa8ec02c65c1a1465
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8328866"
---
# <a name="important-changes-from-direct3d-9-to-direct3d-11"></a>Cambios importantes en Direct3D 11 respecto de Direct3D 9



**Resumen**

-   [Planea tu migración de DirectX](plan-your-directx-port.md)
-   Cambios importantes en Direct3D 11 respecto a Direct3D 9
-   [Asignación de características](feature-mapping.md)


Este tema explica las diferencias de nivel alto entre DirectX 9 y DirectX 11.

Direct3D 11 es esencialmente el mismo tipo de API que Direct3D 9, una interfaz virtualizada de bajo nivel en hardware gráfico. Todavía te permite realizar operaciones de dibujo gráfico en una variedad de implementaciones de hardware. El diseño de la API gráfica cambió desde Direct3D 9. Se expandió el concepto de un contexto de dispositivo y se agregó una API específicamente para infraestructura gráfica. Los recursos almacenados en el dispositivo de Direct3D tienen un mecanismo nuevo para el polimorfismo de datos llamado vista de recursos.

## <a name="core-api-functions"></a>Funciones API clave


En Direct3D9 tenías que crear una interfaz con la API de Direct3D antes de poder empezar a usarla. En tu juego Direct3D11 de la Plataforma universal de Windows (UWP), llamas a una función estática llamada [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) para crear el dispositivo y el contexto de dispositivo.

## <a name="devices-and-device-context"></a>Dispositivo y contexto de dispositivo


Un dispositivo Direct3D 11 representa una tarjeta gráfica virtualizada. Se usa para crear recursos en memoria de vídeo, por ejemplo: cargar texturas a la GPU creando vistas sobre recursos de textura y cadenas de intercambio, y creando muestras de texturas. Para obtener una lista completa de para qué se usa una interfaz de dispositivo de Direct3D 11, consulta [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) y [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575).

Un contexto de dispositivo de Direct3D 11 se usa para establecer el estado de la canalización y generar comandos de representación. Por ejemplo, una cadena de representación de Direct3D 11 usa un contexto de dispositivo para configurar la cadena de representación y dibujar la escena (consulta a continuación). El contexto de dispositivo se usa para acceder (asignar) a la memoria de vídeo usada por los recursos de dispositivo de Direct3D. También se usa para actualizar los datos de los subrecursos, por ejemplo, los datos del búfer de constantes. Para obtener una lista completa de para qué se usa una interfaz de dispositivo de Direct3D 11, consulta [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) y [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598). Ten en cuenta que la mayoría de las muestras usan un contexto inmediato para representar directamente en el dispositivo, pero Direct3D 11 también admite contextos de dispositivo diferidos, que se usan principalmente para multithreading.

En Direct3D11, tanto el controlador de dispositivos como el controlador de contexto de dispositivos se obtienen llamando a [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082). Este método es también donde pides un conjunto específico de características de hardware y se recupera información sobre los niveles de característica de Direct3D admitidos por la tarjeta gráfica. Consulta el tema de [introducción a un dispositivo en Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476880) para obtener más información sobre los dispositivos, los contextos de dispositivo y los subprocesos.

## <a name="device-infrastructure-frame-buffers-and-render-target-views"></a>Infraestructura de dispositivo, búferes de cuadros y vistas de destino de representación


En Direct3D 11, la configuración de hardware y adaptador de dispositivo se establece con la API de la infraestructura de gráficos de DirectX (DXGI) mediante el uso de las interfaces COM [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) y [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/hh404543). Interfaces de DXGI específicas crean y configuran búferes y demás recursos de ventana (visible o fuera de pantalla). La implementación de factores de fábrica de [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556) adquiere recursos de DXGI como el búfer de cuadros. Dado que DXGI es dueño de la cadena de intercambio, se usa una interfaz de DXGI para presentar los cuadros en la pantalla. Consulta [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631).

Usa [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556) para crear una cadena de intercambio compatible con tu juego. Debes crear una cadena de intercambio para una ventana principal o para composición (interoperación de AML), en lugar de crear una cadena de intercambio para un HWND.

## <a name="device-resources-and-resource-views"></a>Recursos de dispositivo y vistas de recurso


Direct3D 11 admite un nivel adicional de polimorfismo en recursos de memoria de vídeo conocidos como vistas. En esencia, donde tenías un único objeto de Direct3D9 para textura, ahora tienes dos objetos: el recurso de textura, que contiene los datos, y la vista de recurso, que indica cómo se usa la vista para representación. Una vista basada en un recurso permite que este se use para un fin específico. Por ejemplo, se crea un recurso de textura 2D como [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635). Después se crea una vista de recurso de sombreador ([**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628)) en él para que pueda usarse como textura en un sombreador. También puede crearse una vista de destino de representación ([**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)) en el mismo recurso de textura 2D para que pueda usarse como superficie de dibujo. En otro ejemplo, los mismos datos de píxeles se representan en dos formatos de píxeles diferentes usando dos vistas separadas en un único recurso de textura.

Debe crearse el recurso subyacente con propiedades que son compatibles con el tipo de vistas que se crearán a partir de él. Por ejemplo, si se aplica una [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) a una superficie, esa superficie se crea con la marca [**D3D11_BIND_RENDER_TARGET**](https://msdn.microsoft.com/library/windows/desktop/ff476085). La superficie también tiene que tener un formato de superficie de DXGI compatible con la representación (consulta [**DXGI_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059)).

La mayoría de los recursos que usas para la representación se heredan desde la interfaz de [**ID3D11Resource**](https://msdn.microsoft.com/library/windows/desktop/ff476584), que hereda desde [**ID3D11DeviceChild**](https://msdn.microsoft.com/library/windows/desktop/ff476380). Los búferes de vértices, de índice y de constantes, y los sombreadores son todos recursos de Direct3D 11. Los estados de muestra y diseños de entrada se heredan directamente de [**ID3D11DeviceChild**](https://msdn.microsoft.com/library/windows/desktop/ff476380).

Las vistas de recurso usan un valor de enumeración DXGI\_FORMAT para indicar el formato de píxel. No todos los D3DFMT se admiten como DXGI\_FORMAT. Por ejemplo, no hay ningún formato RGB de 24 bpp en DXGI que equivalga a D3DFMT\_R8G8B8. Tampoco hay ningún equivalente de BGR para cada formato RGB (DXGI\_FORMAT\_R10G10B10A2\_UNORM equivale a D3DFMT\_A2B10G10R10, pero no existe un equivalente directo para D3DFMT\_A2R10G10B10). Debes planear convertir cualquier contenido en estos formatos heredados a los formatos admitidos en tiempo de compilación. Para obtener la lista completa de formatos de DXGI, consulta la enumeración de [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059).

Los recursos de dispositivo de Direct3D (y las vistas de recurso) se crean antes de que se represente la escena. Como se explica a continuación, se usan contextos de dispositivo para configurar la cadena de representación.

## <a name="device-context-and-the-rendering-chain"></a>Contexto de dispositivo y la cadena de representación


En Direct3D 9 y Direct3D 10.x, había un único objeto de dispositivo de Direct3D que administraba la creación de recursos, su estado y dibujo. En Direct3D 11, la interfaz del dispositivo de Direct3D sigue administrando la creación de los recursos, pero todas las operaciones de dibujo y estado se controlan mediante el uso de un contexto de dispositivo de Direct3D. He aquí un ejemplo de cómo el contexto de dispositivo (interfaz de [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)) se usa para configurar la cadena de representación:

-   Establecer y borrar las vistas de destino de representación (y vista de galería de símbolos de profundidad)
-   Establecer el búfer de vértices, el búfer de índice y un diseño de entrada para la fase de ensamblador de entrada (fase de IA)
-   Enlazar vértices y sombreadores de píxeles a la canalización
-   Enlazar búferes de constantes a los sombreadores
-   Enlazar vistas de textura y muestras al sombreador de píxeles
-   Dibujar la escena

Cuando se llama a uno de los métodos de [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407), se dibuja la escena en la vista de destino de representación. Cuando terminas con todos los dibujos, se usa el adaptador de DXGI para presentar el cuadro completado llamando a [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797).

## <a name="state-management"></a>Administración de estado


Direct3D 9 administraba la configuración de estado con un gran grupo de alternancias individuales configuradas con los métodos SetRenderState, SetSamplerState y SetTextureStageState. Dado que Direct3D 11 no admite la canalización de función fija heredada, SetTextureStageState se reemplaza escribiendo sombreadores de píxeles (PS). No existe un equivalente para un bloque de estado de Direct3D 9. En cambio, Direct3D 11 administra el estado a través del uso de 4 tipos de objetos de estado que proporcionan una forma más fluida de agrupar el estado de representación.

Por ejemplo, en lugar de usar SetRenderState con D3DRS\_ZENABLE, creas un objeto DepthStencilState con este parámetro de configuración de estado y otros relacionados, y los usas para cambiar el estado durante la representación.

Cuando migres/ aplicaciones de Direct3D 9 a objetos de estado, ten en cuenta que las diversas combinaciones de estados están representadas como objetos de estado invariables. Deben crearse una sola vez y volver a usarse mientras sean válidos.

## <a name="direct3d-feature-levels"></a>Niveles de característica de Direct3D


Direct3D tiene un nuevo mecanismo para determinar la compatibilidad de hardware llamado niveles de característica. Los niveles de característica simplifican la tarea de determinar qué puede hacer la tarjeta gráfica permitiéndote solicitar un conjunto bien definido de funcionalidades de GPU. Por ejemplo, el nivel de característica 9\_1 implementa la funcionalidad proporcionada por los adaptadores gráficos de Direct3D 9, incluso el modelo de sombreador 2.x. Dado que 9\_1 es el nivel de característica más bajo, puedes esperar que todos los dispositivos admitan un sombreador de vértices y un sombreador de píxeles, que eran las mismas fases admitidas por el modelo de sombreador programable de Direct3D 9.

Tu juego usará [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) para crear el contexto de dispositivo y el dispositivo de Direct3D. Cuando llamas a esta función, proporcionas una lista de niveles de características que tu juego puede admitir. Devolverá el nivel de característica admitido más alto desde esa lista. Por ejemplo, si tu juego puede usar las texturas BC4/BC5 (una característica de hardware de DirectX 10),deberías incluir al menos 9\_1 y 10\_0 en la lista de niveles de característica admitidos. Si el juego se ejecuta en hardware de DirectX9 y no pueden usarse las texturas BC4/BC5, [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) devolverá9\_1. Entonces tu juego puede retroceder a un formato de textura diferente (y texturas menores).

Si decides extender tu juego de Direct3D 9 para admitir niveles de característica de Direct3D más altos, es mejor terminar de migrar primero tu código de gráficos de Direct3D 9. Después de que tu juego esté funcionando en Direct3D 11, es más fácil agregar rutas de representación adicionales con gráficos mejorados.

Consulta [Niveles de característica de Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476876) para obtener una explicación detallada de la compatibilidad de niveles de característica. Consulta [Características de Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476342) y [Características de Direct3D 11.1](https://msdn.microsoft.com/library/windows/desktop/hh404562) para obtener una lista completa de características de Direct3D 11.

## <a name="feature-levels-and-the-programmable-pipeline"></a>Niveles de característica y la canalización programable


El hardware ha continuado evolucionando desde Direct3D9, y se han agregado varios niveles opcionales nuevos a la canalización de gráficos programable. El conjunto de opciones que tienes para la canalización de gráficos varía con el nivel de característica de Direct3D. El nivel de característica 10.0 incluye la fase de sombreador de geometría con salida de secuencia opcional para una representación en la GPU. El nivel de característica 11\_0 incluye el sombreador de casco y el sombreador de dominios para su uso con teselación de hardware. El nivel de característica 11\_0 incluye compatibilidad completa con sombreadores de DirectCompute, mientras que los niveles de característica 10.x solamente incluyen compatibilidad para una forma limitada de DirectCompute.

Todos los sombreadores se escriben en HLSL mediante el uso de un perfil de sombreador que corresponde a un nivel de característica de Direct3D. Los perfiles de sombreador son compatibles con versiones posteriores. Es por ello que un sombreador de HLSL que se compila usando vs\_4\_0\_level\_9\_1 o ps\_4\_0\_level\_9\_1 funcionará en todos los dispositivos. Los perfiles de sombreador no son compatibles con versiones anteriores. Es por ello que un sombreador compilado mediante el uso de vs_4\_1 solamente funcionará en dispositivos de los niveles de característica 10\_1, 11\_0, o 11\_1.

Direct3D 9 administraba las constantes para los sombreadores mediante el uso de una matriz compartida con SetVertexShaderConstant y SetPixelShaderConstant. Direct3D 11 usa búferes de constantes, que son recursos como un búfer de vértices o un búfer de índice. Los búferes de constante están diseñados para actualizarse de manera eficiente. En lugar de tener todas las constantes de sombreador organizadas en una única matriz global, organizas tus constantes en grupos lógicos y las administras a través de uno o más búferes de constantes. Cuando migras tu juego de Direct3D 9 a Direct3D 11, planea organizar tus búferes de constantes para que puedas actualizarlos apropiadamente. Por ejemplo, agrupa las constantes de sombreador que no se actualizan en cada cuadro en un búfer de constantes separado para que no tengas que cargar constantemente esos datos en la tarjeta gráfica junto con tus constantes de sombreador más dinámicas.

> **Nota**  aplicaciones más de Direct3D 9 usaban ampliamente de sombreadores, combinaba en ocasiones su uso del comportamiento de función fija heredado. Ten en cuenta que Direct3D 11 solamente usa un modelo de sombreado programable. Las características de función fija heredada de Direct3D 9 dejaron de utilizarse.

 

 

 




