---
author: mtoepke
title: Interoperabilidad de DirectX y XAML
description: Puedes usar el lenguaje XAML y Microsoft DirectX juntos en tu juego para la Plataforma universal de Windows (UWP).
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
---

# Interoperabilidad de DirectX y XAML


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Puedes usar el lenguaje XAML y Microsoft DirectX juntos en tu juego para la Plataforma universal de Windows (UWP). La combinación de XAML y DirectX te permite crear marcos de interfaz de usuario flexibles que interoperan con el contenido representado en DirectX; además, es especialmente útil para aplicaciones que hacen un uso intensivo de los elementos gráficos. En este tema explicamos la estructura de una aplicación para UWP que usa DirectX y enumeramos los tipos importantes que se deben usar al generar una aplicación para UWP de modo que funcione con DirectX.

> **Nota**  Las API de DirectX no están definidas como tipos de Windows Runtime, por lo que se suelen usar extensiones de componentes de Visual C++ (C++/CX) para desarrollar componentes XAMLUWP que interoperen con DirectX. También puedes crear una aplicación para UWP con C# y XAML que use DirectX. Para ello, debes encapsular las llamadas a DirectX en un archivo de metadatos de Windows Runtime aparte.

 

## XAML y DirectX


DirectX proporciona dos bibliotecas muy eficaces para gráficos 2D y 3D: Direct2D y Microsoft Direct3D. Aunque XAML proporciona compatibilidad con primitivas y efectos 2D, muchas aplicaciones (como las de modelado o los juegos) necesitan funcionalidades gráficas más complejas. En estas aplicaciones puedes usar Direct2D y Direct3D para representar una parte (o la totalidad) de los elementos gráficos y usar XAML para todo lo demás.

Para el escenario de interoperación de XAML y DirectX debes conocer estos dos conceptos:

-   Las superficies compartidas son regiones de la pantalla con un tamaño establecido y definidas por XAML, en las que puedes hacer que DirectX dibuje indirectamente mediante los tipos [**Windows::UI::Xaml::Media::Brush**](https://msdn.microsoft.com/library/windows/apps/br228076). En el caso de las superficies compartidas, no controlas las llamadas para presentar las cadenas de intercambio. Las actualizaciones de una superficie compartida se sincronizan con las actualizaciones del marco XAML.
-   La cadena de intercambio en sí. Proporciona el búfer subyacente de la canalización de representación de DirectX, la parte de la memoria disponible para la presentación una vez completado el objetivo de representación.

Piensa para qué vas a usar DirectX. ¿Lo usarás para componer o animar un control único que se ajuste a las dimensiones de la ventana de presentación? ¿Podría taparse la superficie compuesta con otras superficies o los bordes de la pantalla? ¿Contiene salida que tenga que ser representada y controlada en tiempo real, como en un juego?

Una vez que has determinado cómo quieres usar DirectX, usas uno de estos tipos de Windows en tiempo de ejecución para incorporar representación de DirectX en la aplicación de la Tienda Windows:

-   Si quieres crear una imagen estática o dibujar una imagen compleja en intervalos controlados por eventos, dibuja en una superficie compartida con [**Windows::UI::Xaml::Media::Imaging::SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041). Este tipo controla una superficie de dibujo de DirectX de un tamaño definido. Normalmente se usa este tipo al crear una imagen o textura, como un mapa de bits, que luego se mostrará en un documento o elemento de interfaz de usuario. Recuerda que no funciona bien con la interactividad en tiempo real como, por ejemplo, un juego de alto rendimiento. Esto se debe a que las actualizaciones de un objeto **SurfaceImageSource** se sincronizan con las actualizaciones de la interfaz de usuario XAML; por ello, se puede producir una latencia en la información visual que se proporciona al usuario como, por ejemplo, una velocidad de fotogramas fluctuante o una respuesta deficiente para las entradas en tiempo real. De todos modos, las actualizaciones son lo suficientemente rápidas para realizar controles dinámicos o simulaciones de datos.

    [
              Los objetos gráficos **SurfaceImageSource**
            ](https://msdn.microsoft.com/library/windows/apps/hh702041) se pueden componer con otros elementos de interfaz de usuario XAML. Puedes transformarlos o proyectarlos y el marco XAML respetará los valores de opacidad o de índice z.

-   Si la imagen es más grande que el estado real de la pantalla proporcionada y el usuario puede realizar una panorámica o acercar o alejar la imagen, usa [**Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050). Este tipo controla una superficie de dibujo de DirectX de un tamaño definido que es mayor que el de la pantalla. Tal como sucede con [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041), usarás este tipo dinámicamente al componer una imagen o control complejos. Asimismo, al igual que **SurfaceImageSource**, este tipo no funciona bien en juegos de alto rendimiento. Algunos ejemplos de elementos XAML que pueden usar un tipo **VirtualSurfaceImageSource**, son los controles de mapa o un visor de documentos con imágenes grandes o de alta densidad.

-   Si usas DirectX para presentar gráficos actualizados en tiempo real o en una situación en la que las actualizaciones deben llegar en intervalos regulares de baja latencia, usa la clase [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) para poder actualizar los elementos gráficos sin tener que sincronizar el temporizador de actualizaciones del marco XAML. Este tipo te permite acceder a la cadena de intercambio del dispositivo gráfico ([**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)) directamente y disponer XAML como una capa sobre el destino de representación. Igualmente, funciona muy bien en juegos y otras aplicaciones DirectX de pantalla completa que requieren una interfaz de usuario basada en XAML. Para usar este método debes ser bastante ducho en DirectX, incluidas las tecnologías Infraestructura de gráficos de Microsoft DirectX (DXGI), Direct2D y Direct3D. Para más información, consulta [Guía de programación para Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345).

## SurfaceImageSource


[
              **SurfaceImageSource**
            ](https://msdn.microsoft.com/library/windows/apps/hh702041) proporciona superficies compartidas de DirectX para dibujar en ellas y luego componer los fragmentos para crear el contenido de la aplicación.

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

    > **Nota**   Solo puedes tener una operación [**BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) pendiente a la vez por [**IDXGIDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174527).

     

    Este método devuelve el desplazamiento del punto (x,y) del rectángulo de destino actualizado en el parámetro *offset*. Puedes usar este desplazamiento para determinar en qué lugar de [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565) se debe dibujar.

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

5.  Llama a [**ISurfaceImageSourceNative::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848324) para completar el mapa de bits. Pasa este mapa de bits a un elemento [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101).

    ```cpp
    m_sisNative->EndDraw();
    // ...
    // The SurfaceImageSource object's underlying ISurfaceImageSourceNative object contains the completed bitmap.
    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

6.  Usa el elemento [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) para dibujar el mapa de bits.

> **Note**   Si llamas al elemento [**SurfaceImageSource::SetSource**](https://msdn.microsoft.com/library/windows/apps/br243255) (heredado de **IBitmapSource::SetSource**), este iniciará una excepción. No lo llames desde el objeto [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041).

 

## VirtualSurfaceImageSource


[
              **VirtualSurfaceImageSource**
            ](https://msdn.microsoft.com/library/windows/apps/hh702050) extiende el elemento [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) cuando el contenido es potencialmente más grande que lo que se puede mostrar en pantalla, por lo que hay que virtualizar el contenido para representarlo de forma óptima.

[
              **VirtualSurfaceImageSource**
            ](https://msdn.microsoft.com/library/windows/apps/hh702050) difiere de [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) en que usa la devolución de llamada, [**IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848337), que se implementó para actualizar regiones de la superficie a medida que se visualizan en la pantalla. No tienes que borrar regiones que estén ocultas; el marco XAML se encarga de ello.

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

        > **Nota**   Solo puedes tener una operación [**BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) pendiente a la vez por [**IDXGIDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174527).

         

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

    2.  Dibuja el contenido específico de la región, pero restringe el dibujo a las regiones enlazadas, para un mejor rendimiento.

    3.  Llama a [**IVirtualSurfaceImageSourceNative::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848324). El resultado es un mapa de bits.

## SwapChainPanel y juegos


[
              **SwapChainPanel**
            ](https://msdn.microsoft.com/library/windows/apps/dn252834) es el tipo de Windows Runtime diseñado para elementos gráficos y juegos de alto rendimiento, en los que puedes administrar la cadena de intercambio directamente. En este caso, puedes crear tu propia cadena de intercambio DirectX y administrar la presentación del contenido representado. A continuación, puedes agregar al objeto **SwapChainPanel** elementos XAML tales como menús, presentaciones de avisos y otras superposiciones de interfaz de usuario.

Para garantizar un buen rendimiento, el tipo [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) tiene algunas limitaciones:

-   No hay más de 4 instancias de [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) por aplicación.
-   Las propiedades **Opacity**, **RenderTransform**, **Projection** y **Clip** heredadas por [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) no son compatibles.
-   Debes establecer los valores de alto y ancho de la cadena de intercambio de DirectX (en [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) según las dimensiones de la ventana de la aplicación. De lo contrario, se escalará el contenido de la pantalla (mediante **DXGI\_SCALING\_STRETCH**) para ajustarlo.
-   Debes establecer el modo de escalado de la cadena de intercambio de DirectX (en [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) a **DXGI\_SCALING\_STRETCH**.
-   No puedes establecer el modo alfa de la cadena de intercambio de DirectX (en [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) a **DXGI\_ALPHA\_MODE\_PREMULTIPLIED**.
-   Debes crear la cadena de intercambio de DirectX llamando a [**IDXGIFactory2::CreateSwapChainForComposition**](https://msdn.microsoft.com/library/windows/desktop/hh404558).

Debes actualizar el elemento [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) en función de las necesidades de la aplicación y no de las actualizaciones del marco XAML. Si necesitas sincronizar las actualizaciones de **SwapChainPanel** con las del marco XAML, regístrate para obtener el evento [**Windows::UI::Xaml::Media::CompositionTarget::Rendering**](https://msdn.microsoft.com/library/windows/apps/br228127). De lo contrario, deberás tener en cuenta los posibles problemas entre subprocesos al intentar actualizar los elementos XAML de un subproceso distinto al que actualiza el objeto **SwapChainPanel**.

Al diseñar tu aplicación, debes seguir algunas prácticas recomendadas generales para que esta use [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834).

-   [
              **SwapChainPanel**
            ](https://msdn.microsoft.com/library/windows/apps/dn252834) hereda de [**Windows::UI::Xaml::Controls::Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) y es compatible con comportamientos de diseño similares. Debes familiarizarte con el tipo **Grid** y sus propiedades.

-   Después de establecer una cadena de intercambio de DirectX, todos los eventos de entrada que se desencadenan para [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) funcionan de la misma manera que para cualquier otro elemento XAML. No tienes que establecer un pincel de fondo para **SwapChainPanel** ni controlar directamente eventos de entrada del objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) de la aplicación, al contrario que sucede con las aplicaciones de DirectX que no usan **SwapChainPanel**.

-   • Todo el contenido del árbol visual de elementos XAML que se encuentra bajo un elemento secundario de un objeto [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) se recorta para que tenga el tamaño de diseño del elemento secundario inmediato del objeto **SwapChainPanel**. Cualquier contenido que se transforme fuera de estos límites de diseño no se representará. Debido ello, debes colocar cualquier contenido XAML que quieras animar con el elemento XAML [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490) en el árbol visual, con un elemento cuyos límites de diseño sean lo suficientemente grandes como para contener el intervalo completo de la animación.

-   Limita el número de elementos visuales XAML inmediatos que se encuentren en un elemento [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834). Si es posible, agrupa los elementos cercanos en un elemento primario común. Sin embargo, ten en cuenta que hay una relación entre el número de elementos secundarios visuales inmediatos y el tamaño de los elementos secundarios: si hay demasiados elementos XAML o son demasiado grandes, esto puede afectar al rendimiento global. Tampoco debes crear un solo elemento secundario XAML de pantalla completa para el elemento **SwapChainPanel** de la aplicación, ya que esto aumenta la actividad de dibujo innecesaria y afecta al rendimiento. Como regla general, no crees más de 8 elementos secundarios visuales XAML inmediatos para el elemento **SwapChainPanel** de la aplicación. Asimismo, cada elemento debe tener el tamaño de diseño necesario para poder abarcar el contenido visual del elemento. De todos modos, puedes crear un árbol visual de elementos que se encuentre en un elemento secundario de **SwapChainPanel** y que sea lo suficientemente complejo, sin afectar demasiado al rendimiento.

> **Nota**   En general, las aplicaciones DirectX deben crear cadenas de intercambio con orientación horizontal y que tengan el mismo tamaño que la ventana de presentación (este tamaño suele ser la resolución de la pantalla nativa en la mayoría de juegos de la Tienda Windows). De esta forma, se garantiza que la aplicación implemente de manera óptima la cadena de intercambio cuando no tenga ninguna superposición de XAML visible. Si la aplicación se gira y se coloca en modo vertical, esta debería llamar a [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801) en la cadena de intercambio existente, aplicar una transformación al contenido si fuera necesario y luego llamar de nuevo a [**SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144) en la misma cadena de intercambio. De forma similar, la aplicación debe llamar de nuevo a **SetSwapChain** en la misma cadena de intercambio, siempre que se cambie el tamaño de la cadena de intercambio mediante una llamada a [**IDXGISwapChain::ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577).

 

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

    Los elementos XAML se actualizan cuando el diseño o la lógica de representación de Windows Runtime envía una señal de actualización.

## Temas relacionados


* [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)
* [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)
* [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)
* [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143)
* [Programming Guide for Direct3D 11 (Guía de programación para Direct3D 11)](https://msdn.microsoft.com/library/windows/desktop/ff476345)

 

 






<!--HONumber=May16_HO2-->


