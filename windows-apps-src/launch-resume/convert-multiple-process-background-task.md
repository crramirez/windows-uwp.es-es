---
author: TylerMSFT
title: "Convertir una tarea en segundo plano multiproceso en una tarea en segundo plano de proceso único"
description: "Puedes convertir una tarea en segundo plano que se ejecuta en un proceso independiente en una tarea en segundo plano que se ejecuta en el proceso de la aplicación en primer plano."
translationtype: Human Translation
ms.sourcegitcommit: 2c34ca40d3c930254500477ab5a2e41e5206d823
ms.openlocfilehash: e342667347cf3b89a5aa193495cbf7195263b276

---

# Convertir una tarea en segundo plano multiproceso en una tarea en segundo plano de proceso único

La forma más sencilla de convertir tu actividad en segundo plano de varios procesos en un proceso único consiste en introducir tu código de método [IBackgroundTask.Run](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) en la aplicación e iniciarla desde [OnBackgroundActivated](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).

Si la aplicación tiene varias tareas en segundo plano, la [muestra de activación en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) muestra cómo usar `BackgroundActivatedEventArgs.TaskInstance.Task.Name` para identificar qué tarea se está iniciando.

Si actualmente estás comunicando procesos de segundo y primer plano, puedes quitar ese código de administración de estado y comunicación.

## Tareas en segundo plano y tipos de desencadenadores que no se pueden convertir

* Las tareas en segundo plano de proceso único no admiten la activación de una tarea en segundo plano de VoIP.
* Las tareas en segundo plano de proceso único no admiten los siguientes desencadenadores: [DeviceUseTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) e **IoTStartupTask**



<!--HONumber=Aug16_HO3-->


