---
title: Uso de la capa Visual con WPF
description: Aprenda técnicas para usar la API de nivel Visual en combinación con el contenido WPF existente para crear efectos y animaciones avanzadas.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a280b36b0ff3a34bfbd52417aec0856156aa420d
ms.sourcegitcommit: e63fbd7a63a7e8c03c52f4219f34513f4b2bb411
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58174184"
---
# <a name="using-the-visual-layer-with-wpf"></a>Uso de la capa Visual con WPF

Puede usar las API de composición de Windows en tiempo de ejecución (también denominado el [capa Visual](visual-layer.md)) en las aplicaciones de Windows Presentation Foundation (WPF) para crear experiencias modernas es clara para los usuarios de Windows 10.

El código completo de este tutorial está disponible en GitHub: [Ejemplo WPF HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition).

## <a name="prerequisites"></a>Requisitos previos

El XAML UWP API de hospedaje tiene estos requisitos previos.

- Se supone que tiene cierta familiaridad con el desarrollo de aplicaciones con WPF y UWP. Para obtener más información, consulta:
  - [Introducción (WPF)](/dotnet/framework/wpf/getting-started/)
  - [Empezar a trabajar con aplicaciones de Windows 10](/windows/uwp/get-started/)
  - [Mejore su aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET framework 4.7.2 o posterior
- Windows 10, versión 1803 o posterior
- SDK de Windows 10 17134 o posterior

## <a name="how-to-use-composition-apis-in-wpf"></a>Cómo usar las API de composición en WPF

En este tutorial, creará una aplicación WPF sencilla interfaz de usuario y agregarle elementos animados de composición. Componentes WPF y composición se mantienen simple, pero se muestra el código de interoperabilidad es el mismo independientemente de la complejidad de los componentes. La aplicación finalizada es similar al siguiente.

![La interfaz de usuario de aplicación en ejecución](images/interop/wpf-comp-interop-app-ui.png)

## <a name="create-a-wpf-project"></a>Crear un proyecto de WPF

El primer paso es crear el proyecto de aplicación WPF, que incluye una definición de aplicación y la página XAML para la interfaz de usuario.

Para crear un nuevo proyecto de aplicación de WPF en Visual C# denominado _HelloComposition_:

1. Abra Visual Studio y seleccione **archivo** > **New** > **proyecto**.

    El **nuevo proyecto** abre el cuadro de diálogo.
1. En el **instalado** categoría, expanda el **Visual C#**  nodo y, a continuación, seleccione **Windows Desktop**.
1. Seleccione el **aplicación de WPF (.NET Framework)** plantilla.
1. Escriba el nombre _HelloComposition_, seleccione el marco de trabajo **.NET Framework 4.7.2**, a continuación, haga clic en **Aceptar**.

    Visual Studio crea el proyecto y abre el Diseñador de la ventana de aplicación predeterminada denominado MainWindow.xaml.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurar el proyecto para usar Windows Runtime APIs

Para usar Windows en tiempo de ejecución (WinRT) API en la aplicación WPF, deberá configurar el proyecto de Visual Studio para tener acceso el tiempo de ejecución de Windows. Además, vectores se usan ampliamente por las API de composición, por lo que deberá agregar las referencias necesarias para usar vectores.

Paquetes de NuGet están disponibles para abordar estas necesidades de ambos. Instale las últimas versiones de estos paquetes para agregar las referencias necesarias al proyecto.  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (requiere el conjunto de formato de administración de paquete predeterminado a PackageReference).
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> Aunque se recomienda usar los paquetes de NuGet para configurar el proyecto, puede agregar manualmente las referencias necesarias. Para obtener más información, consulte [mejorar su aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance). La siguiente tabla muestra los archivos que necesita para agregar referencias a.

|Archivo|Ubicación|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program archivos (x86) \Windows Kits\10\References\<*sdk versión*> \Windows.Foundation.UniversalApiContract\<*versión*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program archivos (x86) \Windows Kits\10\References\<*sdk versión*> \Windows.Foundation.FoundationContract\<*versión*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Program archivos (x86) Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="configure-the-project-to-be-per-monitor-dpi-aware"></a>Configurar el proyecto para ser monitor reconocimiento de PPP

El contenido de la capa visual que agregue a la aplicación no se escala automáticamente para que coincida con la configuración de PPP de la pantalla se muestran en. Deberá habilitar el reconocimiento de PPP por monitor para la aplicación y, a continuación, asegúrese de que el código que utilice para crear el contenido de la capa visual tiene en cuenta la escala de PPP actual cuando se ejecuta la aplicación. Aquí se configura el proyecto para que sea el reconocimiento de PPP. En secciones posteriores, se muestra cómo usar la información de PPP para escalar el contenido de la capa visual.

Las aplicaciones de WPF son sistema PPP de forma predeterminada, pero es necesario declarar a sí mismos para ser monitor reconocimiento de PPP en un archivo app.manifest. Para activar el reconocimiento de PPP por monitor de nivel de Windows en el archivo de manifiesto de aplicación:

1. En **el Explorador de soluciones**, haga clic en el _HelloComposition_ proyecto.
1. En el menú contextual, seleccione **agregar** > **nuevo elemento...** .
1. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione "Archivo de manifiesto de aplicación", a continuación, haga clic en **agregar**. (Puede dejar el nombre predeterminado).
1. En el archivo app.manifest, busque este xml y quitar los comentarios:

    ```xaml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
        </windowsSettings>
      </application>
    ```

1. Agregar esta configuración después de la apertura `<windowsSettings>` etiqueta:

    ```xaml
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitor</dpiAwareness>
    ```

1. También deberá establecer el **DoNotScaleForDpiChanges** en el archivo App.config.

    Abra App.Config y agregue este código xml dentro de la `<configuration>` elemento:

    ```xml
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Windows.DoNotScaleForDpiChanges=false"/>
    </runtime>
    ```

> [!NOTE]
> **AppContextSwitchOverrides** solo puede establecerse una vez. Si la aplicación ya tiene un conjunto, debe punto y coma delimitar este modificador dentro del atributo de valor.

(Para obtener más información, consulte el [ejemplos y Guía de desarrollador de PPP por Monitor](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI) en GitHub.)

## <a name="create-an-hwndhost-derived-class-to-host-composition-elements"></a>Cree una clase derivada de HwndHost a elementos de la composición de host

Para hospedar contenido que cree con la capa visual, deberá crear una clase que derive de [HwndHost](/dotnet/api/system.windows.interop.hwndhost). Esto es donde realiza la mayoría de la configuración para hospedar las API de composición. En esta clase, se usa [servicios de invocación de plataforma (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) y [interoperabilidad COM](/dotnet/api/system.runtime.interopservices.comimportattribute) para poner las API de composición en su aplicación WPF. Para obtener más información sobre PInvoke y la interoperabilidad COM, vea [interoperar con código no administrado](/dotnet/framework/interop/index).

> [!TIP]
> Si necesita, compruebe el código completo al final del tutorial para asegurarse de que todo el código está en los lugares correctos cuando se trabaja con el tutorial.

1. Agregue un nuevo archivo de clase al proyecto que se derive de [HwndHost](/dotnet/api/system.windows.interop.hwndhost).
    - En **el Explorador de soluciones**, haga clic en el _HelloComposition_ proyecto.
    - En el menú contextual, seleccione **agregar** > **clase...** .
    - En el **Agregar nuevo elemento** cuadro de diálogo, nombre de la clase _CompositionHost.cs_, a continuación, haga clic en **agregar**.
1. En CompositionHost.cs, edite la definición de clase que derive de **HwndHost**.

    ```csharp
    // Add
    // using System.Windows.Interop;

    namespace HelloComposition
    {
        class CompositionHost : HwndHost
        {
        }
    }
    ```

1. Agregue el código y el constructor siguiente a la clase.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    int hostHeight, hostWidth;
    object dispatcherQueue;
    ICompositionTarget compositionTarget;

    public Compositor Compositor { get; private set; }

    public Visual Child
    {
        set
        {
            if (Compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }

    internal const int
      WS_CHILD = 0x40000000,
      WS_VISIBLE = 0x10000000,
      LBS_NOTIFY = 0x00000001,
      HOST_ID = 0x00000002,
      LISTBOX_ID = 0x00000001,
      WS_VSCROLL = 0x00200000,
      WS_BORDER = 0x00800000;

    public CompositionHost(double height, double width)
    {
        hostHeight = (int)height;
        hostWidth = (int)width;
    }
    ```

1. Invalidar el **BuildWindowCore** y **DestroyWindowCore** métodos.
    > [!NOTE]
    > En BuildWindowCore, llame a la _InitializeCoreDispatcher_ y _InitComposition_ métodos. Puede crear estos métodos en los pasos siguientes.

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

    protected override HandleRef BuildWindowCore(HandleRef hwndParent)
    {
        // Create Window
        hwndHost = IntPtr.Zero;
        hwndHost = CreateWindowEx(0, "static", "",
                                  WS_CHILD | WS_VISIBLE,
                                  0, 0,
                                  hostWidth, hostHeight,
                                  hwndParent.Handle,
                                  (IntPtr)HOST_ID,
                                  IntPtr.Zero,
                                  0);

        // Create Dispatcher Queue
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content
        InitComposition(hwndHost);

        return new HandleRef(this, hwndHost);
    }

    protected override void DestroyWindowCore(HandleRef hwnd)
    {
        if (compositionTarget.Root != null)
        {
            compositionTarget.Root.Dispose();
        }
        DestroyWindow(hwnd.Handle);
    }
    ```

    - [CreateWindowEx](/windows/desktop/api/winuser/nf-winuser-createwindowexa) y [DestroyWindow](/windows/desktop/api/winuser/nf-winuser-destroywindow) requieren una declaración PInvoke. Esta declaración se coloca al final del código de la clase.

    ```csharp
    #region PInvoke declarations

    [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                  string lpszClassName,
                                                  string lpszWindowName,
                                                  int style,
                                                  int x, int y,
                                                  int width, int height,
                                                  IntPtr hwndParent,
                                                  IntPtr hMenu,
                                                  IntPtr hInst,
                                                  [MarshalAs(UnmanagedType.AsAny)] object pvParam);

    [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
    internal static extern bool DestroyWindow(IntPtr hwnd);

    #endregion PInvoke declarations
    ```

1. Inicializar un subproceso con un [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). El distribuidor core es responsable de procesar los mensajes de ventana y distribución de eventos para WinRT APIs. Las instancias nuevas de **CoreDispatcher** debe crearse en un subproceso que tiene un CoreDispatcher.
    - Cree un método denominado _InitializeCoreDispatcher_ y agregue código para configurar la cola del distribuidor.

    ```csharp
    private object InitializeCoreDispatcher()
    {
        DispatcherQueueOptions options = new DispatcherQueueOptions();
        options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
        options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
        options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

        object queue = null;
        CreateDispatcherQueueController(options, out queue);
        return queue;
    }
    ```

    - La cola del distribuidor también requiere una declaración PInvoke. Colocar esta declaración dentro de la _declaraciones PInvoke_ región que creó en el paso anterior.

    ```csharp
    //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    //{
    //    DQTAT_COM_NONE,
    //    DQTAT_COM_ASTA,
    //    DQTAT_COM_STA
    //};
    internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    {
        DQTAT_COM_NONE = 0,
        DQTAT_COM_ASTA = 1,
        DQTAT_COM_STA = 2
    };

    //typedef enum DISPATCHERQUEUE_THREAD_TYPE
    //{
    //    DQTYPE_THREAD_DEDICATED,
    //    DQTYPE_THREAD_CURRENT
    //};
    internal enum DISPATCHERQUEUE_THREAD_TYPE
    {
        DQTYPE_THREAD_DEDICATED = 1,
        DQTYPE_THREAD_CURRENT = 2,
    };

    //struct DispatcherQueueOptions
    //{
    //    DWORD dwSize;
    //    DISPATCHERQUEUE_THREAD_TYPE threadType;
    //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    //};
    [StructLayout(LayoutKind.Sequential)]
    internal struct DispatcherQueueOptions
    {
        public int dwSize;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_TYPE threadType;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    };

    //HRESULT CreateDispatcherQueueController(
    //  DispatcherQueueOptions options,
    //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
    //);
    [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                            [MarshalAs(UnmanagedType.IUnknown)]
                                            out object dispatcherQueueController);
    ```

    Ahora tiene preparada la cola del distribuidor y empezar a inicializar y crear contenido composición.

1. Inicializar el [Compositor](/uwp/api/windows.ui.composition.compositor). El Compositor es una fábrica que crea una variedad de tipos en el [Windows.UI.Composition](/uwp/api/windows.ui.composition) espacio de nombres que abarca los objetos visuales, el sistema de efectos y el sistema de animación. La clase Compositor también administra la duración de objetos creado a partir de la fábrica.

    ```csharp
    private void InitComposition(IntPtr hwndHost)
    {
        ICompositorDesktopInterop interop;

        compositor = new Compositor();
        object iunknown = compositor as object;
        interop = (ICompositorDesktopInterop)iunknown;
        IntPtr raw;
        interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

        object rawObject = Marshal.GetObjectForIUnknown(raw);
        ICompositionTarget target = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }
    }
    ```

    - **ICompositorDesktopInterop** y **ICompositionTarget** requieren COM importaciones. Coloque el siguiente código después la _CompositionHost_ clase, pero dentro la declaración de espacio de nombres.

    ```csharp
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
    ```

## <a name="create-a-usercontrol-to-add-your-content-to-the-wpf-visual-tree"></a>Crear un UserControl para agregar el contenido en el árbol visual de WPF

El último paso para configurar la infraestructura necesaria para el host de composición contenido consiste en Agregar HwndHost al árbol visual de WPF.

### <a name="create-a-usercontrol"></a>Crear un UserControl

Un control de usuario es una manera cómoda para empaquetar el código que crea y administra el contenido de la composición y agregar fácilmente el contenido en el XAML.

1. Agregue un nuevo archivo de control de usuario al proyecto.
    - En **el Explorador de soluciones**, haga clic en el _HelloComposition_ proyecto.
    - En el menú contextual, seleccione **agregar** > **Control de usuario...** .
    - En el **Agregar nuevo elemento** cuadro de diálogo, nombre el control de usuario _CompositionHostControl.xaml_, a continuación, haga clic en **agregar**.

    Los CompositionHostControl.xaml CompositionHostControl.xaml.cs archivos y se crean y se agrega al proyecto.
1. En CompositionHostControl.xaml, reemplace el `<Grid> </Grid>` etiquetas con este **borde** elemento, que es el contenedor XAML que se ubicarán los HwndHost en.

    ```xaml
    <Border Name="CompositionHostElement"/>
    ```

En el código para el control de usuario, creará una instancia de la clase CompositionHost que creó en el paso anterior y agregarlo como un elemento secundario de _CompositionHostElement_, el borde que creó en la página XAML.

1. En CompositionHostControl.xaml.cs, agregue variables privadas para los objetos que se usará en el código de composición. Agregue estas después de la definición de clase.

    ```csharp
    CompositionHost compositionHost;
    Compositor compositor;
    Windows.UI.Composition.ContainerVisual containerVisual;
    DpiScale currentDpi;
    ```

1. Agregar un controlador para el control de usuario **Loaded** eventos. Esto es donde se configura la instancia de CompositionHost.

    - En el constructor, enlazar el controlador de eventos como se muestra aquí (`Loaded += CompositionHostControl_Loaded;`).

    ```csharp
    public CompositionHostControl()
    {
        InitializeComponent();
        Loaded += CompositionHostControl_Loaded;
    }
    ```

    - Agregue el método de controlador de eventos con el nombre *CompositionHostControl_Loaded*.
    ```csharp
    private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
    {
        // If the user changes the DPI scale setting for the screen the app is on,
        // the CompositionHostControl is reloaded. Don't redo this set up if it's
        // already been done.
        if (compositionHost is null)
        {
            currentDpi = VisualTreeHelper.GetDpi(this);

            compositionHost =
                new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
            ControlHostElement.Child = compositionHost;
            compositor = compositionHost.Compositor;
            containerVisual = compositor.CreateContainerVisual();
            compositionHost.Child = containerVisual;
        }
    }
    ```

    En este método, configure los objetos que se usará en el código de composición. Le presentamos una visión rápida de lo que sucede.

    - En primer lugar, asegúrese de que el conjunto de copia solo se realiza una vez al comprobar si ya existe una instancia de CompositionHost.

    ```csharp
    // If the user changes the DPI scale setting for the screen the app is on,
    // the CompositionHostControl is reloaded. Don't redo this set up if it's
    // already been done.
    if (compositionHost is null)
    {

    }
    ```

    - Obtiene el valor de PPP actual. Esto se usa para escalar correctamente los elementos de la composición.

    ```csharp
    currentDpi = VisualTreeHelper.GetDpi(this);
    ```

    - Cree una instancia de CompositionHost y asignarlo como elemento secundario del borde, _CompositionHostElement_.

    ```csharp
    compositionHost =
        new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
    ControlHostElement.Child = compositionHost;
    ```

    - Obtenga al Compositor de la CompositionHost.

    ```csharp
    compositor = compositionHost.Compositor;
    ```

    - Para crear un contenedor visual, utilice al Compositor. Este es el contenedor de composición que se agregan los elementos de la composición.

    ```csharp
    containerVisual = compositor.CreateContainerVisual();
    compositionHost.Child = containerVisual;
    ```

### <a name="add-composition-elements"></a>Agregar elementos de composición

Con la infraestructura en su lugar, ahora puede generar el contenido de composición que desea mostrar.

En este ejemplo, agrega código que crea y se anima un cuadrado simple [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual).

1. Agregue un elemento de composición. En CompositionHostControl.xaml.cs, agregue estos métodos a la clase CompositionHostControl.

    ```csharp
    // Add
    // using System.Numerics;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size);
        visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

        // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
        // with the bottom of the host container. This is the value to animate to.
        var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
        var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
        float bottom = (float)(hostHeightAdj - squareSizeAdj);

        // Create the animation only if it's needed.
        if (visual.Offset.Y != bottom)
        {
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
        }
    }

    private Windows.UI.Color GetRandomColor()
    {
        Random random = new Random();
        byte r = (byte)random.Next(0, 255);
        byte g = (byte)random.Next(0, 255);
        byte b = (byte)random.Next(0, 255);
        return Windows.UI.Color.FromArgb(255, r, g, b);
    }
    ```

### <a name="handle-dpi-changes"></a>Controlar los cambios de PPP

El código para agregar y animar un elemento tiene en cuenta la escala de PPP actual cuando se crean elementos, pero también debe tener en cuenta los cambios de PPP mientras se ejecuta la aplicación. Puede controlar la [HwndHost.DpiChanged](/dotnet/api/system.windows.interop.hwndhost.dpichanged) basada en eventos para recibir notificaciones de cambios y ajustar los cálculos en el nuevo valor de PPP.

1. En el método CompositionHostControl_Loaded, después de la última línea, agregue esta opción para enlazar el controlador de evento DpiChanged.

    ```csharp
    compositionHost.DpiChanged += CompositionHost_DpiChanged;
    ```

1. Agregue el método de controlador de eventos con el nombre _CompositionHostDpiChanged_. Este código ajusta la escala y el desplazamiento de cada elemento y se vuelve a calcular las animaciones que no se completen.

    ```csharp
    private void CompositionHost_DpiChanged(object sender, DpiChangedEventArgs e)
    {
        currentDpi = e.NewDpi;
        Vector3 newScale = new Vector3((float)e.NewDpi.DpiScaleX, (float)e.NewDpi.DpiScaleY, 1);

        foreach (SpriteVisual child in containerVisual.Children)
        {
            child.Scale = newScale;
            var newOffsetX = child.Offset.X * ((float)e.NewDpi.DpiScaleX / (float)e.OldDpi.DpiScaleX);
            var newOffsetY = child.Offset.Y * ((float)e.NewDpi.DpiScaleY / (float)e.OldDpi.DpiScaleY);
            child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

            // Adjust animations for DPI change.
            AnimateSquare(child, 0);
        }
    }
    ```

## <a name="add-the-user-control-to-your-xaml-page"></a>Agregar el control de usuario a la página XAML

Ahora, puede agregar el control de usuario a la UI de XAML.

1. En MainWindow.xaml, establezca el alto de la ventana en 600 y el ancho de 840.
1. Agregue el XAML para la interfaz de usuario. En MainWindow.xaml, agregue este XAML entre la raíz `<Grid> </Grid>` etiquetas.

    ```xaml
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="210"/>
        <ColumnDefinition Width="600"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="46"/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Button Content="Add composition element" Click="Button_Click"
            Grid.Row="1" Margin="12,0"
            VerticalAlignment="Top" Height="40"/>
    <TextBlock Text="Composition content" FontSize="20"
               Grid.Column="1" Margin="0,12,0,4"
               HorizontalAlignment="Center"/>
    <local:CompositionHostControl x:Name="CompositionHostControl1"
                                  Grid.Row="1" Grid.Column="1"
                                  VerticalAlignment="Top"
                                  Width="600" Height="500"
                                  BorderBrush="LightGray"
                                  BorderThickness="3"/>
    ```

1. Controle el clic de botón para crear nuevos elementos. (El evento Click ya está enlazado en el XAML.)

    En MainWindow.xaml.cs, agregue esto *Button_Click* método controlador de eventos. Este código llama a _CompositionHost.AddElement_ para crear un nuevo elemento con un tamaño generado al azar y desplazamiento.

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
        float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
        CompositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

Ahora puede compilar y ejecutar la aplicación WPF. Si necesita, compruebe el código completo al final del tutorial para asegurarse de que todo el código está en los lugares correctos.

Cuando ejecute la aplicación y haga clic en el botón, debería ver animados cuadrados agregados a la interfaz de usuario.

## <a name="next-steps"></a>Pasos siguientes

Para obtener un ejemplo más completo que se basa en la misma infraestructura, consulte el [ejemplo de integración de WPF Visual layer](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration) en GitHub.

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción (WPF)](/dotnet/framework/wpf/getting-started/) (. NET)
- [Interoperar con código no administrado](/dotnet/framework/interop/) (. NET)
- [Empezar a trabajar con aplicaciones de Windows 10](/windows/uwp/get-started/) (UWP)
- [Mejore su aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Espacio de nombres Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Código completo

Este es el código completo para este tutorial.

### <a name="mainwindowxaml"></a>MainWindow.xaml

```xaml
<Window x:Class="HelloComposition.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloComposition"
        mc:Ignorable="d"
        Title="MainWindow" Height="600" Width="840">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="210"/>
            <ColumnDefinition Width="600"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="46"/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Button Content="Add composition element" Click="Button_Click"
                Grid.Row="1" Margin="12,0"
                VerticalAlignment="Top" Height="40"/>
        <TextBlock Text="Composition content" FontSize="20"
                   Grid.Column="1" Margin="0,12,0,4"
                   HorizontalAlignment="Center"/>
        <local:CompositionHostControl x:Name="CompositionHostControl1"
                                      Grid.Row="1" Grid.Column="1"
                                      VerticalAlignment="Top"
                                      Width="600" Height="500"
                                      BorderBrush="LightGray" BorderThickness="3"/>
    </Grid>
</Window>
```

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

```csharp
using System;
using System.Windows;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
            float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
            CompositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolxaml"></a>CompositionHostControl.xaml

```xaml
<UserControl x:Class="HelloComposition.CompositionHostControl"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
             xmlns:local="clr-namespace:HelloComposition"
             mc:Ignorable="d"
             d:DesignHeight="450" d:DesignWidth="800">
    <Border Name="CompositionHostElement"/>
</UserControl>
```

### <a name="compositionhostcontrolxamlcs"></a>CompositionHostControl.xaml.cs

```csharp
using System;
using System.Numerics;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;
using Windows.UI.Composition;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for CompositionHostControl.xaml
    /// </summary>
    public partial class CompositionHostControl : UserControl
    {
        CompositionHost compositionHost;
        Compositor compositor;
        Windows.UI.Composition.ContainerVisual containerVisual;
        DpiScale currentDpi;

        public CompositionHostControl()
        {
            InitializeComponent();
            Loaded += CompositionHostControl_Loaded;
        }

        private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
        {
            // If the user changes the DPI scale setting for the screen the app is on,
            // the CompositionHostControl is reloaded. Don't redo this set up if it's
            // already been done.
            if (compositionHost is null)
            {
                currentDpi = VisualTreeHelper.GetDpi(this);

                compositionHost = new CompositionHost(CompositionHostElement.ActualHeight, CompositionHostElement.ActualWidth);
                CompositionHostElement.Child = compositionHost;
                compositor = compositionHost.Compositor;
                containerVisual = compositor.CreateContainerVisual();
                compositionHost.Child = containerVisual;
            }
        }

        protected override void OnDpiChanged(DpiScale oldDpi, DpiScale newDpi)
        {
            base.OnDpiChanged(oldDpi, newDpi);
            currentDpi = newDpi;
            Vector3 newScale = new Vector3((float)newDpi.DpiScaleX, (float)newDpi.DpiScaleY, 1);

            foreach (SpriteVisual child in containerVisual.Children)
            {
                child.Scale = newScale;
                var newOffsetX = child.Offset.X * ((float)newDpi.DpiScaleX / (float)oldDpi.DpiScaleX);
                var newOffsetY = child.Offset.Y * ((float)newDpi.DpiScaleY / (float)oldDpi.DpiScaleY);
                child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

                // Adjust animations for DPI change.
                AnimateSquare(child, 0);
            }
        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size);
            visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

            // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
            // with the bottom of the host container. This is the value to animate to.
            var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
            var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
            float bottom = (float)(hostHeightAdj - squareSizeAdj);

            // Create the animation only if it's needed.
            if (visual.Offset.Y != bottom)
            {
                Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
                animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
                animation.Duration = TimeSpan.FromSeconds(2);
                animation.DelayTime = TimeSpan.FromSeconds(delay);
                visual.StartAnimation("Offset", animation);
            }
        }

        private Windows.UI.Color GetRandomColor()
        {
            Random random = new Random();
            byte r = (byte)random.Next(0, 255);
            byte g = (byte)random.Next(0, 255);
            byte b = (byte)random.Next(0, 255);
            return Windows.UI.Color.FromArgb(255, r, g, b);
        }
    }
}

```

### <a name="compositionhostcs"></a>CompositionHost.cs

```csharp
using System;
using System.Runtime.InteropServices;
using System.Windows.Interop;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHost : HwndHost
    {
        IntPtr hwndHost;
        int hostHeight, hostWidth;
        object dispatcherQueue;
        ICompositionTarget compositionTarget;

        public Compositor Compositor { get; private set; }

        public Visual Child
        {
            set
            {
                if (Compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        internal const int
          WS_CHILD = 0x40000000,
          WS_VISIBLE = 0x10000000,
          LBS_NOTIFY = 0x00000001,
          HOST_ID = 0x00000002,
          LISTBOX_ID = 0x00000001,
          WS_VSCROLL = 0x00200000,
          WS_BORDER = 0x00800000;

        public CompositionHost(double height, double width)
        {
            hostHeight = (int)height;
            hostWidth = (int)width;
        }

        protected override HandleRef BuildWindowCore(HandleRef hwndParent)
        {
            // Create Window
            hwndHost = IntPtr.Zero;
            hwndHost = CreateWindowEx(0, "static", "",
                                      WS_CHILD | WS_VISIBLE,
                                      0, 0,
                                      hostWidth, hostHeight,
                                      hwndParent.Handle,
                                      (IntPtr)HOST_ID,
                                      IntPtr.Zero,
                                      0);

            // Create Dispatcher Queue
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition Tree of content
            InitComposition(hwndHost);

            return new HandleRef(this, hwndHost);
        }

        protected override void DestroyWindowCore(HandleRef hwnd)
        {
            if (compositionTarget.Root != null)
            {
                compositionTarget.Root.Dispose();
            }
            DestroyWindow(hwnd.Handle);
        }

        private object InitializeCoreDispatcher()
        {
            DispatcherQueueOptions options = new DispatcherQueueOptions();
            options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
            options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
            options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

            object queue = null;
            CreateDispatcherQueueController(options, out queue);
            return queue;
        }

        private void InitComposition(IntPtr hwndHost)
        {
            ICompositorDesktopInterop interop;

            Compositor = new Compositor();
            object iunknown = Compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }
        }

        #region PInvoke declarations

        //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        //{
        //    DQTAT_COM_NONE,
        //    DQTAT_COM_ASTA,
        //    DQTAT_COM_STA
        //};
        internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        {
            DQTAT_COM_NONE = 0,
            DQTAT_COM_ASTA = 1,
            DQTAT_COM_STA = 2
        };

        //typedef enum DISPATCHERQUEUE_THREAD_TYPE
        //{
        //    DQTYPE_THREAD_DEDICATED,
        //    DQTYPE_THREAD_CURRENT
        //};
        internal enum DISPATCHERQUEUE_THREAD_TYPE
        {
            DQTYPE_THREAD_DEDICATED = 1,
            DQTYPE_THREAD_CURRENT = 2,
        };

        //struct DispatcherQueueOptions
        //{
        //    DWORD dwSize;
        //    DISPATCHERQUEUE_THREAD_TYPE threadType;
        //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        //};
        [StructLayout(LayoutKind.Sequential)]
        internal struct DispatcherQueueOptions
        {
            public int dwSize;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_TYPE threadType;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        };

        //HRESULT CreateDispatcherQueueController(
        //  DispatcherQueueOptions options,
        //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
        //);
        [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                                [MarshalAs(UnmanagedType.IUnknown)]
                                               out object dispatcherQueueController);


        [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                      string lpszClassName,
                                                      string lpszWindowName,
                                                      int style,
                                                      int x, int y,
                                                      int width, int height,
                                                      IntPtr hwndParent,
                                                      IntPtr hMenu,
                                                      IntPtr hInst,
                                                      [MarshalAs(UnmanagedType.AsAny)] object pvParam);

        [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
        internal static extern bool DestroyWindow(IntPtr hwnd);


        #endregion PInvoke declarations

    }
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
}
```