---
title: Cargar recursos en tu juego DirectX
description: La mayoría de los juegos, en algún momento, cargan recursos y activos (como sombreadores, texturas, mallas predefinidas y otros datos de gráficos) del almacenamiento local u otro flujo de datos.
ms.assetid: e45186fa-57a3-dc70-2b59-408bff0c0b41
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, DirectX, carga de recursos
ms.localizationpriority: medium
ms.openlocfilehash: 6a779e0d17cdc3f5a11dd720467e3a0572e3c124
ms.sourcegitcommit: 2571af6bf781a464a4beb5f1aca84ae7c850f8f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82606314"
---
# <a name="load-resources-in-your-directx-game"></a>Cargar recursos en tu juego DirectX



La mayoría de los juegos, en algún momento, cargan recursos y activos (como sombreadores, texturas, mallas predefinidas y otros datos de gráficos) del almacenamiento local u otro flujo de datos. Aquí le guiaremos a través de una vista de alto nivel de lo que debe tener en cuenta al cargar estos archivos para usarlos en el juego de Plataforma universal de Windows (UWP) de DirectX C/C++.

Por ejemplo, es probable que las mallas para objetos poligonales en el juego estén creadas con otra herramienta y exportadas a un formato específico. Lo mismo ocurre con las texturas y otros elementos: aunque un mapa de bits plano no comprimido se pueda escribir con la mayoría de las herramientas y comprender en la mayoría de las API de gráficos, podría resultar extremadamente ineficaz en tu juego. A continuación te guiamos en los pasos básicos para cargar tres tipos distintos de recursos gráficos para usar con Direct3D: mallas (modelos), texturas (mapas de bits) y objetos de sombreador compilados.

## <a name="what-you-need-to-know"></a>Aspectos que debe saber


### <a name="technologies"></a>Tecnologías

-   Biblioteca de modelos de procesamiento paralelo (ppltasks.h)

### <a name="prerequisites"></a>Prerrequisitos

-   Comprender Windows Runtime básico
-   Comprender tareas asincrónicas
-   Comprender los conceptos básicos de programación de gráficos 3D

Esta muestra también incluye tres archivos de código para cargar y administrar recursos. En este tema, incluimos los objetos de código definidos en estos archivos.

-   BasicLoader.h/.cpp 
-   BasicReaderWriter.h/.cpp
-   DDSTextureLoader.h/.cpp

Puedes encontrar el código completo de estas muestras en los siguientes vínculos.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="complete-code-for-basicloader.md">Código completo para BasicLoader</a></p></td>
<td align="left"><p>Código completo de una clase y sus métodos para convertir y cargar objetos de mallas para gráficos en la memoria.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="complete-code-for-basicreaderwriter.md">Código completo para BasicReaderWriter</a></p></td>
<td align="left"><p>Código completo de una clase y sus métodos para leer y escribir archivos de datos binarios en general. Usado por la clase <a href="complete-code-for-basicloader.md">BasicLoader</a>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="complete-code-for-ddstextureloader.md">Código completo para DDSTextureLoader</a></p></td>
<td align="left"><p>Código completo de una clase y método para cargar una textura DDS de la memoria.</p></td>
</tr>
</tbody>
</table>

 

## <a name="instructions"></a>Instructions

### <a name="asynchronous-loading"></a>Carga asincrónica

La carga asincrónica se controla con la plantilla **task** de la Biblioteca de modelos de procesamiento paralelo (PPL). Una plantilla **task** contiene una llamada de método seguida por una expresión lambda que procesa los resultados de la llamada asincrónica después de completarse y, normalmente, respeta el siguiente formato:

`task<generic return type>(async code to execute).then((parameters for lambda){ lambda code contents });`.

Es posible encadenar tareas juntas mediante la sintaxis **.then()**. Cuando se completa una operación, puede ejecutarse otra operación asincrónica que depende de los resultados de la operación previa. De este modo, puedes cargar, convertir y administrar activos complejos en subprocesos independientes, casi invisibles para el jugador.

Para más información, consulta [Programación asincrónica en C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

Ahora veamos la estructura básica de la declaración y creación de un método asincrónico para cargar archivos, **ReadDataAsync**.

```cpp
#include <ppltasks.h>

// ...
concurrency::task<Platform::Array<byte>^> ReadDataAsync(
        _In_ Platform::String^ filename);

// ...

using concurrency;

task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}
```

En este código, cuando tu código llama al método **ReadDataAsync** definido arriba, se crea una tarea para leer un búfer del sistema de archivos. Cuando termina, una tarea encadenada toma el búfer y transmite los bytes de ese búfer en una matriz, mediante el tipo [**DataReader**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) estático.

```cpp
m_basicReaderWriter = ref new BasicReaderWriter();

// ...
return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
      // Perform some operation with the data when the async load completes.          
    });
```

Esta es la llamada que haces a **ReadDataAsync**. Cuando termina, tu código recibe una matriz de bytes leídos desde el archivo proporcionado. Dado que **ReadDataAsync** se define como una tarea, puedes usar una expresión lambda para realizar una operación específica cuando se devuelve la matriz de bytes; por ejemplo, pasar esos datos de bytes a una función DirectX que pueda usarlos.

Si tu juego es lo suficientemente simple; carga los recursos con un método como este, cuando el usuario inicie el juego. Puedes hacer esto antes de iniciar el bucle principal del juego desde algún punto de la secuencia de llamada de la implementación [**IFrameworkView::Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run). Nuevamente llamas a los métodos de carga de recursos de manera asincrónica, para que el juego pueda iniciarse más rápido y el jugador no tenga que esperar hasta que termine la carga para sus primeras interacciones.

¡Pero no quieres que el juego propiamente dicho empiece con cargas asincrónicas sin completar! Crea algún método de señalización de carga completa, como un campo específico, y usa las expresiones lambdas en tus métodos de carga para que se establezca la señal cuando terminen. Comprueba la variable antes de iniciar cualquier componente que use esos recursos cargados.

En este ejemplo, se usan métodos asincrónicos definidos en BasicLoader.cpp para cargar sombreadores, una malla y una textura, cuando se inicia el juego. Tenga en cuenta que establece un campo específico en el objeto Game **,\_m loadingComplete**, cuando finalizan todos los métodos de carga.

```cpp
void ResourceLoading::CreateDeviceResources()
{
    // DirectXBase is a common sample class that implements a basic view provider. 
    
    DirectXBase::CreateDeviceResources(); 

    // ...

    // This flag will keep track of whether or not all application
    // resources have been loaded.  Until all resources are loaded,
    // only the sample overlay will be drawn on the screen.
    m_loadingComplete = false;

    // Create a BasicLoader, and use it to asynchronously load all
    // application resources.  When an output value becomes non-null,
    // this indicates that the asynchronous operation has completed.
    BasicLoader^ loader = ref new BasicLoader(m_d3dDevice.Get());

    auto loadVertexShaderTask = loader->LoadShaderAsync(
        "SimpleVertexShader.cso",
        nullptr,
        0,
        &m_vertexShader,
        &m_inputLayout
        );

    auto loadPixelShaderTask = loader->LoadShaderAsync(
        "SimplePixelShader.cso",
        &m_pixelShader
        );

    auto loadTextureTask = loader->LoadTextureAsync(
        "reftexture.dds",
        nullptr,
        &m_textureSRV
        );

    auto loadMeshTask = loader->LoadMeshAsync(
        "refmesh.vbo",
        &m_vertexBuffer,
        &m_indexBuffer,
        nullptr,
        &m_indexCount
        );

    // The && operator can be used to create a single task that represents
    // a group of multiple tasks. The new task's completed handler will only
    // be called once all associated tasks have completed. In this case, the
    // new task represents a task to load various assets from the package.
    (loadVertexShaderTask && loadPixelShaderTask && loadTextureTask && loadMeshTask).then([=]()
    {
        m_loadingComplete = true;
    });

    // Create constant buffers and other graphics device-specific resources here.
}
```

Observa también que las tareas se agregan con el operador && ,como la expresión lambda que establece que la marca de carga completa se desencadene solo cuando finalicen todas las tareas. Ten en cuenta que si tienes varias marcas, pueden darse condiciones de carrera. Por ejemplo, cuando la expresión lambda establece dos marcas secuencialmente en el mismo valor, otro subproceso podrá ver solo la primera marca, si las examina antes de establecerse la segunda.

Hemos visto cómo cargar archivos de recursos de manera asincrónica. Las cargas sincrónicas de archivos son mucho más simples. Puedes encontrar ejemplos en [Código completo para BasicReaderWriter](complete-code-for-basicreaderwriter.md) y [Código completo para BasicLoader](complete-code-for-basicloader.md).

Obviamente, distintos tipos de activos y recursos a menudo requieren un procesamiento o conversión adicionales, antes de poder usarse en la canalización de gráficos. Echemos un vistazo a tres tipos específicos de recursos: mallas, texturas y sombreadores.

### <a name="loading-meshes"></a>Cargar mallas

Las mallas son datos de vértices, ya sean generados en procedimientos por el código dentro del juego, o bien exportados a un archivo desde otra aplicación (como 3DStudio MAX o Alias WaveFront) o herramienta. Estas mallas representan los modelos del juego; desde primitivos simples, como cubos y esferas, hasta automóviles, casas y caracteres. Por lo general, también tienen color y datos de animación, según su formato. Nos concentraremos en mallas que contienen solo datos de vértices.

Para cargar una malla de manera correcta, debes conocer el formato de los datos del archivo para la malla. Nuestro tipo **BasicReaderWriter** anterior simplemente lee los datos como un flujo de bytes. No sabe que los datos de bytes representan una malla, mucho menos detecta un formato específico de malla exportado por otra aplicación. Debes realizar las conversiones cuando guardas datos de mallas en la memoria.

(Siempre trata de empaquetar datos de activos en un formato lo más parecido posible a la representación interna. De esta forma, podrás reducir la utilización de recursos y ahorrar tiempo).

Tomemos los datos de bytes del archivo de la malla. El formato del ejemplo supone que el archivo tiene un formato específico de muestra con el sufijo .vbo. (De nuevo, este formato no es el mismo que el formato VBO de OpenGL.) Cada vértice se asigna al tipo **BasicVertex**, que es una estructura definida en el código de la herramienta de conversión obj2vbo. El diseño de los datos de vértices en el archivo .vbo es similar al siguiente:

-   Los primeros 32 bits (4 bytes) del flujo de datos contiene el número de vértices (numVertices) de la malla, representado como un valor uint32.
-   Los siguientes 32 bits (4 bytes) del flujo de datos contiene el número de índices (numIndices) de la malla, representado como un valor uint32.
-   Después, los bits subsiguientes \* (numVertices sizeof (**BasicVertex**)) contienen los datos del vértice.
-   Los últimos bytes \* (numIndices 16) de datos contienen los datos del índice, que se representan como una secuencia de valores UInt16.

Lo importante es que conozcas el diseño del nivel de bits de los datos de la malla que has cargado. También asegúrate de ser coherente con endianness. Todas las plataformas de Windows 8 son little-endian.

En el ejemplo, llamas a un método, CreateMesh, a partir del método **LoadMeshAsync**, para que interprete este nivel de bits.

```cpp
task<void> BasicLoader::LoadMeshAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ meshData)
    {
        CreateMesh(
            meshData->Data,
            vertexBuffer,
            indexBuffer,
            vertexCount,
            indexCount,
            filename
            );
    });
}
```

**CreateMesh** interpreta los datos de bytes cargados desde el archivo y crea un búfer de vértices y un búfer de índice para la malla pasando las listas de vértices e índices, respectivamente, a [**ID3D11Device:: CreateBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) y\_especificando el búfer de vértices\_de enlace D3D11\_o el búfer de\_\_índice\_de D3D11. Este código se usa en **BasicLoader**:

```cpp
void BasicLoader::CreateMesh(
    _In_ byte* meshData,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount,
    _In_opt_ Platform::String^ debugName
    )
{
    // The first 4 bytes of the BasicMesh format define the number of vertices in the mesh.
    uint32 numVertices = *reinterpret_cast<uint32*>(meshData);

    // The following 4 bytes define the number of indices in the mesh.
    uint32 numIndices = *reinterpret_cast<uint32*>(meshData + sizeof(uint32));

    // The next segment of the BasicMesh format contains the vertices of the mesh.
    BasicVertex* vertices = reinterpret_cast<BasicVertex*>(meshData + sizeof(uint32) * 2);

    // The last segment of the BasicMesh format contains the indices of the mesh.
    uint16* indices = reinterpret_cast<uint16*>(meshData + sizeof(uint32) * 2 + sizeof(BasicVertex) * numVertices);

    // Create the vertex and index buffers with the mesh data.

    D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
    vertexBufferData.pSysMem = vertices;
    vertexBufferData.SysMemPitch = 0;
    vertexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC vertexBufferDesc(numVertices * sizeof(BasicVertex), D3D11_BIND_VERTEX_BUFFER);

    m_d3dDevice->CreateBuffer(
            &vertexBufferDesc,
            &vertexBufferData,
            vertexBuffer
            );
    
    D3D11_SUBRESOURCE_DATA indexBufferData = {0};
    indexBufferData.pSysMem = indices;
    indexBufferData.SysMemPitch = 0;
    indexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC indexBufferDesc(numIndices * sizeof(uint16), D3D11_BIND_INDEX_BUFFER);
    
    m_d3dDevice->CreateBuffer(
            &indexBufferDesc,
            &indexBufferData,
            indexBuffer
            );
  
    if (vertexCount != nullptr)
    {
        *vertexCount = numVertices;
    }
    if (indexCount != nullptr)
    {
        *indexCount = numIndices;
    }
}
```

Normalmente creas un par de búferes de vértices e índices para todas las mallas del juego. Puedes elegir dónde y cuándo cargar las mallas. Si tienes varias mallas, quizás quieres cargar algunas del disco en puntos específicos del juego; por ejemplo, durante estados de carga predefinidos específicos.  Para mallas grandes, como datos de terreno, puedes transmitir los vértices de una memoria caché; pero este es un procedimiento más complejo y no lo incluimos en este tema.

Recuerda que debes conocer el formato de los datos de vértices. Hay muchísimas formas de representar datos de vértices en las herramientas que se usan para crear modelos. También hay muchas formas de representar el diseño de entrada de los datos de vértices en Direct3D, como franjas y listas de triángulos. Para obtener más información sobre los datos de vértices, consulta [Introducción a los búferes en Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro) y [Primitivos](https://docs.microsoft.com/windows/desktop/direct3d9/primitives).

Echemos un vistazo ahora a la carga de texturas.

### <a name="loading-textures"></a>Carga de texturas

El activo más común en un juego, y el que comprime la mayoría de los archivos en disco y memoria, son las texturas. Al igual que las mallas, las texturas pueden tener varios formatos y, cuando las cargas, las conviertes a un formato que Direct3D pueda usar. Las texturas también vienen en una amplia variedad de tipos para crear distintos efectos. Los niveles de MIP para texturas se pueden usar para mejorar la apariencia y el rendimiento de objetos distantes; los mapas de luces y suciedad se usan para efectos de capa y detalles encima de una textura base, y los mapas normales se usan en cálculos de iluminación por píxel. En un juego moderno, una escena común puede tener miles de texturas individuales y el código debe administrar a todas de manera efectiva.

También al igual que las mallas, hay varios formatos específicos para que el uso de la memoria sea más eficaz. Dado que las texturas fácilmente consumen una amplia parte de la memoria de GPU (y del sistema), por lo general se comprimen de alguna forma. No es necesario que comprimas las texturas del juego. Puedes usar cualquier algoritmo de compresión y descompresión, siempre que proporciones los datos a los sombreadores de Direct3D en un formato que les resulte comprensible (como un mapa de bits [**Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)).

Direct3D proporciona soporte para los algoritmos de compresión de texturas DXT, aunque no todos los formatos DXT son compatibles con el hardware gráfico del jugador. Los archivos DDS contienen texturas DXT (y otros formatos de compresión de texturas) y llevan el sufijo .dds.

Un archivo DDS es un archivo binario que contiene la siguiente información:

-   Un DWORD (número mágico) con el valor de código de cuatro caracteres 'DDS ' (0x20534444).

-   Una descripción de los datos del archivo.

    Los datos se describen con una descripción de encabezado mediante el [**encabezado DDS\_**](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-header); el formato de píxel se define mediante los píxeles [**DDS\_**](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-pixelformat). Tenga en cuenta que el **encabezado\_DDS** y las estructuras de **PIXELFORMAT de DDS\_** REEMPLAZAn las estructuras DDSURFACEDESC2, DDSCAPS2 y DDPIXELFORMAT de DirectDraw 7 desusadas. **El\_encabezado DDS** es el equivalente binario de DDSURFACEDESC2 y DDSCAPS2. **Los\_PIXELFORMAT de DDS** son el equivalente binario de DDPIXELFORMAT.

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    ```

    Si el valor de **dwFlags** en [**DDS\_PIXELFORMAT**](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-pixelformat) se establece en DDPF\_FourCC y **dwFourCC** se establece en "contenido DX10", se presentará una estructura de [**\_\_encabezado DDS adicional DXT10**](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-header-dxt10) para acomodar las matrices de textura o los formatos de DXGI que no se pueden expresar como un formato de píxel RGB, como los formatos de punto flotante, los formatos sRGB, etc. Cuando la **estructura\_DXT10\_del encabezado DDS** esté presente, toda la descripción de los datos tendrá el siguiente aspecto.

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    DDS_HEADER_DXT10    header10;
    ```

-   Puntero a una matriz de bytes que contiene los datos de la superficie principal.
    ```cpp
    BYTE bdata[]
    ```

-   Puntero a una matriz de bytes que contiene las superficies restantes, como los niveles de mapas MIP, caras de un mapa de cubos, profundidades de una textura de volumen. Sigue estos vínculos para obtener más información sobre el diseño de un archivo DDS para: una [textura](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-file-layout-for-textures), un [mapa de cubos](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-file-layout-for-cubic-environment-maps) o una [textura de volumen](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-file-layout-for-volume-textures).

    ```cpp
    BYTE bdata2[]
    ```

Muchas herramientas exportan al formato DDS. Si no tienes una herramienta que exporte la textura a este formato, piensa en crear una. Para más información sobre el formato DDS y cómo usarlo en tu código, consulta [Guía de programación para DDS](https://docs.microsoft.com/windows/desktop/direct3ddds/dx-graphics-dds-pguide). En nuestro ejemplo, usaremos DDS.

Al igual que con otros tipos de recursos, lees los datos de un archivo como un flujo de bytes. Cuando termina la tarea de carga, la llamada a lambda ejecuta código (el método **CreateTexture**) para procesar el flujo de bytes en un formato que Direct3D pueda usar.

```cpp
task<void> BasicLoader::LoadTextureAsync(
    _In_ Platform::String^ filename,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(
            GetExtension(filename) == "dds",
            textureData->Data,
            textureData->Length,
            texture,
            textureView,
            filename
            );
    });
}
```

En el fragmento de código anterior, la expresión lambda comprueba si el nombre de archivo tiene una extensión de "dds". Si la tiene, supones que es una textura DDS. De lo contrario, usa las API de Windows Imaging Component (WIC) para saber cuál es el formato y decodificar los datos como un mapa de bits. De una u otra forma, el resultado será un mapa de bits [**Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d) (o un error).

```cpp
void BasicLoader::CreateTexture(
    _In_ bool decodeAsDDS,
    _In_reads_bytes_(dataSize) byte* data,
    _In_ uint32 dataSize,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView,
    _In_opt_ Platform::String^ debugName
    )
{
    ComPtr<ID3D11ShaderResourceView> shaderResourceView;
    ComPtr<ID3D11Texture2D> texture2D;

    if (decodeAsDDS)
    {
        ComPtr<ID3D11Resource> resource;

        if (textureView == nullptr)
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                nullptr
                );
        }
        else
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                &shaderResourceView
                );
        }

        resource.As(&texture2D);
    }
    else
    {
        if (m_wicFactory.Get() == nullptr)
        {
            // A WIC factory object is required in order to load texture
            // assets stored in non-DDS formats.  If BasicLoader was not
            // initialized with one, create one as needed.
            CoCreateInstance(
                    CLSID_WICImagingFactory,
                    nullptr,
                    CLSCTX_INPROC_SERVER,
                    IID_PPV_ARGS(&m_wicFactory));
        }

        ComPtr<IWICStream> stream;
        m_wicFactory->CreateStream(&stream);

        stream->InitializeFromMemory(
                data,
                dataSize);

        ComPtr<IWICBitmapDecoder> bitmapDecoder;
        m_wicFactory->CreateDecoderFromStream(
                stream.Get(),
                nullptr,
                WICDecodeMetadataCacheOnDemand,
                &bitmapDecoder);

        ComPtr<IWICBitmapFrameDecode> bitmapFrame;
        bitmapDecoder->GetFrame(0, &bitmapFrame);

        ComPtr<IWICFormatConverter> formatConverter;
        m_wicFactory->CreateFormatConverter(&formatConverter);

        formatConverter->Initialize(
                bitmapFrame.Get(),
                GUID_WICPixelFormat32bppPBGRA,
                WICBitmapDitherTypeNone,
                nullptr,
                0.0,
                WICBitmapPaletteTypeCustom);

        uint32 width;
        uint32 height;
        bitmapFrame->GetSize(&width, &height);

        std::unique_ptr<byte[]> bitmapPixels(new byte[width * height * 4]);
        formatConverter->CopyPixels(
                nullptr,
                width * 4,
                width * height * 4,
                bitmapPixels.get());

        D3D11_SUBRESOURCE_DATA initialData;
        ZeroMemory(&initialData, sizeof(initialData));
        initialData.pSysMem = bitmapPixels.get();
        initialData.SysMemPitch = width * 4;
        initialData.SysMemSlicePitch = 0;

        CD3D11_TEXTURE2D_DESC textureDesc(
            DXGI_FORMAT_B8G8R8A8_UNORM,
            width,
            height,
            1,
            1
            );

        m_d3dDevice->CreateTexture2D(
                &textureDesc,
                &initialData,
                &texture2D);

        if (textureView != nullptr)
        {
            CD3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc(
                texture2D.Get(),
                D3D11_SRV_DIMENSION_TEXTURE2D
                );

            m_d3dDevice->CreateShaderResourceView(
                    texture2D.Get(),
                    &shaderResourceViewDesc,
                    &shaderResourceView);
        }
    }


    if (texture != nullptr)
    {
        *texture = texture2D.Detach();
    }
    if (textureView != nullptr)
    {
        *textureView = shaderResourceView.Detach();
    }
}
```

Cuando este código se completa, tienes [**Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d) en la memoria, cargada desde un archivo de imagen. Al igual que con las mallas, probablemente tengas varias texturas en el juego y en una escena determinada. Piensa en crear memorias caché para texturas de acceso frecuente por escena o por nivel, en lugar de cargarlas a todas cuando el juego o nivel se inician.

(Puedes analizar con mayor profundidad el método **CreateDDSTextureFromMemory** llamado en la muestra anterior, en [Código completo para DDSTextureLoader](complete-code-for-ddstextureloader.md)).

También pueden asignarse texturas individuales o "máscaras" de texturas a superficies o polígonos de mallas específicos. Estos datos de asignación generalmente se exportan con la herramienta que el intérprete o el diseñador usaron para crear el modelo y las texturas. Asegúrate de capturar también esta información cuando cargues los datos exportados, porque la necesitarás para asignar las texturas correctas a las superficies correspondientes cuando realices el sombreado de fragmento.

### <a name="loading-shaders"></a>Cargar sombreadores

Los sombreadores son archivos de lenguaje de sombreado de alto nivel (HLSL) compilados, que se cargan en la memoria y se invocan en etapas específicas de la canalización de gráficos. Los sombreadores más comunes y esenciales son los de vértices y píxeles, que procesan los vértices individuales de la malla y los píxeles de la ventanilla de la escena, respectivamente. El código HLSL se ejecuta para transformar la geometría, aplicar texturas y efectos de iluminación, y realizar un procesamiento posterior en la escena representada.

Un juego Direct3D puede tener distintos sombreadores, cada uno compilado en un archivo CSO (Compiled Shader Object, .cso) independiente. Por lo general, no hay tantos que necesites cargar dinámicamente y, en la mayoría de los casos, puedes cargarlos cuando se inicia el juego o por nivel (como un sombreador para efectos de lluvia).

El código de la clase **BasicLoader** proporciona una cantidad de sobrecargas para distintos sombreadores, incluidos los de casco, píxeles, geometría y vértices. El siguiente código incluye sombreadores de píxeles como ejemplo. (Encontrarás el código completo en [Código completo para BasicLoader](complete-code-for-basicloader.md)).

```cpp
concurrency::task<void> LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    );

// ...

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        
       m_d3dDevice->CreatePixelShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader);
    });
}

```

En este ejemplo, se usa la instancia de **BasicReaderWriter** (**m\_BasicReaderWriter**) para leer en el archivo objeto de sombreador compilado (. CSO) proporcionado como un flujo de bytes. Cuando la tarea termina, la expresión lambda llama a [**ID3D11Device::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) con los datos de bytes cargados desde el archivo. La devolución de llamada debe establecer una marca que indique que la carga se realizó correctamente y el código debe comprobar esta marca antes de ejecutar el sombreador.

Los sombreadores de vértices son un poco más complejos. Para un sombreador de vértices, también cargas un diseño de entrada independiente que define los datos de vértices. El siguiente código se usa para cargar asincrónicamente un sombreador de vértices junto con un diseño de entrada de vértices personalizado. Asegúrate de que la información de vértices que cargas desde las mallas pueda representarse en este diseño de entrada de manera correcta.

Creemos el diseño de entrada antes de cargar el sombreador de vértices.

```cpp
void BasicLoader::CreateInputLayout(
    _In_reads_bytes_(bytecodeSize) byte* bytecode,
    _In_ uint32 bytecodeSize,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC* layoutDesc,
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11InputLayout** layout
    )
{
    if (layoutDesc == nullptr)
    {
        // If no input layout is specified, use the BasicVertex layout.
        const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
        {
            { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
        };

        m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                bytecode,
                bytecodeSize,
                layout);
    }
    else
    {
        m_d3dDevice->CreateInputLayout(
                layoutDesc,
                layoutDescNumElements,
                bytecode,
                bytecodeSize,
                layout);
    }
}
```

En este diseño particular, cada vértice tiene los siguientes datos procesados por el sombreador de vértices:

-   Una posición de coordenadas 3D (x, y, z) en el espacio de coordenadas del modelo, representada como un trío de valores de punto flotante de 32 bits.
-   Un vector normal para el vértice, también representado como tres valores de punto flotante de 32 bits.
-   Un valor de coordenada para la textura 2D transformada (u, v), representado como un par de valores flotantes de 32 bits.

Estos elementos de entrada por vértice se denominan [semántica de HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics). Son un conjunto de registros definidos que se usa para pasar datos desde y hacia el objeto de sombreador compilado. La canalización ejecuta el sombreador de vértices una vez para cada vértice en la malla que cargaste. La semántica define la entrada al sombreador de vértices (y la salida de él) a medida que se ejecuta y proporciona estos datos para los cálculos por vértice en el código HLSL del sombreador.

Ahora carga el objeto del sombreador de vértices.

```cpp
concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11VertexShader** shader,
        _Out_opt_ ID3D11InputLayout** layout
        );

// ...

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11VertexShader** shader,
    _Out_opt_ ID3D11InputLayout** layout
    )
{
    // This method assumes that the lifetime of input arguments may be shorter
    // than the duration of this task.  In order to ensure accurate results, a
    // copy of all arguments passed by pointer must be made.  The method then
    // ensures that the lifetime of the copied data exceeds that of the task.

    // Create copies of the layoutDesc array as well as the SemanticName strings,
    // both of which are pointers to data whose lifetimes may be shorter than that
    // of this method's task.
    shared_ptr<vector<D3D11_INPUT_ELEMENT_DESC>> layoutDescCopy;
    shared_ptr<vector<string>> layoutDescSemanticNamesCopy;
    if (layoutDesc != nullptr)
    {
        layoutDescCopy.reset(
            new vector<D3D11_INPUT_ELEMENT_DESC>(
                layoutDesc,
                layoutDesc + layoutDescNumElements
                )
            );

        layoutDescSemanticNamesCopy.reset(
            new vector<string>(layoutDescNumElements)
            );

        for (uint32 i = 0; i < layoutDescNumElements; i++)
        {
            layoutDescSemanticNamesCopy->at(i).assign(layoutDesc[i].SemanticName);
        }
    }

    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
       m_d3dDevice->CreateVertexShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader);

        if (layout != nullptr)
        {
            if (layoutDesc != nullptr)
            {
                // Reassign the SemanticName elements of the layoutDesc array copy to point
                // to the corresponding copied strings. Performing the assignment inside the
                // lambda body ensures that the lambda will take a reference to the shared_ptr
                // that holds the data.  This will guarantee that the data is still valid when
                // CreateInputLayout is called.
                for (uint32 i = 0; i < layoutDescNumElements; i++)
                {
                    layoutDescCopy->at(i).SemanticName = layoutDescSemanticNamesCopy->at(i).c_str();
                }
            }

            CreateInputLayout(
                bytecode->Data,
                bytecode->Length,
                layoutDesc == nullptr ? nullptr : layoutDescCopy->data(),
                layoutDescNumElements,
                layout);   
        }
    });
}

```

En este código, después de leer los datos de bytes para el archivo CSO del sombreador de vértices, creas el sombreador de vértices llamando a [**ID3D11Device::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader). A continuación, creas el diseño de entrada para el sombreador en la misma expresión lambda.

Otros tipos de sombreador, como los sombreadores de casco y geometría, también pueden requerir una configuración específica. Encontrarás el código completo para una variedad de métodos de carga de sombreadores en [Código completo para BasicLoader](complete-code-for-basicloader.md) y en [Muestra de carga de recursos de Direct3D]( https://code.msdn.microsoft.com/windowsapps/Direct3D-Resource-Loading-25406148).

## <a name="remarks"></a>Comentarios

Llegados a este punto, deberías comprender y poder crear o modificar métodos para cargar asincrónicamente activos y recursos comunes para juegos, como mallas, texturas y sombreadores compilados.

## <a name="related-topics"></a>Temas relacionados

* [Muestra de carga de recursos de Direct3D]( https://code.msdn.microsoft.com/windowsapps/Direct3D-Resource-Loading-25406148)
* [Código completo para BasicLoader](complete-code-for-basicloader.md)
* [Código completo para BasicReaderWriter](complete-code-for-basicreaderwriter.md)
* [Código completo para DDSTextureLoader](complete-code-for-ddstextureloader.md)

 

 




