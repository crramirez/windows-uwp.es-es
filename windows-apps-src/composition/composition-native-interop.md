---
author: jwmsft
ms.assetid: 16ad97eb-23f1-0264-23a9-a1791b4a5b95
title: "Interoperación nativa de composición"
description: "La API de Windows.UI.Composition proporciona interfaces nativas de interoperación que permiten mover contenido directamente al compositor."
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 96af2b92ec301a93cbae9eb14422af9e1ec8554c
ms.sourcegitcommit: b42d14c775efbf449a544ddb881abd1c65c1ee86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2017
---
# <a name="composition-native-interoperation-with-directx-and-direct2d"></a>Interoperación nativa de composición con DirectX y Direct2D

La API de Windows.UI.Composition proporciona las interfaces nativas de interoperación [**ICompositorInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620068), [**ICompositionDrawingSurfaceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620058) e [**ICompositionGraphicsDeviceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620065) que permiten mover contenido directamente al compositor.

La interoperación nativa se estructura alrededor de objetos de superficies respaldados por texturas DirectX. Las superficies se crean a partir de un objeto de fábrica denominado [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749). Este objeto se respalda con un objeto de dispositivo Direct2D o Direct3D subyacente, que usa para asignar memoria de vídeo para las superficies. La API de composición nunca crea el dispositivo DirectX subyacente. Es la responsabilidad de la aplicación crear uno y pasarlo al objeto **CompositionGraphicsDevice**. Una aplicación puede crear más de un objeto **CompositionGraphicsDevice** a la vez y puede usar el mismo dispositivo DirectX como el dispositivo de representación de varios objetos **CompositionGraphicsDevice**.

## <a name="creating-a-surface"></a>Crear una superficie

Cada objeto [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) actúa como una fábrica de superficies. Cada superficie se crea con un tamaño inicial (que puede ser 0,0), pero sin píxeles válidos. En su estado inicial, una superficie se puede consumir inmediatamente en un árbol visual, por ejemplo, a través de [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) y [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433), pero en su estado inicial la superficie no tiene ningún efecto en la salida de la pantalla. Para todos los propósitos, es totalmente transparente, incluso si el modo alfa especificado es "opaco".

En ocasiones, los dispositivos DirectX se pueden considerar como inutilizables. Esto puede suceder, entre otras razones, si la aplicación pasa argumentos no válidos a determinadas API de DirectX, si el sistema restablece el adaptador de gráficos o si se actualiza el controlador. Direct3D tiene una API que una aplicación puede usar para detectar, de manera asincrónica, si el dispositivo se pierde por cualquier motivo. Cuando se pierde un dispositivo DirectX, la aplicación debe descartarlo, crear uno nuevo y pasarlo a los objetos [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) asociados anteriormente con el dispositivo DirectX incorrecto.

## <a name="loading-pixels-into-a-surface"></a>Cargar píxeles en una superficie

Para cargar píxeles en la superficie, la aplicación debe llamar al método [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx) que devuelve una interfaz DirectX que representa una textura o contexto Direct2D, en función de lo que solicita la aplicación. A continuación, la aplicación debe representar o cargar píxeles en esa textura. Cuando termine la aplicación, debe llamar al método [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060). Solo en ese punto los píxeles estarán disponibles para la composición. Sin embargo, aún no se mostrarán en pantalla hasta la próxima vez que se confirmen todos los cambios en el árbol visual. Si el árbol visual se confirma antes de que se llame al método **EndDraw**, la actualización que está en curso no estará visible en pantalla y la superficie seguirá mostrando el contenido que tenía antes de **BeginDraw**. Cuando se llama a **EndDraw**, se invalida la textura o el puntero de contexto Direct2D que devuelve BeginDraw. Una aplicación nunca debe almacenar ese puntero en caché más allá de la llamada a **EndDraw**.

La aplicación solo puede llamar a BeginDraw en una superficie a la vez, para cualquier objeto [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) dado. Después de llamar a [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx), la aplicación debe llamar a [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) en esa superficie antes de llamar a **BeginDraw** en otra. Dado que la API es ágil, la aplicación es responsable de sincronizar estas llamadas si desea realizar la representación desde varios subprocesos de trabajo. Si una aplicación desea interrumpir la representación de una superficie y cambiar a otra temporalmente, puede usar el método [**SuspendDraw**](https://msdn.microsoft.com/library/windows/apps/mt620064.aspx). Esto permite que otro objeto **BeginDraw** realice la acción correctamente, pero no hace que la primera actualización de superficie esté disponible para la composición en pantalla. Esto permite que la aplicación haga varias actualizaciones de forma transaccional. Cuando una superficie se suspende, la aplicación puede continuar con la actualización con una llamada al método [**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062) o declarar que la actualización ha finalizado con una llamada a **EndDraw**. Esto significa que se puede actualizar activamente solo una superficie para cualquier objeto **CompositionGraphicsDevice** dado. Cada dispositivo de gráficos mantiene este estado independientemente de los demás, por lo que una aplicación puede representar dos superficies simultáneamente si pertenecen a dispositivos gráficos diferentes. Sin embargo, esto excluye la memoria de vídeo de esas dos superficies de la agrupación y, como tal, el uso de la memoria es menos eficiente.

Los métodos [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx), [**SuspendDraw**](https://msdn.microsoft.com/library/windows/apps/mt620064.aspx), [**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062) y [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) devuelven errores si la aplicación realiza una operación incorrecta (por ejemplo, pasar argumentos no válidos o llamar a **BeginDraw** en una superficie antes de llamar a **EndDraw** en otra). Estos tipos de errores representan errores de aplicación y, por lo tanto, la expectativa es que se controlan con un error inmediato. **BeginDraw** también puede devolver un error si se pierde el dispositivo DirectX subyacente. Este error no es grave, ya que la aplicación puede volver a crear su dispositivo DirectX e intentarlo de nuevo. Por consiguiente, se espera que la aplicación procese la pérdida del dispositivo simplemente omitiendo la representación. Si se produce un error en **BeginDraw** por cualquier motivo, la aplicación tampoco debe llamar a **EndDraw**, ya que el inicio no se realizó correctamente en primer lugar.

## <a name="scrolling"></a>Desplazamiento

Por motivos de rendimiento, cuando una aplicación llama a [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx), no se garantiza que el contenido de la textura devuelta sea el contenido anterior de la superficie. La aplicación debe suponer que el contenido es aleatorio y, por lo tanto, debe asegurar que se toquen todos los píxeles, ya sea borrando la superficie antes de la representación o dibujando suficiente contenido opaco para cubrir todo el rectángulo actualizado. Esto, junto con el hecho de que el puntero de textura solo es válido entre las llamadas a **BeginDraw** y [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060), impide que la aplicación copie el contenido anterior de la superficie. Por este motivo, ofrecemos un método [**Scroll**](https://msdn.microsoft.com/library/windows/apps/mt620063), que permite que la aplicación realice una copia de los píxeles de la misma superficie.

## <a name="usage-example"></a>Ejemplo de uso

En el siguiente ejemplo se muestra un escenario muy sencillo, donde una aplicación crea superficies de dibujo y usa [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx) y [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) para rellenar las superficies con texto. La aplicación usa DirectWrite para el diseño del texto (los detalles no se muestran) y luego usa Direct2D para procesarlo. El dispositivo de gráficos de composición acepta el dispositivo Direct2D directamente en el momento de inicialización. Esto permite a **BeginDraw** devolver un puntero de interfaz ID2D1DeviceContext, que es bastante más eficaz que cuando la aplicación crea un contexto de Direct2D para ajustar una interfaz ID3D11Texture2D devuelta en cada operación de dibujo.

```cpp
//------------------------------------------------------------------------------
//
// Copyright (C) Microsoft. All rights reserved.
//
//------------------------------------------------------------------------------

#include "stdafx.h"

using namespace Microsoft::WRL;
using namespace Windows::Foundation;
using namespace Windows::Graphics::DirectX;
using namespace Windows::UI::Composition;

// This is an app-provided helper to render lines of text
class SampleText
{
private:
    // The text to draw
    ComPtr<IDWriteTextLayout> _text;

    // The composition surface that we use in the visual tree
    ComPtr<ICompositionDrawingSurfaceInterop> _drawingSurfaceInterop;

    // The device that owns the surface
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;

    // For managing our event notifier
    EventRegistrationToken _deviceReplacedEventToken;

public:
    SampleText(IDWriteTextLayout* text, ICompositionGraphicsDevice* compositionGraphicsDevice) :
        _text(text),
        _compositionGraphicsDevice(compositionGraphicsDevice)
    {
        // Create the surface just big enough to hold the formatted text block.
        DWRITE_TEXT_METRICS metrics;
        FailFastOnFailure(text->GetMetrics(&metrics));
        Windows::Foundation::Size surfaceSize = { metrics.width, metrics.height };
        ComPtr<ICompositionDrawingSurface> drawingSurface;
        FailFastOnFailure(_compositionGraphicsDevice->CreateDrawingSurface(
            surfaceSize,
            DirectXPixelFormat::DirectXPixelFormat_B8G8R8A8UIntNormalized,
            DirectXAlphaMode::DirectXAlphaMode_Ignore,
            &drawingSurface));

        // Cache the interop pointer, since that's what we always use.
        FailFastOnFailure(drawingSurface.As(&_drawingSurfaceInterop));

        // Draw the text
        DrawText();

        // If the rendering device is lost, the application will recreate and replace it. We then
        // own redrawing our pixels.
        FailFastOnFailure(_compositionGraphicsDevice->add_RenderingDeviceReplaced(
            Callback<RenderingDeviceReplacedEventHandler>([this](
                ICompositionGraphicsDevice* source, IRenderingDeviceReplacedEventArgs* args)
                -> HRESULT
            {
                // Draw the text again
                DrawText();
                return S_OK;
            }).Get(),
            &_deviceReplacedEventToken));
    }

    ~SampleText()
    {
        FailFastOnFailure(_compositionGraphicsDevice->remove_RenderingDeviceReplaced(
            _deviceReplacedEventToken));
    }

    // Return the underlying surface to the caller
    ComPtr<ICompositionSurface> get_Surface()
    {
        // To the caller, the fact that we have a drawing surface is an implementation detail.
        // Return the base interface instead
        ComPtr<ICompositionSurface> surface;
        FailFastOnFailure(_drawingSurfaceInterop.As(&surface));
        return surface;
    }

private:
    // We may detect device loss on BeginDraw calls. This helper handles this condition or other
    // errors.
    bool CheckForDeviceRemoved(HRESULT hr)
    {
        if (SUCCEEDED(hr))
        {
            // Everything is fine -- go ahead and draw
            return true;
        }
        else if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            // We can't draw at this time, but this failure is recoverable. Just skip drawing for
            // now. We will be asked to draw again once the Direct3D device is recreated
            return false;
        }
        else
        {
            // Any other error is unexpected and, therefore, fatal
            FailFast();
        }
    }

    // Renders the text into our composition surface
    void DrawText()
    {
        // Begin our update of the surface pixels. If this is our first update, we are required
        // to specify the entire surface, which nullptr is shorthand for (but, as it works out,
        // any time we make an update we touch the entire surface, so we always pass nullptr).
        ComPtr<ID2D1DeviceContext> d2dDeviceContext;
        POINT offset;
        if (CheckForDeviceRemoved(_drawingSurfaceInterop->BeginDraw(nullptr,
            __uuidof(ID2D1DeviceContext), &d2dDeviceContext, &offset)))
        {
            // Create a solid color brush for the text. A more sophisticated application might want
            // to cache and reuse a brush across all text elements instead, taking care to recreate
            // it in the event of device removed.
            ComPtr<ID2D1SolidColorBrush> brush;
            FailFastOnFailure(d2dDeviceContext->CreateSolidColorBrush(
                D2D1::ColorF(D2D1::ColorF::Black, 1.0f), &brush));

            // Draw the line of text at the specified offset, which corresponds to the top-left
            // corner of our drawing surface. Notice we don't call BeginDraw on the D2D device
            // context; this has already been done for us by the composition API.
            d2dDeviceContext->DrawTextLayout(D2D1::Point2F(offset.x, offset.y), _text.Get(),
                brush.Get());

            // Our update is done. EndDraw never indicates rendering device removed, so any
            // failure here is unexpected and, therefore, fatal.
            FailFastOnFailure(_drawingSurfaceInterop->EndDraw());
        }
    }
};

class SampleApp
{
    ComPtr<ICompositor> _compositor;
    ComPtr<ID2D1Device> _d2dDevice;
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;
    std::vector<ComPtr<SampleText>> _textSurfaces;

public:
    // Run once when the application starts up
    void Initialize(ICompositor* compositor)
    {
        // Cache the compositor (created outside of this method)
        _compositor = compositor;

        // Create a Direct2D device (helper implementation not shown here)
        FailFastOnFailure(CreateDirect2DDevice(&_d2dDevice));

        // To create a composition graphics device, we need to QI for another interface
        ComPtr<ICompositorInterop> compositorInterop;
        FailFastOnFailure(_compositor.As(&compositorInterop));

        // Create a graphics device backed by our D3D device
        FailFastOnFailure(compositorInterop->CreateGraphicsDevice(
            _d2dDevice.Get(),
            &_compositionGraphicsDevice));
    }

    // Called when Direct3D signals the device lost event
    void OnDirect3DDeviceLost()
    {
        // Create a new device
        FailFastOnFailure(CreateDirect2DDevice(_d2dDevice.ReleaseAndGetAddressOf()));

        // Restore our composition graphics device to good health
        ComPtr<ICompositionGraphicsDeviceInterop> compositionGraphicsDeviceInterop;
        FailFastOnFailure(_compositionGraphicsDevice.As(&compositionGraphicsDeviceInterop));
        FailFastOnFailure(compositionGraphicsDeviceInterop->SetRenderingDevice(_d2dDevice.Get()));
    }

    // Create a surface that is asynchronously filled with an image
    ComPtr<ICompositionSurface> CreateSurfaceFromTextLayout(IDWriteTextLayout* text)
    {
        // Create our wrapper object that will handle downloading and decoding the image (assume
        // throwing new here)
        SampleText* textSurface = new SampleText(text, _compositionGraphicsDevice.Get());

        // Keep our image alive
        _textSurfaces.push_back(textSurface);

        // The caller is only interested in the underlying surface
        return textSurface->get_Surface();
    }

    // Create a visual that holds an image
    ComPtr<IVisual> CreateVisualFromTextLayout(IDWriteTextLayout* text)
    {
        // Create a sprite visual
        ComPtr<ISpriteVisual> spriteVisual;
        FailFastOnFailure(_compositor->CreateSpriteVisual(&spriteVisual));

        // The sprite visual needs a brush to hold the image
        ComPtr<ICompositionSurfaceBrush> surfaceBrush;
        FailFastOnFailure(_compositor->CreateSurfaceBrushWithSurface(
            CreateSurfaceFromTextLayout(text).Get(),
            &surfaceBrush));

        // Associate the brush with the visual
        ComPtr<ICompositionBrush> brush;
        FailFastOnFailure(surfaceBrush.As(&brush));
        FailFastOnFailure(spriteVisual->put_Brush(brush.Get()));

        // Return the visual to the caller as the base class
        ComPtr<IVisual> visual;
        FailFastOnFailure(spriteVisual.As(&visual));

        return visual;
    }

private:
    // This helper (implementation not shown here) creates a Direct2D device and registers
    // for a device loss notification on the underlying Direct3D device. When that notification is
    // raised, assume the OnDirect3DDeviceLost method is called.
    HRESULT CreateDirect2DDevice(ID2D1Device** ppDevice);
};
```

 

 




