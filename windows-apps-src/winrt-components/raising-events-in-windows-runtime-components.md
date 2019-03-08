---
title: Generación de eventos en componentes de Windows Runtime
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: Cómo generar un evento de un tipo de delegado definido por el usuario en un subproceso en segundo plano para que JavaScript pueda recibir el evento.
ms.date: 07/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 851f8a25055c90dfd592d5a68c733258bcd5f7b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605960"
---
# <a name="raising-events-in-windows-runtime-components"></a>Generación de eventos en componentes de Windows Runtime
> [!NOTE]
> Para obtener información sobre cómo provocar eventos en un [C++ / c++ / WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) componente en tiempo de ejecución de Windows, consulte [crear eventos en C++ / c++ / WinRT](../cpp-and-winrt-apis/author-events.md).

Si tu componente de Windows Runtime genera un evento de un tipo de delegado definido por el usuario en un subproceso en segundo plano (subproceso de trabajo) y deseas que JavaScript pueda recibir el evento, puedes implementarlo o generarlo mediante uno de estos métodos:

-   (Opción 1) Genera el evento a través de [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) para calcular las referencias del evento en el contexto del subproceso de JavaScript. Aunque esta suele ser la mejor opción, en algunos escenarios es posible que no proporcione el rendimiento más rápido.
-   (Opción 2) Usa [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt; pero pierde la información de tipo (pierde la información de tipo de evento). Si no es factible la opción 1 o su rendimiento no es adecuado, esta es una buena segunda opción si la pérdida de información de tipo es aceptable.
-   (Opción 3) Crea tu propio proxy y código auxiliar para el componente. Esta opción es la más difícil de implementar, pero conserva la información de tipo y podría proporcionar un mejor rendimiento en comparación con la opción 1 en escenarios exigentes.

Si solo se genera un evento en un subproceso en segundo plano sin usar una de estas opciones, un cliente de JavaScript no recibirá el evento.

## <a name="background"></a>Background

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

En el resto de este artículo se muestra cómo usar C# para crear un componente básico de Windows Runtime y, a continuación, usar C++ para crear un archivo DLL para que el proxy y el código auxiliar que habilitarán JavaScript puedan consumir un evento Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt; generado por el componente en una operación asincrónica. (También puedes usar C++ o Visual Basic para crear el componente. Los pasos que deben seguirse para crear servidores proxy y códigos auxiliares son los mismos.) En este tutorial se basa en crear un ejemplo de componente en proceso en tiempo de ejecución de Windows (C++ / c++ / CX) y le ayudará a comprender sus propósitos.

Este tutorial tiene las siguientes partes:

-   Aquí crearás dos clases básicas de Windows Runtime. Una clase expone un evento de tipo [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) y la otra clase es el tipo que se devuelve a JavaScript como argumento para TValue. Estas clases no se pueden comunicar con JavaScript hasta que completes los pasos posteriores.
-   Esta aplicación activa el objeto de clase principal, llama a un método y controla un evento generado por el componente de Windows Runtime.
-   Estos son necesarios para las herramientas que generan las clases de proxy y código auxiliar.
-   A continuación, usas el archivo IDL para generar el código fuente C para el proxy y el código auxiliar.
-   Registra los objetos proxy-código auxiliar para que el tiempo de ejecución de COM pueda encontrarlos y haz referencia a la DLL de proxy-código auxiliar en el proyecto de aplicación.

## <a name="to-create-the-windows-runtime-component"></a>Para crear el componente de Windows Runtime

En la barra de menús de Visual Studio, elige **Archivo &gt; Nuevo proyecto**. En el cuadro de diálogo **Nuevo proyecto** , expande **JavaScript &gt; Universal de Windows** y, a continuación, selecciona **Aplicación vacía**. Nombra el proyecto ToasterApplication y después selecciona el botón **Aceptar**.

Agregar un C# componente en tiempo de ejecución de Windows a la solución: En el Explorador de soluciones, abra el menú contextual para la solución y, a continuación, elija **agregar &gt; nuevo proyecto**. Expanda **Visual C# &gt; Microsoft Store** y, a continuación, seleccione **componente de Windows en tiempo de ejecución**. Asigna al proyecto el nombre de ToasterComponent y después selecciona el botón **Aceptar** . ToasterComponent será el espacio de nombres de raíz para los componentes que crearás en pasos posteriores.

En el Explorador de soluciones, abre el menú contextual para la solución y, a continuación, elige **Propiedades**. En el cuadro de diálogo **Páginas de propiedades**, selecciona **Propiedades de configuración** en el panel izquierdo y luego, en la parte superior del cuadro de diálogo, establece **Configuración** en **Depurar** y **Plataforma** en x86, x64 o ARM. Elige el botón **Aceptar**.

**Importante** plataforma = Any CPU no funcionará porque no es válida para el archivo DLL de Win32 de código nativo que agregará a la solución más adelante.

En el Explorador de soluciones, cambia el nombre class1.cs por ToasterComponent.cs para que coincida con el nombre del proyecto. Visual Studio cambia automáticamente el nombre de la clase en el archivo para que coincida con el nuevo nombre de archivo.

En el archivo .cs, agrega una directiva using para el espacio de nombres Windows.Foundation para introducir TypedEventHandler en el ámbito.

Cuando necesitas proxies y códigos auxiliares, tu componente debe utilizar interfaces para exponer sus miembros públicos. En ToasterComponent.cs, define una interfaz para el notificador y otra para la notificación del sistema que produce el notificador.

**Tenga en cuenta** en C# puede omitir este paso. En su lugar, crea primero una clase y, a continuación, abre su menú contextual y elige **Refactorizar &gt; Extraer interfaz**. En el código que se genera, otorga manualmente acceso público a las interfaces.

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

**Tenga en cuenta** la llamada asincrónica en el código anterior usa ThreadPool.RunAsync únicamente para mostrar una manera sencilla de desencadenar el evento en un subproceso en segundo plano. Este método en concreto se podría escribir como se muestra en el ejemplo siguiente y funcionaría correctamente porque el programador de tareas de .NET automáticamente calcula la referencia de las llamadas asincrónicas y "await" al subproceso de IU.
  
```csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

Si compilas el proyecto ahora, debería generarse limpiamente.

## <a name="to-program-the-javascript-app"></a>Para programar la aplicación de JavaScript

Ahora podemos agregar un botón a la aplicación de JavaScript para hacer que utilice la clase que acabamos de definir para hacer una notificación del sistema. Antes de hacerlo, debemos agregar una referencia al proyecto ToasterComponent que acabamos de crear. En el Explorador de soluciones, abre el menú contextual del proyecto ToasterApplication, elige **Agregar &gt; Referencias** y, a continuación, elige el botón **Agregar nueva referencia**. En el cuadro de diálogo Agregar referencia, en el panel izquierdo que se encuentra debajo de Solución, selecciona el proyecto de componente y, a continuación, en el panel central, selecciona ToasterComponent. Elige el botón **Aceptar**.

En el Explorador de soluciones, abre el menú contextual para el proyecto ToasterApplication y después selecciona **Establecer como proyecto de inicio**.

Al final del archivo default.js, agrega un espacio de nombres que contenga las funciones para llamar al componente y ser recuperado por este. El espacio de nombres tendrá dos funciones, una para efectuar notificaciones del sistema y otra para controlar el evento completo con notificación del sistema. La implementación de makeToast crea un objeto de notificación del sistema, registra el controlador de eventos y efectúa la notificación del sistema. Hasta ahora, el controlador de eventos no aporta mucho, como se muestra aquí:

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

La función makeToast se debe enlazar con un botón. Actualiza default.html para que incluya un botón y algo de espacio para generar el resultado de efectuar la notificación del sistema:

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

Si no estuviéramos usando un TypedEventHandler, ahora podríamos ejecutar la aplicación en el equipo local y hacer clic en el botón para efectuar la notificación del sistema. Pero en nuestra aplicación, no sucede nada. Para conocer el motivo, vamos a depurar el código administrado que desencadena el ToastCompletedEvent. Detén el proyecto y, a continuación, en la barra de menús, elige **Depurar &gt; Propiedades de la aplicación del notificador**. Cambia **Tipo de depurador** por **Solo administrado**. De nuevo en la barra de menús, elige **Depurar &gt; Excepciones** y, a continuación, selecciona **Excepciones de tiempo de ejecución de lenguaje común**.

Ahora ejecuta la aplicación y haz clic en el botón para efectuar notificaciones del sistema. El depurador captura una excepción de conversión no válida. Aunque no es evidente a partir de su mensaje, esta excepción se produce porque faltan proxies de esa interfaz.

![proxy ausente](./images/debuggererrormissingproxy.png)

El primer paso para crear un proxy y un código auxiliar para un componente, es agregar un identificador o un GUID único a las interfaces. Sin embargo, el formato GUID que se utilizará varía en función de si estás programando en C#, Visual Basic u otro lenguaje. NET, o en C++.

## <a name="to-generate-guids-for-the-components-interfaces-c-and-other-net-languages"></a>Generar GUID para las interfaces del componente (C# y otros lenguajes. NET)

En la barra de menús, elige Herramientas &gt; Crear GUID. En el cuadro de diálogo, selecciona 5. \[Guid(“xxxxxxxx-xxxx...xxxx)\]. Elige el botón Nuevo GUID y, a continuación, elige el botón Copiar.

![herramienta del generador de GUID](./images/guidgeneratortool.png)

Vuelve a la definición de la interfaz y, a continuación, pega el nuevo GUID justo antes de la interfaz IToaster, tal como se muestra en el siguiente ejemplo. (No uses el GUID en el ejemplo. Cada interfaz única debe tener su propio GUID).

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

Agregar una directiva using para el espacio de nombres System.Runtime.InteropServices.

Repite estos pasos para la interfaz IToast.

## <a name="to-generate-guids-for-the-components-interfaces-c"></a>Generar GUID para las interfaces del componente (C++)

En la barra de menús, elige Herramientas &gt; Crear GUID. En el cuadro de diálogo, selecciona 3. static const struct GUID = {...}. Elige el botón Nuevo GUID y, a continuación, elige el botón Copiar.

Pega el GUID justo antes de la definición de interfaz IToaster. Después de pegar, el GUID debería parecerse al siguiente ejemplo. (No uses el GUID en el ejemplo. Cada interfaz única debe tener su propio GUID).
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
Agregar una directiva using para Windows.Foundation.Metadata para incluir GuidAttribute en el alcance.

Ahora convierte manualmente el GUID const a un GuidAttribute para darle formato como se muestra en el siguiente ejemplo. Ten en cuenta que las llaves se reemplazan con corchetes y paréntesis y se quita el punto y coma al final.
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
Repite estos pasos para la interfaz IToast.

Ahora que las interfaces tienen identificadores únicos, podemos crear un archivo IDL con la alimentación del archivo .winmd en la herramienta de línea de comandos winmdidl y, a continuación, generar el código fuente C para el proxy y el código auxiliar, alimentando ese archivo IDL en la herramienta de línea de comandos MIDL. Visual Studio lo hace por nosotros si creamos eventos posteriores a la compilación, tal como se muestra en los pasos siguientes.

## <a name="to-generate-the-proxy-and-stub-source-code"></a>Para generar el código fuente del proxy y el código auxiliar

Para agregar un evento personalizado posterior a la compilación, en el Explorador de soluciones, abre el menú contextual para el proyecto ToasterComponent y, a continuación, elige Propiedades. En el panel izquierdo de las páginas de propiedades, selecciona Eventos de compilación y, a continuación, elige el botón de Edición posterior a la compilación. Agregar los comandos siguientes en la línea de comandos posterior a la compilación. (Se debe llamar primero al archivo por lotes para establecer las variables del entorno y así encontrar la herramienta winmdidl.)

```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```

**Importante**  para an ARM o x64 configuración de proyecto, cambie el parámetro MIDL /env x64 o arm32.

Para asegurarte de que el archivo IDL se vuelve a generar cada vez que se modifica el archivo .winmd, cambia **Ejecutar el evento posterior a la compilación** por **Cuando la compilación actualiza el resultado del proyecto**.
La página de propiedades de eventos de compilación debe ser similar a esto: ![eventos de compilación](./images/buildevents.png)

Volver a compilar la solución para generar y compilar el archivo IDL.

Puedes verificar que MIDL compila correctamente la solución buscando ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c y dlldata.c en el directorio de proyectos ToasterComponent.

## <a name="to-compile-the-proxy-and-stub-code-into-a-dll"></a>Para compilar el proxy y el código auxiliar en un archivo DLL

Ahora que tienes los archivos necesarios, puedes compilarlos para generar un archivo DLL, que es un archivo de C++. Para que esto sea lo más fácil posible, agrega un nuevo proyecto compatible con la compilación de proxies. Abre el menú contextual para la solución ToasterApplication y, a continuación, selecciona **Agregar > Nuevo Proyecto**. En el panel izquierdo de la **nuevo proyecto** cuadro de diálogo, expanda **Visual C++ &gt; Windows &gt; Windows universal**y, a continuación, en el panel central, seleccione **DLL (aplicaciones UWP)** . (Tenga en cuenta que esto no es un proyecto de componente de tiempo de ejecución de Windows de C++). Denomine el proyecto Proxies y, a continuación, elija el **Aceptar** botón. Estos archivos se actualizarán según los eventos posteriores a la compilación cuando cambie algo en la clase de C#.

De forma predeterminada, el proyecto Proxies genera archivos de encabezado .h y archivos .cpp de C++. Dado que se compila la DLL a partir de los archivos generados desde MIDL, los archivos .h y .cpp no son necesarios. En el Explorador de soluciones, abre el menú contextual para estos, elige **Quitar**y, a continuación, confirma la eliminación.

Ahora que el proyecto está vacío, puedes volver a agregar los archivos generados por MIDL. Abre el menú contextual del proyecto Proxies y selecciona **Agregar > Elemento existente.** En el cuadro de diálogo, navegue hasta el directorio del proyecto ToasterComponent y seleccione estos archivos: Archivos ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c y dlldata.c. Elige el botón **Agregar**.

En el proyecto Proxies, crea un archivo .def para definir las exportaciones DLL descritas en dlldata.c. Abre el menú contextual para el proyecto y, a continuación, selecciona **Agregar > Nuevo elemento**. En el panel izquierdo del cuadro de diálogo, selecciona Código y, a continuación, en el panel central, selecciona el archivo de definición de módulos. Asigna en nombre de proxies.def al archivo y, a continuación, elige el botón **Agregar**. Abre este archivo .def y modifícalo para incluir las EXPORTACIONES definidas en dlldata.c:

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

Si compilas el proyecto ahora, se producirá un error. Para compilar correctamente este proyecto, tienes que cambiar la forma en que se compila y se vincula el proyecto. En el Explorador de soluciones, abre el menú contextual del proyecto Proxies y, a continuación, elige **Propiedades**. Cambia las páginas de propiedad de la siguiente manera.

En el panel izquierdo, selecciona **C o C++ > Preprocesador** y, a continuación, en el panel derecho, selecciona **Definiciones de preprocesador**, elige el botón de flecha hacia abajo y, a continuación, selecciona **Edición**. Agrega estas definiciones en el cuadro:

```cpp
WIN32;_WINDOWS
```
En **C o C++ > Encabezados precompilados**, cambia **Encabezado precompilado** por **No utilizar encabezados precompilados** y, a continuación, elige el botón **Aplicar**.

En **Enlazador > General**, cambia **Omitir biblioteca de importación** por **Sí** y, a continuación, elige el botón **Aplicar**.

En **Enlazador > Entrada**, selecciona **Dependencias adicionales**, elige el botón de flecha hacia abajo y, a continuación, selecciona **Edición**. Agrega este texto en el cuadro:

```cpp
rpcrt4.lib;runtimeobject.lib
```

No pegues estas bibliotecas directamente en la fila de la lista. Usa el cuadro **Edición** para garantizar que MSBuild en Visual Studio mantendrá las dependencias adicionales correctas.

Cuando hayas realizado estos cambios, elige el botón **Aceptar** en el cuadro de diálogo **Páginas de propiedades**.

A continuación, toma una dependencia en el proyecto ToasterComponent. Esto garantiza que el notificador efectuará la compilación antes de que se compile el proyecto de proxy. Esto es necesario porque el proyecto de notificador es responsable de generar los archivos para compilar el proxy.

Abre el menú contextual para el proyecto Proxies y, a continuación, Dependencias del proyecto. Activa las casillas de verificación para indicar que el proyecto Proxies depende del proyecto ToasterComponent, para asegurarte de que Visual Studio las compila en el orden correcto.

Comprueba que la solución se genera correctamente seleccionando **Compilar > Generar solución** en la barra de menús de Visual Studio.


## <a name="to-register-the-proxy-and-stub"></a>Para registrar el proxy y el código auxiliar

En el proyecto ToasterApplication, abre el menú contextual para package.appxmanifest y, a continuación, elige **Abrir con**. En el cuadro de diálogo Abrir con, selecciona **Editor de texto XML** y, a continuación, elige el botón **Aceptar**. Vamos a pegar XML que proporcionen un registro de la extensión de windows.activatableClass.proxyStub y que estén basados en los GUID en el proxy. Para buscar los GUID para usar en el archivo .appxmanifest, abre ToasterComponent_i.c. Encuentra entradas similares a las del siguiente ejemplo. Ten en cuenta también las definiciones de IToast, IToaster y una tercera interfaz, un controlador de eventos tipado que tiene dos parámetros: notificador y notificación del sistema. Esto coincide con el evento que se define en la clase Notificador. Ten en cuenta que los GUID de IToast e IToaster coinciden con los GUID que se definen en las interfaces en el archivo C#. Dado que la interfaz de controlador de eventos tipada se genera automáticamente, el GUID para esta interfaz también se genera automáticamente.

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

Ahora copiamos los GUID, los pegamos en package.appxmanifest en un nodo que agregamos y denominamos Extensiones y, a continuación, volvemos a formatearlos. La entrada del manifiesto es similar al ejemplo siguiente pero, nuevamente, recuerda usar tus propios GUID. Ten en cuenta que el identificador de clase GUID en el XML es el mismo que ITypedEventHandler2. Esto es porque ese GUID es el primero que aparece en ToasterComponent_i.c. Estos GUID distinguen mayúsculas de minúsculas. En lugar de reformatear manualmente los GUID para IToast e IToaster, puedes volver a las definiciones de interfaz y obtener el valor GuidAttribute, que tiene el formato correcto. En C++, hay un GUID con el formato correcto en el comentario. En cualquier caso, debes reformatear manualmente el GUID que se usa tanto para el identificador de clase como para el controlador de eventos.

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

Pega el nodo de Extensiones XML como un elemento secundario directo del nodo Packages y un sistema del mismo nivel que, por ejemplo, el nodo Resources.

Antes de continuar, es importante asegurarse de que:

-   El ClassId de ProxyStub está establecido para el primer GUID de la ToasterComponent\_i.c archivo. Se usa el primer GUID que se define en este archivo en el identificador de clase. (Puede ser el mismo que el GUID para ITypedEventHandler2.)
-   La ruta de acceso es el paquete de la ruta de acceso relativa del proxy binario. (En este tutorial, proxies.dll está en la misma carpeta que ToasterApplication.winmd).
-   Los GUID están en el formato correcto. (Es fácil equivocarse en este paso).
-   Los identificadores de interfaz en el manifiesto coinciden con los IID en ToasterComponent\_i.c archivo.
-   Los nombres de interfaz son únicos en el manifiesto. Como el sistema no los usa, puedes elegir los valores. Es una buena práctica para elegir nombres de interfaz que claramente coincidan con las interfaces que has definido. En cuanto a las interfaces generadas, los nombres deben ser indicativos de esas interfaces generadas. Puede usar el ToasterComponent\_archivo i.c para ayudarle a generar nombres de interfaz.

Si intentas ejecutar la solución ahora, obtendrás un error según el cual proxies.dll no forma parte de la carga. Abre el menú contextual para la carpeta **Referencias** en el proyecto ToasterApplications y, a continuación, elige **Agregar referencia**. Selecciona la casilla de verificación situada junto al proyecto Proxy. Además, asegúrate de que la casilla de verificación junto a ToasterComponent también se selecciona. Elige el botón **Aceptar**.

Ahora debería compilarse el proyecto. Ejecuta el proyecto y verifica que puedes efectuar una notificación del sistema.

## <a name="related-topics"></a>Temas relacionados

* [Crear componentes de Windows en tiempo de ejecución en C++](creating-windows-runtime-components-in-cpp.md)
