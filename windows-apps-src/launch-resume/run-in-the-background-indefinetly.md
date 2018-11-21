---
author: TylerMSFT
title: Ejecutar en segundo plano de manera indefinida
description: Usa la funcionalidad extendedExecutionUnconstrained para ejecutar una tarea en segundo plano o una sesión de ejecución extendida en segundo plano de manera indefinida.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: tarea en segundo plano, ejecución extendida, recursos, límites, tarea en segundo plano
ms.author: twhitney
ms.date: 10/3/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: eb6e0735620c4a940d3414f22aaa4f09bb608424
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7431023"
---
# <a name="run-in-the-background-indefinitely"></a>Ejecutar en segundo plano de manera indefinida

Para proporcionar la mejor experiencia para los usuarios, Windows impone límites de recursos en aplicaciones de la Plataforma universal de Windows (UWP). Las aplicaciones en primer plano reciben la mayor cantidad de memoria y tiempo de ejecución; las aplicaciones en segundo plano obtienen menos. Los usuarios se protegen así de un rendimiento de la aplicación en primer plano deficiente y de una gran descarga de la batería.

Sin embargo, los desarrolladores que crean aplicaciones para UWP para uso personal (es decir, aplicaciones de prueba que no se publicarán en la Microsoft Store), o los desarrolladores que crean aplicaciones para UWP de empresa, podrían querer usar todos los recursos disponibles del dispositivo sin ninguna limitación de ejecución extendida o en segundo plano. Las aplicaciones para UWP personales y de línea de negocio pueden usar API en Windows Creators Update (versión 1703) para desactivar la limitación. Ten en cuenta que no puedes poner una app en la Microsoft Store si usa estas API.

## <a name="run-while-minimized"></a>Ejecutar mientras está minimizada

Las aplicaciones para UWP pasan a un estado suspendido cuando no se están ejecutando en primer plano. En el escritorio, esto se produce cuando un usuario minimiza la app. Las aplicaciones usan una sesión de ejecución ampliada para poder continuar en ejecución mientras están minimizadas. Las API de ejecución ampliada que se aceptan por la Microsoft Store se detallan en [Aplazar la suspensión de la aplicación con ejecución ampliada](https://docs.microsoft.com/windows/uwp/launch-resume/run-minimized-with-extended-execution).

Si estás desarrollando una app que no está pensada para enviarse a la Microsoft Store, puedes usar la [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) con la capacidad restringida `extendedExecutionUnconstrained` para que la app puede seguir ejecutándose mientras está minimizada, independientemente del estado de energía del dispositivo.  

La funcionalidad `extendedExecutionUnconstrained` se agrega como una funcionalidad restringida en el manifiesto de la aplicación. Consulta [Declaraciones de funcionalidades de las aplicaciones](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) para más información sobre las funcionalidades restringidas.

_Package.appxmanifest_
```xml
<Package ...>
...
  <Capabilities>  
    <rescap:Capability Name="extendedExecutionUnconstrained"/>  
  </Capabilities>  
</Package>
```

Cuando usas la funcionalidad `extendedExecutionUnconstrained`, se usan [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) y [ExtendedExecutionForegroundReason](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason) en lugar de [ExtendedExecutionSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) y [ExtendedExecutionReason](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason). Se sigue aplicando el mismo patrón para crear la sesión, establecer los miembros y solicitar la extensión de forma asincrónica: 

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

Puedes solicitar esta sesión de ejecución ampliada en cuanto la aplicación pase a primer plano. Las sesiones de ejecución ampliada sin restricciones no están limitadas por las cuotas de energía o por el ahorro de batería del sistema operativo. Siempre que exista una referencia al objeto de sesión, la aplicación permanecerá en el estado de ejecución y no entrará en el estado de suspensión. Si el usuario cierra la app, la sesión se revocará.

El registro para el evento **Revoked** permitirá que tu app realice cualquier trabajo de limpieza necesario. En el estado de suspensión, puedes crear una sesión de ejecución ampliada con [ExtendedExecutionReason.SavingData](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) para guardar datos de usuario antes de que la aplicación finalice y se quite de la memoria.

## <a name="run-background-tasks-indefinitely"></a>Ejecutar tareas en segundo plano de manera indefinida

En la Plataforma universal de Windows, las tareas en segundo plano son procesos que se ejecutan en segundo plano sin ninguna forma de interfaz de usuario. Por lo general, las tareas en segundo plano se pueden ejecutar para un máximo de 25 segundos antes de cancelarse. Algunas de las tareas que se han estado ejecutando durante más tiempo también tienen una comprobación para asegurarse de que la tarea en segundo plano no encuentra inactiva o usando memoria. En Windows Creators Update (versión 1703), se introdujo la funcionalidad restringida de [extendedBackgroundTaskTime](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) para quitar estos límites. La funcionalidad **extendedBackgroundTaskTime** se agrega como una funcionalidad restringida en el archivo de manifiesto de la aplicación:

_Package.appxmanifest_
```xml
<Package ...>
   <Capabilities>  
       <rescap:Capability Name="extendedBackgroundTaskTime"/>  
   </Capabilities>  
</Package>
```

Esta funcionalidad quita las limitaciones de tiempo de ejecución y el guardián de tareas inactivas. Una vez se ha iniciado una tarea en segundo plano, ya sea mediante un desencadenador o una llamada al servicio de aplicaciones, una vez que toma un aplazamiento en el valor de [BackgroundTaskInstance](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) proporcionado por el método **Ejecutar**, se puede ejecutar de manera indefinida. Si la aplicación se establece en **administrado por Windows**, todavía puede tener una cuota de energía aplicada y sus tareas en segundo plano no se activarán cuando el ahorro de batería esté activo.Esto se puede cambiar con la configuración del sistema operativo. Encontrarás más información disponible en [Optimizar la actividad en segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity).

La Plataforma universal de Windows supervisa la ejecución de la tarea en segundo plano para garantizar una buena duración de la batería y una experiencia sin problemas de aplicación en primer plano. Sin embargo, las aplicaciones personales y las aplicaciones de línea de negocio empresariales pueden usar la ejecución ampliada y la funcionalidad **extendedBackgroundTaskTime** para crear aplicaciones que se ejecutarán siempre que sea necesario independientemente de la disponibilidad de los recursos del dispositivo.

Ten en cuenta que las funcionalidades **extendedExecutionUnconstrained** y **extendedBackgroundTaskTime** pueden invalidar la directiva predeterminada para aplicaciones para UWP y podrían provocar una descarga importante de la batería. Antes de usar estas funcionalidades, primero confirma que las directivas de tiempo de tarea en segundo plano y ejecución ampliada predeterminadas no cumplen tus necesidades y realiza pruebas en condiciones de batería restringida para comprender el impacto que tendrá tu aplicación en un dispositivo.

## <a name="see-also"></a>Ver también

[Quitar las restricciones de recursos de tarea en segundo plano](https://docs.microsoft.com/windows/application-management/enterprise-background-activity-controls)
