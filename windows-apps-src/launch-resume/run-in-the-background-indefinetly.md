---
title: Ejecutar en segundo plano de manera indefinida
description: Use la funcionalidad extendedExecutionUnconstrained para ejecutar una tarea en segundo plano o una sesión de ejecución extendida en segundo plano indefinidamente.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: tarea en segundo plano, ejecución extendida, recursos, límites, tarea en segundo plano
ms.date: 10/03/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f843c23a4a1e0738cfc05e96009b2597f4919809
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217658"
---
# <a name="run-in-the-background-indefinitely"></a>Ejecutar en segundo plano de manera indefinida

Para proporcionar la mejor experiencia para los usuarios, Windows impone límites de recursos en las aplicaciones Plataforma universal de Windows (UWP). Las aplicaciones de primer plano reciben la mayor cantidad de memoria y tiempo de ejecución; las aplicaciones en segundo plano son menos. Por tanto, los usuarios están protegidos de un rendimiento deficiente de la aplicación en primer plano y de un consumo de batería intenso.

Sin embargo, los desarrolladores que escriben aplicaciones para UWP para su uso personal (es decir, las aplicaciones de carga lateral que no se publicarán en el Microsoft Store) o los desarrolladores que escriben aplicaciones de UWP para la empresa, pueden querer usar todos los recursos disponibles en el dispositivo sin ninguna limitación de la ejecución en segundo plano o extendida. Las aplicaciones de línea de negocio y personal UWP pueden usar las API de Windows Creators Update (versión 1703) para desactivar la limitación. Tenga en cuenta que no puede poner una aplicación en el Microsoft Store si usa estas API.

## <a name="run-while-minimized"></a>Ejecutar mientras está minimizada

Las aplicaciones UWP se mueven a un estado suspendido cuando no se ejecutan en primer plano. En el escritorio, esto ocurre cuando un usuario minimiza la aplicación. Las aplicaciones usan una sesión de ejecución extendida para seguir ejecutando mientras se minimizan. Las API de ejecución extendida que son aceptadas por el Microsoft Store se detallan en [posponer la suspensión de la aplicación con la ejecución extendida](./run-minimized-with-extended-execution.md).

Si está desarrollando una aplicación que no está pensada para enviarse al Microsoft Store, puede usar la [ExtendedExecutionForegroundSession](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) con la `extendedExecutionUnconstrained` capacidad restringida para que la aplicación pueda continuar ejecutándose mientras esté minimizada, independientemente del estado de energía del dispositivo.  

La `extendedExecutionUnconstrained` funcionalidad se agrega como una capacidad restringida en el manifiesto de la aplicación. Vea [declaraciones](../packaging/app-capability-declarations.md) de funcionalidades de aplicación para obtener más información sobre las funcionalidades restringidas.

> [!NOTE]
> Agregue la declaración de espacio de nombres XML *xmlns: ResCap* y use el prefijo *ResCap* para declarar la funcionalidad.
>
> Para obtener más información, consulte la sección funcionalidades restringidas de [declaraciones de funcionalidades de aplicación](../packaging/app-capability-declarations.md).
>

_Package.appxmanifest_

```xml
<Package
    ...
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp rescap">
  ...
  <Capabilities>
    <rescap:Capability Name="extendedExecutionUnconstrained"/>
  </Capabilities>
</Package>
```

Cuando se usa la `extendedExecutionUnconstrained` funcionalidad, se usan [ExtendedExecutionForegroundSession](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) y [ExtendedExecutionForegroundReason](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason) en lugar de [ExtendedExecutionSession](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) y [ExtendedExecutionReason](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason). Se sigue aplicando el mismo patrón para la creación de la sesión, la configuración de miembros y la solicitud de la extensión de forma asincrónica: 

```cs
var newSession = new ExtendedExecutionForegroundSession();
newSession.Reason = ExtendedExecutionForegroundReason.Unconstrained;
newSession.Description = "Long Running Processing";
newSession.Revoked += SessionRevoked;
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();
switch (result)
{
    case ExtendedExecutionResult.Allowed:
        DoLongRunningWork();
        break;

    default:
    case ExtendedExecutionResult.Denied:
        DoShortRunningWork();
        break;
}
```

Puede solicitar esta sesión de ejecución extendida en cuanto la aplicación llegue al primer plano. Las sesiones de ejecución extendida sin restricciones no están limitadas por las cuotas de energía ni por el protector de batería del sistema operativo. Siempre que exista una referencia al objeto de sesión, la aplicación permanecerá en el estado en ejecución y no entrará en el estado suspendido. Si el usuario cierra la aplicación, se revocará la sesión.

El registro del evento **revocado** permitirá a la aplicación realizar cualquier trabajo de limpieza necesario. En el estado de suspensión, puede crear una sesión de ejecución extendida con   [ExtendedExecutionReason. SavingData](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) para guardar los datos de usuario antes de que se termine la aplicación y se quite de la memoria.

## <a name="run-background-tasks-indefinitely"></a>Ejecutar tareas en segundo plano indefinidamente

En el Plataforma universal de Windows, las tareas en segundo plano son procesos que se ejecutan en segundo plano sin ninguna forma de interfaz de usuario. Por lo general, las tareas en segundo plano pueden ejecutarse durante un máximo de veinticinco segundos antes de que se cancelen. Algunas de las tareas de ejecución más larga también tienen una comprobación para asegurarse de que la tarea en segundo plano no esté inactiva o usando memoria. En Windows Creators Update (versión 1703), se presentó la funcionalidad restringida [extendedBackgroundTaskTime](../packaging/app-capability-declarations.md) para quitar estos límites. La funcionalidad **extendedBackgroundTaskTime** se agrega como una capacidad restringida en el archivo de manifiesto de la aplicación:

> [!NOTE]
> Agregue la declaración de espacio de nombres XML *xmlns: ResCap* y use el prefijo *ResCap* para declarar la funcionalidad.
>
> Para obtener más información, consulte la sección funcionalidades restringidas de [declaraciones de funcionalidades de aplicación](../packaging/app-capability-declarations.md).
>

_Package.appxmanifest_

```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp rescap">
...
  <Capabilities>
    <rescap:Capability Name="extendedBackgroundTaskTime"/>
  </Capabilities>
</Package>
```

Esta funcionalidad quita las limitaciones de tiempo de ejecución y el guardián de tareas inactivas. Una vez que se ha iniciado una tarea en segundo plano, ya sea mediante un desencadenador o una llamada a App Service, una vez que toma un aplazamiento en el [BackgroundTaskInstance](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) proporcionado por el método **Run** , se puede ejecutar indefinidamente. Si la aplicación se establece en **administrado por Windows**, todavía puede tener una cuota de energía aplicada y sus tareas en segundo plano no se activarán cuando el ahorro de batería esté activo.Esto puede cambiarse con la configuración del sistema operativo. Hay más información disponible en [optimización de la actividad en segundo plano](../debug-test-perf/optimize-background-activity.md).

El Plataforma universal de Windows supervisa la ejecución de tareas en segundo plano para garantizar una buena duración de la batería y una experiencia de aplicación de primer plano fluida. Sin embargo, las aplicaciones personales y las aplicaciones de línea de negocio de empresa pueden usar la ejecución extendida y la capacidad de **extendedBackgroundTaskTime** para crear aplicaciones que se ejecuten siempre que sea necesario, independientemente de la disponibilidad de los recursos del dispositivo.

Tenga en cuenta que las funcionalidades de **extendedExecutionUnconstrained** y **extendedBackgroundTaskTime** pueden invalidar la directiva predeterminada para las aplicaciones para UWP y pueden provocar una pérdida de batería significativa. Antes de usar estas funcionalidades, primero confirme que las directivas de ejecución extendida predeterminada y tiempo de tarea en segundo plano no satisfagan sus necesidades y realice pruebas en condiciones con restricción de batería para comprender el impacto que tendrá la aplicación en un dispositivo.

## <a name="see-also"></a>Consulte también

[Quitar las restricciones de recursos de tareas en segundo plano](/windows/application-management/enterprise-background-activity-controls)