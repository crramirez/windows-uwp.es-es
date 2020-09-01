---
title: 'Extensibilidad: .NET TraceProcessing'
description: En este tutorial, aprenderá a extender .NET TraceProcessing.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: f5680bdc6502c4b917667e5a59084286b445063c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166739"
---
# <a name="extend-traceprocessor"></a>Extender TraceProcessor

Muchos tipos de datos de seguimiento tienen compatibilidad integrada en [TraceProcessor](/dotnet/api/microsoft.windows.eventtracing.traceprocessor), pero si tiene los otros proveedores que desea analizar (incluidos sus propios proveedores personalizados), esos datos también están disponibles desde el seguimiento en directo mientras se produce el procesamiento.

> [!NOTE]
> Esta parte de la API está en versión preliminar y en desarrollo activo. Puede cambiar en futuras versiones.

Por ejemplo, esta es una manera sencilla de obtener la lista de identificadores de proveedor en un seguimiento.

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    HashSet<Guid> providerIds = new HashSet<Guid>();
    trace.Use((e) => providerIds.Add(e.ProviderId));
    trace.Process();

    foreach (Guid providerId in providerIds)
    {
        Console.WriteLine(providerId);
    }
}
```

En el ejemplo siguiente se muestra un origen de datos personalizado simplificado.

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    CustomDataSource customDataSource = new CustomDataSource();
    trace.Use(customDataSource);

    trace.Process();

    Console.WriteLine(customDataSource.Count);
}

class CustomDataSource : IFilteredEventConsumer
{
    public IReadOnlyList<Guid> ProviderIds { get; } = new Guid[] { new Guid("your provider ID") };

    public int Count { get; private set; }

    public void Process(EventContext eventContext)
    {
        ++Count;
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a extender TraceProcessor.

El siguiente paso es aprender a [cargar símbolos](symbols.md) para seguimientos.