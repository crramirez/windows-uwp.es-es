---
title: Generación de eventos en componentes de Windows Runtime
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: Cómo generar un evento de un tipo delegado definido por el usuario en un subproceso en segundo plano para que JavaScript pueda recibir el evento.
ms.date: 07/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 084f9d17c773bbbfa0234d0093f0691583189c15
ms.sourcegitcommit: 8008b7820793fa58b13e2938bf49969b3e692b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81688204"
---
# <a name="raising-events-in-windows-runtime-components"></a>Generación de eventos en componentes de Windows Runtime

> [!NOTE]
> Para obtener más información sobre la generación de eventos en un componente Windows Runtime de [c++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) , vea [crear eventos en c++/WinRT](../cpp-and-winrt-apis/author-events.md).

Si el componente de Windows Runtime genera un evento de un tipo delegado definido por el usuario en un subproceso en segundo plano (subproceso de trabajo) y desea que JavaScript pueda recibir el evento, puede implementarlo y/o generarlo de alguna de estas maneras.

-   (Opción 1) Genere el evento a través de [**Windows. UI. Core. CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher) para calcular las referencias del evento en el contexto del subproceso de JavaScript. Aunque esta suele ser la mejor opción, en algunos escenarios es posible que no proporcione el rendimiento más rápido.
-   (Opción 2) Use el **objeto\<\> [Windows. Foundation. EventHandler](/uwp/api/windows.foundation.eventhandler-1)** (pero pierda la información de tipo de evento). Si la opción 1 no es factible, o si su rendimiento no es el adecuado, se trata de una buena opción siempre que se acepte la pérdida de información de tipo. Si va a crear un componente de Windows Runtime de C#, el tipo de **objeto\> Windows.\<Foundation. EventHandler** no está disponible; en su lugar, ese tipo se proyecta en [**System. EventHandler**](/dotnet/api/system.eventhandler), por lo que se debe usar en su lugar.
-   (Opción 3) Crea tu propio proxy y código auxiliar para el componente. Esta opción es la más difícil de implementar, pero conserva la información de tipo y podría proporcionar un mejor rendimiento en comparación con la opción 1 en escenarios exigentes.

Si solo se genera un evento en un subproceso en segundo plano sin usar una de estas opciones, un cliente de JavaScript no recibirá el evento.

## <a name="background"></a>Información previa

Todos los componentes y aplicaciones de Windows Runtime son fundamentalmente objetos COM, independientemente del lenguaje que se usa para crearlos. En la API de Windows, la mayoría de los componentes son objetos COM ágiles que se pueden comunicar igual de bien con los objetos en el subproceso en segundo plano y en el subproceso de IU. Si no se puede realizar un objeto COM ágil, se requerirán objetos auxiliares conocidos como proxies y códigos auxiliares para comunicarse con otros objetos COM a través de los límites del subproceso de IU y el subproceso en segundo plano. (En términos COM, esto se conoce como una comunicación entre subprocesamientos controlados).

La mayoría de los objetos de la API de Windows son ágiles o tienen proxies y códigos auxiliares integrados. Sin embargo, no se pueden crear proxies ni códigos auxiliares para los tipos genéricos como Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](/uwp/api/windows.foundation.typedeventhandler) porque no son tipos completos hasta que se proporciona el argumento de tipo. Solo en el caso de los clientes de JavaScript la falta de proxys o códigos auxiliares se convierte en un problema, pero si deseas que tu componente se pueda usar desde JavaScript así como desde un lenguaje de .NET o C++, a continuación debes usar una de las tres opciones siguientes.

## <a name="option-1-raise-the-event-through-the-coredispatcher"></a>(Opción 1) Generar el evento a través de CoreDispatcher

Puede enviar eventos de cualquier tipo de delegado definido por el usuario mediante [Windows.UI.Core.CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher), y JavaScript podrá recibirlos. Si no estás seguro de qué opción usar, prueba con esta primero. Si la latencia entre la activación de los eventos y el controlador de eventos se convierte en un problema, prueba una de las otras opciones.

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

> [!NOTE]
> Si va a crear un componente de Windows Runtime de C#, el tipo de **objeto\> Windows.\<Foundation. EventHandler** no está disponible; en su lugar, ese tipo se proyecta en [**System. EventHandler**](/dotnet/api/system.eventhandler), por lo que se debe usar en su lugar.

Otra manera de enviar un evento desde un subproceso en segundo plano es usar el objeto&gt; [Windows. Foundation. EventHandler](/uwp/api/windows.foundation.eventhandler)&lt;como el tipo del evento. Windows proporciona esta creación de instancias concreta del tipo genérico y proporciona un proxy y un código auxiliar para esta. El inconveniente es que se pierde la información de tipo de tus argumentos de evento y el remitente. Los clientes de C++ y. NET deben saber a través de la documentación qué tipo recuperar cuando se recibe el evento. Los clientes de JavaScript no necesitan la información de tipo original. Encuentran las propiedades de los argumentos en función de sus nombres en los metadatos.

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

Este tutorial tiene estas partes.

-   Aquí crearás dos clases básicas de Windows Runtime. Una clase expone un evento de tipo [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](/uwp/api/windows.foundation.typedeventhandler-2) y la otra clase es el tipo que se devuelve a JavaScript como argumento para TValue. Estas clases no se pueden comunicar con JavaScript hasta que completes los pasos posteriores.
-   Esta aplicación activa el objeto de clase principal, llama a un método y controla un evento generado por el componente de Windows Runtime.
-   Estos son necesarios para las herramientas que generan las clases de proxy y código auxiliar.
-   A continuación, usas el archivo IDL para generar el código fuente C para el proxy y el código auxiliar.
-   Registra los objetos proxy-código auxiliar para que el tiempo de ejecución de COM pueda encontrarlos y haz referencia a la DLL de proxy-código auxiliar en el proyecto de aplicación.

## <a name="to-create-the-windows-runtime-component"></a>Para crear el componente de Windows Runtime

En la barra de menús de Visual Studio, elija **archivo &gt; nuevo proyecto**. En el cuadro de diálogo **Nuevo proyecto** , expande **JavaScript &gt; Universal de Windows** y, a continuación, selecciona **Aplicación vacía**. Nombra el proyecto ToasterApplication y después selecciona el botón **Aceptar**.

Agregar un componente de Windows Runtime de C# a la solución: en el Explorador de soluciones, abre el menú contextual de la solución y, a continuación, elige **Agregar &gt; Nuevo proyecto**. Expanda **Visual &gt; C# Microsoft Store** y, a continuación, seleccione **Windows Runtime componente**. Asigna al proyecto el nombre de ToasterComponent y después selecciona el botón **Aceptar** . ToasterComponent será el espacio de nombres de raíz para los componentes que crearás en pasos posteriores.

En el Explorador de soluciones, abre el menú contextual para la solución y, a continuación, elige **Propiedades**. En el cuadro de diálogo **Páginas de propiedades**, selecciona **Propiedades de configuración** en el panel izquierdo y luego, en la parte superior del cuadro de diálogo, establece **Configuración** en **Depurar** y **Plataforma** en x86, x64 o ARM. Elija el botón **Aceptar** .

**Plataforma importante** = cualquier CPU no funcionará porque no es válido para el archivo dll de Win32 de código nativo que agregará a la solución más adelante.

En el Explorador de soluciones, cambia el nombre class1.cs por ToasterComponent.cs para que coincida con el nombre del proyecto. Visual Studio cambia automáticamente el nombre de la clase en el archivo para que coincida con el nuevo nombre de archivo.

En el archivo .cs, agrega una directiva using para el espacio de nombres Windows.Foundation para introducir TypedEventHandler en el ámbito.

Cuando necesitas proxies y códigos auxiliares, tu componente debe utilizar interfaces para exponer sus miembros públicos. En ToasterComponent.cs, define una interfaz para el notificador y otra para la notificación del sistema que produce el notificador.

**Nota** en C# puede omitir este paso. En su lugar, crea primero una clase y, a continuación, abre su menú contextual y elige **Refactorizar &gt; Extraer interfaz**. En el código que se genera, otorga manualmente acceso público a las interfaces.

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

**Tenga en cuenta** que la llamada asincrónica del código anterior usa ThreadPool. RunAsync únicamente para mostrar una manera sencilla de desencadenar el evento en un subproceso en segundo plano. Este método en concreto se podría escribir como se muestra en el ejemplo siguiente y funcionaría correctamente porque el programador de tareas de .NET automáticamente calcula la referencia de las llamadas asincrónicas y "await" al subproceso de IU.
  
```csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

Si compilas el proyecto ahora, se debería compilar limpiamente.

## <a name="to-program-the-javascript-app"></a>Para programar la aplicación JavaScript

Ahora puedes agregar un botón a la aplicación JavaScript para que utilice la clase que acabamos de definir para crear el Toast. Antes de hacer esto, debemos agregar una referencia al proyecto ToasterComponent que acabamos de crear. En Explorador de soluciones, abra el menú contextual del proyecto ToasterApplication, elija **agregar &gt; referencias**y, a continuación, elija el botón **Agregar nueva referencia** . En el cuadro de diálogo Agregar referencia, en el panel izquierdo bajo Solución, selecciona el proyecto de componente. A continuación, en el panel central, selecciona ToasterComponent. Elija el botón **Aceptar** .

En Explorador de soluciones, abra el menú contextual del proyecto ToasterApplication y, a continuación, elija **establecer como proyecto de inicio**.

Al final del archivo default.js, agrega un espacio de nombres para las funciones que llamarán al componente y a las que el componente devolverán la llamada. El espacio de nombres tendrá dos funciones, una para crear el Toast y otra para controlar el evento de finalización de Toast. La implementación de makeToast crea un objeto tostador, registra el controlador de eventos y realiza la notificación del sistema. Hasta ahora, el controlador de eventos no hace mucho, tal como se muestra a continuación:

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

La función makeToast se debe enlazar a un botón. Actualice default.html para que incluya un botón y algo de espacio para generar el resultado de crear el Toast:

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

Si no usamos un TypedEventHandler, ahora podremos ejecutar la aplicación en el equipo local y hacer clic en el botón para realizar la notificación del sistema. Pero en nuestra aplicación, no ocurre nada. Para averiguar por qué, vamos a depurar el código administrado que activa el ToastCompletedEvent. Detenga el proyecto y, a continuación, en la barra de menús, elija **depurar &gt; propiedades de la aplicación del tostador**. Cambie **tipo de depurador** a **solo administrado**. De nuevo en la barra de menús, elija **depurar &gt; excepciones**y, a continuación, seleccione **excepciones de Common Language Runtime**.

Ahora ejecuta la aplicación y haz clic en el botón para crear el Toast. El depurador detecta una excepción de conversión no válida. Aunque en el mensaje no resulta evidente, esta excepción se produce porque faltan servidores proxy para esa interfaz.

![falta el proxy](./images/debuggererrormissingproxy.png)

El primer paso para crear un servidor proxy y un código auxiliar para un componente es agregar un identificador o un GUID único a las interfaces. No obstante, el formato del GUID depende de si el código se escribe en C#, Visual Basic, u otro lenguaje de .NET o C++.

## <a name="to-generate-guids-for-the-components-interfaces-c-and-other-net-languages"></a>Para generar GUID para las interfaces del componente (C# y otros lenguajes .NET)

En la barra de menús, elija &gt; herramientas crear GUID. En el cuadro de diálogo, seleccione 5. \[GUID ("xxxxxxxx-xxxx... XXXX ")\]. Elige el botón Nuevo GUID y, a continuación, elige el botón Copiar.

![herramienta Generador de GUID](./images/guidgeneratortool.png)

Vuelva a la definición de la interfaz y, a continuación, pegue el nuevo GUID justo delante de la interfaz IToaster, tal como se muestra en el ejemplo siguiente. (No use el GUID del ejemplo. Cada interfaz debe tener su propio GUID).

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

Agregue una directiva using para el espacio de nombres System. Runtime. InteropServices.

Repita estos pasos para la interfaz IToast.

## <a name="to-generate-guids-for-the-components-interfaces-c"></a>Para generar GUID para las interfaces del componente (C++)

En la barra de menús, elija &gt; herramientas crear GUID. En el cuadro de diálogo, seleccione 3. Static const struct GUID = {...}. Elige el botón Nuevo GUID y, a continuación, elige el botón Copiar.

Pegue el GUID justo delante de la definición de la interfaz IToaster. Después de pegarlo, el GUID debe parecerse al del ejemplo siguiente. (No use el GUID del ejemplo. Cada interfaz debe tener su propio GUID).
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
Agregue una directiva using para Windows. Foundation. Metadata para traer GuidAttribute en el ámbito.

Ahora convierte manualmente el const GUID en un GuidAttribute para darle formato, tal como se muestra en el ejemplo siguiente. Observe que las llaves se reemplazan por corchetes y paréntesis, y que se quita el punto y coma final.
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
Repita estos pasos para la interfaz IToast.

Ahora que las interfaces tienen identificadores únicos, podemos crear un archivo IDL alimentando el archivo. winmd en la herramienta de línea de comandos winmdidl y, a continuación, generar el código fuente de C para el proxy y el código auxiliar alimentando ese archivo IDL en la herramienta de línea de comandos MIDL. Visual Studio hace esto automáticamente si creamos eventos posteriores a la compilación, tal como se muestra en los pasos siguientes.

## <a name="to-generate-the-proxy-and-stub-source-code"></a>Para generar el código fuente del servidor proxy y del código auxiliar

Para agregar un evento posterior a la compilación personalizado, en el Explorador de soluciones, abre el menú contextual del proyecto ToasterComponent y, a continuación, elige Propiedades. En el panel izquierdo de las páginas de propiedades, selecciona Eventos de compilación y, a continuación, elige el botón Edición posterior a la compilación. Agrega los comandos siguientes a la línea de comandos de ejecución posterior a la compilación. (Primero se debe llamar al archivo por lotes para establecer las variables de entorno para encontrar la herramienta winmdidl).

```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```

**Importante**  para una configuración de proyecto ARM o x64, cambie el parámetro MIDL/env a x64 o ARM32.

Para asegurarse de que el archivo IDL se regenera cada vez que se cambia el archivo. winmd, cambie **ejecutar el evento posterior** a la compilación a **cuando la compilación actualice la salida del proyecto.**
La página de propiedades eventos de compilación debe ser similar ![a esta: eventos de compilación](./images/buildevents.png)

Recompila la solución para generar y compilar el código IDL.

Para comprobar que MIDL ha compilado la solución correctamente, busque ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c, y dlldata.c en el directorio del proyecto ToasterComponent.

## <a name="to-compile-the-proxy-and-stub-code-into-a-dll"></a>Para compilar el servidor proxy y el código auxiliar en un archivo DLL

Ahora que tiene los archivos necesarios, puede compilarlos para generar un archivo DLL, que es un archivo de C++. Para simplificar esta tarea, agregue un nuevo proyecto para compilar los servidores proxy. Abra el menú contextual de la solución ToasterApplication y, a continuación, elija **agregar > nuevo proyecto**. En el panel izquierdo del cuadro de diálogo **nuevo proyecto** , expanda ** &gt; Visual C++ &gt; ventanas uniuniversales de Windows**y, a continuación, en el panel central, seleccione **dll (aplicaciones para UWP)**. (Observe que este no es un proyecto de componente de Windows Runtime de C++). Asigne un nombre a los servidores proxy del proyecto y elija el botón **Aceptar** . Estos archivos los actualizarán los eventos posteriores a la compilación cuando se produzca algún cambio en la clase de C#.

De forma predeterminada, el proyecto Proxies genera archivos .h de encabezado y archivos .cpp de C++. Dado que un archivo DLL se compila a partir de los archivos generados desde MIDL, los archivos .h y .cpp no son necesarios. En Explorador de soluciones, abra el menú contextual para ellos, elija **quitar**y, a continuación, confirme la eliminación.

Ahora que el proyecto está vacío, puede volver a agregar los archivos generados por MIDL. Abra el menú contextual del proyecto proxies y, a continuación, elija **agregar > elemento existente.** En el cuadro de diálogo, navegue hasta el directorio del proyecto ToasterComponent y selecciona estos archivos: ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c y dlldata.c. Elija el botón **Agregar**.

En el proyecto Proxies, crea un archivo .def para definir las exportaciones de DLL descritas en dlldata.c. Abra el menú contextual del proyecto y, a continuación, elija **agregar > nuevo elemento**. En el panel izquierdo del cuadro de diálogo, seleccione Código y, a continuación, en el panel central, seleccione Archivo de definición de módulo. Asigne al archivo el nombre proxies. def y, a continuación, elija el botón **Agregar** . Abre este archivo .def y modifícalo para que incluya las exportaciones definidas en dlldata.c:

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

Si compilas ahora el proyecto, se producirá un error. Para compilarlo correctamente, debes cambiar el modo en que se compila y se vincula. En Explorador de soluciones, abra el menú contextual del proyecto proxies y, a continuación, elija **propiedades**. Cambie las páginas de propiedades como se indica a continuación.

En el panel izquierdo, seleccione **C/C++ > preprocesador**y, a continuación, en el panel derecho, seleccione **definiciones de preprocesador**, elija el botón de flecha abajo y, a continuación, seleccione **Editar**. Agregues estas definiciones al cuadro:

```cpp
WIN32;_WINDOWS
```
En **C/C++ > encabezados precompilados**, cambie **encabezado precompilado** a **no usar encabezados precompilados**y, a continuación, elija el botón **aplicar** .

En **vinculador > general**, cambie **omitir biblioteca de importación** a **ye**s y, a continuación, elija el botón **aplicar** .

En **vinculador > entrada**, seleccione **dependencias adicionales**, elija el botón de flecha abajo y, a continuación, seleccione **Editar**. Agrega este texto al cuadro:

```cpp
rpcrt4.lib;runtimeobject.lib
```

No pegues estas bibliotecas directamente en la fila de la lista. Utilice el cuadro de **edición** para asegurarse de que MSBuild en Visual Studio mantendrá las dependencias adicionales correctas.

Cuando haya realizado estos cambios, elija el botón **Aceptar** en el cuadro de diálogo **páginas de propiedades** .

A continuación, toma una dependencia del proyecto ToasterComponent. Esto garantiza que el proyecto Toaster se compilará antes de que lo haga el proyecto Proxies. Esto es necesario porque el proyecto Toaster es el responsable de generar los archivos para compilar el servidor proxy.

Abra el menú contextual del proyecto Proxies y, a continuación, elija Dependencias del proyecto. Active las casillas para indicar que el proyecto Proxies depende del proyecto ToasterComponent y garantizar que Visual Studio los compilará en el orden correcto.

Compruebe que la solución se compila correctamente; para ello, elija **Compilar > recompilar solución** en la barra de menús de Visual Studio.


## <a name="to-register-the-proxy-and-stub"></a>Para registrar el servidor proxy y el código auxiliar

En el proyecto ToasterApplication, abra el menú contextual de Package. appxmanifest y, a continuación, elija **abrir con**. En el cuadro de diálogo Abrir con, seleccione **Editor de texto XML** y, a continuación, elija el botón **Aceptar** . Vamos a pegar algunos XML que proporciona un registro de la extensión Windows. activatableClass. proxyStub y que se basan en los GUID del proxy. Para encontrar los GUID que se usarán en el archivo .appxmanifest, abra ToasterComponent_i.c. Busque entradas similares a las del ejemplo siguiente. Observe también las definiciones de IToast, IToaster y una tercera interfaz, un controlador de eventos con tipo que tiene dos parámetros: un sistema de notificación y una notificación del sistema. Esto coincide con el evento que se define en la clase tostadora. Tenga en cuenta que los GUID de IToast y IToaster coinciden con los GUID que se definen en las interfaces del archivo de C#. Dado que la interfaz del controlador de eventos con tipo se genera automáticamente, el GUID para esta interfaz también se genera automáticamente.

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

Ahora copiaremos los GUID, los pegaremos en package. appxmanifest en un nodo que agregaremos y denominaremos extensiones y, a continuación, volveremos a darles formato. La entrada de manifiesto se parece a la del ejemplo siguiente, pero de nuevo, recuerde que debe usar sus propios GUID. Observa que el GUID del ClassId del archivo XML es el mismo que el de ITypedEventHandler2. Esto se debe a que ese GUID es el primero que aparece en el archivo ToasterComponent_i.c. Aquí, los GUID no distinguen mayúsculas de minúsculas. En lugar de cambiar manualmente el formato de los GUID para IToast y IToaster, puede volver a las definiciones de interfaz y obtener el valor de GuidAttribute, que tiene el formato correcto. En C++, hay un GUID con el formato correcto en el comentario. En cualquier caso, debes cambiar manualmente el formato del GUID que se utiliza para el ClassId y el controlador de eventos.

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

Pegue el nodo XML de las extensiones como un elemento secundario directo del nodo paquete y un elemento del mismo nivel, por ejemplo, el nodo recursos.

Antes de continuar, es importante que de asegure de que:

-   El ClassId ProxyStub se establece en el primer GUID del archivo ToasterComponent\_i. c. Use el primer GUID que aparece definido en ese archivo para el ClassId. (Podría ser el mismo que el GUID de ITypedEventHandler2).
-   La ruta de acceso es la ruta de acceso relativa del paquete del proxy binario. (En este tutorial, proxies.dll está en la misma carpeta que ToasterApplication.winmd).
-   Los GUID están en el formato correcto. (Es fácil equivocarse con esto).
-   Los identificadores de interfaz del manifiesto coinciden con los\_IID del archivo ToasterComponent i. c.
-   Los nombres de interfaz son únicos en el manifiesto. Dado que el sistema no utiliza estos nombres, puede elegir los que desees. Conviene que elijas nombres de interfaz que coincidan claramente con las interfaces que has definido. Para las interfaces generadas, los nombres deben indicar que se trata de interfaces generadas. Puede usar el archivo ToasterComponent\_i. c para ayudarle a generar nombres de interfaz.

Si intenta ejecutar la solución ahora, obtendrá un error en el que se le informará de que proxies.dll no forma parte de la carga. Abra el menú contextual de la carpeta **referencias** en el proyecto ToasterApplication y, a continuación, elija **Agregar referencia**. Active la casilla situada junto al proyecto Proxies. Además, asegúrate de que la casilla situada junto a ToasterComponent también está seleccionada. Elija el botón **Aceptar** .

Ahora el proyecto debería compilarse. Ejecute el proyecto y compruebe que puede crear el Toast.

## <a name="related-topics"></a>Temas relacionados

* [Componentes de Windows Runtime con C++/CX](creating-windows-runtime-components-in-cpp.md)
