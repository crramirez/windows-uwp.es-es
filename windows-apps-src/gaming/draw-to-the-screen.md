---
author: mtoepke
title: Dibujar en la pantalla
description: Por fin hemos portado el código que dibuja un cubo giratorio en la pantalla.
ms.assetid: cc681548-f694-f613-a19d-1525a184d4ab
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, juegos, games, directx, gráficos, graphics
ms.localizationpriority: medium
ms.openlocfilehash: 0050b854b6c8c02cd6eda5e4903fe07ee25d521d
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7437142"
---
# <a name="draw-to-the-screen"></a>Dibujar en la pantalla




**API importantes**

-   [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)
-   [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)
-   [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)

Por fin hemos portado el código que dibuja un cubo giratorio en la pantalla.

En OpenGL ES 2.0, el contexto de dibujo se define como un tipo EGLContext que contiene los parámetros de superficie y ventana, así como los recursos necesarios para dibujar los destinos de representación que se usarán para componer la imagen final. Usas este contexto para configurar los recursos gráficos, de modo tal que los resultados de la canalización de sombreador se muestren correctamente en pantalla. Uno de los recursos principales es el "búfer de reserva" (u "objeto de búfer de trama") que contiene los destinos de representación compuestos y finales, listos para la presentación en pantalla.

Con Direct3D, el proceso de configurar los recursos de gráficos para dibujar en la pantalla es más didáctico y requiere pocas API adicionales. (Pero recuerda que una plantilla Direct3D de Microsoft Visual Studio puede simplificar de manera significativa este proceso). Para obtener un contexto (denominado contexto de dispositivo Direct3D), primero debes obtener un objeto [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) y usarlo para crear y configurar un objeto [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598). Estos dos objetos se usan en conjunto para configurar los recursos específicos que necesitas para dibujar en pantalla.

En menos palabras, las API de DXGI contienen sobre todo varias API para administrar recursos que directamente pertenecen al adaptador de gráficos y Direct3D contiene las API para una interfaz entre la GPU y tu programa principal que se ejecuta en la CPU. 

Para que podamos hacer comparaciones en nuestra muestra, estos son los tipos relevantes de cada API:

-   [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575): proporciona una representación visual del dispositivo de gráficos y sus recursos.
-   [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598): proporciona la interfaz para configurar búferes y emitir comandos de representación.
-   [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631): la cadena de intercambio es análoga al búfer de reserva en OpenGL ES 2.0. Es la región de la memoria en el adaptador de gráficos que contiene las imágenes finales de representación para mostrar. Se denomina "cadena de intercambio" porque tiene varios búferes que pueden escribirse e "intercambiarse" para presentar la representación más reciente en pantalla.
-   [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582): contiene el búfer del mapa de bits 2D donde el contexto del dispositivo Direct3D dibuja y, además, presenta la cadena de intercambio. Al igual que con OpenGL ES 2.0, puedes tener varios destinos de representación. Algunos de ellos no se enlazan a la cadena de intercambio, pero se usan para técnicas de sombreado de varios pases.

En la plantilla, el objeto de representador contiene estos campos:

Direct3D 11: declaraciones de dispositivo y contexto de dispositivo

``` syntax
Platform::Agile<Windows::UI::Core::CoreWindow>       m_window;

Microsoft::WRL::ComPtr<ID3D11Device1>                m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>          m_d3dContext;
Microsoft::WRL::ComPtr<IDXGISwapChain1>                      m_swapChainCoreWindow;
Microsoft::WRL::ComPtr<ID3D11RenderTargetView>          m_d3dRenderTargetViewWin;
```

Analiza este ejemplo para saber cómo se configura un búfer de reserva como destino de representación y cómo se proporciona a la cadena de intercambio.

``` syntax
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(backBuffer));
m_d3dDevice->CreateRenderTargetView(
  backBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

El tiempo de ejecución de Direct3D crea implícitamente una interfaz [**IDXGISurface1**](https://msdn.microsoft.com/library/windows/desktop/ff471343) para [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635); esta representa la textura como un "búfer de reserva", para que así la cadena de intercambio pueda mostrarla.

La inicialización y la configuración del dispositivo Direct3D, el contexto del dispositivo y los destinos de representación, pueden enlazarse en los métodos personalizados **CreateDeviceResources** y **CreateWindowSizeDependentResources** de la plantilla Direct3D.

Para obtener más información sobre el contexto del dispositivo Direct3D en relación con EGL y el tipo EGLContext, puedes leer [Portar el código EGL a DXGI y Direct3D](moving-from-egl-to-dxgi.md).

## <a name="instructions"></a>Instrucciones

### <a name="step-1-rendering-the-scene-and-displaying-it"></a>Paso 1: representar la escena y mostrarla

Después de actualizar los datos del cubo (en este caso, al girarlo levemente alrededor del eje y), el método Render establece la ventanilla en las dimensiones del contexto de dibujo (un elemento EGLContext). Este contexto contiene el búfer de color que se mostrará en la superficie de la ventana (un EGLSurface), usando la pantalla configurada (EGLDisplay). Esta vez el ejemplo actualiza los atributos de datos de vértice, reenlaza el búfer de índices, dibuja el cubo y realiza el intercambio en el búfer de color dibujado por la canalización de sombreado en la superficie de pantalla.

OpenGL ES 2.0: representar un marco para mostrar

``` syntax
void Render(GraphicsContext *drawContext)
{
  Renderer *renderer = drawContext->renderer;

  int loc;
   
  // Set the viewport
  glViewport ( 0, 0, drawContext->width, drawContext->height );
   
   
  // Clear the color buffer
  glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
  glEnable(GL_DEPTH_TEST);


  // Use the program object
  glUseProgram (renderer->programObject);

  // Load the a_position attribute with the vertex position portion of a vertex buffer element
  loc = glGetAttribLocation(renderer->programObject, "a_position");
  glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
      sizeof(Vertex), 0);
  glEnableVertexAttribArray(loc);

  // Load the a_color attribute with the color position portion of a vertex buffer element
  loc = glGetAttribLocation(renderer->programObject, "a_color");
  glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
      sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
  glEnableVertexAttribArray(loc);

  // Bind the index buffer
  glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);

  // Load the MVP matrix
  glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);

  // Draw the cube
  glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

  eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
}
```

En Direct3D 11 el proceso es muy similar. (Suponemos que estás usando la ventanilla y la configuración del destino de representación de la plantilla Direct3D).

-   Actualiza los búferes de constantes (la matriz de proyección de la vista de modelo, en este caso) con llamadas a [**ID3D11DeviceContext1::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/hh446790).
-   Establece el búfer de vértices con [**ID3D11DeviceContext1::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456).
-   Establece el búfer de índices con [**ID3D11DeviceContext1::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453).
-   Establece la topología de triángulos específica (una lista de triángulos) con [**ID3D11DeviceContext1::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455).
-   Establece el diseño de entrada del búfer de vértices con [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454).
-   Enlaza el sombreador de vértices con [**ID3D11DeviceContext1::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493).
-   Enlaza el sombreador de fragmentos con [**ID3D11DeviceContext1::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472).
-   Envía los vértices indexados mediante sombreadores y crea los resultados de color en el búfer del destino de representación con [**ID3D11DeviceContext1::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409).
-   Muestra el búfer del destino de representación con [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797).

Direct3D 11: representar un marco para mostrar

``` syntax
void RenderObject::Render()
{
  // ...

  // Only update shader resources that have changed since the last frame.
  m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    NULL,
    &m_constantBufferData,
    0,
    0);

  // Set up the IA stage corresponding to the current draw operation.
  UINT stride = sizeof(VertexPositionColor);
  UINT offset = 0;
  m_d3dContext->IASetVertexBuffers(
    0,
    1,
    m_vertexBuffer.GetAddressOf(),
    &stride,
    &offset);

  m_d3dContext->IASetIndexBuffer(
    m_indexBuffer.Get(),
    DXGI_FORMAT_R16_UINT,
    0);

  m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
  m_d3dContext->IASetInputLayout(m_inputLayout.Get());

  // Set up the vertex shader corresponding to the current draw operation.
  m_d3dContext->VSSetShader(
    m_vertexShader.Get(),
    nullptr,
    0);

  m_d3dContext->VSSetConstantBuffers(
    0,
    1,
    m_constantBuffer.GetAddressOf());

  // Set up the pixel shader corresponding to the current draw operation.
  m_d3dContext->PSSetShader(
    m_pixelShader.Get(),
    nullptr,
    0);

  m_d3dContext->DrawIndexed(
    m_indexCount,
    0,
    0);

    // ...

  m_swapChainCoreWindow->Present1(1, 0, &parameters);
}

```

Una vez realizada la llamada a [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797), el marco aparece en la pantalla configurada.

## <a name="previous-step"></a>Paso anterior


[Portar GLSL](port-the-glsl.md)

## <a name="remarks"></a>Observaciones

Este ejemplo pasa por alto gran parte de la complejidad que implica configurar los recursos del dispositivo, en especial aplicaciones DirectX para la Plataforma universal de Windows (UWP). Sugerimos que revises el código completo de la plantilla, sobre todo las partes encargadas de la administración y configuración de recursos de dispositivo y ventana. Las aplicaciones para UWP tienen que admitir tanto eventos de rotación como eventos de suspensión y reanudación. Asimismo, la plantilla muestra los procedimientos recomendados para controlar la pérdida de una interfaz o un cambio en los parámetros de la pantalla.

## <a name="related-topics"></a>Temas relacionados


* [Portar un representador simple de OpenGL ES 2.0 a Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [Portar objetos de sombreador](port-the-shader-config.md)
* [Portar GLSL](port-the-glsl.md)
* [Dibujar en la pantalla](draw-to-the-screen.md)

 

 




