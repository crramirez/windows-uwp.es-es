---
title: 'Extensibilidad: .NET TraceProcessing'
description: En este tutorial, aprenderá a extender .NET TraceProcessing.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: bf5f7a7c1bb007b7f1a19508fa0ee7bbaf298654
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629146"
---
# <a name="extend-traceprocessor"></a>Extender TraceProcessor

Muchos tipos de datos de seguimiento tienen compatibilidad integrada en [TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor), pero si tiene los otros proveedores que desea analizar (incluidos sus propios proveedores personalizados), esos datos también están disponibles desde el seguimiento en directo mientras se produce el procesamiento.

Por ejemplo, esta es una manera sencilla de obtener la lista de identificadores de proveedor en un seguimiento:

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

En el ejemplo siguiente se muestra un origen de datos personalizado simplificado:

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
