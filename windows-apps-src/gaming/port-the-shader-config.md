---
author: mtoepke
title: Migrar objetos de sombreador
description: Cuando portes el representador simple de OpenGL ES 2.0, el primer paso es configurar los objetos equivalentes del sombreador de fragmentos y vértices en Direct3D 11, y asegurarte de que el programa principal pueda comunicarse con los objetos del sombreador después de su compilación.
ms.assetid: 0383b774-bc1b-910e-8eb6-cc969b3dcc08
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, games, juegos, port, portar, shader, sombreador, direct3d, opengl
ms.localizationpriority: medium
ms.openlocfilehash: bbf7e05a93ccce4188d62f9800a5f225be713cc6
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "7103717"
---
# <a name="port-the-shader-objects"></a>Portar objetos de sombreador




**API importantes**

-   [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379)
-   [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)

Cuando portes el representador simple de OpenGL ES 2.0, el primer paso es configurar los objetos equivalentes del sombreador de fragmentos y vértices en Direct3D 11 y asegurarte de que el programa principal pueda comunicarse con los objetos de sombreador después de su compilación.

> **¿Nota**  ha creado un nuevo proyecto de Direct3D? Si no lo hiciste, sigue las instrucciones incluidas en [Crear un nuevo proyecto de DirectX 11 para la Plataforma universal de Windows (UWP)](user-interface.md). En este tutorial suponemos que tienes los recursos de Direct3D y DXGI creados para dibujar en la pantalla y que se proporcionan en la plantilla.

 

De forma muy parecida a OpenGL ES 2.0, los sombreadores compilados en Direct3D deben asociarse con un contexto de dibujo. Ten en cuenta que Direct3D no tiene el concepto de un objeto de programa sombreador en sí; debes asignar sombreadores directamente a una interfaz [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385). Este paso sigue el proceso de OpenGL ES 2.0 para crear y enlazar objetos de sombreador y te proporciona los comportamientos de API correspondientes en Direct3D.

<a name="instructions"></a>Instrucciones
------------

### <a name="step-1-compile-the-shaders"></a>Paso 1: compilar los sombreadores

En esta muestra simple de OpenGL ES 2.0, el sombreador se almacena como archivos de texto y se carga como datos de cadena de la compilación en tiempo de ejecución.

OpenGL ES 2.0: compilar un sombreador

``` syntax
GLuint __cdecl CompileShader (GLenum shaderType, const char *shaderSrcStr)
// shaderType can be GL_VERTEX_SHADER or GL_FRAGMENT_SHADER. Returns 0 if compilation fails.
{
  GLuint shaderHandle;
  GLint compiledShaderHandle;
   
  // Create an empty shader object.
  shaderHandle = glCreateShader(shaderType);

  if (shaderHandle == 0)
  return 0;

  // Load the GLSL shader source as a string value. You could obtain it from
  // from reading a text file or hardcoded.
  glShaderSource(shaderHandle, 1, &shaderSrcStr, NULL);
   
  // Compile the shader.
  glCompileShader(shaderHandle);

  // Check the compile status
  glGetShaderiv(shaderHandle, GL_COMPILE_STATUS, &compiledShaderHandle);

  if (!compiledShaderHandle) // error in compilation occurred
  {
    // Handle any errors here.
              
    glDeleteShader(shaderHandle);
    return 0;
  }

  return shaderHandle;

}
```

En Direct3D, los sombreadores no se compilan en tiempo de ejecución; siempre se compilan en archivos CSO cuando se compila el resto del programa. Cuando compilas tu aplicación con Microsoft Visual Studio, los archivos HLSL se compilan en archivos CSO (.cso) que la aplicación debe cargar. Asegúrate de incluir estos archivos CSO con la aplicación cuando la empaquetes.

> **Nota**  en el ejemplo siguiente, se realiza la carga y sombreador compilación asincrónicamente mediante la sintaxis de expresión lambda y la palabra clave **auto** . ReadDataAsync() es un método implementado en la plantilla que se lee en un archivo CSO como una matriz de datos de bytes (fileData).

 

Direct3D 11: compilar un sombreador

``` syntax
auto loadVSTask = DX::ReadDataAsync(m_projectDir + "SimpleVertexShader.cso");
auto loadPSTask = DX::ReadDataAsync(m_projectDir + "SimplePixelShader.cso");

auto createVSTask = loadVSTask.then([this](Platform::Array<byte>^ fileData) {

m_d3dDevice->CreateVertexShader(
  fileData->Data,
  fileData->Length,
  nullptr,
  &m_vertexShader);

auto createPSTask = loadPSTask.then([this](Platform::Array<byte>^ fileData) {
  m_d3dDevice->CreatePixelShader(
    fileData->Data,
    fileData->Length,
    nullptr,
    &m_pixelShader;
};
```

### <a name="step-2-create-and-load-the-vertex-and-fragment-pixel-shaders"></a>Paso 2: crear y cargar los sombreadores de fragmentos (píxeles) y vértices

OpenGL ES 2.0 tiene la noción de un "programa" sombreador, que sirve como interfaz entre el programa principal que se ejecuta en la CPU y los sombreadores que se ejecutan en la GPU. Los sombreadores se compilan (o se cargan en fuentes compiladas) y se asocian con un programa que permite su ejecución en la GPU.

OpenGL ES 2.0: cargar los sombreadores de fragmentos y vértices en un programa sombreador

``` syntax
GLuint __cdecl LoadShaderProgram (const char *vertShaderSrcStr, const char *fragShaderSrcStr)
{
  GLuint programObject, vertexShaderHandle, fragmentShaderHandle;
  GLint linkStatusCode;

  // Load the vertex shader and compile it to an internal executable format.
  vertexShaderHandle = CompileShader(GL_VERTEX_SHADER, vertShaderSrcStr);
  if (vertexShaderHandle == 0)
  {
    glDeleteShader(vertexShaderHandle);
    return 0;
  }

   // Load the fragment/pixel shader and compile it to an internal executable format.
  fragmentShaderHandle = CompileShader(GL_FRAGMENT_SHADER, fragShaderSrcStr);
  if (fragmentShaderHandle == 0)
  {
    glDeleteShader(fragmentShaderHandle);
    return 0;
  }

  // Create the program object proper.
  programObject = glCreateProgram();
   
  if (programObject == 0)    return 0;

  // Attach the compiled shaders
  glAttachShader(programObject, vertexShaderHandle);
  glAttachShader(programObject, fragmentShaderHandle);

  // Compile the shaders into binary executables in memory and link them to the program object..
  glLinkProgram(programObject);

  // Check the project object link status and determine if the program is available.
  glGetProgramiv(programObject, GL_LINK_STATUS, &linkStatusCode);

  if (!linkStatusCode) // if link status <> 0
  {
    // Linking failed; delete the program object and return a failure code (0).

    glDeleteProgram (programObject);
    return 0;
  }

  // Deallocate the unused shader resources. The actual executables are part of the program object.
  glDeleteShader(vertexShaderHandle);
  glDeleteShader(fragmentShaderHandle);

  return programObject;
}

// ...

glUseProgram(renderer->programObject);
```

Direct3D no tiene el concepto de un objeto de programa sombreador. En cambio, los sombreadores se crean cuando se llama a uno de los métodos de creación de sombreador en la interfaz [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) (como [**ID3D11Device::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) o [**ID3D11Device::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)). Para establecer los sombreadores del contexto de dibujo actual, se los proporcionamos a la interfaz [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) correspondiente con un método de sombreador set, como [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) del sombreador de vértices o [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) del sombreador de fragmentos.

Direct3D 11: establecer los sombreadores para el contexto de dibujo del dispositivo de gráficos

``` syntax
m_d3dContext->VSSetShader(
  m_vertexShader.Get(),
  nullptr,
  0);

m_d3dContext->PSSetShader(
  m_pixelShader.Get(),
  nullptr,
  0);
```

### <a name="step-3-define-the-data-to-supply-to-the-shaders"></a>Paso 3: definir los datos que hay que suministrar a los sombreadores

En nuestro ejemplo de OpenGL ES 2.0, tenemos que declarar un elemento **uniform** para la canalización del sombreador:

-   **u\_mvpMatrix**: matriz 4x4 de elementos flotantes que representa la matriz de transformación de la proyección de vista de modelo final, que toma las coordenadas del modelo para el cubo y las transforma en coordenadas de proyección 2D para la conversión de digitalización.

y dos valores **attribute** para los datos de vértice:

-   **a\_position**: vector de cuatro elementos flotantes para las coordenadas del modelo de un vértice.
-   **a\_color**: vector de cuatro elementos flotantes para el valor de color RGBA asociado con el vértice.

Open GL ES 2.0: definiciones de GLSL para los uniformes y atributos

``` syntax
uniform mat4 u_mvpMatrix;
attribute vec4 a_position;
attribute vec4 a_color;
```

Las variables del programa principal correspondiente se definen como campos en el objeto del representador, en este caso. (Consulta el encabezado del artículo [Portar un representador simple de OpenGL ES 2.0 a Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)). Una vez hecho esto, necesitaremos especificar las ubicaciones en la memoria donde el programa principal suministrará estos valores para la canalización del sombreador. Normalmente lo hacemos justo antes de realizar una llamada draw.

OpenGL ES 2.0: marcar la ubicación de los datos de uniformes y atributos

``` syntax

// Inform the shader of the attribute locations
loc = glGetAttribLocation(renderer->programObject, "a_position");
glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), 0);
glEnableVertexAttribArray(loc);

loc = glGetAttribLocation(renderer->programObject, "a_color");
glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);


// Inform the shader program of the uniform location
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");
```

Direct3D no tiene el concepto de "atributo" ni "uniforme" en el mismo sentido (o al menos no comparte esta sintaxis). En cambio, tiene búferes de constantes, representados como subrecursos de Direct3D, recursos que se comparten entre el programa principal y los programas sombreadores. Algunos de estos subrecursos, como las posiciones de vértice y los colores, se describen como semántica de HLSL. Para obtener más información sobre búferes de constantes y semántica de HLSL, así como su relación con los conceptos de OpenGL ES 2.0, lee [Migrar objetos de búfer de trama, uniformes y atributos](porting-uniforms-and-attributes.md).

Al trasladar este proceso a Direct3D, convertimos el elemento uniforme a un búfer de constantes de Direct3D (cbuffer) y le asignamos un registro de búsqueda mediante la semántica HLSL **register**. Los dos atributos de vértice se administran como elementos de entrada en las fases de canalización del sombreador y también se les asigna la [semántica de HLSL](https://msdn.microsoft.com/library/windows/desktop/bb205574) (POSITION y COLOR0) que se encarga de informar a los sombreadores. El sombreador de píxeles toma un valor SV\_POSITION; aquí el prefijo SV\_ indica que es un valor del sistema generado por la GPU. (En este caso, es una posición de píxeles generada durante la conversión de digitalización). VertexShaderInput y PixelShaderInput no se declaran como búferes de constantes, porque el primero se usará para definir el búfer de vértices (consulta [Portar datos y búferes de vértices](port-the-vertex-buffers-and-data-config.md)), y los datos del segundo se generan como resultado de una fase previa en la canalización, que en este caso corresponde al sombreador de vértices.

Direct3D: definiciones de HLSL para los búferes de constantes y datos de vértice

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float4 pos : POSITION;
  float4 color : COLOR0;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

Para más información sobre la migración a búferes de constates y la aplicación de la semántica de HLSL, consulta [Migrar objetos de búfer de trama, uniformes y atributos](porting-uniforms-and-attributes.md).

Estas son las estructuras para el diseño de los datos que se pasan a la canalización de sombreador con un búfer de vértices o de constantes.

Direct3D 11: declarar el diseño de búferes de vértices y de constantes

``` syntax
// Constant buffer used to send MVP matrices to the vertex shader.
struct ModelViewProjectionConstantBuffer
{
  DirectX::XMFLOAT4X4 modelViewProjection;
};

// Used to send per-vertex data to the vertex shader.
struct VertexPositionColor
{
  DirectX::XMFLOAT4 pos;
  DirectX::XMFLOAT4 color;
};
```

Te recomendamos que uses los tipos de DirectXMath\XM* de los elementos de búferes de constantes, dado que proporcionan un empaquetado y alineación apropiados para el contenido cuando se envían a la canalización del sombreador. Si usas matrices y tipos flotantes estándar de la plataforma de Windows, debes realizar el empaquetado y la alineación tú mismo.

Para enlazar un búfer de constantes, crea una descripción de diseño a modo de la estructura [**CD3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/jj151620) y pásala a [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501). A continuación, en el método de representación, pasa el búfer de constantes a [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) antes de dibujar nada.

Direct3D 11: enlazar el búfer de constantes

``` syntax
CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);

m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);

// ...

// Only update shader resources that have changed since the last frame.
m_d3dContext->UpdateSubresource(
  m_constantBuffer.Get(),
  0,
  NULL,
  &m_constantBufferData,
  0,
  0);
```

El búfer de vértices se crea y actualiza de manera similar. Esto se explica en el siguiente paso, [Portar datos y búferes de vértices](port-the-vertex-buffers-and-data-config.md).

<a name="next-step"></a>Paso siguiente
---------

[Portar datos y búferes de vértices](port-the-vertex-buffers-and-data-config.md)
## <a name="related-topics"></a>Temas relacionados


[Portar un representador simple de OpenGL ES 2.0 a Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[Portar datos y búferes de vértices](port-the-vertex-buffers-and-data-config.md)

[Migrar GLSL](port-the-glsl.md)

[Dibujar en la pantalla](draw-to-the-screen.md)

 

 




