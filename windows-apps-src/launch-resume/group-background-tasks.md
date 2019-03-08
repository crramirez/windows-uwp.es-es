---
title: Agrupar registro de tarea en segundo plano
description: Registra y anula el registro de tareas en segundo plano como parte de un grupo para aislar esos registros.
ms.date: 04/05/2017
ms.topic: article
keywords: windows 10, tarea en segundo plano
ms.openlocfilehash: a70c814e5e35359746076c5418d1f1d973e61773
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623860"
---
# <a name="group-background-task-registration"></a>Agrupar registro de tarea en segundo plano

**API importantes**

[Clase BackgroundTaskRegistrationGroup](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup)

Ahora se pueden registrar tareas en segundo plano en un grupo, que puedes considerar como espacio de nombres lógico. Este aislamiento ayuda a garantizar que los diferentes componentes de una aplicación, o diferentes bibliotecas, no interfieren con el registro de tareas en segundo plano de cada uno de los otros.

Cuando una aplicación y el marco (o biblioteca) que utiliza registra un fondo de la tarea con el mismo nombre, la aplicación podría quitar accidentalmente los registros de tareas en segundo plano del marco. Los autores de aplicaciones también podrían quitar accidentalmente los registros de tareas en segundo plan de biblioteca y marco porque no pudieron anular el registro de todas las tareas en segundo plano registradas con [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks).  Con los grupos, puede aislar los registros de tareas en segundo plano para que esto no suceda.

## <a name="features-of-groups"></a>Características de los grupos

* Los grupos se identifican de manera única por un GUID. También pueden tener una cadena de nombre descriptivo asociado que es más fácil de leer durante la depuración.
* Se pueden registrar varias tareas en segundo plano en un grupo.
* Las tareas en segundo plano registradas en un grupo no aparecerán en [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks). Por lo tanto, las aplicaciones que usan actualmente **BackgroundTaskRegistration.AllTasks** para anular el registro de sus tareas no anulan accidentalmente el registro de las tareas en segundo plano registradas en un grupo. Consulte [anular el registro de tareas en segundo plano en un grupo](#unregister-background-tasks-in-a-group) siguiente para ver cómo anular el registro de todos los desencadenadores en segundo plano que se han registrado como parte de un grupo.
* Cada registro de tareas en segundo plano tendrá una propiedad de grupo para determinar con qué grupo está asociado.
* Tareas en segundo plano en curso registrar con un grupo hará que la activación recorrer [BackgroundTaskRegistrationGroup.BackgroundActivated](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup.BackgroundActivated) eventos en lugar de [Application.OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated#Windows_UI_Xaml_Application_OnBackgroundActivated_Windows_ApplicationModel_Activation_BackgroundActivatedEventArgs_).

## <a name="register-a-background-task-in-a-group"></a>Registrar una tarea en segundo plano en un grupo

A continuación se muestra cómo registrar una tarea en segundo plano (desencadenada por un cambio de zona horaria, en este ejemplo) como parte de un grupo.

```csharp
private const string groupFriendlyName = "myGroup";
private const string groupId = "3F2504E0-4F89-41D3-9A0C-0305E82C3301";
private const string myTaskName = "My Background Trigger";

public static void RegisterBackgroundTaskInGroup()
{
   BackgroundTaskRegistrationGroup group = BackgroundTaskRegistration.GetTaskGroup(groupId);
   bool isTaskRegistered = false;

   // See if this task already belongs to a group
   if (group != null)
   {
       foreach (var taskKeyValue in group.AllTasks)
       {
           if (taskKeyValue.Value.Name == myTaskName)
           {
               isTaskRegistered = true;
               break;
           }
       }
   }

   // If the background task is not in a group, register it
   if (!isTaskRegistered)
   {
       if (group == null)
       {
           group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
       }

       var builder = new BackgroundTaskBuilder();
       builder.Name = myTaskName;
       builder.TaskGroup = group; // we specify the group, here
       builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));

       // Because builder.TaskEntryPoint is not specified, OnBackgroundActivated() will be raised when the background task is triggered
       BackgroundTaskRegistration task = builder.Register();
   }
}
```

## <a name="unregister-background-tasks-in-a-group"></a>Anular el registro de tareas en segundo plano en un grupo

A continuación se muestra cómo anular el registro de las tareas en segundo plano que se han registrado como parte de un grupo.
Dado que las tareas en segundo plano registradas en un grupo no aparecen en [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks), debe iterar a través de los grupos, buscar las tareas en segundo plano registradas para cada grupo y anular su registro.

```csharp
private static void UnRegisterAllTasks()
{
    // Unregister tasks that are part of a group
    foreach (var groupKeyValue in BackgroundTaskRegistration.AllTaskGroups)
    {
        foreach (var groupedTask in groupKeyValue.Value.AllTasks)
        {
            groupedTask.Value.Unregister(true); // passing true to cancel currently running instances of this background task
        }
    }

    // Unregister tasks that aren't part of a group
    foreach(var taskKeyValue in BackgroundTaskRegistration.AllTasks)
    {
        taskKeyValue.Value.Unregister(true); // passing true to cancel currently running instances of this background task
    }
}
```

## <a name="register-persistent-events"></a>Registrar eventos persistentes

Al usar los grupos de registro de tareas en segundo plano con tareas en segundo plano en proceso, las activaciones en segundo plano se dirigen hacia el evento del grupo del lugar del evento del objeto Application o CoreApplication. Esto permite que varios componentes dentro de la aplicación controlen la activación en lugar de colocar todas las rutas de código de activación en el objeto Application. A continuación se muestra cómo registrarse para el evento activado en segundo plano del grupo. Comprueba primero [BackgroundTaskRegistration.GetTaskGroup](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.gettaskgroup) para determinar si ya se ha registrado el grupo. De no ser así, crea un grupo nuevo con tu identificador y nombre descriptivo. A continuación, registra un controlador de eventos para el evento BackgroundActivated en el grupo.

```csharp
void RegisterPersistentEvent()
{
    var group = BackgroundTaskRegistration.GetTaskGroup(groupId);
    if (group == null)
    {
        group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
    }

    group.BackgroundActivated += MyEventHandler;
}
```
