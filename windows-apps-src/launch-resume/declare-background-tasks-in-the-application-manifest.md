---
title: Declarar tareas en segundo plano en el manifiesto de la aplicación
description: Habilita el uso de tareas en segundo plano declarándolas como extensiones en el manifiesto de la aplicación.
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, tarea en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 4527cface4681bf4866249c6398d43e6af782725
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8337331"
---
# <a name="declare-background-tasks-in-the-application-manifest"></a>Declarar tareas en segundo plano en el manifiesto de la aplicación




**API importantes**

-   [**Esquema BackgroundTasks**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

Habilita el uso de tareas en segundo plano declarándolas como extensiones en el manifiesto de la aplicación.

> [!Important]
>  Este artículo es específico de tareas fuera del proceso en segundo plano. Las tareas en segundo plano dentro del proceso no se declaran en el manifiesto.

Las tareas en segundo plano fuera del proceso deben declararse en el manifiesto de la aplicación. De lo contrario, tu aplicación no podrá registrarlas (se generará una excepción). Además, las tareas en segundo plano fuera del proceso deben declararse en el manifiesto de la aplicación para pasar la certificación.

Este tema supone que has creado una o más clases de tareas en segundo plano y que tu aplicación registra cada tarea en segundo plano para ejecutar en respuesta a, al menos, un desencadenador.

## <a name="add-extensions-manually"></a>Agregar extensiones manualmente


Abre el manifiesto de la aplicación (Package.appxmanifest) y ve al elemento Application. Crea un elemento Extensions (si no existe uno ya).

El siguiente fragmento de código está tomado de la [muestra de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666):

```xml
<Application Id="App"
   ...
   <Extensions>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
       <BackgroundTasks>
         <Task Type="systemEvent" />
         <Task Type="timer" />
       </BackgroundTasks>
     </Extension>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
       <BackgroundTasks>
         <Task Type="systemEvent"/>
       </BackgroundTasks>
     </Extension>
   </Extensions>
 </Application>
```

## <a name="add-a-background-task-extension"></a>Agregar extensión de tarea en segundo plano  

Declara tu primera tarea en segundo plano.

Copia este código al elemento Extensions (agregarás atributos en los siguientes pasos).

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="">
      <BackgroundTasks>
        <Task Type="" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

1.  Cambia el atributo EntryPoint para que tenga la misma cadena que usó tu código como punto de entrada al registrar la tarea en segundo plano (**namespace.classname**).

    En este ejemplo, el punto de entrada es ExampleBackgroundTaskNameSpace.ExampleBackgroundTaskClassName:

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTaskClassName">
       <BackgroundTasks>
         <Task Type="" />
       </BackgroundTasks>
    </Extension>
</Extensions>
```

2.  Cambia la lista del atributo Task Type para indicar el tipo de registro de tareas usado con esta tarea en segundo plano. Si la tarea en segundo plano se registra con varios tipos de desencadenadores, agrega elementos Task y atributos Type adicionales para cada uno.

    **Nota**Asegúrate para mostrar cada uno de los tipos de desencadenadores que uses, la tarea en segundo plano no se registrará con los tipos de desencadenadores no declarados (el método de [**registrar**](https://msdn.microsoft.com/library/windows/apps/br224772) un error y lanzará una excepción).

    Este ejemplo de fragmento de código indica el uso de desencadenadores de eventos del sistema y de notificaciones de inserción:

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
    <BackgroundTasks>
        <Task Type="systemEvent" />
        <Task Type="pushNotification" />
    </BackgroundTasks>
</Extension>
```

### <a name="add-multiple-background-task-extensions"></a>Agregar varias extensiones de tareas en segundo plano

Repite el paso 2 para todas las clases de tareas en segundo plano que haya registrado tu aplicación.

El siguiente ejemplo es el elemento Application completo de la [muestra de tarea en segundo plano]( http://go.microsoft.com/fwlink/p/?linkid=227509). Este muestra el uso de 2 clases de tareas en segundo plano con un total de 3 tipos de desencadenadores. Copia la sección Extensions de este ejemplo y modifícala conforme sea necesario para declarar tareas en segundo plano en el manifiesto de la aplicación.

```xml
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="BackgroundTask.App">
        <uap:VisualElements
          DisplayName="BackgroundTask"
          Square150x150Logo="Assets\StoreLogo-sdk.png"
          Square44x44Logo="Assets\SmallTile-sdk.png"
          Description="BackgroundTask"

          BackgroundColor="#00b2f0">
          <uap:LockScreen Notification="badgeAndTileText" BadgeLogo="Assets\smalltile-Windows-sdk.png" />
            <uap:SplashScreen Image="Assets\Splash-sdk.png" />
            <uap:DefaultTile DefaultSize="square150x150Logo" Wide310x150Logo="Assets\tile-sdk.png" >
                <uap:ShowNameOnTiles>
                    <uap:ShowOn Tile="square150x150Logo" />
                    <uap:ShowOn Tile="wide310x150Logo" />
                </uap:ShowNameOnTiles>
            </uap:DefaultTile>
        </uap:VisualElements>

      <Extensions>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
          <BackgroundTasks>
            <Task Type="systemEvent" />
            <Task Type="timer" />
          </BackgroundTasks>
        </Extension>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
          <BackgroundTasks>
            <Task Type="systemEvent"/>
          </BackgroundTasks>
        </Extension>
      </Extensions>
    </Application>
</Applications>
```

## <a name="declare-where-your-background-task-will-run"></a>Declarar donde se ejecutará la tarea en segundo plano

Puedes especificar dónde se ejecutan las tareas en segundo plano:

* De manera predeterminada, se ejecutan en el proceso de BackgroundTaskHost.exe.
* En el mismo proceso que la aplicación en primer plano.
* Usa `ResourceGroup` para colocar varias tareas en segundo plano en el mismo proceso de host o separarlas en distintos procesos.
* Usa `SupportsMultipleInstances` para ejecutar el proceso en segundo plano en un nuevo proceso que obtiene sus propios límites de recursos (memoria, cpu) cada vez que se desencadena un nuevo desencadenador.

### <a name="run-in-the-same-process-as-your-foreground-application"></a>Ejecutar en el mismo proceso que la aplicación en primer plano

En este XML de ejemplo se declara una tarea en segundo plano que se ejecuta en el mismo proceso que la aplicación en primer plano.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="ExecModelTestBackgroundTasks.ApplicationTriggerTask">
        <BackgroundTasks>
            <Task Type="systemEvent" />
        </BackgroundTasks>
    </Extension>
</Extensions>
```

Cuando especifiques **EntryPoint**, la aplicación recibe una devolución de llamada al método especificado cuando se active el desencadenador. Si no especificas un **EntryPoint**, la aplicación recibe la devolución de llamada mediante [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).  Consulta [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md) para obtener detalles.

### <a name="specify-where-your-background-task-runs-with-the-resourcegroup-attribute"></a>Especifica dónde se ejecuta la tarea en segundo plano con el atributo ResourceGroup.

En este XML de ejemplo se declara una tarea en segundo plano que se ejecuta en un proceso de BackgroundTaskHost.exe, pero en uno independiente de otras instancias de tareas en segundo plano de la misma aplicación. Observa el atributo `ResourceGroup`, que identifica las tareas en segundo plano que se ejecutan juntas.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.SessionConnectedTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimeZoneTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="timer" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.ApplicationTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.MaintenanceTriggerTask" ResourceGroup="foobar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

### <a name="run-in-a-new-process-each-time-a-trigger-fires-with-the-supportsmultipleinstances-attribute"></a>Ejecutar en un nuevo proceso cada vez que un desencadenador se activa con el atributo SupportsMultipleInstances

Este ejemplo declara una tarea en segundo plano que se ejecuta en un proceso nuevo que obtiene sus propios límites de recursos (memoria y CPU) cada vez que se activa un nuevo desencadenador. Ten en cuenta el uso de `SupportsMultipleInstances` que permite este comportamiento. Para poder usar este atributo debe destinarse versión de SDK '10.0.15063' (Windows 10 Creators Update) o superior.

```xml
<Package
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application ...>
            ...
            <Extensions>
                <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask">
                    <BackgroundTasks uap4:SupportsMultipleInstances=“True”>
                        <Task Type="timer" />
                    </BackgroundTasks>
                </Extension>
            </Extensions>
        </Application>
    </Applications>
```

> [!NOTE]
> No puedes especificar `ResourceGroup` ni `ServerName` junto con `SupportsMultipleInstances`.

## <a name="related-topics"></a>Temas relacionados

* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
