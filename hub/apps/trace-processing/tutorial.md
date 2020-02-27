---
title: 'Acceder a los datos de seguimiento: .NET TraceProcessing'
description: En este tutorial, aprenderá a acceder a los datos de seguimiento mediante TraceProcessor.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: 170a8c3084e180714a319d67dca2b6a5756ea474
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629116"
---
# <a name="access-trace-data"></a>Acceder a los datos de seguimiento

.NET TraceProcessing está disponible en [NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) con el siguiente identificador de paquete:

Microsoft. Windows. EventTracing. Processing. All

Este paquete le permite tener acceso a los datos de un archivo de seguimiento. Si aún no tiene un archivo de seguimiento, puede usar la [grabadora de rendimiento de Windows](https://docs.microsoft.com/windows-hardware/test/wpt/start-a-recording) para crear uno.

En la siguiente aplicación de consola de ejemplo se muestra cómo obtener acceso a las líneas de comandos de todos los procesos contenidos en el seguimiento:

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

## <a name="using-traceprocessor"></a>Usar TraceProcessor

Para procesar un seguimiento, llame a [TraceProcessor. Create](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor.create). La interfaz principal es [ITraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.itraceprocessor)y el uso de esta interfaz implica el siguiente patrón:

1. En primer lugar, indique al procesador qué datos desea usar de un seguimiento.
2. En segundo lugar, procese el seguimiento; etc
3. Por último, acceda a los resultados.

Indicando al procesador qué tipos de datos desea adelantar significa que no necesita dedicar tiempo a procesar grandes volúmenes de todos los tipos de datos de seguimiento posibles. En su lugar, [TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor) simplemente realiza el trabajo necesario para proporcionar los tipos de datos específicos que solicite.

## <a name="recommended-project-settings"></a>Configuración de proyecto recomendada

Hay un par de opciones de configuración del proyecto que se recomienda usar con TraceProcessor:

1. Se recomienda ejecutar exe como 64 bits.

    El valor predeterminado de Visual Studio para C# una nueva aplicación de consola de .NET Framework es cualquier CPU con preferencia de 32 bits activada. Es posible que el valor predeterminado de .NET Core ya tenga la configuración recomendada.

    El procesamiento de seguimiento puede consumir mucha memoria, especialmente con seguimientos más grandes, y se recomienda cambiar el destino de la plataforma a x64 (o deshabilitar la comprobación preferida de 32 bits) en exe que usan TraceProcessor. Para cambiar esta configuración, vea la pestaña compilar en propiedades del proyecto. Para cambiar esta configuración para todas las configuraciones, asegúrese de que la lista desplegable configuración está establecida en todas las configuraciones, en lugar del valor predeterminado de la configuración actual.

2. Se recomienda usar NuGet con el modo de PackageReference de estilo más nuevo en lugar del modo packages. config anterior.

    Para cambiar el valor predeterminado para los proyectos nuevos, vea herramientas, administrador de paquetes NuGet, configuración del administrador de paquetes, Administración de paquetes, formato de administración de paquetes predeterminado.

## <a name="built-in-data-sources"></a>Orígenes de datos integrados

Un archivo. ETL puede capturar muchos tipos de datos en un seguimiento. Tenga en cuenta que los datos que se encuentra en un archivo. ETL dependen de los proveedores que se habilitaron cuando se capturó el seguimiento. En la lista siguiente se muestran los tipos de datos de seguimiento disponibles en TraceProcessor:

| Código                                      | Descripción                                                                                                                | Elementos de WPA relacionados                                                    |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| seguimiento. UseClassicEvents()                  | Proporciona eventos ETW clásicos de un seguimiento, que no incluyen información del esquema.                                         | Tabla de eventos genéricos (cuando el tipo de evento es clásico o WPP)             |
| seguimiento. UseConnectedStandbyData()           | Proporciona datos de un seguimiento sobre el sistema que entra y sale del modo de espera conectado.                                        | Tabla de Resumen de CS                                                     |
| seguimiento. UseCpuIdleStates()                  | Proporciona datos de un seguimiento sobre los Estados C de la CPU.                                                                             | Tabla de Estados de inactividad de CPU (cuando el tipo es real)                          |
| seguimiento. UseCpuSamplingData()                | Proporciona datos de un seguimiento sobre el uso de CPU basado en el muestreo periódico del puntero de instrucción.                          | Tabla de uso de CPU (muestreado)                                            |
| seguimiento. UseCpuSchedulingData()              | Proporciona datos de un seguimiento sobre la programación de subprocesos de CPU, incluidos los modificadores de contexto y los eventos de subprocesos listos.                | Tabla de uso de CPU (precisa)                                            |
| seguimiento. UseDevicePowerData()                | Proporciona datos de un seguimiento sobre los Estados D-States.                                                                          | Tabla DState de dispositivos                                                  |
| seguimiento. UseDirectXData()                    | Proporciona datos de un seguimiento sobre la actividad de DirectX.                                                                         | Tabla de uso de GPU                                                |
| traceUseDiskIOData()                      | Proporciona datos de un seguimiento de la actividad de e/s de disco.                                                                        | Tabla de uso de disco                                                     |
| seguimiento. UseEnergyEstimationData()           | Proporciona datos de un seguimiento sobre el uso de energía estimado por proceso del motor de estimación de energía.                         | Tabla de resumen del motor de estimación de energía (por proceso)                  |
| seguimiento. UseEnergyMeterData()                | Proporciona datos de un seguimiento sobre el uso de energía medido de la interfaz de medidor de energía (EMI).                                  | Tabla del motor de estimación de energía (por EMI)                              |
| seguimiento. UseFileIOData()                     | Proporciona datos de un seguimiento sobre la actividad de e/s de archivos.                                                                        | Tabla de e/s de archivos                                                       |
| seguimiento. UseGenericEvents()                  | Proporciona eventos manifestd y TraceLogging a partir de un seguimiento.                                                                  | Tabla de eventos genéricos (cuando el tipo de evento es Manifestd o TraceLogging) |
| seguimiento. UseHandles()                        | Proporciona datos parciales de un seguimiento sobre los identificadores de kernel activos.                                                            | Tabla de identificadores                                                        |
| seguimiento. UseHardFaults()                     | Proporciona datos de un seguimiento sobre errores de página severos.                                                                         | Tabla de errores severos                                                    |
| seguimiento. UseHeapSnapshots()                  | Proporciona datos de un seguimiento sobre el uso del montón de proceso.                                                                       | Tabla de instantáneas de montón                                                  |
| seguimiento. UseHypercalls()                     | Proporciona datos sobre hiperllamadas de Hyper-V que se proprodujo durante un seguimiento.                                                        |                                                                      |
| seguimiento. UseImageSections()                  | Proporciona datos de un seguimiento sobre las secciones de una imagen.                                                                 | Columna de nombre de sección de la tabla de uso de CPU (muestreado)                 |
| seguimiento. UseInterruptHandlingData()          | Proporciona datos de un seguimiento de la actividad de la rutina de servicio de interrupción (ISR) y la llamada a procedimiento diferido (DPC).               | Tabla de DPC/ISR                                                        |
| seguimiento. UseMarks()                          | Proporciona las marcas (marcas de tiempo con etiqueta) de un seguimiento.                                                                      | Tabla de marcas                                                          |
| seguimiento. UseMemoryUtilizationData()          | Proporciona datos de un seguimiento sobre el uso total de la memoria del sistema.                                                          | Tabla de uso de memoria                                             |
| seguimiento. UseMetadata()                       | Proporciona metadatos de seguimiento disponibles sin más procesamiento.                                                              | Configuración del sistema, seguimientos y general                             |
| seguimiento. UsePlatformIdleStates()             | Proporciona datos de un seguimiento sobre los Estados inactivos de la plataforma real y de destino de un sistema.                                   | Tabla de estado inactivo de plataforma                                            |
| seguimiento. UsePoolAllocations()                | Proporciona datos de un seguimiento sobre el uso de memoria del grupo de kernel.                                                                 | Tabla de resumen del grupo                                                   |
| seguimiento. UsePowerConfigurationData()         | Proporciona datos de un seguimiento acerca de la configuración de energía del sistema.                                                               | Configuración del sistema, configuración de energía                                 |
| seguimiento. UsePowerDependencyCoordinatorData() | Proporciona datos de un seguimiento sobre las fases activas del Coordinador de dependencias de energía.                                               | Tabla de Resumen de fase de notificación                                     |
| seguimiento. UseProcesses()                      | Proporciona datos sobre los procesos activos durante un seguimiento, así como sus imágenes y PDB.                                      | Tabla de procesos; Tabla images; Concentrador de símbolos                           |
| seguimiento. UseProcessorCounters()              | Proporciona datos de un seguimiento de los valores del contador de rendimiento del procesador desde el monitor de contador de procesador (PCM).                |                                                                      |
| seguimiento. UseProcessorFrequencyData()         | Proporciona datos de un seguimiento sobre la frecuencia con la que se ejecutan los procesadores.                                                    | Tabla de frecuencia del procesador (cuando el tipo es real)                      |
| seguimiento. UseProcessorProfileData()           | Proporciona datos de un seguimiento sobre el perfil de energía del procesador activo.                                                       | Tabla de perfiles de procesador                                             |
| seguimiento. UseProcessorParkingData()           | Proporciona datos de un seguimiento sobre qué procesadores se han detenido o desactivado.                                                 | Tabla de estado de estacionamiento del procesador                                        |
| seguimiento. UseProcessorParkingLimits()         | Proporciona datos de un seguimiento sobre el número máximo permitido de procesadores desactivados.                                        | Tabla de estado de Cap de estacionamiento de núcleo                                         |
| seguimiento. UseProcessorQualityOfServiceData()  | Proporciona datos de un seguimiento sobre la calidad del nivel de servicio para cada procesador.                                          | Tabla de clases QoS de procesador                                            |
| seguimiento. UseProcessorThrottlingData()        | Proporciona datos de un seguimiento sobre la limitación de frecuencia máxima del procesador.                                                   | Tabla de restricciones del procesador                                          |
| seguimiento. UseReadyBootData()                  | Proporciona datos de un seguimiento de la actividad de captura previa de arranque desde el arranque preparado.                                                | Tabla de eventos de arranque listos                                              |
| seguimiento. UseReferenceSetData()               | Proporciona datos de un seguimiento sobre las páginas de memoria virtual utilizadas por cada proceso.                                             | Tabla del conjunto de referencia                                                  |
| seguimiento. UseRegionsOfInterest()              | Proporciona regiones con nombre de intervalos de interés a partir de un seguimiento, tal y como se especifica en un archivo de configuración XML.                       | Tabla de regiones de interés                                            |
| seguimiento. UseRegistryData()                   | Proporciona datos sobre la actividad del registro durante un seguimiento.                                                                      | Tabla del registro                                                       |
| seguimiento. UseResidentSetData()                | Proporciona datos de un seguimiento sobre las páginas de memoria virtual para cada proceso residente en la memoria física.       | Tabla de conjuntos residentes                                                   |
| seguimiento. UseRundownData()                    | Proporciona datos de un seguimiento sobre los intervalos en los que se ha producido la recolección de datos de detención de seguimiento.                            | Regiones sombreadas en la escala de tiempo del gráfico                                 |
| seguimiento. UseScheduledTasks()                 | Proporciona datos acerca de las tareas programadas que se ejecutaron durante un seguimiento.                                                               | Tabla de tareas programadas                                                |
| seguimiento. UseServices()                       | Proporciona datos sobre los servicios que estaban activos o que se ha capturado su estado durante un seguimiento.                                  | Tabla servicios; Configuración del sistema, servicios                       |
| seguimiento. UseStacks()                         | Proporciona datos sobre las pilas registradas durante un seguimiento.                                                                        |                                                                      |
| seguimiento. UseStackEvents()                    | Proporciona datos sobre los eventos asociados a las pilas registradas durante un seguimiento.                                                 | Tabla de pilas                                                         |
| seguimiento. UseStackTags()                      | Proporciona un asignador que agrupa pilas de un seguimiento en etiquetas de la pila, tal y como se especifica en un archivo de configuración XML.               | Columnas como etiqueta de pila y pila (etiquetas de marco)                     |
| seguimiento. UseSymbols()                        | Proporciona la capacidad de cargar símbolos para un seguimiento.                                                                          | Configurar rutas de acceso de símbolos; Cargar símbolos                                 |
| seguimiento. UseSyscalls()                       | Proporciona datos sobre llamadas syscall que se produjeron durante un seguimiento.                                                                 | Tabla llamadas syscall                                                       |
| seguimiento. UseSystemMetadata()                 | Proporciona metadatos generales en todo el sistema a partir de un seguimiento.                                                                       | Pruebas de configuración                                                 |
| seguimiento. UseSystemPowerSourceData()          | Proporciona datos de un seguimiento sobre la fuente de alimentación del sistema activa (AC frente a DC).                                                | Tabla de origen de alimentación del sistema                                            |
| seguimiento. UseSystemSleepData()                | Proporciona datos de un seguimiento sobre el estado de energía general del sistema.                                                               | Tabla de transición de energía                                               |
| seguimiento. UseTargetCpuIdleStates()            | Proporciona datos de un seguimiento sobre la CPU de destino C-States.                                                                      | Tabla de Estados de inactividad de CPU (cuando el tipo es destino)                          |
| seguimiento. UseTargetProcessorFrequencyData()   | Proporciona datos de un seguimiento sobre las frecuencias del procesador de destino.                                                             | Tabla de frecuencia del procesador (cuando el tipo es destino)                      |
| seguimiento. UseThreads()                        | Proporciona datos sobre los subprocesos activos durante un seguimiento.                                                                         | Tabla de duraciones de subprocesos                                               |
| seguimiento. UseTraceStatistics()                | Proporciona estadísticas sobre los eventos de un seguimiento.                                                                           | Configuración del sistema, estadísticas de seguimiento                               |
| seguimiento. UseUtcData()                        | Proporciona datos de un seguimiento sobre la actividad de telemetría de Microsoft mediante el cliente de telemetría universal (UTC).                      | Tabla UTC                                                            |
| seguimiento. UseWindowInFocus()                  | Proporciona datos de un seguimiento sobre los cambios en la ventana de la interfaz de usuario activa en el foco.                                                 | Ventana en la tabla de enfoque                                                |
| seguimiento. UseWindowsTracePreprocessorEvents() | Proporciona eventos de preprocesador de seguimiento de software (WPP) de Windows desde un seguimiento.                                                    | Tabla de seguimiento WPP; Tabla de eventos genéricos (cuando el tipo de evento es WPP)       |
| seguimiento. UseWinINetData()                    | Proporciona datos de un seguimiento de la actividad de Internet a través de Windows Internet (WinINet).                                         | Descargar tabla de detalles                                               |
| seguimiento. UseWorkingSetData()                 | Proporciona datos de un seguimiento sobre las páginas de memoria virtual que estaban en el espacio de trabajo para cada proceso o categoría de kernel. | Tabla de instantáneas de memoria virtual                                       |

Vea también los métodos de extensión de [ITraceSource](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.itracesource) para todos los datos de seguimiento disponibles o examine el método disponible en "trace". se muestra con IntelliSense.

## <a name="next-steps"></a>Pasos siguientes

En esta información general, aprendió a acceder a los datos de seguimiento mediante TraceProcessor y a los orígenes de datos integrados a los que puede tener acceso.

El siguiente paso es aprender a [ampliar](extensibility.md) TraceProcessor para tener acceso a los datos de seguimiento personalizados.
