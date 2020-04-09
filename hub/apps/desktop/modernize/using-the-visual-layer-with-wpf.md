---
title: Usar la capa visual con WPF
description: Obtén información sobre las técnicas de uso de las API de la capa visual en combinación con contenido de WPF ya existente para crear animaciones y efectos avanzados.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: a2f30ba67acc12d622acd09f9fae872ee2058a2f
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215152"
---
# <a name="using-the-visual-layer-with-wpf"></a>Usar la capa visual con WPF

Puedes usar las API de Composition de Windows Runtime (también llamada la [capa visual](/windows/uwp/composition/visual-layer)) en las aplicaciones de Windows Presentation Foundation (WPF) para crear experiencias modernas que atraigan a los usuarios de Windows 10.

El código completo de este tutorial está disponible en GitHub: [Ejemplo HelloComposition de WPF](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition).

## <a name="prerequisites"></a>Requisitos previos

La API de hospedaje XAML de UWP tiene estos requisitos previos.

- Damos por hecho que estás familiarizado con el desarrollo de aplicaciones con WPF y UWP. Para obtener más información, consulta:
  - [Introducción (WPF)](/dotnet/framework/wpf/getting-started/)
  - [Introducción a las aplicaciones de Windows 10](/windows/uwp/get-started/)
  - [Mejorar tu aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 o una versión posterior
- Windows 10 versión 1803 o posterior
- SDK de Windows 10 17134 o posterior

## <a name="how-to-use-composition-apis-in-wpf"></a>Procedimiento para usar las API de Composition en WPF

En este tutorial, crearás una interfaz de usuario de aplicación de WPF simple y le agregarás elementos de Composition animados. Tanto WPF como los componentes de Composition son sencillos, pero el código de interoperabilidad mostrado es el mismo, independientemente de la complejidad de los componentes. La aplicación finalizada tiene el siguiente aspecto.

![Interfaz de usuario de la aplicación en ejecución](images/visual-layer-interop/wpf-comp-interop-app-ui.png)

## <a name="create-a-wpf-project"></a>Crear un proyecto de WPF

El primer paso es crear el proyecto de aplicación de WPF, que incluye una definición de aplicación y la página XAML para la interfaz de usuario.

Para crear un nuevo proyecto de aplicación de WPF en Visual C# llamado _HelloComposition_:

1. Abre Visual Studio y selecciona **Archivo** > **Nuevo** > **Proyecto**.

    Se abre el cuadro de diálogo **Nuevo proyecto**.
1. En la categoría **Instalados**, expande el nodo **Visual C#** y, después, selecciona **Escritorio de Windows**.
1. Selecciona la plantilla **Aplicación de WPF (.NET Framework)** .
1. Escribe el nombre _HelloComposition_, selecciona Framework **.NET Framework 4.7.2** y, después, haz clic en **Aceptar**.

    Visual Studio crea el proyecto y abre el diseñador de la ventana predeterminada de la aplicación llamada MainWindow.xaml.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurar el proyecto para usar las API de Windows Runtime

Para usar las API de Windows Runtime (WinRT) en la aplicación de WPF, tienes que configurar el proyecto de Visual Studio para que tenga acceso a Windows Runtime. Además, las API de Composition usan mucho los vectores, por lo que tienes que agregar las referencias necesarias para usar vectores.

Hay paquetes NuGet disponibles para abordar estas dos necesidades. Instala las versiones más recientes de estos paquetes para agregar las referencias necesarias al proyecto.  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (requiere el formato de administración de paquetes predeterminado establecido en PackageReference).
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> Aunque se recomienda usar paquetes NuGet para configurar el proyecto, puedes agregar manualmente las referencias necesarias. Para más información, consulta [Mejorar tu aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance). En la tabla siguiente se muestran los archivos a los que hay que agregar referencias.

|Archivo|Ubicación|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Archivos de programa (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Archivos de programa (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Archivos de programa (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="configure-the-project-to-be-per-monitor-dpi-aware"></a>Configuración del proyecto para que reconozca el valor de ppp de cada monitor

El contenido de la capa visual que agregues a la aplicación no se escala automáticamente para que coincida con la configuración de ppp de la pantalla en la que se muestra. Tienes que habilitar en la aplicación el reconocimiento de ppp en cada monitor y, a continuación, asegurarte de que el código que usas para crear el contenido de la capa visual tenga en cuenta la escala de ppp actual cuando se ejecute la aplicación. Aquí, configuramos el proyecto para que reconozca el valor de ppp. En las secciones siguientes, se muestra cómo usar la información de ppp para escalar el contenido de la capa visual.

Las aplicaciones de WPF reconocen el valor de ppp del sistema de forma predeterminada, pero deben declararse en el archivo app.manifest para que reconozcan el valor de ppp en cada monitor. Para activar en el archivo de manifiesto de la aplicación el reconocimiento de ppp en cada monitor en el nivel de Windows:

1. En el **Explorador de soluciones**, haz clic con el botón derecho en el proyecto _HelloComposition_.
1. En el menú contextual, selecciona **Agregar** > **Nuevo elemento...** .
1. En el cuadro de diálogo **Agregar nuevo elemento**, selecciona "Archivo de manifiesto de aplicación" y, a continuación, haz clic en **Agregar**. (Puedes dejar el nombre predeterminado).
1. En el archivo app.manifest, busca este código XML y elimina el comentario:

    ```xaml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
        </windowsSettings>
      </application>
    ```

1. Agrega esta configuración después de la etiqueta `<windowsSettings>` de apertura:

    ```xaml
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitor</dpiAwareness>
    ```

1. También tienes que establecer el valor de **DoNotScaleForDpiChanges** en el archivo app.config.

    Abre App.Config y agrega este código XML dentro del elemento `<configuration>`:

    ```xml
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Windows.DoNotScaleForDpiChanges=false"/>
    </runtime>
    ```

> [!NOTE]
> **AppContextSwitchOverrides** solo se puede establecer una vez. Si la aplicación ya tiene un conjunto, debes delimitar este modificador dentro del atributo value.

(Para más información, consulta la [guía para desarrolladores y los ejemplos de ppp por monitor](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI) en GitHub).

## <a name="create-an-hwndhost-derived-class-to-host-composition-elements"></a>Crear una clase derivada de HwndHost para hospedar los elementos de Composition

Para hospedar el contenido que crees con la capa visual, debes crear una clase que derive de [HwndHost](/dotnet/api/system.windows.interop.hwndhost). Aquí es donde se realiza la mayoría de la configuración para hospedar las API de Composition. En esta clase, se usan los [servicios de invocación de plataforma (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) y la [interoperabilidad COM](/dotnet/api/system.runtime.interopservices.comimportattribute) para incorporar las API de Composition a la aplicación de WPF. Para más información sobre PInvoke y la interoperabilidad COM, consulta [Interoperar con código no administrado](/dotnet/framework/interop/index).

> [!TIP]
> Si es necesario, comprueba el código completo al final del tutorial para asegurarte de que todo el código está en el lugar correcto mientras trabajas en el tutorial.

1. Agrega al proyecto un nuevo archivo de clase que derive de [HwndHost](/dotnet/api/system.windows.interop.hwndhost).
    - En el **Explorador de soluciones**, haz clic con el botón derecho en el proyecto _HelloComposition_.
    - En el menú contextual, selecciona **Agregar** > **Clase...** .
    - En el cuadro de diálogo **Agregar nuevo elemento**, asigna a la clase el nombre _CompositionHost.cs_ y, después, haz clic en **Agregar**.
1. En CompositionHost.cs, edita la definición de clase para que derive de **HwndHost**.

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

1. Agrega el siguiente código y constructor a la clase.

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

1. Invalida los métodos **BuildWindowCore** y **DestroyWindowCore**.
    > [!NOTE]
    > En BuildWindowCore, se llama a los métodos _InitializeCoreDispatcher_ e _InitComposition_. Estos métodos se crean en los pasos siguientes.

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

    - [CreateWindowEx](/windows/desktop/api/winuser/nf-winuser-createwindowexa) y [DestroyWindow](/windows/desktop/api/winuser/nf-winuser-destroywindow) requieren una declaración PInvoke. Coloca esta declaración al final del código de la clase.

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

1. Inicializa un subproceso con [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). El distribuidor principal es responsable de procesar los mensajes de ventana y de distribuir los eventos de las API de WinRT. Se deben crear nuevas instancias de **CoreDispatcher** en un subproceso que tenga un objeto CoreDispatcher.
    - Crea un método llamado _InitializeCoreDispatcher_ y agrega código para configurar la cola del distribuidor.

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

    - La cola del distribuidor también requiere una declaración PInvoke. Coloca esta declaración dentro de la región de _declaraciones PInvoke_ que creaste en el paso anterior.

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

    Ahora tienes la cola del distribuidor lista y puedes empezar a inicializar y crear el contenido de Composition.

1. Inicializa la clase [Compositor](/uwp/api/windows.ui.composition.compositor). Compositor es una fábrica que crea diversos tipos en el espacio de nombres [Windows.UI.Composition](/uwp/api/windows.ui.composition), tales como la capa visual, el sistema de efectos y el sistema de animación. La clase Compositor también administra la duración de los objetos creados con la fábrica.

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

    - **ICompositorDesktopInterop** e **ICompositionTarget** requieren importaciones de COM. Coloca este código después de la clase _CompositionHost_, pero dentro de la declaración del espacio de nombres.

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

## <a name="create-a-usercontrol-to-add-your-content-to-the-wpf-visual-tree"></a>Crear un control UserControl para agregar el contenido al árbol visual de WPF

El último paso para configurar la infraestructura necesaria para hospedar el contenido de Composition es agregar HwndHost al árbol visual de WPF.

### <a name="create-a-usercontrol"></a>Crear un control UserControl

Un control UserControl es una manera cómoda de empaquetar el código que crea y administra el contenido de Composition, y agrega fácilmente el contenido al código XAML.

1. Agrega un nuevo control de usuario al proyecto.
    - En el **Explorador de soluciones**, haz clic con el botón derecho en el proyecto _HelloComposition_.
    - En el menú contextual, selecciona **Agregar** > **Control de usuario...** .
    - En el cuadro de diálogo **Agregar nuevo elemento**, asigna al control de usuario el nombre _CompositionHostControl.xaml_ y, después, haz clic en **Agregar**.

    Se crean los archivos CompositionHostControl. XAML y CompositionHostControl.xaml.cs y se agregan al proyecto.
1. En CompositionHostControl. XAML, reemplaza las etiquetas `<Grid> </Grid>` por este elemento **Border**, que es el contenedor XAML en el que irá HwndHost.

    ```xaml
    <Border Name="CompositionHostElement"/>
    ```

En el código del control de usuario, crea una instancia de la clase CompositionHost que creaste en el paso anterior y agrégala como elemento secundario de _CompositionHostElement_, el borde que creaste en la página XAML.

1. En CompositionHostControl.xaml.cs, agrega las variables privadas para los objetos que vas a usar en el código de Composition. Agrégalos después de la definición de clase.

    ```csharp
    CompositionHost compositionHost;
    Compositor compositor;
    Windows.UI.Composition.ContainerVisual containerVisual;
    DpiScale currentDpi;
    ```

1. Agrega un controlador para el evento **Loaded** del control de usuario. Aquí es donde se configura la instancia de CompositionHost.

    - En el constructor, enlaza el controlador de eventos como se muestra aquí (`Loaded += CompositionHostControl_Loaded;`).

    ```csharp
    public CompositionHostControl()
    {
        InitializeComponent();
        Loaded += CompositionHostControl_Loaded;
    }
    ```

    - Agrega el método de control de eventos con el nombre *CompositionHostControl_Loaded*.
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

    En este método, se configuran los objetos que se van a usar en el código de Composition. A continuación se muestra rápidamente lo que está ocurriendo.

    - En primer lugar, comprueba si ya existe una instancia de CompositionHost para asegurarte de que la configuración solo se realiza una vez.

    ```csharp
    // If the user changes the DPI scale setting for the screen the app is on,
    // the CompositionHostControl is reloaded. Don't redo this set up if it's
    // already been done.
    if (compositionHost is null)
    {

    }
    ```

    - Obtén el valor de ppp actual. Se usa para escalar correctamente los elementos de Composition.

    ```csharp
    currentDpi = VisualTreeHelper.GetDpi(this);
    ```

    - Crea una instancia de CompositionHost y asígnala como elemento secundario del borde _CompositionHostElement_.

    ```csharp
    compositionHost =
        new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
    ControlHostElement.Child = compositionHost;
    ```

    - Obtén el compositor desde CompositionHost.

    ```csharp
    compositor = compositionHost.Compositor;
    ```

    - Usa el compositor para crear un objeto visual contenedor. Este es el contenedor de Composition al que se agregan los elementos de Composition.

    ```csharp
    containerVisual = compositor.CreateContainerVisual();
    compositionHost.Child = containerVisual;
    ```

### <a name="add-composition-elements"></a>Agregar elementos de Composition

Una vez implementada la infraestructura, ahora puedes generar el contenido de Composition que quieres mostrar.

En este ejemplo, se agrega código que crea y anima un cuadrado [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) simple.

1. Agrega un elemento de Composition. En CompositionHostControl.xaml.cs, agrega estos métodos a la clase CompositionHostControl.

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

### <a name="handle-dpi-changes"></a>Administración de los cambios de ppp

El código para agregar y animar un elemento tiene en cuenta la escala de ppp actual cuando se crean los elementos, pero también tienes que tener en cuenta los cambios de ppp mientras se ejecuta la aplicación. Puedes controlar el evento [HwndHost.DpiChanged](/dotnet/api/system.windows.interop.hwndhost.dpichanged) para recibir notificaciones de los cambios y ajustar los cálculos en función del nuevo valor de ppp.

1. En el método CompositionHostControl_Loaded, después de la última línea, agrega esto para enlazar el controlador de eventos DpiChanged.

    ```csharp
    compositionHost.DpiChanged += CompositionHost_DpiChanged;
    ```

1. Agrega el método de control de eventos con el nombre _CompositionHostDpiChanged_. Este código ajusta la escala y el desplazamiento de cada elemento, y vuelve a calcular las animaciones que no se han completado.

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

Ahora, puedes agregar el control de usuario a la interfaz de usuario XAML.

1. En MainWindow.xaml, establece el alto de la ventana en 600 y el ancho en 840.
1. Agrega el código XAML de la interfaz de usuario. En MainWindow.xaml, agrega este código XAML entre las etiquetas `<Grid> </Grid>` raíz.

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

1. Controla el clic de botón para crear nuevos elementos. (El evento Click ya está enlazado en el código XAML).

    En MainWindow.xaml.cs, agrega este método de controlador de eventos *Button_Click*. Este código llama a _CompositionHost.AddElement_ para crear un nuevo elemento con un tamaño y desplazamiento generados de forma aleatoria.

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

Ahora puedes compilar y ejecutar la aplicación de WPF. Si es necesario, comprueba el código completo al final del tutorial para asegurarte de que todo el código está en el lugar correcto.

Al ejecutar la aplicación y hacer clic en el botón, verás que se agregan los cuadrados animados a la interfaz de usuario.

## <a name="next-steps"></a>Pasos siguientes

Para ver un ejemplo más completo que se basa en la misma infraestructura, consulta en GitHub el [ejemplo de integración de la capa visual en WPF](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration).

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción (WPF)](/dotnet/framework/wpf/getting-started/) (.NET)
- [Interoperar con código no administrado](/dotnet/framework/interop/) (.NET)
- [Introducción a las aplicaciones de Windows 10](/windows/uwp/get-started/) (UWP)
- [Mejorar tu aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Espacio de nombres Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Código completo

Este es el código completo de este tutorial.

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