---
title: Ejecutar una tarea en segundo plano cuando se actualice la aplicación para UWP
description: Aprende a crear una tarea en segundo plano que se ejecute cuando se actualice la aplicación de la tienda de la Plataforma universal de Windows (UWP).
ms.date: 04/21/2017
ms.topic: article
keywords: Windows 10, uwp, actualización, la tarea en segundo plano, updatetask, tarea en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 8cd7d4494340d1c5e617361f2e3d750b35ebabb9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603530"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Ejecutar una tarea en segundo plano cuando se actualice la aplicación para UWP

Aprende a escribir una tarea en segundo plano que se ejecute tras la actualización de la aplicación de la tienda de la Plataforma universal de Windows (UWP).

La tarea en segundo plano de tarea de actualización se invoca por el sistema operativo después de que el usuario instale una actualización de una aplicación instalada en el dispositivo. Esto permite que la aplicación realizar tareas de inicialización como inicializar un nuevo canal de notificación de inserción, actualización de esquema de base de datos etc., antes de que el usuario inicia la aplicación actualizada.

La tarea de actualización difiere del inicio de una tarea en segundo plano usando el desencadenador [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) porque en ese caso la aplicación debe ejecutarse al menos una vez para registrar la tarea en segundo plano que se activará por el desencadenador **ServicingComplete**.  La tarea de actualización no está registrada y, por tanto, una aplicación que no se haya ejecutado nunca, pero que esté actualizada, seguirá teniendo su tarea de actualización desencadenada.

## <a name="step-1-create-the-background-task-class"></a>Paso 1: Crear la clase de tarea en segundo plano

Al igual que ocurre con otros tipos de tareas en segundo plano, la tarea en segundo plano de actualización de tareas se implementa como componente de Windows Runtime. Para crear este componente, sigue los pasos indicados en la sección **Crear la clase de tareas en segundo plano** de [Crear y registrar una tarea en segundo plano fuera de proceso](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task). Estos son los pasos que debes realizar:

- Agregar un proyecto de componente de Windows Runtime a la solución.
- Creación de una referencia al componente desde la aplicación.
- Crear una clase sellada pública en el componente que implemente [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).
- Implementar el método [**Ejecutar**](https://msdn.microsoft.com/library/windows/apps/br224811), que es el punto de entrada necesario al que se llama cuando se ejecuta la tarea de actualización. Si vas a hacer llamadas asincrónicas desde tu tarea en segundo plano, en [Crear y registrar una tarea en segundo plano fuera de proceso](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) explica cómo usar un aplazamiento en tu método **Ejecutar**.

No tienes que registrar esta tarea en segundo plano (la sección "Registrar la tarea en segundo plano por ejecutar" del tema **Crear y registrar una tarea en segundo plano fuera de proceso** tema) para usar la tarea de actualización. Este es el motivo principal para usar una tarea de actualización porque no es necesario agregar ningún código a la aplicación para registrar la tarea y no es necesario que la aplicación tenga que ejecutarse al menos una vez antes de actualizarse para registrar la tarea en segundo plano.

El siguiente código de ejemplo muestra un punto de inicio básico para una clase de tarea en segundo plano de tarea de actualización en C#. La propia clase de tareas en segundo plano, y todas las demás clases en el proyecto de tareas en segundo plano, deben ser clases de tipo **public** y **sealed**. La clase de tarea en segundo plano debe derivarse de **IBackgroundTask** y tiene un método **Run()** público con la firma que se muestra a continuación:

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

En el Explorador de soluciones de Visual Studio, haz clic con el botón derecho en el archivo **Package.appxmanifest** y haz clic en **Ver código** para ver el manifiesto de paquete. Agrega el XML `<Extensions>` siguiente para declarar la tarea de actualización:

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

En el XML anterior, asegúrate de que el atributo `EntryPoint` está establecido en el nombre namespace.class de la clase de la tarea de actualización. El nombre distingue mayúsculas de minúsculas.

## <a name="step-3-debugtest-your-update-task"></a>Paso 3: La tarea de actualización de depuración y pruebas

Asegúrate de que has implementado la aplicación en el equipo o que hay algo que actualizar.

Establece un punto de interrupción en el método Run() de la tarea en segundo plano.

![Establecer punto de interrupción](images/run-func-breakpoint.png)

A continuación, en el Explorador de soluciones, haz clic con el botón derecho en el proyecto de tu aplicación (no en el proyecto de la tarea en segundo plano) y, a continuación, haz clic en **Propiedades**. En la ventana de Propiedades de la aplicación, haz clic en **Depurar** a la izquierda y, a continuación, selecciona **No iniciar, pero depurar mi código al empezar**:

![establecer la configuración de depuración](images/do-not-launch-but-debug.png)

A continuación, para garantizar que se activa UpdateTask, aumenta el número de versión del paquete. En el Explorador de soluciones, haz doble clic en el archivo **Package.appxmanifest** de la aplicación para abrir el diseñador de paquetes y, a continuación, actualiza el número de **Compilación**:

![actualizar la versión](images/bump-version.png)

Ahora en Visual Studio de 2017 al presionar F5 se actualizará la aplicación y el sistema activará tu componente UpdateTask en segundo plano. El depurador se asociará automáticamente al proceso en segundo plano. Se seleccionará el punto de interrupción y podrá recorrer la lógica del código de actualización.

Cuando se complete la tarea en segundo plano, podrás iniciar la aplicación en primer plano desde el menú Inicio de Windows en la misma sesión de depuración. El depurador se asociará automáticamente de nuevo, este vez a su proceso en primer plano, y podrás analizar la lógica de tu aplicación.

> [!NOTE]
> Usuarios de Visual Studio 2015: Los pasos anteriores se aplican a Visual Studio 2017. Si usas Visual Studio 2015, puedes usar las mismas técnicas para activar y probar la UpdateTask, excepto que Visual Studio no se asociará a ella. Un procedimiento alternativo de VS 2015 es configurar un [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) que establezca la UpdateTask como su punto de entrada y activar la ejecución directamente desde la aplicación en primer plano.

## <a name="see-also"></a>Consulte también

[Crear y registrar una tarea en segundo plano fuera de proceso](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
