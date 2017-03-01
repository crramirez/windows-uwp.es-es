---
author: mtoepke
title: Interoperabilidad de DirectX y XAML
description: Puedes usar el lenguaje XAML y Microsoft DirectX juntos en tu juego para la Plataforma universal de Windows (UWP).
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, juegos, games, directx, interoperabilidad de xaml, xaml interop
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 6934ac8bfbff487e57d0097cb129faf853a3eb9f
ms.lasthandoff: 02/07/2017

---

# <a name="directx-and-xaml-interop"></a>Interoperabilidad de DirectX y XAML


\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Puedes usar el lenguaje XAML y Microsoft DirectX juntos en tu juego para la Plataforma universal de Windows (UWP). La combinación de XAML y DirectX te permite crear marcos de interfaz de usuario flexibles que interoperan con el contenido representado en DirectX; además, es especialmente útil para aplicaciones que hacen un uso intensivo de los elementos gráficos. En este tema explicamos la estructura de una aplicación para UWP que usa DirectX y enumeramos los tipos importantes que se deben usar al generar una aplicación para UWP de modo que funcione con DirectX.

Si la aplicación se centra principalmente en la representación 2D, es aconsejable usar la biblioteca [**Win2D**](https://github.com/microsoft/win2d) de Windows Runtime. Esta biblioteca la mantiene Microsoft y se basa en las tecnologías básicas de Direct2D . Simplifica en gran medida el modelo de uso para implementar gráficos 2D e incluye abstracciones útiles para algunas de las técnicas descritas en este documento. Consulta la página del proyecto para obtener más detalles. Este documento ofrece orientación para aquellos desarrolladores de aplicaciones que decidan *no* utilizar Win2D.

> **Nota** Las API de DirectX no están definidas como tipos de Windows Runtime, por lo que se suelen usar extensiones de componentes de Visual C++ (C++/CX) para desarrollar componentes XAML para UWP que interoperen con DirectX. También puedes crear una aplicación para UWP con C# y XAML que use DirectX. Para ello, debes encapsular las llamadas a DirectX en un archivo de metadatos de Windows Runtime independiente.

 

## <a name="xaml-and-directx"></a>XAML y DirectX

DirectX proporciona dos bibliotecas muy eficaces para gráficos 2D y 3D: Direct2D y Microsoft Direct3D. Aunque XAML proporciona compatibilidad con primitivas y efectos 2D, muchas aplicaciones (como las de modelado o los juegos) necesitan funcionalidades gráficas más complejas. En estas aplicaciones, puedes usar Direct2D y Direct3D para representar una parte, o la totalidad, de los elementos gráficos y emplear XAML para todo lo demás.

Si vas a implementar interoperabilidad entre XAML y DirectX personalizada, debes conocer estos dos conceptos:

-   Las superficies compartidas son regiones de la pantalla con un tamaño establecido y definidas por XAML, en las que puedes usar DirectX para dibujar indirectamente, mediante los tipos [**Windows::UI::Xaml::Media::ImageSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imagesource.aspx). En las superficies compartidas no controlas el momento preciso en el que el nuevo contenido aparece en pantalla. En lugar de eso, las actualizaciones de una superficie compartida se sincronizan con las actualizaciones del marco XAML.
-   Las [cadenas de intercambio ](https://msdn.microsoft.com/library/windows/desktop/bb206356(v=vs.85).aspx) representan un conjunto de búferes usados para mostrar gráficos con una latencia mínima. Por lo general, las cadenas de intercambio se actualizan a 60 fotogramas por segundo, independientes del subproceso de la interfaz de usuario. Sin embargo, las cadenas de intercambio usan más recursos de memoria y CPU para permitir actualizaciones rápidas y son más difíciles de usar, ya que se deben administrar varios subprocesos.

Piensa para qué vas a usar DirectX. ¿Lo usarás para componer o animar un control único que se ajuste a las dimensiones de la ventana de presentación? ¿Contiene salida que tenga que representarse y controlarse en tiempo real, como sucede en un juego? Si es así, probablemente tendrás que implementar una cadena de intercambio. De lo contrario, debería bastar con usar una superficie compartida.

Una vez que hayas determinado cómo quieres usar DirectX, usarás uno de estos tipos de Windows en tiempo de ejecución para incorporar la representación de DirectX a la aplicación de la Tienda Windows:

-   Si quieres crear una imagen estática o dibujar una imagen compleja en intervalos controlados por eventos, dibuja en una superficie compartida con [**Windows::UI::Xaml::Media::Imaging::SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041). Este tipo controla una superficie de dibujo de DirectX de un tamaño definido. Normalmente se usa este tipo al crear una imagen o textura, como un mapa de bits, que luego se mostrará en un documento o elemento de interfaz de usuario. Recuerda que no funciona bien con la interactividad en tiempo real como, por ejemplo, un juego de alto rendimiento. Esto se debe a que las actualizaciones de un objeto **SurfaceImageSource** se sincronizan con las actualizaciones de la interfaz de usuario XAML; por ello, se puede producir una latencia en la información visual que se proporciona al usuario como, por ejemplo, una velocidad de fotogramas fluctuante o una respuesta deficiente para las entradas en tiempo real. De todos modos, las actualizaciones son lo suficientemente rápidas como para realizar controles dinámicos o simulaciones de datos.

-   Si la imagen es más grande que el estado real de la pantalla proporcionada y el usuario puede realizar una panorámica o acercar o alejar la imagen, usa [**Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050). Este tipo controla una superficie de dibujo de DirectX de un tamaño definido que es mayor que el de la pantalla. Tal como sucede con [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041), usarás este tipo dinámicamente al componer una imagen o control complejos. Asimismo, al igual que **SurfaceImageSource**, este tipo no funciona bien en juegos de alto rendimiento. Algunos ejemplos de elementos XAML que pueden usar un tipo **VirtualSurfaceImageSource**, son los controles de mapa o un visor de documentos con imágenes grandes o de alta densidad.

-   Si usas DirectX para presentar gráficos actualizados en tiempo real o en una situación en la que las actualizaciones deben llegar en intervalos regulares de baja latencia, usa la clase [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) para poder actualizar los elementos gráficos sin tener que sincronizar el temporizador de actualizaciones del marco XAML. Este tipo te permite acceder a la cadena de intercambio del dispositivo gráfico ([**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)) directamente y disponer XAML como una capa sobre el destino de la representación. Funciona muy bien en juegos y aplicaciones DirectX de pantalla completa que requieren una interfaz de usuario basada en XAML. Para usar este método debes ser bastante ducho en DirectX, incluidas las tecnologías Infraestructura de gráficos de Microsoft DirectX (DXGI), Direct2D y Direct3D. Para obtener más información, consulta [Programming Guide for Direct3D 11 (Guía de programación para Direct3 11)](https://msdn.microsoft.com/library/windows/desktop/ff476345).

## <a name="surfaceimagesource"></a>SurfaceImageSource


[**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) proporciona superficies compartidas de DirectX para dibujar en ellas y luego compone los fragmentos para crear el contenido de la aplicación.

El procedimiento básico para crear y actualizar un objeto [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) en el código subyacente es:

1.  Definir el tamaño de la superficie compartida pasando los valores de alto y ancho al constructor [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041). También puedes indicar si la superficie necesita compatibilidad alfa (opacidad).

    Por ejemplo:

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  Obtén un puntero para [**ISurfaceImageSourceNative**](https://msdn.microsoft.com/library/windows/desktop/hh848322). Convierte el objeto [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) en [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821) (o **IUnknown**), y llama a **QueryInterface** para obtener la implementación subyacente **ISurfaceImageSourceNative**. Puedes usar los métodos definidos en esta implementación para establecer el dispositivo y ejecutar las operaciones de dibujo.

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNative> m_sisNative;
    // ...
    IInspectable* sisInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);
    sisInspectable->QueryInterface(__uuidof(ISurfaceImageSourceNative), (void **)&m_sisNative);
    ```

3.  Establece el dispositivo DXGI llamando primero a [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) y, a continuación, pasa el dispositivo y el contexto a [**ISurfaceImageSourceNative::SetDevice**](https://msdn.microsoft.com/library/windows/desktop/hh848325). Por ejemplo:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device>              m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext>           m_d3dContext;
    // ...
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext
            )
        );  
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    // ...
    m_sisNative->SetDevice(dxgiDevice.Get());
    ```

4.  Proporciona un puntero al objeto [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565) de [**ISurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) y dibuja en esa superficie mediante DirectX. Solo se dibujará el área especificada para su actualización en el parámetro *updateRect*.

    > **Nota** Solo puedes tener una operación [**BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) pendiente en cada momento por cada [**IDXGIDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174527).

     

    Este método devuelve el desplazamiento del punto (x,y) del rectángulo de destino actualizado en el parámetro *offset*. Este desplazamiento se usa para determinar en qué lugar de [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565) se debe dibujar.

    ```cpp
    ComPtr<IDXGISurface> surface;

    HRESULT beginDrawHR = m_sisNative->BeginDraw(updateRect, &surface, &offset);
    if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED || beginDrawHR == DXGI_ERROR_DEVICE_RESET)
    {
              // Device changed
    }
    else
    {
        // Draw to IDXGISurface (the surface paramater)
    }
    ```

5.  Llama a [**ISurfaceImageSourceNative::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848324) para completar el mapa de bits. El mapa de bits puede usarse como origen de un elemento [**Image**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.image.aspx) o [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) de XAML.

    ```cpp
    m_sisNative->EndDraw();
    // ...
    // The SurfaceImageSource object's underlying ISurfaceImageSourceNative object contains the completed bitmap.
    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

> **Nota** Actualmente, si llamas al elemento [**SurfaceImageSource::SetSource**](https://msdn.microsoft.com/library/windows/apps/br243255) (heredado de **IBitmapSource::SetSource**), se inicia una excepción. No lo llames desde el objeto [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041).

 

## <a name="virtualsurfaceimagesource"></a>VirtualSurfaceImageSource


[**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) extiende el elemento [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) cuando el contenido es potencialmente superior al espacio en pantalla, por lo que hay que virtualizar el contenido para representarlo de forma óptima.

[**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) difiere de [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) en que usa una devolución de llamada, [**IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848337), que se implementa para actualizar regiones de la superficie a medida que se visualizan en la pantalla. No tienes que borrar regiones que estén ocultas; el marco XAML se encarga de ello.

A continuación encontrarás el procedimiento básico para crear y actualizar un objeto [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) en el código subyacente:

1.  Crea una instancia de [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) con el tamaño que quieras. Por ejemplo:

    `VirtualSurfaceImageSource^ virtualSIS = ref new VirtualSurfaceImageSource(2000, 2000);`

2.  Obtén un puntero para [**IVirtualSurfaceImageSourceNative**](https://msdn.microsoft.com/library/windows/desktop/hh848328). Convierte el objeto [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) en [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821) o en [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509), y llama a [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) para obtener la implementación subyacente **IVirtualSurfaceImageSourceNative**. Puedes usar los métodos definidos en esta implementación para establecer el dispositivo y ejecutar las operaciones de dibujo.

    ```cpp
    Microsoft::WRL::ComPtr<IVirtualSurfaceImageSourceNative>  m_vsisNative;
    // ...
    IInspectable* vsisInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(virtualSIS);
    vsisInspectable->QueryInterface(__uuidof(IVirtualSurfaceImageSourceNative), (void **)&m_vsisNative);
    ```

3.  Establece el dispositivo DXGI llamando a [**IVirtualSurfaceImageSourceNative::SetDevice**](https://msdn.microsoft.com/library/windows/desktop/hh848325). Por ejemplo:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device>              m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext>           m_d3dContext;
    // ...
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext
            )
        );  
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    // ...
    m_vsisNative->SetDevice(dxgiDevice.Get());
    ```

4.  Llama a [**IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848334) y pasa una referencia a la implementación de [**IVirtualSurfaceUpdatesCallbackNative**](https://msdn.microsoft.com/library/windows/desktop/hh848336).

    ```cpp
    class MyContentImageSource : public IVirtualSurfaceUpdatesCallbackNative
    {
    // ...
      private:
         virtual HRESULT STDMETHODCALLTYPE UpdatesNeeded() override;
    }

    // ...

    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
      // .. Perform drawing here ...
    }
    void MyContentImageSource::Initialize()
    {
      // ...
      m_vsisNative->RegisterForUpdatesNeeded(this);
      // ...
    }
    ```

    El marco llama a la implementación de [**IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848334) cuando hay que actualizar un región de la clase [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050).

    Esto puede suceder cuando el marco determina que es necesario dibujar la región (como cuando el usuario muestra una vista panorámica o acerca o aleja la vista de la superficie), o después de que la aplicación llame a [**IVirtualSurfaceImageSourceNative::Invalidate**](https://msdn.microsoft.com/library/windows/desktop/hh848332) en esa región.

5.  En [**IVirtualSurfaceImageSourceNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848337), usa los métodos [**IVirtualSurfaceImageSourceNative::GetUpdateRectCount**](https://msdn.microsoft.com/library/windows/desktop/hh848329) y [**IVirtualSurfaceImageSourceNative::GetUpdateRects**](https://msdn.microsoft.com/library/windows/desktop/hh848330) para determinar qué regiones de la superficie deben dibujarse.

    ```cpp
    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
        HRESULT hr = S_OK;

        try
        {
            ULONG drawingBoundsCount = 0;  

                  m_vsisNative->GetUpdateRectCount(&drawingBoundsCount);
            std::unique_ptr<RECT[]> drawingBounds(new RECT[drawingBoundsCount]);
            m_vsisNative->GetUpdateRects(drawingBounds.get(), drawingBoundsCount);
            
            for (ULONG i = 0; i < drawingBoundsCount; ++i)
            {
                // Drawing code here ...
            }
        }
        catch (Platform::Exception^ exception)
        {
            hr = exception->HResult;
        }

        return hr;
    }
    ```

6.  Por último, debes realizar lo siguiente para cada región que hay que actualizar:

    1.  Proporciona un puntero al objeto [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565) de [**IVirtualSurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) y dibuja en esa superficie mediante DirectX. Solo se dibujará el área especificada para actualización en el parámetro *updateRect*.

        Tal como sucede con [**IlSurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323), este método devuelve el desplazamiento del punto (x,y) del rectángulo de destino actualizado en el parámetro *offset*. Puedes usar este desplazamiento para determinar en qué lugar de [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565) se debe dibujar.

        > **Nota** Solo puedes tener una operación [**BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) pendiente en cada momento por cada [**IDXGIDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174527).

         

        ```cpp
        ComPtr<IDXGISurface> bigSurface;

        HRESULT beginDrawHR = m_vsisNative->BeginDraw(updateRect, &bigSurface, &offset);
        if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED || beginDrawHR == DXGI_ERROR_DEVICE_RESET)
        {
                  // Device changed
        }
        else
        {
            // Draw to IDXGISurface
        }
        ```

    2.  Dibuja el contenido específico de la región, pero restringe el dibujo a las regiones enlazadas, lo que mejora el rendimiento.

    3.  Llama a [**IVirtualSurfaceImageSourceNative::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848324). El resultado es un mapa de bits.

## <a name="swapchainpanel-and-gaming"></a>SwapChainPanel y juegos


[**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) es el tipo de Windows Runtime diseñado para permitir elementos gráficos y juegos de alto rendimiento en los que puedes administrar la cadena de intercambio directamente. En este caso, puedes crear tu propia cadena de intercambio de DirectX y administrar la presentación del contenido representado.

Para garantizar un buen rendimiento, el tipo [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) tiene algunas limitaciones:

-   No hay más de 4 instancias de [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) por aplicación.
-   Debes establecer los valores de alto y ancho de la cadena de intercambio de DirectX (en [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) según las dimensiones actuales del elemento de dicha cadena de intercambio. Si no, el contenido de la pantalla se escalará (mediante **DXGI\_SCALING\_STRETCH**) para ajustarlo.
-   Debes establecer el modo de escalado de la cadena de intercambio de DirectX (en [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) a **DXGI\_SCALING\_STRETCH**.
-   No puedes establecer el modo alfa de la cadena de intercambio de DirectX (en [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) a **DXGI\_ALPHA\_MODE\_PREMULTIPLIED**.
-   Debes crear la cadena de intercambio de DirectX llamando a [**IDXGIFactory2::CreateSwapChainForComposition**](https://msdn.microsoft.com/library/windows/desktop/hh404558).

Debes actualizar el elemento [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) en función de las necesidades de la aplicación y no de las actualizaciones del marco XAML. Si necesitas sincronizar las actualizaciones de **SwapChainPanel** con las del marco XAML, regístrate para obtener el evento [**Windows::UI::Xaml::Media::CompositionTarget::Rendering**](https://msdn.microsoft.com/library/windows/apps/br228127). En caso contrario, deberás tener en cuenta los posibles problemas entre subprocesos al intentar actualizar los elementos XAML de un subproceso distinto al que actualiza el objeto **SwapChainPanel**.

Si tienes que recibir una entrada de puntero de latencia baja en tu **SwapChainPanel**, usa [**SwapChainPanel::CreateCoreIndependentInputSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.swapchainpanel.createcoreindependentinputsource). Este método devuelve un objeto [**CoreIndependentInputSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coreindependentinputsource) que puede usarse para recibir eventos de entrada con una latencia mínima en un subproceso en segundo plano. Ten en cuenta que, cuando se llama a este método, no se desencadenará eventos normales de entrada de puntero XAML para **SwapChainPanel**, dado que todas las entradas se redirigirán al subproceso en segundo plano.


> **Nota** En general, las aplicaciones DirectX deben crear cadenas de intercambio con orientación horizontal y con el mismo tamaño que la ventana de presentación (que suele ser la resolución de la pantalla nativa en la mayoría de juegos de la Tienda Windows). De esta forma, se garantiza que la aplicación implemente de manera óptima la cadena de intercambio cuando no tenga ninguna superposición de XAML visible. Si la aplicación se gira y se coloca en modo vertical, esta debería llamar a [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801) en la cadena de intercambio existente, aplicar una transformación al contenido si fuera necesario y luego llamar de nuevo a [**SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144) en la misma cadena de intercambio. De forma similar, la aplicación debe llamar de nuevo a **SetSwapChain** en la misma cadena de intercambio, siempre que se cambie el tamaño de la cadena de intercambio mediante una llamada a [**IDXGISwapChain::ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577).


 

El procedimiento básico para crear y actualizar un objeto [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) en el código subyacente es el siguiente:

1.  Obtén una instancia de un panel de la cadena de intercambio para la aplicación. Las instancias se indican en el lenguaje XAML con la etiqueta `<SwapChainPanel>`.

    `Windows::UI::Xaml::Controls::SwapChainPanel^ swapChainPanel;`

    A continuación se muestra un ejemplo de la etiqueta `<SwapChainPanel>`.

    ```xml
    <SwapChainPanel x:Name="swapChainPanel">
        <SwapChainPanel.ColumnDefinitions>
            <ColumnDefinition Width="300*"/>
            <ColumnDefinition Width="1069*"/>
        </SwapChainPanel.ColumnDefinitions>
    …
    ```

2.  Obtén un puntero para [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143). Convierte el objeto [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) en [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821) (o **IUnknown**), y llama a **QueryInterface** para obtener la implementación subyacente **ISwapChainPanelNative**.

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  Crea el dispositivo DXGI y la cadena de intercambio, y establece la cadena de intercambio en [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143) pasándola a [**SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144).

    ```cpp
    Microsoft::WRL::ComPtr<IDXGISwapChain1>               m_swapChain;    
    // ...
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
            swapChainDesc.Width = m_bounds.Width;
            swapChainDesc.Height = m_bounds.Height;
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;           // This is the most common swapchain format.
            swapChainDesc.Stereo = false; 
            swapChainDesc.SampleDesc.Count = 1;                          // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Quality = 0;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.BufferCount = 2;
            swapChainDesc.Scaling = DXGI_SCALING_STRETCH;
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // We recommend using this swap effect for all. applications
            swapChainDesc.Flags = 0;
                    
    // QI for DXGI device
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // Get the DXGI adapter.
    Microsoft::WRL::ComPtr<IDXGIAdapter> dxgiAdapter;
    dxgiDevice->GetAdapter(&dxgiAdapter);

    // Get the DXGI factory.
    Microsoft::WRL::ComPtr<IDXGIFactory2> dxgiFactory;
    dxgiAdapter->GetParent(__uuidof(IDXGIFactory2), &dxgiFactory);
    // Create a swap chain by calling CreateSwapChainForComposition.
    dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,        // Allow on any display. 
                &m_swapChain
                );
            
    m_swapChainNative->SetSwapChain(m_swapChain.Get());
    ```

4.  Dibuja en la cadena de intercambio de DirectX y preséntala para mostrar el contenido.

    ```cpp
    HRESULT hr = m_swapChain->Present(1, 0);
    ```

    Los elementos XAML se actualizan cuando el diseño o la lógica de representación de Windows Runtime envían una señal de actualización.

## <a name="related-topics"></a>Temas relacionados

* [**Win2D**](http://microsoft.github.io/Win2D/html/Introduction.htm)
* [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)
* [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)
* [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)
* [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143)
* [Programming Guide for Direct3D 11 (Guía de programación para Direct3D 11)](https://msdn.microsoft.com/library/windows/desktop/ff476345)

 

 





