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
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "4426886"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Ejecutar una tarea en segundo plano cuando se actualice la aplicación para UWP

Obtén información sobre cómo escribir una tarea en segundo plano que se ejecuta después de que se actualice la aplicación de la tienda de la plataforma Universal de Windows (UWP).

La tarea en segundo plano de tarea de actualización se invoca el sistema operativo después de que el usuario instala una actualización a una aplicación que está instalada en el dispositivo. Esto permite que la aplicación realizar tareas de inicialización como la inicialización de un nuevo canal de notificación de inserción, la actualización de esquema de base de datos y así sucesivamente, antes de que el usuario inicia la aplicación actualizada.

La tarea de actualización difiere de inicio de una tarea en segundo plano con el desencadenador de [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) porque en ese caso la aplicación debe ejecutar al menos una vez antes de que se actualiza con el fin de registrar la tarea en segundo plano que se activará la ** ServicingComplete** desencadenador.  La tarea de actualización no está registrada y por lo tanto, una aplicación que nunca se ha ejecutado, pero que se ha actualizado, seguirán teniendo su tarea de actualización desencadenada.

## <a name="step-1-create-the-background-task-class"></a>Paso 1: Crear la clase de tarea en segundo plano

Como con otros tipos de tareas en segundo plano, se implementa la tarea en segundo plano la tarea de actualización como un componente de Windows Runtime. Para crear este componente, sigue los pasos descritos en la sección de **crear la clase de tarea en segundo plano** de [crear y registrar una tarea en segundo plano fuera de proceso](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task). Los pasos incluyen:

- Agregar un proyecto de componente de Windows Runtime a la solución.
- Creación de una referencia al componente desde la aplicación.
- Crear una clase pública, sealed en el componente que implementa [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).
- Implementar el método [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) , que es el punto de entrada necesario al que se llama cuando se ejecuta la tarea de actualización. Si vas a hacer llamadas asincrónicas desde la tarea en segundo plano, [crear y registrar una tarea en segundo plano fuera de proceso](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) se explica cómo usar un aplazamiento en el método **Run** .

No es necesario registrar esta tarea en segundo plano (la sección "Registrar la tarea en segundo plano se ejecute" en el tema **crear y registrar una tarea en segundo plano fuera de proceso** ) para usar la tarea de actualización. Esta es la razón principal para usar una tarea de actualización, ya que no es necesario agregar código a tu aplicación para registrar la tarea y la aplicación no tiene que ejecutar al menos una vez antes de que se actualiza para registrar la tarea en segundo plano.

El código de ejemplo siguiente muestra un punto de partida básico para una clase de tarea en segundo plano de tarea de actualización en C#. La propia clase de tarea en segundo plano (y todas las demás clases en el proyecto de tarea en segundo plano) deben ser **públicos** y **sealed**. La clase de tarea en segundo plano debe derivar de **IBackgroundTask** y tener un método **Run()** público con la firma que se muestra a continuación:

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

En el Explorador de soluciones de Visual Studio, haz clic en **Package.appxmanifest** y haz clic en el **Código de vista** para ver el manifiesto del paquete. Agrega el siguiente `<Extensions>` XML para declarar la tarea de actualización:

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

En el XML anterior, asegúrate de que el `EntryPoint` atributo se establece en el nombre de namespace.class de la clase de tarea de actualización. El nombre distingue mayúsculas de minúsculas.

## <a name="step-3-debugtest-your-update-task"></a>Paso 3: La tarea de actualización de depuración y pruebas

Asegúrate de que ha implementado la aplicación en el equipo para que no hay algo para actualizar.

Establece un punto de interrupción en el método Run() de la tarea en segundo plano.

![punto de interrupción del conjunto](images/run-func-breakpoint.png)

A continuación, en el Explorador de soluciones, haz clic en el proyecto de la aplicación (no el proyecto de tarea en segundo plano) y, a continuación, haz clic en **las propiedades**. En la ventana de propiedades de la aplicación, haz clic en **Depurar** de la izquierda y luego selecciona **no iniciar, pero depurar mi código al empezar**:

![establecer la configuración de depuración](images/do-not-launch-but-debug.png)

A continuación, para garantizar que se activará la UpdateTask, aumentar el número de versión del paquete. En el Explorador de soluciones, haz doble clic en el archivo **Package.appxmanifest** de la aplicación para abrir el Diseñador de paquetes y, a continuación, actualiza el número **de compilación** :

![actualizar la versión](images/bump-version.png)

Ahora, en Visual Studio 2017 al presionar F5, se actualizará la aplicación y el sistema activará el componente UpdateTask en segundo plano. El depurador se conectará automáticamente al proceso en segundo plano. Obtener alcanzará el punto de interrupción y puede pasar la lógica del código de actualización.

Cuando se completa la tarea en segundo plano, puedes iniciar la aplicación en primer plano desde el menú de inicio de Windows en la misma sesión de depuración. El depurador se conectará automáticamente nuevo, en este momento de su proceso en primer plano, y puede pasar la lógica de la aplicación.

> [!NOTE]
> Los usuarios de Visual Studio 2015: los pasos anteriores se aplican a Visual Studio 2017. Si estás usando Visual Studio 2015, puedes usar las mismas técnicas de desencadenador y prueba UpdateTask, excepto que Visual Studio no se adjuntará a ella. Un procedimiento alternativo en VS 2015 es un [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) que establece el UpdateTask como su punto de entrada de configuración y desencadenar la ejecución directamente desde la aplicación en primer plano.

## <a name="see-also"></a>Ver también

[Crear y registrar una tarea en segundo plano fuera de proceso](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
