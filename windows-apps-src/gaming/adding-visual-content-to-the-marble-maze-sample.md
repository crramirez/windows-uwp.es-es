---
author: mtoepke
title: Agregar contenido visual a la muestra de Marble Maze
description: "En este documento se describe cómo el juego Marble Maze usa Direct3D y Direct2D en el entorno de aplicaciones de la Plataforma universal de Windows (UWP) para que puedas conocer los patrones y adaptarlos cuando trabajes con el contenido de tu propio juego."
ms.assetid: 6e43422e-e1a1-b79e-2c4b-7d5b4fa88647
translationtype: Human Translation
ms.sourcegitcommit: eb0115bf83627a9ba8209cce6bdd9edecc165ddf
ms.openlocfilehash: 6b7880703d40d6ef5ed5f42f3e09bc5573170e1f

---

# <a name="adding-visual-content-to-the-marble-maze-sample"></a>Agregar contenido visual al ejemplo de Marble Maze


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En este documento se describe cómo el juego Marble Maze usa Direct3D y Direct2D en el entorno de aplicaciones de la Plataforma universal de Windows (UWP) de modo que puedas conocer los patrones y adaptarlos cuando trabajes con el contenido de tu propio juego. Para averiguar qué lugar ocupan los componentes visuales del juego en la estructura global de Marble Maze, consulta [Estructura de la aplicación Marble Maze](marble-maze-application-structure.md).

A la hora de desarrollar los aspectos visuales de Marble Maze, seguimos estos pasos básicos:

1.  Crear un marco básico que inicialice los entornos de Direct3D y de Direct2D.
2.  Usar programas de edición de imágenes y modelos para diseñar los activos 2D y 3D que aparecen en el juego.
3.  Asegurarse de que los activos 2D y 3D se cargan y aparecen en el juego correctamente.
4.  Integrar sombreadores de vértices y píxeles que mejoren la calidad visual de los activos del juego.
5.  Integrar la lógica de juego, como la animación y la entrada de usuario.

Además, nos centramos primero en agregar los activos 3D y luego los activos 2D. Por ejemplo, nos centramos en la lógica de juego básica antes de agregar el sistema de menús y el temporizador.

Durante el proceso de desarrollo también fue necesario iterar varias veces algunos de estos pasos. Por ejemplo, al realizar cambios en los modelos de malla y canica, también tuvimos que cambiar parte del código de sombreador que admite esos modelos.

> **Nota** El código de ejemplo correspondiente a este documento se encuentra en la [muestra de Marble Maze con DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

 
A continuación ofrecemos algunos de los puntos clave tratados en este documento para cuando trabajes con DirectX y contenido visual de juegos; en concreto, para cuando inicialices las bibliotecas de elementos gráficos DirectX, cargues recursos de escena y actualices y presentes la escena:

-   Normalmente, agregar contenido al juego implica muchos pasos, que a menudo requieren además iteración. Los desarrolladores de juegos suelen centrarse primero en agregar el contenido 3D y solo después el 2D.
-   Para llegar a más clientes y ofrecerles a todos una gran experiencia, admite la gama más amplia posible de hardware gráfico.
-   Separa claramente los formatos en tiempo de diseño y en tiempo de ejecución. Estructura tus activos en tiempo de diseño para maximizar la flexibilidad y permitir iteraciones rápidas en el contenido. Aplica formato y comprime los activos para que se carguen y representen de la forma más eficaz posible en tiempo de ejecución.
-   Crea los dispositivos de Direct3D y Direct2D en una aplicación para UWP de forma similar a como lo harías en una aplicación de escritorio de Windows clásica. Una diferencia importante es cómo se asocia la cadena de intercambio a la ventana de entrada.
-   Cuando diseñes tu juego, asegúrate de que el formato de malla que elijas admita los escenarios clave. Por ejemplo, si tu juego requiere colisiones, asegúrate de que puedes obtener datos de colisión de tus mallas.
-   Separa la lógica de juego de la lógica de representación actualizando primero todos los objetos de escena antes de representarlos.
-   Normalmente, se dibujan los objetos de la escena 3D y luego todos los objetos 2D que aparecen delante de la escena.
-   Sincroniza el dibujo con el espacio en blanco vertical para garantizar que tu juego no pierde tiempo dibujando fotogramas que nunca se mostrarán realmente en la pantalla.

## <a name="getting-started-with-directx-graphics"></a>Introducción a los elementos gráficos de DirectX


Cuando planeamos el juego de la Plataforma universal de Windows (UWP) Marble Maze, elegimos C++ y Direct3D 11.1 porque son las mejores opciones para crear juegos 3D que requieren el máximo control sobre la representación y un alto rendimiento. DirectX 11.1 admite hardware de DirectX 9 a DirectX 11 y, por lo tanto, puede ayudarte a llegar a más clientes de forma más eficaz porque no necesitas reescribir el código para cada una de las versiones anteriores de DirectX.

Marble Maze usa Direct3D 11.1 para representar los activos 3D del juego, en concreto la canica y el laberinto. Marble Maze también emplea Direct2D, DirectWrite y Windows Imaging Component (WIC) para dibujar los activos 2D del juego, como los menús y el temporizador. Por último, Marble Maze usa XAML para proporcionar una barra de la aplicación y te permite agregar controles XAML.

El desarrollo de un juego exige una buena planificación. Si no tienes experiencia con los elementos gráficos de DirectX, te recomendamos que leas el tema Crear un juego DirectX para familiarizarte con los conceptos básicos de creación de juegos DirectX para UWP. Al tiempo que lees este documento y trabajas con el código fuente de Marble Maze, puedes consultar los siguientes recursos para obtener información más detallada sobre los elementos gráficos DirectX.

-   [Direct3D 11 Graphics](https://msdn.microsoft.com/library/windows/desktop/ff476080) (Gráficos Direct3D 11) Se describe Direct3D 11, una eficaz API de elementos gráficos 3D acelerados por hardware para representar la geometría 3D en la plataforma Windows.
-   [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) Se describe Direct2D, una API de elementos gráficos 2D acelerados por hardware que proporciona alto rendimiento y representación de alta calidad de la geometría 2D, mapas de bits y texto.
-   [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) Se describe DirectWrite, que admite la representación de texto de alta calidad.
-   [Windows Imaging Component](https://msdn.microsoft.com/library/windows/desktop/ee719902) Describe WIC, una plataforma extensible que proporciona API de bajo nivel para imágenes digitales.

### <a name="feature-levels"></a>Niveles de características

Direct3D 11 introduce un paradigma llamado niveles de características. Un nivel de características es un conjunto bien definido de funcionalidades GPU. Usa los niveles de características para hacer que tu juego se ejecute en versiones anteriores de hardware Direct3D. Marble Maze admite el nivel de características 9.1 porque no precisa funciones avanzadas de los niveles más altos. Te recomendamos que admitas el mayor abanico de hardware posible y que escales el contenido del juego de modo que todos tus clientes disfruten de una gran experiencia, con independencia de que sus equipos sean de gama alta o baja. Para más información sobre los niveles de características, consulta [Direct3D 11 en hardware de versión anterior](https://msdn.microsoft.com/library/windows/desktop/ff476872).

## <a name="initializing-direct3d-and-direct2d"></a>Inicializar Direct3D y Direct2D


Un dispositivo representa el adaptador de pantalla. Crea los dispositivos de Direct3D y Direct2D en una aplicación para UWP de forma similar a como lo harías en una aplicación de escritorio de Windows clásica. La principal diferencia es cómo conectas la cadena de intercambio de Direct3D al sistema basado en ventanas.

La *aplicación (universal de Windows) DirectX 11 y XAML* excluye algunas funciones genéricas del sistema operativo y de presentación 3D de las funciones específicas del juego. La clase **DeviceResources** actúa como base para administrar Direct3D y Direct2D. Esta clase controla la infraestructura general, no activos específicos del juego. Marble Maze define la clase **MarbleMaze** para controlar activos específicos del juego, que tiene una referencia a un objeto **DeviceResources** para proporcionarle acceso a Direct3D y Direct2D.

Durante la inicialización, el método **DeviceResources::Initialize** crea recursos independientes de dispositivo, así como los dispositivos Direct3D y Direct2D.

```cpp
// Initialize the Direct3D resources required to run. 
void DeviceResources::DeviceResources(CoreWindow^ window, float dpi)
{
    m_window = window;

    CreateDeviceIndependentResources();
    CreateDeviceResources();
    CreateWindowSizeDependentResources();
    SetDpi(dpi);
}
```

La clase **DeviceResources** separa esta característica para poder responder con mayor facilidad cuando el entorno cambia. Por ejemplo, llama al método **CreateWindowSizeDependentResources** cuando cambia el tamaño de la ventana.

###  <a name="initializing-the-direct2d-directwrite-and-wic-factories"></a>Inicializar las fábricas de Direct2D, DirectWrite y WIC

El método **DeviceResources::CreateDeviceIndependentResources** crea las fábricas para Direct2D, DirectWrite y WIC. En los elementos gráficos de DirectX, las fábricas son los puntos de partida para crear recursos gráficos. Marble Maze especifica **D2D1\_FACTORY\_TYPE\_SINGLE\_THREADED** porque realiza todos los dibujos en el subproceso principal.

```cpp
// These are the resources required independent of hardware. 
void DeviceResources::CreateDeviceIndependentResources()
{
    D2D1_FACTORY_OPTIONS options;
    ZeroMemory(&options, sizeof(D2D1_FACTORY_OPTIONS));

#if defined(_DEBUG)
     // If the project is in a debug build, enable Direct2D debugging via SDK Layers.
    options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

    DX::ThrowIfFailed(
        D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            __uuidof(ID2D1Factory1),
            &options,
            &m_d2dFactory
            )
        );

    DX::ThrowIfFailed(
        DWriteCreateFactory(
            DWRITE_FACTORY_TYPE_SHARED,
            __uuidof(IDWriteFactory),
            &m_dwriteFactory
            )
        );

    DX::ThrowIfFailed(
        CoCreateInstance(
            CLSID_WICImagingFactory,
            nullptr,
            CLSCTX_INPROC_SERVER,
            IID_PPV_ARGS(&m_wicFactory)
            )
        );
}
```

###  <a name="creating-the-direct3d-and-direct2d-devices"></a>Crear los dispositivos Direct3D y Direct2D

El método **DeviceResources::CreateDeviceResources** llama a [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) para crear el objeto de dispositivo que representa el adaptador de pantalla de Direct3D. Dado que Marble Maze admite el nivel de características 9.1 y posterior, el método **DeviceResources::CreateDeviceResources** especifica los niveles de 9.1 a 11.1 en la matriz de valores **\\**. Direct3D recorre la lista en orden y asigna a la aplicación el primer nivel de características disponible. Por lo tanto, las entradas de la matriz **D3D\_FEATURE\_LEVEL** se enumeran de mayor a menor para que la aplicación obtenga el nivel de características más alto disponible. El método **DeviceResources::CreateDeviceResources** obtiene el dispositivo de Direct3D 11.1 al consultar al dispositivo de Direct3D 11 devuelto por **D3D11CreateDevice**.

```cpp
// This array defines the set of DirectX hardware feature levels this app will support. 
// Note the ordering should be preserved. 
// Don't forget to declare your application's minimum required feature level in its 
// description.  All applications are assumed to support 9.1 unless otherwise stated.
D3D_FEATURE_LEVEL featureLevels[] = 
{
    D3D_FEATURE_LEVEL_11_1,
    D3D_FEATURE_LEVEL_11_0,
    D3D_FEATURE_LEVEL_10_1,
    D3D_FEATURE_LEVEL_10_0,
    D3D_FEATURE_LEVEL_9_3,
    D3D_FEATURE_LEVEL_9_2,
    D3D_FEATURE_LEVEL_9_1
};

// Create the DX11 API device object, and get a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;
DX::ThrowIfFailed(
    D3D11CreateDevice(
        nullptr,                    // Specify null to use the default adapter.
        D3D_DRIVER_TYPE_HARDWARE,
        0,                          // Leave as 0 unless it is a software device.
        creationFlags,              // Optionally, set debug and Direct2D compatibility flags.
        featureLevels,              // A list of feature levels that this app can support.
        ARRAYSIZE(featureLevels),   // The number of entries in the above list.
        D3D11_SDK_VERSION,          // Always set this to D3D11_SDK_VERSION for modern.
        &device,                    // Returns the Direct3D device created.
        &m_featureLevel,            // Returns the feature level of the device created.
        &context                    // Returns the device immediate context.
        )
    );    

// Get the Direct3D 11.1 device by querying the Direct3D 11 device.
DX::ThrowIfFailed(
    device.As(&m_d3dDevice)
    );
```

De este modo, el método **DeviceResources::CreateDeviceResources** crea el dispositivo de Direct2D. Direct2D usa la Infraestructura de gráficos de DirectX de Microsoft (DXGI) para interoperar con Direct3D. DXGI permite que las superficies de memoria de vídeo se compartan entre los tiempos de ejecución de gráficos. Marble Maze usa el dispositivo DXGI subyacente del dispositivo Direct3D para crear el dispositivo Direct2D de la fábrica Direct2D.

```cpp
// Obtain the underlying DXGI device of the Direct3D 11.1 device.
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

// Obtain the Direct2D device for 2-D rendering.
DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

// And get its corresponding device context object.
DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

```cpp
// Obtain the underlying DXGI device of the Direct3D 11.1 device.
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

// Obtain the Direct2D device for 2-D rendering.
DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

// And get its corresponding device context object.
DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

Para obtener más información sobre DXGI y la interoperabilidad entre Direct2D y Direct3D, consulta los temas [DXGI Overview](https://msdn.microsoft.com/library/windows/desktop/bb205075) (Introducción a DXGI) y [Direct2D and Direct3D Interoperability Overview](https://msdn.microsoft.com/library/windows/desktop/dd370966) (Introducción a la interoperabilidad entre Direct2D y Direct3D).

### <a name="associating-direct3d-with-the-view"></a>Asociar Direct3D con la vista

El método **DeviceResources::CreateWindowSizeDependentResources** crea los recursos de elementos gráficos que dependen de un tamaño de ventana determinado, como la cadena de intercambio y los destinos de presentación de Direct3D y Direct2D. Una diferencia importante entre una aplicación DirectX para UWP y una aplicación de escritorio es la forma en que se asocia la cadena de intercambio a la ventana de salida. Una cadena de intercambio es responsable de mostrar el búfer donde realiza la representación el dispositivo en el monitor. En el documento sobre la estructura de la aplicación Marble Maze se describen las diferencias entre el sistema de ventanas de una aplicación para UWP y el de una aplicación de escritorio. Dado que una aplicación de la Tienda Windows no funciona con objetos **HWND**, Marble Maze debe usar el método [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559) para asociar la salida del dispositivo a la vista. El siguiente ejemplo muestra la parte del método **DeviceResources::CreateWindowSizeDependentResources** que crea la cadena de intercambio.

```cpp
// Obtain the final swap chain for this window from the DXGI factory.
DX::ThrowIfFailed(
    dxgiFactory->CreateSwapChainForCoreWindow(
        m_d3dDevice.Get(),
        reinterpret_cast<IUnknown*>(m_window),
        &swapChainDesc,
        nullptr,    // Allow on all displays.
        &m_swapChain
        )
    );
```

Para minimizar el consumo de energía, lo que representa un factor muy importante en dispositivos con batería, como portátiles y tabletas, el método **DeviceResources::CreateWindowSizeDependentResources** llamada al método [**IDXGIDevice1::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/ff471334) para asegurarse de que el juego se presente solo después el espacio en blanco vertical. La sincronización con el espacio en blanco vertical se describe con mayor detalle en la sección Presentar la escena de este documento.

```cpp
// Ensure that DXGI does not queue more than one frame at a time. This both reduces  
// latency and ensures that the application will only render after each VSync, minimizing  
// power consumption.
DX::ThrowIfFailed(
    dxgiDevice->SetMaximumFrameLatency(1)
    );
```

El método **DeviceResources::CreateWindowSizeDependentResources** inicializa los recursos de elementos gráficos de un modo que funciona para la mayoría de los juegos.

> **Nota** El término *vista* tiene significados diferentes en Windows Runtime y Direct3D. En Windows en tiempo de ejecución, una vista se refiere a la colección de configuraciones de la interfaz de usuario de una aplicación, incluidos el área de presentación y los comportamientos de entrada, además del subproceso que usa para el procesamiento. La configuración y los parámetros que se necesitan se especifican al crear una vista. El proceso de configuración de la vista de la aplicación se describe en [Estructura de la aplicación Marble Maze](marble-maze-application-structure.md). En Direct3D, el término vista tiene varios significados. En primer lugar, una vista de recursos define los subrecursos a los que puede obtener acceso un recurso. Por ejemplo, cuando un objeto de textura se asocia a una vista de recursos de sombreador, dicho sombreador podrá obtener acceso a la textura más adelante. Una de las ventajas de una vista de recursos es que puedes interpretar datos de formas distintas en diferentes etapas del proceso de representación. Para más información sobre las vistas de recursos, consulta [Vistas de texturas (Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb205128). Cuando se usa en el contexto de una transformación de vista o de una matriz de transformación de vista, vista se refiere a la ubicación y la orientación de la cámara. Una transformación de vista cambia la posición de los objetos en el mundo alrededor de la posición y la orientación de la cámara. Para más información sobre las transformaciones de vistas, consulta [Transformación de vista (Direct3D 9)](https://msdn.microsoft.com/library/windows/desktop/bb206342). El modo en que Marble Maze usa las vistas de recursos y las vistas de matrices se describe con más detalle en este tema.

 

## <a name="loading-scene-resources"></a>Cargar recursos de escena


Marble Maze usa la clase **BasicLoader**, que se declara en BasicLoader.h, para cargar texturas y sombreadores. Marble Maze usa la clase **SDKMesh** para cargar las mallas 3D del laberinto y la canica.

Para garantizar que la aplicación responda adecuadamente, Marble Maze carga los recursos de escena de forma asincrónica o en segundo plano. Mientras los activos se cargan en segundo plano, el juego puede responder a los eventos de la ventana. Este proceso se explica con mayor detalle en la sección [Cargar activos del juego en segundo plano](marble-maze-application-structure.md#loading-game-assets-in-the-background) en esta guía.

###  <a name="loading-the-2-d-overlay-and-user-interface"></a>Cargar la superposición 2D y la interfaz de usuario

En Marble Maze, la superposición es la imagen que aparece en la parte superior de la pantalla. La superposición siempre aparece delante de la escena. En Marble Maze, la superposición contiene el logotipo de Windows y la cadena de texto "DirectX Marble Maze game sample" (Muestra del juego Marble Maze con DirectX). La clase **SampleOverlay**, que se define en SampleOverlay.h, se ocupa de la administración de la superposición. Aunque usamos la superposición como parte de las muestras Direct3D, puedes adaptar este código para que muestre cualquier imagen que aparezca delante de tu escena.

Otro aspecto importante de la superposición es que, debido a que su contenido no cambia, la clase **SampleOverlay** dibuja, o almacena en caché, su contenido en un objeto [**ID2D1Bitmap1**](https://msdn.microsoft.com/library/windows/desktop/hh404349) durante la inicialización. En el momento del dibujo, la clase **SampleOverlay** solo tiene que dibujar el mapa de bits en la pantalla. De esta forma, las rutinas costosas, como el dibujo de texto, no deben realizarse para cada trama.

La interfaz de usuario (IU) consiste en componentes 2D, como los menús y las pantallas de aviso (HUD), que aparecen delante de tu escena. Marble Maze define los siguientes elementos de la interfaz de usuario:

-   Elementos de menú que permiten al usuario iniciar el juego o ver las mejores puntuaciones.
-   Un temporizador que cuenta 3 segundos antes de que empiece el juego.
-   Un temporizador que registra el tiempo de juego transcurrido.
-   Una tabla que enumera los tiempos de finalización más rápidos.
-   Texto que indica "Pausa" cuando el juego está en pausa.

Marble Maze define los elementos de la interfaz de usuario del juego en UserInterface.h. Marble Maze define la clase **ElementBase** como un tipo base para todos los elementos de la interfaz de usuario. La clase **ElementBase** define atributos como el tamaño, la posición, la alineación y la visibilidad de un elemento de la interfaz de usuario. También controla cómo se actualizan y se presentan los elementos.

```cpp
class ElementBase
{
public:
    virtual void Initialize() { }
    virtual void Update(float timeTotal, float timeDelta) { }
    virtual void Render() { }

    void SetAlignment(AlignType horizontal, AlignType vertical);
    virtual void SetContainer(const D2D1_RECT_F& container);
    void SetVisible(bool visible);

    D2D1_RECT_F GetBounds();

    bool IsVisible() const { return m_visible; }

protected:
    ElementBase();

    virtual void CalculateSize() { }

    Alignment       m_alignment;
    D2D1_RECT_F     m_container;
    D2D1_SIZE_F     m_size;
    bool            m_visible;
};
```

Al proporcionar una clase base común para los elementos de la interfaz de usuario, la clase **UserInterface**, que administra la interfaz de usuario, solo necesita tener una colección de objetos **ElementBase**, lo que simplifica la administración de la interfaz de usuario y proporciona un administrador de interfaz de usuario reusable. Marble Maze define tipos que derivan de **ElementBase** que implementan comportamientos específicos del juego. Por ejemplo, **HighScoreTable** define el comportamiento de la tabla de puntuaciones máximas. Para obtener más información sobre estos tipos, consulta el código fuente.

> **Nota** Dado que XAML te permite crear interfaces de usuario complejas con mayor facilidad, como las de los juegos de simulación y estrategia, considera la posibilidad de usar XAML para definir la interfaz de usuario. Para obtener información sobre el desarrollo de una interfaz de usuario con XAML en un juego DirectX de UWP, consulta [Extender la muestra de juego (Windows)](tutorial-resources.md). En este documento se hace referencia a la muestra del juego de disparos 3D con DirectX.

 

###  <a name="loading-shaders"></a>Cargar sombreadores

Marble Maze usa el método **BasicLoader::LoadShader** para cargar un sombreador a partir de un archivo.

En la actualidad, los sombreadores son una unidad fundamental de la programación de GPU en los juegos. Prácticamente todos los gráficos 3D se procesan mediante sombreadores, tanto si se trata de una transformación de modelo y la iluminación de una escena, como si se trata de un procesamiento de geometría más compleja, desde el aspecto visual de un personaje hasta la teselación. Para más información sobre el modelo de programación con sombreadores, consulta [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561).

Marble Maze usa sombreadores de vértices y de píxeles. Un sombreador de vértices siempre funciona en un vértice de entrada y produce un vértice como salida. Un sombreador de píxeles toma valores numéricos, datos de textura, valores interpolados por vértice y otros datos para producir un color de píxel como salida. Dado que un sombreador transforma un elemento cada vez, el hardware gráfico que proporciona varias canalizaciones de sombreador puede procesar conjuntos de elementos en paralelo. El número de canalizaciones paralelas disponibles para la GPU puede ser muchísimo mayor que el número disponible para la CPU. Por lo tanto, incluso los sombreadores básicos pueden mejor la capacidad de proceso de forma significativa.

El método **MarbleMaze::LoadDeferredResources** carga un sombreador de vértices y un sombreador de píxeles después de cargar la superposición. Las versiones en tiempo de diseño de estos sombreadores se definen en BasicVertexShader.hlsl y en BasicPixelShader.hlsl, respectivamente. Marble Maze aplica estos sombreadores tanto a la canica como al laberinto durante la fase de representación.

El proyecto Marble Maze incluye tanto la versión .hlsl (el formato en tiempo de diseño) como la versión .cso (el formato en tiempo de ejecución) de los archivos del sombreador. Durante la compilación, Visual Studio usa el compilador de efectos fxc.exe para compilar el archivo .hlsl de origen en un sombreador binario .cso. Para más información acerca de la herramienta compilador de efectos, consulta el tema sobre el [compilador de efectos](https://msdn.microsoft.com/library/windows/desktop/bb232919).

El sombreador de vértices usa las matrices de modelo, vista y proyección suministradas para transformar la geometría de entrada. Los datos de posición de la geometría de entrada se transforman y salen dos veces: una vez en el espacio de pantalla, lo cual es necesario para la representación, y otra vez en el espacio global para permitir al sombreador de píxeles realizar los cálculos de iluminación. El vector normal de superficie se transforma en espacio global, que también usa el sombreador de píxeles para la iluminación. Las coordenadas de textura se pasan sin cambios al sombreador de píxeles.

```hlsl
sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

El sombreador de píxeles recibe los resultados del sombreador de vértices como entrada. Este sombreador realiza cálculos de iluminación para imitar un foco de luz de bordes suaves que se mantiene sobre el laberinto y se alinea con la posición de la canica. La iluminación es mayor para las superficies que apuntan directamente hacia la luz. El componente de difusión va disminuyendo hasta llegar a cero a medida que el valor normal de la superficie se vuelve perpendicular a la luz, y el término ambiente disminuye a medida que el valor normal se aleja de la luz. Los puntos más cercanos a la canica (y por tanto más cerca del centro del foco de luz) se iluminan con más intensidad. Sin embargo, la iluminación se modula para puntos por debajo de la canica para simular una sombra suave. En un entorno real, un objeto como la canica blanca reflejaría de forma difusa el foco de luz en otros objetos de la escena. Esto se intenta con las superficies que están a la vista de la mitad brillante de la canica. Los factores de iluminación adicionales están en un ángulo y una distancia relativos a la canica. El color de píxel resultante es una composición de la textura muestreada con el resultado de los cálculos de iluminación.

```hlsl
float4 main(sPSInput input) : SV_TARGET
{
    float3 lightDirection = float3(0, 0, -1);
    float3 ambientColor = float3(0.43, 0.31, 0.24);
    float3 lightColor = 1 - ambientColor;
    float spotRadius = 50;

    // Basic ambient (Ka) and diffuse (Kd) lighting from above.
    float3 N = normalize(input.norm);
    float NdotL = dot(N, lightDirection);
    float Ka = saturate(NdotL + 1);
    float Kd = saturate(NdotL);

    // Spotlight.
    float3 vec = input.worldPos - marblePosition;
    float dist2D = sqrt(dot(vec.xy, vec.xy));
    Kd = Kd * saturate(spotRadius / dist2D);

    // Shadowing from ball.
    if (input.worldPos.z > marblePosition.z)
        Kd = Kd * saturate(dist2D / (marbleRadius * 1.5));

    // Diffuse reflection of light off ball.
    float dist3D = sqrt(dot(vec, vec));
    float3 V = normalize(vec);
    Kd += saturate(dot(-V, N)) * saturate(dot(V, lightDirection))
        * saturate(marbleRadius / dist3D);

    // Final composite.
    float4 diffuseTexture = Texture.Sample(Sampler, input.tex);
    float3 color = diffuseTexture.rgb * ((ambientColor * Ka) + (lightColor * Kd));
    return float4(color * lightStrength, diffuseTexture.a);
}
```

> **Precaución** El sombreador de píxeles compilado contiene 32 instrucciones aritméticas y 1 instrucción de textura. Este sombreador debería funcionar correctamente en equipos de escritorio y en tabletas de gama alta. Sin embargo, es posible que un equipo de gama baja no pueda procesar este sombreador y aún así pueda proporcionar una velocidad de fotogramas interactiva. Ten en cuenta el hardware típico del público de destino y diseña los sombreadores para que cumplan con las capacidades de ese hardware.

 

El método **MarbleMaze::LoadDeferredResources** usa el método **BasicLoader::LoadShader** para cargar los sombreadores. El siguiente ejemplo carga el sombreador de vértices. El formato en tiempo de ejecución para este sombreador es BasicVertexShader.cso. La variable de miembro **m\_vertexShader** es un objeto [**ID3D11VertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476641).

```cpp
\loader->LoadShader(
    L"BasicVertexShader.cso",
    layoutDesc,
    ARRAYSIZE(layoutDesc),
    &m_vertexShader,
    &m_inputLayout
    );
```

La variable de miembro **m\_inputLayout** es un objeto [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575). El objeto de diseño de entrada encapsula el estado de entrada de la fase del ensamblador de entrada. Una tarea de la fase del ensamblador de entrada es aumentar la eficacia de los sombreadores mediante valores generados por el sistema, también conocidos como *semántica*, para procesar solamente los primitivos o los vértices que aún no se han procesados. Usa el método [**ID3D11Device::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512) para crear un diseño de entrada a partir de una matriz de descripciones de elementos de entrada. La matriz contiene uno o más elementos de entrada; cada uno de ellos describe un elemento de datos de vértice de un búfer de vértices. El conjunto completo de descripciones de elementos de entrada describe todos los elementos de datos de vértice de todos los búferes de vértices que se vincularán a la fase del ensamblador de entrada. El siguiente ejemplo muestra la descripción de diseño que usa Marble Maze. La descripción de diseño describe un búfer de vértices que contiene cuatro elementos de datos de vértice. Las partes importantes de cada entrada de la matriz son el nombre semántico, el formato de datos y el desplazamiento de bytes. Por ejemplo, el elemento **POSITION** especifica la posición del vértice en el espacio del objeto. Comienza con un desplazamiento de byte de 0 y contiene tres componentes de punto flotante (para un total de 12 bytes). El elemento **NORMAL** especifica el vector normal. Comienza con un desplazamiento de byte de 12 porque aparece directamente después de **POSITION** en el diseño, lo que requiere 12 bytes. El elemento **NORMAL** contiene un número entero sin asignar de 32 bits y con cuatro componentes.

```cpp
D3D11_INPUT_ELEMENT_DESC layoutDesc[] = 
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TEXCOORD",  0, DXGI_FORMAT_R32G32_FLOAT,   0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TANGENT", 0, DXGI_FORMAT_R32G32B32_FLOAT,  0, 32, D3D11_INPUT_PER_VERTEX_DATA, 0 }, 
};
m_vertexStride = 44; // You must set this to match the size of layoutDesc above.
```

Compara el diseño de entrada con la estructura **sVSInput** definida por el sombreador de vértices, como se muestra en el siguiente ejemplo. La estructura **sVSInput** define los elementos **POSITION**, **NORMAL** y **TEXCOORD0**. El tiempo de ejecución de DirectX asigna cada elemento del diseño a la estructura de entrada definida por el sombreador.

```hlsl
struct sVSInput
{
    float3 pos : POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
};

struct sPSInput
{
    float4 pos : SV_POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
    float3 worldPos : TEXCOORD1;
};

sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

En el documento [Semántica](https://msdn.microsoft.com/library/windows/desktop/bb509647) se describe cada una de las cadenas de semántica disponibles con más detalle.

> **Nota** En un diseño, puedes especificar componentes adicionales que no se usan para permitir que varios sombreadores compartan el mismo diseño. Por ejemplo, el sombreador no usa el elemento **TANGENT**. Puedes ver el elemento **TANGENT** si quieres experimentar con técnicas como la asignación normal. Al usar la asignación normal, también conocida como mapa de rugosidad, puedes crear el efecto de baches en la superficie de los objetos. Para más información, consulta el tema sobre el [mapa de rugosidad (Direct3D 9)](https://msdn.microsoft.com/library/windows/desktop/bb172379).

 

Para obtener más información sobre el estado de la fase de ensamblado de entrada, consulta los temas sobre la [fase de ensamblador de entrada](https://msdn.microsoft.com/library/windows/desktop/bb205116) y [primeros pasos con la fase de ensamblador de entrada](https://msdn.microsoft.com/library/windows/desktop/bb205117).

El proceso según el cual se usan los sombreadores de vértices y de píxeles para representar la escena se describe en la sección [Representar la escena](#rendering_the_scene) más adelante en este documento.

### <a name="creating-the-constant-buffer"></a>Crear el búfer de constates

El búfer de Direct3D agrupa una colección de datos. Un búfer de constantes es un tipo de búfer que puedes usar para pasarles datos a los sombreadores. Marble Maze usa un búfer de constantes para conservar la vista de modelo (o global) y las matrices de proyección para el objeto activo de la escena.

En el siguiente ejemplo se muestra cómo el método **MarbleMaze::LoadDeferredResources** crea un búfer de constantes que más tarde albergará datos de matriz. El ejemplo crea una estructura **D3D11\_BUFFER\_DESC** que usa la marca **D3D11\_BIND\_CONSTANT\_BUFFER** para especificar el uso como búfer de constantes. A continuación, este ejemplo pasa dicha estructura al método [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501). La variable **m\_constantBuffer** es un objeto [**ID3D11Buffer**](https://msdn.microsoft.com/library/windows/desktop/ff476351).

```cpp
// Create the constant buffer for updating model and camera data.
D3D11_BUFFER_DESC constantBufferDesc = {0};
constantBufferDesc.ByteWidth           = ((sizeof(ConstantBuffer) + 15) / 16) * 16; // Multiple of 16 bytes
constantBufferDesc.Usage               = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags           = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags      = 0;
constantBufferDesc.MiscFlags           = 0;
// This will not be used as a structured buffer, so this parameter is ignored.
constantBufferDesc.StructureByteStride = 0;

DX::ThrowIfFailed(
    m_d3dDevice->CreateBuffer(
        &constantBufferDesc,
        nullptr,             // Leave the buffer uninitialized.
        &m_constantBuffer
        )
    );
```

El método **MarbleMaze::Update** actualiza más tarde los objetos **ConstantBuffer**, uno para el laberinto y otro para la canica. El método **MarbleMaze::Render** vincula cada objeto **ConstantBuffer** al búfer de constantes antes de que se presenten los objetos. En el siguiente ejemplo se muestra la estructura **ConstantBuffer**, que se encuentra en MarbleMaze.h.

```cpp
// Describes the constant buffer that draws the meshes.
struct ConstantBuffer
{
    float4x4 model;
    float4x4 view;
    float4x4 projection;

    float3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

Para comprender mejor cómo se asignan búferes de constantes al código de sombreador, compara la estructura **ConstantBuffer** con el búfer de constantes **SimpleConstantBuffer** definido por el sombreador de vértices en BasicVertexShader.hlsl:

```hlsl
cbuffer ConstantBuffer : register(b0)
{
    matrix model;
    matrix view;
    matrix projection;
    float3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

El diseño de la estructura **ConstantBuffer** coincide con el objeto **cbuffer**. La variable **cbuffer** especifica el registro b0, lo que significa que los datos del búfer de constantes se almacena en el registro 0. El método **MarbleMaze::Render** especifica el registro 0 cuando activa el búfer de constantes. Este proceso se describe con más detalle más adelante en este documento.

Para obtener más información sobre los búferes de constantes, consulta [Introduction to Buffers in Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476898) (Introducción a los búferes en Direct3D 11). Para obtener más información sobre la palabra clave register, consulta [**register**](https://msdn.microsoft.com/library/windows/desktop/dd607359).

###  <a name="loading-meshes"></a>Cargar mallas

Marble Maze usa SDK-Mesh como formato en tiempo de ejecución porque dicho formato proporciona un modo básico de cargar datos de malla para aplicaciones de ejemplo. Para uso de producción, deberías usar un formato de malla que cumpla los requisitos específicos de tu juego.

El método **MarbleMaze::LoadDeferredResources** carga los datos de malla después de cargar los sombreadores de vértices y de píxeles. Una malla es una colección de datos de vértices que a menudo incluye información como posiciones, datos de valores normales, colores, materiales y coordenadas de textura. Las mallas suelen crearse con software de creación 3D y mantenerse en archivos independientes del código de la aplicación. La canica y el laberinto son dos ejemplos de mallas usadas por el juego.

Marble Maze usa la clase **SDKMesh** para administrar mallas. Esta clase se declara en SDKMesh.h. **SDKMesh** proporciona métodos para cargar, representar y destruir datos de malla.

> **Importante** Marble Maze usa el formato SDK-Mesh y proporciona la clase **SDKMesh** solamente a modo de ejemplo. Aunque el formato SDK-Mesh es útil para aprender y crear prototipos, es un formato muy básico que posiblemente no cumpla los requisitos de gran parte del desarrollo de juegos. Te recomendamos usar de un formato de malla que cumpla los requisitos específicos de tu juego.

 

En el siguiente ejemplo se muestra cómo el método **MarbleMaze::LoadDeferredResources** usa el método **SDKMesh::Create** para cargar datos de malla para el laberinto y la bola.

```cpp
// Load the meshes.
DX::ThrowIfFailed(
    m_mazeMesh.Create(
        m_d3dDevice.Get(),
        L"Media\\Models\\maze1.sdkmesh",
        false
        )
    );

DX::ThrowIfFailed(
    m_marbleMesh.Create(
        m_d3dDevice.Get(),
        L"Media\\Models\\marble2.sdkmesh",
        false
        )
    );
```

###  <a name="loading-collision-data"></a>Cargar datos de colisión

Aunque esta sección no se centra en cómo implementa Marble Maze la simulación física entre la canica y el laberinto, observa que la geometría de malla del sistema físico se lee cuando se cargan las mallas.

```cpp
// Extract mesh geometry for the physics system.
DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_walls",
        m_collision.m_wallTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_Floor",
        m_collision.m_groundTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_floorSides",
        m_collision.m_floorTriList
        )
    );

m_physics.SetCollision(&m_collision);
float radius = m_marbleMesh.GetMeshBoundingBoxExtents(0).x / 2;
m_physics.SetRadius(radius);
```

El modo en que se cargan los datos de colisión depende en gran medida del formato en tiempo de ejecución que se use. Para obtener más información sobre cómo Marble Maze carga la geometría de colisión de un archivo SDK-Mesh, consulta el método **MarbleMaze::ExtractTrianglesFromMesh** en el código fuente.

## <a name="updating-game-state"></a>Actualizar el estado del juego


Marble Maze separa la lógica de juego de la lógica de presentación actualizando primero todos los objetos de escena antes de presentarlos.

El documento Estructura de la aplicación Marble Maze describe el bucle principal del juego. La actualización de la escena, que forma parte del bucle del juego, tiene lugar después de que se procesen los eventos y la entrada de Windows y antes de que se presente la escena. El método **MarbleMaze::Update** controla la actualización de la interfaz de usuario y del juego.

### <a name="updating-the-user-interface"></a>Actualizar la interfaz de usuario

El método **MarbleMaze::Update** llamada al método **UserInterface::Update** para actualizar el estado de la interfaz de usuario.

```cpp
UserInterface::GetInstance().Update(timeTotal, timeDelta);
```

El método **UserInterface::Update** actualiza cada elemento de la colección de la interfaz de usuario.

```cpp
void UserInterface::Update(float timeTotal, float timeDelta)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        (*iter)->Update(timeTotal, timeDelta);
    }
}
```

Las clases derivadas de **ElementBase** implementan el método **Update** para realizar comportamientos específicos. Por ejemplo, el método **StopwatchTimer::Update** actualiza el tiempo transcurrido con la cantidad proporcionada y actualiza el texto que muestra más tarde.

```cpp
void StopwatchTimer::Update(float timeTotal, float timeDelta)
{
    if (m_active)
    {
        m_elapsedTime += timeDelta;

        WCHAR buffer[16];
        GetFormattedTime(buffer);
        SetText(buffer);
    }

    TextElement::Update(timeTotal, timeDelta);
}
```

###  <a name="updating-the-scene"></a>Actualizar la escena

El método **MarbleMaze::Update** actualiza el juego según el estado actual de la máquina de estados. Cuando el juego está en modo activo, Marble Maze actualiza la cámara para seguir a la canica, actualiza la parte de la matriz de vistas de los búferes de constantes y actualiza la simulación física.

En el siguiente ejemplo se muestra cómo el método **MarbleMaze::Update** actualiza la posición de la cámara. Marble Maze usa la variable **m\_resetCamera** para indicar que la cámara debe restablecerse para que se posicione directamente por encima de la canica. La cámara se restablece cuando el juego comienza o cuando la canica se cae dentro del laberinto. Cuando el menú principal o la pantalla de máximas puntuaciones están activos, la cámara se establece en una ubicación constante. De lo contrario, Marble Maze usa el parámetro *timeDelta* para interpolar la posición de la cámara entre su posición actual y su posición de destino. La posición de destino es ligeramente por encima y delante de la canica. El uso del tiempo transcurrido entre fotogramas permite a la cámara seguir, o perseguir, de forma gradual a la canica.

```cpp
static float eyeDistance = 200.0f;
static float3 eyePosition = float3(0, 0, 0);

// Gradually move the camera above the marble.
float3 targetEyePosition = marblePosition - (eyeDistance * float3(g.x, g.y, g.z));
if (m_resetCamera)
{
    eyePosition = targetEyePosition;
    m_resetCamera = false;
}
else
{
    eyePosition = eyePosition + ((targetEyePosition - eyePosition) * min(1, timeDelta * 8));
}

// Look at the marble. 
if ((m_gameState == GameState::MainMenu) || (m_gameState == GameState::HighScoreDisplay))
{
    // Override camera position for menus.
    eyePosition = marblePosition + float3(75.0f, -150.0f, -75.0f);
    m_camera->SetViewParameters(eyePosition, marblePosition, float3(0.0f, 0.0f, -1.0f));
}
else
{
    m_camera->SetViewParameters(eyePosition, marblePosition, float3(0.0f, 1.0f, 0.0f));
}
```

En el siguiente ejemplo se muestra cómo el método **MarbleMaze::Update** actualiza los búferes de constantes para la canica y el laberinto. La matriz de modelo, o global, del laberinto siempre permanece como la matriz de identidad. Excepto por la diagonal principal, cuyos elementos son todos unos, la matriz de identidad es una matriz cuadrada compuesta de ceros. La matriz de modelo de la canica se basa en su matriz de posición multiplicada por su matriz de rotación. Las funciones **mul** y **translation** se definen en BasicMath.h.

```cpp
// Update the model matrices based on the simulation.
m_mazeConstantBufferData.model = identity();
m_marbleConstantBufferData.model = mul(
    translation(marblePosition.x, marblePosition.y, marblePosition.z),
    marbleRotationMatrix
    );

// Update the view matrix based on the camera.
float4x4 view;
m_camera->GetViewMatrix(&view);
m_mazeConstantBufferData.view = view;
m_marbleConstantBufferData.view = view;
```

Para obtener información sobre cómo el método **MarbleMaze::Update** lee la entrada del usuario y simula el movimiento de la canica, consulta el tema [Agregar métodos de entrada e interactividad en la muestra de Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md).

## <a name="rendering-the-scene"></a>Presentar la escena


Cuando se presenta una escena, normalmente se incluyen estos pasos.

1.  Establecer el búfer actual de la galería de símbolos de profundidad de destino de presentación.
2.  Borrar las vistas de presentación y de galería de símbolos.
3.  Preparar los sombreadores de vértices y de píxeles para dibujar.
4.  Presentar los objetos 3D de la escena.
5.  Presentar los objetos 2D que desees que aparezcan delante de la escena.
6.  Mostrar la imagen presentada en el monitor.

El método **MarbleMaze::Render** vincula las vistas de destino de presentación y las vistas de galería de símbolos de profundidad, borra esas vistas, dibuja la escena y luego dibuja la superposición.

###  <a name="preparing-the-render-targets"></a>Preparar los destinos de presentación

Antes de representar la escena, debes establecer el búfer actual de la galería de símbolos de profundidad de destino de representación. Si no está garantizado que tu escena dibuje encima de todos los píxeles de la pantalla, borra también las vistas de representación y de galería de símbolos. Marble Maze borra las vistas de presentación y de galería de símbolos en cada fotograma para garantizar que no haya artefactos visibles del fotograma anterior.

En el siguiente ejemplo se muestra cómo el método **MarbleMaze::Render** llama al método [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) para establecer el búfer de destino de presentación y de galería de símbolos de profundidad como los actuales. La clase **DirectXBase** define e inicializa la variable de miembro **m\_renderTargetView** (un objeto [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)) y la variable de miembro **m\_depthStencilView** (un objeto [**ID3D11DepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476377)).

```cpp
// Bind the render targets.
m_d3dContext->OMSetRenderTargets(
    1,
    m_renderTargetView.GetAddressOf(),
    m_depthStencilView.Get()
    );

// Clear the render target and depth stencil to default values. 
const float clearColor[4] = { 0.0f, 0.0f, 0.0f, 1.0f };

m_d3dContext->ClearRenderTargetView(
    m_renderTargetView.Get(),
    clearColor
    );

m_d3dContext->ClearDepthStencilView(
    m_depthStencilView.Get(),
    D3D11_CLEAR_DEPTH,
    1.0f,
    0
    );
```

La interfaz [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) y la interfaz [**ID3D11DepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476377) admiten el mecanismo de vista de textura proporcionado por Direct3D 10 y versiones posteriores. Para obtener más información sobre las vistas de textura, consulta el tema [Texture Views (Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb205128) (Vistas de textura [Direct3D 10]). El método [**OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) prepara la fase de fusión de salida para la canalización de Direct3D. Para más información, consulta el tema sobre la [fase de fusión de salida](https://msdn.microsoft.com/library/windows/desktop/bb205120).

### <a name="preparing-the-vertex-and-pixel-shaders"></a>Preparar los sombreadores de vértices y de píxeles

Antes de presentar los objetos de la escena, sigue estos pasos para preparar los sombreadores de vértices y de píxeles para dibujar:

1.  Establece el diseño de entrada del sombreador como el diseño actual.
2.  Establece los sombreadores de vértices y de píxeles como los sombreadores actuales.
3.  Actualiza todos los búferes de constantes con los datos que debas pasar a los sombreadores.

> **Importante** Marble Maze usa un par de sombreadores de vértices y de píxeles para todos los objetos 3D. Si tu juego usa más de un par de sombreadores, debes realizar estos pasos cada vez que uses objetos que usen diferentes sombreadores. Para reducir la sobrecarga asociada al cambio de estado de los sombreadores, te recomendamos que agrupes las llamadas de representación para todos los objetos que usen los mismos sombreadores.

 

En la sección [Cargar sombreadores](#loading_shaders) de este documento se describe cómo crear el diseño de entrada cuando se crea el sombreador de vértices. En el siguiente ejemplo muestra cómo el método **MarbleMaze::Render** usa el método [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) para establecer este diseño como el diseño actual.

```cpp
m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

En el siguiente ejemplo se muestra como el método **MarbleMaze::Render** usa el método [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) y el método [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) para establecer los sombreadores de vértices y de píxeles como los sombreadores actuales, respectivamente.

```cpp
// Set the vertex shader stage state.
m_d3dContext->VSSetShader(
    m_vertexShader.Get(),   // Use this vertex shader. 
    nullptr,                // Don't use shader linkage.
    0                       // Don't use shader linkage.
    );

// Set the pixel shader stage state.
m_d3dContext->PSSetShader(
    m_pixelShader.Get(),    // Use this pixel shader. 
    nullptr,                // Don't use shader linkage.
    0                       // Don't use shader linkage.
    );

m_d3dContext->PSSetSamplers(
    0,                       // Starting at the first sampler slot
    1,                       // set one sampler binding
    m_sampler.GetAddressOf() // to use this sampler.
    );
```

Una vez que **MarbleMaze::Render** establezca los sombreadores y su diseño de entrada, usa el método [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) para actualizar el búfer de constantes con las matrices de modelo, vista y proyección para el laberinto. El método **UpdateSubresource** copia los datos de matriz de la memoria de CPU a la memoria de GPU. Recuerda que los componentes de modelo y vista de la estructura **ConstantBuffer** se actualizan en el método **MarbleMaze::Update**. A continuación, el método **MarbleMaze::Render** llama al método [**ID3D11DeviceContext::VSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476491) y el método [**ID3D11DeviceContext::PSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476470) para establecer este búfer de constantes como el búfer actual.

```cpp
// Update the constant buffer with the new data.
m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    nullptr,
    &m_mazeConstantBufferData,
    0,
    0
    );

m_d3dContext->VSSetConstantBuffers(
    0,                // Starting at the first constant buffer slot
    1,                // set one constant buffer binding
    m_constantBuffer.GetAddressOf() // to use this buffer.
    );

m_d3dContext->PSSetConstantBuffers(
    0,                // Starting at the first constant buffer slot
    1,                // set one constant buffer binding
    m_constantBuffer.GetAddressOf() // to use this buffer.
    );
```

El método **MarbleMaze::Render** realiza pasos similares para preparar la presentación de la canica.

### <a name="rendering-the-maze-and-the-marble"></a>Representar el laberinto y la canica

Una vez activados los sombreadores actuales, puedes dibujar los objetos de la escena. El método **MarbleMaze::Render** llamada al método **SDKMesh::Render** para presentar la malla del laberinto.

```cpp
m_mazeMesh.Render(m_d3dContext.Get(), 0, INVALID_SAMPLER_SLOT, INVALID_SAMPLER_SLOT);
```

El método **MarbleMaze::Render** realiza pasos similares para presentar la canica.

Como se mencionó anteriormente en este documento, la clase **SDKMesh** se proporciona con fines de demostración, pero no recomendamos su uso en un juego con calidad de producción. Sin embargo, observa que el método **SDKMesh::RenderMesh**, que es llamado por **SDKMesh::Render**, usa el método [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) y el método [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) para establecer los búferes actuales de vértices e índices que definen la malla. También usa el método [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476410) para dibujar los búferes. Para obtener más información sobre cómo trabajar con búferes de vértices y de índices, consulta el tema [Introduction to Buffers in Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476898) (Introducción a los búferes en Direct3D 11).

### <a name="drawing-the-user-interface-and-overlay"></a>Dibujar la interfaz de usuario y la superposición

Una vez dibujados los objetos 3D de la escena, Marble Maze dibuja los elementos 2D de la interfaz de usuario que aparecen delante de la escena.

El método **MarbleMaze::Render** termina dibujando la interfaz de usuario y la superposición.

```cpp
// Draw the user interface and the overlay.
UserInterface::GetInstance().Render();

m_sampleOverlay->Render();
```

El método **UserInterface::Render** usa un objeto [**ID2D1DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/hh404479) para dibujar los elementos de la interfaz de usuario. Este método establece el estado de dibujo, dibuja todos los elementos de la interfaz de usuario activos y luego restaura el estado de dibujo anterior.

```cpp
void UserInterface::Render()
{
    m_d2dContext->SaveDrawingState(m_stateBlock.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if ((*iter)->IsVisible())
            (*iter)->Render();
    }

    m_d2dContext->EndDraw();
    m_d2dContext->RestoreDrawingState(m_stateBlock.Get());
}
```

El método **SampleOverlay::Render** usa una técnica similar para dibujar el mapa de bits de la superposición.

###  <a name="presenting-the-scene"></a>Presentar la escena

Una vez dibujados todos los objetos 2D y 3D de la escena, Marble Maze presenta la imagen representada en el monitor. Sincroniza el dibujo con el espacio en blanco vertical para garantizar que no se pierde tiempo dibujando fotogramas que nunca se mostrarán realmente en la pantalla. Marble Maze también controla los cambios de dispositivo cuando presenta la escena.

Una vez que se devuelve el método **MarbleMaze::Render**, el bucle del juego llama al método **MarbleMaze::Present** para enviar la imagen presentada al monitor o pantalla. La clase **MarbleMaze** no reemplaza el método **DirectXBase::Present**. El método **DirectXBase::Present** llama a [**IDXGISwapChain1::Present**](https://msdn.microsoft.com/library/windows/desktop/hh446797) para realizar la operación de presentación, tal como se muestra en el siguiente ejemplo:

```cpp
// The application may optionally specify "dirty" or "scroll" rects 
// to improve efficiency in certain scenarios. 
// In this sample, however, we do not utilize those features.
DXGI_PRESENT_PARAMETERS parameters = {0};
parameters.DirtyRectsCount = 0;
parameters.pDirtyRects = nullptr;
parameters.pScrollRect = nullptr;
parameters.pScrollOffset = nullptr;

// The first argument instructs DXGI to block until VSync, putting the  
// application to sleep until the next VSync.  
// This ensures we don't waste any cycles rendering frames that will  
// never be displayed to the screen.
HRESULT hr = m_swapChain->Present1(1, 0, &parameters);
```

En este ejemplo, **m\_swapChain** es un objeto [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631). La inicialización de este objeto se describe en la sección [Inicializar Direct3D y Direct2D](#initializing) en este documento.

El primer parámetro para [**IDXGISwapChain1::Present**](https://msdn.microsoft.com/library/windows/desktop/hh446797), *SyncInterval*, especifica el número de espacios en blanco verticales que hay que esperar antes de presentar el fotograma. Marble Maze especifica 1 de modo que espera hasta el siguiente espacio en blanco vertical. Un espacio en blanco vertical es el tiempo que pasa entre que se termina de dibujar un fotograma en el monitor y comienza el siguiente fotograma.

El método [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) devuelve un código de error que indica que el dispositivo se quitó o experimentó algún tipo de error. En este caso, Marble Maze reinicializa el dispositivo.

```cpp
// Reinitialize the renderer if the device was disconnected  
// or the driver was upgraded. 
if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    Initialize(m_window, m_dpi);
}
else
{
    DX::ThrowIfFailed(hr);
}
```

## <a name="next-steps"></a>Pasos siguientes


Consulta [Agregar métodos de entrada e interactividad en la muestra de Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md) para obtener información sobre algunos de los procedimientos clave a tener en cuenta cuando trabajes con dispositivos de entrada. En este documento se describe cómo Marble Maze admite la entrada mediante función táctil, acelerómetro, controlador de Xbox 360 o mouse.

## <a name="related-topics"></a>Temas relacionados


* [Agregar métodos de entrada e interactividad en la muestra de Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Estructura de la aplicación Marble Maze](marble-maze-application-structure.md)
* [Desarrollar Marble Maze, un juego para UWP en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 







<!--HONumber=Dec16_HO1-->


