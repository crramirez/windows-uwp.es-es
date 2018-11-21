---
author: joannaleecy
title: Configuración
description: Aprende a ensamblar la canalización de representación para mostrar gráficos. Representación de juego, configurar y preparar los datos.
ms.assetid: 7720ac98-9662-4cf3-89c5-7ff81896364a
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, juegos, representar
ms.localizationpriority: medium
ms.openlocfilehash: 134c9005a796f52fb61ba628c0a85c8dbd875442
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7439967"
---
# <a name="rendering-framework-ii-game-rendering"></a>Marco de representación II: representación de juego

En [Marco de representación](tutorial--assembling-the-rendering-pipeline.md), hemos analizado cómo se puede tomar la información de las escenas y mostrarla en la pantalla de visualización. Ahora, vamos a dar un paso atrás y aprender a preparar los datos para la representación.

>[!Note]
>Si no has descargado el código del juego más reciente de este ejemplo, ve a [Muestra de juego de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Ten en cuenta que este ejemplo forma parte de una gran colección de ejemplos de funciones para UWP. Si necesitas instrucciones sobre cómo descargar el ejemplo, consulta [Obtener las muestras de UWP desde GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Resumen rápido del objetivo. Es necesario comprender cómo se debe configurar un marco de representación básico para mostrar la salida de gráficos para un juego DirectX de UWP. Podemos agruparlos aproximadamente en estos tres pasos.

 1. Establecer una conexión con nuestra interfaz gráfica
 2. Preparación: Crear los recursos que necesitamos para dibujar los gráficos
 3. Mostrar los gráficos: Representar un fotograma

[Marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md) explica cómo se representan los gráficos, en los pasos 1 y 3. 

Este artículo explica cómo configurar otras partes del fotograma y preparar los datos necesarios antes de que puede producirse la representación, que es el paso 2 del proceso.

## <a name="design-the-renderer"></a>Diseñar el representador

El representador es responsable de crear y mantener todos los objetos D3D11 y D2D usados para generar los elementos visuales del juego. La clase __GameRenderer__ es el representador de este juego de muestra y está diseñada para satisfacer las necesidades de representación del juego.

Estos son algunos conceptos que puedes usar para intentar diseñar el representador de tu juego:
* Dado que las API de Direct3D 11 están definidas como API [COM](https://msdn.microsoft.com/library/windows/desktop/ms694363.aspx), debes proporcionar referencias de [ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class) a los objetos definidos por estas API. Estos objetos se liberan automáticamente cuando su última referencia sale del ámbito cuando la aplicación finaliza. Para obtener más información, consulta [ComPtr](https://github.com/Microsoft/DirectXTK/wiki/ComPtr). Ejemplos de estos objetos: búferes de constantes, objetos de sombreador - [sombreador de vértices](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders), [sombreador de píxeles](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders) y objetos de recursos de sombreador.
* Los búferes de constantes se definen en esta clase para contener diversos datos necesarios para la representación.
    * Utiliza varios búferes de constantes con diferentes frecuencias para reducir la cantidad de datos que deben enviarse a la GPU por fotograma. Este ejemplo separa las constantes en diferentes búferes, según la frecuencia con la que deben actualizarse. Este es el procedimiento recomendado para la programación de Direct3D. 
    * En esta muestra de juego se definen 4 búferes de constantes.
        1. __m\_constantBufferNeverChanges__ contiene los parámetros de iluminación. Se establece una vez en el método __FinalizeCreateGameDeviceResources__ y nunca vuelve a cambiar.
        2. __m\_constantBufferChangeOnResize__ contiene la matriz de proyección. La matriz de proyección depende del tamaño y la relación de aspecto de la ventana. Se establece en [__CreateWindowSizeDependentResources__](#createwindowsizedependentresources-method) y, a continuación, se actualiza después de que los recursos se cargan en el método [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method). Si la representación es en 3D, también se cambia dos veces por fotograma.
        3. __m\_constantBufferChangesEveryFrame__ contiene la matriz de vista. Esta matriz depende de la posición de la cámara y la dirección de vista (lo normal para la proyección) y cambia solamente una vez por fotograma en el método __Render__. Esto se explicó anteriormente en __Marco de representación I: Introducción a la representación__, en el método [__GameRenderer::Render__](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method).
        4. __m\_constantBufferChangesEveryPrim__ contiene las propiedades de la matriz de modelos y los materiales de cada primitivo. La matriz de modelos transforma los vértices desde las coordenadas locales en coordenadas globales. Estas constantes son específicas de cada primitivo y se actualizan para cada llamada de dibujo. Esto se explicó anteriormente en __Marco de representación I: Introducción a la representación__, en el tutorial [Representación de primitivos](tutorial--assembling-the-rendering-pipeline.md#primitive-rendering).
* Los objetos de recursos del sombreador que guardan las texturas para los primitivos se definen también en esta clase.
    * Algunas texturas están predefinidas ([DDS](https://msdn.microsoft.com/library/windows/desktop/bb943991.aspx) es un formato de archivo que puede usarse para almacenar texturas comprimidas y sin comprimir. Las texturas DDS se usan para las paredes y el piso del mundo, así como esferas de munición).
    * En esta muestra de juego los objetos de recursos de sombreador son los siguientes: __m\_sphereTexture__, __m\_cylinderTexture__, __m\_ceilingTexture__, __m\_floorTexture__ y __m\_wallsTexture__.
* Los objetos del sombreador se definen en esta clase para calcular nuestros primitivos y texturas. 
    * En esta muestra de juego, los objetos de sombreador son __m\_vertexShader__, __m\_vertexShaderFlat__, __m\_pixelShader__ y __m\_pixelShaderFlat__.
    * El sombreador de vértices procesa la iluminación básica y los primitivos y el sombreador de píxeles (en ocasiones llamado sombreador de fragmentos), procesa las texturas y cualquier efecto por píxel.
    * Existen dos versiones de estos sombreadores (regular y plano) para representar distintos primitivos. La razón por la que tenemos diferentes versiones es que las versiones planas son mucho más sencillas y no crean resaltados especulares ni ningún efecto de iluminación por píxel. Se usan en paredes y logran que las representaciones sean más rápidas en dispositivos de baja potencia.

## <a name="gamerendererh"></a>GameRenderer.h

Veamos ahora el código en el objeto de clase del representador de la muestra de juego y comparémoslo con la __Sample3DSceneRenderer.h__ proporcionada en la plantilla de la aplicación DirectX 11.

```cpp
// Class object handling the rendering of the game
ref class GameRenderer
{
internal:
    GameRenderer(const std::shared_ptr<DX::DeviceResources>& deviceResources);
    
    // Compared with Sample3DSceneRenderer.h in the DirectX 11 App template sample. 
    
    // These methods are present.
    void CreateDeviceDependentResources();
    void CreateWindowSizeDependentResources();
    void ReleaseDeviceDependentResources();
    void Render();

    // Added: Async related methods to prepare 3D game objects for rendering
    concurrency::task<void> CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game);
    void FinalizeCreateGameDeviceResources();
    concurrency::task<void> LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();
    // --- end of async related methods section

    // Added: Methods for rendering overlay
    Simple3DGameDX::IGameUIControl^ GameUIControl()  { return m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.left, m_gameInfoOverlayRect.top);
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.right, m_gameInfoOverlayRect.bottom);
    };
    bool GameInfoOverlayVisible() { return m_gameInfoOverlay->Visible(); }
    // --- end of rendering overlay section

    //...
    protected private:
    // Cached pointer to device resources.
    std::shared_ptr<DX::DeviceResources>                m_deviceResources;

    // ...

    // Shader resource objects
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_sphereTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_cylinderTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_ceilingTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_floorTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant Buffers
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferNeverChanges;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;

    // Texture sampler
    Microsoft::WRL::ComPtr<ID3D11SamplerState>          m_samplerLinear;

    // Shader objects: Vertex shaders and pixel shaders
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShader;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShader;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11InputLayout>           m_vertexLayout;
};
```

## <a name="constructor"></a>Constructor

A continuación, vamos a examinar el constructor __GameRenderer__ y a compararlo con el constructor __Sample3DSceneRenderer__ proporcionado en la plantilla de la aplicación DirectX 11.

```cpp
// Constructor method of the main rendering class object
GameRenderer::GameRenderer(const std::shared_ptr<DX::DeviceResources>& deviceResources) : //...
{
    // Compared with Sample3DSceneRenderer::Sample3DSceneRenderer in the DirectX 11 App template sample. 
    
    // Added: Create a new GameHud object to rendered text on the top left corner of the screen
    m_gameHud = ref new GameHud(
        deviceResources,
        "Windows platform samples",
        "DirectX first-person game sample"
        );
    //--- end of new GameHud object section
        
    // Added: Game info rendered as an overlay on the top right corner of the screen (eg. Hits, Shots, Time)    
    m_gameInfoOverlay = ref new GameInfoOverlay(deviceResources);
    //--- end of game info rendered as overlay section

    // These methods are present.
    CreateDeviceDependentResources();
    CreateWindowSizeDependentResources();
}
```

## <a name="create-and-load-directx-graphic-resources"></a>Crear y cargar recursos gráficos de DirectX

En la muestra de juego (y en la plantilla __DirectX 11 App (Universal Windows)__ de Visual Studio, la creación y la carga de recursos del juego se implementa mediante estos dos métodos, que se llaman desde el constructor __GameRenderer__:

* [__CreateDeviceDependentResources__](#createdevicedependentresources-method)
* [__CreateWindowSizeDependentResources__](#createwindowsizedependentresources-method)

## <a name="createdevicedependentresources-method"></a>Método CreateDeviceDependentResources

En la plantilla de la aplicación DirectX 11, este método se usa para cargar asíncronamente el sombreador de vértices y píxeles, crear el búfer de sombreador y de constante, y crear una malla con vértices que contengan la posición y la información de color. 

En el juego de muestra, estas operaciones de los objetos de escena se dividen en su lugar entre los métodos [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) y [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method). 

Para esta muestra de juego, ¿qué se incluye en este método?

* Variables instanciadas (__m\_gameResourcesLoaded__ = false y __m\_levelResourcesLoaded__ = "false") que indican si se han cargado recursos antes de pasar a representar, dado que estamos cargándolos asíncronamente. 
* Dado que la representación de HUD y superposición están en objetos de clase independientes, llama a los métodos __GameHud::CreateDeviceDependentResources__ y __GameInfoOverlay::CreateDeviceDependentResources__ aquí.

Este es el código para __GameRenderer::CreateDeviceDependentResources__.

```cpp
// This method is called in GameRenderer constructor when it's created in GameMain constructor.
void GameRenderer::CreateDeviceDependentResources()
{
    // instantiate variables that indicate if resources were loaded.
    m_gameResourcesLoaded = false;
    m_levelResourcesLoaded = false;

    // game HUD and overlay are design as separate class objects.
    m_gameHud->CreateDeviceDependentResources();
    m_gameInfoOverlay->CreateDeviceDependentResources();
}
```
La siguiente tabla enumera los métodos que se usan para crear y cargar recursos. __CreateGameDeviceResourcesAsync__ y __FinalizeCreateGameDeviceResources__ se agregan en el juego de muestra, para que los recursos se carguen asíncronamente.

|Plantilla original de aplicación DirectX 11           |Juego de muestra                                                                |
|-------------------------------------------|---------------------------------------------------------------------------|
|CreateDeviceDependentResources             |CreateDeviceDependentResources                                             |
|                                           | - CreateGameDeviceResourcesAsync (agregado)                                  |
|                                           | - FinalizeCreateGameDeviceResources (agregado)                               |
|CreateWindowSizeDependentResources         |CreateWindowSizeDependentResources                                         |

Antes de profundizar en los otros métodos que se usan para crear y cargar recursos, vamos primero a crear el representador y ver cómo encaja en el bucle del juego.

## <a name="create-the-renderer"></a>Crear el representador

El __GameRenderer__ se crea en el constructor de __GameMain__. También llama a los dos otros métodos, [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) y [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) que se agregan para ayudar a crear y cargar los recursos.

```cpp

GameMain::GameMain(const std::shared_ptr<DX::DeviceResources>& deviceResources) : // ...
{
    m_deviceResources->RegisterDeviceNotify(this);

    // These methods are used in the DirectX 11 App template to create the class objects used for rendering. 
    // But are replaced in this game sample with GameRenderer as shown below.
    // m_sceneRenderer = std::unique_ptr<Sample3DSceneRenderer>(new Sample3DSceneRenderer(m_deviceResources));
    // m_fpsTextRenderer = std::unique_ptr<SampleFpsTextRenderer>(new SampleFpsTextRenderer(m_deviceResources));
    
    // Creation of GameRenderer
    m_renderer = ref new GameRenderer(m_deviceResources);
    
    //...

     create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and in parallel all the necessary resources are loaded on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();
    
    //...
}
```

## <a name="creategamedeviceresourcesasync-method"></a>Método CreateGameDeviceResourcesAsync

__CreateGameDeviceResourcesAsync__ se llama desde el método constructor __GameMain__ del bucle __create\_task__, ya que estamos cargando los recursos del juego asíncronamente.
        
__CreateDeviceResourcesAsync__ es un método que ejecuta un conjunto distinto de tareas asíncronas para cargar los recursos del juego. Puesto que se espera que se ejecute en un subproceso independiente, solo tiene acceso a los métodos de dispositivo de Direct3D11 (los definidos en __ID3D11Device__), y no a los métodos de contexto de dispositivo (los definidos en __ID3D11DeviceContext__), por lo que no realiza ninguna representación.

El método __FinalizeCreateGameDeviceResources__ se ejecuta en el subproceso principal y carece de acceso a los métodos de contexto de dispositivo de Direct3D11.

En principio:
* Usa solamente los métodos __ID3D11Device__ de __CreateGameDeviceResourcesAsync__ porque son de subprocesamiento libre, lo que significa que se pueden ejecutar en cualquier subproceso. También se espera que no se ejecuten en el mismo subproceso en que se creó __GameRenderer__. 
* No uses métodos de __ID3D11DeviceContext__ aquí, porque deben ejecutarse en un solo subproceso y en el mismo subproceso que __GameRenderer__.
* Usa este método para crear búferes de constantes.
* Usa este método para cargar texturas (por ejemplo, los archivos .dds) y la información del sombreador (por ejemplo, los archivos .cso) en los [sombreadores](tutorial--assembling-the-rendering-pipeline.md#shaders).

Este método se usa para:
* Crear los 4 [búferes de constantes](tutorial--assembling-the-rendering-pipeline.md#buffer): __m\_constantBufferNeverChanges__, __m\_constantBufferChangeOnResize__, __m\_constantBufferChangesEveryFrame__ y __m\_ constantBufferChangesEveryPrim__
* Crear un objeto [sampler-state](tutorial--assembling-the-rendering-pipeline.md#sampler-state) que encapsula la información de muestreo de una textura
* Crear un grupo de tareas que contiene todas las tareas asíncronas creadas por el método. Espera a que finalicen todas estas tareas asíncronas y después llama a __FinalizeCreateGameDeviceResources__.
* Crea un cargador usando [Cargador básico](tutorial--assembling-the-rendering-pipeline.md#basicloader). Agrega las operaciones de carga asíncrona del cargador como tareas del grupo de tareas creado anteriormente.
* Los métodos como __BasicLoader::LoadShaderAsync__ y __BasicLoader::LoadTextureAsync__ se usan para cargar:
    * objetos de sombreador compilados (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso y PixelShaderFlat.cso). Para obtener más información, consulta [Diversos formatos de archivo de sombreador](tutorial--assembling-the-rendering-pipeline.md#various-shader-file-formats).
    * texturas específicas del juego (Assets\\seafloor.dds, metal_texture.dds, cellceiling.dds, cellfloor.dds, cellwall.dds).

```cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    // Create the device dependent game resources.
    // Only the d3dDevice is used in this method.  It is expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the d3dDevice are free-threaded and are safe while any methods
    // in the d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.
    m_game = game;

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    // Define D3D11_BUFFER_DESC.
    // For API ref, go to: https://msdn.microsoft.com/library/windows/desktop/ff476092.aspx
    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));
    
    bd.Usage = D3D11_USAGE_DEFAULT;
    // ...
    
    // Create the constant buffers: m_constantBufferNeverChanges, m_constantBufferChangeOnResize,
    // m_constantBufferChangesEveryFrame, m_constantBufferChangesEveryPrim
    // CreateBuffer is used to create one of these buffers: vertex buffer, index buffer, or 
    // shader-constant buffer. For CreateBuffer API ref info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476501.aspx
    
    DX::ThrowIfFailed(
        d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferNeverChanges) 
        );
    // ...
    
    // Define D3D11_SAMPLER_DESC. For API ref, go to: https://msdn.microsoft.com/library/windows/desktop/ff476207.aspx
    D3D11_SAMPLER_DESC sampDesc;

    // ZeroMemory fills a block of memory with zeros. 
    // For API ref, go to: https://msdn.microsoft.com/en-us/library/windows/desktop/aa366920(v=vs.85).aspx
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    // ...
    
    // Create a sampler-state object that encapsulates sampling information for a texture.
    // The sampler-state interface holds a description for sampler state that you can bind to any 
    // shader stage of the pipeline for reference by texture sample operations.
    DX::ThrowIfFailed(
        d3dDevice->CreateSamplerState(&sampDesc, &m_samplerLinear)
        );

    // Start the async tasks to load the shaders and textures (resources).
    
    // Load compiled shader objects (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso, and PixelShaderFlat.cso).
    // The BasicLoader class is used to convert and load common graphics resources, such as meshes, textures, 
    // and various shader objects into the constant buffers. 
    // For more info, go to: https://docs.microsoft.com/windows/uwp/gaming/complete-code-for-basicloader
    BasicLoader^ loader = ref new BasicLoader(d3dDevice);

    std::vector<task<void>> tasks;

    uint32 numElements = ARRAYSIZE(PNTVertexLayout);

    // Load shaders asynchronously with the shader and pixel data using the BasicLoader::LoadShaderAsync method
    // Push these method calls into a list of tasks
    tasks.push_back(loader->LoadShaderAsync("VertexShader.cso", PNTVertexLayout, numElements, &m_vertexShader, &m_vertexLayout));
    tasks.push_back(loader->LoadShaderAsync("VertexShaderFlat.cso", nullptr, numElements, &m_vertexShaderFlat, nullptr));
    tasks.push_back(loader->LoadShaderAsync("PixelShader.cso", &m_pixelShader));
    tasks.push_back(loader->LoadShaderAsync("PixelShaderFlat.cso", &m_pixelShaderFlat));

    // Make sure the previous versions are set to NULL before any of the textures are loaded.
    m_sphereTexture = nullptr;
    // ...

    // Load Game specific textures (Assets\\seafloor.dds, metal_texture.dds, cellceiling.dds, cellfloor.dds, cellwall.dds).
    // Push these method calls also into a list of tasks
    tasks.push_back(loader->LoadTextureAsync("Assets\\seafloor.dds", nullptr, &m_sphereTexture));
    // ...
    
    tasks.push_back(create_task([]()
    {
        // Simulate loading additional resources by introducing a delay.
        wait(GameConstants::InitialLoadingDelay);
    }));

    // Returns when all the async tasks for loading the shader and texture assets have completed.
    return when_all(tasks.begin(), tasks.end());
}
```

## <a name="finalizecreategamedeviceresources-method"></a>Método FinalizeCreateGameDeviceResources

Se llama al método __FinalizeCreateGameDeviceResources__ una vez que se completan todas las tareas de carga de recursos que están en el método __CreateGameDeviceResourcesAsync__. 

* Inicializar constantBufferNeverChanges con las posiciones de luz y color. Carga los datos iniciales en los búferes de constantes realizando una llamada de método de contexto de dispositivo a __ID3D11DeviceContext::UpdateSubresource__.
* Dado que los recursos cargados de forma asincrónica han terminado de cargarse, es el momento de asociarlos con los objetos del juego adecuados.
* Para cada objeto del juego, crea la malla y el material con las texturas que se han cargado. A continuación, asocia la malla y el material al objeto del juego.
* Para el objeto de juego de destino, la textura que se compone de anillos de colores concéntricos, con un valor numérico en la parte superior, no se carga desde un archivo de textura. En su lugar, se genera en procedimientos usando el código de __TargetTexture.cpp__. La clase __TargetTexture__ crea los recursos necesarios para dibujar la textura en un recurso de fuera de la pantalla en el momento de inicialización. La textura resultante se asocia con los objetos de juego de objetivo adecuados.

__FinalizeCreateGameDeviceResources__ y [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) comparten partes similares del código para estos elementos:
* Usa __SetProjParams__ para garantizar que la cámara tenga la matriz de proyección adecuada. Para obtener más información, visita [Cámaras y espacio en coordenadas](tutorial--assembling-the-rendering-pipeline.md#camera-and-coordinate-space).
* Controlar la rotación de pantalla multiplicando la matriz de rotación 3D por la matriz de proyección de la cámara. A continuación, actualiza el búfer de constantes __ConstantBufferChangeOnResize__ con la matriz de proyección resultante.
* Establece la variable global __m\_gameResourcesLoaded__ __booleano__ para indicar que los recursos se han cargado en los búferes, listos para el siguiente paso. Recuerda que primero inicializamos esta variable como __FALSE__ en el método constructor de __GameRenderer__, a través del método __GameRenderer::CreateDeviceDependentResources__. 
* Cuando este __m\_gameResourcesLoaded__ es __TRUE__, puede realizarse la representación de los objetos de escena. Esto se explicó anteriormente en el artículo __Marco de representación I: Introducción a la representación__, en el método [__GameRenderer::Render method__](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method).

```cpp
// When creating this sample game using the DirectX 11 App template, this method needs to be created.
// This new method is called from GameMain constructor in the .then loop.
// Make sure the 2D rendering is occurring on the same thread as the main rendering.
// Note: Helper class .h and .cpp files used in this method are located in the SharedContent/cpp/GameContent folder
void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now associate all the resources with the appropriate game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize constantBufferNeverChanges with the light positions and color.
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    auto d3dDevice = m_deviceResources->GetD3DDevice();
    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    // ...
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);

    // CPU copies data from memory (constantBufferNeverChanges) to a subresource 
    // created in non-mappable memory (m_constantBufferNeverChanges) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method. For UpdateSubresource API ref info, 
    // go to: https://msdn.microsoft.com/library/windows/desktop/ff476486.aspx
    // To learn more about what a subresource is, go to:
    // https://msdn.microsoft.com/library/windows/desktop/ff476901.aspx

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferNeverChanges.Get(),
        0,
        nullptr,
        &constantBufferNeverChanges,
        0,
        0
        );

    // For the objects that function as targets, they have two unique generated textures.
    // One version is used to show that they have never been hit and the other is 
    // used to show that they have been hit.
    // TargetTexture is a helper class to procedurally generate textures for game
    // targets. The class creates the necessary resources to draw the texture into 
    // an off screen resource at initialization time.

    TargetTexture^ textureGenerator = ref new TargetTexture(
        d3dDevice,
        m_deviceResources->GetD2DFactory(),
        m_deviceResources->GetDWriteFactory(),
        m_deviceResources->GetD2DDeviceContext()
        );

    // CylinderMesh is a class derived from MeshObject and creates a ID3D11Buffer of
    // vertices and indices to represent a canonical cylinder (capped at
    // both ends) that is positioned at the origin with a radius of 1.0,
    // a height of 1.0 and with its axis in the +Z direction.
    // In the game sample, there are various types of mesh types:
    // CylinderMesh (vertical rods), SphereMesh (balls that the player shoots), 
    // FaceMesh (target objects), and WorldMesh (Floors and ceilings that define the enclosed area)

    MeshObject^ cylinderMesh = ref new CylinderMesh(d3dDevice, 26);
    // ...

    // The Material class maintains the properties that represent how an object will
    // look when it is rendered.  This includes the color of the object, the
    // texture used to render the object, and the vertex and pixel shader that
    // should be used for rendering.

    Material^ cylinderMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    // ...
    auto objects = m_game->RenderObjects();

    // Attach the textures to the appropriate game objects.
    // We'll loop through all the objects that need to be rendered.
    for (auto object = objects.begin(); object != objects.end(); object++)
    {

        if ((*object)->TargetId() == GameConstants::WorldFloorId)
        {
            // Assign a normal material for the floor object.
            // This normal material uses the floor texture (cellfloor.dds) that was loaded asynchronously from
            // the Assets folder using BasicLoader::LoadTextureAsync method in the earlier 
            // CreateGameDeviceResourcesAsync loop

            (*object)->NormalMaterial(
                ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
            // Creates a mesh object called WorldFloorMesh and assign it to the floor object.
            (*object)->Mesh(ref new WorldFloorMesh(d3dDevice));
        }
        // ...
        else if (Cylinder^ cylinder = dynamic_cast<Cylinder^>(*object))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (Face^ target = dynamic_cast<Face^>(*object))
        {
            const int bufferLength = 16;
            char16 str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            Platform::String^ string = ref new Platform::String(str, len);

            ComPtr<ID3D11ShaderResourceView> texture;
            textureGenerator->CreateTextureResourceView(string, &texture);
            target->NormalMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            textureGenerator->CreateHitTextureResourceView(string, &texture);
            target->HitMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            target->Mesh(targetMesh);
        }
        // ...
    }


    // The SetProjParams method calculates the projection matrix based on input params and
    // ensures that the camera has been initialized with the right projection
    // matrix.  
    // The camera is not created at the time the first window resize event occurs.

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();
    m_game->GameCamera()->SetProjParams(
        XM_PI / 2,
        renderTargetSize.Width / renderTargetSize.Height,
        0.01f,
        100.0f
        );

    // Make sure that the correct projection matrix is set in the ConstantBufferChangeOnResize buffer.

    // Get the 3D rotation transform matrix. We are handling screen rotations directly to eliminate an unaligned 
    // fullscreen copy. So it is necessary to post multiply the 3D rotation matrix to the camera's projection matrix
    // to get the projection matrix that we need.

    auto orientation = m_deviceResources->GetOrientationTransform3D();

    ConstantBufferChangeOnResize changesOnResize;

    // The matrices are transposed due to the shader code expecting the matrices in the opposite
    // row/column order from the DirectX math library.

    // XMStoreFloat4x4 takes a matrix and writes the components out to sixteen single-precision floating-point values at the given address. 
    // The most significant component of the first row vector is written to the first four bytes of the address, 
    // followed by the second most significant component of the first row, and so on. The second row is then written out in a 
    // like manner to memory beginning at byte 16, followed by the third row to memory beginning at byte 32, and finally 
    // the fourth row to memory beginning at byte 48. For more API ref info, go to: 
    // https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.storing.xmstorefloat4x4.aspx

    XMStoreFloat4x4(
        &changesOnResize.projection,
        XMMatrixMultiply(
            XMMatrixTranspose(m_game->GameCamera()->Projection()),
            XMMatrixTranspose(XMLoadFloat4x4(&orientation))
            )
        );

    // UpdateSubresource method instructs CPU to copy data from memory (changesOnResize) to a subresource 
    // created in non-mappable memory (m_constantBufferChangeOnResize ) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method.

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferChangeOnResize.Get(),
        0,
        nullptr,
        &changesOnResize,
        0,
        0
        );

    // Finally we set the m_gameResourcesLoaded as TRUE, so we can start rendering.
    m_gameResourcesLoaded = true;
}
```

## <a name="createwindowsizedependentresource-method"></a>Método CreateWindowSizeDependentResource

Los métodos CreateWindowSizeDependentResources se llaman cada vez que cambia el tamaño de ventana, la orientación, la representación habilitada para estéreo o los cambios de resolución. En el juego de muestra, actualiza la matriz de proyección en __ConstantBufferChangeOnResize__.

Los recursos de tamaño de ventana se actualizan de esta manera: 
* El marco de la aplicación obtiene uno de los diversos eventos posibles que indican un cambio en el estado de ventana. 
* El bucle principal del juego, a continuación, recibe informe del evento y llamadas __CreateWindowSizeDependentResources__ en la instancia (__GameMain__) de la clase principal, que, a continuación, llama a la implementación __CreateWindowSizeDependentResources__ de la clase (__GameRenderer__) del representador del juego.
* El trabajo principal de este método es asegurarse de que los elementos visuales no se conviertan en confusos o inválidos debido a un cambio en las propiedades de ventana.

Para esta muestra de juego, un cierto número de llamadas a métodos son las mismas que en el método [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method). Para ver un tutorial de código, ve a la sección anterior.

Los ajustes de representación al HUD y al tamaño de ventana de superposición del juego se tratan en [Agregar una interfaz de usuario](#tutorial--adding-a-user-interface).

```cpp
// Initializes view parameters when the window size changes.
void GameRenderer::CreateWindowSizeDependentResources()
{

    // Game HUD and overlay window size rendering adjustments are done here
    // but they'll be covered in the UI section instead.

    m_gameHud->CreateWindowSizeDependentResources();

    // ...

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    // In Sample3DSceneRenderer::CreateWindowSizeDependentResources, we had:
    // Size outputSize = m_deviceResources->GetOutputSize();

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();

    // ...

    m_gameInfoOverlay->CreateWindowSizeDependentResources(m_gameInfoOverlaySize);

    if (m_game != nullptr)
    {
        // Similar operations as the last section of FinalizeCreateGameDeviceResources method
        m_game->GameCamera()->SetProjParams(
            XM_PI / 2, renderTargetSize.Width / renderTargetSize.Height,
            0.01f,
            100.0f
            );

        XMFLOAT4X4 orientation = m_deviceResources->GetOrientationTransform3D();

        ConstantBufferChangeOnResize changesOnResize;
        XMStoreFloat4x4(
            &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera()->Projection()),
                XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                )
            );

        d3dContext->UpdateSubresource(
            m_constantBufferChangeOnResize.Get(),
            0,
            nullptr,
            &changesOnResize,
            0,
            0
            );
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Este es el proceso básico para implementar el marco de representación de gráficos de un juego. Cuanto mayor sea el juego, más abstracciones tendrías que emplear para controlar las jerarquías de los tipos de objetos y los comportamientos de las animaciones. Debes implementar métodos más complejos de carga y administración de activos, como mallas y texturas. A continuación, vamos a aprender cómo [agregar una interfaz de usuario](tutorial--adding-a-user-interface.md).