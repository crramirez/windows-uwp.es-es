---
title: Usar la capa visual con Windows Forms
description: Obtén información sobre las técnicas de uso de las API de la capa visual en combinación con contenido de Windows Forms ya existente para crear animaciones y efectos avanzados.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 9da9dee48beef6e3c1cd38ffbe9761ed89fd940d
ms.sourcegitcommit: 93d0b2996b4742b33cd6d641e036f42672cf5238
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "69999637"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>Usar la capa visual con Windows Forms

Puedes usar las API de Composition de Windows Runtime (también llamada la [capa visual](/windows/uwp/composition/visual-layer)) en las aplicaciones de Windows Forms para crear experiencias modernas que atraigan a los usuarios de Windows 10.

El código completo de este tutorial está disponible en GitHub: [Ejemplo HelloComposition de Windows Forms](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition).

## <a name="prerequisites"></a>Requisitos previos

La API de hospedaje de UWP tiene estos requisitos previos.

- Damos por hecho que estás familiarizado con el desarrollo de aplicaciones con Windows Forms y UWP. Para obtener más información, consulta:
  - [Introducción a Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Introducción a las aplicaciones de Windows 10](/windows/uwp/get-started/)
  - [Mejorar tu aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 o una versión posterior
- Windows 10 versión 1803 o posterior
- SDK de Windows 10 17134 o posterior

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Procedimiento para usar las API de Composition en Windows Forms

En este tutorial, crearás una interfaz de usuario de aplicación de Windows Forms simple y le agregarás elementos de Composition animados. Tanto Windows Forms como los componentes de Composition son sencillos, pero el código de interoperabilidad mostrado es el mismo, independientemente de la complejidad de los componentes. La aplicación finalizada tiene el siguiente aspecto.

![Interfaz de usuario de la aplicación en ejecución](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Creación de un proyecto de Windows Forms

El primer paso es crear el proyecto de aplicación de Windows Forms, que incluye una definición de aplicación y el formulario principal para la interfaz de usuario.

Para crear un nuevo proyecto de aplicación de Windows Forms en Visual C# llamado _HelloComposition_:

1. Abre Visual Studio y selecciona **Archivo** > **Nuevo** > **Proyecto**.<br/>Se abre el cuadro de diálogo **Nuevo proyecto**.
1. En la categoría **Instalados**, expande el nodo **Visual C#** y, después, selecciona **Escritorio de Windows**.
1. Selecciona la plantilla **Aplicación de Windows Forms (.NET Framework)** .
1. Escribe el nombre _HelloComposition_, selecciona Framework **.NET Framework 4.7.2** y, después, haz clic en **Aceptar**.

Visual Studio crea el proyecto y abre el diseñador de la ventana predeterminada de la aplicación llamada Form1.cs.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurar el proyecto para usar las API de Windows Runtime

Para usar las API de Windows Runtime (WinRT) en la aplicación de Windows Forms, tienes que configurar el proyecto de Visual Studio para que tenga acceso a Windows Runtime. Además, las API de Composition usan mucho los vectores, por lo que tienes que agregar las referencias necesarias para usar vectores.

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

## <a name="create-a-custom-control-to-manage-interop"></a>Crear un control personalizado para administrar la interoperabilidad

Para hospedar el contenido que creas con la capa visual, crea un control personalizado derivado de [Control](/dotnet/api/system.windows.forms.control). Este control permite acceder a una ventana [Handle](/dotnet/api/system.windows.forms.control.handle), que se necesita para crear el contenedor para el contenido de la capa visual.

Aquí es donde se realiza la mayoría de la configuración para hospedar las API de Composition. En este control, se usan los [servicios de invocación de plataforma (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) y la [interoperabilidad COM](/dotnet/api/system.runtime.interopservices.comimportattribute) para incorporar las API de Composition a la aplicación de Windows Forms. Para más información sobre PInvoke y la interoperabilidad COM, consulta [Interoperar con código no administrado](/dotnet/framework/interop/index).

> [!TIP]
> Si es necesario, comprueba el código completo al final del tutorial para asegurarte de que todo el código está en el lugar correcto mientras trabajas en el tutorial.

1. Agrega un nuevo archivo de control personalizado al proyecto que derive de [Control](/dotnet/api/system.windows.forms.control).
    - En el **Explorador de soluciones**, haz clic con el botón derecho en el proyecto _HelloComposition_.
    - En el menú contextual, selecciona **Agregar** > **Nuevo elemento...** .
    - En el cuadro de diálogo **Agregar nuevo elemento**, selecciona **Control personalizado**.
    - Asigna al control el nombre _CompositionHost.cs_ y haz clic en **Agregar**. CompositionHost.cs se abre en la vista de diseño.

1. Cambia a la vista de código para ver CompositionHost.cs y agrega el código siguiente a la clase.

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

1. Agrega código al constructor.

    En el constructor, se llama a los métodos _InitializeCoreDispatcher_ e _InitComposition_. Estos métodos se crean en los pasos siguientes.

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

1. Inicializa un subproceso con [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). El distribuidor principal es responsable de procesar los mensajes de ventana y de distribuir los eventos de las API de WinRT. Se deben crear nuevas instancias de **Compositor** en un subproceso que tenga un objeto CoreDispatcher.
    - Crea un método llamado _InitializeCoreDispatcher_ y agrega código para configurar la cola del distribuidor.

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

    - La cola del distribuidor requiere una declaración PInvoke. Coloca esta declaración al final del código de la clase. (Este código se coloca dentro de una región para mantener ordenado el código de la clase).

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
        compositionTarget = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }

        containerVisual = compositor.CreateContainerVisual();
        Child = containerVisual;
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

## <a name="create-a-custom-control-to-host-composition-elements"></a>Crear un control personalizado para hospedar elementos de Composition

Es una buena idea colocar el código que genera y administra los elementos de Composition en un control independiente que derive de CompositionHost. Esto permite reutilizar el código de interoperabilidad que has creado en la clase CompositionHost.

Aquí se crea un control personalizado derivado de CompositionHost. Este control se agrega al cuadro de herramientas de Visual Studio para que puedas agregarlo al formulario.

1. Agrega al proyecto un nuevo archivo de control personalizado que derive de CompositionHost.
    - En el **Explorador de soluciones**, haz clic con el botón derecho en el proyecto _HelloComposition_.
    - En el menú contextual, selecciona **Agregar** > **Nuevo elemento...** .
    - En el cuadro de diálogo **Agregar nuevo elemento**, selecciona **Control personalizado**.
    - Asigna al control el nombre _CompositionHostControl.cs_ y haz clic en **Agregar**. CompositionHostControl.cs se abre en la vista de diseño.

1. En el panel Propiedades de la vista de diseño de CompositionHostControl.cs, establece la propiedad **BackColor** en **ControlLight**.

    Establecer el color de fondo es opcional. Lo hacemos aquí para que puedas ver el control personalizado contra el fondo del formulario.

1. Cambia a la vista de código de CompositionHostControl.cs y actualiza la declaración de clase para que derive de CompositionHost.

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. Actualiza el constructor para llamar al constructor base.

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>Agregar elementos de Composition

Una vez implementada la infraestructura, ahora puedes agregar contenido de Composition a la interfaz de usuario de la aplicación.

En este ejemplo, agregarás código a la clase CompositionHostControl que crea y anima un objeto [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) simple.

1. Agrega un elemento de Composition.

    En CompositionHostControl.cs, agrega estos métodos a la clase CompositionHostControl.

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

Ahora que tienes un control personalizado para hospedar el contenido de Composition, puedes agregarlo a la interfaz de usuario de la aplicación. Aquí, agregarás una instancia del control CompositionHostControl creado en el paso anterior. CompositionHostControl se agrega automáticamente al cuadro de herramientas de Visual Studio en **Componentes de _nombre del proyecto_** .

1. En la vista de diseño de Form1.cs, agrega un botón a la interfaz de usuario.

    - Arrastra un botón desde el cuadro de herramientas hasta Form1. Colócalo en la esquina superior izquierda del formulario. (Consulta la imagen al principio del tutorial para comprobar la ubicación de los controles).
    - En el panel Propiedades, cambia la propiedad **Text** de _button1_ a _Add composition element_ (Agregar elemento de Composition).
    - Cambia el tamaño del botón para que se muestre todo el texto.

    (Para más información, consulta [cómo agregar controles a Windows Forms](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms)).

1. Agrega un control CompositionHostControl a la interfaz de usuario.

    - Arrastra un control CompositionHostControl desde el cuadro de herramientas hasta Form1. Colócalo a la derecha del botón.
    - Cambia el tamaño de CompositionHost para que llene el resto del formulario.

1. Controla el evento Click del botón.

   - En el panel Propiedades, haz clic en el rayo para cambiar a la vista de eventos.
   - En la lista de eventos, selecciona el evento **Click**, escribe *Button_Click* y presiona Entrar.
   - Este código se agrega a Form1.cs:

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. Agrega código al controlador de clic del botón para crear nuevos elementos.

    - En Form1.cs, agrega código al controlador del evento *Button_Click* que creó anteriormente. Este código llama a _CompositionHostControl1.AddElement_ para crear un nuevo elemento con un tamaño y desplazamiento generados de forma aleatoria. (La instancia de CompositionHostControl recibió automáticamente el nombre _compositionHostControl1_ al arrastrarla al formulario).

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

Ahora puedes compilar y ejecutar la aplicación de Windows Forms. Al hacer clic en el botón, verás que se agregan los cuadrados animados a la interfaz de usuario.

## <a name="next-steps"></a>Pasos siguientes

Para ver un ejemplo más completo que se basa en la misma infraestructura, consulta en GitHub el [ejemplo de integración de la capa visual en Windows Forms](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration).

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms) (.NET)
- [Interoperar con código no administrado](/dotnet/framework/interop/) (.NET)
- [Introducción a las aplicaciones de Windows 10](/windows/uwp/get-started/) (UWP)
- [Mejorar tu aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Espacio de nombres Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

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