---
title: Ejecutar una tarea en segundo plano cuando se actualice la aplicación para UWP
description: Obtenga información sobre cómo crear una tarea en segundo plano que se ejecute cuando se actualice la aplicación de la tienda de Plataforma universal de Windows (UWP).
ms.date: 04/21/2017
ms.topic: article
keywords: Windows 10, UWP, actualización, tarea en segundo plano, updatetask, tarea en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: ff4dd5487357728b6a79f4c4d31a437075fcd006
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164809"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Ejecutar una tarea en segundo plano cuando se actualice la aplicación para UWP

Obtenga información sobre cómo escribir una tarea en segundo plano que se ejecute después de que se actualice la aplicación de la tienda de Plataforma universal de Windows (UWP).

El sistema operativo invoca la tarea en segundo plano actualizar tarea después de que el usuario instala una actualización en una aplicación instalada en el dispositivo. Esto permite que la aplicación realice tareas de inicialización, como inicializar un nuevo canal de notificaciones de envío, actualizar el esquema de la base de datos, etc., antes de que el usuario inicie la aplicación actualizada.

La tarea de actualización difiere del inicio de una tarea en segundo plano mediante el desencadenador [ServicingComplete](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) porque, en ese caso, la aplicación debe ejecutarse al menos una vez antes de que se actualice para registrar la tarea en segundo plano que activará el desencadenador **ServicingComplete** .  La tarea de actualización no está registrada y, por lo tanto, una aplicación que nunca se ha ejecutado, pero que está actualizada, todavía tendrá su tarea de actualización desencadenada.

## <a name="step-1-create-the-background-task-class"></a>Paso 1: crear la clase de tarea en segundo plano

Como con otros tipos de tareas en segundo plano, la tarea de actualización en segundo plano se implementa como un componente de Windows Runtime. Para crear este componente, siga los pasos de la sección **creación de la clase de tarea en segundo plano** de [creación y registro de una tarea en segundo plano fuera de proceso](./create-and-register-a-background-task.md). Entre los pasos se incluyen:

- Agregar un proyecto de componente de Windows Runtime a la solución.
- Crear una referencia de la aplicación al componente.
- Crear una clase pública y sellada en el componente que implementa [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask).
- Implementar el método [**Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) , que es el punto de entrada necesario al que se llama cuando se ejecuta la tarea de actualización. Si va a realizar llamadas asincrónicas desde la tarea en segundo plano, [crear y registrar una tarea en segundo plano fuera de proceso](./create-and-register-a-background-task.md) explica cómo usar un aplazamiento en el método **Run** .

No es necesario registrar esta tarea en segundo plano (la sección "registrar la tarea en segundo plano para ejecutar" en el tema **crear y registrar una tarea en segundo plano fuera de proceso** ) para usar la tarea de actualización. Esta es la razón principal por la que se usa una tarea de actualización porque no es necesario agregar código a la aplicación para registrar la tarea y la aplicación no tiene que ejecutarse al menos una vez antes de actualizarse para registrar la tarea en segundo plano.

En el código de ejemplo siguiente se muestra un punto de partida básico para una clase de tarea en segundo plano de tarea de actualización en C#. La propia clase de tarea en segundo plano, y todas las demás clases del proyecto de tarea en segundo plano, deben ser **públicas** y **selladas**. La clase de tarea en segundo plano debe derivar de **IBackgroundTask** y tener un método **Run ()** público con la firma que se muestra a continuación:

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

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>Paso 2: declarar la tarea en segundo plano en el manifiesto del paquete

En el Explorador de soluciones de Visual Studio, haga clic con el botón derecho en **Package. appxmanifest** y haga clic en **Ver código** para ver el manifiesto del paquete. Agregue el siguiente `<Extensions>` código XML para declarar la tarea de actualización:

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

En el XML anterior, asegúrese de que el `EntryPoint` atributo está establecido en el nombre del espacio de nombres. clase de la clase de tarea de actualización. El nombre distingue entre mayúsculas y minúsculas.

## <a name="step-3-debugtest-your-update-task"></a>Paso 3: depurar o probar la tarea de actualización

Asegúrese de que ha implementado la aplicación en la máquina para que haya algo que actualizar.

Establezca un punto de interrupción en el método Run () de la tarea en segundo plano.

![establecer punto de interrupción](images/run-func-breakpoint.png)

A continuación, en el explorador de soluciones, haga clic con el botón derecho en el proyecto de la aplicación (no en el proyecto de tarea en segundo plano) y luego haga clic en **propiedades**. En el ventana Propiedades de aplicaciones, haga clic en **depurar** a la izquierda y seleccione no **iniciar, pero depurar mi código al iniciarse**:

![establecer la configuración de depuración](images/do-not-launch-but-debug.png)

A continuación, para asegurarse de que se desencadene el UpdateTask, aumente el número de versión del paquete. En el Explorador de soluciones, haga doble clic en el archivo **Package. appxmanifest** de la aplicación para abrir el diseñador de paquetes y, a continuación, actualice el número de **compilación** :

![actualización de la versión](images/bump-version.png)

Ahora, en Visual Studio 2019 al presionar F5, la aplicación se actualizará y el sistema activará el componente UpdateTask en segundo plano. El depurador se asociará automáticamente al proceso en segundo plano. Se alcanzará el punto de interrupción y podrá recorrer la lógica de código de actualización.

Cuando se completa la tarea en segundo plano, puede iniciar la aplicación en primer plano desde el menú Inicio de Windows dentro de la misma sesión de depuración. El depurador se asociará de nuevo automáticamente, esta vez al proceso en primer plano, y podrá recorrer la lógica de la aplicación.

> [!NOTE]
> Usuarios de Visual Studio 2015: los pasos anteriores se aplican a Visual Studio 2017 o Visual Studio 2019. Si usa Visual Studio 2015, puede usar las mismas técnicas para desencadenar y probar UpdateTask, excepto que Visual Studio no se asociará a él. Un procedimiento alternativo en VS 2015 es configurar un [ApplicationTrigger](./trigger-background-task-from-app.md) que establezca UpdateTask como punto de entrada y desencadenar la ejecución directamente desde la aplicación en primer plano.

## <a name="see-also"></a>Vea también

[Crear y registrar una tarea en segundo plano fuera del proceso](./create-and-register-a-background-task.md)