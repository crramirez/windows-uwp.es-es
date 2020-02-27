---
title: 'Cargar símbolos: .NET TraceProcessing'
description: En este tutorial, aprenderá cómo cargar símbolos al procesar seguimientos.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: a6954538159c6fffb3185aa8b3137af26e17b32f
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629096"
---
# <a name="use-symbols-in-net-traceprocessing"></a>Usar símbolos en .NET TraceProcessing

[TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor) admite la carga de símbolos y la obtención de pilas de varios orígenes de datos. La siguiente aplicación de consola examina los ejemplos de CPU y genera la duración estimada en la que se estaba ejecutando una función específica (en función del muestreo estadístico del seguimiento del uso de CPU).

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Cpu;
using Microsoft.Windows.EventTracing.Symbols;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 3)
        {
            Console.Error.WriteLine("Usage: GetCpuSampleDuration.exe <trace.etl> <imageName> <functionName>");
            return;
        }

        string tracePath = args[0];
        string imageName = args[1];
        string functionName = args[2];
        Dictionary<string, Duration> matchDurationByCommandLine = new Dictionary<string, Duration>();

        using (ITraceProcessor trace = TraceProcessor.Create(tracePath))
        {
            IPendingResult<ISymbolDataSource> pendingSymbolData = trace.UseSymbols();
            IPendingResult<ICpuSampleDataSource> pendingCpuSamplingData = trace.UseCpuSamplingData();

            trace.Process();

            ISymbolDataSource symbolData = pendingSymbolData.Result;
            ICpuSampleDataSource cpuSamplingData = pendingCpuSamplingData.Result;

            symbolData.LoadSymbolsForConsoleAsync(SymCachePath.Automatic, SymbolPath.Automatic).GetAwaiter().GetResult();

            Console.WriteLine();
            IThreadStackPattern pattern = AnalyzerThreadStackPattern.Parse($"{imageName}!{functionName}");

            foreach (ICpuSample sample in cpuSamplingData.Samples)
            {
                if (sample.Stack != null && sample.Stack.Matches(pattern))
                {
                    string commandLine = sample.Process.CommandLine;

                    if (!matchDurationByCommandLine.ContainsKey(commandLine))
                    {
                        matchDurationByCommandLine.Add(commandLine, Duration.Zero);
                    }

                    matchDurationByCommandLine[commandLine] += sample.Weight;
                }
            }

            foreach (string commandLine in matchDurationByCommandLine.Keys)
            {
                Console.WriteLine($"{commandLine}: {matchDurationByCommandLine[commandLine]}");
            }
        }
    }
}
```

La ejecución de este programa produce una salida similar a la siguiente:

```shell
C:\GetCpuSampleDuration\bin\Debug\> GetCpuSampleDuration.exe C:\boot.etl user32.dll LoadImageInternal
0.0% (0 of 1165; 0 loaded)
<snip>
100.0% (1165 of 1165; 791 loaded)
wininit.exe: 15.99 ms
C:\Windows\Explorer.EXE: 5 ms
winlogon.exe: 20.15 ms
"C:\Users\AdminUAC\AppData\Local\Microsoft\OneDrive\OneDrive.exe" /background: 2.09 ms
```

(Los detalles de salida variarán en función del seguimiento).

## <a name="symbols-format"></a>Formato de símbolos

Internamente, TraceProcessor usa el formato [SymCache](https://docs.microsoft.com/windows-hardware/test/wpt/loading-symbols#symcache-path) , que es una memoria caché de algunos de los datos almacenados en un archivo PDB. Al cargar los símbolos, TraceProcessor requiere que se especifique una ubicación que se va a usar para estos archivos SymCache (una ruta de acceso de SymCache) y que, opcionalmente, se puede especificar un SymbolPath para acceder a los PDB. Cuando se proporciona un SymbolPath, TraceProcessor creará archivos SymCache a partir de archivos PDB según sea necesario y el procesamiento posterior de los mismos datos puede usar los archivos SymCache directamente para mejorar el rendimiento.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a cargar símbolos cuando se procesan seguimientos.

El siguiente paso es aprender a [usar streaming](streaming.md) para tener acceso a los datos de seguimiento sin almacenar en búfer todo en memoria.
