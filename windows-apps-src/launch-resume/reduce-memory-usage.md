---
ms.assetid: 3a3ea86e-fa47-46ee-9e2e-f59644c0d1db
description: En este artículo se muestra cómo reducir el uso de memoria cuando la aplicación pasa al estado de segundo plano.
title: Reducir el uso de memoria cuando la aplicación pasa al estado de segundo plano
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0b5e0ea6deef7dfe3531c8d0406e08bfae80f0e2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260458"
---
# <a name="free-memory-when-your-app-moves-to-the-background"></a>Liberar memoria cuando la aplicación pase a segundo plano

En este artículo se muestra cómo reducir la cantidad de memoria que usa la aplicación cuando pasa al estado de segundo plano para evitar su suspensión o, posiblemente, su finalización.

## <a name="new-background-events"></a>Nuevos eventos de segundo plano

Windows 10, versión 1607, incorpora dos nuevos eventos de ciclo de vida de aplicación: [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) y [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground). Estos eventos permiten que la aplicación sepa cuándo entra y sale del estado de segundo plano.

Cuando la aplicación pasa al estado de segundo plano, pueden cambiar las restricciones de memoria que aplica el sistema. Usa estos eventos para comprobar el consumo de memoria actual y liberar recursos para permanecer por debajo del límite, de modo que no se suspenda o, tal vez, se finalice la aplicación mientras está en segundo plano.

### <a name="events-for-controlling-your-apps-memory-usage"></a>Eventos para controlar el uso de memoria de la aplicación

El evento [MemoryManager.AppMemoryUsageLimitChanging](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging) se genera justo antes de que cambie el límite del total de memoria que puede usar la aplicación. Por ejemplo, si la aplicación pasa a segundo plano y en la consola Xbox el límite de memoria cambia de 1024 MB a 128 MB.  
Este es el evento más importante que se debe controlar para evitar que la plataforma suspenda o finalice la aplicación.

El evento [MemoryManager.AppMemoryUsageIncreased](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusageincreased) se genera cuando el consumo de memoria de la aplicación ha aumentado a un valor superior en la enumeración [AppMemoryUsageLevel](https://docs.microsoft.com/uwp/api/windows.system.appmemoryusagelevel). Por ejemplo, de **Low** a **Medium**. Si bien controlar este evento es opcional, se recomienda hacerlo porque la aplicación sigue siendo responsable de mantenerse por debajo del límite.

El evento [MemoryManager.AppMemoryUsageDecreased](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagedecreased) se genera cuando el consumo de memoria de la aplicación ha disminuido a un valor inferior en la enumeración **AppMemoryUsageLevel**. Por ejemplo, de **High** a **Low**. Controlar este evento es opcional, pero indica que la aplicación puede asignar memoria adicional si es necesario.

## <a name="handle-the-transition-between-foreground-and-background"></a>Controlar la transición entre el primer plano y el segundo plano

Cuando la aplicación pasa de primer plano a segundo plano, se genera el evento [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground). Cuando la aplicación vuelve al primer plano, se genera el evento [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground). Puedes registrar controladores para estos eventos cuando creas la aplicación. En la plantilla de proyecto predeterminada, esto se realiza en el constructor de clase **App** en App.xaml.cs.

Dado que la ejecución en segundo plano reducirá los recursos de memoria que la aplicación tiene permitido conservar, también debes registrar los eventos [**AppMemoryUsageIncreased**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusageincreased) y [**AppMemoryUsageLimitChanging**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging), que puedes usar para comprobar el uso actual de memoria de la aplicación y el límite actual. En los siguientes ejemplos se muestran los controladores de estos eventos. Para obtener más información sobre el ciclo de vida de las aplicaciones para UWP, consulta [Ciclo de vida de la aplicación](..//launch-resume/app-lifecycle.md).

[!code-cs[RegisterEvents](./code/ReduceMemory/cs/App.xaml.cs#SnippetRegisterEvents)]

Cuando se genera el evento [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground), establece la variable de seguimiento para indicar que actualmente se está ejecutando en segundo plano. Esto será útil cuando escribas el código para reducir el uso de memoria.

[!code-cs[EnteredBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetEnteredBackground)]

Cuando la aplicación cambia al estado de segundo plano, el sistema reduce el límite de memoria de la aplicación con el fin de garantizar que la aplicación en primer plano actual tenga los recursos suficientes para proporcionar una experiencia del usuario que responda adecuadamente.

El controlador del evento [**AppMemoryUsageLimitChanging**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging) permite a la aplicación saber que la memoria que tiene asignada se ha reducido y proporciona el nuevo límite en los argumentos del evento pasados al controlador. Compara la propiedad [**MemoryManager.AppMemoryUsage**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusage), que proporciona el uso actual de la aplicación, con propiedad la [**NewLimit**](https://docs.microsoft.com/uwp/api/windows.system.appmemoryusagelimitchangingeventargs.newlimit) de los argumentos del evento, que especifica el nuevo límite. Si el uso de memoria supera el límite, debes reducir dicho uso de memoria.

En este ejemplo, esto se realiza en el método auxiliar **ReduceMemoryUsage**, que se define más adelante en este artículo.

[!code-cs[MemoryUsageLimitChanging](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE]
> Las configuraciones de algunos dispositivo permitirán que una aplicación siga ejecutándose por encima del nuevo límite de memoria hasta que el sistema sufra presión de recursos, pero las de otros dispositivos no lo permitirán. En concreto, en Xbox, las aplicaciones se suspenderán o finalizarán si no reducen el uso de memoria por debajo de los nuevos límites en un plazo de 2 (dos) segundos. Esto significa que puedes ofrecer la mejor experiencia en la gama más amplia de dispositivos mediante el uso de este evento para reducir el uso de recursos por debajo del límite en el plazo de 2 (dos) segundos desde que se genere el evento.

Es posible que, aunque el uso de memoria de la aplicación esté actualmente por debajo del límite de memoria para aplicaciones en segundo plano, cuando cambie por primera vez a segundo plano, pueda aumentar el consumo de memoria con el tiempo y empezar a acercarse al límite. El controlador del evento [**AppMemoryUsageIncreased**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusageincreased) ofrece la posibilidad de comprobar el uso actual cuando aumenta y, si es necesario, liberar memoria.

Comprueba si el valor de [**AppMemoryUsageLevel**](https://docs.microsoft.com/uwp/api/Windows.System.AppMemoryUsageLevel) es **High** u **OverLimit**, y si es así, reduce el uso de memoria. En este ejemplo, esto se controla con el método auxiliar **ReduceMemoryUsage**. También puede suscribirte al evento [**AppMemoryUsageDecreased**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagedecreased), comprobar si la aplicación está por debajo del límite y, si es así, saber que puedes asignar recursos adicionales.

[!code-cs[MemoryUsageIncreased](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

**ReduceMemoryUsage** es un método auxiliar que puedes implementar para liberar memoria cuando la aplicación supera el límite de uso mientras se ejecuta en segundo plano. La manera de liberar memoria depende de los detalles específicos de la aplicación, pero una forma recomendada es desechar la interfaz de usuario y los demás recursos asociados con la vista de la aplicación. Para ello, comprueba que estés ejecutando en el estado de segundo plano y luego establece la propiedad [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.content) de la ventana de la aplicación en `null`, anula el registro de los controladores de eventos de la interfaz de usuario y quita todas las referencias que pudieras tener a la página. Si no logras anular el registro de los controladores de eventos de la interfaz de usuario ni borrar todas las referencias que pudieras tener a la página, no podrás liberar los recursos de la página. A continuación, llama a **GC.Collect** para recuperar la memoria liberada inmediatamente. Por lo general, no hay que forzar la recolección de elementos no utilizados porque el sistema se encarga automáticamente. En este caso concreto, estamos reduciendo la cantidad de memoria que se carga a esta aplicación cuando va en segundo plano para reducir la probabilidad de que el sistema determine que debe finalizar la aplicación para recuperar la memoria.

[!code-cs[UnloadViewContent](./code/ReduceMemory/cs/App.xaml.cs#SnippetUnloadViewContent)]

Cuando se recopila el contenido de la ventana, cada fotograma comienza su proceso de desconexión. Si hay objetos Page en el árbol de objetos visuales en el contenido de la ventana, iniciarán la generación del evento Unloaded. Los objetos Pages no se puede borrar completamente de la memoria, a menos que se eliminen todas las referencias a ellos. En la devolución de llamada de Unloaded, realiza los siguientes pasos para garantizar que la memoria se libere rápidamente:
* Borra y establece cualquier estructura de datos de gran tamaño de Page en `null`.
* Anula el registro de todos los controladores de eventos que tienen métodos de devolución de llamada dentro de Page. Asegúrate de registrar esas devoluciones de llamada durante el controlador del evento Loaded correspondiente a Page. El evento Loaded se genera si la interfaz de usuario se ha reconstituido y el objeto Page se ha agregado al árbol de objetos visuales.
* Llama a `GC.Collect` al final de la devolución de llamada de Unloaded para efectuar rápidamente la recolección de elementos no utilizados de cualquiera de las estructuras de datos de gran tamaño que estableciste recién en `null`. Una vez más, por lo general, no hay que forzar la recolección de elementos no utilizados porque el sistema se encarga automáticamente. En este caso concreto, estamos reduciendo la cantidad de memoria que se carga a esta aplicación cuando va en segundo plano para reducir la probabilidad de que el sistema determine que debe finalizar la aplicación para recuperar la memoria.

[!code-cs[MainPageUnloaded](./code/ReduceMemory/cs/App.xaml.cs#SnippetMainPageUnloaded)]

En el controlador del evento [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground), establece la variable de seguimiento (`isInBackgroundMode`) para indicar que la aplicación ya no se ejecuta en segundo plano. A continuación, comprueba si el valor de la propiedad [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.content) de la ventana actual es `null`, que será así si has desechado las vistas de la aplicación para liberar memoria durante la ejecución en segundo plano. Si el contenido de la ventana es `null`, reconstruye la vista de la aplicación. En este ejemplo, el contenido de la ventana se crea en el método auxiliar **CreateRootFrame**.

[!code-cs[LeavingBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetLeavingBackground)]

El método auxiliar **CreateRootFrame** vuelve a crear el contenido de la vista de la aplicación. El código de este método es casi idéntico al código del controlador [**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) proporcionado en la plantilla de proyecto predeterminada. La única diferencia es que el controlador **Launching** determina el estado de ejecución anterior a partir de la propiedad [**PreviousExecutionState**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.previousexecutionstate) de la clase [**LaunchActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) y el método **CreateRootFrame** simplemente obtiene el estado de ejecución anterior que se pasó como argumento. Para minimizar el código duplicado, puedes refactorizar el código del controlador de eventos **Launching** predeterminado para llamar a **CreateRootFrame**.

[!code-cs[CreateRootFrame](./code/ReduceMemory/cs/App.xaml.cs#SnippetCreateRootFrame)]

## <a name="guidelines"></a>Instrucciones

### <a name="moving-from-the-foreground-to-the-background"></a>Pasar de primer plano a segundo plano

Cuando una aplicación pasa de primer plano a segundo plano, el sistema funciona en nombre de la aplicación para liberar los recursos que no son necesarios en segundo plano. Por ejemplo, los marcos de interfaz de usuario vacían las texturas almacenadas en caché y el subsistema de vídeo libera la memoria asignada en nombre de la aplicación. Aun así, una aplicación debe supervisar cuidadosamente su uso de memoria para evitar que el sistema la suspenda o la finalice.

Cuando una aplicación pasa de primer plano a segundo plano, primero obtiene un evento **EnteredBackground** y después un evento **AppMemoryUsageLimitChanging**.

- **Usa** el evento **EnteredBackground** para liberar los recursos de interfaz de usuario que sabes que la aplicación no necesita mientras se ejecuta en segundo plano. Por ejemplo, podrías liberar la imagen de portada de una canción.
- **Usa** el evento **MemoryManager.AppMemoryUsageLimitChanging** para asegurarte de que la aplicación esté usando menos memoria que el nuevo límite de segundo plano. Si no es así, asegúrate de liberar recursos. Si no lo haces, puede que se suspenda o finalice la aplicación según la directiva específica del dispositivo.
- **Invoca** manualmente el recolector de elementos no utilizados si la aplicación supera el nuevo límite de memoria cuando se genera el evento **AppMemoryUsageLimitChanging**.
- **Usa** el evento **AppMemoryUsageIncreased** para seguir supervisando el uso de memoria de la aplicación mientras se ejecuta en segundo plano si esperas que cambie. Si el valor de **AppMemoryUsageLevel** es **High** u **OverLimit**, asegúrate de liberar recursos.
- **Considera** liberar recursos de interfaz de usuario en el controlador del evento **AppMemoryUsageLimitChanging** en lugar de hacerlo en el controlador de **EnteredBackground** como una optimización del rendimiento. Usa un valor booleano que se establece los controladores de los eventos **EnteredBackground/LeavingBackground** para saber si la aplicación está en segundo plano o en primer plano. A continuación, en el controlador del evento **AppMemoryUsageLimitChanging**, si el valor de **AppMemoryUsage** supera el límite y la aplicación está en segundo plano (según el valor booleano), puedes liberar recursos de interfaz de usuario.
- **No** realices operaciones prolongadas en el evento **EnteredBackground** porque puedes provocar que al usuario la transición entre aplicaciones le parezca lenta.

### <a name="moving-from-the-background-to-the-foreground"></a>Pasar de segundo plano a primer plano

Cuando una aplicación pasa de segundo plano a primer plano, primero obtiene un evento **AppMemoryUsageLimitChanging** y después un evento **LeavingBackground**.

- **No** uses el evento **LeavingBackground** para volver a crear los recursos de interfaz de usuario que la aplicación desechó cuando pasó a segundo plano.

## <a name="related-topics"></a>Temas relacionados

* [Muestra de reproducción de contenido multimedia en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback): enseña cómo liberar memoria cuando la aplicación pasa al estado de segundo plano.
* [Herramientas de diagnóstico](https://devblogs.microsoft.com/devops/diagnostic-tools-debugger-window-in-visual-studio-2015/): usa las herramientas de diagnóstico para observar los eventos de recolección de elementos no utilizados y validar que la aplicación esté liberando memoria según lo previsto.
