---
author: eliotcowley
title: Captura de pantalla
description: El espacio de nombres Windows.Graphics.Capture proporciona API para adquirir fotogramas desde una pantalla o ventana de aplicación, para crear secuencias de vídeo o instantáneas para crear experiencias interactivas y de colaboración.
ms.assetid: 349C959D-9C74-44E7-B5F6-EBDB5CA87B9F
ms.author: elcowle
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, captura de pantalla
ms.localizationpriority: medium
ms.openlocfilehash: d28ed1fce79a815155180ab8a3c708e2c8bf8916
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5817179"
---
# <a name="screen-capture"></a>Captura de pantalla

A partir de Windows 10, versión 1803, el espacio de nombres [Windows.Graphics.Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture) proporciona API para adquirir fotogramas desde una pantalla o ventana de aplicación, para crear secuencias de vídeo o instantáneas para crear experiencias interactivas y de colaboración.

Con la captura de pantalla, los desarrolladores invocan la interfaz de usuario de sistema seguro para que los usuarios finales elijan la pantalla o la ventana de la aplicación que se capturará, y se dibuja un borde de notificación amarilla alrededor del elemento capturado activamente. En el caso de varias sesiones de captura simultánea, se dibuja un borde amarillo alrededor de cada elemento que se captura.

> [!NOTE]
> La captura de pantalla API solo se admiten en cascos envolventes de realidad mixta de Windows y de escritorio.

## <a name="add-the-screen-capture-capability"></a>Agregar la funcionalidad de captura de pantalla

Las API que se encuentran en el espacio de nombres **Windows.Graphics.Capture** requieren una funcionalidad general deben declararse en el manifiesto de la aplicación:
    
1. Abre **Package.appxmanifest** en el **Explorador de soluciones**.
2. Selecciona la pestaña **Funcionalidades**.
3. Comprobar la **captura de gráficos**.

![Captura de gráficos](images/screen-capture-1.png)

## <a name="launch-the-system-ui-to-start-screen-capture"></a>Iniciar la interfaz de usuario del sistema para iniciar la captura de pantalla

Antes de iniciar la interfaz de usuario del sistema, puedes comprobar si la aplicación es capaz de realizar capturas de pantallas actualmente. Son varios los motivos por los que es posible que la aplicación no pueda usar la captura de pantalla, como si el dispositivo no cumple los requisitos de hardware o si la aplicación destinada para la captura bloquea las capturas de pantalla. Usa el método **IsSupported** de la clase [GraphicsCaptureSession](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturesession) para determinar si se admite la captura de pantalla de UWP:

```cs
// This runs when the application starts.
public void OnInitialization()
{
    if (!GraphicsCaptureSession.IsSupported()) 
    {
        // Hide the capture UI if screen capture is not supported.
        CaptureButton.Visibility = Visibility.Collapsed; 
    }    
}
```

Una vez que hayas comprobado que se admite la captura de pantalla, usa la clase [GraphicsCapturePicker](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturepicker) para invocar la interfaz de usuario del selector de sistema. El usuario final usa esta interfaz de usuario para seleccionar la ventana de la aplicación o la pantalla de la que se deben realizar capturas de pantalla. El selector de devolverá un [GraphicsCaptureItem](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscaptureitem) que se usará para crear un **GraphicsCaptureSession**:

```cs
public async Task StartCaptureAsync() 
{ 
    // The GraphicsCapturePicker follows the same pattern the 
    // file pickers do. 
    var picker = new GraphicsCapturePicker(); 
    GraphicsCaptureItem item = await picker.PickSingleItemAsync(); 

    // The item may be null if the user dismissed the 
    // control without making a selection or hit Cancel. 
    if (item != null) 
    {
        // We'll define this method later in the document.
        StartCaptureInternal(item); 
    } 
}
```

Dado que este es el código de la interfaz de usuario, debe llamarse en el subproceso de interfaz de usuario. Si estás llamarlo desde el código subyacente para una página de la aplicación (por ejemplo, **MainPage.xaml.cs**) Esto se realiza por TI automáticamente, pero si no, puedes forzar que se ejecute en el subproceso de interfaz de usuario con el siguiente código:

```cs
CoreWindow window = CoreApplication.MainView.CoreWindow;
           
await window.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, async () =>
{
    await StartCaptureAsync();
});
```

## <a name="create-a-capture-frame-pool-and-capture-session"></a>Crear una sesión de captura y grupo de fotogramas de captura

Con **GraphicsCaptureItem**, crearás un [Direct3D11CaptureFramePool](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframepool) con el dispositivo D3D, el formato de píxel admitido (**DXGI\_FORMAT\_B8G8R8A8\_UNORM**), el número de fotogramas deseados (que puede ser un entero) y el tamaño del fotograma. La propiedad **ContentSize** de la clase **GraphicsCaptureItem** se puede usar como el tamaño del fotograma:

```cs
private GraphicsCaptureItem _item;
private Direct3D11CaptureFramePool _framePool;
private CanvasDevice _canvasDevice;
private GraphicsCaptureSession _session;

public void StartCaptureInternal(GraphicsCaptureItem item) 
{
    _item = item;

    _framePool = Direct3D11CaptureFramePool.Create( 
        _canvasDevice, // D3D device 
        DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format 
        2, // Number of frames 
        _item.Size); // Size of the buffers   
} 
```

A continuación, obtén una instancia de la clase **GraphicsCaptureSession** para tu **Direct3D11CaptureFramePool** pasando el **GraphicsCaptureItem** al método **CreateCaptureSession**:

```cs
_session = _framePool.CreateCaptureSession(_item);
```

Una vez que el usuario haya dado su consentimiento explícito a la captura de ventana de aplicación o pantalla en la interfaz de usuario del sistema, el **GraphicsCaptureItem** se puede asociar a varios objetos **CaptureSession**. De este modo, la aplicación puede elegir capturar el mismo elemento para diversas experiencias.

Para capturar varios elementos al mismo tiempo, la aplicación debe crear una sesión de captura para cada elemento que se va a capturar, lo que requiere invocar la interfaz de usuario del selector para cada elemento que se va a capturar.

## <a name="acquire-capture-frames"></a>Adquirir fotogramas de captura

Con tu sesión de captura y grupo de fotogramas creados, llama al método **StartCapture** de tu instancia **GraphicsCaptureSession** para notificar al sistema para que inicie el envío de fotogramas de captura a tu aplicación:

```cs
_session.StartCapture();
```

Para adquirir estos fotogramas de captura, que son objetos [Direct3D11CaptureFrame](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframe), puedes usar el evento **Direct3D11CaptureFramePool.FrameArrived**:

```cs
_framePool.FrameArrived += (s, a) => 
{ 
    // The FrameArrived event fires for every frame on the thread that         
    // created the Direct3D11CaptureFramePool. This means we don't have to 
    // do a null-check here, as we know we're the only one  
    // dequeueing frames in our application.  

    // NOTE: Disposing the frame retires it and returns  
    // the buffer to the pool.
    using (var frame = _framePool.TryGetNextFrame()) 
    {
        // We'll define this method later in the document.
        ProcessFrame(frame); 
    }  
}; 
```

Se recomienda evitar el uso del subproceso de la interfaz de usuario si es posible para **FrameArrived**, ya que este evento se generará cada vez que haya un nuevo fotograma disponible, lo que será frecuente. Si decides elegir escuchar **FrameArrived** en el subproceso de la interfaz de usuario, ten en cuenta la cantidad de trabajo que realizas cada vez que se desencadena el evento.

También puedes extraer fotogramas manualmente con el método **Direct3D11CaptureFramePool.TryGetNextFrame** hasta que consigas todos los fotogramas que necesites.

El objeto **Direct3D11CaptureFrame** contiene las propiedades **ContentSize**, **Surface** y **SystemRelativeTime**. **SystemRelativeTime** es tiempo QPC ([QueryPerformanceCounter](https://msdn.microsoft.com/library/windows/desktop/ms644904)) que se puede usar para sincronizar otros elementos multimedia.

## <a name="processing-capture-frames"></a>Procesamiento de fotogramas de captura

Cada fotograma de **Direct3D11CaptureFramePool** se comprueba al llamar a **TryGetNextFrame** y se vuelve a comprobar en función de la duración del objeto **Direct3D11CaptureFrame**. Para aplicaciones nativas, soltar el objeto **Direct3D11CaptureFrame** es suficiente para volver a integrar el fotograma en la agrupación de fotogramas. Para aplicaciones administradas, se recomienda usar el método **Direct3D11CaptureFrame.Dispose** (**Cerrar** en C++). **Direct3D11CaptureFrame** implementa la interfaz de [IClosable](https://docs.microsoft.com/uwp/api/Windows.Foundation.IClosable), que se proyecta como [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) para los llamadores de C#.

Las aplicaciones no deben guardar referencias a objetos **Direct3D11CaptureFrame**, ni deben guardar referencias a la superficie de Direct3D subyacente después de que se haya vuelto a integrar el fotograma.

Al procesar un fotograma, se recomienda que las aplicaciones tomen el bloqueo de [ID3D11Multithread](https://msdn.microsoft.com/library/windows/desktop/mt644886) en el mismo dispositivo que está asociado al objeto **Direct3D11CaptureFramePool**.

La superficie de Direct3D subyacente siempre será del tamaño especificado al crear (o volver a crear) el **Direct3D11CaptureFramePool**. Si el tamaño del contenido es mayor que el del fotograma, el contenido se recorta al tamaño del fotograma. Si el contenido es menor que el fotograma, el resto del fotograma contiene datos sin definir. Se recomienda que las aplicaciones se copien fuera de un subrectángulo con la propiedad **ContentSize** para que ese **Direct3D11CaptureFrame** evite mostrar contenido no definido.

## <a name="react-to-capture-item-resizing-or-device-lost"></a>Reaccionar para capturar la pérdida del dispositivo o el cambio de tamaño de elementos

Durante el proceso de captura, las aplicaciones pueden desear cambiar aspectos de su **Direct3D11CaptureFramePool**. Esto incluye proporcionar un nuevo dispositivo Direct3D, cambiar el tamaño de los búferes de cuadros o incluso cambiar el número de búferes del grupo. En cada uno de estos escenarios, el método **Recreate** del objeto **Direct3D11CaptureFramePool** es la herramienta recomendada.

Cuando se llama a **Recreate**, se descartan todos los marcos existentes. Esto es para evitar repartir fotogramas cuyas superficies de Direct3D subyacentes pertenecen a un dispositivo al que ya no puede tener acceso la aplicación. Por este motivo, puede ser recomendable procesar todos los fotogramas pendientes antes de llamar a **Recreate**.

## <a name="putting-it-all-together"></a>Implementación

El siguiente fragmento de código es un ejemplo de principio a fin de cómo implementar la captura de pantalla en una aplicación para UWP. En este ejemplo, tenemos un botón en el front-end que, al hacer clic en, llama al método **Button_ClickAsync** .

> [!NOTE]
> Este fragmento de código usa [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm), una biblioteca para la representación de gráficos 2D. Consulta su documentación para obtener información acerca de cómo se configura para el proyecto.

```cs
using Microsoft.Graphics.Canvas;
using Microsoft.Graphics.Canvas.UI.Composition;
using System;
using System.Numerics;
using System.Threading.Tasks;
using Windows.Foundation;
using Windows.Graphics;
using Windows.Graphics.Capture;
using Windows.Graphics.DirectX;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;

namespace WindowsGraphicsCapture
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // Capture API objects.
        private SizeInt32 _lastSize;
        private GraphicsCaptureItem _item;
        private Direct3D11CaptureFramePool _framePool;
        private GraphicsCaptureSession _session;

        // Non-API related members.
        private CanvasDevice _canvasDevice;
        private CompositionGraphicsDevice _compositionGraphicsDevice;
        private Compositor _compositor;
        private CompositionDrawingSurface _surface;

        public MainPage()
        {
            this.InitializeComponent();
            Setup();
        }

        private void Setup()
        {
            _canvasDevice = new CanvasDevice();
            _compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(Window.Current.Compositor, _canvasDevice);
            _compositor = Window.Current.Compositor;

            _surface = _compositionGraphicsDevice.CreateDrawingSurface(
                new Size(400, 400),
                DirectXPixelFormat.B8G8R8A8UIntNormalized,
                DirectXAlphaMode.Premultiplied);    // This is the only value that currently works with the composition APIs.

            var visual = _compositor.CreateSpriteVisual();
            visual.RelativeSizeAdjustment = Vector2.One;
            var brush = _compositor.CreateSurfaceBrush(_surface);
            brush.HorizontalAlignmentRatio = 0.5f;
            brush.VerticalAlignmentRatio = 0.5f;
            brush.Stretch = CompositionStretch.Uniform;
            visual.Brush = brush;
            ElementCompositionPreview.SetElementChildVisual(this, visual);
        }

        public async Task StartCaptureAsync()
        {
            // The GraphicsCapturePicker follows the same pattern the 
            // file pickers do. 
            var picker = new GraphicsCapturePicker();
            GraphicsCaptureItem item = await picker.PickSingleItemAsync();

            // The item may be null if the user dismissed the 
            // control without making a selection or hit Cancel. 
            if (item != null)
            {
                StartCaptureInternal(item);
            }
        }

        private void StartCaptureInternal(GraphicsCaptureItem item)
        {
            // Stop the previous capture if we had one.
            StopCapture();

            _item = item;
            _lastSize = _item.Size;

            _framePool = Direct3D11CaptureFramePool.Create(
               _canvasDevice, // D3D device 
               DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format 
               2, // Number of frames 
               _item.Size); // Size of the buffers 

            _framePool.FrameArrived += (s, a) =>
            {
                // The FrameArrived event is raised for every frame on the thread
                // that created the Direct3D11CaptureFramePool. This means we 
                // don't have to do a null-check here, as we know we're the only 
                // one dequeueing frames in our application.  

                // NOTE: Disposing the frame retires it and returns  
                // the buffer to the pool.

                using (var frame = _framePool.TryGetNextFrame())
                {
                    ProcessFrame(frame);
                }
            };

            _item.Closed += (s, a) =>
            {
                StopCapture();
            };

            _session = _framePool.CreateCaptureSession(_item);
            _session.StartCapture();
        }

        public void StopCapture()
        {
            _session?.Dispose();
            _framePool?.Dispose();
            _item = null;
            _session = null;
            _framePool = null;
        }

        private void ProcessFrame(Direct3D11CaptureFrame frame)
        {
            // Resize and device-lost leverage the same function on the
            // Direct3D11CaptureFramePool. Refactoring it this way avoids 
            // throwing in the catch block below (device creation could always 
            // fail) along with ensuring that resize completes successfully and 
            // isn’t vulnerable to device-lost.   
            bool needsReset = false;
            bool recreateDevice = false;

            if ((frame.ContentSize.Width != _lastSize.Width) ||
                (frame.ContentSize.Height != _lastSize.Height))
            {
                needsReset = true;
                _lastSize = frame.ContentSize;
            }

            try
            {
                // Take the D3D11 surface and draw it into a  
                // Composition surface.

                // Convert our D3D11 surface into a Win2D object.
                var canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface(
                    _canvasDevice,
                    frame.Surface);

                // Helper that handles the drawing for us.
                FillSurfaceWithBitmap(canvasBitmap);
            }

            // This is the device-lost convention for Win2D.
            catch (Exception e) when (_canvasDevice.IsDeviceLost(e.HResult))
            {
                // We lost our graphics device. Recreate it and reset 
                // our Direct3D11CaptureFramePool.  
                needsReset = true;
                recreateDevice = true;
            }

            if (needsReset)
            {
                ResetFramePool(frame.ContentSize, recreateDevice);
            }
        }

        private void FillSurfaceWithBitmap(CanvasBitmap canvasBitmap)
        {
            CanvasComposition.Resize(_surface, canvasBitmap.Size);

            using (var session = CanvasComposition.CreateDrawingSession(_surface))
            {
                session.Clear(Colors.Transparent);
                session.DrawImage(canvasBitmap);
            }
        }

        private void ResetFramePool(SizeInt32 size, bool recreateDevice)
        {
            do
            {
                try
                {
                    if (recreateDevice)
                    {
                        _canvasDevice = new CanvasDevice();
                    }

                    _framePool.Recreate(
                        _canvasDevice,
                        DirectXPixelFormat.B8G8R8A8UIntNormalized,
                        2,
                        size);
                }
                // This is the device-lost convention for Win2D.
                catch (Exception e) when (_canvasDevice.IsDeviceLost(e.HResult))
                {
                    _canvasDevice = null;
                    recreateDevice = true;
                }
            } while (_canvasDevice == null);
        }

        private async void Button_ClickAsync(object sender, RoutedEventArgs e)
        {
            await StartCaptureAsync();
        }
    }
}
```

## <a name="see-also"></a>Ver también

* [Windows.Graphics.Capture Namespace](https://docs.microsoft.com/uwp/api/windows.graphics.capture)
