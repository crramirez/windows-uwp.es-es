---
title: 'Uso de streaming: .NET TraceProcessing'
description: En este tutorial, aprenderá a usar streaming para tener acceso a los datos de seguimiento de inmediato y usar menos memoria.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: e04f306a6a5c03d1f502b9cfb6c2cbb737e0098f
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629086"
---
# <a name="use-streaming-with-traceprocessor"></a>Uso de streaming con TraceProcessor

De forma predeterminada, TraceProcessor accede a los datos cargándolo en la memoria a medida que se procesa el seguimiento. Este enfoque de almacenamiento en búfer es fácil de usar, pero puede ser costoso en términos de uso de memoria.

TraceProcessor también proporciona seguimiento. UseStreaming (), que admite el acceso a varios tipos de datos de seguimiento de una manera de transmisión por secuencias (procesando datos a medida que se leen desde el archivo de seguimiento, en lugar de almacenar en búfer los datos en memoria). Por ejemplo, un seguimiento de llamadas syscall puede ser bastante grande y el almacenamiento en búfer de toda la lista de llamadas syscall en un seguimiento puede ser bastante costoso.

## <a name="accessing-buffered-data"></a>Obtener acceso a los datos almacenados en búfer

En el código siguiente se muestra el acceso a los datos syscall de forma normal y almacenada en búfer mediante seguimiento. UseSyscalls():

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 1)
        {
            Console.Error.WriteLine("Usage: <trace.etl>");
            return;
        }

        using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
        {
            IPendingResult<ISyscallDataSource> pendingSyscallData = trace.UseSyscalls();

            trace.Process();

            ISyscallDataSource syscallData = pendingSyscallData.Result;

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            foreach (ISyscall syscall in syscallData.Syscalls)
            {
                IProcess process = syscall.Thread?.Process;

                if (process == null)
                {
                    continue;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            }

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="accessing-streaming-data"></a>Acceso a los datos de streaming

Con un seguimiento llamadas syscall de gran tamaño, el intento de almacenar en búfer los datos syscall en la memoria puede ser bastante costoso, o incluso podría no ser posible. En el código siguiente se muestra cómo obtener acceso a los mismos datos syscall de una manera de streaming, reemplazando Trace. UseSyscalls () con seguimiento. UseStreaming(). UseSyscalls():

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 1)
        {
            Console.Error.WriteLine("Usage: <trace.etl>");
            return;
        }

        using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
        {
            IPendingResult<IThreadDataSource> pendingThreadData = trace.UseThreads();

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            trace.UseStreaming().UseSyscalls(ConsumerSchedule.SecondPass, context =>
            {
                Syscall syscall = context.Data;
                IProcess process = syscall.GetThread(pendingThreadData.Result)?.Process;

                if (process == null)
                {
                    return;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            });

            trace.Process();

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="how-streaming-works"></a>Cómo funciona la transmisión por secuencias

De forma predeterminada, todos los datos de streaming se proporcionan durante el primer paso del seguimiento, y los datos almacenados en búfer de otros orígenes no están disponibles. En el ejemplo anterior se muestra cómo combinar la transmisión por secuencias con el almacenamiento en búfer: los datos del subproceso se almacenan en búfer antes de que se transmitan los datos syscall. Como resultado, el seguimiento se debe leer dos veces: una vez para obtener los datos de subprocesos almacenados en búfer y una segunda vez para tener acceso a los datos syscall de streaming con los datos de subprocesos almacenados en búfer disponibles. Para combinar la transmisión por secuencias y el almacenamiento en búfer de esta manera, el ejemplo pasa ConsumerSchedule. SecondPass al seguimiento. UseStreaming(). UseSyscalls (), que hace que se produzca el procesamiento de syscall en un segundo paso a través del seguimiento. Al ejecutar en un segundo paso, la devolución de llamada syscall puede tener acceso al resultado pendiente desde el seguimiento. UseThreads () cuando procesa cada syscall. Sin este argumento opcional, la transmisión de syscall se habría ejecutado en el primer paso del seguimiento (solo habría un paso) y el resultado pendiente de seguimiento. UseThreads () todavía no estaba disponible. En ese caso, la devolución de llamada seguirá teniendo acceso al ThreadId desde syscall, pero no tendría acceso al proceso para el subproceso (porque el subproceso para procesar los datos de vinculación se proporciona a través de otros eventos que es posible que aún no se hayan procesado).

Algunas diferencias clave en el uso entre el almacenamiento en búfer y el streaming:

1. El almacenamiento en búfer devuelve un [&gt;IPendingResult&lt;t ](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.ipendingresult-1)y el resultado que contiene solo está disponible antes de que se haya procesado el seguimiento. Una vez procesado el seguimiento, los resultados se pueden enumerar mediante técnicas como foreach y LINQ.
2. Streaming devuelve void y, en su lugar, toma un argumento de devolución de llamada. Llama una vez a la devolución de llamada cuando cada elemento está disponible. Dado que los datos no se almacenan en búfer, nunca hay una lista de resultados que se deben enumerar con foreach o LINQ: la devolución de llamada de streaming necesita almacenar en búfer la parte de los datos que desea guardar para su uso una vez completado el procesamiento.
3. El código para procesar los datos almacenados en búfer aparece después de la llamada a Trace. Process (), cuando los resultados pendientes están disponibles.
4. El código para procesar los datos de streaming aparece antes de la llamada a Trace. Process (), como devolución de llamada al seguimiento. UseStreaming. use... método ().
5. Un consumidor de streaming puede elegir procesar solo parte de la secuencia y cancelar futuras devoluciones de llamada mediante una llamada a context. Cancelar (). A un consumidor de almacenamiento en búfer siempre se le proporciona una lista completa y almacenada en búfer.

## <a name="correlated-streaming-data"></a>Datos de streaming correlacionados

A veces, los datos de seguimiento se incluyen en una secuencia de eventos; por ejemplo, los llamadas syscall se registran mediante eventos Enter y Exit independientes, pero los datos combinados de ambos eventos pueden resultar más útiles. Seguimiento de método. UseStreaming(). UseSyscalls () correlaciona los datos de ambos eventos y los proporciona a medida que el par está disponible. Hay algunos tipos de datos correlacionados disponibles a través del seguimiento. UseStreaming():

| Código                                        | Descripción                                                                                                                                     |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| seguimiento. UseStreaming(). UseContextSwitchData() | Transmite datos de cambio de contexto correlacionado (de eventos compactos y no compactos, con SwitchInThreadIds más precisos que los eventos sin formato de compactación). |
| seguimiento. UseStreaming(). UseScheduledTasks()    | Transmite datos de tareas programadas correlacionadas.                                                                                                         |
| seguimiento. UseStreaming(). UseSyscalls()          | Transmite datos de llamadas del sistema correlacionados.                                                                                                            |
| seguimiento. UseStreaming(). UseWindowInFocus()     | Transmite datos correlacionados de ventana en el foco.                                                                                                        |

## <a name="standalone-streaming-events"></a>Eventos de streaming independiente

Además, trace. UseStreaming () proporciona eventos analizados para varios tipos de eventos independientes distintos:

| Código                                               | Descripción                                     |
|----------------------------------------------------|-------------------------------------------------|
| seguimiento. UseStreaming(). UseLastBranchRecordEvents()   | Secuencias analizadas los últimos eventos de registro de bifurcación (LBR). |
| seguimiento. UseStreaming(). UseReadyThreadEvents()        | Secuencias analizadas eventos de subprocesos listos.             |
| seguimiento. UseStreaming(). UseThreadCreateEvents()       | Secuencias análisis de eventos de creación de subprocesos.            |
| seguimiento. UseStreaming(). UseThreadExitEvents()         | Secuencias de eventos de salida de subprocesos analizados.              |
| seguimiento. UseStreaming(). UseThreadRundownStartEvents() | Eventos de inicio detallado de subproceso analizado de secuencias.     |
| seguimiento. UseStreaming(). UseThreadRundownStopEvents()  | Eventos de detención de detención de subprocesos analizados.      |
| seguimiento. UseStreaming(). UseThreadSetNameEvents()      | Flujos analizados eventos de nombre de conjunto de subprocesos.          |

## <a name="underlying-streaming-events-for-correlated-data"></a>Eventos de streaming subyacentes para datos correlacionados

Por último, trace. UseStreaming () también proporciona los eventos subyacentes que se usan para correlacionar los datos de la lista anterior. Estos eventos subyacentes son:

| Código                                                        | Descripción                                                                                | Se incluye en                                 |
|-------------------------------------------------------------|--------------------------------------------------------------------------------------------|---------------------------------------------|
| seguimiento. UseStreaming(). UseCompactContextSwitchEvents()        | Secuencias analizadas eventos de cambio de contexto de compactos.                                              | seguimiento. UseStreaming(). UseContextSwitchData() |
| seguimiento. UseStreaming(). UseContextSwitchEvents()               | Eventos de cambio de contexto analizados de secuencias. Es posible que SwitchInThreadIds no sea preciso en algunos casos. | seguimiento. UseStreaming(). UseContextSwitchData() |
| seguimiento. UseStreaming(). UseFocusChangeEvents()                 | Eventos de cambio de foco de ventana analizados.                                                 | seguimiento. UseStreaming(). UseWindowInFocus()     |
| seguimiento. UseStreaming(). UseScheduledTaskStartEvents()          | Secuencias analizadas eventos de inicio de tarea programada.                                                | seguimiento. UseStreaming(). UseScheduledTasks()    |
| seguimiento. UseStreaming(). UseScheduledTaskStopEvents()           | Secuencias analizadas eventos de detención de tarea programada.                                                 | seguimiento. UseStreaming(). UseScheduledTasks()    |
| seguimiento. UseStreaming(). UseScheduledTaskTriggerEvents()        | Secuencias analizadas eventos del desencadenador de tareas programadas.                                              | seguimiento. UseStreaming(). UseScheduledTasks()    |
| seguimiento. UseStreaming(). UseSessionLayerSetActiveWindowEvents() | Secuencia de la capa de sesión analizada eventos de la ventana activa.                                     | seguimiento. UseStreaming(). UseWindowInFocus()     |
| seguimiento. UseStreaming(). UseSyscallEnterEvents()                | Streams analizados syscall Enter Events.                                                       | seguimiento. UseStreaming(). UseSyscalls()          |
| seguimiento. UseStreaming(). UseSyscallExitEvents()                 | Secuencias analizadas eventos de salida syscall.                                                        | seguimiento. UseStreaming(). UseSyscalls()          |

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a usar streaming para tener acceso a los datos de seguimiento de inmediato y usar menos memoria.

El siguiente paso consiste en ver el acceso a los datos que desea de los seguimientos. Examine los [ejemplos](https://github.com/microsoft/eventtracing-processing-samples) para ver algunas ideas. Tenga en cuenta que no todos los seguimientos incluyen todos los tipos de datos admitidos.
