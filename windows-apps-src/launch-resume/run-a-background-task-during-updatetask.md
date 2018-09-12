---
author: TylerMSFT
title: Ejecutar una tarea en segundo plano cuando se actualice la aplicación para UWP
description: Aprende a crear una tarea en segundo plano que se ejecute cuando se actualice la aplicación de la tienda de la Plataforma universal de Windows (UWP).
ms.author: twhitney
ms.date: 04/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, actualización, tarea en segundo plano, updatetask, tarea en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: fcba2cb736f86cebc6d2664e2ec3b557d47c86d7
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "3931287"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Ejecutar una tarea en segundo plano cuando se actualice la aplicación para UWP

Aprenda a escribir una tarea en segundo plano que se ejecuta después de actualiza su aplicación de tienda Universal Windows Platform (UWP).

El sistema operativo invoca la tarea Actualizar tareas en segundo plano después de que el usuario instala una actualización para una aplicación que está instalada en el dispositivo. Esto permite a su aplicación realizar tareas de inicialización como la inicialización de un nuevo canal de notificación de inserción, actualización de esquema de base de datos etc., antes de que el usuario inicie la aplicación actualizada.

La tarea de actualización difiere de iniciar una tarea en segundo plano mediante el desencadenador [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) porque en ese caso la aplicación debe ejecutarse al menos una vez antes de que se actualice para registrar la tarea en segundo plano que se activará la ** ServicingComplete** desencadenador.  No se ha registrado la tarea de actualizar y por lo que una aplicación que nunca se ha ejecutado, pero que se actualiza, todavía tendrá su tarea de actualización desencadenada.

## <a name="step-1-create-the-background-task-class"></a>Paso 1: Crear la clase de tarea de fondo

Como con otros tipos de tareas en segundo plano, se implementa la tarea en segundo plano la tarea de actualización como un componente en tiempo de ejecución de Windows. Para crear este componente, siga los pasos descritos en la sección **crear la clase de tarea en segundo plano** de [crear y registrar una tarea en segundo plano fuera de proceso](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task). Los pasos incluyen:

- Agregar un proyecto de componente de tiempo de ejecución de Windows a la solución.
- Creación de una referencia al componente desde su aplicación.
- Crear una clase pública, sellada en el componente que implementa [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).
- Implementar el método [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) , que es el punto de entrada requiere que se llama cuando se ejecuta la tarea de actualización. Si va a realizar llamadas asincrónicas de la tarea en segundo plano, [crear y registrar una tarea en segundo plano fuera de proceso](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) explica cómo utilizar un aplazamiento en el método **Run** .

No es necesario registrar esta tarea en segundo plano (la sección "Registro de la tarea en segundo plano para ejecutar" en el tema **crear y registrar una tarea en segundo plano fuera de proceso** ) para utilizar la tarea de actualización. Esta es la razón principal para utilizar una tarea de actualización ya no es necesario agregar ningún código a su aplicación para registrar la tarea y la aplicación no tiene que ejecutar al menos una vez antes de la actualización para registrar la tarea en segundo plano.

El código de ejemplo siguiente muestra el punto de partida para una clase de tarea de fondo de tarea de actualización en C#. La propia clase de tarea de fondo - y todas las demás clases en el proyecto de la tarea de fondo - deben ser **público** y **sellado**. La clase de tarea de fondo debe derivar de **IBackgroundTask** y tiene un método **Run()** public con la firma que se muestra a continuación:

```cs
using Windows.ApplicationModel.Background;

namespace BackgroundTasks
{
    public sealed class UpdateTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // your app migration/update code here
        }
    }
}
```

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>Paso 2: Declarar su tarea en segundo plano en el manifiesto del paquete

En el Explorador de soluciones de Visual Studio, haga clic en **Package.appxmanifest** y haga clic en **Ver código** para ver el manifiesto del paquete. Agregue el siguiente `<Extensions>` XML para declarar la tarea de actualización:

```XML
<Package ...>
    ...
  <Applications>  
    <Application ...>  
        ...
      <Extensions>  
        <Extension Category="windows.updateTask"  EntryPoint="BackgroundTasks.UpdateTask">  
        </Extension>  
      </Extensions>

    </Application>  
  </Applications>  
</Package>
```

En el XML anterior, asegúrese de que el `EntryPoint` está establecido en el nombre de namespace.class de la clase de tarea de actualización. El nombre distingue mayúsculas de minúsculas.

## <a name="step-3-debugtest-your-update-task"></a>Paso 3: Depuración y prueba la tarea de actualización

Asegúrese de que ha implementado la aplicación en el equipo para que hay algo que actualizar.

Establezca un punto de interrupción en el método Run() de la tarea en segundo plano.

![punto de interrupción establecido](images/run-func-breakpoint.png)

A continuación, en el Explorador de soluciones, haga clic en proyecto de la aplicación (no en el proyecto de tarea de fondo) y, a continuación, haga clic en **Propiedades**. En la ventana Propiedades, haga clic en **Depurar** en la izquierda, luego seleccione **no se inicia, pero depurar el código cuando se inicia**:

![establecer la configuración de depuración](images/do-not-launch-but-debug.png)

A continuación, para asegurarse de que el UpdateTask se activa, aumentar el número de versión del paquete. En el Explorador de soluciones, haga doble clic en el archivo de **Package.appxmanifest** de la aplicación para abrir el Diseñador de paquetes y, a continuación, actualizar el número de **compilación** :

![actualización de la versión](images/bump-version.png)

Ahora, en Visual Studio 2017 cuando presiona F5, se actualizará la aplicación y el sistema activará el componente UpdateTask en segundo plano. El depurador se conectará automáticamente al proceso de fondo. Obtener alcanzará el punto de interrupción y puede recorrer la lógica de actualización de código.

Cuando se completa la tarea en segundo plano, puede iniciar la aplicación de primer plano en el menú Inicio de Windows en la misma sesión de depuración. El depurador se conectará automáticamente nuevo, esta vez para el proceso en primer plano, y puede pasar por la lógica de la aplicación.

> [!NOTE]
> Usuarios de Visual Studio 2015: los pasos anteriores se aplican a Visual Studio 2017. Si está utilizando Visual Studio 2015, puede utilizar las mismas técnicas al desencadenador y prueba UpdateTask, excepto de Visual Studio no se conectará a él. Un procedimiento alternativo en 2015 VS es un [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) que la UpdateTask se establece como su punto de entrada de instalación y desencadene la ejecución directamente desde la aplicación de primer plano.

## <a name="see-also"></a>Ver también

[Crear y registrar una tarea en segundo plano fuera del proceso.](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
