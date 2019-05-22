---
title: Uso de la capa Visual con Windows Forms
description: Aprenda técnicas para usar las API de nivel Visual en combinación con el contenido existente de Windows Forms para crear efectos y animaciones avanzadas.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0a3aff0bee68b971accd96f895601343726024d0
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985035"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>Uso de la capa Visual con Windows Forms

Puede usar las API de composición de Windows en tiempo de ejecución (también denominado el [capa Visual](/windows/uwp/composition/visual-layer)) en las aplicaciones de Windows Forms para crear experiencias modernas es clara para los usuarios de Windows 10.

El código completo de este tutorial está disponible en GitHub: [Ejemplo de Windows Forms HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition).

## <a name="prerequisites"></a>Requisitos previos

La API de hospedaje de UWP tiene estos requisitos previos.

- Se supone que tiene cierta familiaridad con el desarrollo de aplicaciones con Windows Forms y UWP. Para obtener más información, consulta:
  - [Introducción a Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Empezar a trabajar con aplicaciones de Windows 10](/windows/uwp/get-started/)
  - [Mejore su aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET framework 4.7.2 o posterior
- Windows 10, versión 1803 o posterior
- SDK de Windows 10 17134 o posterior

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Cómo usar las API de composición de Windows Forms

En este tutorial, creará una sencilla interfaz de usuario de Windows Forms y agregarle elementos animados de composición. Componentes de Windows Forms y de composición se mantienen simple, pero se muestra el código de interoperabilidad es el mismo independientemente de la complejidad de los componentes. La aplicación finalizada es similar al siguiente.

![La interfaz de usuario de aplicación en ejecución](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Crear un proyecto de Windows Forms

El primer paso es crear el proyecto de aplicación de Windows Forms, que incluye una definición de aplicación y el formulario principal de la interfaz de usuario.

Para crear un nuevo proyecto de aplicación de Windows Forms en Visual C# denominado _HelloComposition_:

1. Abra Visual Studio y seleccione **archivo** > **New** > **proyecto**.<br/>El **nuevo proyecto** abre el cuadro de diálogo.
1. En el **instalado** categoría, expanda el **Visual C#**  nodo y, a continuación, seleccione **Windows Desktop**.
1. Seleccione el **Windows Forms App (.NET Framework)** plantilla.
1. Escriba el nombre _HelloComposition,_ seleccione Framework **.NET Framework 4.7.2**, a continuación, haga clic en **Aceptar**.

Visual Studio crea el proyecto y abre el Diseñador de la ventana de aplicación predeterminada denominado Form1.cs.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurar el proyecto para usar Windows Runtime APIs

Para usar Windows en tiempo de ejecución (WinRT) API en su aplicación de Windows Forms, deberá configurar el proyecto de Visual Studio para tener acceso el tiempo de ejecución de Windows. Además, vectores se usan ampliamente por las API de composición, por lo que deberá agregar las referencias necesarias para usar vectores.

Paquetes de NuGet están disponibles para abordar estas necesidades de ambos. Instale las últimas versiones de estos paquetes para agregar las referencias necesarias al proyecto.  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (requiere el conjunto de formato de administración de paquete predeterminado a PackageReference).
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> Aunque se recomienda usar los paquetes de NuGet para configurar el proyecto, puede agregar manualmente las referencias necesarias. Para obtener más información, consulte [mejorar su aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance). La siguiente tabla muestra los archivos que necesita para agregar referencias a.

|Archivo|Location|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program archivos (x86) \Windows Kits\10\References\<*sdk versión*> \Windows.Foundation.UniversalApiContract\<*versión*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program archivos (x86) \Windows Kits\10\References\<*sdk versión*> \Windows.Foundation.FoundationContract\<*versión*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Program archivos (x86) Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="create-a-custom-control-to-manage-interop"></a>Crear un control personalizado para administrar la interoperabilidad

Para hospedar contenido que cree con la capa visual, crea un control personalizado derivado de [Control](/dotnet/api/system.windows.forms.control). Este control proporciona acceso a una ventana [controlar](/dotnet/api/system.windows.forms.control.handle), lo que necesitará para crear el contenedor de contenido de la capa visual.

Esto es donde realiza la mayoría de la configuración para hospedar las API de composición. En este control, usa [servicios de invocación de plataforma (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) y [interoperabilidad COM](/dotnet/api/system.runtime.interopservices.comimportattribute) para poner las API de composición en su aplicación de Windows Forms. Para obtener más información sobre PInvoke y la interoperabilidad COM, vea [interoperar con código no administrado](/dotnet/framework/interop/index).

> [!TIP]
> Si necesita, compruebe el código completo al final del tutorial para asegurarse de que todo el código está en los lugares correctos cuando se trabaja con el tutorial.

1. Agregue un nuevo archivo de control personalizado al proyecto que se derive de [Control](/dotnet/api/system.windows.forms.control).
    - En **el Explorador de soluciones**, haga clic en el _HelloComposition_ proyecto.
    - En el menú contextual, seleccione **agregar** > **nuevo elemento...** .
    - En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **Custom Control**.
    - Nombre del control _CompositionHost.cs_, a continuación, haga clic en **agregar**. CompositionHost.cs se abre en la vista de diseño.

1. Cambiar a vista de código para CompositionHost.cs y agregue el código siguiente a la clase.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    object dispatcherQueue;
    protected ContainerVisual containerVisual;
    protected Compositor compositor;

    private ICompositionTarget compositionTarget;

    public Visual Child
    {
        set
        {
            if (compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }
    ```

1. Agregue código al constructor.

    En el constructor, llame a la _InitializeCoreDispatcher_ y _InitComposition_ métodos. Puede crear estos métodos en los pasos siguientes.

    ```csharp
    public CompositionHost()
    {
        InitializeComponent();

        // Get the window handle.
        hwndHost = Handle;

        // Create dispatcher queue.
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content.
        InitComposition(hwndHost);
    }

1. Initialize a thread with a [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). The core dispatcher is responsible for processing window messages and dispatching events for WinRT APIs. New instances of **Compositor** must be created on a thread that has a CoreDispatcher.
    - Create a method named _InitializeCoreDispatcher_ and add code to set up the dispatcher queue.

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

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

    - La cola del distribuidor, requiere una declaración PInvoke. Esta declaración se coloca al final del código de la clase. (Este código se coloque dentro de una región para mantener el código de clase ordenada.)

    ```csharp
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

    #endregion PInvoke declarations
    ```

    Ahora tiene preparada la cola del distribuidor y empezar a inicializar y crear contenido composición.

1. Inicializar el [Compositor](/uwp/api/windows.ui.composition.compositor). El Compositor es una fábrica que crea una variedad de tipos en el [Windows.UI.Composition](/uwp/api/windows.ui.composition) espacio de nombres que abarca la capa visual, el sistema de efectos y el sistema de animación. La clase Compositor también administra la duración de objetos creado a partir de la fábrica.

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
        compositionTarget = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }

        containerVisual = compositor.CreateContainerVisual();
        Child = containerVisual;
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

## <a name="create-a-custom-control-to-host-composition-elements"></a>Crear un control personalizado a los elementos de la composición de host

Es una buena idea poner el código que genera y administra los elementos de composición en un control independiente que se deriva de CompositionHost. Esto evita que el código de interoperabilidad que creó en la clase CompositionHost reutilizables.

En este caso, cree un control personalizado derivado de CompositionHost. Este control se agrega al cuadro de herramientas de Visual Studio, por lo que puede agregar al formulario.

1. Agregue un nuevo archivo de control personalizado al proyecto que se deriva de CompositionHost.
    - En **el Explorador de soluciones**, haga clic en el _HelloComposition_ proyecto.
    - En el menú contextual, seleccione **agregar** > **nuevo elemento...** .
    - En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **Custom Control**.
    - Nombre del control _CompositionHostControl.cs_, a continuación, haga clic en **agregar**. CompositionHostControl.cs se abre en la vista de diseño.

1. En el panel de propiedades de la vista de diseño CompositionHostControl.cs, establezca el **BackColor** propiedad **ControlLight**.

    Establecer el color de fondo es opcional. Lo hacemos aquí para que pueda ver su control personalizado con el fondo del formulario.

1. Cambiar a vista de código para CompositionHostControl.cs y actualice la declaración de clase que derive de CompositionHost.

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. Actualizar el constructor para llamar al constructor base.

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>Agregar elementos de composición

Con la infraestructura en su lugar, ahora puede agregar contenido de la composición al interfaz de usuario de la aplicación.

En este ejemplo, agregue código a la clase CompositionHostControl que crea y se anima un sencillo [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual).

1. Agregue un elemento de composición.

    En CompositionHostControl.cs, agregue estos métodos a la clase CompositionHostControl.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size); // Requires references
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX, offsetY, 0);
        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X);
        Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
        float bottom = Height - visual.Size.Y;
        animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
        animation.Duration = TimeSpan.FromSeconds(2);
        animation.DelayTime = TimeSpan.FromSeconds(delay);
        visual.StartAnimation("Offset", animation);
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

## <a name="add-the-control-to-your-form"></a>Agregar el control al formulario

Ahora que tiene un control personalizado para hospedar contenido de la composición, puede agregarlo al interfaz de usuario de la aplicación. En este caso, agregue una instancia de la CompositionHostControl que creó en el paso anterior. CompositionHostControl se agrega automáticamente al cuadro de herramientas de Visual Studio en  **_nombre del proyecto_ componentes**.

1. En la vista de diseño de Form1.CS, agregue un botón a la interfaz de usuario.

    - Arrastre un botón del cuadro de herramientas a Form1. Colóquelo en la esquina superior izquierda del formulario. (Vea la imagen al principio del tutorial para comprobar la colocación de los controles).
    - En el panel Propiedades, cambie la **texto** propiedad desde _button1_ a _Agregar elemento de composición_.
    - Cambiar el tamaño del botón para que se muestre todo el texto.

    (Para obtener más información, vea [Cómo: Agregar controles a Windows Forms](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms).)

1. Agregar un CompositionHostControl a la interfaz de usuario.

    - Arrastre un CompositionHostControl desde el cuadro de herramientas a Form1. Colóquelo a la derecha del botón.
    - Cambiar el tamaño de la CompositionHost para que rellene el resto del formulario.

1. Identificador del botón, haga clic en eventos.

   - En el panel Propiedades, haga clic en el rayo para cambiar a la vista de eventos.
   - En la lista de eventos, seleccione el **haga clic en** eventos, escriba *Button_Click*, y presione ENTRAR.
   - Este código se agrega en Form1.cs:

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. Agregue código al botón haga clic en el controlador para crear nuevos elementos.

    - En Form1.cs, agregue código a la *Button_Click* controlador de eventos que creó anteriormente. Este código llama a _CompositionHostControl1.AddElement_ para crear un nuevo elemento con un tamaño generado al azar y desplazamiento. (La instancia de CompositionHostControl automáticamente se denominaba _compositionHostControl1_ cuando se arrastra hasta el formulario.)

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
        float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
        compositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

Ahora puede compilar y ejecutar la aplicación de Windows Forms. Al hacer clic en el botón, debería ver animados cuadrados agregados a la interfaz de usuario.

## <a name="next-steps"></a>Pasos siguientes

Para obtener un ejemplo más completo que se basa en la misma infraestructura, consulte el [ejemplo de integración de Windows Forms Visual layer](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration) en GitHub.

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms) (. NET)
- [Interoperar con código no administrado](/dotnet/framework/interop/) (. NET)
- [Empezar a trabajar con aplicaciones de Windows 10](/windows/uwp/get-started/) (UWP)
- [Mejore su aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Espacio de nombres Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Código completo

Este es el código completo para este tutorial.

### <a name="form1cs"></a>Form1.cs

```csharp
using System;
using System.Windows.Forms;

namespace HelloComposition
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, EventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
            float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
            compositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolcs"></a>CompositionHostControl.cs

```csharp
using System;
using System.Numerics;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHostControl : CompositionHost
    {
        public CompositionHostControl() : base()
        {

        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size); // Requires references
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX, offsetY, 0);
            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X);
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            float bottom = Height - visual.Size.Y;
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
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
using System.Windows.Forms;
using Windows.UI.Composition;

namespace HelloComposition
{
    public partial class CompositionHost : Control
    {
        IntPtr hwndHost;
        object dispatcherQueue;
        protected ContainerVisual containerVisual;
        protected Compositor compositor;
        private ICompositionTarget compositionTarget;

        public Visual Child
        {
            set
            {
                if (compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        public CompositionHost()
        {
            // Get the window handle.
            hwndHost = Handle;

            // Create dispatcher queue.
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition tree of content.
            InitComposition(hwndHost);
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

            compositor = new Compositor();
            object iunknown = compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }

            containerVisual = compositor.CreateContainerVisual();
            Child = containerVisual;
        }

        protected override void OnPaint(PaintEventArgs pe)
        {
            base.OnPaint(pe);
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