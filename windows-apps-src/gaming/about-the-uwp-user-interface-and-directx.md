---
title: Objeto de aplicación y DirectX
description: Los juegos DirectX para Plataforma universal de Windows (UWP) no usan muchos elementos y objetos de la interfaz de usuario de Windows.
ms.assetid: 46f92156-29f8-d65e-2587-7ba1de5b48a6
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, DirectX, objeto de aplicación
ms.localizationpriority: medium
ms.openlocfilehash: 29eaba70a7114624474275b8f98ec77f8038b2b0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163169"
---
# <a name="the-app-object-and-directx"></a>Objeto de aplicación y DirectX

Los juegos DirectX para Plataforma universal de Windows (UWP) no usan muchos elementos y objetos de la interfaz de usuario de Windows. Como se ejecutan a un nivel más bajo en la pila de Windows Runtime, deben interoperar con el marco de la interfaz de usuario de una manera más básica, mediante el acceso y la interoperación con el objeto de aplicación directamente. Aprende cuándo y cómo se realiza esta interoperación y de qué manera puedes usar eficazmente, como desarrollador de DirectX, este modelo en el desarrollo de una aplicación para UWP.

Vea el [Glosario de gráficos de Direct3D](../graphics-concepts/index.md) para obtener información sobre los conceptos o términos de gráficos desconocidos que se encuentran durante la lectura.

## <a name="the-important-core-user-interface-namespaces"></a>Los espacios de nombres de interfaz de usuario principales

En primer lugar, veamos los espacios de nombres de Windows Runtime que debes incluir (con **using**) en la aplicación para UWP. Pronto los describiremos en detalle.

-   [**Windows. ApplicationModel. Core**](/uwp/api/Windows.ApplicationModel.Core)
-   [**Windows. ApplicationModel. Activation**](/uwp/api/Windows.ApplicationModel.Activation)
-   [**Windows. UI. Core**](/uwp/api/Windows.UI.Core)
-   [**Windows.System**](/uwp/api/Windows.System)
-   [**Windows.Foundation**](/uwp/api/Windows.Foundation)

> [!NOTE]
> Si no va a desarrollar una aplicación para UWP, use los componentes de la interfaz de usuario proporcionados en las bibliotecas y los espacios de nombres específicos de JavaScript o XAML en lugar de los tipos proporcionados en estos espacios de nombres.

## <a name="the-windows-runtime-app-object"></a>El objeto de aplicación de Windows Runtime

Es posible que desees que tu aplicación para UWP obtenga una ventana y un proveedor de vistas de los que se pueda obtener una vista y a los que se pueda conectar la cadena de intercambio (los búferes de presentación). También puedes enlazar esta vista a los eventos específicos de ventana para la aplicación en ejecución. Para obtener la ventana primaria del objeto de aplicación, definida por el tipo [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) , cree un tipo que implemente [**IFrameworkViewSource**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkViewSource). Para ver un ejemplo de código de [C++/WinRT](../cpp-and-winrt-apis/index.md) que muestra cómo implementar **IFrameworkViewSource**, consulte [interoperación nativa de composición con DirectX y Direct2D](../composition/composition-native-interop.md).

Este es el conjunto básico de pasos para obtener una ventana mediante el marco de la interfaz de usuario principal.

1.  Crea un tipo que implemente [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView). Esta será la vista.

    En este tipo, define:

    -   Un método [**Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) que use una instancia de [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) como parámetro. Puedes obtener una instancia de este tipo si llamas a [**CoreApplication.CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview). El objeto de aplicación lo llama cuando se inicia la aplicación.
    -   Un método [**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow) que use una instancia de [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) como parámetro. Puedes obtener una instancia de este tipo si accedes a la propiedad [**CoreWindow**](/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) en la nueva instancia de [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView).
    -   Un método [**Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load) que use una cadena para un punto de entrada como parámetro único. El objeto aplicación proporciona la cadena de punto de entrada al llamar a este método. Ahora es cuando hay que configurar recursos. Aquí es donde crearás los recursos de dispositivo. El objeto de aplicación lo llama cuando se inicia la aplicación.
    -   Un método [**Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) que active el objeto [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) e inicie el distribuidor de eventos de ventana. El objeto aplicación lo llama cuando se inicia el proceso de la aplicación.
    -   Un método [**Uninitialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize) que limpie los recursos configurado en la llamada a [**Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load). El objeto de aplicación llama a este método cuando se cierra la aplicación.

2.  Crea un tipo que implemente [**IFrameworkViewSource**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkViewSource). Este será el proveedor de vistas.

    En este tipo, define:

    -   Un método denominado [**CreateView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource.createview) que devuelva una instancia de la implementación de [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView), tal como la creaste en el paso 1.

3.  Pase una instancia del proveedor de vistas a [**CoreApplication. Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run) desde **Main**.

Con estos conceptos básicos en mente, veamos otras opciones para ampliar este enfoque.

## <a name="core-user-interface-types"></a>Tipos de interfaz de usuario principales

Estos son otros tipos de interfaz de usuario principales de Windows Runtime que podrían resultar útiles:

-   [**Windows. ApplicationModel. Core. CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)
-   [**Windows.UI.Core.CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)
-   [**Windows.UI.Core.CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)

Puedes usar estos tipos para acceder a la vista de la aplicación, en concreto, a las partes que extraen el contenido de la ventana primaria de la aplicación y controlan los eventos desencadenados de esa ventana. El proceso de la ventana de la aplicación es un *contenedor uniproceso de aplicación* (ASTA) que está aislado y controla todas las devoluciones de llamadas.

La vista de la aplicación la genera el proveedor de vistas para la ventana de aplicación y, en la mayoría de los casos, la implementará un paquete de marco específico o el mismo sistema, por lo que no tendrás que implementarla. Para DirectX debes implementar un proveedor de vistas ligero, como se indicó previamente. Hay una relación uno a uno específica entre los siguientes componentes y comportamientos:

-   Una vista de la aplicación, que se representa mediante el tipo [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView), y que define los métodos de actualización de la ventana.
-   Un ASTA, cuya atribución define el comportamiento de subprocesos para la aplicación. No puedes crear instancias de tipos con atributos STA de COM en un ASTA.
-   Un proveedor de vistas, que la aplicación obtiene del sistema o que puedes implementar.
-   Una ventana primaria, que se representa mediante el tipo [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow).
-   Origen para todos los eventos de activación. Las vistas y las ventanas tienen eventos de activación independientes.

En resumen, el objeto aplicación proporciona una fábrica de proveedores de vistas. Crea un proveedor de vistas y una instancia de ventana primaria para la aplicación. El proveedor de vistas define la vista de la aplicación para la ventana primaria de la aplicación. Veamos ahora los detalles de la vista y la ventana primaria.

## <a name="coreapplicationview-behaviors-and-properties"></a>Comportamientos y propiedades de CoreApplicationView

[**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) representa la vista actual de la aplicación. El singleton de aplicación crea la vista de la aplicación durante la inicialización, pero la vista permanece inactiva hasta que se activa. Puedes obtener la clase [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) que muestra la vista al accediendo a la propiedad [**CoreApplicationView.CoreWindow**](/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) de esta y puedes controlar eventos de activación y desactivación para la vista registrando delegados con el evento [**CoreApplicationView.Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated).

## <a name="corewindow-behaviors-and-properties"></a>Comportamientos y propiedades de CoreWindow

La ventana primaria, que es una instancia de [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow), se crea y se pasa al proveedor de vistas cuando se inicializa el objeto de aplicación. Si la aplicación tiene una ventana para mostrar, la muestra; de lo contrario, simplemente inicializa la vista.

[**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) proporciona diversos eventos específicos de entrada y comportamientos básicos de ventana. Puedes controlar estos eventos registrando tus propios delegados con ellos.

También puede obtener el distribuidor de eventos de ventana para la ventana accediendo a la propiedad [**CoreWindow. Dispatcher**](/uwp/api/windows.ui.core.corewindow.dispatcher) , que proporciona una instancia de [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher).

## <a name="coredispatcher-behaviors-and-properties"></a>Comportamientos y propiedades de CoreDispatcher

Puedes determinar el comportamiento de los subprocesos de la distribución de eventos para una ventana con el tipo [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher). En este tipo, hay un método especialmente importante: el método [**CoreDispatcher.ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents), que inicia el procesamiento de eventos de ventana. Llamar a este método con la opción incorrecta para la aplicación puede provocar comportamientos de procesamiento de eventos inesperados de todo tipo.

| Opción CoreProcessEventsOption                                                           | Descripción                                                                                                                                                                                                                                  |
|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [**CoreProcessEventsOption.ProcessOneAndAllPending**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption) | Distribuye todos los eventos disponibles actualmente en la cola. Si no hay eventos pendientes, espera al siguiente evento nuevo.                                                                                                                                 |
| [**CoreProcessEventsOption.ProcessOneIfPresent**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption)     | Distribuye un evento si está pendiente en la cola. Si no hay eventos pendientes, no espera a que se genere un evento nuevo, sino que se devuelve inmediatamente.                                                                                          |
| [**CoreProcessEventsOption.ProcessUntilQuit**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption)        | Espera eventos nuevos y distribuye todos los eventos disponibles. Continúa con este comportamiento hasta que la ventana se cierra o la aplicación llama al método [**Close**](/uwp/api/windows.ui.core.corewindow.close) en la instancia de [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow). |
| [**CoreProcessEventsOption.ProcessAllIfPresent**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption)     | Distribuye todos los eventos disponibles actualmente en la cola. Si no hay eventos pendientes, se devuelve inmediatamente.                                                                                                                                          |
UWP con DirectX debe usar la opción [**CoreProcessEventsOption.ProcessAllIfPresent**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption) para evitar comportamientos de bloqueo que puedan interrumpir las actualizaciones de elementos gráficos.

## <a name="asta-considerations-for-directx-devs"></a>Consideraciones de ASTA para desarrolladores con DirectX

El objeto de aplicación que define la representación en tiempo de ejecución de tu aplicación para UWP y DirectX usa un modelo de subprocesos denominado contenedor uniproceso de aplicación (ASTA) para hospedar las vistas de la interfaz de usuario de tu aplicación. Si estás desarrollando una aplicación para UWP y DirectX, estarás familiarizado con las propiedades de ASTA, porque los subprocesos que envíes desde tu aplicación para UWP y DirectX deben usar las API de [**Windows::System::Threading**](/uwp/api/Windows.System.Threading) o deben usar [**CoreWindow::CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher). (Puedes obtener el objeto [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) del ASTA llamando a [**CoreWindow::GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) desde la aplicación).

Lo más importante que debe tener en cuenta, como desarrollador de una aplicación DirectX de UWP, es que debe habilitar el subproceso de la aplicación para enviar subprocesos del MTA estableciendo **Platform:: MTAThread** en **Main ()**.

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

-   Los primitivos de espera, como [**CoWaitForMultipleObjects**](/windows/win32/api/combaseapi/nf-combaseapi-cowaitformultipleobjects), se comportan de forma diferente en un ASTA que en un contenedor uniproceso (STA).
-   El bucle modal de llamada de COM opera de forma diferente en un ASTA. Ya no se pueden recibir llamadas no relacionadas mientras haya una llamada saliente en curso. Por ejemplo, el siguiente comportamiento creará un interbloqueo de un ASTA (y hará que la aplicación se bloquee inmediatamente después):
    1.  El ASTA llama a un objeto MTA y pasa un puntero de interfaz P1.
    2.  Después, el ASTA llama al mismo objeto MTA. El objeto MTA llama a P1 antes de volver al ASTA.
    3.  P1 no puede entrar en el ASTA porque está bloqueado con una llamada no relacionada. Sin embargo, el subproceso de MTA está bloqueado mientras intenta realizar la llamada a P1.

    Para resolverlo:
    -   Usa el patrón **async** definido en la Biblioteca de patrones de procesamiento paralelo (PPLTasks.h).
    -   Llama a [**CoreDispatcher::ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) desde el ASTA de la aplicación (el subproceso principal de la aplicación) lo antes posible para permitir las llamadas arbitrarias.

    Dicho esto, no puedes depender del envío inmediato de llamadas no relacionadas al ASTA de tu aplicación. Para obtener más información sobre las llamadas asincrónicas, lea [programación asincrónica en C++](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

En general, cuando diseñes tu aplicación para UWP, usa la clase [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) para la clase [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) y el método [**CoreDispatcher::ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) de tu aplicación para controlar todos los subprocesos de interfaz de usuario en vez de intentar crear y administrar tú mismo los subprocesos de MTA. Cuando necesites un subproceso independiente que no puedas controlar con **CoreDispatcher**, usa patrones asincrónicos y sigue las instrucciones mencionadas más arriba para evitar problemas de reentrada.