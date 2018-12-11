---
title: El objeto de aplicación y DirectX
description: Los juegos DirectX para Plataforma universal de Windows (UWP) no usan muchos elementos y objetos de la interfaz de usuario de Windows.
ms.assetid: 46f92156-29f8-d65e-2587-7ba1de5b48a6
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, DirectX, objeto de aplicación
ms.localizationpriority: medium
ms.openlocfilehash: e12ad6ce221440e8840006b3883980721b899ae6
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8886357"
---
# <a name="the-app-object-and-directx"></a>El objeto de aplicación y DirectX



Los juegos DirectX para Plataforma universal de Windows (UWP) no usan muchos elementos y objetos de la interfaz de usuario de Windows. Como se ejecutan a un nivel más bajo en la pila de Windows Runtime, deben interoperar con el marco de la interfaz de usuario de una manera más básica, mediante el acceso y la interoperación con el objeto de aplicación directamente. Aprende cuándo y cómo se realiza esta interoperación y de qué manera puedes usar eficazmente, como desarrollador de DirectX, este modelo en el desarrollo de una aplicación para UWP.

Consulte el [Glosario de gráficos de Direct3D](../graphics-concepts/index.md) para obtener información acerca de los términos de gráficos desconocidos o los conceptos que se producen durante la lectura.

## <a name="the-important-core-user-interface-namespaces"></a>Los espacios de nombres de interfaz de usuario principales


En primer lugar, veamos los espacios de nombres de Windows Runtime que debes incluir (con **using**) en la aplicación para UWP. Pronto los describiremos en detalle.

-   [**Windows.ApplicationModel.Core**](https://msdn.microsoft.com/library/windows/apps/br205865)
-   [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
-   [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.System**](https://msdn.microsoft.com/library/windows/apps/br241814)
-   [**Windows.Foundation**](https://msdn.microsoft.com/library/windows/apps/br226021)

> **Nota**  si no estás desarrollando una aplicación para UWP, usa los componentes de interfaz de usuario incluidos en las bibliotecas de JavaScript o XAML específicas y espacios de nombres en lugar de los tipos proporcionados en estos espacios de nombres.

 

## <a name="the-windows-runtime-app-object"></a>El objeto de aplicación de Windows Runtime


Es posible que desees que tu aplicación para UWP obtenga una ventana y un proveedor de vistas de los que se pueda obtener una vista y a los que se pueda conectar la cadena de intercambio (los búferes de presentación). También puedes enlazar esta vista a los eventos específicos de ventana para la aplicación en ejecución. Para obtener la ventana primaria del objeto de aplicación, definida por el tipo [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225), crea un tipo que implemente [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482), de la misma manera que en el fragmento de código anterior.

Este es el conjunto básico de pasos necesarios para obtener una ventana con el marco de trabajo de la interfaz de usuario básico:

1.  Crea un tipo que implemente [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478). Esta será la vista.

    En este tipo, define:

    -   Un método [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) que use una instancia de [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) como parámetro. Puedes obtener una instancia de este tipo si llamas a [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278). El objeto de aplicación lo llama cuando se inicia la aplicación.
    -   Un método [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509) que use una instancia de [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) como parámetro. Puedes obtener una instancia de este tipo si accedes a la propiedad [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) en la nueva instancia de [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017).
    -   Un método [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501) que use una cadena para un punto de entrada como parámetro único. El objeto aplicación proporciona la cadena de punto de entrada al llamar a este método. Ahora es cuando hay que configurar recursos. Aquí es donde crearás los recursos de dispositivo. El objeto de aplicación lo llama cuando se inicia la aplicación.
    -   Un método [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) que active el objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) e inicie el distribuidor de eventos de ventana. El objeto aplicación lo llama cuando se inicia el proceso de la aplicación.
    -   Un método [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523) que limpie los recursos configurado en la llamada a [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501). El objeto de aplicación llama a este método cuando se cierra la aplicación.

2.  Crea un tipo que implemente [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482). Este será el proveedor de vistas.

    En este tipo, define:

    -   Un método denominado [**CreateView**](https://msdn.microsoft.com/library/windows/apps/hh700491) que devuelva una instancia de la implementación de [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478), tal como la creaste en el paso1.

3.  Pasa una instancia del proveedor de vistas a [**CoreApplication.Run**](https://msdn.microsoft.com/library/windows/apps/hh700469) desde **main**.

Con estos conceptos básicos en mente, veamos otras opciones para ampliar este enfoque.

## <a name="core-user-interface-types"></a>Tipos de interfaz de usuario principales


Estos son otros tipos de interfaz de usuario principales de Windows Runtime que podrían resultar útiles:

-   [**Windows.ApplicationModel.Core.CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)
-   [**Windows.UI.Core.CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)
-   [**Windows.UI.Core.CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)

Puedes usar estos tipos para acceder a la vista de la aplicación, en concreto, a las partes que extraen el contenido de la ventana primaria de la aplicación y controlan los eventos desencadenados de esa ventana. El proceso de la ventana de la aplicación es un *contenedor uniproceso de aplicación* (ASTA) que está aislado y controla todas las devoluciones de llamadas.

La vista de la aplicación la genera el proveedor de vistas para la ventana de aplicación y, en la mayoría de los casos, la implementará un paquete de marco específico o el mismo sistema, por lo que no tendrás que implementarla. Para DirectX debes implementar un proveedor de vistas ligero, como se indicó previamente. Hay una relación uno a uno específica entre los siguientes componentes y comportamientos:

-   Una vista de la aplicación, que se representa mediante el tipo [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017), y que define los métodos de actualización de la ventana.
-   Un ASTA, cuya atribución define el comportamiento de subprocesos para la aplicación. No puedes crear instancias de tipos con atributos STA de COM en un ASTA.
-   Un proveedor de vistas, que la aplicación obtiene del sistema o que puedes implementar.
-   Una ventana primaria, que se representa mediante el tipo [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225).
-   Origen para todos los eventos de activación. Las vistas y las ventanas tienen eventos de activación independientes.

En resumen, el objeto aplicación proporciona una fábrica de proveedores de vistas. Crea un proveedor de vistas y una instancia de ventana primaria para la aplicación. El proveedor de vistas define la vista de la aplicación para la ventana primaria de la aplicación. Veamos ahora los detalles de la vista y la ventana primaria.

## <a name="coreapplicationview-behaviors-and-properties"></a>Comportamientos y propiedades de CoreApplicationView


[**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) representa la vista actual de la aplicación. El singleton de aplicación crea la vista de la aplicación durante la inicialización, pero la vista permanece inactiva hasta que se activa. Puedes obtener la clase [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) que muestra la vista al accediendo a la propiedad [**CoreApplicationView.CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) de esta y puedes controlar eventos de activación y desactivación para la vista registrando delegados con el evento [**CoreApplicationView.Activated**](https://msdn.microsoft.com/library/windows/apps/br225018).

## <a name="corewindow-behaviors-and-properties"></a>Comportamientos y propiedades de CoreWindow


La ventana primaria, que es una instancia de [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225), se crea y se pasa al proveedor de vistas cuando se inicializa el objeto de aplicación. Si la aplicación tiene una ventana para mostrar, la muestra; de lo contrario, simplemente inicializa la vista.

[**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) proporciona diversos eventos específicos de entrada y comportamientos básicos de ventana. Puedes controlar estos eventos registrando tus propios delegados con ellos.

También puedes obtener el distribuidor de eventos de ventana para la ventana a través de la propiedad [**CoreWindow.Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208264) que proporciona una instancia de [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211).

## <a name="coredispatcher-behaviors-and-properties"></a>Comportamientos y propiedades de CoreDispatcher


Puedes determinar el comportamiento de los subprocesos de la distribución de eventos para una ventana con el tipo [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211). En este tipo, hay un método especialmente importante: el método [**CoreDispatcher.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215), que inicia el procesamiento de eventos de ventana. Llamar a este método con la opción incorrecta para la aplicación puede provocar comportamientos de procesamiento de eventos inesperados de todo tipo.

| Opción CoreProcessEventsOption                                                           | Descripción                                                                                                                                                                                                                                  |
|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [**CoreProcessEventsOption.ProcessOneAndAllPending**](https://msdn.microsoft.com/library/windows/apps/br208217) | Distribuye todos los eventos disponibles actualmente en la cola. Si no hay eventos pendientes, espera al siguiente evento nuevo.                                                                                                                                 |
| [**CoreProcessEventsOption.ProcessOneIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217)     | Distribuye un evento si está pendiente en la cola. Si no hay eventos pendientes, no espera a que se genere un evento nuevo, sino que se devuelve inmediatamente.                                                                                          |
| [**CoreProcessEventsOption.ProcessUntilQuit**](https://msdn.microsoft.com/library/windows/apps/br208217)        | Espera eventos nuevos y distribuye todos los eventos disponibles. Continúa con este comportamiento hasta que la ventana se cierra o la aplicación llama al método [**Close**](https://msdn.microsoft.com/library/windows/apps/br208260) en la instancia de [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). |
| [**CoreProcessEventsOption.ProcessAllIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217)     | Distribuye todos los eventos disponibles actualmente en la cola. Si no hay eventos pendientes, se devuelve inmediatamente.                                                                                                                                          |

 

UWP con DirectX debe usar la opción [**CoreProcessEventsOption.ProcessAllIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217) para evitar comportamientos de bloqueo que puedan interrumpir las actualizaciones de elementos gráficos.

## <a name="asta-considerations-for-directx-devs"></a>Consideraciones de ASTA para desarrolladores con DirectX


El objeto de aplicación que define la representación en tiempo de ejecución de tu aplicación para UWP y DirectX usa un modelo de subprocesos denominado contenedor uniproceso de aplicación (ASTA) para hospedar las vistas de la interfaz de usuario de tu aplicación. Si estás desarrollando una aplicación para UWP y DirectX, estarás familiarizado con las propiedades de ASTA, porque los subprocesos que envíes desde tu aplicación para UWP y DirectX deben usar las API de [**Windows::System::Threading**](https://msdn.microsoft.com/library/windows/apps/br229642) o deben usar [**CoreWindow::CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211). (Puedes obtener el objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) del ASTA llamando a [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589) desde la aplicación).

Como desarrollador de una aplicación DirectX para UWP, lo más importante que tienes que tener en cuenta es que debes habilitar el subproceso de la aplicación para enviar subprocesos de contenedor multiproceso (MTA) estableciendo **Platform::MTAThread** en **main()**.

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto myDXAppSource = ref new MyDXAppSource(); // your view provider factory 
    CoreApplication::Run(myDXAppSource);
    return 0;
}
```

Cuando se activa el objeto de aplicación de la aplicación DirectX para UWP, se crea el ASTA que se usará para la vista de la interfaz de usuario. El nuevo subproceso de ASTA llama a tu fábrica de proveedores de vistas para crear el proveedor de vistas de tu objeto de aplicación y, como resultado, el código de tu proveedor de vistas se ejecutará en ese subproceso de ASTA.

Además, cualquier subproceso que elabores a partir del ASTA debe estar en un MTA. Ten en cuenta que cualquier subproceso de MTA que elabores todavía puede crear problemas de reentrada y provocar un interbloqueo.

Si vas a portar el código existente para que se ejecute en el subproceso de ASTA, ten en cuenta estas consideraciones:

-   Los primitivos de espera, como [**CoWaitForMultipleObjects**](https://msdn.microsoft.com/library/windows/desktop/hh404144), se comportan de forma diferente en un ASTA que en un contenedor uniproceso (STA).
-   El bucle modal de llamada de COM opera de forma diferente en un ASTA. Ya no se pueden recibir llamadas no relacionadas mientras haya una llamada saliente en curso. Por ejemplo, el siguiente comportamiento creará un interbloqueo de un ASTA (y hará que la aplicación se bloquee inmediatamente después):
    1.  El ASTA llama a un objeto MTA y pasa un puntero de interfaz P1.
    2.  Después, el ASTA llama al mismo objeto MTA. El objeto MTA llama a P1 antes de volver al ASTA.
    3.  P1 no puede entrar en el ASTA porque está bloqueado con una llamada no relacionada. Sin embargo, el subproceso de MTA está bloqueado mientras intenta realizar la llamada a P1.

    Para resolverlo:
    -   Usa el patrón **async** definido en la Biblioteca de patrones de procesamiento paralelo (PPLTasks.h).
    -   Llama a [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) desde el ASTA de la aplicación (el subproceso principal de la aplicación) lo antes posible para permitir las llamadas arbitrarias.

    Dicho esto, no puedes depender del envío inmediato de llamadas no relacionadas al ASTA de tu aplicación. Para obtener más información sobre las llamadas asincrónicas, consulta [Programación asincrónica en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

En general, cuando diseñes tu aplicación para UWP, usa la clase [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) para la clase [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) y el método [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) de tu aplicación para controlar todos los subprocesos de interfaz de usuario en vez de intentar crear y administrar tú mismo los subprocesos de MTA. Cuando necesites un subproceso independiente que no puedas controlar con **CoreDispatcher**, usa patrones asincrónicos y sigue las instrucciones mencionadas más arriba para evitar problemas de reentrada.

 

 




