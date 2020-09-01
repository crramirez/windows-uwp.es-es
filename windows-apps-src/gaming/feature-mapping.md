---
title: Asignar características de DirectX 9 a las API de DirectX 11
description: Aprende a trasladar las características de tu juego Direct3D 9 a Direct3D 11 y a la Plataforma universal de Windows (UWP).
ms.assetid: 3aa8a114-4e47-ae0a-9447-88ba324377b8
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, DirectX 9, DirectX 11, portabilidad
ms.localizationpriority: medium
ms.openlocfilehash: 8f7bdc8cef43ffa323cae89459ac9bcb549c10f1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172049"
---
# <a name="map-directx-9-features-to-directx-11-apis"></a>Asignar características de DirectX 9 a las API de DirectX 11

Aprende a trasladar las características de tu juego Direct3D 9 a Direct3D 11 y a la Plataforma universal de Windows (UWP).

Consulte también [planeación del puerto DirectX](plan-your-directx-port.md)y [cambios importantes de Direct3D 9 a Direct3D 11](understand-direct3d-11-1-concepts.md).

## <a name="mapping-direct3d-9-to-directx-11-apis"></a>Asignar Direct3D 9 a las API de DirectX 11

[Direct3D](/windows/desktop/direct3d) sigue siendo la base de los elementos gráficos DirectX, pero la API cambió a partir de DirectX 9:

-   La Infraestructura de gráficos de Microsoft DirectX (DXGI) se usa para configurar adaptadores de gráficos. Usa [DXGI](/windows/desktop/direct3ddxgi/dx-graphics-dxgi) para seleccionar formatos de búfer, crear cadenas de intercambio, presentar marcos y crear recursos compartidos. Consulta [Introducción a DXGI](/windows/desktop/direct3ddxgi/d3d10-graphics-programming-guide-dxgi).
-   El contexto de dispositivo de Direct3D se usa para establecer el estado de canalización y generar comandos de representación. La mayoría de nuestras muestras usan un contexto inmediato para representar directamente en el dispositivo. Direct3D 11 también admite la representación multithreading, en cuyo caso se usan contextos diferidos. Consulta [Introducción a un dispositivo en Direct3D 11](/windows/desktop/direct3d11/overviews-direct3d-11-devices-intro).
-   Algunas características ya no se usan, particularmente la canalización de función fija. Consulta [Características desusadas](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated).

Para obtener una lista completa de las características de  Direct3D 11, consulta [Características de Direct3D 11](/windows/desktop/direct3d11/direct3d-11-features) y [Características de Direct3D 11](/windows/desktop/direct3d11/direct3d-11-1-features).

## <a name="moving-from-direct2d-9-to-direct2d-11"></a>Pasar de Direct2D 9 a Direct2D 11

[Direct2D (Windows)](/windows/desktop/Direct2D/direct2d-portal) sigue siendo parte importante de los gráficos DirectX y Windows. Todavía puedes usar Direct2D para dibujar juegos 2D y dibujar superposiciones (HUD) sobre Direct3D.

Direct2D se ejecuta encima de Direct3D. Los juegos 2D pueden implementarse en cualquiera de sus API. Por ejemplo, un juego 2D implementado con Direct3D puede usar la proyección ortográfica, establecer valores Z para controlar el orden de dibujo de primitivos y usar sombreadores de píxeles para agregar efectos especiales.

Dado que Direct2D se basa en Direct3D, también usa DXGI y contextos de dispositivo. Consulta [Introducción a la API de Direct2D](/windows/desktop/Direct2D/the-direct2d-api).

La API [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal) agrega compatibilidad para texto con formato otorgado mediante Direct2D. Consulta [Introducción a DirectWrite](/windows/desktop/DirectWrite/introducing-directwrite).

## <a name="replace-deprecated-helper-libraries"></a>Reemplazar bibliotecas de aplicaciones auxiliares desusadas

D3DX y DXUT están en desuso y no pueden usarse para juegos de UWP. Estas bibliotecas auxiliares proporcionaban recursos para tareas, como la carga de texturas y mallas.

-   El tutorial [Migración simple de Direct3D 9 a UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md) te muestra cómo configurar una ventana, inicializar Direct3D y hacer una representación básica en 3D.
-   El tutorial [Crear un juego para UWP sencillo con DirectX](tutorial--create-your-first-uwp-directx-game.md) te muestra tareas comunes de programación de juegos, entre ellas los elementos gráficos, la carga de archivos, la interfaz de usuario, los controles y el sonido.
-   El proyecto [DirectX Tool Kit (Kit de herramientas de DirectX)](https://github.com/Microsoft/DirectXTK) de la comunidad, te ofrece clases auxiliares que puedes usar con aplicaciones de Direct3D 11 y UWP.

## <a name="move-shader-programs-from-fx-to-hlsl"></a>Trasladar programas sombreadores de FX a HLSL

La biblioteca de utilidades de D3DX (D3DX 9, D3DX 10 y D3DX 11), incluidos los efectos, está en desuso para UWP. Todos los juegos de DirectX para UWP controlan la canalización de gráficos con [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl) sin efectos.

Visual Studio todavía usa FXC como opción avanzada para compilar objetos de sombreador. Los sombreadores de los juegos de la UWP se compilan con anticipación. El código de bytes se carga en tiempo de ejecución y, a continuación, cada recurso de sombreador se enlaza a la canalización de elementos gráficos durante el pase de representación apropiado. Los sombreadores deben moverse a sus propios archivos .HLSL independientes y las técnicas de representación deben implementarse en el código C++.

Para ver rápidamente la carga de recursos de sombreador, consulta [Migración simple de Direct3D 9 a UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

Direct3D 11 incorporó el modelo de sombreador 5, que requiere el nivel de característica 11 0 de Direct3D \_ (o superior). Consulta [HLSL Shader Model 5 Features for Direct3D 11 (Características del modelo de sombreador 5 de HLSL para Direct3D 11)](/windows/desktop/direct3dhlsl/overviews-direct3d-11-hlsl).

## <a name="replace-xnamath-and-d3dxmath"></a>Reemplazar XNAMath y D3DXMath

El código que usa XNAMath (o D3DXMath) debe migrarse a [DirectXMath](/windows/desktop/dxmath/directxmath-portal). DirectXMath incluye tipos portátiles entre x86, x64 y ARM. Consulta [Migración de código de la biblioteca de matemáticas XNA](/windows/desktop/dxmath/pg-xnamath-migration).

Ten en cuenta que con los sombreadores conviene usar tipos flotantes de DirectXMath. Por ejemplo las estructuras [**XMFLOAT4**](/windows/desktop/api/directxmath/ns-directxmath-xmfloat4) y [**XMFLOAT4X4**](/windows/desktop/api/directxmath/ns-directxmath-xmfloat4x4) alinean los datos de manera conveniente para los búferes de constantes.

## <a name="replace-directsound-with-xaudio2-and-background-audio"></a>Reemplazar DirectSound con XAudio2 (y audio de fondo)

DirectSound no es compatible con la UWP:

-   Usa [XAudio2](/windows/desktop/xaudio2/xaudio2-apis-portal) para agregar efectos de sonido a tu juego.

##  <a name="replace-directinput-with-xinput-and-windows-runtime-apis"></a>Reemplazar DirectInput con las API de XInput y Windows Runtime

DirectInput no es compatible con la UWP:

-   Usa la clase para las devoluciones de llamada de eventos de entrada [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) para la entrada táctil, de mouse y de teclado.
-   Usa [XInput](/windows/desktop/xinput/getting-started-with-xinput) 1.4 para la compatibilidad con el dispositivo de juego y los auriculares con micrófono. Si estás usando una base de código compartido para escritorio y UWP, consulta [XInput Versions (Versiones de XInput)](/windows/desktop/xinput/xinput-versions) para obtener información sobre la compatibilidad con versiones anteriores.
-   Regístrate para usar los eventos de la clase [**EdgeGesture**](/uwp/api/Windows.UI.Input.EdgeGesture) si tu juego necesita usar la barra de aplicaciones.

## <a name="use-microsoft-media-foundation-instead-of-directshow"></a>Usar Microsoft Media Foundation en lugar de DirectShow

DirectShow ya no forma parte de la API de DirectX ni de la API de Windows. [Microsoft Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk) proporciona contenido de vídeo para Direct3D mediante superficies compartidas. Consulta [Las API de vídeo en Direct3D 11](/windows/desktop/medfound/direct3d-11-video-apis).

## <a name="replace-directplay-with-networking-code"></a>Reemplazar DirectPlay con código de red

Microsoft DirectPlay está en desuso. Si tu juego usa servicios de red, debes proporcionar un código de red que cumpla con los requisitos de UWP. Usa estas API:

-   [Win32 y COM para aplicaciones UWP (funciones de red) (Windows)](/uwp/win32-and-com/win32-and-com-for-uwp-apps)
-   [**Espacio de nombres Windows.Networking (Windows)**](/uwp/api/Windows.Networking)
-   [**Espacio de nombres Windows.Networking.Sockets (Windows)**](/uwp/api/Windows.Networking.Sockets)
-   [**Espacio de nombres Windows.Networking.Connectivity (Windows)**](/uwp/api/Windows.Networking.Connectivity)
-   [**Espacio de nombres Windows.ApplicationModel.Background (Windows)**](/uwp/api/Windows.ApplicationModel.Background)

Estos artículos te ayudarán a agregar características de red y a declarar la compatibilidad de las redes en el manifiesto del paquete de la aplicación.

-   [Conexión con sockets (aplicaciones para UWP con C#/VB/C + + y XAML) (Windows)](/previous-versions/windows/apps/hh452976(v=win.10))
-   [Conexión con WebSockets (aplicaciones para UWP con C#/VB/C + + y XAML) (Windows)](/previous-versions/windows/apps/hh994396(v=win.10))
-   [Conexión a servicios web (aplicaciones para UWP con C#/VB/C + + y XAML) (Windows)](/previous-versions/windows/apps/hh761504(v=win.10))
-   [Conceptos básicos de redes](../networking/networking-basics.md)

Ten en cuenta que todas las aplicaciones para UWP (incluidos los juegos) usan tipos específicos de tareas en segundo plano para mantener la conectividad mientras la aplicación está suspendida. Si tu juego necesita mantener el estado de conexión durante la suspensión, consulta [Conceptos básicos de redes](../networking/networking-basics.md).

## <a name="function-mapping"></a>Asignación de funciones

Usa esta tabla cuando tengas que convertir código de Direct3D 9 a Direct3D 11. También te puede resultar útil para distinguir entre el dispositivo y el contexto del dispositivo.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Direct3D9</th>
<th align="left">Equivalente en Direct3D 11</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3ddevice9">IDirect3DDevice9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2">ID3D11Device2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11devicecontext2">ID3D11DeviceContext2</a></p>
<p>Las fases de la canalización de gráficos se describen en <a href="https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline">Canalización de gráficos</a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3d9">IDirect3D9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2">IDXGIFactory2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiadapter2">IDXGIAdapter2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nn-dxgi1_3-idxgidevice3">IDXGIDevice3</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-present">IDirect3DDevice9: reenviar:P</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::Present1</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-testcooperativelevel">IDirect3DDevice9::TestCooperativeLevel</a></p></td>
<td align="left"><p>Llame a <a href="/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::P resent1</a> con la marca de DXGI_PRESENT_TEST establecida.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dbasetexture9">IDirect3DBaseTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dtexture9">IDirect3DTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dcubetexture9">IDirect3DCubeTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvolumetexture9">IDirect3DVolumeTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dindexbuffer9">IDirect3DIndexBuffer9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexbuffer9">IDirect3DVertexBuffer9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer">ID3D11Buffer</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture1d">ID3D11Texture1D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d">ID3D11Texture2D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture3d">ID3D11Texture3D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview">ID3D11ShaderResourceView</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview">ID3D11RenderTargetView</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview">ID3D11DepthStencilView</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexshader9">IDirect3DVertexShader9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dpixelshader9">IDirect3DPixelShader9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader">ID3D11VertexShader</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11pixelshader">ID3D11PixelShader</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexdeclaration9">IDirect3DVertexDeclaration9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout">ID3D11InputLayout</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setrenderstate">IDirect3DDevice9::SetRenderState</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setsamplerstate">IDirect3DDevice9::SetSamplerState</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11blendstate1">ID3D11BlendState1</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilstate">ID3D11DepthStencilState</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11rasterizerstate1">ID3D11RasterizerState1</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11samplerstate">ID3D11SamplerState</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawindexedprimitive">IDirect3DDevice9::D rawIndexedPrimitive</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawprimitive">IDirect3DDevice9::D rawPrimitive</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw">ID3D11DeviceContext::D RAW</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed">ID3D11DeviceContext::D rawIndexed</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawindexedinstanced">ID3D11DeviceContext::D rawIndexedInstanced</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawinstanced">ID3D11DeviceContext::D rawInstanced</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetprimitivetopology">ID3D11DeviceContext:: IASetPrimitiveTopology</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawauto">ID3D11DeviceContext::D rawAuto</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-beginscene">IDirect3DDevice9::BeginScene</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-endscene">IDirect3DDevice9::EndScene</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawprimitiveup">IDirect3DDevice9::D rawPrimitiveUP</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawindexedprimitiveup">IDirect3DDevice9::D rawIndexedPrimitiveUP</a></p></td>
<td align="left"><p>No hay equivalente directo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-showcursor">IDirect3DDevice9::ShowCursor</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setcursorposition">IDirect3DDevice9::SetCursorPosition</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setcursorproperties">IDirect3DDevice9::SetCursorProperties</a></p></td>
<td align="left"><p>Usa API de cursor estándar.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-reset">IDirect3DDevice9:: RESET</a></p></td>
<td align="left"><p>El dispositivo LOST y POOL_MANAGED ya no existen. <a href="/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::P resent1</a> puede producir un error con un valor devuelto <a href="https://docs.microsoft.com/windows/desktop/direct3ddxgi/dxgi-error">DXGI_ERROR_DEVICE_REMOVED</a> .</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawrectpatch">IDirect3DDevice9:DrawRectPatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawtripatch">IDirect3DDevice9:DrawTriPatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-lightenable">IDirect3DDevice9:LightEnable</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-multiplytransform">IDirect3DDevice9:MultiplyTransform</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setlight">IDirect3DDevice9:SetLight</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-setmaterial">IDirect3DDevice9:SetMaterial</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-setnpatchmode">IDirect3DDevice9:SetNPatchMode</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-settransform">IDirect3DDevice9:SetTransform</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setfvf">IDirect3DDevice9:SetFVF</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-settexturestagestate">IDirect3DDevice9:SetTextureStageState</a></p></td>
<td align="left"><p>La canalización de función fija está en desuso.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-checkdepthstencilmatch">IDirect3DDevice9:CheckDepthStencilMatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-checkdeviceformat">IDirect3DDevice9:CheckDeviceFormat</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-getdevicecaps">IDirect3DDevice9:GetDeviceCaps</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-validatedevice">IDirect3DDevice9:ValidateDevice</a></p></td>
<td align="left"><p>Los bits de capacidad se sustituyen por los niveles de características. Solo unos pocos casos de uso de características y formato son opcionales para un nivel de características determinado. Se pueden comprobar con <a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport">ID3D11Device:: CheckFeatureSupport</a> y <a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-checkformatsupport">ID3D11Device:: CheckFormatSupport</a>.</p></td>
</tr>
</tbody>
</table>

## <a name="surface-format-mapping"></a>Asignación de formatos de superficie

Usa esta tabla para convertir formatos de Direct3D 9 a formatos de DXGI.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Formato de Direct3D 9</th>
<th align="left">Formato de Direct3D 11</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>D3DFMT_UNKNOWN</p></td>
<td align="left"><p>DXGI_FORMAT_UNKNOWN</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8B8</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8A8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8A8_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8X8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8X8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R5G6B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G6R5_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X1R5G5B5</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A1R5G5B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G5R5A1_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A4R4G4B4</p></td>
<td align="left"><p>DXGI_FORMAT_B4G4R4A4_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R3G3B2</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8</p></td>
<td align="left"><p>DXGI_FORMAT_A8_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R3G3B2</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4R4G4B4</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2B10G10R10</p></td>
<td align="left"><p>DXGI_FORMAT_R10G10B10A2</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8B8G8R8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p>
<p>DXGI_FORMAT_R8G8B8A8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8B8G8R8</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2R10G10B10</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8P8</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_P8</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8_UNORM</p>
<div class="alert">
<strong>Nota:</strong>    Use. r swizzle en el sombreador para duplicar de rojo en otros componentes para obtener el comportamiento de Direct3D 9.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_UNORM</p>
<div class="alert">
<strong>Nota:</strong>    Use swizzle. rrrg en el sombreador para duplicar rojo y desplace el color verde a los componentes alfa para obtener el comportamiento de Direct3D 9.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A4L4</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L6V5U5</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8L8V8U8</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_Q8W8V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_W11V11U10</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A2W10V10U10</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_UYVY</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8_B8G8</p></td>
<td align="left"><p>DXGI_FORMAT_G8R8_G8B8_UNORM</p>
<div class="alert">
<strong>Nota:</strong>    En Direct3D 9, los datos se escalaron verticalmente 255.0 f, pero esto se puede controlar en el sombreador.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_YUY2</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G8R8_G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_B8G8_UNORM</p>
<div class="alert">
<strong>Nota:</strong>    En Direct3D 9, los datos se escalaron verticalmente 255.0 f, pero esto se puede controlar en el sombreador.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT1</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM & DXGI_FORMAT_BC1_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT2</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM & DXGI_FORMAT_BC1_UNORM_SRGB</p>
<div class="alert">
<strong>Nota:</strong>    DXT1 y DXT2 son los mismos desde una perspectiva de API/hardware. La única diferencia está en el uso de un componente alfa multiplicado previamente, lo que permite a una aplicación realizar su seguimiento sin necesidad de usar otro formato.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT3</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM & DXGI_FORMAT_BC2_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT4</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM & DXGI_FORMAT_BC2_UNORM_SRGB</p>
<div class="alert">
<strong>Nota:</strong>    DXT3 y DXT4 son los mismos desde una perspectiva de API/hardware. La única diferencia está en el uso de un componente alfa multiplicado previamente, lo que permite a una aplicación realizar su seguimiento sin necesidad de usar otro formato.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT5</p></td>
<td align="left"><p>DXGI_FORMAT_BC3_UNORM & DXGI_FORMAT_BC3_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16 & D3DFMT_D16_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D15S1</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24S8</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24X8</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24X4S4</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32F_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24FS8</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_S1D15</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_S8D24</p></td>
<td align="left"><p>DXGI_FORMAT_D24_UNORM_S8_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8D24</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4S4D24</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UNORM</p>
<div class="alert">
<strong>Nota:</strong>    Use. r swizzle en el sombreador para duplicar el color rojo en otros componentes con el fin de obtener el comportamiento de D3D9.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_INDEX16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_INDEX32</p></td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_Q16W16V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_MULTI2_ARGB8</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A32B32G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_CxV8U8</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT1</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT2</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT3</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT4</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPED3DCOLOR</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UBYTE4</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UINT</p>
<div class="alert">
<strong>Nota:</strong>    El sombreador obtiene valores UINT, pero si se necesitan flotantes de estilo de Direct3D 9 (0,0 f, 1,0 f... 255. f), UINT solo se puede convertir a float32 en el sombreador.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SINT</p>
<div class="alert">
<strong>Nota:</strong>    El sombreador obtiene valores de SINT, pero si se necesitan flotantes de estilo de Direct3D 9, se puede convertir SINT a float32 en el sombreador.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<div class="alert">
<strong>Nota:</strong>    El sombreador obtiene valores de SINT, pero si se necesitan flotantes de estilo de Direct3D 9, se puede convertir SINT a float32 en el sombreador.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_UBYTE4N</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_USHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_USHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UDEC3</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_DEC3N</p></td>
<td align="left"><p>No disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT16_2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT16_4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>FourCC 'ATI1'</p></td>
<td align="left"><p>DXGI_FORMAT_BC4_UNORM</p>
<div class="alert">
<strong>Nota:</strong>    Requiere el nivel de característica 10,0 o posterior
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>FourCC 'ATI2'</p></td>
<td align="left"><p>DXGI_FORMAT_BC5_UNORM</p>
<div class="alert">
<strong>Nota:</strong>    Requiere el nivel de característica 10,0 o posterior
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

## <a name="additional-mapping-info"></a>Información de asignación adicional

- **IDirect3DDevice9:: SetCursorPosition** se ha reemplazado por [**SetCursorPos**](/windows/desktop/api/winuser/nf-winuser-setcursorpos).
- **IDirect3DDevice9:: SetCursorProperties** se ha reemplazado por [**setCursor**](/windows/desktop/api/winuser/nf-winuser-setcursor).
- **IDirect3DDevice9:: SetIndices** se ha reemplazado por [**ID3D11DeviceContext:: IASetIndexBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer).
- **IDirect3DDevice9:: SetRenderTarget** se ha reemplazado por [**ID3D11DeviceContext:: OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets).
- **IDirect3DDevice9:: SetScissorRect** se ha reemplazado por [**ID3D11DeviceContext:: RSSetScissorRects**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetscissorrects).
- **IDirect3DDevice9:: SetStreamSource** se ha reemplazado por [**ID3D11DeviceContext:: IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers).
- **IDirect3DDevice9:: SetVertexDeclaration** se ha reemplazado por [**ID3D11DeviceContext:: IASetInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout).
- **IDirect3DDevice9:: SetViewport** se ha reemplazado por [**ID3D11DeviceContext:: RSSetViewports**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports).
- **IDirect3DDevice9:: ShowCursor** se ha reemplazado por [**ShowCursor**](/windows/desktop/api/winuser/nf-winuser-showcursor).

El control de la rampa de gamma de hardware de la tarjeta de vídeo a través de **IDirect3DDevice9:: SetGammaRamp** se reemplaza por **IDXGIOutput:: SetGammaControl**. Vea [usar corrección Gamma](/windows/win32/direct3ddxgi/using-gamma-correction).

**IDirect3DDevice9::P rocessvertices** se reemplaza por la funcionalidad de flujo y salida de los sombreadores de geometría. Vea [Introducción a la fase Stream-Output](/windows/win32/direct3d11/d3d10-graphics-programming-guide-output-stream-stage-getting-started).

El método **IDirect3DDevice9:: SetClipPlane** para establecer los planos de clips del usuario se ha reemplazado por la semántica de salida del sombreador de vértices **SV_ClipDistance** de HLSL (vea [semántica](/windows/win32/direct3dhlsl/dx-graphics-hlsl-semantics)), disponible en VS_4_0 y superior, o el nuevo atributo de función de HLSL clipplanes (consulte [planos de recortes de usuario en hardware de nivel de característica 9](/windows/win32/direct3dhlsl/user-clip-planes-on-10level9)).

**IDirect3DDevice9:: SetPaletteEntries** y **IDirect3DDevice9:: SetCurrentTexturePalette** están en desuso. Reemplácelos por un sombreador de píxeles que busque los colores en una textura de **R8G8B8A8** de 256x1 en su lugar.

Las funciones de teselación de funciones fijas como **DrawRectPatch**, **DrawTriPatch**, **SetNPatchMode**y **DeletePatch** están desusadas. Reemplácelos por los sombreadores de teselación SM 5.0 de canalización programable (si el hardware admite sombreadores de teselación).

Los códigos **IDirect3DDevice9:: SetFVF**y FVF ya no se admiten. Debe migrar desde códigos FVF de D3D8/D3D9 a declaraciones de vértices D3D9 antes de migrar a D3D11 diseños de entrada.

Todos los tipos de **D3DDECLTYPE** que no se admiten directamente se pueden emular de manera bastante eficaz con un pequeño número de operaciones bit a bit al principio de un sombreador de vértices en VS_4_0 y hacia arriba.