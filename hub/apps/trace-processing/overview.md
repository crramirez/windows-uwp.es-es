---
title: 'Qué es .NET TraceProcessing: .NET TraceProcessing'
description: En esta introducción, aprenderá qué es .NET TraceProcessing.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: 1283a6b183673cbfb0d3d27290fc24ae6b020302
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629079"
---
# <a name="process-event-tracing-for-windows-etw-traces-in-net"></a>Procesos de seguimiento de eventos para Windows (ETW) en .NET

[Seguimiento de eventos para Windows (ETW)](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal) es un eficaz sistema de recopilación de seguimientos integrado en el sistema operativo Windows. Windows tiene una integración profunda con ETW, incluidos los datos sobre el comportamiento del sistema hasta el kernel para eventos como cambios de contexto, asignación de memoria, creación y salida de procesos, etc. Los datos de todo el sistema disponibles en ETW lo convierten en una buena opción para el análisis de rendimiento de un extremo a otro o para otras preguntas que requieran la interacción entre varios componentes del sistema.

A diferencia del registro de texto, ETW proporciona eventos estructurados diseñados para el procesamiento automático de datos. Microsoft ha creado herramientas eficaces basadas en estos eventos estructurados, incluido el [analizador de rendimiento de Windows (WPA)](https://docs.microsoft.com/windows-hardware/test/wpt/windows-performance-analyzer), que proporciona una interfaz gráfica para visualizar y explorar los datos de seguimiento capturados en un archivo de seguimiento de ETW (. ETL).

Dentro de Microsoft, usamos grandes seguimientos de ETW para medir el rendimiento de las nuevas compilaciones de Windows. Dado el volumen de datos generado por el sistema de ingeniería de Windows, es esencial el análisis automatizado. En nuestro análisis de seguimiento automatizado, usamos C# mucho y .net, por lo que hemos creado la [API TraceProcessing de .net](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) para tener acceso a muchos tipos de datos de seguimiento de ETW. Esta tecnología también se usa en el analizador de rendimiento de Windows para potenciar varias de sus tablas.

Los paquetes NuGet de .NET TraceProcessing permiten analizar sus propias aplicaciones y sistemas con las mismas herramientas que Microsoft usa para analizar Windows.

## <a name="next-steps"></a>Pasos siguientes

En esta información general, aprendió qué es .NET TraceProcessing.

El siguiente paso consiste en [procesar el primer seguimiento](quickstart.md).

## <a name="in-this-section"></a>En esta sección

* [Acceder a los datos de seguimiento](tutorial.md)
* [Extender TraceProcessor](extensibility.md)
* [Cargar símbolos](symbols.md)
* [Usar streaming](streaming.md)
* [Ejemplos](https://github.com/microsoft/eventtracing-processing-samples)
* [Referencia de las API](reference.md)
