---
author: drewbatgit
ms.assetid: 
description: "En este artículo se muestra cómo reproducir elementos multimedia mientras la aplicación se está ejecutando en segundo plano."
title: Reproducir elementos multimedia en segundo plano
translationtype: Human Translation
ms.sourcegitcommit: c8cbc538e0979f48b657197d59cb94a90bc61210
ms.openlocfilehash: a477827553ac1780ac625deeee08d84ab638d4c2

---

# Reproducir elementos multimedia en segundo plano
En este artículo se muestra cómo configurar la aplicación para que sigan reproduciéndose los elementos multimedia cuando la aplicación se mueva del primer plano al segundo plano. Esto significa que incluso después de que el usuario haya minimizado la aplicación, haya regresado a la pantalla principal o haya salido de la aplicación de alguna otra manera, la aplicación podrá seguir reproduciendo audio. 

Entre los escenarios para reproducir audio en segundo plano encontramos:

-   **Listas de reproducción de larga duración** El usuario selecciona brevemente una aplicación en primer plano para elegir e iniciar una lista de reproducción. Con ello, el usuario espera que dicha lista de reproducción siga sonando en segundo plano.

-   **Uso del conmutador de tareas** El usuario abre rápidamente una aplicación en primer plano para iniciar la reproducción de audio y después cambia a otra aplicación que ya estaba abierta mediante el conmutador de tareas. El usuario espera que el audio siga reproduciéndose en segundo plano.

La implementación de audio en segundo plano que se describe en este artículo te permitirá que la aplicación se ejecute universalmente en todos los dispositivos Windows, incluidos los dispositivos móviles, los dispositivos de escritorio y Xbox.

> [!NOTE]
> El código de este artículo es una adaptación de la [Background Audio sample](http://go.microsoft.com/fwlink/p/?LinkId=800141) (Muestra de audio en segundo plano) para la UWP.

## Explicación del modelo de proceso único
Con Windows10, versión 1607, se ha introducido un nuevo modelo de proceso único que simplifica enormemente el proceso de habilitar el audio en segundo plano. Anteriormente, la aplicación debía administrar un proceso en segundo plano además de la aplicación en primer plano y, a continuación, comunicar manualmente los cambios de estado entre los dos procesos. En el nuevo modelo, simplemente debe agregarse la funcionalidad de audio en segundo plano al manifiesto de la aplicación y dicha aplicación continuará reproduciendo audio automáticamente cuando se mueva a segundo plano. Dos nuevos eventos de ciclo de vida de la aplicación, [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) y [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) permiten a tu aplicación saber cuándo entra y sale del segundo plano. Cuando la aplicación se mueve a las transiciones al segundo plano o desde este, las restricciones de memoria que impone el sistema pueden cambiar, así que puedes usar estos eventos para comprobar el consumo de memoria actual y liberar recursos para que permanezcan por debajo del límite.

Al eliminar la compleja comunicación entre procesos y la administración de estados, el nuevo modelo permite implementar audio en segundo plano mucho más rápidamente y con una reducción significativa del código. Sin embargo, en la versión actual se sigue admitiendo el modelo de dos procesos por motivos de compatibilidad con versiones anteriores. Para obtener más información, consulta [Modelo de audio en segundo plano heredado](background-audio.md).

## Requisitos para el audio en segundo plano
La aplicación debe cumplir los siguientes requisitos para reproducir audio mientras está en segundo plano.

* Agrega la funcionalidad **Background Media Playback** al manifiesto de la aplicación, como se describe más adelante en este artículo.
* Si la aplicación deshabilita la integración automática de **MediaPlayer** con los controles de transporte de contenido multimedia del sistema (SMTC), por ejemplo, al establecer la propiedad [**CommandManager.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) en "false", debes implementar la integración manual con los SMTC para habilitar la reproducción de contenido multimedia en segundo plano. También deberás realizar manualmente la integración con SMTC usas una API que no sea **MediaPlayer**, como [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph), para reproducir audio, si quieres que el audio continúe reproduciéndose cuando la aplicación pase a segundo plano. Los requisitos mínimos para la integración de SMTC se describen en la sección "Usar los controles de transporte de medios del sistema de audio en segundo plano" de [Control manual de los controles de transporte de multimedia del sistema](system-media-transport-controls.md).
* Mientras la aplicación esté en segundo plano, debes mantenerte por debajo de los límites de uso de memoria establecidos por el sistema para aplicaciones en segundo plano. Más adelante en este artículo se proporciona orientación para administrar la memoria mientras la aplicación está en segundo plano.

## Funcionalidad de manifiesto de reproducción de elementos multimedia en segundo plano
Para habilitar el audio en segundo plano, debes agregar la funcionalidad de reproducción de elementos multimedia en segundo plano al archivo de manifiesto de la aplicación, Package.appxmanifest. 

**Agregar funcionalidades al manifiesto de la aplicación con el diseñador de manifiestos**

1.  En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2.  Selecciona la pestaña **Funcionalidades**.
3.  Selecciona la casilla **Reproducción de contenido multimedia en segundo plano**.

Para establecer la funcionalidad mediante la edición manual del xml del manifiesto de la aplicación, primero debes asegurarte de que el prefijo del espacio de nombres *uap3* está definido en el elemento **paquete**. De lo contrario, agrégalo como se muestra a continuación.
```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
```

A continuación, agrega la funcionalidad *backgroundMediaPlayback* al elemento **Capabilities**:
```xml
<Capabilities>
    <uap3:Capability Name="backgroundMediaPlayback"/>
</Capabilities>
```

##Controlar la transición entre el primer plano y el segundo plano
Cuando la aplicación se mueve del primer plano al segundo plano, se genera el evento [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground). Y cuando la aplicación vuelve al primer plano, se genera el evento [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground). Dado que estos son eventos de ciclo de vida de la aplicación, debes registrar controladores para estos eventos al crear la aplicación. En la plantilla de proyecto predeterminada, esto significa agregarlos al constructor de clase **App** en App.xaml.cs. Como la ejecución en segundo plano reducirá los recursos de memoria que el sistema permite retener a la aplicación, también debes registrar los eventos [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) y [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging), que se usará para comprobar el uso de memoria actual de la aplicación y el límite actual. En los siguientes ejemplos se muestran los controladores de estos eventos. Para obtener más información sobre el ciclo de vida de las aplicaciones para UWP, consulta [Ciclo de vida de la aplicación](../\launch-resume\app-lifecycle.md).

[!code-cs[RegisterEvents](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetRegisterEvents)]

Crea una variable para realizar el seguimiento de si actualmente estás ejecutando en segundo plano.

[!code-cs[DeclareBackgroundMode](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetDeclareBackgroundMode)]

Cuando se genera el evento [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground), establece la variable de seguimiento para indicar que actualmente se está ejecutando en segundo plano. No se deben realizar tareas de larga duración en el evento **EnteredBackground** porque esto puede causar que al usuario la transición a segundo plano le parezca lenta.

[!code-cs[EnteredBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetEnteredBackground)]

Cuando la aplicación cambia a segundo plano, el sistema reduce el límite de memoria para la aplicación con el fin de garantizar que la aplicación en primer plano actual tiene los recursos suficientes para proporcionar una experiencia del usuario que responde adecuadamente. El controlador del evento [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) permite a la aplicación saber que la memoria que tiene asignada se ha reducido y proporciona el nuevo límite en los argumentos del evento pasados al controlador. Compara la propiedad [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsage), que proporciona el uso actual de la aplicación, con propiedad la [**NewLimit**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLimitChangingEventArgs.NewLimit) de los argumentos del evento, que especifica el nuevo límite. Si el uso de memoria supera el límite, deberás reducir dicho uso de memoria. En este ejemplo, esto se realiza en el método auxiliar **ReduceMemoryUsage**, que se define más adelante en este artículo.

[!code-cs[MemoryUsageLimitChanging](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE] 
> Las configuraciones de algunos dispositivo permitirán una aplicación seguir ejecutándose por encima del nuevo límite de memoria hasta que el sistema sufra presión de recursos, y las de otros no lo permitirán. En concreto, en Xbox, las aplicaciones se suspenderán o finalizarán si no reducen la memoria por debajo de los nuevos límites en un plazo de 2 >>> segundos. Esto significa que puedes ofrecer la mejor experiencia en la gama más amplia de dispositivos mediante el uso de este evento para reducir el uso de recursos por debajo del límite en el plazo de 2 segundos desde que se genere el evento.


Es posible que cuando la aplicación pase a segundo plano por primera vez, su uso de memoria esté por debajo del límite de memoria para las aplicaciones en segundo plano, pero que luego, en algún momento posterior, aumente su uso y empiece a acercarse a dicho límite. El controlador [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) te ofrece la oportunidad de comprobar el uso actual cuando aumenta y, si es necesario, liberar memoria. Comprueba si [**AppMemoryUsageLevel**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLevel) es **High** o **OverLimit**, y si es así, reduzca el uso de memoria. De nuevo, en este ejemplo este proceso lo controla el método auxiliar **ReduceMemoryUsage**. También puede suscribirte al evento [**AppMemoryUsageDecreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageDecreased), comprobar si la aplicación está por debajo del límite y, si es así, saber que puedes asignar recursos adicionales.

[!code-cs[MemoryUsageIncreased](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

**ReduceMemoryUsage** es un método auxiliar que se puede implementar para liberar memoria cuando la aplicación supere el límite de uso para las aplicaciones que se ejecutan en segundo plano. La manera de liberar memoria depende de los detalles específicos de la aplicación, pero una forma recomendada de liberar memoria es desechar la interfaz de usuario y los demás recursos asociados con la vista de la aplicación. En primer lugar, asegúrate de que se ejecuta en modo en segundo plano y, a continuación, establece la propiedad [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) de la ventana de la aplicación en null. Llama a **GC. Collect** para indicar al sistema que recupere inmediatamente la memoria liberada.

[!code-cs[UnloadViewContent](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetUnloadViewContent)]

Cuando se recopila el contenido de la ventana, cada fotograma comienza su proceso de desconexión. Si hay objetos Page en el árbol de objetos visuales bajo el contenido de la ventana, iniciarán la generación del evento [**Unloaded**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Unloaded). Los objetos Pages no se puede borrar completamente de la memoria, a menos que se eliminen todas las referencias a ellos. En la devolución de llamada de **Unloaded**, realice los siguientes pasos para garantizar que la memoria se libere rápidamente:
* Borra y establece cualquier estructura de datos de gran tamaño de Page en null.
* Anular el registro de todos los controladores de eventos que tienen métodos de devolución de llamada dentro de Page. Asegúrate de registrar esas devoluciones de llamada durante el controlador del evento Loaded correspondiente a Page. El evento Loaded se genera si la interfaz de usuario se ha reconstituido y el objeto Page se ha agregado al árbol de objetos visuales.
* Llama a **GC. Recopilar** al final de la devolución de llamada de Unloaded para efectuar rápidamente la recolección de elementos no utilizados de cualquier de estructura de datos de gran tamaño que se acabe de establecer en null.

[!code-cs[Unloaded](./code/BackgroundAudio_RS1/cs/MainPage.xaml.cs#SnippetUnloaded)]

En el controlador del evento [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground), debes establecer la variable de seguimiento para indicar que la aplicación ya no se ejecuta en segundo plano. A continuación, comprueba si el [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) de la ventana actual es null, que será así si has desechado las vistas de la aplicación para borrar la memoria mientras se ejecutaba en segundo plano. Si el contenido de la ventana es null, reconstruya la vista de la aplicación. En este ejemplo, el contenido de la ventana se crea en el método auxiliar **CreateRootFrame**.

[!code-cs[LeavingBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetLeavingBackground)]

El método auxiliar **CreateRootFrame** vuelve a crear el contenido de la vista de la aplicación. El código de este método es idéntico al del código del controlador [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) proporcionado en la plantilla de proyecto predeterminada. La única diferencia es que el controlador **Launching** determina el estado de ejecución anterior a partir de la propiedad [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs.PreviousExecutionState) de la clase[**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) y el **CreateRootFrame** simplemente obtiene el estado de ejecución anterior pasado como argumento. Para minimizar el código duplicado, puedes refactorizar el código del controlador de eventos **Launching** predeterminado para llamar a **CreateRootFrame** si lo deseas.

[!code-cs[CreateRootFrame](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetCreateRootFrame)]

## Disponibilidad de la red para las aplicaciones multimedia en segundo plano
Todos los orígenes multimedia con reconocimiento de red, aquellos que no se crean a partir de una emisión o un archivo, mantendrán la conexión de red activa mientras recuperan contenido remoto y la liberarán cuando no lo estén haciendo. [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaStreamSource), específicamente, depende de la aplicación para notificar a la plataforma de forma correcta el rango en búfer mediante [**SetBufferedRange**](https://msdn.microsoft.com/library/windows/apps/dn282762). Una vez que todo el contenido esté almacenado en búfer, la red ya no estará reservada en nombre de la aplicación.

Si necesitas realizar llamadas de red que se produzcan en segundo plano cuando no se está descargando contenido multimedia, estas deben estar encapsuladas en una tarea apropiada, como [**ApplicationTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.ApplicationTrigger), [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.MaintenanceTrigger) o [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.TimeTrigger). Para obtener más información, consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](https://msdn.microsoft.com/en-us/windows/uwp/launch-resume/support-your-app-with-background-tasks).

## Temas relacionados
* [Reproducción de contenido multimedia](media-playback.md)
* [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Integrar con los controles de transporte multimedia del sistema](integrate-with-systemmediatransportcontrols.md)
* [Muestra de audio en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 







<!--HONumber=Aug16_HO3-->


