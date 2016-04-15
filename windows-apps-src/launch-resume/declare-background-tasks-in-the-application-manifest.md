---
title: Declarar tareas en segundo plano en el manifiesto de la aplicación
description: Habilita el uso de tareas en segundo plano declarándolas como extensiones en el manifiesto de la aplicación.
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
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

    > **Note**  Normally, an app will run in a special process called "BackgroundTaskHost.exe". It is possible to add an Executable element to the Extension element, allowing the background task to run in the context of the app. Only use the Executable element with background tasks that require it, such as the [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).    

## Add Additional Background Task Extensions


Repeat step 2 for each additional background task class registered by your app.

The following example is the complete Application element from the [background task sample]( http://go.microsoft.com/fwlink/p/?linkid=227509). This shows the use of 2 background task classes with a total of 3 trigger types. Copy the Extensions section of this example, and modify it as needed, to declare background tasks in your application manifest.

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

 

 





<!--HONumber=Mar16_HO1-->


