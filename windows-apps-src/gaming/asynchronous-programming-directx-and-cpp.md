---
author: mtoepke
title: Programación asincrónica (DirectX y C++)
description: En este tema se tratan diversos aspectos que deben tenerse en cuenta al usar la programación asincrónica y los subprocesos con DirectX.
ms.assetid: 17613cd3-1d9d-8d2f-1b8d-9f8d31faaa6b
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, programación asincrónica, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 9f395136ff7f331f58b9c8e0ac1ee9efea296c02
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5765241"
---
# <a name="asynchronous-programming-directx-and-c"></a>Programación asincrónica (DirectX y C++)



En este tema se cubren diversos puntos que deben tener en cuenta al usar la programación asincrónica y los subprocesos con DirectX.

## <a name="async-programming-and-directx"></a>Programación asincrónica y DirectX


Si estás aprendiendo acerca de DirectX, o incluso si tienes experiencia ya, considera colocar todas las canalizaciones de procesamiento de gráficos en un subproceso. En cualquier escena dada de un juego, existen recursos comunes como mapas de bits, sombreadores y otros activos que requieren acceso exclusivo. Estos mismos recursos requieren que sincronices cualquier acceso a estos recursos a través de los subprocesos paralelos. La representación es un proceso difícil para paralelizar entre múltiples subprocesos.

No obstante, si tu juego es lo suficientemente complejo, o si buscas obtener un rendimiento mejorado, puedes usar la programación asincrónica para paralelizar parte de los componentes que no son específicos de tu canalización de representación. El hardware moderno presenta CPU con tecnología de hyperthreading y de múltiples núcleos, y tu aplicación debe sacar provecho de ello. Puedes garantizar esto usando la programación asincrónica para algunos de los componentes de tu juego que no necesitan acceso directo al contexto de dispositivo de Direct3D:

-   E/S de archivo
-   física
-   AI
-   redes
-   audio
-   controles
-   Componentes de UI basados en XAML

Tu aplicación puede controlar estos componentes en múltiples subprocesos simultáneos. E/S de archivo, especialmente la carga de recursos, se beneficia enormemente de la carga asincrónica, ya que tu juego o aplicación puede estar en un estado interactivo mientras se carga o transmiten varios (o varios cientos) de megabytes de recursos. La forma más sencilla de crear y administrar estos subprocesos es usando la [biblioteca de modelos de procesamiento paralelo](https://msdn.microsoft.com/library/dd492418.aspx) y el patrón **task**, según se incluye en el espacio de nombres **concurrency** definido en PPLTasks.h. El uso de la [biblioteca de modelos de procesamiento paralelo](https://msdn.microsoft.com/library/dd492418.aspx) permite sacar provecho directo de las CPU con tecnología de hyperthreading y de múltiples núcleos, y puede mejorar elementos como, por ejemplo, las horas de carga percibidas, así como las complicaciones y los retrasos que se derivan del procesamiento de red y los cálculos de CPU intensivos.

> **Nota**  en unos Universal de Windows Platform (UWP), la interfaz de usuario se ejecuta totalmente en un contenedor uniproceso (STA). Si creas una interfaz de usuario para tu juego de DirectX usando la [interoperabilidad de XAML](directx-and-xaml-interop.md), solamente puedes obtener acceso a los controles mediante el uso del STA.

 

## <a name="multithreading-with-direct3d-devices"></a>Multithreading con dispositivos de Direct3D


El multithreading para contextos de dispositivos solamente está disponible en dispositivos gráficos que admiten un nivel de característica de Direct3D de 11\_0 o superior. No obstante, probablemente quieras maximizar el uso de la poderosa GPU en muchas plataformas, como plataformas de juego dedicadas. En el caso más simple, probablemente quieras separar la representación de una superposición de pantalla de visualización frontal (HUD) desde la proyección y representación de escenas 3D, y hacer que ambos componentes usen canalizaciones paralelas separadas. No obstante, ambos subprocesos deben usar el mismo objeto [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) para crear y administrar los objetos de recursos (texturas, mallas, sombreadores y otros recursos), el cual es de un solo subproceso y requiere la implementación de algún tipo de mecanismo de sincronización (como secciones críticas) para acceder a él con seguridad. Y, si bien puedes crear listas de comandos separadas para el contexto de dispositivo en subprocesos diferentes (para la representación diferida), no puedes reproducir esas listas de comandos nuevamente de manera simultánea en la misma instancia de **ID3D11DeviceContext**.

Ahora, tu aplicación también puede usar [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379), que es más seguro para el multithreading, para crear objetos de recursos. Por ello, ¿por qué no puede usarse siempre **ID3D11Device** en lugar de [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)? Actualmente, la compatibilidad de los controladores con multithreading probablemente no esté disponible para algunas interfaces gráficas. Puedes consultar el dispositivo y descubrir que admite multithreading, pero si buscas llegar a un público mayor, probablemente debas quedarte con **ID3D11DeviceContext** de un solo subproceso para la administración de objetos de recursos. Ahora bien, cuando el controlador de dispositivos gráficos no admite listas de comandos o multithreading, Direct3D 11 intenta controlar el acceso sincronizado al contexto de dispositivos internamente, y si no se admiten listas de comandos, proporciona una implementación de software. Como resultado, puedes escribir código multiproceso que se ejecutará en plataformas con interfaces gráficas que no admiten controladores para el acceso a contexto de dispositivos multiproceso.

Si tu aplicación admite subprocesos separados para procesar listas de comandos y para mostrar fotogramas, probablemente quieras mantener la GPU activa, procesando las listas de comandos mientras se muestran los fotogramas en el tiempo esperado sin demoras ni titubeos perceptibles. En este caso, puedes usar un objeto [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) separado para cada subproceso, y para compartir recursos, como texturas, creándolos con la marca D3D11\_RESOURCE\_MISC\_SHARED. En este escenario, debe llamarse a [**ID3D11DeviceContext::Flush**](https://msdn.microsoft.com/library/windows/desktop/ff476425) en el subproceso de procesamiento para completar la ejecución de la lista de comandos antes de mostrar los resultados del procesamiento del objeto de recursos en el subproceso de visualización.

## <a name="deferred-rendering"></a>Representación diferida


La representación diferida registra comandos de gráficos en una lista de comandos para que puedan volver a reproducirse en algún otro momento y está diseñada para admitir la representación en un subproceso mientras se registran comandos para la representación en subprocesos adicionales. Después de que se completen estos comandos, pueden ejecutarse en el subproceso que genera el objeto de visualización final (búfer de cuadros, textura u otra salida de gráficos).

Crea un contexto diferido usando [**ID3D11Device::CreateDeferredContext**](https://msdn.microsoft.com/library/windows/desktop/ff476505) (en lugar de [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) o [**D3D11CreateDeviceAndSwapChain**](https://msdn.microsoft.com/library/windows/desktop/ff476083), que crean un contexto inmediato). Para más información, consulta [Immediate and Deferred Rendering](https://msdn.microsoft.com/library/windows/desktop/ff476892) (Representación inmediata y diferida).

## <a name="related-topics"></a>Temas relacionados


* [Introducción a multithreading en Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476891)

 

 




