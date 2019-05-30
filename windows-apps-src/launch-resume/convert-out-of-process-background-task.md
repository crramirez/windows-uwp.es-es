---
title: Migrar una tarea en segundo plano fuera de proceso a una tarea en segundo plano en proceso
description: Puerto de una tarea fuera de proceso en segundo plano en una tarea de proceso en segundo plano que se ejecuta en el proceso de aplicación de primer plano.
ms.date: 09/19/2018
ms.topic: article
keywords: Windows 10, uwp, tarea en segundo plano, el servicio de aplicación
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 42aaa5600b30924acc84aa61c2a15dbb2a320c10
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366258"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>Migrar una tarea en segundo plano fuera de proceso a una tarea en segundo plano en proceso

La manera más sencilla para portar su actividad en segundo plano fuera de proceso (OOP) para la actividad en el proceso consiste en llevar su [IBackgroundTask.Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396) método dentro de la aplicación de código e iniciar desde [OnBackgroundActivated ](/uwp/api/windows.ui.xaml.application.onbackgroundactivated). La técnica que se describe aquí no está sobre la creación de una corrección de compatibilidad de una tarea en segundo plano OOP para una tarea en segundo plano en curso; su reescritura sobre (o migrar) una versión de la programación orientada a objetos a una versión en proceso.

Si la aplicación tiene varias tareas en segundo plano, la [muestra de activación en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) muestra cómo usar `BackgroundActivatedEventArgs.TaskInstance.Task.Name` para identificar qué tarea se está iniciando.

Si actualmente estás comunicando procesos de segundo y primer plano, puedes quitar ese código de administración de estado y comunicación.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Tareas en segundo plano y tipos de desencadenadores que no se pueden convertir

* Las tareas en segundo plano dentro del proceso no admiten la activación de una tarea en segundo plano de VoIP.
* Tareas en proceso en segundo plano no admiten los siguientes desencadenadores:  [DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) y **IoTStartupTask**
