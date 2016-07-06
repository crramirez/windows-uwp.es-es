---
author: TylerMSFT
title: "Declarar tareas en segundo plano en el manifiesto de la aplicación"
description: "Habilita el uso de tareas en segundo plano declarándolas como extensiones en el manifiesto de la aplicación."
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: d7dbdab0e8d404e6607585045d49bb3dd1407de6

---

# Declarar tareas en segundo plano en el manifiesto de la aplicación


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**Esquema BackgroundTasks**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

Habilita el uso de tareas en segundo plano declarándolas como extensiones en el manifiesto de la aplicación.

Las tareas en segundo plano deben declararse en el manifiesto de la aplicación. De lo contrario, tu aplicación no podrá registrarlas (se generará una excepción). Además, las tareas en segundo plano deben declararse en el manifiesto de la aplicación para pasar la certificación.

Este tema supone que has creado una o más clases de tareas en segundo plano y que tu aplicación registra cada tarea en segundo plano para ejecutar en respuesta a, al menos, un desencadenador.

## Agregar extensiones manualmente


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

## Agregar extensión de tarea en segundo plano


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

    **Nota**  Asegúrate de incluir en la lista todos los tipos de desencadenadores que estás usando, o la tarea en segundo plano no se registrará con los tipos de desencadenadores no declarados (el método [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) generará un error y lanzará una excepción).

    Este ejemplo de fragmento de código indica el uso de desencadenadores de eventos del sistema y de notificaciones de inserción:

    ```xml
                <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
                  <BackgroundTasks>
                    <Task Type="systemEvent" />
                    <Task Type="pushNotification" />
                  </BackgroundTasks>
                </Extension>
    ```

    > **Nota**  Normalmente, una aplicación se ejecutará en un proceso especial llamado "BackgroundTaskHost.exe". Se puede agregar un elemento Executable al elemento Extension, lo cual permite a la tarea en segundo plano ejecutarse en el contexto de la aplicación. Usa el elemento Executable únicamente con tareas en segundo plano que lo requieran, como por ejemplo [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).    

## Agregar extensiones adicionales de tareas en segundo plano


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

## Temas relacionados

* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)



<!--HONumber=Jun16_HO5-->


