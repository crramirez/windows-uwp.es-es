---
ms.assetid: beac6333-655a-4bcf-9caf-bba15f715ea5
title: Subprocesamiento y programación asincrónica
description: El subprocesamiento y la programación asincrónica permiten a tu aplicación realizar el trabajo de forma asincrónica en subprocesos paralelos.
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp, asincrónico, asincrónica, subprocesos, subprocesamiento
ms.localizationpriority: medium
ms.openlocfilehash: 22c151b90be30b39da7decd9a0ce3109e29b7fb7
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8343561"
---
# <a name="threading-and-async-programming"></a>Subprocesamiento y programación asincrónica
El subprocesamiento y la programación asincrónica permiten a tu aplicación realizar el trabajo de forma asincrónica en subprocesos paralelos.

Una aplicación puede usar el grupo de subprocesos para realizar trabajos de manera asincrónica en subprocesos paralelos. El grupo de subprocesos administra un conjunto de subprocesos y usa una cola para asignar elementos de trabajo a subprocesos a medida que están disponibles. El grupo de subprocesos es similar a los modelos de programación asincrónica disponibles en Windows Runtime porque puede usarse para realizar trabajos largos sin bloquear la interfaz de usuario, pero el grupo de subprocesos ofrece más control que los modelos de programación asincrónica y puedes usarlo para completar varios elementos de trabajo en paralelo. Puedes usar el grupo de subprocesos para hacer lo siguiente:

-   Enviar elementos de trabajo, controlar su prioridad y cancelarlos.

-   Programar elementos de trabajo con temporizadores y temporizadores periódicos.

-   Reservar recursos para elementos de trabajo críticos.

-   Ejecutar elementos de trabajo en respuesta a eventos con nombre y semáforos.

El grupo de subprocesos es más eficaz en la administración de subprocesos porque reduce la sobrecarga que conlleva crear y destruir subprocesos. Esto significa que tiene acceso para optimizar los subprocesos en varios núcleos de CPU y puede equilibrar los recursos de subprocesos entre las aplicaciones y cuando las tareas en segundo plano están en ejecución. El uso del grupo de subprocesos integrado resulta práctico porque puedes centrarte en la escritura de código que realiza una tarea en lugar de en la mecánica de la administración de subprocesos.

| Tema                                                                                                          | Descripción                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------|
| [Programación asincrónica (aplicaciones para UWP)](asynchronous-programming-universal-windows-platform-apps.md)              | Este tema describe la programación asincrónica en la plataforma Universal de Windows (UWP) y su representación en C#, Microsoft Visual Basic.NET, las extensiones de componentes de VisualC ++ (C++ / CX) y JavaScript. |
| [Programación asincrónica en C++/CX (aplicaciones para UWP)](asynchronous-programming-in-cpp-universal-windows-platform-apps.md)| En este artículo se describe la manera recomendada de consumir métodos asincrónicos en las extensiones de componentes de Visual C++ (C++/CX) usando la clase <code>task</code><code>concurrency</code> que se define en el espacio de nombres  en ppltasks.h. |
| [Procedimientos recomendados para usar el grupo de subprocesos](best-practices-for-using-the-thread-pool.md)                         | En este tema se describen los procedimientos recomendados para trabajar con el grupo de subprocesos. |
| [Llamar a API asincrónicas en C# o Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md)             | La Plataforma universal de Windows (UWP) incluye muchas API asincrónicas para que tu aplicación tenga capacidad de respuesta mientras realiza trabajos que pudieran llevar algún tiempo. En este tema se describe cómo usar métodos asincrónicos desde la UWP en C# o Microsoft Visual Basic. |
| [Crear un elemento de trabajo periódico](create-a-periodic-work-item.md)                                                   | Obtén información sobre cómo crear un elemento de trabajo que se repita periódicamente. |
| [Enviar un elemento de trabajo al grupo de subprocesos](submit-a-work-item-to-the-thread-pool.md)                               | Obtén información acerca de cómo realizar trabajo en un subproceso separado mediante el envío de un elemento de trabajo al grupo de subprocesos. |
| [Enviar un elemento de trabajo con un temporizador](use-a-timer-to-submit-a-work-item.md)                                       | Obtén información acerca de cómo crear un elemento de trabajo que se ejecute después de que transcurra un temporizador. |
