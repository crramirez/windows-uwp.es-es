---
title: Herramientas de diagnóstico de elementos gráficos
description: Obtén información sobre cómo obtener y usar las características de diagnóstico de elementos gráficos como, por ejemplo, el uso de la GPU, los análisis de fotogramas de gráficos y la depuración de elementos gráficos en Visual Studio.
ms.assetid: 629ea462-18ed-a333-07e9-cc87ea2dcd93
ms.date: 11/20/2019
ms.topic: article
keywords: windows 10, uwp, juegos, gráficos, diagnósticos, herramientas, directx
ms.localizationpriority: medium
ms.openlocfilehash: 14711a356f00ac027bf990554c90547017b9cc4d
ms.sourcegitcommit: f464e5157ca22cfe31075f2f1219b8227587bf03
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74263095"
---
# <a name="graphics-diagnostics-tools"></a>Herramientas de diagnóstico de elementos gráficos

Las herramientas de diagnóstico de gráficos están disponibles en Windows 10 como una característica opcional. Para usar las características de diagnóstico de gráficos (proporcionadas en tiempo de ejecución y Visual Studio) para desarrollar aplicaciones o juegos de DirectX, instale la característica opcional herramientas de gráficos.

1. Vaya a **configuración** > **aplicaciones** > **aplicaciones & características/características opcionales**.
2. Si **herramientas de gráficos** ya aparece en **características instaladas**, habrá terminado. De lo contrario, haga clic en **Agregar una característica**.
3. Busque y/o seleccione **herramientas de gráficos**y, a continuación, haga clic en **instalar**.

Entre las características de diagnóstico de elementos gráficos se incluye la capacidad de crear dispositivos de depuración Direct3D (a través de las capas del SDK de Direct3D) en DirectX en tiempo de ejecución, además de la depuración de gráficos, los análisis de fotogramas y el uso de la GPU.

-   La depuración de elementos gráficos te permite realizar un seguimiento de las llamadas de Direct3D que realiza la aplicación. A continuación, puedes volver a reproducir estas llamadas, inspeccionar parámetros, depurar y experimentar con los sombreadores, así como visualizar los activos de elementos gráficos para diagnosticar problemas de representación. Puede tomar registros en PC, simuladores o dispositivos de Windows y, a continuación, reproducirlos en otro hardware.
-   Análisis de fotogramas de gráficos en Visual Studio se ejecuta en un registro de depuración de gráficos y recopila el tiempo de línea de base para las llamadas de dibujo de Direct3D. A continuación, realiza un conjunto de experimentos modificando varias configuraciones de gráficos y genera una tabla de resultados de tiempo. Puedes usar estos elementos gráficos para comprender los problemas de rendimiento de los gráficos de la aplicación y puedes revisar los resultados de los experimentos para identificar oportunidades de mejora del rendimiento.
-   El uso de la GPU en Visual Studio permite supervisar el uso de la GPU en tiempo real. Recopila y analiza los datos de control de tiempo de las cargas de trabajo que controlan la CPU y la GPU, de modo que puede determinar dónde se encuentran los cuellos de botella.

## <a name="related-topics"></a>Temas relacionados

[Información general de Diagnóstico de gráficos en Visual Studio](/visualstudio/debugger/overview-of-visual-studio-graphics-diagnostics?view=vs-2015)