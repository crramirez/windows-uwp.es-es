---
title: 'Qué es .NET TraceProcessing: .NET TraceProcessing'
description: En esta introducción, aprenderá qué es .NET TraceProcessing.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: 03f88d6bd1ac150d94f73f9f9177249c3b5ae78f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170559"
---
# <a name="process-etw-traces-in-net"></a>Procesar seguimientos de ETW en .NET

[Seguimiento de eventos para Windows (ETW)](/windows/win32/etw/event-tracing-portal) es un eficaz sistema de recopilación de seguimientos integrado en el sistema operativo Windows. Windows tiene una integración profunda con ETW, incluidos los datos sobre el comportamiento del sistema hasta el kernel para eventos como cambios de contexto, asignación de memoria, creación y salida de procesos, etc. Los datos de todo el sistema disponibles en ETW lo convierten en una buena opción para el análisis de rendimiento de un extremo a otro o para otras preguntas que requieran la interacción entre varios componentes del sistema.

A diferencia del registro de texto, ETW proporciona eventos estructurados diseñados para el procesamiento automático de datos. Microsoft ha creado herramientas eficaces basadas en estos eventos estructurados, incluido el [analizador de rendimiento de Windows (WPA)](/windows-hardware/test/wpt/windows-performance-analyzer), que proporciona una interfaz gráfica para visualizar y explorar los datos de seguimiento capturados en un archivo de seguimiento de ETW (. ETL).

Dentro de Microsoft, usamos grandes seguimientos de ETW para medir el rendimiento de las nuevas compilaciones de Windows. Dado el volumen de datos generado por el sistema de ingeniería de Windows, es esencial el análisis automatizado. En nuestro análisis automatizado de seguimiento, usamos principalmente C# y .NET, por lo que hemos creado la [API TraceProcessing de .net](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) para tener acceso a muchos tipos de datos de seguimiento de ETW. Esta tecnología también se usa en el analizador de rendimiento de Windows para potenciar varias de sus tablas.

Los paquetes NuGet de .NET TraceProcessing permiten analizar sus propias aplicaciones y sistemas con las mismas herramientas que Microsoft usa para analizar Windows.

## <a name="next-steps"></a>Pasos siguientes

En esta información general, aprendió qué es .NET TraceProcessing.

El siguiente paso consiste en [procesar el primer seguimiento](quickstart.md).

## <a name="related-topics"></a>Temas relacionados

* [Acceder a los datos de seguimiento](tutorial.md)
* [Extender TraceProcessor](extensibility.md)
* [Cargar símbolos](symbols.md)
* [Usar streaming](streaming.md)
* [Muestras](https://github.com/microsoft/eventtracing-processing-samples)
* [Referencia de API](reference.md)