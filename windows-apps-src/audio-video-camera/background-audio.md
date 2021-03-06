---
ms.assetid: b7333924-d641-4ba5-92a2-65925b44ccaa
description: En este artículo se muestra cómo reproducir elementos multimedia mientras la aplicación se está ejecutando en segundo plano.
title: Reproducir elementos multimedia en segundo plano
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a0c816470f4a6caf79cb3370a39bc76abb7ef878
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364038"
---
# <a name="play-media-in-the-background"></a>Reproducir elementos multimedia en segundo plano
En este artículo se muestra cómo configurar la aplicación para que sigan reproduciéndose los elementos multimedia cuando la aplicación se mueva del primer plano al segundo plano. Esto significa que, incluso después de que el usuario haya minimizado la aplicación, regresado a la pantalla principal o salido de la aplicación de alguna otra manera, la aplicación podrá seguir reproduciendo audio. 

Entre los escenarios para reproducir audio en segundo plano encontramos:

-   **Listas de reproducción de larga duración** El usuario selecciona brevemente una aplicación en primer plano para elegir e iniciar una lista de reproducción. Con ello, el usuario espera que dicha lista de reproducción siga sonando en segundo plano.

-   **Uso del conmutador de tareas:** el usuario abre rápidamente una aplicación en primer plano para iniciar la reproducción de audio y, después, cambia a otra aplicación que ya estaba abierta mediante el conmutador de tareas. El usuario espera que el audio siga reproduciéndose en segundo plano.

La implementación de audio en segundo plano que se describe en este artículo te permitirá que la aplicación se ejecute universalmente en todos los dispositivos Windows, incluidos los dispositivos móviles, los dispositivos de escritorio y Xbox.

> [!NOTE]
> El código de este artículo es una adaptación de la [Background Audio sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback) (Muestra de audio en segundo plano) para la UWP.

## <a name="explanation-of-one-process-model"></a>Explicación del modelo de proceso único
Con Windows 10, versión 1607, se ha introducido un nuevo modelo de proceso único que simplifica enormemente el proceso de habilitar el audio en segundo plano. Anteriormente, la aplicación debía administrar un proceso en segundo plano además de la aplicación en primer plano y, a continuación, comunicar manualmente los cambios de estado entre los dos procesos. En el nuevo modelo, simplemente debe agregarse la funcionalidad de audio en segundo plano al manifiesto de la aplicación y dicha aplicación continuará reproduciendo audio automáticamente cuando se mueva a segundo plano. Dos nuevos eventos de ciclo de vida de la aplicación, [**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) y [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) permiten a tu aplicación saber cuándo entra y sale del segundo plano. Cuando la aplicación se mueve a las transiciones al segundo plano o desde este, las restricciones de memoria que impone el sistema pueden cambiar, así que puedes usar estos eventos para comprobar el consumo de memoria actual y liberar recursos para que permanezcan por debajo del límite.

Al eliminar la compleja comunicación entre procesos y la administración de estados, el nuevo modelo permite implementar audio en segundo plano mucho más rápidamente y con una reducción significativa del código. Sin embargo, en la versión actual se sigue admitiendo el modelo de dos procesos por motivos de compatibilidad con versiones anteriores. Para obtener más información, consulta [Modelo de audio en segundo plano heredado](legacy-background-media-playback.md).

## <a name="requirements-for-background-audio"></a>Requisitos para el audio en segundo plano
La aplicación debe cumplir los siguientes requisitos para reproducir audio mientras está en segundo plano.

* Agrega la funcionalidad **Background Media Playback** al manifiesto de la aplicación, como se describe más adelante en este artículo.
* Si la aplicación deshabilita la integración automática de **MediaPlayer** con los controles de transporte de contenido multimedia del sistema (SMTC), por ejemplo, al establecer la propiedad [**CommandManager.IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) en "false", debes implementar la integración manual con los SMTC para habilitar la reproducción de contenido multimedia en segundo plano. También deberás realizar manualmente la integración con SMTC usas una API que no sea **MediaPlayer**, como [**AudioGraph**](/uwp/api/Windows.Media.Audio.AudioGraph), para reproducir audio, si quieres que el audio continúe reproduciéndose cuando la aplicación pase a segundo plano. Los requisitos mínimos para la integración de SMTC se describen en la sección "Usar los controles de transporte de medios del sistema de audio en segundo plano" de [Control manual de los controles de transporte de multimedia del sistema](system-media-transport-controls.md).
* Mientras la aplicación esté en segundo plano, debes mantenerte por debajo de los límites de uso de memoria establecidos por el sistema para aplicaciones en segundo plano. Más adelante en este artículo se proporciona orientación para administrar la memoria mientras la aplicación está en segundo plano.

## <a name="background-media-playback-manifest-capability"></a>Funcionalidad de manifiesto de reproducción de elementos multimedia en segundo plano
Para habilitar el audio en segundo plano, debes agregar la funcionalidad de reproducción de elementos multimedia en segundo plano al archivo de manifiesto de la aplicación, Package.appxmanifest. 

**Agregar funcionalidades al manifiesto de la aplicación con el diseñador de manifiestos**

1.  En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2.  Seleccione la pestaña **Funcionalidades**.
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

## <a name="handle-transitioning-between-foreground-and-background"></a>Controlar la transición entre el primer plano y el segundo plano
Cuando la aplicación pasa de primer plano a segundo plano, se genera el evento [**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground). Y cuando la aplicación vuelve al primer plano, se genera el evento [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground). Dado que estos son eventos de ciclo de vida de la aplicación, debes registrar controladores para estos eventos al crear la aplicación. En la plantilla de proyecto predeterminada, esto significa agregarlos al constructor de clase **App** en App.xaml.cs. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BackgroundAudio_RS1/cs/App.xaml.cs" id="SnippetRegisterEvents":::

Crea una variable para realizar el seguimiento de si actualmente estás ejecutando en segundo plano.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BackgroundAudio_RS1/cs/App.xaml.cs" id="SnippetDeclareBackgroundMode":::

Cuando se genera el evento [**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground), establece la variable de seguimiento para indicar que actualmente estás ejecutando en segundo plano. No se deben realizar tareas de larga duración en el evento **EnteredBackground** porque esto puede causar que al usuario la transición a segundo plano le parezca lenta.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BackgroundAudio_RS1/cs/App.xaml.cs" id="SnippetEnteredBackground":::

En el controlador del evento [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground), debes establecer la variable de seguimiento para indicar que la aplicación ya no se ejecuta en segundo plano.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BackgroundAudio_RS1/cs/App.xaml.cs" id="SnippetLeavingBackground":::

### <a name="memory-management-requirements"></a>Requisitos de administración de memoria
La parte más importante de controlar la transición entre el primer y segundo plano es administrar la memoria que usa tu aplicación. Como la ejecución en segundo plano reducirá los recursos de memoria que el sistema permite retener a la aplicación, también debes registrar los eventos [**AppMemoryUsageIncreased**](/uwp/api/windows.system.memorymanager.appmemoryusageincreased) y [**AppMemoryUsageLimitChanging**](/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging). Cuando se generan estos eventos, debes comprobar el límite actual y el uso de memoria actual de la aplicación y, a continuación, reducir el uso de memoria si es necesario. Para obtener información acerca de cómo reducir el uso de memoria mientras se ejecuta en segundo plano, consulta [Liberar memoria cuando la aplicación pasa a segundo plano](../launch-resume/reduce-memory-usage.md).

## <a name="network-availability-for-background-media-apps"></a>Disponibilidad de la red para las aplicaciones multimedia en segundo plano
Todos los orígenes multimedia con reconocimiento de red, aquellos que no se crean a partir de una emisión o un archivo, mantendrán la conexión de red activa mientras recuperan contenido remoto y la liberarán cuando no lo estén haciendo. [**MediaStreamSource**](/uwp/api/Windows.Media.Core.MediaStreamSource), específicamente, depende de la aplicación para notificar a la plataforma de forma correcta el rango en búfer mediante [**SetBufferedRange**](/uwp/api/windows.media.core.mediastreamsource.setbufferedrange). Una vez que todo el contenido esté almacenado en búfer, la red ya no estará reservada en nombre de la aplicación.

Si necesita realizar llamadas de red que se producen en segundo plano cuando los medios no se están descargando, se deben encapsular en una tarea adecuada, como [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) o [**TimeTrigger**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger). Para obtener más información, consulte [compatibilidad con la aplicación con tareas en segundo plano](../launch-resume/support-your-app-with-background-tasks.md).

## <a name="related-topics"></a>Temas relacionados
* [Reproducción de multimedia](media-playback.md)
* [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Integrar con los controles de transporte de contenido multimedia del sistema](integrate-with-systemmediatransportcontrols.md)
* [Muestra de audio en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 
