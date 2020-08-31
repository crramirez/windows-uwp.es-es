---
title: Configurar
description: Obtenga información sobre cómo ensamblar la canalización de representación para mostrar los gráficos. Reproducción de juegos, configurar y preparar datos.
ms.assetid: 7720ac98-9662-4cf3-89c5-7ff81896364a
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, juegos y representación
ms.localizationpriority: medium
ms.openlocfilehash: a87382aeffb0e0b7a8eaca1c4baec8561049e91e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168249"
---
# <a name="rendering-framework-ii-game-rendering"></a>Plataforma de representación II: representación de juego

> [!NOTE]
> Este tema forma parte de la serie de tutoriales de [creación de una plataforma universal de Windows sencilla (UWP) con DirectX](tutorial--create-your-first-uwp-directx-game.md) . El tema de ese vínculo establece el contexto de la serie.

En el [marco de representación I](tutorial--assembling-the-rendering-pipeline.md), hemos explicado cómo se toma la información de la escena y se presenta en la pantalla de presentación. Ahora, tomaremos un paso atrás y aprenderemos a preparar los datos para la representación.

>[!Note]
>Si no ha descargado el código de juego más reciente para este ejemplo, vaya a [juego de ejemplo de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este ejemplo forma parte de una gran colección de ejemplos de características de UWP. Para obtener instrucciones sobre cómo descargar el ejemplo, consulte [obtener los ejemplos de UWP en github](../get-started/get-app-samples.md).

## <a name="objective"></a>Objetivo

Resumen rápido sobre el objetivo. Es mejor entender cómo configurar un marco de representación básico para mostrar la salida de gráficos para un juego DirectX de UWP. Podemos agruparlos de forma flexible en estos tres pasos.

 1. Establecer una conexión con nuestra interfaz de gráficos
 2. Preparación: cree los recursos que necesitamos para dibujar los gráficos.
 3. Mostrar los gráficos: representar el marco

[Marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md) explicando cómo se representan los gráficos, que abarcan los pasos 1 y 3. 

En este artículo se explica cómo configurar otras partes de este marco y preparar los datos necesarios antes de que se produzca la representación, que es el paso 2 del proceso.

## <a name="design-the-renderer"></a>Diseño del representador

El representador es responsable de la creación y el mantenimiento de todos los objetos D3D11 y D2D que se usan para generar los objetos visuales de juego. La clase __GameRenderer__ es el representador de este juego de ejemplo y está diseñado para satisfacer las necesidades de representación del juego.

Estos son algunos conceptos que puede usar para diseñar el representador del juego:
* Dado que las API de Direct3D 11 se definen como API de [com](/windows/desktop/com/the-component-object-model) , debe proporcionar referencias de [ComPtr](/cpp/windows/comptr-class) a los objetos definidos por estas API. Estos objetos se liberan automáticamente cuando su última referencia sale del alcance cuando la aplicación finaliza. Para obtener más información, vea [ComPtr](https://github.com/Microsoft/DirectXTK/wiki/ComPtr). Ejemplo de estos objetos: búferes de constantes, objetos de sombreador: [sombreador de vértices](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders), [sombreador de píxeles](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders)y objetos de recursos de sombreador.
* Los búferes de constantes se definen en esta clase para contener varios datos necesarios para la representación.
    * Use varios búferes de constantes con distintas frecuencias para reducir la cantidad de datos que se deben enviar a la GPU por fotograma. En este ejemplo se separan las constantes en búferes diferentes en función de la frecuencia con la que deben actualizarse. Este es el procedimiento recomendado para la programación de Direct3D. 
    * En este juego de ejemplo, se definen 4 búferes de constantes.
        1. __m \_ constantBufferNeverChanges__ contiene los parámetros de iluminación. Se establece una vez en el método __FinalizeCreateGameDeviceResources__ y nunca vuelve a cambiar.
        2. __m \_ constantBufferChangeOnResize__ contiene la matriz de proyección. La matriz de proyección depende del tamaño y la relación de aspecto de la ventana. Se establece en [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) y luego se actualiza después de que se carguen los recursos en el método [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) . Si se representa en 3D, también se cambia dos veces por fotograma.
        3. __m \_ constantBufferChangesEveryFrame__ contiene la matriz de la vista. Esta matriz depende de la posición de la cámara y la dirección del aspecto (normal a la proyección) y cambia una vez por fotograma en el método __Render__ . Esto se analizó anteriormente en el __marco de representación I: Introducción a la representación__, en el [método __GameRenderer:: Render__ ](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method).
        4. __m \_ constantBufferChangesEveryPrim__ contiene la matriz del modelo y las propiedades de material de cada primitiva. La matriz de modelos transforma los vértices desde las coordenadas locales en coordenadas globales. Estas constantes son específicas de cada primitivo y se actualizan para cada llamada de dibujo. Esto se analizó anteriormente en el __marco de representación I: Introducción a la representación__, en la [representación primitiva](tutorial--assembling-the-rendering-pipeline.md#primitive-rendering).
* Los objetos de recursos de sombreador que contienen texturas para los primitivos también se definen en esta clase.
    * Algunas texturas están predefinidas ([DDS](/windows/desktop/direct3ddds/dx-graphics-dds-pguide) es un formato de archivo que se puede usar para almacenar texturas comprimidas y sin comprimir. Las texturas DDS se utilizan para las paredes y el piso del mundo, así como las esferas de munición).
    * En este juego de ejemplo, los objetos de recursos del sombreador son: __m \_ sphereTexture__, __m \_ cylinderTexture__, __m \_ ceilingTexture__, __m \_ floorTexture__, __m \_ wallsTexture__.
* Los objetos de sombreador se definen en esta clase para calcular los primitivos y las texturas. 
    * En este juego de ejemplo, los objetos de sombreador son __m \_ vertexShader__, __m \_ vertexShaderFlat__y __m \_ u__, __m \_ pixelShaderFlat__.
    * El sombreador de vértices procesa la iluminación básica y los primitivos y el sombreador de píxeles (en ocasiones llamado sombreador de fragmentos), procesa las texturas y cualquier efecto por píxel.
    * Existen dos versiones de estos sombreadores (regular y plano) para representar distintos primitivos. La razón por la que tenemos versiones diferentes es que las versiones planas son mucho más sencillas y no realizan resaltes especulares ni efectos de iluminación por píxel. Se usan en paredes y logran que las representaciones sean más rápidas en dispositivos de baja potencia.

## <a name="gamerendererh"></a>GameRenderer.h

Ahora veamos el código del objeto de la clase de representador del juego de ejemplo.

```cppwinrt
// Class handling the rendering of the game
class GameRenderer : public std::enable_shared_from_this<GameRenderer>
{
public:
    GameRenderer(std::shared_ptr<DX::DeviceResources> const& deviceResources);

    void CreateDeviceDependentResources();
    void CreateWindowSizeDependentResources();
    void ReleaseDeviceDependentResources();
    void Render();
    // --- end of async related methods section

    winrt::Windows::Foundation::IAsyncAction CreateGameDeviceResourcesAsync(_In_ std::shared_ptr<Simple3DGame> game);
    void FinalizeCreateGameDeviceResources();
    winrt::Windows::Foundation::IAsyncAction LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();

    Simple3DGameDX::IGameUIControl* GameUIControl() { return &m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.left, m_gameInfoOverlayRect.top);
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.right, m_gameInfoOverlayRect.bottom);
    };
    bool GameInfoOverlayVisible() { return m_gameInfoOverlay.Visible(); }
    // --- end of rendering overlay section
...
private:
    // Cached pointer to device resources.
    std::shared_ptr<DX::DeviceResources>        m_deviceResources;

    ...

    // Shader resource objects
    winrt::com_ptr<ID3D11ShaderResourceView>    m_sphereTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_cylinderTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_ceilingTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_floorTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant buffers
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferNeverChanges;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;

    // Texture sampler
    winrt::com_ptr<ID3D11SamplerState>          m_samplerLinear;

    // Shader objects: Vertex shaders and pixel shaders
    winrt::com_ptr<ID3D11VertexShader>          m_vertexShader;
    winrt::com_ptr<ID3D11VertexShader>          m_vertexShaderFlat;
    winrt::com_ptr<ID3D11PixelShader>           m_pixelShader;
    winrt::com_ptr<ID3D11PixelShader>           m_pixelShaderFlat;
    winrt::com_ptr<ID3D11InputLayout>           m_vertexLayout;
};
```

## <a name="constructor"></a>Constructor

A continuación, vamos a examinar el constructor __GameRenderer__ del juego de ejemplo y compararlo con el constructor __Sample3DSceneRenderer__ proporcionado en la plantilla de la aplicación DirectX 11.

```cppwinrt
// Constructor method of the main rendering class object
GameRenderer::GameRenderer(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
    m_gameInfoOverlay(deviceResources),
    m_gameHud(deviceResources, L"Windows platform samples", L"DirectX first-person game sample")
{
    // m_gameInfoOverlay is a GameHud object to render text in the top left corner of the screen.
    // m_gameHud is Game info rendered as an overlay on the top-right corner of the screen,
    // for example hits, shots, and time.

    CreateDeviceDependentResources();
    CreateWindowSizeDependentResources();
}
```

## <a name="create-and-load-directx-graphic-resources"></a>Crear y cargar recursos gráficos de DirectX

En el juego de ejemplo (y en la plantilla de __DirectX 11 App (universal Windows)__ de Visual Studio, la creación y carga de recursos de juegos se implementa mediante estos dos métodos que se llaman desde el constructor __GameRenderer__ :

* [__CreateDeviceDependentResources__](#createdevicedependentresources-method)
* [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method)

## <a name="createdevicedependentresources-method"></a>Método CreateDeviceDependentResources

En la plantilla de aplicación DirectX 11, este método se usa para cargar el sombreador de vértices y píxeles de forma asincrónica, crear el sombreador y el búfer de constantes, crear una malla con vértices que contengan información de posición y color. 

En el juego de ejemplo, estas operaciones de los objetos de la escena se dividen entre los métodos [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) y [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) . 

Para este juego de ejemplo, ¿qué pasa a este método?

* Variables con instancias (__m \_ gameResourcesLoaded__ = false y __m \_ levelResourcesLoaded__ = false) que indican si los recursos se han cargado antes de avanzar a render, ya que se cargan de forma asincrónica. 
* Dado que la representación de HUD y superposición está en objetos de clase independientes, llame a los métodos __GameHud:: CreateDeviceDependentResources__ y __GameInfoOverlay:: CreateDeviceDependentResources__ aquí.

Este es el código de __GameRenderer:: CreateDeviceDependentResources__.

```cppwinrt
// This method is called in GameRenderer constructor when it's created in GameMain constructor.
void GameRenderer::CreateDeviceDependentResources()
{
    // instantiate variables that indicate whether resources were loaded.
    m_gameResourcesLoaded = false;
    m_levelResourcesLoaded = false;

    // game HUD and overlay are design as separate class objects.
    m_gameHud.CreateDeviceDependentResources();
    m_gameInfoOverlay.CreateDeviceDependentResources();
}
```

A continuación se muestra una lista de los métodos que se usan para crear y cargar recursos.

- CreateDeviceDependentResources
  - CreateGameDeviceResourcesAsync (agregado)
  - FinalizeCreateGameDeviceResources (agregado)
- CreateWindowSizeDependentResources

Antes de profundizar en los otros métodos que se usan para crear y cargar recursos, vamos a crear primero el representador y ver cómo encaja en el bucle del juego.

## <a name="create-the-renderer"></a>Creación del representador

El __GameRenderer__ se crea en el constructor de __GameMain__. También llama a los otros dos métodos, [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) y [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) , que se agregan para ayudar a crear y cargar recursos.

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
{
    m_deviceResources->RegisterDeviceNotify(this);

    // Creation of GameRenderer
    m_renderer = std::make_shared<GameRenderer>(m_deviceResources);

    ...

    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...

    // Asynchronously initialize the game class and load the renderer device resources.
    // By doing all this asynchronously, the game gets to its main loop more quickly
    // and in parallel all the necessary resources are loaded on other threads.
    m_game->Initialize(m_controller, m_renderer);

    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);

    // The finalize code needs to run in the same thread context
    // as the m_renderer object was created because the D3D device context
    // can ONLY be accessed on a single thread.
    // co_await of an IAsyncAction resumes in the same thread context.
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();

    ...
}
```

## <a name="creategamedeviceresourcesasync-method"></a>Método CreateGameDeviceResourcesAsync

Se llama a __CreateGameDeviceResourcesAsync__ desde el método de constructor __GameMain__ en el bucle de __creación de \_ tarea__ , ya que estamos cargando los recursos de juego de forma asincrónica.
        
__CreateDeviceResourcesAsync__ es un método que ejecuta un conjunto de tareas asincrónicas distinto para cargar los recursos del juego. Dado que se espera que se ejecute en un subproceso independiente, solo tiene acceso a los métodos de dispositivo de Direct3D 11 (los definidos en __ID3D11Device__) y no a los métodos de contexto de dispositivo (los métodos definidos en __ID3D11DeviceContext__), por lo que no realiza ninguna representación.

El método __FinalizeCreateGameDeviceResources__ se ejecuta en el subproceso principal y tiene acceso a los métodos de contexto de dispositivo de Direct3D 11.

En principio:
* Use solo métodos __ID3D11Device__ en __CreateGameDeviceResourcesAsync__ porque son de subprocesos libres, lo que significa que se pueden ejecutar en cualquier subproceso. También se espera que no se ejecuten en el mismo subproceso en el que se creó el __GameRenderer__ . 
* No use métodos en __ID3D11DeviceContext__ aquí porque deben ejecutarse en un único subproceso y en el mismo subproceso que __GameRenderer__.
* Utilice este método para crear búferes de constantes.
* Use este método para cargar texturas (como los archivos. DDS) e información del sombreador (como los archivos. CSO) en los [sombreadores](tutorial--assembling-the-rendering-pipeline.md#shaders).

Este método se usa para:
* Cree los 4 [búferes de constantes](tutorial--assembling-the-rendering-pipeline.md#buffer): __m \_ constantBufferNeverChanges__, __m \_ constantBufferChangeOnResize__, __m \_ constantBufferChangesEveryFrame__, __m \_ constantBufferChangesEveryPrim__
* Crear un objeto [de estado de muestra](tutorial--assembling-the-rendering-pipeline.md#sampler-state) que encapsula la información de muestreo de una textura
* Cree un grupo de tareas que contenga todas las tareas asincrónicas creadas por el método. Espera a que se completen todas estas tareas asincrónicas y, a continuación, llama a __FinalizeCreateGameDeviceResources__.
* Cree un cargador mediante el [cargador básico](tutorial--assembling-the-rendering-pipeline.md#the-basicloader-class). Agregue las operaciones de carga asincrónica del cargador como tareas en el grupo de tareas creado anteriormente.
* Los métodos como __basicloader:: LoadShaderAsync__ y  __Basicloader:: LoadTextureAsync__ se usan para cargar:
    * objetos de sombreador compilados (VertextShader. CSO, VertexShaderFlat. CSO, u. CSO y PixelShaderFlat. CSO). Para obtener más información, vaya a [varios formatos de archivo de sombreador](tutorial--assembling-the-rendering-pipeline.md#various-shader-file-formats).
    * texturas específicas del juego (assets \\ Seafloor. DDS, metal_texture. DDS, cellceiling. DDS, cellfloor. DDS, CELLWall. DDS).

```cppwinrt
IAsyncAction GameRenderer::CreateGameDeviceResourcesAsync(_In_ std::shared_ptr<Simple3DGame> game)
{
    auto lifetime = shared_from_this();

    // Create the device dependent game resources.
    // Only the d3dDevice is used in this method. It is expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the d3dDevice are free-threaded and are safe while any methods
    // in the d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.
    m_game = game;

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    // Define D3D11_BUFFER_DESC. See
    // https://docs.microsoft.com/windows/win32/api/d3d11/ns-d3d11-d3d11_buffer_desc
    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));

    // Create the constant buffers.
    bd.Usage = D3D11_USAGE_DEFAULT;
    ...

    // Create the constant buffers: m_constantBufferNeverChanges, m_constantBufferChangeOnResize,
    // m_constantBufferChangesEveryFrame, m_constantBufferChangesEveryPrim
    // CreateBuffer is used to create one of these buffers: vertex buffer, index buffer, or 
    // shader-constant buffer. For CreateBuffer API ref info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11device-createbuffer.
    winrt::check_hresult(
        d3dDevice->CreateBuffer(&bd, nullptr, m_constantBufferNeverChanges.put())
        );

    ...

    // Define D3D11_SAMPLER_DESC. For API ref, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/ns-d3d11-d3d11_sampler_desc.
    D3D11_SAMPLER_DESC sampDesc;

    // ZeroMemory fills a block of memory with zeros. For API ref, see
    // https://docs.microsoft.com/previous-versions/windows/desktop/legacy/aa366920(v=vs.85).
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    ...

    // Create a sampler-state object that encapsulates sampling information for a texture.
    // The sampler-state interface holds a description for sampler state that you can bind to any 
    // shader stage of the pipeline for reference by texture sample operations.
    winrt::check_hresult(
        d3dDevice->CreateSamplerState(&sampDesc, m_samplerLinear.put())
        );

    // Start the async tasks to load the shaders and textures.

    // Load compiled shader objects (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso, and PixelShaderFlat.cso).
    // The BasicLoader class is used to convert and load common graphics resources, such as meshes, textures, 
    // and various shader objects into the constant buffers. For more info, see
    // https://docs.microsoft.com/windows/uwp/gaming/complete-code-for-basicloader.
    BasicLoader loader{ d3dDevice };

    std::vector<IAsyncAction> tasks;

    uint32_t numElements = ARRAYSIZE(PNTVertexLayout);

    // Load shaders asynchronously with the shader and pixel data using the
    // BasicLoader::LoadShaderAsync method. Push these method calls into a list of tasks.
    tasks.push_back(loader.LoadShaderAsync(L"VertexShader.cso", PNTVertexLayout, numElements, m_vertexShader.put(), m_vertexLayout.put()));
    tasks.push_back(loader.LoadShaderAsync(L"VertexShaderFlat.cso", nullptr, numElements, m_vertexShaderFlat.put(), nullptr));
    tasks.push_back(loader.LoadShaderAsync(L"PixelShader.cso", m_pixelShader.put()));
    tasks.push_back(loader.LoadShaderAsync(L"PixelShaderFlat.cso", m_pixelShaderFlat.put()));

    // Make sure the previous versions if any of the textures are released.
    m_sphereTexture = nullptr;
    ...

    // Load Game specific textures (Assets\\seafloor.dds, metal_texture.dds, cellceiling.dds,
    // cellfloor.dds, cellwall.dds).
    // Push these method calls also into a list of tasks.
    tasks.push_back(loader.LoadTextureAsync(L"Assets\\seafloor.dds", nullptr, m_sphereTexture.put()));
    ...

    // Simulate loading additional resources by introducing a delay.
    tasks.push_back([]() -> IAsyncAction { co_await winrt::resume_after(GameConstants::InitialLoadingDelay); }());

    // Returns when all the async tasks for loading the shader and texture assets have completed.
    for (auto&& task : tasks)
    {
        co_await task;
    }
}
```

## <a name="finalizecreategamedeviceresources-method"></a>Método FinalizeCreateGameDeviceResources

El método __FinalizeCreateGameDeviceResources__ se llama después de que se completen todas las tareas de recursos de carga que se encuentran en el método __CreateGameDeviceResourcesAsync__ . 

* Inicialice constantBufferNeverChanges con las posiciones y el color de la luz. Carga los datos iniciales en los búferes de constantes con una llamada del método de contexto de dispositivo a __ID3D11DeviceContext:: UpdateSubresource__.
* Dado que los recursos cargados de forma asincrónica han finalizado la carga, es el momento de asociarlos con los objetos de juego adecuados.
* Para cada objeto de juego, cree la malla y el material usando las texturas que se han cargado. A continuación, asocie la malla y el material al objeto de juego.
* En el caso del objeto Game de destinos, la textura compuesta por anillos de color concéntricos, con un valor numérico en la parte superior, no se carga desde un archivo de textura. En su lugar, se genera de procedimientos mediante el código en __TargetTexture. cpp__. La clase __TargetTexture__ crea los recursos necesarios para dibujar la textura en un recurso fuera de la pantalla en el momento de la inicialización. A continuación, la textura resultante se asocia con los objetos de juego de destino adecuados.

__FinalizeCreateGameDeviceResources__ y [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) comparten partes similares de código para estos:
* Use __SetProjParams__ para asegurarse de que la cámara tiene la matriz de proyección adecuada. Para obtener más información, vaya a [cámara y espacio de coordenadas](tutorial--assembling-the-rendering-pipeline.md#camera-and-coordinate-space).
* Controlar la rotación de la pantalla mediante post multiplicando la matriz de rotación 3D a la matriz de proyección de la cámara. A continuación, actualice el búfer de constantes __ConstantBufferChangeOnResize__ con la matriz de proyección resultante.
* Establezca la variable global de __valor booleano__ __m \_ gameResourcesLoaded__ para indicar que los recursos se cargan ahora en los búferes, listos para el siguiente paso. Recuerde que primero inicializamos esta variable como __false__ en el método de constructor de __GameRenderer__, a través del método __GameRenderer:: CreateDeviceDependentResources__ . 
* Cuando este __m \_ GameResourcesLoaded__ es __true__, se puede producir la representación de los objetos de la escena. Esto se trató en el __marco de representación I: Introducción al__ artículo de representación, en el [__método GameRenderer:: Render__](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method).

```cppwinrt
// This method is called from the GameMain constructor.
// Make sure that 2D rendering is occurring on the same thread as the main rendering.
void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now associate all the resources with the appropriate game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize the Constant buffer with the light positions
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4(3.5f, 2.5f, 5.5f, 1.0f);
    ...
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);

    // CPU copies data from memory (constantBufferNeverChanges) to a subresource 
    // created in non-mappable memory (m_constantBufferNeverChanges) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method. For UpdateSubresource API ref info, 
    // go to: https://msdn.microsoft.com/library/windows/desktop/ff476486.aspx
    // To learn more about what a subresource is, go to:
    // https://msdn.microsoft.com/library/windows/desktop/ff476901.aspx

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferNeverChanges.get(),
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

    TargetTexture textureGenerator(
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

    auto cylinderMesh = std::make_shared<CylinderMesh>(d3dDevice, (uint16_t)26);
    ...

    // The Material class maintains the properties that represent how an object will
    // look when it is rendered.  This includes the color of the object, the
    // texture used to render the object, and the vertex and pixel shader that
    // should be used for rendering.

    auto cylinderMaterial = std::make_shared<Material>(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.get(),
        m_vertexShader.get(),
        m_pixelShader.get()
        );

    ...

    // Attach the textures to the appropriate game objects.
    // We'll loop through all the objects that need to be rendered.
    for (auto&& object : m_game->RenderObjects())
    {
        if (object->TargetId() == GameConstants::WorldFloorId)
        {
            // Assign a normal material for the floor object.
            // This normal material uses the floor texture (cellfloor.dds) that was loaded asynchronously from
            // the Assets folder using BasicLoader::LoadTextureAsync method in the earlier 
            // CreateGameDeviceResourcesAsync loop

            object->NormalMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.get(),
                    m_vertexShaderFlat.get(),
                    m_pixelShaderFlat.get()
                    )
                );
            // Creates a mesh object called WorldFloorMesh and assign it to the floor object.
            object->Mesh(std::make_shared<WorldFloorMesh>(d3dDevice));
        }
        ...
        else if (auto cylinder = dynamic_cast<Cylinder*>(object.get()))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (auto target = dynamic_cast<Face*>(object.get()))
        {
            const int bufferLength = 16;
            wchar_t str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            auto string{ winrt::hstring(str, len) };

            winrt::com_ptr<ID3D11ShaderResourceView> texture;
            textureGenerator.CreateTextureResourceView(string, texture.put());
            target->NormalMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.get(),
                    m_vertexShader.get(),
                    m_pixelShader.get()
                    )
                );

            texture = nullptr;
            textureGenerator.CreateHitTextureResourceView(string, texture.put());
            target->HitMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.get(),
                    m_vertexShader.get(),
                    m_pixelShader.get()
                    )
                );

            target->Mesh(targetMesh);
        }
        ...
    }

    // The SetProjParams method calculates the projection matrix based on input params and
    // ensures that the camera has been initialized with the right projection
    // matrix.  
    // The camera is not created at the time the first window resize event occurs.

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();
    m_game->GameCamera().SetProjParams(
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
            XMMatrixTranspose(m_game->GameCamera().Projection()),
            XMMatrixTranspose(XMLoadFloat4x4(&orientation))
            )
        );

    // UpdateSubresource method instructs CPU to copy data from memory (changesOnResize) to a subresource 
    // created in non-mappable memory (m_constantBufferChangeOnResize ) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method.

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferChangeOnResize.get(),
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

Se llama a los métodos CreateWindowSizeDependentResources cada vez que cambia el tamaño de la ventana, la orientación, la representación habilitada para estéreo o la resolución. En el juego de ejemplo, se actualiza la matriz de proyección en __ConstantBufferChangeOnResize__.

Los recursos de tamaño de la ventana se actualizan de esta manera: 
* El marco de trabajo de la aplicación obtiene uno de varios posibles eventos que indican un cambio en el estado de la ventana. 
* A continuación, el bucle principal del juego se informa del evento y llama a __CreateWindowSizeDependentResources__ en la instancia de la clase principal (__GameMain__), que luego llama a la implementación de __createwindowsizedependentresources__ en la clase de representador de juegos (__GameRenderer__).
* El trabajo principal de este método es asegurarse de que los objetos visuales no se confunden o no son válidos debido a un cambio en las propiedades de la ventana.

Para este juego de ejemplo, varias llamadas a métodos son las mismas que las del método [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) . Para ver el tutorial de código, vaya a la sección anterior.

Los ajustes de representación del tamaño de la ventana de la pantalla de visualización y superposición se describen en [Agregar una interfaz de usuario](tutorial--adding-a-user-interface.md).

```cppwinrt
// Initializes view parameters when the window size changes.
void GameRenderer::CreateWindowSizeDependentResources()
{
    // Game HUD and overlay window size rendering adjustments are done here
    // but they'll be covered in the UI section instead.

    m_gameHud.CreateWindowSizeDependentResources();

    ...

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    // In Sample3DSceneRenderer::CreateWindowSizeDependentResources, we had:
    // Size outputSize = m_deviceResources->GetOutputSize();

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();

    ...

    m_gameInfoOverlay.CreateWindowSizeDependentResources(m_gameInfoOverlaySize);

    if (m_game != nullptr)
    {
        // Similar operations as the last section of FinalizeCreateGameDeviceResources method
        m_game->GameCamera().SetProjParams(
            XM_PI / 2, renderTargetSize.Width / renderTargetSize.Height,
            0.01f,
            100.0f
            );

        XMFLOAT4X4 orientation = m_deviceResources->GetOrientationTransform3D();

        ConstantBufferChangeOnResize changesOnResize;
        XMStoreFloat4x4(
            &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera().Projection()),
                XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                )
            );

        d3dContext->UpdateSubresource(
            m_constantBufferChangeOnResize.get(),
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

Este es el proceso básico para implementar el marco de representación de gráficos de un juego. Cuanto más grande sea su juego, más abstracciones tendrá que poner en marcha para controlar las jerarquías de los tipos de objeto y los comportamientos de animación. Debe implementar métodos más complejos para cargar y administrar recursos como mallas y texturas. A continuación, vamos a obtener información sobre cómo [Agregar una interfaz de usuario](tutorial--adding-a-user-interface.md).