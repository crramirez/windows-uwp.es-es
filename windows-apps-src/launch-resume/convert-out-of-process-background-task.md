---
author: TylerMSFT
title: Convertir una tarea en segundo plano fuera del proceso en una tarea en segundo plano dentro del proceso
description: Puedes convertir una tarea en segundo plano fuera del proceso en una tarea en segundo plano dentro del proceso que se ejecuta en el proceso de la aplicación en primer plano.
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, tarea en segundo plano, el servicio de aplicación
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 1144443f943f134991d050dea1457f252eaaf36d
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "2829731"
---
# <a name="convert-an-out-of-process-background-task-to-an-in-process-background-task"></a>Convertir una tarea en segundo plano fuera del proceso en una tarea en segundo plano dentro del proceso

La forma más sencilla de convertir tu actividad en segundo plano fuera del proceso en una actividad dentro del proceso consiste en introducir tu código de método [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) en la aplicación e iniciarlo desde [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).

Si la aplicación tiene varias tareas en segundo plano, la [muestra de activación en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) muestra cómo usar `BackgroundActivatedEventArgs.TaskInstance.Task.Name` para identificar qué tarea se está iniciando.

Si actualmente estás comunicando procesos de segundo y primer plano, puedes quitar ese código de administración de estado y comunicación.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Tareas en segundo plano y tipos de desencadenadores que no se pueden convertir

* Las tareas en segundo plano dentro del proceso no admiten la activación de una tarea en segundo plano de VoIP.
* Las tareas en segundo plano dentro del proceso no admiten los siguientes desencadenadores: [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) e **IoTStartupTask**.
