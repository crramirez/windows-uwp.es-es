---
author: msatranjr
title: "Generación de eventos en componentes de Windows Runtime"
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: 
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: abd46f788c8947da4c8704baf0f0ea802fff38ef
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="raising-events-in-windows-runtime-components"></a>Generación de eventos en componentes de Windows Runtime


\[ Actualizado para aplicaciones para UWP en Windows10. Para leer artículos sobre Windows8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Si tu componente de Windows Runtime genera un evento de un tipo de delegado definido por el usuario en un subproceso en segundo plano (subproceso de trabajo) y deseas que JavaScript pueda recibir el evento, puedes implementarlo o generarlo mediante uno de estos métodos:

-   (Opción 1) Genera el evento a través de [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) para calcular las referencias del evento en el contexto del subproceso de JavaScript. Aunque esta suele ser la mejor opción, en algunos escenarios es posible que no proporcione el rendimiento más rápido.
-   (Opción 2) Usa [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt; pero pierde la información de tipo (pierde la información de tipo de evento). Si no es factible la opción 1 o su rendimiento no es adecuado, esta es una buena segunda opción si la pérdida de información de tipo es aceptable.
-   (Opción 3) Crea tu propio proxy y código auxiliar para el componente. Esta opción es la más difícil de implementar, pero conserva la información de tipo y podría proporcionar un mejor rendimiento en comparación con la opción 1 en escenarios exigentes.

Si solo se genera un evento en un subproceso en segundo plano sin usar una de estas opciones, un cliente de JavaScript no recibirá el evento.

## <a name="background"></a>En segundo plano


Todos los componentes y aplicaciones de Windows Runtime son fundamentalmente objetos COM, independientemente del lenguaje que se usa para crearlos. En la API de Windows, la mayoría de los componentes son objetos COM ágiles que se pueden comunicar igual de bien con los objetos en el subproceso en segundo plano y en el subproceso de IU. Si no se puede realizar un objeto COM ágil, se requerirán objetos auxiliares conocidos como proxies y códigos auxiliares para comunicarse con otros objetos COM a través de los límites del subproceso de IU y el subproceso en segundo plano. (En términos COM, esto se conoce como una comunicación entre subprocesamientos controlados).

La mayoría de los objetos de la API de Windows son ágiles o tienen proxies y códigos auxiliares integrados. Sin embargo, no se pueden crear proxies ni códigos auxiliares para los tipos genéricos como Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) porque no son tipos completos hasta que se proporciona el argumento de tipo. Solo en el caso de los clientes de JavaScript la falta de proxys o códigos auxiliares se convierte en un problema, pero si deseas que tu componente se pueda usar desde JavaScript así como desde un lenguaje de .NET o C++, a continuación debes usar una de las tres opciones siguientes.

## <a name="option-1-raise-the-event-through-the-coredispatcher"></a>(Opción 1) Generar el evento a través de CoreDispatcher


Puede enviar eventos de cualquier tipo de delegado definido por el usuario mediante [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx), y JavaScript podrá recibirlos. Si no estás seguro de qué opción usar, prueba con esta primero. Si la latencia entre la activación de los eventos y el controlador de eventos se convierte en un problema, prueba una de las otras opciones.

El siguiente ejemplo muestra cómo usar CoreDispatcher para generar un evento fuertemente tipado. Observa que el argumento de tipo es Notificación del sistema, y no Objeto.

```csharp
public event EventHandler<Toast> ToastCompletedEvent;
private void OnToastCompleted(Toast args)
{
    var completedEvent = ToastCompletedEvent;
    if (completedEvent != null)
    {
        completedEvent(this, args);
    }
}

public void MakeToastWithDispatcher(string message)
{
    Toast toast = new Toast(message);
    // Assume you have a CoreDispatcher at class scope.
    // Initialize it here, then use it from the background thread.
    var window = Windows.UI.Core.CoreWindow.GetForCurrentThread();
    m_dispatcher = window.Dispatcher;

    Task.Run( () =>
    {
        if (ToastCompletedEvent != null)
        {
            m_dispatcher.RunAsync(CoreDispatcherPriority.Normal,
            new DispatchedHandler(() =>
            {
                this.OnToastCompleted(toast);
            })); // end m_dispatcher.RunAsync
         }
     }); // end Task.Run
}
```

## <a name="option-2-use-eventhandlerltobjectgt-but-lose-type-information"></a>(Opción 2) Usar EventHandler&lt;Object&gt; y perder información de tipo


Otra forma de enviar un evento desde un subproceso en segundo plano es usar [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt; como el tipo de evento. Windows proporciona esta creación de instancias concreta del tipo genérico y proporciona un proxy y un código auxiliar para esta. El inconveniente es que se pierde la información de tipo de tus argumentos de evento y el remitente. Los clientes de C++ y. NET deben saber a través de la documentación qué tipo recuperar cuando se recibe el evento. Los clientes de JavaScript no necesitan la información de tipo original. Encuentran las propiedades de los argumentos en función de sus nombres en los metadatos.

En este ejemplo se muestra cómo usar Windows.Foundation.EventHandler&lt;Object&gt; en C#:

```csharp
public sealed Class1
{
// Declare the event
public event EventHandler<Object> ToastCompletedEvent;

    // Raise the event
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message);
        // Fire the event from a background thread to allow this thread to continue
        Task.Run(() =>
        {
            if (ToastCompletedEvent != null)
            {
                OnToastCompleted(toast);
            }
        });
    }

    private void OnToastCompleted(Toast args)
    {
        var completedEvent = ToastCompletedEvent;
        if (completedEvent != null)
        {
            completedEvent(this, args);
        }
    }
}
```

Este evento se consume en el lado de JavaScript de la forma siguiente:

```javascript
toastCompletedEventHandler: function (event) {
   var toastType = event.toast.toastType;
   document.getElementById("toasterOutput").innerHTML = "<p>Made " + toastType + " toast</p>";
}
```

## <a name="option-3-create-your-own-proxy-and-stub"></a>(Opción 3) Crear tu propio proxy y código auxiliar


Para posibles mejoras de rendimiento en los tipos de eventos definidos por el usuario que tienen información de tipo totalmente conservada, tienes que crear tus propios objetos proxy y de código auxiliar e incrustarlos en el paquete de la aplicación. Por lo general, tienes que usar esta opción solo en raras ocasiones donde ninguna de las dos opciones sea adecuada. Además, no hay ninguna garantía de que esta opción proporcione un mejor rendimiento que las otras dos opciones. El rendimiento real depende de muchos factores. Usar el generador de perfiles de Visual Studio u otras herramientas de generación de perfiles para determinar el rendimiento real de tu aplicación y si el evento es en realidad un cuello de botella.

En el resto de este artículo se muestra cómo usar C# para crear un componente básico de Windows Runtime y, a continuación, usar C++ para crear un archivo DLL para que el proxy y el código auxiliar que habilitarán JavaScript puedan consumir un evento Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt; generado por el componente en una operación asincrónica. (También puedes usar C++ o Visual Basic para crear el componente. Los pasos que se relacionan con la creación de los servidores proxy y código auxiliar son los mismos). Este tutorial se basa en la creación de una muestra de componente en el proceso de Windows Runtime (C++ / CX) y ayuda a explica sus objetivos.

Este tutorial tiene las siguientes partes:

-   Aquí crearás dos clases básicas de Windows Runtime. Una clase expone un evento de tipo [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) y la otra clase es el tipo que se devuelve a JavaScript como argumento para TValue. Estas clases no se pueden comunicar con JavaScript hasta que completes los pasos posteriores.
-   Esta aplicación activa el objeto de clase principal, llama a un método y controla un evento generado por el componente de Windows Runtime.
-   Estos son necesarios para las herramientas que generan las clases de proxy y código auxiliar.
-   A continuación, usas el archivo IDL para generar el código fuente C para el proxy y el código auxiliar.
-   Registra los objetos proxy-código auxiliar para que el tiempo de ejecución de COM pueda encontrarlos y haz referencia a la DLL de proxy-código auxiliar en el proyecto de aplicación.

## <a name="to-create-the-windows-runtime-component"></a>Para crear el componente de Windows Runtime

En la barra de menús de Visual Studio, elige **Archivo &gt; Nuevo proyecto**. En el cuadro de diálogo **Nuevo proyecto** , expande **JavaScript &gt; Universal de Windows** y, a continuación, selecciona **Aplicación vacía**. Nombra el proyecto ToasterApplication y después selecciona el botón **Aceptar**.

Agregar un componente de Windows Runtime de C# a la solución: en el Explorador de soluciones, abre el menú contextual de la solución y, a continuación, elige **Agregar &gt; Nuevo proyecto**. Expande **Visual C# &gt; Tienda Windows** y, a continuación, selecciona **Componente de Windows Runtime**. Asigna al proyecto el nombre de ToasterComponent y después selecciona el botón **Aceptar** . ToasterComponent será el espacio de nombres de raíz para los componentes que crearás en pasos posteriores.

En el Explorador de soluciones, abre el menú contextual para la solución y, a continuación, elige **Propiedades**. En el cuadro de diálogo **Páginas de propiedades**, selecciona **Propiedades de configuración** en el panel izquierdo y luego, en la parte superior del cuadro de diálogo, establece **Configuración** en **Depurar** y **Plataforma** en x86, x64 o ARM. Elige el botón **Aceptar**.

**Importante**: Plataforma = cualquier CPU no funcionará porque no es válido para la DLL de Win32 de código nativo que agregarás a la solución más adelante.

En el Explorador de soluciones, cambia el nombre class1.cs por ToasterComponent.cs para que coincida con el nombre del proyecto. Visual Studio cambia automáticamente el nombre de la clase en el archivo para que coincida con el nuevo nombre de archivo.

En el archivo .cs, agrega una directiva using para el espacio de nombres Windows.Foundation para introducir TypedEventHandler en el ámbito.

Cuando necesitas proxies y códigos auxiliares, tu componente debe utilizar interfaces para exponer sus miembros públicos. En ToasterComponent.cs, define una interfaz para el notificador y otra para la notificación del sistema que produce el notificador.

**Nota**: En C# puedes omitir este paso. En su lugar, crea primero una clase y, a continuación, abre su menú contextual y elige **Refactorizar &gt; Extraer interfaz**. En el código que se genera, otorga manualmente acceso público a las interfaces.

```csharp
    public interface IToaster
        {
            void MakeToast(String message);
            event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

        }
        public interface IToast
        {
            String ToastType { get; }
        }
```

La interfaz de IToast tiene una cadena que se puede recuperar para describir el tipo de notificación del sistema. La interfaz de IToaster tiene un método para realizar la notificación del sistema y un evento para indicar que se realiza la notificación del sistema. Dado que este evento devuelve la parte determinada (es decir, el tipo) de la notificación del sistema, se conoce como un evento tipado.

A continuación, necesitamos clases que implementen estas interfaces y sean públicas y estén selladas de modo que sean accesibles desde la aplicación de JavaScript que programarás más adelante.

```csharp
    public sealed class Toast : IToast
        {
            private string _toastType;

            public string ToastType
            {
                get
                {
                    return _toastType;
                }
            }
            internal Toast(String toastType)
            {
                _toastType = toastType;
            }

        }
        public sealed class Toaster : IToaster
        {
            public event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

            private void OnToastCompleted(Toast args)
            {
                var completedEvent = ToastCompletedEvent;
                if (completedEvent != null)
                {
                    completedEvent(this, args);
                }
            }

            public void MakeToast(string message)
            {
                Toast toast = new Toast(message);
                // Fire the event from a thread-pool thread to enable this thread to continue
                Windows.System.Threading.ThreadPool.RunAsync(
                (IAsyncAction action) =>
                {
                    if (ToastCompletedEvent != null)
                    {
                        OnToastCompleted(toast);
                    }
                });
           }
        }
```

En el código anterior, creamos la notificación del sistema y, a continuación, hacemos girar un elemento de trabajo del grupo de subprocesos para iniciar la notificación. Aunque el IDE podría sugerir que apliques la palabra clave "await" para la llamada asincrónica, no es necesario en este caso porque el método no realiza ningún trabajo que dependa de los resultados de la operación.

**Nota**: La llamada asincrónica en el código anterior usa ThreadPool.RunAsync únicamente para mostrar un método sencillo de desencadenar el evento en un subproceso en segundo plano. Este método en concreto se podría escribir como se muestra en el ejemplo siguiente y funcionaría correctamente porque el programador de tareas de .NET automáticamente calcula la referencia de las llamadas asincrónicas y "await" al subproceso de IU.
  
````csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

If you build the project now, it should build cleanly.

## To program the JavaScript app

Now we can add a button to the JavaScript app to cause it to use the class we just defined to make toast. Before we do this, we must add a reference to the ToasterComponent project we just created. In Solution Explorer, open the shortcut menu for the ToasterApplication project, choose **Add &gt; References**, and then choose the **Add New Reference** button. In the Add Reference dialog box, in the left pane under Solution, select the component project, and then in the middle pane, select ToasterComponent. Choose the **OK** button.

In Solution Explorer, open the shortcut menu for the ToasterApplication project and then choose **Set as Startup Project**.

At the end of the default.js file, add a namespace to contain the functions to call the component and be called back by it. The namespace will have two functions, one to make toast and one to handle the toast-complete event. The implementation of makeToast creates a Toaster object, registers the event handler, and makes the toast. So far, the event handler doesn’t do much, as shown here:

```javascript
    WinJS.Namespace.define("ToasterApplication"), {
       makeToast: function () {

          var toaster = new ToasterComponent.Toaster();
          //toaster.addEventListener("ontoastcompletedevent", ToasterApplication.toastCompletedEventHandler);
          toaster.ontoastcompletedevent = ToasterApplication.toastCompletedEventHandler;
          toaster.makeToast("Peanut Butter");
       },

       toastCompletedEventHandler: function(event) {
           // The sender of the event (the delegate's first type parameter)
           // is mapped to event.target. The second argument of the delegate
           // is contained in event, which means in this case event is a
           // Toast class, with a toastType string.
           var toastType = event.toastType;

           document.getElementById('toastOutput').innerHTML = "<p>Made " + toastType + " toast</p>";
        },
    });
```

The makeToast function must be hooked up to a button. Update default.html to include a button and some space to output the result of making toast:

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

If we weren’t using a TypedEventHandler, we would now be able to run the app on the local machine and click the button to make toast. But in our app, nothing happens. To find out why, let’s debug the managed code that fires the ToastCompletedEvent. Stop the project, and then on the menu bar, choose **Debug &gt; Toaster Application properties**. Change **Debugger Type** to **Managed Only**. Again on the menu bar, choose **Debug &gt; Exceptions**, and then select **Common Language Runtime Exceptions**.

Now run the app and click the make-toast button. The debugger catches an invalid cast exception. Although it’s not obvious from its message, this exception is occurring because proxies are missing for that interface.

![missing proxy](./images/debuggererrormissingproxy.png)

The first step in creating a proxy and stub for a component is to add a unique ID or GUID to the interfaces. However, the GUID format to use differs depending on whether you're coding in C#, Visual Basic, or another .NET language, or in C++.

## To generate GUIDs for the component's interfaces (C# and other .NET languages)

On the menu bar, choose Tools &gt; Create GUID. In the dialog box, select 5. \[Guid(“xxxxxxxx-xxxx...xxxx)\]. Choose the New GUID button and then choose the Copy button.

![guid generator tool](./images/guidgeneratortool.png)

Go back to the interface definition, and then paste the new GUID just before the IToaster interface, as shown in the following example. (Don't use the GUID in the example. Every unique interface should have its own GUID.)

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

Add a using directive for the System.Runtime.InteropServices namespace.

Repeat these steps for the IToast interface.

## To generate GUIDs for the component's interfaces (C++)

On the menu bar, choose Tools &gt; Create GUID. In the dialog box, select 3. static const struct GUID = {...}. Choose the New GUID button and then choose the Copy button.

Paste the GUID just before the IToaster interface definition. After you paste, the GUID should resemble the following example. (Don't use the GUID in the example. Every unique interface should have its own GUID.)
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
Add a using directive for Windows.Foundation.Metadata to bring GuidAttribute into scope.

Now manually convert the const GUID to a GuidAttribute so that it's formatted as shown in the following example. Notice that the curly braces are replaced with brackets and parentheses, and the trailing semicolon is removed.
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
Repeat these steps for the IToast interface.

Now that the interfaces have unique IDs, we can create an IDL file by feeding the .winmd file into the winmdidl command-line tool, and then generate the C source code for the proxy and stub by feeding that IDL file into the MIDL command-line tool. Visual Studio do this for us if we create post-build events as shown in the following steps.

## To generate the proxy and stub source code

To add a custom post-build event, in Solution Explorer, open the shortcut menu for the ToasterComponent project and then choose Properties. In the left pane of the property pages, select Build Events, and then choose the Edit Post-build button. Add the following commands to the post-build command line. (The batch file must be called first to set the environment variables to find the winmdidl tool.)
```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```
**Important**  For an ARM or x64 project configuration, change the MIDL /env parameter to x64 or arm32.

To make sure the IDL file is regenerated every time the .winmd file is changed, change **Run the post-build event** to **When the build updates the project output.**
The Build Events property page should resemble this:
![build events](./images/buildevents.png)

Rebuild the solution to generate and compile the IDL.

You can verify that MIDL correctly compiled the solution by looking for ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c, and dlldata.c in the ToasterComponent project directory.

## To compile the proxy and stub code into a DLL

Now that you have the required files, you can compile them to produce a DLL, which is a C++ file. To make this as easy as possible, add a new project to support building the proxies. Open the shortcut menu for the ToasterApplication solution and then choose **Add > New Project**. In the left pane of the **New Project** dialog box, expand **Visual C++ &gt; Windows &gt; Univeral Windows**, and then in the middle pane, select **DLL (Windows Store apps)**. (Notice that this is NOT a C++ Windows Runtime Component project.) Name the project Proxies and then choose the **OK** button. These files will be updated by the post-build events when something changes in the C# class.

By default, the Proxies project generates header .h files and C++ .cpp files. Because the DLL is built from the files produced from MIDL, the .h and .cpp files are not required. In Solution Explorer, open the shortcut menu for them, choose **Remove**, and then confirm the deletion.

Now that the project is empty, you can add back the MIDL-generated files. Open the shortcut menu for the Proxies project, and then choose **Add > Existing Item.** In the dialog box, navigate to the ToasterComponent project directory and select these files: ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c, and dlldata.c files. Choose the **Add** button.

In the Proxies project, create a .def file to define the DLL exports described in dlldata.c. Open the shortcut menu for the project, and then choose **Add > New Item**. In the left pane of the dialog box, select Code and then in the middle pane, select Module-Definition File. Name the file proxies.def and then choose the **Add** button. Open this .def file and modify it to include the EXPORTS that are defined in dlldata.c:

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

If you build the project now, it will fail. To correctly compile this project, you have to change how the project is compiled and linked. In Solution Explorer, open the shortcut menu for the Proxies project and then choose **Properties**. Change the property pages as follows.

In the left pane, select **C/C++ > Preprocessor**, and then in the right pane, select **Preprocessor Definitions**, choose the down-arrow button, and then select **Edit**. Add these definitions in the box:

```cpp
WIN32;_WINDOWS
```
Under **C/C++ > Precompiled Headers**, change **Precompiled Header** to **Not Using Precompiled Headers**, and then choose the **Apply** button.

Under **Linker > General**, change **Ignore Import Library** to **Ye**s, and then choose the **Apply** button.

Under **Linker > Input**, select **Additional Dependencies**, choose the down-arrow button, and then select **Edit**. Add this text in the box:

```cpp
rpcrt4.lib;runtimeobject.lib
```

Do not paste these libs directly into the list row. Use the **Edit** box to ensure that MSBuild in Visual Studio will maintain the correct additional dependencies.

When you have made these changes, choose the **OK** button in the **Property Pages** dialog box.

Next, take a dependency on the ToasterComponent project. This ensures that the Toaster will build before the proxy project builds. This is required because the Toaster project is responsible for generating the files to build the proxy.

Open the shortcut menu for the Proxies project and then choose Project Dependencies. Select the check boxes to indicate that the Proxies project depends on the ToasterComponent project, to ensure that Visual Studio builds them in the correct order.

Verify that the solution builds correctly by choosing **Build > Rebuild Solution** on the Visual Studio menu bar.


## To register the proxy and stub

In the ToasterApplication project, open the shortcut menu for package.appxmanifest and then choose **Open With**. In the Open With dialog box, select **XML Text Editor** and then choose the **OK** button. We're going to paste in some XML that provides a windows.activatableClass.proxyStub extension registration and which are based on the GUIDs in the proxy. To find the GUIDs to use in the .appxmanifest file, open ToasterComponent_i.c. Find entries that resemble the ones in the following example. Also notice the definitions for IToast, IToaster, and a third interface—a typed event handler that has two parameters: a Toaster and Toast. This matches the event that's defined in the Toaster class. Notice that the GUIDs for IToast and IToaster match the GUIDs that are defined on the interfaces in the C# file. Because the typed event handler interface is autogenerated, the GUID for this interface is also autogenerated.

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);


MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);


MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

Now we copy the GUIDs, paste them in package.appxmanifest in a node that we add and name Extensions, and then reformat them. The manifest entry resembles the following example—but again, remember to use your own GUIDs. Notice that the ClassId GUID in the XML is the same as ITypedEventHandler2. This is because that GUID is the first one that's listed in ToasterComponent_i.c. The GUIDs here are case-insensitive. Instead of manually reformatting the GUIDs for IToast and IToaster, you can go back into the interface definitions and get the GuidAttribute value, which has the correct format. In C++, there is a correctly-formatted GUID in the comment. In any case, you must manually reformat the GUID that's used for both the ClassId and the event handler.

```cpp
      <Extensions> <!--Use your own GUIDs!!!-->
        <Extension Category="windows.activatableClass.proxyStub">
          <ProxyStub ClassId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d">
            <Path>Proxies.dll</Path>
            <Interface Name="IToast" InterfaceId="F8D30778-9EAF-409C-BCCD-C8B24442B09B"/>
            <Interface Name="IToaster"  InterfaceId="E976784C-AADE-4EA4-A4C0-B0C2FD1307C3"/>  
            <Interface Name="ITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast" InterfaceId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d"/>
          </ProxyStub>      
        </Extension>
      </Extensions>
```

Paste the Extensions XML node as a direct child of the Package node, and a peer of, for example, the Resources node.

Before moving on, it’s important to ensure that:

-   The ProxyStub ClassId is set to the first GUID in the ToasterComponent\_i.c file. Use the first GUID that's defined in this file for the classId. (This might be the same as the GUID for ITypedEventHandler2.)
-   The Path is the package relative path of the proxy binary. (In this walkthrough, proxies.dll is in the same folder as ToasterApplication.winmd.)
-   The GUIDs are in the correct format. (This is easy to get wrong.)
-   The interface IDs in the manifest match the IIDs in ToasterComponent\_i.c file.
-   The interface names are unique in the manifest. Because these are not used by the system, you can choose the values. It is a good practice to choose interface names that clearly match interfaces that you have defined. For generated interfaces, the names should be indicative of the generated interfaces. You can use the ToasterComponent\_i.c file to help you generate interface names.

If you try to run the solution now, you will get an error that proxies.dll is not part of the payload. Open the shortcut menu for the **References** folder in the ToasterApplication project and then choose **Add Reference**. Select the check box next to the Proxies project. Also, make sure that the check box next to ToasterComponent is also selected. Choose the **OK** button.

The project should now build. Run the project and verify that you can make toast.

## Related topics

* [Creating Windows Runtime Components in C++](creating-windows-runtime-components-in-cpp.md)
