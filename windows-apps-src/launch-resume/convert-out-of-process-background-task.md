---
author: TylerMSFT
title: Migrar una tarea en segundo plano fuera de proceso a una tarea en segundo plano en proceso
description: Migrar una tarea en segundo plano fuera de proceso en una tarea en segundo plano en proceso que se ejecuta dentro de su proceso de la aplicación en primer plano.
ms.author: twhitney
ms.date: 09/19/2018
ms.topic: article
keywords: Windows 10, uwp, tarea en segundo plano, servicio de aplicaciones
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 47008fd7ba0b7724aa8fbdc2dd6cbd55288faea0
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "6164701"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>Migrar una tarea en segundo plano fuera de proceso a una tarea en segundo plano en proceso

La forma más sencilla de migrar tu actividad en segundo plano fuera de proceso (OOP) actividad dentro del proceso es llevar tu código de método [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) dentro de la aplicación e iniciarlo desde [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated). La técnica que se describe aquí no está sobre la creación de una corrección de una tarea en segundo plano OOP en una tarea en segundo plano en proceso; su información sobre cómo volver a escribir (o migración) una versión OOP a un proceso.

Si la aplicación tiene varias tareas en segundo plano, la [muestra de activación en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) muestra cómo usar `BackgroundActivatedEventArgs.TaskInstance.Task.Name` para identificar qué tarea se está iniciando.

Si actualmente estás comunicando procesos de segundo y primer plano, puedes quitar ese código de administración de estado y comunicación.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Tareas en segundo plano y tipos de desencadenadores que no se pueden convertir

* Las tareas en segundo plano dentro del proceso no admiten la activación de una tarea en segundo plano de VoIP.
* Las tareas en segundo plano dentro del proceso no admiten los siguientes desencadenadores: [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) e **IoTStartupTask**.
