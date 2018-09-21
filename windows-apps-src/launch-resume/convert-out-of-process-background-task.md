---
author: TylerMSFT
title: Una tarea en segundo plano de fuera de proceso para una tarea en segundo plano en el proceso de puerto
description: Puerto de una tarea en segundo plano de fuera de proceso para una tarea en segundo plano en el proceso que se ejecuta dentro de su proceso de la aplicación en primer plano.
ms.author: twhitney
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10 uwp, tarea en segundo plano, servicio de aplicación
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: b9010f82b0460bd46757bc1e0d58c01dec459104
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/21/2018
ms.locfileid: "4114421"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>Una tarea en segundo plano de fuera de proceso para una tarea en segundo plano en el proceso de puerto

La forma más sencilla de trasladar su actividad en segundo plano fuera de proceso (OOP) para la actividad en el proceso es poner el código del método [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) dentro de la aplicación e iniciarlo desde [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated). La técnica que se describe aquí no es acerca de la creación de una corrección de una tarea en segundo plano OOP para una tarea en segundo plano en proceso; de TI acerca de reescritura (o trasladar) una versión de programación orientada a objetos a una versión en proceso.

Si la aplicación tiene varias tareas en segundo plano, la [muestra de activación en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) muestra cómo usar `BackgroundActivatedEventArgs.TaskInstance.Task.Name` para identificar qué tarea se está iniciando.

Si actualmente estás comunicando procesos de segundo y primer plano, puedes quitar ese código de administración de estado y comunicación.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Tareas en segundo plano y tipos de desencadenadores que no se pueden convertir

* Las tareas en segundo plano dentro del proceso no admiten la activación de una tarea en segundo plano de VoIP.
* Las tareas en segundo plano dentro del proceso no admiten los siguientes desencadenadores: [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) e **IoTStartupTask**.
