---
author: TylerMSFT
title: Convertir una tarea en segundo plano fuera del proceso en una tarea en segundo plano dentro del proceso
description: "Puedes convertir una tarea en segundo plano fuera del proceso en una tarea en segundo plano dentro del proceso que se ejecuta en el proceso de la aplicación en primer plano."
translationtype: Human Translation
ms.sourcegitcommit: 7d1c160f8b725cd848bf8357325c6ca284b632ae
ms.openlocfilehash: b361a558ecef2030370590eedbef69bd04cf68bd

---

# Convertir una tarea en segundo plano fuera del proceso en una tarea en segundo plano dentro del proceso

La forma más sencilla de convertir tu actividad en segundo plano fuera del proceso en una actividad dentro del proceso consiste en introducir tu código de método [IBackgroundTask.Run](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) en la aplicación e iniciarlo desde [OnBackgroundActivated](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).

Si la aplicación tiene varias tareas en segundo plano, la [muestra de activación en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) muestra cómo usar `BackgroundActivatedEventArgs.TaskInstance.Task.Name` para identificar qué tarea se está iniciando.

Si actualmente estás comunicando procesos de segundo y primer plano, puedes quitar ese código de administración de estado y comunicación.

## Tareas en segundo plano y tipos de desencadenadores que no se pueden convertir

* Las tareas en segundo plano dentro del proceso no admiten la activación de una tarea en segundo plano de VoIP.
* Las tareas en segundo plano dentro del proceso no admiten los siguientes desencadenadores: [DeviceUseTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) e **IoTStartupTask**.



<!--HONumber=Nov16_HO1-->


