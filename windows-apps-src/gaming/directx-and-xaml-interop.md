---
author: mtoepke
title: Interoperabilidad de DirectX y XAML
description: Puedes usar el lenguaje XAML y Microsoft DirectX juntos en tu juego para la Plataforma universal de Windows (UWP).
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, juegos, games, directx, interoperabilidad de xaml, xaml interop
ms.localizationpriority: medium
ms.openlocfilehash: 7f3a70be3dd31b0a5e4214621ab9fb4efa72cc54
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "6249699"
---
# <a name="directx-and-xaml-interop"></a>Interoperabilidad de DirectX y XAML



Puedes usar el lenguaje XAML y Microsoft DirectX juntos en tu juego para la Plataforma universal de Windows (UWP). La combinación de XAML y DirectX te permite crear marcos de interfaz de usuario flexibles que interoperan con el contenido representado en DirectX; además, es especialmente útil para aplicaciones que hacen un uso intensivo de los elementos gráficos. En este tema explicamos la estructura de una aplicación para UWP que usa DirectX y enumeramos los tipos importantes que se deben usar al generar una aplicación para UWP de modo que funcione con DirectX.

Si la aplicación se centra principalmente en la representación 2D, es aconsejable usar la biblioteca [Win2D](https://github.com/microsoft/win2d) de Windows Runtime. Esta biblioteca la mantiene Microsoft y se basa en las tecnologías básicas de Direct2D . Simplifica en gran medida el modelo de uso para implementar gráficos 2D e incluye abstracciones útiles para algunas de las técnicas descritas en este documento. Consulta la página del proyecto para obtener más detalles. Este documento ofrece orientación para aquellos desarrolladores de aplicaciones que decidan *no* utilizar Win2D.

> **Nota**APIs DirectX no están definidas como tipos de Windows Runtime, por lo que suele usar extensiones de componentes VisualC ++ (C++ / CX) para desarrollar componentes XAML UWP que interoperen con DirectX. También puedes crear una aplicación para UWP con C# y XAML que use DirectX. Para ello, debes encapsular las llamadas a DirectX en un archivo de metadatos de Windows Runtime independiente.

 

## <a name="xaml-and-directx"></a>XAML y DirectX

DirectX proporciona dos bibliotecas muy eficaces para gráficos 2D y 3D: Direct2D y MicrosoftDirect3D. Aunque XAML proporciona compatibilidad con primitivas y efectos 2D, muchas aplicaciones (como las de modelado o los juegos) necesitan funcionalidades gráficas más complejas. En estas aplicaciones, puedes usar Direct2D y Direct3D para representar una parte, o la totalidad, de los elementos gráficos y emplear XAML para todo lo demás.

Si vas a implementar interoperabilidad entre XAML y DirectX personalizada, debes conocer estos dos conceptos:

-   Las superficies compartidas son regiones de la pantalla con un tamaño establecido y definidas por XAML, en las que puedes usar DirectX para dibujar indirectamente, con los tipos [Windows::UI::Xaml::Media::ImageSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imagesource.aspx). En las superficies compartidas no controlas el momento preciso en el que el nuevo contenido aparece en pantalla. En lugar de eso, las actualizaciones de una superficie compartida se sincronizan con las actualizaciones del marco XAML.
-   Las [cadenas de intercambio ](https://msdn.microsoft.com/library/windows/desktop/bb206356(v=vs.85).aspx) representan un conjunto de búferes usados para mostrar gráficos con una latencia mínima. Por lo general, las cadenas de intercambio se actualizan a 60 fotogramas por segundo, independientes del subproceso de la interfaz de usuario. Sin embargo, las cadenas de intercambio usan más recursos de memoria y CPU para permitir actualizaciones rápidas y son más difíciles de usar, ya que se deben administrar varios subprocesos.

Piensa para qué vas a usar DirectX. ¿Lo usarás para componer o animar un control único que se ajuste a las dimensiones de la ventana de presentación? ¿Contiene salida que tenga que representarse y controlarse en tiempo real, como sucede en un juego? Si es así, probablemente tendrás que implementar una cadena de intercambio. De lo contrario, debería bastar con usar una superficie compartida.

Una vez que hayas determinado cómo quieres usar DirectX, usarás uno de estos tipos de Windows Runtime para incorporar la representación de DirectX en tu aplicación para UWP:

-   Si quieres componer una imagen estática o dibujar una imagen compleja en intervalos controlados por eventos, dibuja en una superficie compartida con [Windows::UI::Xaml::Media::Imaging::SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041). Este tipo controla una superficie de dibujo de DirectX de un tamaño definido. Normalmente se usa este tipo al crear una imagen o textura, como un mapa de bits, que luego se mostrará en un documento o elemento de interfaz de usuario. Recuerda que no funciona bien con la interactividad en tiempo real como, por ejemplo, un juego de alto rendimiento. Esto se debe a que las actualizaciones de un objeto **SurfaceImageSource** se sincronizan con las actualizaciones de la interfaz de usuario XAML; por ello, se puede producir una latencia en la información visual que se proporciona al usuario como, por ejemplo, una velocidad de fotogramas fluctuante o una respuesta deficiente para las entradas en tiempo real. De todos modos, las actualizaciones son lo suficientemente rápidas como para realizar controles dinámicos o simulaciones de datos.

-   Si la imagen es más grande que el estado real de la pantalla proporcionada y el usuario puede realizar una panorámica o acercar o alejar la imagen, usa [Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050). Este tipo controla una superficie de dibujo de DirectX de un tamaño definido que es mayor que el de la pantalla. Tal como sucede con [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041), se usa este tipo al componer dinámicamente una imagen o control complejos. Asimismo, al igual que **SurfaceImageSource**, este tipo no funciona bien en juegos de alto rendimiento. Algunos ejemplos de elementos XAML que pueden usar un tipo **VirtualSurfaceImageSource**, son los controles de mapa o un visor de documentos con imágenes grandes o de alta densidad.

-   Si usas DirectX para presentar gráficos actualizados en tiempo real o en una situación en la que las actualizaciones deben llegar en intervalos regulares de baja latencia, usa la clase [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) para poder actualizar los elementos gráficos sin tener que sincronizar el temporizador de actualizaciones del marco XAML. Este tipo te permite obtener acceso a la cadena de intercambio del dispositivo gráfico ([IDXGISwapChain1](https://msdn.microsoft.com/library/windows/desktop/hh404631)) directamente y disponer XAML como una capa sobre el destino de la representación. Funciona muy bien en juegos y aplicaciones DirectX de pantalla completa que requieren una interfaz de usuario basada en XAML. Para usar este método debes ser bastante ducho en DirectX, incluidas las tecnologías Infraestructura de gráficos de MicrosoftDirectX (DXGI), Direct2D y Direct3D. Para obtener más información, consulta [Programming Guide for Direct3D 11 (Guía de programación para Direct3 11)](https://msdn.microsoft.com/library/windows/desktop/ff476345).

## <a name="surfaceimagesource"></a>SurfaceImageSource


[SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) proporciona superficies compartidas de DirectX para dibujar en ellas y luego compone los bits en contenido de aplicación.

El procedimiento básico para crear y actualizar un objeto [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) en el código subyacente es:

1.  Definir el tamaño de la superficie compartida pasando los valores de alto y ancho al constructor [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041). También puedes indicar si la superficie necesita compatibilidad alfa (opacidad).

    Por ejemplo:

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  Obtén un puntero para [ISurfaceImageSourceNativeWithD2D](https://msdn.microsoft.com/library/windows/desktop/dn302137). Convierte el objeto [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) en [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821) (o **IUnknown**), y llama a **QueryInterface** para obtener la implementación subyacente de **ISurfaceImageSourceNativeWithD2D**. Puedes usar los métodos definidos en esta implementación para establecer el dispositivo y ejecutar las operaciones de dibujo.

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* sisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);

    sisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **)&m_sisNativeWithD2D);
    ```

3.  Crea los dispositivos DXGI y D2D llamando primero a [D3D11CreateDevice](https://msdn.microsoft.com/library/windows/desktop/ff476082) y [D2D1CreateDevice](https://msdn.microsoft.com/library/windows/desktop/hh404272(v=vs.85).aspx) y, a continuación, pasa el dispositivo y el contexto a [ISurfaceImageSourceNativeWithD2D::SetDevice](https://msdn.microsoft.com/library/dn302141.aspx). 

    > [!NOTE]
    > Si vas a dibujar en tu **SurfaceImageSource** desde un subproceso en segundo plano, también tendrás que asegurarte de que el dispositivo DXGI ha habilitado el acceso multiproceso. Solo debe hacerse si vas a dibujar desde un subproceso en segundo plano, por motivos de rendimiento.

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

4.  Proporciona un puntero para el objeto [ID2D1DeviceContext](https://msdn.microsoft.com/library/windows/desktop/bb174565) a [ISurfaceImageSourceNativeWithD2D::BeginDraw](https://msdn.microsoft.com/library/dn302138.aspx), y usa el contexto de dibujo devuelto para dibujar el contenido del rectángulo deseado dentro de **SurfaceImageSource**. **ISurfaceImageSourceNativeWithD2D::BeginDraw** y los comandos de dibujo se pueden llamar desde un subproceso en segundo plano. Solo se dibujará el área especificada para actualización en el parámetro *updateRect*.

    Este método devuelve el desplazamiento del punto (x,y) del rectángulo de destino actualizado en el parámetro *offset*. Puedes usar este desplazamiento para determinar dónde se debe dibujar el contenido actualizado con **ID2D1DeviceContext**.

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

5. Llama a [ISurfaceImageSourceNativeWithD2D::EndDraw](https://msdn.microsoft.com/library/dn302139.aspx) para completar el mapa de bits. El mapa de bits puede usarse como origen de un elemento [Image](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.image.aspx) o [ImageBrush](https://msdn.microsoft.com/library/windows/apps/br210101) de XAML. **ISurfaceImageSourceNativeWithD2D::EndDraw** debe llamarse solo desde el subproceso de la interfaz de usuario.

    ```cpp
    m_sisNative->EndDraw();

    // ...
    // The SurfaceImageSource object's underlying 
    // ISurfaceImageSourceNativeWithD2D object contains the completed bitmap.

    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

    > [!NOTE]
    > Actualmente, si llamas a [SurfaceImageSource::SetSource](https://msdn.microsoft.com/library/windows/apps/br243255) (heredado de **IBitmapSource::SetSource**), se inicia una excepción. No lo llames desde el objeto [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041).

    > [!NOTE]
    > Las aplicaciones deben evitar dibujar en **SurfaceImageSource** mientras su elemento [Window](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) asociado está oculto, de lo contrario se producirá un error en las API de **ISurfaceImageSourceNativeWithD2D**. Para ello, regístrate como escucha de eventos para el evento [Window.VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) para realizar un seguimiento de los cambios de visibilidad.

## <a name="virtualsurfaceimagesource"></a>VirtualSurfaceImageSource

[VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) extiende el elemento [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) cuando el contenido es potencialmente superior al espacio en pantalla, por lo que hay que virtualizar el contenido para representarlo de forma óptima.

[VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) difiere de [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) en que usa una devolución de llamada, [IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848337), que se implementa para actualizar regiones de la superficie a medida que se visualizan en la pantalla. No tienes que borrar las regiones que están ocultas; el marco XAML se encarga de ello.

A continuación, encontrarás el procedimiento básico para crear y actualizar un objeto [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) en el código subyacente:

1.  Crea una instancia de [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) con el tamaño que quieras. Por ejemplo:

    ```cpp
    VirtualSurfaceImageSource^ virtualSIS = 
        ref new VirtualSurfaceImageSource(2000, 2000);
    ```

2.  Obtén punteros para [IVirtualSurfaceImageSourceNative](https://msdn.microsoft.com/library/windows/desktop/hh848328) y [ISurfaceImageSourceNativeWithD2D](https://msdn.microsoft.com/library/windows/desktop/dn302137). Convierte el objeto [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) en [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821) o [IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509), y llama a [QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521) para obtener las implementaciones subyacentes de **IVirtualSurfaceImageSourceNative** y **ISurfaceImageSourceNativeWithD2D**. Puedes usar los métodos definidos en estas implementaciones para establecer el dispositivo y ejecutar las operaciones de dibujo.

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

3.  Crea los dispositivos DXGI y D2D llamando primero a **D3D11CreateDevice** y **D2D1CreateDevice**y, a continuación, pasa el dispositivo D2D a **ISurfaceImageSourceNativeWithD2D::SetDevice**.

    > [!NOTE]
    > Si vas a dibujar en tu **VirtualSurfaceImageSource** desde un subproceso en segundo plano, también tendrás que asegurarte de que el dispositivo DXGI ha habilitado el acceso multiproceso. Solo debe hacerse si vas a dibujar desde un subproceso en segundo plano, por motivos de rendimiento.

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

4.  Llama a [IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848334) y pasa una referencia a la implementación de [IVirtualSurfaceUpdatesCallbackNative](https://msdn.microsoft.com/library/windows/desktop/hh848336).

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

    El marco llama a la implementación de [IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848334) cuando hay que actualizar una región de [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050).

    Esto puede suceder cuando el marco determina que es necesario dibujar la región (como cuando el usuario muestra una vista panorámica o acerca o aleja la vista de la superficie), o después de que la aplicación llame a [IVirtualSurfaceImageSourceNative::Invalidate](https://msdn.microsoft.com/library/windows/desktop/hh848332) en esa región.

5.  En [IVirtualSurfaceImageSourceNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848337), usa los métodos [IVirtualSurfaceImageSourceNative::GetUpdateRectCount](https://msdn.microsoft.com/library/windows/desktop/hh848329) y [IVirtualSurfaceImageSourceNative::GetUpdateRects](https://msdn.microsoft.com/library/windows/desktop/hh848330) para determinar qué regiones de la superficie deben dibujarse.

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

    1.  Proporciona un puntero para el objeto **ID2D1DeviceContext** a **ISurfaceImageSourceNativeWithD2D::BeginDraw**, y usa el contexto de dibujo devuelto para dibujar el contenido del rectángulo deseado dentro de **SurfaceImageSource**. **ISurfaceImageSourceNativeWithD2D::BeginDraw** y los comandos de dibujo se pueden llamar desde un subproceso en segundo plano. Solo se dibujará el área especificada para actualización en el parámetro *updateRect*.

        Este método devuelve el desplazamiento del punto (x,y) del rectángulo de destino actualizado en el parámetro *offset*. Puedes usar este desplazamiento para determinar dónde se debe dibujar el contenido actualizado con **ID2D1DeviceContext**.

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

    2.  Dibuja el contenido específico de la región, pero restringe el dibujo a las regiones delimitadas para mejorar el rendimiento.

    3.  Llama a **ISurfaceImageSourceNativeWithD2D::EndDraw**. El resultado es un mapa de bits.

> [!NOTE]
> Las aplicaciones deben evitar dibujar en **SurfaceImageSource** mientras su elemento [Window](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) asociado está oculto, de lo contrario se producirá un error en las API de **ISurfaceImageSourceNativeWithD2D**. Para ello, regístrate como escucha de eventos para el evento [Window.VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) para realizar un seguimiento de los cambios de visibilidad.

## <a name="swapchainpanel-and-gaming"></a>SwapChainPanel y juegos


[SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) es el tipo de Windows Runtime diseñado para permitir elementos gráficos y juegos de alto rendimiento en los que puedes administrar la cadena de intercambio directamente. En este caso, puedes crear tu propia cadena de intercambio de DirectX y administrar la presentación del contenido representado.

Para garantizar un buen rendimiento, el tipo [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) tiene algunas limitaciones:

-   No hay más de 4 instancias de [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) por aplicación.
-   Debes establecer el alto y ancho de la cadena de intercambio de DirectX (en [DXGI\_SWAP\_CHAIN\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528)) en las dimensiones actuales del elemento de dicha cadena de intercambio. Si no, el contenido de la pantalla se escalará (mediante **DXGI\_SCALING\_STRETCH**) para ajustarlo.
-   Debes establecer el modo de escalado de la cadena de intercambio de DirectX (en [DXGI\_SWAP\_CHAIN\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528)) en **DXGI\_SCALING\_STRETCH**.
-   No puedes establecer el modo alfa de la cadena de intercambio de DirectX (en [DXGI\_SWAP\_CHAIN\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528)) en **DXGI\_ALPHA\_MODE\PREMULTIPLIED**.
-   Debes crear la cadena de intercambio de DirectX llamando a [IDXGIFactory2::CreateSwapChainForComposition](https://msdn.microsoft.com/library/windows/desktop/hh404558).

Debes actualizar la clase [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) en función de las necesidades de la aplicación y no de las actualizaciones del marco XAML. Si necesitas sincronizar las actualizaciones de **SwapChainPanel** con las del marco XAML, regístrate para el evento [Windows::UI::Xaml::Media::CompositionTarget::Rendering](https://msdn.microsoft.com/library/windows/apps/br228127). En caso contrario, deberás tener en cuenta los posibles problemas entre subprocesos al intentar actualizar los elementos XAML de un subproceso distinto al que actualiza el objeto **SwapChainPanel**.

Si tienes que recibir una entrada de puntero de latencia baja en tu **SwapChainPanel**, usa [SwapChainPanel::CreateCoreIndependentInputSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.swapchainpanel.createcoreindependentinputsource). Este método devuelve un objeto [CoreIndependentInputSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coreindependentinputsource) que puede usarse para recibir eventos de entrada con una latencia mínima en un subproceso en segundo plano. Ten en cuenta que, cuando se llama a este método, no se desencadenará eventos normales de entrada de puntero XAML para **SwapChainPanel**, dado que todas las entradas se redirigirán al subproceso en segundo plano.


> **Nota**	En general, las aplicaciones DirectX deben crear cadenas de intercambio con orientación horizontal y con el mismo tamaño que la ventana de presentación (que suele ser la resolución de la pantalla nativa en la mayoría de juegos de Microsoft Store). De esta forma, se garantiza que la aplicación implemente de manera óptima la cadena de intercambio cuando no tenga ninguna superposición de XAML visible. Si la aplicación se gira y se coloca en modo vertical, esta debería llamar a [IDXGISwapChain1::SetRotation](https://msdn.microsoft.com/library/windows/desktop/hh446801) en la cadena de intercambio existente, aplicar una transformación al contenido si fuera necesario y luego llamar de nuevo a [SetSwapChain](https://msdn.microsoft.com/library/windows/desktop/dn302144) en la misma cadena de intercambio. De forma similar, la aplicación debe llamar de nuevo a **SetSwapChain** en la misma cadena de intercambio, siempre que se cambie el tamaño de la cadena de intercambio mediante una llamada a [IDXGISwapChain::ResizeBuffers](https://msdn.microsoft.com/library/windows/desktop/bb174577).


 

El procedimiento básico para crear y actualizar un objeto [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) en el código subyacente es el siguiente:

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

2.  Obtén un puntero para [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143). Convierte el objeto [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) en [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821) (o **IUnknown**), y llama a **QueryInterface** para obtener la implementación subyacente de **ISwapChainPanelNative**.

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  Crea el dispositivo DXGI y la cadena de intercambio, y establece la cadena de intercambio en [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143) pasándola a [SetSwapChain](https://msdn.microsoft.com/library/windows/desktop/dn302144).

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

* [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm)
* [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041)
* [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050)
* [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834)
* [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143)
* [Programming Guide for Direct3D 11 (Guía de programación para Direct3D 11)](https://msdn.microsoft.com/library/windows/desktop/ff476345)

 

 




