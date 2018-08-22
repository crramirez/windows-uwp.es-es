---
author: msatranjr
title: Generación de eventos en componentes de Windows Runtime
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: Cómo generar un evento de un tipo definido por el usuario delegado en un subproceso en segundo plano para que pueda recibir el evento JavaScript.
ms.author: misatran
ms.date: 07/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 89c021bb2c094aafc9b534acef9b009817669461
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "2801205"
---
# <a name="raising-events-in-windows-runtime-components"></a>Generación de eventos en componentes de Windows Runtime
> [!NOTE]
> Para obtener información sobre cómo generar eventos en una [C + + / WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) componente de tiempo de ejecución de Windows, vea [crear eventos en C + + / WinRT](../cpp-and-winrt-apis/author-events.md).

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

Agregar un componente de Windows Runtime de C# a la solución: en el Explorador de soluciones, abre el menú contextual de la solución y, a continuación, elige **Agregar &gt; Nuevo proyecto**. Expanda **Visual C# &gt; Microsoft Store** y, a continuación, seleccione el **Componente de tiempo de ejecución de Windows**. Asigna al proyecto el nombre de ToasterComponent y después selecciona el botón **Aceptar** . ToasterComponent será el espacio de nombres de raíz para los componentes que crearás en pasos posteriores.

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
  
```csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

Si genera el proyecto ahora, debe generar sin errores.

## <a name="to-program-the-javascript-app"></a>Para programar la aplicación de JavaScript

Ahora podemos agregar un botón a la aplicación de JavaScript para hacer que utilice la clase que se acaba de definir para hacer que una notificación del sistema. Antes de que lo hacemos, debemos agregar una referencia al proyecto de ToasterComponent que acabamos de crear. En el Explorador de soluciones, abra el menú contextual del proyecto ToasterApplication, elija **Agregar &gt; referencias**y, a continuación, elija el botón **Agregar nueva referencia** . En el cuadro de diálogo Agregar referencia, en el panel izquierdo en la solución, seleccione el proyecto del componente y, a continuación, en el panel central, seleccione ToasterComponent. Elige el botón **Aceptar**.

En el Explorador de soluciones, abra el menú contextual para el proyecto ToasterApplication y, a continuación, elija **establecer como proyecto de inicio**.

Al final del archivo default.js, agregue un espacio de nombres para que contenga las funciones para llamar al componente y recibir la llamada por ella. El espacio de nombres tendrá dos funciones, uno para realizar una notificación del sistema y otro para controlar el evento complete una notificación del sistema. La implementación de makeToast crea un objeto Toaster, registra el controlador de eventos y hace que la notificación del sistema. Hasta ahora, el controlador de eventos no hace mucho, como se muestra aquí:

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

La función makeToast se debe enlazar a un botón. Actualizar default.html para incluir un botón y algo de espacio para generar el resultado de realizar una notificación del sistema:

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

Si se no estábamos utilizando un TypedEventHandler, ahora se sería capaces de ejecutar la aplicación en el equipo local y haga clic en el botón para hacer una notificación del sistema. Pero, en nuestra aplicación, no ocurre nada. Para conocer el motivo, vamos a depurar el código administrado que se desencadena la ToastCompletedEvent. Detenga el proyecto y, a continuación, en la barra de menús, elija **Depurar &gt; propiedades de la aplicación Toaster**. Cambie el **tipo de depurador** a **Sólo administrado**. Nuevo en la barra de menús, elija **Depurar &gt; excepciones**y, a continuación, seleccione **Excepciones de Common Language Runtime**.

Ahora, ejecute la aplicación y haga clic en el botón hacer-notificación del sistema. El depurador detecta una excepción de conversión no válido. Aunque no es evidente de su mensaje, esta excepción se produce porque faltan los servidores proxy para la interfaz.

![proxy que faltan](./images/debuggererrormissingproxy.png)

El primer paso en la creación de un proxy y código auxiliar de un componente es agregar un GUID o un identificador único para las interfaces. Sin embargo, el formato GUID para usar varía en función de si está escribiendo código en C#, Visual Basic u otro lenguaje. NET, o en C++.

## <a name="to-generate-guids-for-the-components-interfaces-c-and-other-net-languages"></a>Para generar los GUID para las interfaces del componente (C# y otros lenguajes de. NET)

En la barra de menús, elija herramientas &gt; crear GUID. En el cuadro de diálogo, seleccione 5. \[GUID ("xxxxxxxx-xxxx... xxxx) \]. Elija el botón nuevo GUID y, a continuación, elija el botón Copiar.

![herramienta Generador de GUID](./images/guidgeneratortool.png)

Vuelva a la definición de interfaz y, a continuación, pegue el nuevo GUID justo antes de la interfaz IToaster, tal como se muestra en el siguiente ejemplo. (No utilice el GUID en el ejemplo. Cada interfaz único debe tener su propio GUID).

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

Agregue una directiva using para el espacio de nombres System.Runtime.InteropServices.

Repita estos pasos para la interfaz IToast.

## <a name="to-generate-guids-for-the-components-interfaces-c"></a>Para generar los GUID para las interfaces del componente (C++)

En la barra de menús, elija herramientas &gt; crear GUID. En el cuadro de diálogo, seleccione 3. struct const estático GUID = {...}. Elija el botón nuevo GUID y, a continuación, elija el botón Copiar.

Pegue el GUID justo antes de la definición de interfaz IToaster. Después de pegar, el GUID debe parecerse al ejemplo siguiente. (No utilice el GUID en el ejemplo. Cada interfaz único debe tener su propio GUID).
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
Agregue una directiva using para Windows.Foundation.Metadata incorporar GuidAttribute en ámbito.

Ahora convertir manualmente el GUID const a un GuidAttribute de modo que tenga el formato tal como se muestra en el siguiente ejemplo. Tenga en cuenta que las llaves se reemplazan con corchetes y paréntesis, y se quita el punto y coma final.
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
Repita estos pasos para la interfaz IToast.

Ahora que las interfaces tienen identificadores únicos, podemos crear un archivo IDL por el archivo .winmd en la alimentación a la herramienta de línea de comandos winmdidl y, a continuación, genere el código de origen C para el proxy y código auxiliar mediante la alimentación ese archivo IDL en la herramienta de línea de comandos de MIDL. Visual Studio hacer esto por nosotros Si creamos eventos posteriores a la compilación, como se muestra en los siguientes pasos.

## <a name="to-generate-the-proxy-and-stub-source-code"></a>Para generar al proxy y código auxiliar de código fuente

Para agregar un evento posterior a la compilación personalizado, en el Explorador de soluciones, abra el menú contextual para el proyecto ToasterComponent y, a continuación, elija Propiedades. En el panel izquierdo de las páginas de propiedades, seleccione eventos de generación y, a continuación, elija el botón Editar posteriores a la compilación. Agregue los siguientes comandos en la línea de comandos posterior a la compilación. (El archivo por lotes primero se debe llamar para establecer las variables de entorno para buscar la herramienta winmdidl.)

```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```

**Importante**  Para un ARM o x64 configuración de proyecto, cambie el parámetro de /env MIDL a x64 o arm32.

Para asegurarse de que el archivo IDL se vuelve a generar cada vez que se modifica el archivo .winmd, cambiar **ejecutar el evento posterior a la compilación** a **cuando la compilación actualiza el resultado del proyecto.**
La página de propiedades eventos de generación debe ser similar a esto: ![eventos de compilación](./images/buildevents.png)

Volver a generar la solución para generar y compilar el archivo IDL.

Puede comprobar que MIDL compilado correctamente la solución mediante la búsqueda de ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c y dlldata.c en el directorio del proyecto ToasterComponent.

## <a name="to-compile-the-proxy-and-stub-code-into-a-dll"></a>Para compilar al proxy y código auxiliar código en un archivo DLL

Ahora que tiene los archivos necesarios, se puede compilar para generar un archivo DLL, que es un archivo de C++. Para hacer esto tan fácil como sea posible, agregue un nuevo proyecto para admitir la creación de los servidores proxy. Abra el menú contextual de la solución ToasterApplication y, a continuación, elija **Agregar > Nuevo proyecto**. En el panel izquierdo del cuadro de diálogo **Nuevo proyecto** , expanda **Visual C++ &gt; Windows &gt; Windows universal**y, a continuación, en el panel central, seleccione **archivo DLL (UWP aplicaciones)**. (Tenga en cuenta que esto no es un proyecto de componente de tiempo de ejecución de Windows de C++). Nombre del proyecto los servidores proxy y, a continuación, elija el botón **Aceptar** . Estos archivos se actualizarán por los eventos posteriores a la compilación cuando cambia algo en la clase de C#.

De forma predeterminada, el proyecto de los servidores proxy genera archivos de encabezado. h y archivos .cpp de C++. Dado que se basa el archivo DLL desde los archivos producidos a partir de MIDL, los archivos .h y .cpp no son necesarios. En el Explorador de soluciones, abra el menú contextual para ellos, elija **Quitar**y, a continuación, confirme la eliminación.

Ahora que el proyecto está vacío, puede volver a agregar los archivos generados por MIDL. Abra el menú contextual del proyecto de los servidores proxy y, a continuación, elija **Agregar > elemento existente.** En el cuadro de diálogo, desplácese hasta el directorio del proyecto ToasterComponent y seleccione estos archivos: los archivos ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c y dlldata.c. Elija el botón **Agregar** .

En el proyecto de los servidores proxy, cree un archivo def para definir las exportaciones DLL que se describen en dlldata.c. Abra el menú contextual para el proyecto y, a continuación, elija **Agregar > nuevo elemento**. En el panel izquierdo del cuadro de diálogo, seleccione el código y, a continuación, en el panel central, seleccione archivo de definición de módulo. Nombre del archivo proxies.def y, a continuación, elija el botón **Agregar** . Abra este archivo def y modificar para que incluya las exportaciones que se definen en dlldata.c:

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

Si genera el proyecto ahora, se producirá un error. Para compilar correctamente este proyecto, tendrá que cambiar la forma en que el proyecto se compila y vinculado. En el Explorador de soluciones, abra el menú contextual del proyecto de los servidores proxy y, a continuación, elija **Propiedades**. Cambiar las páginas de propiedades de la siguiente manera.

En el panel izquierdo, seleccione **C o C++ > preprocesador**y, a continuación, en el panel derecho, seleccione **Las definiciones del preprocesador**, elija el botón de flecha abajo y, a continuación, seleccione **Editar**. Agregue estas definiciones en el cuadro:

```cpp
WIN32;_WINDOWS
```
Bajo **C o C++ > precompilado encabezados**, cambiar el **Encabezado precompilado** en **No utilizar encabezados precompilado**y, a continuación, elija el botón **Aplicar** .

Bajo **vinculador > General**, cambiar **Omitir bibliotecas de importación** a **Ye**s y, a continuación, elija el botón **Aplicar** .

Bajo **vinculador > Entrada**, seleccione **Dependencias adicionales**, elija el botón de flecha hacia abajo y, a continuación, seleccione **Editar**. Agregue este texto en el cuadro:

```cpp
rpcrt4.lib;runtimeobject.lib
```

No se pega estas bibliotecas directamente en la fila de la lista. Use el cuadro de **Edición** para garantizar que MSBuild en Visual Studio se mantienen las dependencias adicionales correctas.

Cuando haya realizado los cambios, elija el botón **Aceptar** en el cuadro de diálogo **Páginas de propiedades** .

A continuación, tomar una dependencia en el proyecto ToasterComponent. Esto garantiza que la Toaster generará antes de genera el proyecto de proxy. Esto es necesario porque el proyecto Toaster es responsable de generar los archivos para crear al proxy.

Abra el menú contextual del proyecto de los servidores proxy y, a continuación, elija dependencias del proyecto. Seleccione las casillas de verificación para indicar que depende el proyecto de los servidores proxy en el proyecto ToasterComponent, para asegurarse de que Visual Studio las compilaciones en el orden correcto.

Compruebe que la solución se genera correctamente eligiendo **crear > volver a generar solución** en la barra de menús de Visual Studio.


## <a name="to-register-the-proxy-and-stub"></a>Para registrar el proxy y código auxiliar

En el proyecto ToasterApplication, abra el menú contextual para package.appxmanifest y, a continuación, elija **Abrir con**. En el cuadro de diálogo Abrir con, seleccione **Editor de texto XML** y, a continuación, elija el botón **Aceptar** . Vamos a pegar en XML que proporciona que un registro de la extensión de windows.activatableClass.proxyStub y que se basan en los GUID en el servidor proxy. Para buscar los GUID para usar en el archivo .appxmanifest, abra ToasterComponent_i.c. Busque las entradas similares a las que en el ejemplo siguiente. Tenga en cuenta también las definiciones de IToast, IToaster y una tercera interfaz: un controlador de eventos con tipo que tiene dos parámetros: un Toaster y notificación del sistema. Esto coincide con el evento que se define en la clase Toaster. Tenga en cuenta que los GUID de IToast y IToaster coinciden con los GUID que se definen en las interfaces en el archivo de C#. Dado que la interfaz de controlador de eventos con tipo es generado automáticamente, el GUID de esta interfaz es también generado automáticamente.

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

Ahora se copie los GUID, péguelos en package.appxmanifest en un nodo que se agrega y extensiones de nombre y, a continuación, volver a formatear. La entrada de manifiesto es similar al ejemplo siguiente, pero una vez más, recuerde que debe usar su propio GUID. Tenga en cuenta que el GUID ClassId en el XML es el mismo que ITypedEventHandler2. Esto es debido a que ese GUID es el primero que aparece en ToasterComponent_i.c. Aquí los GUID distinguen mayúsculas de minúsculas. En lugar de formatear manualmente los GUID para IToast y IToaster, puede volver a las definiciones de interfaz y obtener el valor de GuidAttribute, que tiene el formato correcto. En C++, hay un GUID con el formato correcto en el comentario. En cualquier caso, debe formatear manualmente el GUID que se utiliza para la propiedad ClassId y el controlador de eventos.

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

Pegue el nodo XML de las extensiones como un elemento secundario directo del nodo de paquete y un par de, por ejemplo, el nodo de recursos.

Antes de continuar, es importante para asegurarse de que:

-   ProxyStub ClassId se establece en el primer GUID en el archivo ToasterComponent\_i.c. Use el primer GUID que se define en este archivo para la propiedad classId. (Puede ser el mismo que el GUID de ITypedEventHandler2.)
-   La ruta de acceso es la ruta de acceso relativa de paquete del proxy binario. (En este tutorial, proxies.dll está en la misma carpeta que ToasterApplication.winmd).
-   Los GUID se encuentran en el formato correcto. (Esto es fácil obtener incorrecto).
-   El ID de interfaz en el manifiesto coincide con el IID en el archivo ToasterComponent\_i.c.
-   Los nombres de interfaz son únicos en el manifiesto. Debido a que no se utilizan por el sistema, puede elegir los valores. Es una práctica recomendada para elegir los nombres de interfaz que claramente coinciden con interfaces que haya definido. Para las interfaces generadas, los nombres deben ser indicativos de las interfaces generadas. Puede usar el archivo ToasterComponent\_i.c que le ayudarán a generar los nombres de interfaz.

Si intenta ejecutar la solución ahora, obtendrá un error que proxies.dll no forma parte de la carga. Abra el menú contextual para la carpeta **referencias** en el proyecto ToasterApplication y, a continuación, seleccione **Agregar referencia**. Seleccione la casilla de verificación situada junto al proyecto de los servidores proxy. Además, asegúrese de que también está activada la casilla de verificación situada junto a ToasterComponent. Elige el botón **Aceptar**.

Ahora debe generar el proyecto. Ejecute el proyecto y compruebe que puede hacer que una notificación del sistema.

## <a name="related-topics"></a>Temas relacionados

* [Crear componentes de Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md)
