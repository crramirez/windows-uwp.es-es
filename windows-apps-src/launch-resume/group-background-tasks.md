---
title: Agrupar el registro de tareas en segundo plano
description: Registrar o anular el registro de las tareas en segundo plano como parte de un grupo para aislar esos registros.
ms.date: 04/05/2017
ms.topic: article
keywords: Windows 10, tarea en segundo plano
ms.openlocfilehash: 61419ac45acd27758e3c874ac4b03510561ccaf8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155869"
---
# <a name="group-background-task-registration"></a>Agrupar el registro de tareas en segundo plano

**API importantes**

[Clase BackgroundTaskRegistrationGroup](/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup)

Ahora se pueden registrar tareas en segundo plano en un grupo, que se puede considerar como un espacio de nombres lógico. Este aislamiento ayuda a garantizar que los distintos componentes de una aplicación, o bibliotecas diferentes, no interfieran con el registro de tareas en segundo plano de los demás.

Cuando una aplicación y el marco (o biblioteca) que usa registra una tarea en segundo plano con el mismo nombre, la aplicación podría quitar accidentalmente los registros de tareas en segundo plano del marco. Los autores de aplicaciones también podrían quitar accidentalmente los registros de tareas en segundo plano de Marcos y bibliotecas, ya que podrían anular el registro de todas las tareas en segundo plano registradas mediante [BackgroundTaskRegistration. AllTasks](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks).  Con los grupos, puede aislar los registros de tareas en segundo plano para que esto no suceda.

## <a name="features-of-groups"></a>Características de los grupos

* Los grupos se pueden identificar de forma única mediante un GUID. También pueden tener una cadena de nombre descriptivo asociada que es más fácil de leer durante la depuración.
* Se pueden registrar varias tareas en segundo plano en un grupo.
* Las tareas en segundo plano registradas en un grupo no aparecerán en [BackgroundTaskRegistration. AllTasks](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks). Por lo tanto, las aplicaciones que usan actualmente **BackgroundTaskRegistration. AllTasks** para anular el registro de sus tareas no eliminarán accidentalmente las tareas en segundo plano registradas en un grupo. Consulte [anulación del registro de tareas en segundo plano en un grupo](#unregister-background-tasks-in-a-group) siguiente para ver cómo anular el registro de todos los desencadenadores en segundo plano que se han registrado como parte de un grupo.
* Cada registro de tareas en segundo plano tendrá una propiedad de grupo para determinar a qué grupo está asociado.
* El registro de tareas en segundo plano en proceso con un grupo hará que la activación pase por el evento [BackgroundTaskRegistrationGroup. BackgroundActivated](/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup.BackgroundActivated) en lugar de [Application. OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated#Windows_UI_Xaml_Application_OnBackgroundActivated_Windows_ApplicationModel_Activation_BackgroundActivatedEventArgs_).

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

A continuación se muestra cómo anular el registro de las tareas en segundo plano que se registraron como parte de un grupo.
Dado que las tareas en segundo plano registradas en un grupo no aparecen en [BackgroundTaskRegistration. AllTasks](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks), debe recorrer en iteración los grupos, buscar las tareas en segundo plano registradas en cada grupo y anular su registro.

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

Cuando se usan grupos de registro de tareas en segundo plano con tareas en segundo plano en proceso, las activaciones en segundo plano se dirigen al evento del grupo en lugar de a la del objeto de aplicación o CoreApplication. Esto permite que varios componentes de la aplicación controlen la activación en lugar de colocar todas las rutas de acceso del código de activación en el objeto de aplicación. A continuación se muestra cómo registrarse para el evento activado en segundo plano del grupo. En primer lugar, compruebe [BackgroundTaskRegistration. GetTaskGroup](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.gettaskgroup) para determinar si el grupo ya se ha registrado. Si no es así, cree un nuevo grupo con el identificador y el nombre descriptivo. A continuación, registre un controlador de eventos en el evento BackgroundActivated del grupo.

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