---
title: Interoperabilidad de DirectX y XAML
description: Puedes usar el lenguaje XAML y Microsoft DirectX juntos en tu juego para la Plataforma universal de Windows (UWP).
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, DirectX, interoperabilidad XAML
ms.localizationpriority: medium
ms.openlocfilehash: a34510939b84c885bf90ac2b6b42ffa158decc8e
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216578"
---
# <a name="directx-and-xaml-interop"></a>Interoperabilidad de DirectX y XAML

Puedes usar el lenguaje XAML y Microsoft DirectX juntos en tu juego para la Plataforma universal de Windows (UWP). La combinación de XAML y DirectX te permite crear marcos de interfaz de usuario flexibles que interoperan con el contenido representado en DirectX; además, es especialmente útil para aplicaciones que hacen un uso intensivo de los elementos gráficos. En este tema explicamos la estructura de una aplicación para UWP que usa DirectX y enumeramos los tipos importantes que se deben usar al generar una aplicación para UWP de modo que funcione con DirectX.

Si la aplicación se centra principalmente en la representación 2D, es aconsejable usar la biblioteca [Win2D](https://github.com/microsoft/win2d) de Windows Runtime. Esta biblioteca la mantiene Microsoft y se basa en las tecnologías básicas de Direct2D . Simplifica en gran medida el modelo de uso para implementar gráficos 2D e incluye abstracciones útiles para algunas de las técnicas descritas en este documento. Consulta la página del proyecto para obtener más detalles. Este documento ofrece orientación para aquellos desarrolladores de aplicaciones que decidan *no* utilizar Win2D.

> [!NOTE]
> Las API de DirectX no se definen como tipos Windows Runtime, pero normalmente puede usar [C++/WinRT](../cpp-and-winrt-apis/index.md) para desarrollar componentes de UWP para XAML que interoperen con DirectX. También puedes crear una aplicación para UWP con C# y XAML que use DirectX. Para ello, debes encapsular las llamadas a DirectX en un archivo de metadatos de Windows Runtime independiente.

## <a name="xaml-and-directx"></a>XAML y DirectX

DirectX proporciona dos bibliotecas muy eficaces para gráficos 2D y 3D: Direct2D y Microsoft Direct3D. Aunque XAML proporciona compatibilidad con primitivas y efectos 2D, muchas aplicaciones (como las de modelado o los juegos) necesitan funcionalidades gráficas más complejas. En estas aplicaciones, puedes usar Direct2D y Direct3D para representar una parte, o la totalidad, de los elementos gráficos y emplear XAML para todo lo demás.

Si vas a implementar interoperabilidad entre XAML y DirectX personalizada, debes conocer estos dos conceptos:

-   Las superficies compartidas son regiones de la pantalla con un tamaño establecido y definidas por XAML, en las que puedes usar DirectX para dibujar indirectamente, mediante los tipos [Windows::UI::Xaml::Media::ImageSource](/uwp/api/windows.ui.xaml.media.imagesource). En las superficies compartidas no controlas el momento preciso en el que el nuevo contenido aparece en pantalla. En lugar de eso, las actualizaciones de una superficie compartida se sincronizan con las actualizaciones del marco XAML.
-   Las [cadenas de intercambio ](/windows/desktop/direct3d9/what-is-a-swap-chain-) representan un conjunto de búferes usados para mostrar gráficos con una latencia mínima. Por lo general, las cadenas de intercambio se actualizan a 60 fotogramas por segundo, independientes del subproceso de la interfaz de usuario. Sin embargo, las cadenas de intercambio usan más recursos de memoria y CPU para permitir actualizaciones rápidas y son más difíciles de usar, ya que se deben administrar varios subprocesos.

Piensa para qué vas a usar DirectX. ¿Lo usarás para componer o animar un control único que se ajuste a las dimensiones de la ventana de presentación? ¿Contiene salida que tenga que representarse y controlarse en tiempo real, como sucede en un juego? Si es así, probablemente tendrás que implementar una cadena de intercambio. De lo contrario, debería bastar con usar una superficie compartida.

Una vez que haya determinado cómo desea usar DirectX, use uno de estos tipos de Windows Runtime para incorporar la representación de DirectX a la aplicación de UWP:

-   Si quieres crear una imagen estática o dibujar una imagen compleja en intervalos controlados por eventos, dibuja en una superficie compartida con [Windows::UI::Xaml::Media::Imaging::SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource). Este tipo controla una superficie de dibujo de DirectX de un tamaño definido. Normalmente se usa este tipo al crear una imagen o textura, como un mapa de bits, que luego se mostrará en un documento o elemento de interfaz de usuario. Recuerda que no funciona bien con la interactividad en tiempo real como, por ejemplo, un juego de alto rendimiento. Esto se debe a que las actualizaciones de un objeto **SurfaceImageSource** se sincronizan con las actualizaciones de la interfaz de usuario XAML; por ello, se puede producir una latencia en la información visual que se proporciona al usuario como, por ejemplo, una velocidad de fotogramas fluctuante o una respuesta deficiente para las entradas en tiempo real. De todos modos, las actualizaciones son lo suficientemente rápidas como para realizar controles dinámicos o simulaciones de datos.

-   Si la imagen es más grande que el estado real de la pantalla proporcionada y el usuario puede realizar una panorámica o acercar o alejar la imagen, usa [Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource). Este tipo controla una superficie de dibujo de DirectX de un tamaño definido que es mayor que el de la pantalla. Tal como sucede con [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource), usarás este tipo dinámicamente al componer una imagen o control complejos. Asimismo, al igual que **SurfaceImageSource**, este tipo no funciona bien en juegos de alto rendimiento. Algunos ejemplos de elementos XAML que pueden usar un tipo **VirtualSurfaceImageSource**, son los controles de mapa o un visor de documentos con imágenes grandes o de alta densidad.

-   Si usas DirectX para presentar gráficos actualizados en tiempo real o en una situación en la que las actualizaciones deben llegar en intervalos regulares de baja latencia, usa la clase [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) para poder actualizar los elementos gráficos sin tener que sincronizar el temporizador de actualizaciones del marco XAML. Este tipo te permite acceder a la cadena de intercambio del dispositivo gráfico ([IDXGISwapChain1](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)) directamente y disponer XAML como una capa sobre el destino de la representación. Funciona muy bien en juegos y aplicaciones DirectX de pantalla completa que requieren una interfaz de usuario basada en XAML. Para usar este método debes ser bastante ducho en DirectX, incluidas las tecnologías Infraestructura de gráficos de Microsoft DirectX (DXGI), Direct2D y Direct3D. Para obtener más información, vea [Guía de programación para Direct3D 11](/windows/desktop/direct3d11/dx-graphics-overviews).

## <a name="surfaceimagesource"></a>SurfaceImageSource

[SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) proporciona superficies compartidas de DirectX para dibujar en ellas y luego componer los fragmentos para crear el contenido de la aplicación.

Este es el proceso básico para crear y actualizar un objeto [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) en el código subyacente:

1.  Definir el tamaño de la superficie compartida pasando los valores de alto y ancho al constructor [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource). También puedes indicar si la superficie necesita compatibilidad alfa (opacidad).

    Por ejemplo:

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  Obtiene un puntero a [ISurfaceImageSourceNativeWithD2D](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d). Convierta el objeto [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) en [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (o **IUnknown**) y llame a **QueryInterface** en él para obtener la implementación de **ISurfaceImageSourceNativeWithD2D** subyacente. Puedes usar los métodos definidos en esta implementación para establecer el dispositivo y ejecutar las operaciones de dibujo.

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* sisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);

    sisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **)&m_sisNativeWithD2D);
    ```

3.  Cree los dispositivos DXGI y D2D llamando primero a [D3D11CreateDevice](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) y [D2D1CreateDevice](/windows/desktop/api/d2d1_1/nf-d2d1_1-d2d1createdevice) y, a continuación, pase el dispositivo y el contexto a [ISurfaceImageSourceNativeWithD2D:: SetDevice](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-setdevice). 

    > [!NOTE]
    > Si va a dibujar en el **SurfaceImageSource** desde un subproceso en segundo plano, también debe asegurarse de que el dispositivo DXGI tenga habilitado el acceso multiproceso. Esto solo debe hacerse si va a dibujar desde un subproceso en segundo plano, por motivos de rendimiento.

    Por ejemplo:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
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
            &m_d3dContext);

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_sisNativeWithD2D->SetDevice(m_d2dDevice.Get());
    ```

4.  Proporcione un puntero al objeto [ID2D1DeviceContext](/windows/desktop/api/dxgi/nn-dxgi-idxgisurface) a [ISurfaceImageSourceNativeWithD2D:: BeginDraw](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-begindraw)y use el contexto de dibujo devuelto para dibujar el contenido del rectángulo deseado en el **SurfaceImageSource**. Se puede llamar a **ISurfaceImageSourceNativeWithD2D:: BeginDraw** y a los comandos de dibujo desde un subproceso en segundo plano. Solo se dibujará el área especificada para su actualización en el parámetro *updateRect*.

    Este método devuelve el desplazamiento del punto (x,y) del rectángulo de destino actualizado en el parámetro *offset*. Este desplazamiento se usa para determinar dónde se debe dibujar el contenido actualizado con el **ID2D1DeviceContext**.

    ```cpp
    Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

    HRESULT beginDrawHR = m_sisNative->BeginDraw(
        updateRect, 
        &drawingContext, 
        &offset);

    if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
        || beginDrawHR == DXGI_ERROR_DEVICE_RESET
        || beginDrawHR == D2DERR_RECREATE_TARGET)
    {
        // The D3D and D2D device was lost and need to be re-created.
        // Recovery steps are:
        // 1) Re-create the D3D and D2D devices
        // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the new D2D
        //    device
        // 3) Redraw the contents of the SurfaceImageSource
    }
    else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
    {
        // The devices were not lost but the entire contents of the surface
        // were. Recovery steps are:
        // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the D2D 
        //    device again
        // 2) Redraw the entire contents of the SurfaceImageSource
    }
    else 
    {
        // Draw your updated rectangle with the drawingContext
    }
    ```

5. Llame a [ISurfaceImageSourceNativeWithD2D:: EndDraw](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-enddraw) para completar el mapa de bits. El mapa de bits puede usarse como origen de un elemento [Image](/uwp/api/windows.ui.xaml.controls.image) o [ImageBrush](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) de XAML. **ISurfaceImageSourceNativeWithD2D:: EndDraw** solo debe llamarse desde el subproceso de la interfaz de usuario.

    ```cpp
    m_sisNative->EndDraw();

    // ...
    // The SurfaceImageSource object's underlying 
    // ISurfaceImageSourceNativeWithD2D object contains the completed bitmap.

    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

    > [!NOTE]
    > La llamada a [SurfaceImageSource:: SetSource](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource) (heredada de **IBitmapSource:: SetSource**) actualmente produce una excepción. No lo llames desde el objeto [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource).

    > [!NOTE]
    > Las aplicaciones deben evitar dibujar en **SurfaceImageSource** mientras su [ventana](/uwp/api/Windows.UI.Xaml.Window) asociada está oculta; de lo contrario, se producirá un error en las API de **ISurfaceImageSourceNativeWithD2D** . Para ello, Regístrese como un agente de escucha de eventos para el evento [window. VisibilityChanged](/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) para realizar el seguimiento de los cambios de visibilidad.

## <a name="virtualsurfaceimagesource"></a>VirtualSurfaceImageSource

[VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) extiende el elemento [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) cuando el contenido es potencialmente más grande que lo que se puede mostrar en pantalla, por lo que hay que virtualizar el contenido para representarlo de forma óptima.

[VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) difiere de [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) en que este último usa la devolución de llamada [IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative-updatesneeded) que se implementó para actualizar regiones de la superficie a medida que se visualizan en la pantalla. No tienes que borrar regiones que estén ocultas; el marco XAML se encarga de ello.

Este es el proceso básico para crear y actualizar un objeto [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) en el código subyacente:

1.  Cree una instancia de [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) con el tamaño que desee. Por ejemplo:

    ```cpp
    VirtualSurfaceImageSource^ virtualSIS = 
        ref new VirtualSurfaceImageSource(2000, 2000);
    ```

2.  Obtiene punteros a [IVirtualSurfaceImageSourceNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative) y [ISurfaceImageSourceNativeWithD2D](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d). Convierta el objeto [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) como [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) o [IUnknown](/windows/desktop/api/unknwn/nn-unknwn-iunknown), y llame a [QueryInterface](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) en él para obtener las implementaciones subyacentes **IVirtualSurfaceImageSourceNative** y **ISurfaceImageSourceNativeWithD2D** . Los métodos definidos en estas implementaciones se usan para establecer el dispositivo y ejecutar las operaciones de dibujo.

    ```cpp
    Microsoft::WRL::ComPtr<IVirtualSurfaceImageSourceNative>  m_vsisNative;
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* vsisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(virtualSIS);

    vsisInspectable->QueryInterface(
        __uuidof(IVirtualSurfaceImageSourceNative), 
        (void **) &m_vsisNative);

    vsisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **) &m_sisNativeWithD2D);
    ```

3.  Cree los dispositivos DXGI y D2D llamando primero a **D3D11CreateDevice** y **D2D1CreateDevice**y, a continuación, pase el dispositivo D2D a **ISurfaceImageSourceNativeWithD2D:: SetDevice**.

    > [!NOTE]
    > Si va a dibujar en el **VirtualSurfaceImageSource** desde un subproceso en segundo plano, también debe asegurarse de que el dispositivo DXGI tenga habilitado el acceso multiproceso. Esto solo debe hacerse si va a dibujar desde un subproceso en segundo plano, por motivos de rendimiento.

    Por ejemplo:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
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
            &m_d3dContext);  

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    
    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_vsisNativeWithD2D->SetDevice(m_d2dDevice.Get());

    m_vsisNative->SetDevice(dxgiDevice.Get());
    ```

4.  Llama a [IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-registerforupdatesneeded) y pasa una referencia a la implementación de [IVirtualSurfaceUpdatesCallbackNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative).

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

    El marco llama a la implementación de [IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-registerforupdatesneeded) cuando hay que actualizar un región de la clase [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource).

    Esto puede suceder cuando el marco determina que es necesario dibujar la región (como cuando el usuario muestra una vista panorámica o acerca o aleja la vista de la superficie), o después de que la aplicación llame a [IVirtualSurfaceImageSourceNative::Invalidate](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-invalidate) en esa región.

5.  En [IVirtualSurfaceImageSourceNative::UpdatesNeeded](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative-updatesneeded), usa los métodos [IVirtualSurfaceImageSourceNative::GetUpdateRectCount](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-getupdaterectcount) y [IVirtualSurfaceImageSourceNative::GetUpdateRects](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-getupdaterects) para determinar qué regiones de la superficie deben dibujarse.

    ```cpp
    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
        HRESULT hr = S_OK;

        try
        {
            ULONG drawingBoundsCount = 0;  
            m_vsisNative->GetUpdateRectCount(&drawingBoundsCount);

            std::unique_ptr<RECT[]> drawingBounds(
                new RECT[drawingBoundsCount]);

            m_vsisNative->GetUpdateRects(
                drawingBounds.get(), 
                drawingBoundsCount);
            
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

    1.  Proporcione un puntero al objeto **ID2D1DeviceContext** a **ISurfaceImageSourceNativeWithD2D:: BeginDraw**y use el contexto de dibujo devuelto para dibujar el contenido del rectángulo deseado en el **SurfaceImageSource**. Se puede llamar a **ISurfaceImageSourceNativeWithD2D:: BeginDraw** y a los comandos de dibujo desde un subproceso en segundo plano. Solo se dibujará el área especificada para su actualización en el parámetro *updateRect*.

        Este método devuelve el desplazamiento del punto (x,y) del rectángulo de destino actualizado en el parámetro *offset*. Este desplazamiento se usa para determinar dónde se debe dibujar el contenido actualizado con el **ID2D1DeviceContext**.

        ```cpp
        Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

        HRESULT beginDrawHR = m_sisNative->BeginDraw(
            updateRect, 
            &drawingContext, 
            &offset);

        if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
            || beginDrawHR == DXGI_ERROR_DEVICE_RESET 
            || beginDrawHR == D2DERR_RECREATE_TARGET)
        {
            // The D3D and D2D devices were lost and need to be re-created.
            // Recovery steps are:
            // 1) Re-create the D3D and D2D devices
            // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    new D2D device
            // 3) Redraw the contents of the VirtualSurfaceImageSource
        }
        else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
        {
            // The devices were not lost but the entire contents of the 
            // surface were lost. Recovery steps are:
            // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    D2D device again
            // 2) Redraw the entire contents of the VirtualSurfaceImageSource
        }
        else
        {
            // Draw your updated rectangle with the drawingContext
        }
        ```

    2.  Dibuja el contenido específico de la región, pero restringe el dibujo a las regiones enlazadas, lo que mejora el rendimiento.

    3.  Llame a **ISurfaceImageSourceNativeWithD2D:: EndDraw**. El resultado es un mapa de bits.

> [!NOTE]
> Las aplicaciones deben evitar dibujar en **SurfaceImageSource** mientras su [ventana](/uwp/api/Windows.UI.Xaml.Window) asociada está oculta; de lo contrario, se producirá un error en las API de **ISurfaceImageSourceNativeWithD2D** . Para ello, Regístrese como un agente de escucha de eventos para el evento [window. VisibilityChanged](/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) para realizar el seguimiento de los cambios de visibilidad.

## <a name="swapchainpanel-and-gaming"></a>SwapChainPanel y juegos


[SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) es el tipo de Windows Runtime diseñado para elementos gráficos y juegos de alto rendimiento, en los que puedes administrar la cadena de intercambio directamente. En este caso, puedes crear tu propia cadena de intercambio de DirectX y administrar la presentación del contenido representado.

Para garantizar un buen rendimiento, el tipo [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) tiene algunas limitaciones:

-   No hay más de 4 instancias de [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) por aplicación.
-   Debe establecer el alto y el ancho de la cadena de intercambio de DirectX (en la [cadena de intercambio de DXGI \_ \_ \_ DESC1](/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1)) en las dimensiones actuales del elemento de la cadena de intercambio. Si no lo hace, se escalará el contenido de la pantalla (mediante el ajuste de **escala de DXGI \_ \_ **) para ajustarse.
-   Debe establecer el modo de escalado de la cadena de intercambio de DirectX (en la [cadena de intercambio de dxgi \_ \_ \_ DESC1](/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1)) en el ajuste de **escala de dxgi \_ \_ **.
-   Debes crear la cadena de intercambio de DirectX llamando a [IDXGIFactory2::CreateSwapChainForComposition](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcomposition).

Debes actualizar el elemento [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) en función de las necesidades de la aplicación y no de las actualizaciones del marco XAML. Si necesitas sincronizar las actualizaciones de **SwapChainPanel** con las del marco XAML, regístrate para obtener el evento [Windows::UI::Xaml::Media::CompositionTarget::Rendering](/uwp/api/windows.ui.xaml.media.compositiontarget.rendering). En caso contrario, deberás tener en cuenta los posibles problemas entre subprocesos al intentar actualizar los elementos XAML de un subproceso distinto al que actualiza el objeto **SwapChainPanel**.

Si tienes que recibir una entrada de puntero de latencia baja en tu **SwapChainPanel**, usa [SwapChainPanel::CreateCoreIndependentInputSource](/uwp/api/windows.ui.xaml.controls.swapchainpanel.createcoreindependentinputsource). Este método devuelve un objeto [CoreIndependentInputSource](/uwp/api/windows.ui.core.coreindependentinputsource) que puede usarse para recibir eventos de entrada con una latencia mínima en un subproceso en segundo plano. Ten en cuenta que, cuando se llama a este método, no se desencadenará eventos normales de entrada de puntero XAML para **SwapChainPanel**, dado que todas las entradas se redirigirán al subproceso en segundo plano.


> **Nota:**   En general, las aplicaciones de DirectX deben crear cadenas de intercambio en orientación horizontal y igual al tamaño de la ventana de presentación (que suele ser la resolución de pantalla nativa en la mayoría de los juegos Microsoft Store). De esta forma, se garantiza que la aplicación implemente de manera óptima la cadena de intercambio cuando no tenga ninguna superposición de XAML visible. Si la aplicación se gira y se coloca en modo vertical, esta debería llamar a [IDXGISwapChain1::SetRotation](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation) en la cadena de intercambio existente, aplicar una transformación al contenido si fuera necesario y luego llamar de nuevo a [SetSwapChain](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain) en la misma cadena de intercambio. Del mismo modo, la aplicación debe llamar a **SetSwapChain** de nuevo en la misma cadena de intercambio siempre que se cambie el tamaño de la cadena de intercambio llamando a [IDXGISwapChain:: ResizeBuffers](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers).


 

Este es el proceso básico para crear y actualizar un objeto [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) en el código subyacente:

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

2.  Obtén un puntero para [ISwapChainPanelNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative). Convierte el objeto [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) en [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (o **IUnknown**), y llama a **QueryInterface** para obtener la implementación subyacente **ISwapChainPanelNative**.

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  Cree el dispositivo DXGI y la cadena de intercambio, y establezca la cadena de intercambio en [ISwapChainPanelNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative) pasándolo a [SetSwapChain](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain).

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

* [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)
* [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)
* [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource)
* [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)
* [ISwapChainPanelNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative)
* [Programming Guide for Direct3D 11 (Guía de programación para Direct3D 11)](/windows/desktop/direct3d11/dx-graphics-overviews)