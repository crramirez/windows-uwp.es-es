---
title: Asignar características de DirectX 9 a las API de DirectX 11
description: Aprende a trasladar las características de tu juego Direct3D 9 a Direct3D 11 y a la Plataforma universal de Windows (UWP).
ms.assetid: 3aa8a114-4e47-ae0a-9447-88ba324377b8
---

# Asignar características de DirectX 9 a las API de DirectX 11


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Resumen**

-   [Planea tu migración de DirectX](plan-your-directx-port.md)
-   [Cambios importantes en Direct3D 11 respecto a Direct3D 9](understand-direct3d-11-1-concepts.md)
-   Asignación de características


Aprende a trasladar las características de tu juego Direct3D 9 a Direct3D 11 y a la Plataforma universal de Windows (UWP).

## Asignar Direct3D 9 a las API de DirectX 11


[Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) sigue siendo la base de los elementos gráficos DirectX, pero la API cambió a partir de DirectX 9:

-   La Infraestructura de gráficos de Microsoft DirectX (DXGI) se usa para configurar adaptadores de gráficos. Usa [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534) para seleccionar formatos de búfer, crear cadenas de intercambio, presentar marcos y crear recursos compartidos. Consulta [Introducción a DXGI](https://msdn.microsoft.com/library/windows/desktop/bb205075).
-   El contexto de dispositivo de Direct3D se usa para establecer el estado de canalización y generar comandos de representación. La mayoría de nuestras muestras usan un contexto inmediato para representar directamente en el dispositivo. Direct3D 11 también admite la representación multithreading, en cuyo caso se usan contextos diferidos. Consulta [Introducción a un dispositivo en Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476880).
-   Algunas características ya no se usan, particularmente la canalización de función fija. Consulta [Características desusadas](https://msdn.microsoft.com/library/windows/desktop/cc308047).

Para obtener una lista completa de las características de  Direct3D 11, consulta [Características de Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476342) y [Características de Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/hh404562).

## Pasar de Direct2D 9 a Direct2D 11


[Direct2D (Windows)](https://msdn.microsoft.com/library/windows/desktop/dd370990) sigue siendo parte importante de los elementos gráficos de DirectX y Windows. Todavía puedes usar Direct2D para dibujar juegos 2D y dibujar superposiciones (HUD) sobre Direct3D.

Direct2D se ejecuta encima de Direct3D. Los juegos 2D pueden implementarse en cualquiera de sus API. Por ejemplo, un juego 2D implementado con Direct3D puede usar la proyección ortográfica, establecer valores Z para controlar el orden de dibujo de primitivos y usar sombreadores de píxeles para agregar efectos especiales.

Dado que Direct2D se basa en Direct3D, también usa DXGI y contextos de dispositivo. Consulta [Introducción a la API de Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd317121).

La API [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) agrega compatibilidad para texto con formato otorgado mediante Direct2D. Consulta [Introducción a DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd371554).

## Reemplazar bibliotecas de aplicaciones auxiliares desusadas


D3DX y DXUT están en desuso y no pueden usarse para juegos de UWP. Estas bibliotecas auxiliares proporcionaban recursos para tareas, como la carga de texturas y mallas.

-   El tutorial [Migración simple de Direct3D 9 a UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md) te muestra cómo configurar una ventana, inicializar Direct3D y hacer una representación básica en 3D.
-   El tutorial [Crear un juego para UWP sencillo con DirectX](tutorial--create-your-first-metro-style-directx-game.md) te muestra tareas comunes de programación de juegos, entre ellas los elementos gráficos, la carga de archivos, la interfaz de usuario, los controles y el sonido.
-   El proyecto [DirectX Tool Kit (Kit de herramientas de DirectX)](http://go.microsoft.com/fwlink/p/?LinkID=248929) de la comunidad, te ofrece clases auxiliares que puedes usar con aplicaciones de Direct3D 11 y UWP.

## Trasladar programas sombreadores de FX a HLSL


La biblioteca de utilidades de D3DX (D3DX 9, D3DX 10 y D3DX 11), incluidos los efectos, está en desuso para UWP. Todos los juegos de DirectX para UWP controlan la canalización de gráficos con [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561) sin efectos.

Visual Studio todavía usa FXC como opción avanzada para compilar objetos de sombreador. Los sombreadores de los juegos de la UWP se compilan con anticipación. El código de bytes se carga en tiempo de ejecución y, a continuación, cada recurso de sombreador se enlaza a la canalización de elementos gráficos durante el pase de representación apropiado. Los sombreadores deben moverse a sus propios archivos .HLSL independientes y las técnicas de representación deben implementarse en el código C++.

Para ver rápidamente la carga de recursos de sombreador, consulta [Migración simple de Direct3D 9 a UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

Direct3D 11 introdujo el modelo de sombreador 5, que requiere el nivel de característica 11\_0 (o superior) de Direct3D. Consulta [HLSL Shader Model 5 Features for Direct3D 11 (Características del modelo de sombreador 5 de HLSL para Direct3D 11)](https://msdn.microsoft.com/library/windows/desktop/ff471419).

## Reemplazar XNAMath y D3DXMath


El código que usa XNAMath (o D3DXMath) debe migrarse a [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833). DirectXMath incluye tipos portátiles entre x86, x64 y ARM. Consulta [Migración de código de la biblioteca de matemáticas XNA](https://msdn.microsoft.com/library/windows/desktop/ee418730).

Ten en cuenta que con los sombreadores conviene usar tipos flotantes de DirectXMath. Por ejemplo las estructuras [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608) y [**XMFLOAT4X4**](https://msdn.microsoft.com/library/windows/desktop/ee419621) alinean los datos de manera conveniente para los búferes de constantes.

## Reemplazar DirectSound con XAudio2 (y audio de fondo)


DirectSound no es compatible con la UWP:

-   Usa [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) para agregar efectos de sonido a tu juego.

##  Reemplazar DirectInput con XInput y las API de la UWP


DirectInput no es compatible con la UWP:

-   Usa la clase para las devoluciones de llamada de eventos de entrada [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) para la entrada táctil, de mouse y de teclado.
-   Usa [XInput](https://msdn.microsoft.com/library/windows/desktop/ee417001) 1.4 para la compatibilidad con el dispositivo de juego y los auriculares con micrófono. Si estás usando una base de código compartido para escritorio y UWP, consulta [XInput Versions (Versiones de XInput)](https://msdn.microsoft.com/library/windows/desktop/hh405051) para obtener información sobre la compatibilidad con versiones anteriores.
-   Regístrate para usar los eventos de la clase [**EdgeGesture**](https://msdn.microsoft.com/library/windows/apps/hh701600) si tu juego necesita usar la barra de aplicaciones.

## Usar Microsoft Media Foundation en lugar de DirectShow


DirectShow ya no forma parte de la API de DirectX ni de la API de Windows. [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) proporciona contenido de vídeo para Direct3D mediante superficies compartidas. Consulta [Las API de vídeo en Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/hh447677).

## Reemplazar DirectPlay con código de red


Microsoft DirectPlay está en desuso. Si tu juego usa servicios de red, debes proporcionar un código de red que cumpla con los requisitos de UWP. Usa estas API:

-   [Win32 y COM para aplicaciones de la Tienda Windows (redes) (Windows)](https://msdn.microsoft.com/library/windows/apps/br205759)
-   [**Espacio de nombres Windows.Networking (Windows)**](https://msdn.microsoft.com/library/windows/apps/br207124)
-   [**Espacio de nombres Windows.Networking.Sockets (Windows)**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [**Espacio de nombres Windows.Networking.Connectivity (Windows)**](https://msdn.microsoft.com/library/windows/apps/br207308)
-   [**Espacio de nombres Windows.ApplicationModel.Background (Windows)**](https://msdn.microsoft.com/library/windows/apps/br224847)

Estos artículos te ayudarán a agregar características de red y a declarar la compatibilidad de las redes en el manifiesto del paquete de la aplicación.

-   [Conexión con sockets (aplicaciones de la Tienda Windows con C#/VB/C++ y XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh452976)
-   [Conexión con Websockets (aplicaciones de la Tienda Windows con C#/VB/C++ y XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh994396)
-   [Conexión a servicios web (aplicaciones de la Tienda Windows con C#/VB/C++ y XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)
-   [Conceptos básicos de redes](https://msdn.microsoft.com/library/windows/apps/mt280233)

Ten en cuenta que todas las aplicaciones para UWP (incluidos los juegos) usan tipos específicos de tareas en segundo plano para mantener la conectividad mientras la aplicación está suspendida. Si tu juego necesita mantener el estado de conexión durante la suspensión, consulta [Conceptos básicos de redes](https://msdn.microsoft.com/library/windows/apps/mt280233).

## Asignación de funciones


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
<td align="left"><p>[<strong>IDirect3DDevice9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174336)</p></td>
<td align="left"><p>[<strong>ID3D11Device2</strong>](https://msdn.microsoft.com/library/windows/desktop/dn280493)</p>
<p>[<strong>ID3D11DeviceContext2</strong>](https://msdn.microsoft.com/library/windows/desktop/dn280498)</p>
<p>Las fases de la canalización de elementos gráficos se describen en [Graphics Pipeline](https://msdn.microsoft.com/library/windows/desktop/ff476882).</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3D9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174300)</p></td>
<td align="left"><p>[<strong>IDXGIFactory2</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404556)</p>
<p>[<strong>IDXGIAdapter2</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404537)</p>
<p>[<strong>IDXGIDevice3</strong>](https://msdn.microsoft.com/library/windows/desktop/dn280345)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DDevice9::Present</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174423)</p></td>
<td align="left"><p>[<strong>IDXGISwapChain1::Present1</strong>](https://msdn.microsoft.com/library/windows/desktop/hh446797)</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9::TestCooperativeLevel</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174472)</p></td>
<td align="left"><p>Llama a [<strong>IDXGISwapChain1::Present1</strong>](https://msdn.microsoft.com/library/windows/desktop/hh446797) con el conjunto de marcas DXGI_PRESENT_TEST.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DBaseTexture9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174322)</p>
<p>[<strong>IDirect3DTexture9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205909)</p>
<p>[<strong>IDirect3DCubeTexture9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174329)</p>
<p>[<strong>IDirect3DVolumeTexture9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205941)</p>
<p>[<strong>IDirect3DIndexBuffer9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205865)</p>
<p>[<strong>IDirect3DVertexBuffer9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205915)</p></td>
<td align="left"><p>[<strong>ID3D11Buffer</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476351)</p>
<p>[<strong>ID3D11Texture1D</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476633)</p>
<p>[<strong>ID3D11Texture2D</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476635)</p>
<p>[<strong>ID3D11Texture3D</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476637)</p>
<p>[<strong>ID3D11ShaderResourceView</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476628)</p>
<p>[<strong>ID3D11RenderTargetView</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476582)</p>
<p>[<strong>ID3D11DepthStencilView</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476377)</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DVertexShader9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205922)</p>
<p>[<strong>IDirect3DPixelShader9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205869)</p></td>
<td align="left"><p>[<strong>ID3D11VertexShader</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476641)</p>
<p>[<strong>ID3D11PixelShader</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476576)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DVertexDeclaration9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205919)</p></td>
<td align="left"><p>[<strong>ID3D11InputLayout</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476575)</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9::SetRenderState</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205805)</p>
<p>[<strong>IDirect3DDevice9::SetSamplerState</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205806)</p></td>
<td align="left"><p>[<strong>ID3D11BlendState1</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404571)</p>
<p>[<strong>ID3D11DepthStencilState</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476375)</p>
<p>[<strong>ID3D11RasterizerState1</strong>](https://msdn.microsoft.com/library/windows/desktop/hh446828)</p>
<p>[<strong>ID3D11SamplerState</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476588)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DDevice9::DrawIndexedPrimitive</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174369)</p>
<p>[<strong>IDirect3DDevice9::DrawPrimitive</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174371)</p></td>
<td align="left"><p>[<strong>ID3D11DeviceContext::Draw</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476407)</p>
<p>[<strong>ID3D11DeviceContext::DrawIndexed</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476409)</p>
<p>[<strong>ID3D11DeviceContext::DrawIndexedInstanced</strong>](https://msdn.microsoft.com/library/windows/desktop/bb173566)</p>
<p>[<strong>ID3D11DeviceContext::DrawInstanced</strong>](https://msdn.microsoft.com/library/windows/desktop/bb173567)</p>
<p>[<strong>ID3D11DeviceContext::IASetPrimitiveTopology</strong>](https://msdn.microsoft.com/library/windows/desktop/bb173590)</p>
<p>[<strong>ID3D11DeviceContext::DrawAuto</strong>](https://msdn.microsoft.com/library/windows/desktop/bb173564)</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9::BeginScene</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174350)</p>
<p>[<strong>IDirect3DDevice9::EndScene</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174375)</p>
<p>[<strong>IDirect3DDevice9::DrawPrimitiveUP</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174372)</p>
<p>[<strong>IDirect3DDevice9::DrawIndexedPrimitiveUP</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174370)</p></td>
<td align="left"><p>No hay equivalente directo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DDevice9::ShowCursor</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174470)</p>
<p>[<strong>IDirect3DDevice9::SetCursorPosition</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174429)</p>
<p>[<strong>IDirect3DDevice9::SetCursorProperties</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174430)</p></td>
<td align="left"><p>Usa API de cursor estándar.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9::Reset</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174425)</p></td>
<td align="left"><p>El dispositivo LOST y POOL_MANAGED ya no existen. Puede haber un error en el método [<strong>IDXGISwapChain1::Present1</strong>](https://msdn.microsoft.com/library/windows/desktop/hh446797) que tenga un valor devuelto [<strong>DXGI_ERROR_DEVICE_REMOVED</strong>](https://msdn.microsoft.com/library/windows/desktop/bb509553).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DDevice9:DrawRectPatch</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174373)</p>
<p>[<strong>IDirect3DDevice9:DrawTriPatch</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174374)</p>
<p>[<strong>IDirect3DDevice9:LightEnable</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174421)</p>
<p>[<strong>IDirect3DDevice9:MultiplyTransform</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174422)</p>
<p>[<strong>IDirect3DDevice9:SetLight</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205798)</p>
<p>[<strong>IDirect3DDevice9:SetMaterial</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174437)</p>
<p>[<strong>IDirect3DDevice9:SetNPatchMode</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174438)</p>
<p>[<strong>IDirect3DDevice9:SetTransform</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174463)</p>
<p>[<strong>IDirect3DDevice9:SetFVF</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174433)</p>
<p>[<strong>IDirect3DDevice9:SetTextureStageState</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174462)</p></td>
<td align="left"><p>La canalización de función fija está en desuso.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9:CheckDepthStencilMatch</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174308)</p>
<p>[<strong>IDirect3DDevice9:CheckDeviceFormat</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174309)</p>
<p>[<strong>IDirect3DDevice9:GetDeviceCaps</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174320)</p>
<p>[<strong>IDirect3DDevice9:ValidateDevice</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205859)</p></td>
<td align="left"><p>Los bits de capacidad se reemplazan con niveles de características. Solo unos pocos casos de uso de características y formato son opcionales para un nivel de características determinado. Esto puede revisarse con [<strong>ID3D11Device::CheckFeatureSupport</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476497) y [<strong>ID3D11Device::CheckFormatSupport</strong>](https://msdn.microsoft.com/library/windows/desktop/bb173536).</p></td>
</tr>
</tbody>
</table>

 

## Asignación de formatos de superficie


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
<strong>Nota</strong> Usa la referencia .r en el sombreador para duplicar el color rojo en otros componentes y obtener el comportamiento de Direct3D 9.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_UNORM</p>
<div class="alert">
<strong>Nota</strong> Usa la referencia .r en el sombreador para duplicar el color rojo y mover el color verde a otros componentes alfa, y obtener el comportamiento de Direct3D 9.
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
<strong>Nota</strong> En Direct3D 9 la escala de datos aumentó en 255.0f, pero puede controlarse en el sombreador.
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
<strong>Nota</strong> En Direct3D 9 la escala de datos aumentó en 255.0f, pero puede controlarse en el sombreador.
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
<strong>Nota</strong> DXT1 y DXT2 son iguales desde una perspectiva de API o hardware. La única diferencia está en el uso de un componente alfa multiplicado previamente, lo que permite a una aplicación realizar su seguimiento sin necesidad de usar otro formato.
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
<strong>Nota</strong> DXT3 y DXT4 son iguales desde una perspectiva de API o hardware. La única diferencia está en el uso de un componente alfa multiplicado previamente, lo que permite a una aplicación realizar su seguimiento sin necesidad de usar otro formato.
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
<strong>Nota</strong> Usa la referencia .r en el sombreador para duplicar el color rojo en otros componentes y obtener el comportamiento de D3D9.
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
<strong>Nota</strong> El sombreador obtiene valores UINT, pero si se necesitan flotantes integrales de estilo de Direct3D 9 (0.0f, 1.0f... 255.f), UINT solo puede convertirse a float32 en el sombreador.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SINT</p>
<div class="alert">
<strong>Nota</strong> El sombreador obtiene valores SINT, pero si se necesitan flotantes integrales de estilo de Direct3D 9, SINT solo puede convertirse a float32 en el sombreador.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<div class="alert">
<strong>Nota</strong> El sombreador obtiene valores SINT, pero si se necesitan flotantes integrales de estilo de Direct3D 9, SINT solo puede convertirse a float32 en el sombreador.
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
<strong>Nota</strong> Requiere el nivel de característica 10.0 o posterior
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>FourCC 'ATI2'</p></td>
<td align="left"><p>DXGI_FORMAT_BC5_UNORM</p>
<div class="alert">
<strong>Nota</strong> Requiere el nivel de característica 10.0 o posterior
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

 

 






<!--HONumber=Mar16_HO1-->


