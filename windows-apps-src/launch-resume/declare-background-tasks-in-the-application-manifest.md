---
author: TylerMSFT
title: "Declarar tareas en segundo plano en el manifiesto de la aplicación"
description: "Habilita el uso de tareas en segundo plano declarándolas como extensiones en el manifiesto de la aplicación."
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 364edc93c52d3c7c8cbe5f1a85c8ca751eb44b35
ms.lasthandoff: 02/07/2017

---

# <a name="declare-background-tasks-in-the-application-manifest"></a>Declarar tareas en segundo plano en el manifiesto de la aplicación


\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


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

    **Nota**  Asegúrate de incluir en la lista todos los tipos de desencadenadores que uses; de lo contrario, la tarea en segundo plano no se registrará con los tipos de desencadenadores no declarados (el método [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) generará un error y lanzará una excepción).

    Este ejemplo de fragmento de código indica el uso de desencadenadores de eventos del sistema y de notificaciones de inserción:

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
    <BackgroundTasks>
        <Task Type="systemEvent" />
        <Task Type="pushNotification" />
    </BackgroundTasks>
</Extension>
```


## <a name="add-additional-background-task-extensions"></a>Agregar extensiones adicionales de tareas en segundo plano

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

## <a name="declare-your-background-task-to-run-in-a-different-process"></a>Declarar la tarea en segundo plano para que se ejecute en un proceso diferente

La nueva característica de Windows 10, versión 1507, permite ejecutar la tarea en segundo plano en un proceso diferente de BackgroundTaskHost.exe (el proceso en el que las tareas en segundo plano se ejecutan de manera predeterminada).  Existen dos opciones: ejecutar en el mismo proceso que la aplicación en primer plano o ejecutar en una instancia de BackgroundTaskHost.exe independiente de otras instancias de tareas en segundo plano de la misma aplicación.  

### <a name="run-in-the-foreground-application"></a>Ejecutar en la aplicación en primer plano

En este XML de ejemplo se declara una tarea en segundo plano que se ejecuta en el mismo proceso que la aplicación en primer plano. Observa el atributo `Executable`:

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="ExecModelTestBackgroundTasks.ApplicationTriggerTask" Executable="$targetnametoken$.exe">
        <BackgroundTasks>
            <Task Type="systemEvent" />
        </BackgroundTasks>
    </Extension>
</Extensions>
```

> [!Note]
> Usa el elemento Executable únicamente con tareas en segundo plano que lo requieran, por ejemplo, [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).  

### <a name="run-in-a-different-background-host-process"></a>Ejecutar en un proceso de host en segundo plano diferente

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


## <a name="related-topics"></a>Temas relacionados


* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)

