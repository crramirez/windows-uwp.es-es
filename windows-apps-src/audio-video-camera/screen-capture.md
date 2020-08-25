---
title: Captura de pantalla
description: El espacio de nombres Windows.Graphics.Capture proporciona API para adquirir fotogramas desde una pantalla o ventana de aplicación, para crear secuencias de vídeo o instantáneas para crear experiencias interactivas y de colaboración.
ms.assetid: 349C959D-9C74-44E7-B5F6-EBDB5CA87B9F
ms.date: 06/14/2019
ms.topic: article
dev_langs:
- csharp
- vb
keywords: Windows 10, UWP, captura de pantalla
ms.localizationpriority: medium
ms.openlocfilehash: fce0dbad0e36fe2470d8e07944afa80054cfb3d7
ms.sourcegitcommit: a5031e95b90ee72babace8e80370551f3fa88593
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2020
ms.locfileid: "88722030"
---
# <a name="screen-capture"></a>Captura de pantalla

A partir de Windows 10, versión 1803, el espacio de nombres [Windows. Graphics. Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture) proporciona las API para adquirir fotogramas de una ventana de pantalla o de aplicación, para crear secuencias de vídeo o instantáneas para generar experiencias de colaboración e interactivas.

Con la captura de pantalla, los desarrolladores invocan la interfaz de usuario segura del sistema para que los usuarios finales seleccionen la ventana de pantalla o de la aplicación que se va a capturar, y el sistema dibuja un borde de notificación amarillo alrededor del elemento capturado activamente. En el caso de varias sesiones de captura simultáneas, se dibuja un borde amarillo alrededor de cada elemento que se captura.

> [!NOTE]
> Las API de captura de pantalla solo se admiten en auriculares de escritorio y Windows Mixed Reality.

## <a name="add-the-screen-capture-capability"></a>Agregar la funcionalidad de captura de pantalla

Las API que se encuentran en el espacio de nombres **Windows. Graphics. Capture** requieren que se declare una funcionalidad general en el manifiesto de la aplicación:

1. Abra **Package. appxmanifest** en el **Explorador de soluciones**.
2. Seleccione la pestaña **Funcionalidades**.
3. Compruebe la **captura de gráficos**.

![Captura de gráficos](images/screen-capture-1.png)

## <a name="launch-the-system-ui-to-start-screen-capture"></a>Iniciar la interfaz de usuario del sistema para iniciar la captura de pantalla

Antes de iniciar la interfaz de usuario del sistema, puede comprobar si la aplicación puede realizar capturas de pantalla. Hay varios motivos por los que es posible que la aplicación no pueda usar la captura de pantalla, lo que incluye si el dispositivo no cumple los requisitos de hardware o si la aplicación destinada a captura de pantalla bloquea la captura de pantalla. Use el método **IsSupported** de la clase [GraphicsCaptureSession](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturesession) para determinar si se admite la captura de pantalla de UWP:

```csharp
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

```vb
Public Sub OnInitialization()
    If Not GraphicsCaptureSession.IsSupported Then
        CaptureButton.Visibility = Visibility.Collapsed
    End If
End Sub
```

Una vez que haya comprobado que se admite la captura de pantalla, use la clase [GraphicsCapturePicker](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturepicker) para invocar la interfaz de usuario del selector de sistema. El usuario final utiliza esta interfaz de usuario para seleccionar la ventana de pantalla o de la aplicación en la que se van a realizar capturas de pantalla. El selector devolverá un [GraphicsCaptureItem](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscaptureitem) que se usará para crear un **GraphicsCaptureSession**:

```csharp
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

```vb
Public Async Function StartCaptureAsync() As Task
    ' The GraphicsCapturePicker follows the same pattern the
    ' file pickers do.
    Dim picker As New GraphicsCapturePicker
    Dim item As GraphicsCaptureItem = Await picker.PickSingleItemAsync()

    ' The item may be null if the user dismissed the
    ' control without making a selection or hit Cancel.
    If item IsNot Nothing Then
        StartCaptureInternal(item)
    End If
End Function
```

Dado que se trata de código de interfaz de usuario, debe llamarse en el subproceso de la interfaz de usuario. Si lo llama desde el código subyacente de una página de la aplicación (como **mainpage.Xaml.CS**), esto se hace automáticamente, pero si no es así, puede forzar que se ejecute en el subproceso de la interfaz de usuario con el código siguiente:

```csharp
CoreWindow window = CoreApplication.MainView.CoreWindow;

await window.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, async () =>
{
    await StartCaptureAsync();
});
```

```vb
Dim window As CoreWindow = CoreApplication.MainView.CoreWindow
Await window.Dispatcher.RunAsync(CoreDispatcherPriority.Normal,
                                 Async Sub() Await StartCaptureAsync())
```

## <a name="create-a-capture-frame-pool-and-capture-session"></a>Crear un grupo de tramas de captura y una sesión de captura

Con **GraphicsCaptureItem**, creará un [Direct3D11CaptureFramePool](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframepool) con el dispositivo D3D, el formato de píxel compatible (** \_ formato DXGI \_ B8G8R8A8 \_ UNORM**), el número de fotogramas deseados (que puede ser cualquier entero) y el tamaño del marco. La propiedad de **contenido** de la clase **GraphicsCaptureItem** se puede usar como el tamaño del marco:

```csharp
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

```vb
WithEvents CaptureItem As GraphicsCaptureItem
WithEvents FramePool As Direct3D11CaptureFramePool
Private _canvasDevice As CanvasDevice
Private _session As GraphicsCaptureSession

Private Sub StartCaptureInternal(item As GraphicsCaptureItem)
    CaptureItem = item

    FramePool = Direct3D11CaptureFramePool.Create(
        _canvasDevice, ' D3D device
        DirectXPixelFormat.B8G8R8A8UIntNormalized, ' Pixel format
        2, '  Number of frames
        CaptureItem.Size) ' Size of the buffers
End Sub
```

A continuación, obtenga una instancia de la clase **GraphicsCaptureSession** para **Direct3D11CaptureFramePool** pasando **GraphicsCaptureItem** al método **CreateCaptureSession** :

```csharp
_session = _framePool.CreateCaptureSession(_item);
```

```vb
_session = FramePool.CreateCaptureSession(CaptureItem)
```

Una vez que el usuario ha dado su consentimiento explícitamente para capturar una ventana de la aplicación o mostrarla en la interfaz de usuario del sistema, el **GraphicsCaptureItem** se puede asociar a varios objetos **CaptureSession** . De esta manera, la aplicación puede elegir capturar el mismo elemento para varias experiencias.

Para capturar varios elementos al mismo tiempo, la aplicación debe crear una sesión de captura para cada elemento que se va a capturar, lo que requiere invocar la interfaz de usuario del selector para cada elemento que se va a capturar.

## <a name="acquire-capture-frames"></a>Adquirir fotogramas de captura

Con el grupo de Marcos y la sesión de captura creados, llame al método **StartCapture** en la instancia de **GraphicsCaptureSession** para notificar al sistema que empiece a enviar fotogramas de captura a la aplicación:

```csharp
_session.StartCapture();
```

```vb
_session.StartCapture()
```

Para adquirir estos fotogramas de captura, que son objetos [Direct3D11CaptureFrame](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframe) , puede usar el evento **Direct3D11CaptureFramePool. FrameArrived** :

```csharp
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

```vb
Private Sub FramePool_FrameArrived(sender As Direct3D11CaptureFramePool, args As Object) Handles FramePool.FrameArrived
    ' The FrameArrived event is raised for every frame on the thread
    ' that created the Direct3D11CaptureFramePool. This means we
    ' don't have to do a null-check here, as we know we're the only
    ' one dequeueing frames in our application.  

    ' NOTE Disposing the frame retires it And returns  
    ' the buffer to the pool.

    Using frame = FramePool.TryGetNextFrame()
        ProcessFrame(frame)
    End Using
End Sub
```

Se recomienda evitar el uso del subproceso de la interfaz de usuario si es posible para **FrameArrived**, ya que este evento se generará cada vez que haya un nuevo marco disponible, lo que será frecuente. Si elige escuchar **FrameArrived** en el subproceso de la interfaz de usuario, tenga en cuanta cuánto trabajo está haciendo cada vez que se activa el evento.

Como alternativa, puede extraer fotogramas manualmente con el método **Direct3D11CaptureFramePool. TryGetNextFrame** hasta que obtenga todos los fotogramas que necesite.

El objeto **Direct3D11CaptureFrame** contiene **las propiedades containing**, **Surface**y **SystemRelativeTime**. **SystemRelativeTime** es el tiempo de QPC ([QueryPerformanceCounter](https://docs.microsoft.com/windows/desktop/api/profileapi/nf-profileapi-queryperformancecounter)) que se puede usar para sincronizar otros elementos multimedia.

## <a name="process-capture-frames"></a>Procesar fotogramas de captura

Cada fotograma del **Direct3D11CaptureFramePool** se desprotege cuando se llama a **TryGetNextFrame**y se vuelve a proteger según la duración del objeto **Direct3D11CaptureFrame** . En el caso de las aplicaciones nativas, la liberación del objeto **Direct3D11CaptureFrame** es suficiente para volver a comprobar el fotograma en el grupo de Marcos. En el caso de las aplicaciones administradas, se recomienda usar el método **Direct3D11CaptureFrame. Dispose** (**Close** en C++). **Direct3D11CaptureFrame** implementa la interfaz [IClosable](https://docs.microsoft.com/uwp/api/Windows.Foundation.IClosable) , que se proyecta como [IDisposable](https://docs.microsoft.com/dotnet/api/system.idisposable) para los llamadores de C#.

Las aplicaciones no deben guardar las referencias a los objetos **Direct3D11CaptureFrame** , ni deben guardar las referencias a la superficie de Direct3D subyacente una vez que se ha vuelto a proteger el fotograma.

Al procesar un fotograma, se recomienda que las aplicaciones tomen el bloqueo [ID3D11Multithread](https://docs.microsoft.com/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11multithread) en el mismo dispositivo que está asociado con el objeto **Direct3D11CaptureFramePool** .

La superficie de Direct3D subyacente siempre será el tamaño especificado al crear (o volver a crear) el **Direct3D11CaptureFramePool**. Si el contenido es mayor que el fotograma, el contenido se recorta al tamaño del marco. Si el contenido es menor que el fotograma, el resto del marco contiene datos sin definir. Se recomienda que las aplicaciones copien un elemento sub-Rect mediante la propiedad de **contenido** de ese **Direct3D11CaptureFrame** para evitar que se muestre contenido no definido.

## <a name="take-a-screenshot"></a>Realizar una captura de pantalla

En nuestro ejemplo, convertiremos cada **Direct3D11CaptureFrame** en un [CanvasBitmap](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasBitmap.htm), que forma parte de las [API de Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm).

```csharp
// Convert our D3D11 surface into a Win2D object.
CanvasBitmap canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface(
    _canvasDevice,
    frame.Surface);
```

Una vez que tenemos el **CanvasBitmap**, podemos guardarlo como un archivo de imagen. En el ejemplo siguiente, se guarda como un archivo PNG en la carpeta **imágenes guardadas** del usuario.

```csharp
StorageFolder pictureFolder = KnownFolders.SavedPictures;
StorageFile file = await pictureFolder.CreateFileAsync("test.png", CreationCollisionOption.ReplaceExisting);

using (var fileStream = await file.OpenAsync(FileAccessMode.ReadWrite))
{
    await canvasBitmap.SaveAsync(fileStream, CanvasBitmapFileFormat.Png, 1f);
}
```

## <a name="react-to-capture-item-resizing-or-device-lost"></a>ReAct para capturar el cambio de tamaño del elemento o el dispositivo perdido

Durante el proceso de captura, las aplicaciones pueden querer cambiar los aspectos de sus **Direct3D11CaptureFramePool**. Esto incluye proporcionar un nuevo dispositivo Direct3D, cambiar el tamaño de los búferes de fotogramas o incluso cambiar el número de búferes dentro del grupo. En cada uno de estos escenarios, el método **Recreate** en el objeto **Direct3D11CaptureFramePool** es la herramienta recomendada.

Cuando se llama a **Recreate** , se descartan todos los marcos existentes. Esto es para evitar que se entreguen fotogramas cuyas superficies de Direct3D subyacentes pertenezcan a un dispositivo al que la aplicación ya no tenga acceso. Por esta razón, puede ser conveniente procesar todos los marcos pendientes antes de llamar a **Recreate**.

## <a name="putting-it-all-together"></a>Resumen

El siguiente fragmento de código es un ejemplo de un extremo a otro de cómo implementar la captura de pantalla en una aplicación de UWP. En este ejemplo, tenemos dos botones en el front-end: uno llama **Button_ClickAsync**y el otro llama a **ScreenshotButton_ClickAsync**.

> [!NOTE]
> Este fragmento de código usa [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm), una biblioteca para la representación de gráficos 2D. Consulte su documentación para obtener información sobre cómo configurarlo para su proyecto.

```csharp
using Microsoft.Graphics.Canvas;
using Microsoft.Graphics.Canvas.UI.Composition;
using System;
using System.Numerics;
using System.Threading.Tasks;
using Windows.Foundation;
using Windows.Graphics;
using Windows.Graphics.Capture;
using Windows.Graphics.DirectX;
using Windows.Storage;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;

namespace ScreenCaptureTest
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
        private CanvasBitmap _currentFrame;
        private string _screenshotFilename = "test.png";

        public MainPage()
        {
            this.InitializeComponent();
            Setup();
        }

        private void Setup()
        {
            _canvasDevice = new CanvasDevice();

            _compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(
                Window.Current.Compositor,
                _canvasDevice);

            _compositor = Window.Current.Compositor;

            _surface = _compositionGraphicsDevice.CreateDrawingSurface(
                new Size(400, 400),
                DirectXPixelFormat.B8G8R8A8UIntNormalized,
                DirectXAlphaMode.Premultiplied);    // This is the only value that currently works with
                                                    // the composition APIs.

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
                CanvasBitmap canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface(
                    _canvasDevice,
                    frame.Surface);

                _currentFrame = canvasBitmap;

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

        private async void ScreenshotButton_ClickAsync(object sender, RoutedEventArgs e)
        {
            await SaveImageAsync(_screenshotFilename, _currentFrame);
        }

        private async Task SaveImageAsync(string filename, CanvasBitmap frame)
        {
            StorageFolder pictureFolder = KnownFolders.SavedPictures;

            StorageFile file = await pictureFolder.CreateFileAsync(
                filename,
                CreationCollisionOption.ReplaceExisting);

            using (var fileStream = await file.OpenAsync(FileAccessMode.ReadWrite))
            {
                await frame.SaveAsync(fileStream, CanvasBitmapFileFormat.Png, 1f);
            }
        }
    }
}
```

```vb
Imports System.Numerics
Imports Microsoft.Graphics.Canvas
Imports Microsoft.Graphics.Canvas.UI.Composition
Imports Windows.Graphics
Imports Windows.Graphics.Capture
Imports Windows.Graphics.DirectX
Imports Windows.UI
Imports Windows.UI.Composition
Imports Windows.UI.Xaml.Hosting

Partial Public NotInheritable Class MainPage
    Inherits Page

    ' Capture API objects.
    WithEvents CaptureItem As GraphicsCaptureItem
    WithEvents FramePool As Direct3D11CaptureFramePool

    Private _lastSize As SizeInt32
    Private _session As GraphicsCaptureSession

    ' Non-API related members.
    Private _canvasDevice As CanvasDevice
    Private _compositionGraphicsDevice As CompositionGraphicsDevice
    Private _compositor As Compositor
    Private _surface As CompositionDrawingSurface

    Sub New()
        InitializeComponent()
        Setup()
    End Sub

    Private Sub Setup()
        _canvasDevice = New CanvasDevice()
        _compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(Window.Current.Compositor, _canvasDevice)
        _compositor = Window.Current.Compositor
        _surface = _compositionGraphicsDevice.CreateDrawingSurface(
            New Size(400, 400), DirectXPixelFormat.B8G8R8A8UIntNormalized, DirectXAlphaMode.Premultiplied)
        Dim visual = _compositor.CreateSpriteVisual()
        visual.RelativeSizeAdjustment = Vector2.One
        Dim brush = _compositor.CreateSurfaceBrush(_surface)
        brush.HorizontalAlignmentRatio = 0.5F
        brush.VerticalAlignmentRatio = 0.5F
        brush.Stretch = CompositionStretch.Uniform
        visual.Brush = brush
        ElementCompositionPreview.SetElementChildVisual(Me, visual)
    End Sub

    Public Async Function StartCaptureAsync() As Task
        ' The GraphicsCapturePicker follows the same pattern the
        ' file pickers do.
        Dim picker As New GraphicsCapturePicker
        Dim item As GraphicsCaptureItem = Await picker.PickSingleItemAsync()

        ' The item may be null if the user dismissed the
        ' control without making a selection or hit Cancel.
        If item IsNot Nothing Then
            StartCaptureInternal(item)
        End If
    End Function

    Private Sub StartCaptureInternal(item As GraphicsCaptureItem)
        ' Stop the previous capture if we had one.
        StopCapture()

        CaptureItem = item
        _lastSize = CaptureItem.Size

        FramePool = Direct3D11CaptureFramePool.Create(
            _canvasDevice, ' D3D device
            DirectXPixelFormat.B8G8R8A8UIntNormalized, ' Pixel format
            2, '  Number of frames
            CaptureItem.Size) ' Size of the buffers

        _session = FramePool.CreateCaptureSession(CaptureItem)
        _session.StartCapture()
    End Sub

    Private Sub FramePool_FrameArrived(sender As Direct3D11CaptureFramePool, args As Object) Handles FramePool.FrameArrived
        ' The FrameArrived event is raised for every frame on the thread
        ' that created the Direct3D11CaptureFramePool. This means we
        ' don't have to do a null-check here, as we know we're the only
        ' one dequeueing frames in our application.  

        ' NOTE Disposing the frame retires it And returns  
        ' the buffer to the pool.

        Using frame = FramePool.TryGetNextFrame()
            ProcessFrame(frame)
        End Using
    End Sub

    Private Sub CaptureItem_Closed(sender As GraphicsCaptureItem, args As Object) Handles CaptureItem.Closed
        StopCapture()
    End Sub

    Public Sub StopCapture()
        _session?.Dispose()
        FramePool?.Dispose()
        CaptureItem = Nothing
        _session = Nothing
        FramePool = Nothing
    End Sub

    Private Sub ProcessFrame(frame As Direct3D11CaptureFrame)
        ' Resize and device-lost leverage the same function on the
        ' Direct3D11CaptureFramePool. Refactoring it this way avoids
        ' throwing in the catch block below (device creation could always
        ' fail) along with ensuring that resize completes successfully And
        ' isn't vulnerable to device-lost.

        Dim needsReset As Boolean = False
        Dim recreateDevice As Boolean = False

        If (frame.ContentSize.Width <> _lastSize.Width) OrElse
            (frame.ContentSize.Height <> _lastSize.Height) Then
            needsReset = True
            _lastSize = frame.ContentSize
        End If

        Try
            ' Take the D3D11 surface and draw it into a  
            ' Composition surface.

            ' Convert our D3D11 surface into a Win2D object.
            Dim bitmap = CanvasBitmap.CreateFromDirect3D11Surface(
                _canvasDevice,
                frame.Surface)

            ' Helper that handles the drawing for us.
            FillSurfaceWithBitmap(bitmap)
            ' This is the device-lost convention for Win2D.
        Catch e As Exception When _canvasDevice.IsDeviceLost(e.HResult)
            ' We lost our graphics device. Recreate it and reset
            ' our Direct3D11CaptureFramePool.  
            needsReset = True
            recreateDevice = True
        End Try

        If needsReset Then
            ResetFramePool(frame.ContentSize, recreateDevice)
        End If
    End Sub

    Private Sub FillSurfaceWithBitmap(canvasBitmap As CanvasBitmap)
        CanvasComposition.Resize(_surface, canvasBitmap.Size)

        Using session = CanvasComposition.CreateDrawingSession(_surface)
            session.Clear(Colors.Transparent)
            session.DrawImage(canvasBitmap)
        End Using
    End Sub

    Private Sub ResetFramePool(size As SizeInt32, recreateDevice As Boolean)
        Do
            Try
                If recreateDevice Then
                    _canvasDevice = New CanvasDevice()
                End If
                FramePool.Recreate(_canvasDevice, DirectXPixelFormat.B8G8R8A8UIntNormalized, 2, size)
                ' This is the device-lost convention for Win2D.
            Catch e As Exception When _canvasDevice.IsDeviceLost(e.HResult)
                _canvasDevice = Nothing
                recreateDevice = True
            End Try
        Loop While _canvasDevice Is Nothing
    End Sub

    Private Async Sub Button_ClickAsync(sender As Object, e As RoutedEventArgs) Handles CaptureButton.Click
        Await StartCaptureAsync()
    End Sub

End Class
```

## <a name="record-a-video"></a>Grabar un vídeo

Si desea grabar un vídeo de la aplicación, puede hacerlo más fácilmente con el [espacio de nombres Windows. Media. AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording). Esto forma parte del SDK de la extensión de escritorio, por lo que solo funciona en el escritorio y requiere que se agregue una referencia a él desde el proyecto. Consulte información general de las [familias de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) para obtener más información.

## <a name="see-also"></a>Vea también

* [Espacio de nombres Windows. Graphics. Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture)
