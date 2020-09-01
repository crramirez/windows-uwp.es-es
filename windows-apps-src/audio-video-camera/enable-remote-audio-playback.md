---
description: En este artículo se muestra cómo usar AudioPlaybackConnection para habilitar dispositivos remotos conectados con Bluetooth para reproducir audio en el equipo local.
title: Habilitar la reproducción de audio desde dispositivos conectados por Bluetooth remotos
ms.date: 05/03/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8a23758612b3c595f808fe2ffe4f38e558cf0740
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163969"
---
# <a name="enable-audio-playback-from-remote-bluetooth-connected-devices"></a>Habilitar la reproducción de audio desde dispositivos conectados por Bluetooth remotos

En este artículo se muestra cómo usar [AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection) para habilitar dispositivos remotos conectados con Bluetooth para reproducir audio en el equipo local.

A partir de Windows 10, la versión 2004 de los orígenes de audio remotos pueden transmitir audio a dispositivos Windows, lo que permite escenarios como la configuración de un equipo para que se comporte como un altavoz Bluetooth y permitir que los usuarios escuchen audio desde su teléfono. La implementación de usa los componentes de Bluetooth en el sistema operativo para procesar los datos de audio entrantes y reproducirlos en los puntos de conexión de audio del sistema, como los altavoces de PC integrados o los auriculares con cable. La habilitación del receptor A2DP de Bluetooth subyacente está administrada por las aplicaciones, que son responsables del escenario del usuario final, en lugar del sistema.

La clase [AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection) se usa para habilitar y deshabilitar las conexiones desde un dispositivo remoto, así como para crear la conexión, lo que permite que se inicie la reproducción de audio remota.

## <a name="add-a-user-interface"></a>Agregar una interfaz de usuario

En los ejemplos de este artículo, usaremos la siguiente interfaz de usuario XAML simple que define el control **ListView** para mostrar los dispositivos remotos disponibles, un **TextBlock** para mostrar el estado de la conexión y tres botones para habilitar, deshabilitar y abrir conexiones.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml" id="snippet_AudioPlaybackConnectionXAML":::

## <a name="use-devicewatcher-to-monitor-for-remote-devices"></a>Usar DeviceWatcher para supervisar dispositivos remotos

La clase [DeviceWatcher](/uwp/api/windows.devices.enumeration.devicewatcher) permite detectar dispositivos conectados. El método [AudioPlaybackConnection. GetDeviceSelector](/uwp/api/windows.media.audio.audioplaybackconnection.getdeviceselector) devuelve una cadena que indica al observador del dispositivo qué tipos de dispositivos se deben supervisar. Pase esta cadena al constructor **DeviceWatcher** . 

El evento [DeviceWatcher. Added](/uwp/api/windows.devices.enumeration.devicewatcher.added) se genera para cada dispositivo que se conecta cuando se inicia el monitor de dispositivos, así como para cualquier dispositivo conectado mientras se ejecuta el monitor de dispositivos. El evento [DeviceWatcher. removed](/uwp/api/windows.devices.enumeration.devicewatcher.removed) se genera si se desconecta un dispositivo conectado previamente. 

Llame a [DeviceWatcher. Start](/uwp/api/windows.devices.enumeration.devicewatcher.start) para empezar a supervisar los dispositivos conectados que admiten conexiones de reproducción de audio. En este ejemplo, se iniciará el administrador de dispositivos cuando se cargue el control de **cuadrícula** principal en la interfaz de usuario. Para obtener más información sobre el uso de **DeviceWatcher**, consulte [enumerar dispositivos](../devices-sensors/enumerate-devices.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_MainGridLoaded":::


En el evento de **agregado** de Device Watcher, cada dispositivo detectado se representa mediante un objeto [DeviceInformation](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) . Agregue cada uno de los dispositivos detectados a una colección observable enlazada al control **ListView** en la interfaz de usuario.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeclareDevices":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeviceWatcher_Added":::


## <a name="enable-and-release-audio-playback-connections"></a>Habilitar y liberar conexiones de reproducción de audio

Antes de abrir una conexión con un dispositivo, la conexión debe estar habilitada. Esto informa al sistema de que hay una nueva aplicación que quiere que se reproduzca audio desde el dispositivo remoto en el equipo, pero el audio no comienza a reproducirse hasta que se abre la conexión, que se muestra en un paso posterior.

En el controlador de clics del botón **Habilitar conexión de reproducción de audio** , obtenga el identificador de dispositivo asociado al dispositivo seleccionado actualmente en el control **ListView** . En este ejemplo se mantiene un diccionario de objetos **AudioPlaybackConnection** que se han habilitado. Este método comprueba primero si ya existe una entrada en el diccionario para el dispositivo seleccionado. Después, el método intenta crear un **AudioPlaybackConnection** para el dispositivo seleccionado llamando a [TryCreateFromId](/uwp/api/windows.media.audio.audioplaybackconnection.trycreatefromid) y pasando el identificador de dispositivo seleccionado. 

Si la conexión se ha creado correctamente, agregue el nuevo objeto **AudioPlaybackConnection** al Diccionario de la aplicación, registre un controlador para el evento [StateChanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged) del objeto y llame a[StartAsync](/uwp/api/windows.media.audio.audioplaybackconnection.startasync) para notificar al sistema que la nueva conexión está habilitada. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeclareConnections":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_EnableAudioPlaybackConnection":::


## <a name="open-the-audio-playback-connection"></a>Apertura de la conexión de reproducción de audio

En el paso anterior, se creó una conexión de reproducción de audio, pero el sonido no comienza a reproducirse hasta que la conexión se abra llamando a [Open](/uwp/api/windows.media.audio.audioplaybackconnection.open) o [OpenAsync](/uwp/api/windows.media.audio.audioplaybackconnection.openasync). En el botón **abrir conexión de reproducción de audio** , haga clic en controlador, obtenga el dispositivo seleccionado actualmente y use el identificador para recuperar el **AudioPlaybackConnection** del Diccionario de conexiones de la aplicación. Espere una llamada a **OpenAsync** y compruebe el valor de **Estado** del objeto [AudioPlaybackConnectionOpenResultStatus](/uwp/api/windows.media.audio.audioplaybackconnectionopenresult) devuelto para ver si la conexión se abrió correctamente y, en caso afirmativo, actualice el cuadro de texto estado de la conexión.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_OpenAudioPlaybackConnectionButton":::

## <a name="monitor-audio-playback-connection-state"></a>Supervisar el estado de conexión de reproducción de audio

El evento [AudioPlaybackConnection. ConnectionStateChanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged) se desencadena cuando cambia el estado de la conexión. En este ejemplo, el controlador de este evento actualiza el cuadro de texto de estado. Recuerde actualizar la interfaz de usuario dentro de una llamada a [Dispatcher. RunAsync](/uwp/api/windows.ui.core.coredispatcher.runasync) para asegurarse de que la actualización se realiza en el subproceso de la interfaz de usuario.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_ConnectionStateChanged":::

## <a name="release-connections-and-handle-removed-devices"></a>Liberar conexiones y administrar dispositivos quitados

Este ejemplo proporciona un botón de **conexión de reproducción de audio de versión** para permitir que el usuario libere una conexión de reproducción de audio. En el controlador de este evento, obtenemos el dispositivo seleccionado actualmente y usamos el identificador del dispositivo para buscar el **AudioPlaybackConnection** en el diccionario. Llame a **Dispose** para liberar la referencia y liberar los recursos asociados y quitar la conexión del diccionario.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_ReleaseAudioPlaybackConnectionButton":::

Debe controlar el caso en el que se quita un dispositivo mientras una conexión está habilitada o abierta. Para ello, implemente un controlador para el evento [DeviceWatcher. removed](/uwp/api/windows.devices.enumeration.devicewatcher.removed) de Device Watcher. En primer lugar, se usa el identificador del dispositivo quitado para quitar el dispositivo de la colección observable enlazada al control **ListView** de la aplicación. Después, si una conexión asociada con este dispositivo se encuentra en el Diccionario de la aplicación, se llama a **Dispose** para liberar los recursos asociados y, a continuación, se quita la conexión del diccionario. Todo esto se realiza en una llamada a **Dispatcher. RunAsync** para asegurarse de que las actualizaciones de la interfaz de usuario se realizan en el subproceso de la interfaz de usuario.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeviceWatcher_Removed":::

## <a name="related-topics"></a>Temas relacionados

[Reproducción de medios](media-playback.md)


 