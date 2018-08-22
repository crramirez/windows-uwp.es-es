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
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "2787067"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Ejecutar una tarea en segundo plano cuando se actualice la aplicación para UWP

Obtenga información sobre cómo escribir una tarea en segundo plano que se ejecuta después de actualiza la aplicación de almacenamiento de la plataforma de Windows Universal (UWP).

Se invoca la tarea de fondo de la tarea Actualizar el sistema operativo después de que el usuario instala una actualización para una aplicación que esté instalada en el dispositivo. Esto permite que su aplicación realizar tareas de inicialización tales como inicializar un nuevo canal de notificación de inserción, actualización de esquema de base de datos y así sucesivamente, antes de que el usuario inicia la aplicación actualizada.

La tarea de actualización difiere de inicio de una tarea en segundo plano con el desencadenador [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) debido a que en ese caso la aplicación debe ejecutar al menos una vez antes de que se actualice con el fin de registrar la tarea en segundo plano que se activará la ** ServicingComplete** desencadenador.  No se ha registrado la tarea Actualizar y por lo que una aplicación que nunca se ha ejecutado, pero que se actualiza, aún tendrá su tarea de actualización que se desencadena.

## <a name="step-1-create-the-background-task-class"></a>Paso 1: Crear la clase de tarea de fondo

Como con otros tipos de tareas en segundo plano, se implementa la tarea en segundo plano la tarea de actualización como un componente de tiempo de ejecución de Windows. Para crear este componente, siga los pasos descritos en la sección **crear la clase de tarea en segundo plano** de [crear y registrar una tarea en segundo plano de fuera de proceso](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task). Los pasos incluyen:

- Adición de un proyecto de componente de tiempo de ejecución de Windows a la solución.
- Creación de una referencia desde su aplicación para el componente.
- Creación de una clase sellada, pública en el componente que implementa [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).
- Implementar el método [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) , que es el punto de entrada requiere que se llama cuando se ejecuta la tarea de actualización. Si va a realizar llamadas asincrónicas desde la tarea en segundo plano, [crear y registrar una tarea en segundo plano de fuera de proceso](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) se explica cómo usar un aplazamiento en el método **Run** .

No es necesario registrar esta tarea en segundo plano (la sección "Registrar la tarea en segundo plano para ejecutar" en el tema **crear y registrar una tarea en segundo plano de fuera de proceso** ) para utilizar la tarea de actualización. Esta es la razón principal para utilizar una tarea de actualización debido a que no es necesario agregar ningún código a la aplicación para registrar la tarea y la aplicación no tiene que ejecutar al menos una vez antes de que se actualiza para registrar la tarea en segundo plano.

El siguiente ejemplo de código muestra un punto de partida básico para una clase de tarea de fondo de tarea de actualización en C#. La propia clase de tarea de fondo - y todas las demás clases en el proyecto de la tarea de fondo - deben ser **público** y **sellado**. La clase de tarea de fondo debe derivar de **IBackgroundTask** y tener un método **Run()** public con la firma que se muestra a continuación:

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

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>Paso 2: Declarar la tarea en segundo plano en el manifiesto del paquete

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

En el XML anterior, asegúrese de que el `EntryPoint` está establecido en el nombre de EspacioDeNombres.clase de la clase de tarea de actualización. El nombre distingue mayúsculas de minúsculas.

## <a name="step-3-debugtest-your-update-task"></a>Paso 3: Depuración y prueba la tarea de actualización

Asegúrese de que ha implementado la aplicación en su equipo para que hay algo que se debe actualizar.

Establecer un punto de interrupción en el método Run() de la tarea en segundo plano.

![punto de interrupción establecido](images/run-func-breakpoint.png)

A continuación, en el Explorador de soluciones, haga clic en proyecto de la aplicación (no el proyecto de la tarea de fondo) y, a continuación, haga clic en **Propiedades**. En la ventana de propiedades de la aplicación, haga clic en **Depurar** en la izquierda, a continuación, seleccione **no se inicia, pero depurar mi código cuando se inicia**:

![establecer la configuración de depuración](images/do-not-launch-but-debug.png)

A continuación, para asegurarse de que se activará la UpdateTask, aumente el número de versión del paquete. En el Explorador de soluciones, haga doble clic en el archivo de **Package.appxmanifest** de su aplicación para abrir el Diseñador de paquetes y, a continuación, actualice el número de **generación** :

![actualización de la versión](images/bump-version.png)

Ahora, en 2017 de Visual Studio al presionar F5, se actualizará la aplicación y el sistema se activará el componente UpdateTask en segundo plano. El depurador se conectará automáticamente al proceso de fondo. Obtener alcanzará su punto de interrupción y puede paso a través de la lógica de actualización de código.

Una vez completada la tarea en segundo plano, puede iniciar la aplicación de primer plano en el menú de inicio de Windows en la misma sesión de depuración. El depurador se vuelva a vincular de forma automática, este tiempo para el proceso de primer plano, y puede paso a través de la lógica de su aplicación.

> [!NOTE]
> Los usuarios de Visual Studio 2015: los pasos anteriores se aplican a 2017 de Visual Studio. Si está utilizando 2015 de Visual Studio, puede usar las mismas técnicas para desencadenar y prueba UpdateTask, excepto Visual Studio no se adjunta a ella. Un procedimiento alternativo en 2015 VS es un [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) que establece el UpdateTask como su punto de entrada del programa de instalación y desencadene la ejecución directamente desde la aplicación de primer plano.

## <a name="see-also"></a>Consulta también

[Crear y registrar una tarea en segundo plano fuera del proceso.](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
