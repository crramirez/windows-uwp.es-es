---
title: Usar el nivel visual con Windows Forms
description: Aprenda técnicas para usar las API de nivel visual en combinación con el contenido de Windows Forms existente para crear animaciones y efectos avanzados.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 9da9dee48beef6e3c1cd38ffbe9761ed89fd940d
ms.sourcegitcommit: 93d0b2996b4742b33cd6d641e036f42672cf5238
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "69999637"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>Usar el nivel visual con Windows Forms

Puede usar Windows Runtime las API de composición (también denominada [capa visual](/windows/uwp/composition/visual-layer)) en las aplicaciones de Windows Forms para crear experiencias modernas que se encienden para los usuarios de Windows 10.

El código completo de este tutorial está disponible en GitHub: [Windows Forms ejemplo de HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition).

## <a name="prerequisites"></a>Requisitos previos

La API de hospedaje de UWP tiene estos requisitos previos.

- Damos por hecho que estás familiarizado con el desarrollo de aplicaciones mediante Windows Forms y UWP. Para obtener más información, consulta:
  - [Introducción con Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Introducción a las aplicaciones de Windows 10](/windows/uwp/get-started/)
  - [Mejorar la aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 o posterior
- Windows 10 versión 1803 o posterior
- Windows 10 SDK 17134 o posterior

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Cómo usar las API de composición en Windows Forms

En este tutorial, creará una interfaz de usuario de Windows Forms simple y le agregará elementos de composición animados. Los componentes de Windows Forms y de composición se mantienen sencillos, pero el código de interoperabilidad mostrado es el mismo independientemente de la complejidad de los componentes. La aplicación finalizada tiene el siguiente aspecto.

![La interfaz de usuario de la aplicación en ejecución](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Creación de un proyecto de Windows Forms

El primer paso es crear el proyecto de aplicación de Windows Forms, que incluye una definición de aplicación y el formulario principal de la interfaz de usuario.

Para crear un nuevo proyecto de aplicación de Windows Forms C# en Visual denominado _HelloComposition_:

1. Abra Visual Studio y seleccione **archivo** > **nuevo** > **proyecto**.<br/>Se abrirá el cuadro de diálogo **nuevo proyecto** .
1. En la categoría **instalado** , expanda el nodo  **C# visual** y, a continuación, seleccione escritorio de **Windows**.
1. Seleccione la plantilla **Windows Forms App (.NET Framework)** .
1. Escriba el nombre _HelloComposition,_ seleccione Framework **.NET Framework 4.7.2**y, a continuación, haga clic en **Aceptar**.

Visual Studio crea el proyecto y abre el diseñador para la ventana de la aplicación predeterminada denominada Form1.cs.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configuración del proyecto para usar Windows Runtime API

Para usar las API de Windows Runtime (WinRT) en la aplicación Windows Forms, debe configurar el proyecto de Visual Studio para tener acceso a la Windows Runtime. Además, las API de composición usan los vectores de manera extensa, por lo que debe agregar las referencias necesarias para usar vectores.

Los paquetes de NuGet están disponibles para abordar estas dos necesidades. Instale las versiones más recientes de estos paquetes para agregar las referencias necesarias al proyecto.  

- [Microsoft. Windows. SDK. Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (requiere el formato de administración de paquetes predeterminado establecido en PackageReference).
- [System. Numerics. vectores](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> Aunque se recomienda usar los paquetes de NuGet para configurar el proyecto, puede Agregar manualmente las referencias necesarias. Para obtener más información, consulte [mejorar la aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance). En la tabla siguiente se muestran los archivos a los que es necesario agregar referencias.

|Archivo|Location|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Archivos de programa (x86) \Windows\<Kits\10\References*SDK versión*>\<\Windows.Foundation.UniversalApiContract*versión*>|
|Windows.Foundation.FoundationContract.winmd|C:\Archivos de programa (x86) \Windows\<Kits\10\References*SDK versión*>\<\Windows.Foundation.FoundationContract*versión*>|
|System. Numerics. Vectors. dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System. Numerics. dll|C:\Archivos de programa (x86) \Reference\.Assemblies\Microsoft\Framework NETFramework\v4.7.2|

## <a name="create-a-custom-control-to-manage-interop"></a>Crear un control personalizado para administrar la interoperabilidad

Para hospedar el contenido que crea con la capa visual, cree un control personalizado derivado de [control](/dotnet/api/system.windows.forms.control). Este control proporciona acceso a un [identificador](/dotnet/api/system.windows.forms.control.handle)de ventana, que se necesita para crear el contenedor para el contenido de la capa visual.

Aquí es donde se realiza la mayoría de la configuración para hospedar las API de composición. En este control, se usan los servicios de invocación de [plataforma (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) y la interoperabilidad [com](/dotnet/api/system.runtime.interopservices.comimportattribute) para llevar las API de composición a la aplicación Windows Forms. Para obtener más información sobre PInvoke y la interoperabilidad COM, vea interoperar [con código no administrado](/dotnet/framework/interop/index).

> [!TIP]
> Si es necesario, compruebe el código completo al final del tutorial para asegurarse de que todo el código se encuentra en los lugares correctos mientras trabaja en el tutorial.

1. Agregue un nuevo archivo de control personalizado al proyecto que derive de [control](/dotnet/api/system.windows.forms.control).
    - En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto _HelloComposition_ .
    - En el menú contextual, seleccione **Agregar** > **nuevo elemento.** ...
    - En el cuadro de diálogo **Agregar nuevo elemento** , seleccione **control personalizado**.
    - Asigne al control el nombre _CompositionHost.CS_y, a continuación, haga clic en **Agregar**. CompositionHost.cs se abre en el Vista de diseño.

1. Cambie a la vista de código para CompositionHost.cs y agregue el código siguiente a la clase.

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

    En el constructor, se llama a los métodos _InitializeCoreDispatcher_ y _InitComposition_ . Cree estos métodos en los pasos siguientes.

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
    ```

1. Inicializar un subproceso con un [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). El distribuidor principal es responsable de procesar los mensajes de ventana y de enviar los eventos para las API de WinRT. Las nuevas instancias de **compositor** deben crearse en un subproceso que tenga un CoreDispatcher.
    - Cree un método denominado _InitializeCoreDispatcher_ y agregue el código para configurar la cola del distribuidor.

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

    - La cola del distribuidor requiere una declaración PInvoke. Coloque esta declaración al final del código para la clase. (Este código se coloca dentro de una región para mantener el código de clase ordenado).

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

    Ahora tiene la cola del distribuidor lista y puede empezar a inicializar y crear el contenido de la composición.

1. Inicialice el [compositor](/uwp/api/windows.ui.composition.compositor). El compositor es un generador que crea una variedad de tipos en el espacio de nombres [Windows. UI. Composition](/uwp/api/windows.ui.composition) que abarca la capa visual, el sistema de efectos y el sistema de animación. La clase compositor también administra la duración de los objetos creados a partir del generador.

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

    - **ICompositorDesktopInterop** y **ICompositionTarget** requieren importaciones com. Coloque este código después de la clase _CompositionHost_ , pero dentro de la declaración del espacio de nombres.

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>Crear un control personalizado para hospedar elementos de composición

Es una buena idea colocar el código que genera y administra los elementos de composición en un control independiente que se deriva de CompositionHost. Esto mantiene el código de interoperabilidad que ha creado en la clase CompositionHost reutilizable.

Aquí se crea un control personalizado derivado de CompositionHost. Este control se agrega al cuadro de herramientas de Visual Studio para que pueda agregarlo al formulario.

1. Agregue un nuevo archivo de control personalizado al proyecto que deriva de CompositionHost.
    - En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto _HelloComposition_ .
    - En el menú contextual, seleccione **Agregar** > **nuevo elemento.** ...
    - En el cuadro de diálogo **Agregar nuevo elemento** , seleccione **control personalizado**.
    - Asigne al control el nombre _CompositionHostControl.CS_y, a continuación, haga clic en **Agregar**. CompositionHostControl.cs se abre en el Vista de diseño.

1. En el panel Propiedades de la vista de diseño CompositionHostControl.cs, establezca la propiedad **BackColor** en **ControlLight**.

    Establecer el color de fondo es opcional. Lo hacemos aquí para que pueda ver el control personalizado con el fondo del formulario.

1. Cambie a la vista de código para CompositionHostControl.cs y actualice la declaración de clase para que derive de CompositionHost.

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. Actualice el constructor para llamar al constructor base.

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>Agregar elementos de composición

Una vez implementada la infraestructura, ahora puede agregar contenido de composición a la interfaz de usuario de la aplicación.

En este ejemplo, agregará código a la clase CompositionHostControl que crea y anima un [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)simple.

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

Ahora que tiene un control personalizado para hospedar contenido de composición, puede agregarlo a la interfaz de usuario de la aplicación. Aquí, agregará una instancia del CompositionHostControl creado en el paso anterior. CompositionHostControl se agrega automáticamente al cuadro de herramientas de Visual Studio en componentes del **_nombre del proyecto_** .

1. En la vista de diseño Form1.CS, agregue un botón a la interfaz de usuario.

    - Arrastre un botón desde el cuadro de herramientas hasta Form1. Colóquelo en la esquina superior izquierda del formulario. (Vea la imagen al principio del tutorial para comprobar la ubicación de los controles).
    - En el panel Propiedades, cambie la propiedad **Text** de _button1_ a _Agregar elemento de composición_.
    - Cambie el tamaño del botón para que se muestre todo el texto.

    (Para obtener más información, [consulte Cómo: Agregue controles a Windows Forms](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms)).

1. Agregue un CompositionHostControl a la interfaz de usuario.

    - Arrastre un CompositionHostControl desde el cuadro de herramientas hasta Form1. Colóquelo a la derecha del botón.
    - Cambie el tamaño del CompositionHost para que rellene el resto del formulario.

1. Controle el evento click del botón.

   - En el panel Propiedades, haga clic en el rayo para cambiar a la vista de eventos.
   - En la lista eventos, seleccione el evento **click** , escriba *Button_Click*y presione Entrar.
   - Este código se agrega en Form1.cs:

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. Agregue código al controlador de clic de botón para crear nuevos elementos.

    - En Form1.cs, agregue código al controlador de eventos *Button_Click* que creó anteriormente. Este código llama a _CompositionHostControl1. AddElement_ para crear un nuevo elemento con un tamaño y desplazamiento generados de forma aleatoria. (La instancia de CompositionHostControl se llamaba automáticamente _compositionHostControl1_ al arrastrarla al formulario).

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

Ahora puede compilar y ejecutar la aplicación Windows Forms. Al hacer clic en el botón, debería ver los cuadrados animados agregados a la interfaz de usuario.

## <a name="next-steps"></a>Pasos siguientes

Para obtener un ejemplo más completo que se basa en la misma infraestructura, consulte el [ejemplo de integración de capas visuales de Windows Forms](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration) en github.

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción con Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms) 2003
- [Interoperar con código no administrado](/dotnet/framework/interop/) 2003
- [Introducción a las aplicaciones de Windows 10](/windows/uwp/get-started/) UWP
- [Mejorar la aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) UWP
- [Espacio de nombres Windows. UI. Composition](/uwp/api/windows.ui.composition) UWP

## <a name="complete-code"></a>Código completo

Este es el código completo de este tutorial.

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