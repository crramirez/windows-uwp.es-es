---
title: Migrar una tarea en segundo plano fuera de proceso a una tarea en segundo plano en proceso
description: Trasladar una tarea en segundo plano fuera de proceso a una tarea en segundo plano en proceso que se ejecuta dentro del proceso de aplicación en primer plano.
ms.date: 09/19/2018
ms.topic: article
keywords: Windows 10, UWP, tarea en segundo plano, App Service
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: f500be50c8b57415f5ad2fe62c28cc21f4b57b68
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162659"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>Migrar una tarea en segundo plano fuera de proceso a una tarea en segundo plano en proceso

La manera más sencilla de migrar la actividad en segundo plano fuera de proceso (OOP) a la actividad en proceso es traer el código del método [IBackgroundTask. Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396) dentro de la aplicación e iniciarlo desde [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated). La técnica que se describe aquí no trata sobre la creación de correcciones de compatibilidad (shim) desde una tarea en segundo plano OOP en una tarea en segundo plano en proceso; se trata de volver a escribir (o trasladar) una versión de OOP a una versión en proceso.

Si la aplicación tiene varias tareas en segundo plano, la [muestra de activación en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) muestra cómo usar `BackgroundActivatedEventArgs.TaskInstance.Task.Name` para identificar qué tarea se está iniciando.

Si actualmente estás comunicando procesos de segundo y primer plano, puedes quitar ese código de administración de estado y comunicación.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Tareas en segundo plano y tipos de desencadenadores que no se pueden convertir

* Las tareas en segundo plano dentro del proceso no admiten la activación de una tarea en segundo plano de VoIP.
* Las tareas en segundo plano en proceso no admiten los siguientes desencadenadores:  [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) y **IoTStartupTask**