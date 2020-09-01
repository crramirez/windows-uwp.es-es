---
title: 'Inicio rápido proceso de un seguimiento: .NET TraceProcessing'
description: En esta guía de inicio rápido, aprenderá a acceder a los datos en un seguimiento de ETW.
author: maiak
ms.author: maiak
ms.date: 02/20/2020
ms.topic: quickstart
ms.openlocfilehash: 5f8671d8a1490710837908bb4df8f4aa3c99ecdd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172659"
---
# <a name="quickstart-process-your-first-trace"></a>Inicio rápido: procesar el primer seguimiento

Pruebe TraceProcessor para tener acceso a los datos en un seguimiento de seguimiento de eventos para Windows (ETW). TraceProcessor permite tener acceso a los datos de seguimiento de ETW como objetos .NET.

En esta guía de inicio rápido aprenderá a:

1. Instale el paquete NuGet de TraceProcessing.
2. Cree un TraceProcessor.
3. Use TraceProcessor para tener acceso a las líneas de comandos de proceso contenidas en el seguimiento.

## <a name="prerequisites"></a>Requisitos previos

Visual Studio 2019

## <a name="install-the-traceprocessing-nuget-package"></a>Instalación del paquete NuGet de TraceProcessing

.NET TraceProcessing está disponible en [NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) con el siguiente identificador de paquete:

Microsoft. Windows. EventTracing. Processing. All

Puede usar este paquete en una aplicación de consola para mostrar las líneas de comandos de proceso contenidas en un seguimiento de ETW (archivo. ETL).

1. Cree una nueva aplicación de consola de .NET Core. En Visual Studio, seleccione Archivo, nuevo, proyecto... y seleccione la plantilla aplicación de consola (.NET Core) para C#.

    Escriba un nombre de proyecto, por ejemplo, TraceProcessorQuickstart y elija crear.

2. En Explorador de soluciones, haga clic con el botón derecho en dependencias y elija administrar paquetes de NuGet... y cambie a la pestaña examinar.

3. En el cuadro de búsqueda, escriba Microsoft. Windows. EventTracing. Processing. All y busque.

    Seleccione instalar en el paquete de NuGet con ese nombre y cierre la ventana de NuGet.

## <a name="create-a-traceprocessor"></a>Creación de un TraceProcessor

1. Cambie Program.cs al siguiente contenido:

    ```csharp
    using Microsoft.Windows.EventTracing;
    using Microsoft.Windows.EventTracing.Processes;
    using System;

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
                // TODO: call trace.Use...

                trace.Process();

                Console.WriteLine("TODO: Access data from the trace");
            }
        }
    }
    ```

2. Proporcione un nombre de seguimiento para utilizarlo al ejecutar el proyecto.

    En Explorador de soluciones, haga clic con el botón derecho en el proyecto y elija Propiedades. Cambie a la pestaña depurar y escriba la ruta de acceso a un seguimiento (archivo. ETL) en los argumentos de la aplicación.

    Si aún no tiene un archivo de seguimiento, puede usar la [grabadora de rendimiento de Windows](/windows-hardware/test/wpt/start-a-recording) para crear uno.

3. Ejecute la aplicación.

    Elija depurar, iniciar sin depurar para ejecutar el código.

## <a name="use-traceprocessor-to-access-process-command-lines-contained-in-the-trace"></a>Usar TraceProcessor para tener acceso a las líneas de comandos del proceso incluidas en el seguimiento

1. Cambie Program.cs al siguiente contenido:

    ```csharp
    using Microsoft.Windows.EventTracing;
    using Microsoft.Windows.EventTracing.Processes;
    using System;

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
                IPendingResult<IProcessDataSource> pendingProcessData = trace.UseProcesses();

                trace.Process();

                IProcessDataSource processData = pendingProcessData.Result;

                foreach (IProcess process in processData.Processes)
                {
                    Console.WriteLine(process.CommandLine);
                }
            }
        }
    }
    ```

2. Vuelva a ejecutar la aplicación.

    Esta vez, debería ver una lista de líneas de comandos de todos los procesos que se estaban ejecutando mientras se registraba el seguimiento.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado una aplicación de consola, ha instalado TraceProcessor y lo ha usado para tener acceso a las líneas de comandos de proceso desde un seguimiento de ETW. Ahora tiene una aplicación que tiene acceso a los datos de seguimiento.

La información del proceso es solo uno de los muchos tipos de datos almacenados en un seguimiento de ETW al que puede tener acceso la aplicación.

El siguiente paso consiste en [examinar TraceProcessor](tutorial.md) y los otros orígenes de datos a los que puede tener acceso.