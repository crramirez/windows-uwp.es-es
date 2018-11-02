---
author: normesta
ms.assetid: 95CF7F3D-9E3A-40AC-A083-D8A375272181
title: Procedimientos recomendados para usar el grupo de subprocesos
description: En este tema se describen los procedimientos recomendados para trabajar con el grupo de subprocesos.
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, subprocesos, grupo de subprocesos
ms.localizationpriority: medium
ms.openlocfilehash: ff607e3b39ea9d9a3731cc1f231fe1eb27b0b155
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5939048"
---
# <a name="best-practices-for-using-the-thread-pool"></a>Procedimientos recomendados para usar el grupo de subprocesos

En este tema se describen los procedimientos recomendados para trabajar con el grupo de subprocesos.

## <a name="dos"></a>Qué hacer


-   Usa el grupo de subprocesos para realizar trabajos paralelos en tu aplicación.

-   Usa elementos de trabajo para realizar tareas largas sin bloquear el subproceso de interfaz de usuario.

-   Crea elementos de trabajo de corta duración e independientes. Los elementos de trabajo se ejecutan de manera asincrónica y pueden enviarse al grupo en cualquier orden desde la cola.

-   Distribuye actualizaciones al subproceso de interfaz de usuario con el [**Windows.UI.Core.CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211).

-   Usa [**ThreadPoolTimer.CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921) en lugar de la función **Sleep**.

-   Usa el grupo de subprocesos en lugar de crear tu propio sistema de administración de subprocesos. El grupo de subprocesos se ejecuta en el nivel del sistema operativo con funcionalidades avanzadas y está optimizado para escalar dinámicamente según los recursos y la actividad del dispositivo en el proceso y en todo el sistema.

-   En C++, asegúrate de que los delegados del elemento de trabajo usen el modelo de subprocesos ágil (los delegados de C++ son ágiles de manera predeterminada).

-   Usa elementos de trabajo preasignados cuando no puedes tolerar un error de asignación de recursos en el momento de uso.

## <a name="donts"></a>Qué no hacer


-   No crees temporizadores periódicos con un valor *period* de &lt;1 milisegundo (incluido 0). Esto hará que el elemento de trabajo se comporte como un temporizador de único disparo.

-   No envíes elementos de trabajo periódicos que demoren más en completarse que la cantidad de tiempo especificada en el parámetro *period*.

-   No intentes enviar actualizaciones de interfaz de usuario (que no sean notificaciones del sistema y notificaciones) desde un elemento de trabajo que proviene de una tarea en segundo plano. En lugar de ello, usa los controladores de progreso y finalización de tareas en segundo plano, por ejemplo, [**IBackgroundTaskInstance.Progress**](https://msdn.microsoft.com/library/windows/apps/BR224800).

-   Cuando usas controladores de elemento de trabajo que usen la palabra clave **async**, ten en cuenta que el elemento de trabajo del grupo de subprocesos se puede establecer en el estado completo antes de que se haya ejecutado todo el código en el controlador. Es posible que el código que sigue a una palabra clave **await** dentro del controlador se ejecute después de que el elemento de trabajo se haya establecido en el estado completo.

-   No intentes ejecutar un elemento de trabajo preasignado más de una vez sin reiniciarlo. [Crear un elemento de trabajo periódico](create-a-periodic-work-item.md)

## <a name="related-topics"></a>Temas relacionados


* [Crear un elemento de trabajo periódico](create-a-periodic-work-item.md)
* [Enviar un elemento de trabajo al grupo de subprocesos](submit-a-work-item-to-the-thread-pool.md)
* [Enviar un elemento de trabajo con un temporizador](use-a-timer-to-submit-a-work-item.md)
