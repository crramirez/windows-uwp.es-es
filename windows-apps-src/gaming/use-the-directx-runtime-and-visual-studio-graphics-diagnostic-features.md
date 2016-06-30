---
author: mtoepke
title: "Herramientas de diagnóstico de gráficos"
description: "Obtén información sobre cómo obtener y usar las características de diagnóstico de elementos gráficos como, por ejemplo, el uso de la GPU, los análisis de fotogramas de gráficos y la depuración de elementos gráficos en Visual Studio."
ms.assetid: 629ea462-18ed-a333-07e9-cc87ea2dcd93
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: f266cb50893fd37162f21be169d6daf6e37c6bb9

---

# Herramientas de diagnóstico de gráficos


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Con Windows 10, las herramientas de diagnóstico de elementos gráficos ya están disponibles en Windows como una característica opcional. Para usar las características de diagnóstico de elementos gráficos proporcionadas en tiempo de ejecución y Visual Studio para desarrollar juegos o aplicaciones de DirectX, instala la característica opcional de herramientas de elementos gráficos:

1.  Ve a **Configuración**, selecciona **Sistema**, selecciona **Características opcionales** y, a continuación, haz clic en **Agregar una característica**. Ve a **Configuración**, selecciona **Sistema**, selecciona **Apps & features**, selecciona **Administrar características opcionales** y haz clic en **Agregar una característica**.
2.  En la lista **Agregar una característica**, haz clic en **Herramientas de elementos gráficos**.

Entre las características del diagnóstico de elementos gráficos se incluye la capacidad de crear dispositivos de depuración Direct3D (a través de las capas de SDK de Direct3D) en DirectX en tiempo de ejecución, además de la depuración de gráficos, los análisis de fotogramas y el uso de la GPU.

-   La depuración de elementos gráficos te permite realizar un seguimiento de las llamadas de Direct3D que realiza la aplicación. A continuación, puedes volver a reproducir estas llamadas, inspeccionar parámetros, depurar y experimentar con los sombreadores, así como visualizar los activos de elementos gráficos para diagnosticar problemas de representación. Los registros pueden tomarse en PC con Windows, simuladores o dispositivos, y pueden reproducirse en diferentes tipos de hardware.
-   Análisis de fotogramas de gráficos de Visual Studio se ejecuta en un registro de depuración de elementos gráficos y recopila los intervalos de línea base de las llamadas de dibujos de Direct3D. A continuación, realiza una serie de experimentos al modificar distintas opciones de configuración de elementos gráficos y producir una tabla de resultados de intervalos. Puedes usar estos elementos gráficos para comprender los problemas de rendimiento de los gráficos de la aplicación y puedes revisar los resultados de los experimentos para identificar oportunidades de mejora del rendimiento.
-   El uso de la GPU en Visual Studio permite supervisar el uso de la GPU en tiempo real. Recopila y analiza los datos de intervalos de las cargas de trabajo que controlan la CPU y la GPU, por lo que puedes determinar dónde están los cuellos de botella.

## Temas relacionados


[Información general de diagnósticos de elementos gráficos en Visual Studio](http://go.microsoft.com/fwlink/p/?LinkID=526382)

 

 







<!--HONumber=Jun16_HO4-->


