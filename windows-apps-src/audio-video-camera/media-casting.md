---
ms.assetid: 40B97E0C-EB1B-40C2-A022-1AB95DFB085E
description: En este artículo te mostramos cómo transmitir contenido multimedia a dispositivos remotos desde una aplicación universal de Windows.
title: Transmitir contenido multimedia
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a23e6c58325a8679a8df2ec0d3f429f75a230602
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363928"
---
# <a name="media-casting"></a>Transmitir contenido multimedia



En este artículo te mostramos cómo transmitir contenido multimedia a dispositivos remotos desde una aplicación universal de Windows.

## <a name="built-in-media-casting-with-mediaplayerelement"></a>Transmisión de contenido multimedia integrada con MediaPlayerElement

La forma más sencilla de transmitir contenido multimedia desde una aplicación universal de Windows es usar la característica de transmisión integrada del control [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement).

Para permitir que el usuario abra un archivo de vídeo para reproducirlo en el control **MediaPlayerElement**, agrega los siguientes espacios de nombres al proyecto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetBuiltInCastingUsing":::

En el archivo XAML de la aplicación, agrega una clase **MediaPlayerElement** y establece [**AreTransportControlsEnabled**](/uwp/api/windows.ui.xaml.controls.mediaelement.aretransportcontrolsenabled) como true.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetMediaElement":::

Agrega un botón para permitir al usuario que inicie seleccionando un archivo.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetOpenButton":::

En el controlador de eventos [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) para el botón, crea una nueva instancia de la [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker), agrega tipos de archivo de vídeo a la colección [**FileTypeFilter**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) y establece la ubicación inicial para la biblioteca de vídeos del usuario.

Llama a [**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) para iniciar el cuadro de diálogo del selector de archivos. Cuando se devuelve este método, el resultado es un objeto [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) que representa el archivo de vídeo. Comprueba que el archivo no es null, que sí lo será si el usuario cancela la operación de selección. Llama al método [**OpenAsync**](/uwp/api/windows.storage.storagefile.openasync) del archivo para obtener una interfaz [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) para el archivo. Por último, crea un nuevo objeto **MediaSource** a partir del archivo seleccionado mediante una llamada a [**CreateFromStorageFile**](/uwp/api/windows.media.core.mediasource.createfromstoragefile) y asígnalo a la propiedad [**Source**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) del objeto **MediaPlayerElement** para que el archivo de vídeo sea el origen de vídeo para el control.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetOpenButtonClick":::

Una vez cargado el vídeo en **MediaPlayerElement**, el usuario simplemente puede presionar el botón de transmisión en los controles de transporte para iniciar un cuadro de diálogo integrado que le permite elegir un dispositivo al que se transmitirá el contenido multimedia cargado.

![botón de transmisión de mediaelement](images/media-element-casting-button.png)

> [!NOTE] 
> A partir de Windows 10, versión 1607, se recomienda usar la clase **MediaPlayer** para reproducir elementos multimedia. **MediaPlayerElement** es un control XAML ligero que se usa para representar el contenido de un objeto **MediaPlayer** en una página XAML. El control **MediaElement** se sigue admitiendo para la compatibilidad con versiones anteriores. Para obtener más información sobre el uso de **MediaPlayer** y **MediaPlayerElement** para reproducir contenido multimedia, consulta [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md). Para obtener información sobre el uso de **MediaSource** y las API relacionadas para trabajar con contenido multimedia, consulta [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md).

## <a name="media-casting-with-the-castingdevicepicker"></a>Transmisión de contenido multimedia con el CastingDevicePicker

Una segunda forma de transmitir contenido multimedia a un dispositivo es usar la clase [**CastingDevicePicker**](/uwp/api/Windows.Media.Casting.CastingDevicePicker). Para usar esta clase, se incluye el espacio de nombres de [**Windows.Media.Casting**](/uwp/api/Windows.Media.Casting) en el proyecto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetCastingNamespace":::

Declarar una variable de miembro para el objeto **CastingDevicePicker**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareCastingPicker":::

Cuando tu página se inicia, crea una nueva instancia del selector de conversión y establece el [**Filter**](/uwp/api/windows.media.casting.castingdevicepicker.filter) a la propiedad [**SupportsVideo**](/uwp/api/Windows.Media.Casting.CastingDevicePickerFilter) para indicar que los dispositivos de conversión enumerados por el selector deben admitir el vídeo. Registrar un controlador para el evento [**CastingDeviceSelected**](/uwp/api/windows.media.casting.castingdevicepicker.castingdeviceselected), que se genera cuando el usuario elige un dispositivo de conversión.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetInitCastingPicker":::

Agrega un botón en el archivo XAML para permitir al usuario iniciar el selector.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetCastPickerButton":::

En el controlador de eventos **Click** para el botón, llama a [**TransformToVisual**](/uwp/api/windows.ui.xaml.uielement.transformtovisual) para obtener la transformación de un elemento de interfaz de usuario con respecto a otro elemento. En este ejemplo, la transformación es la posición del botón Selector en relación a la raíz visual de la ventana de la aplicación. Llama al método [**Show**](/uwp/api/windows.media.casting.castingdevicepicker.show) del objeto [**CastingDevicePicker**](/uwp/api/Windows.Media.Casting.CastingDevicePicker) para iniciar el cuadro de diálogo del selector de conversión. Especifica la ubicación y las dimensiones del botón Selector de conversión de tipos para que el sistema pueda hacer que el cuadro de diálogo salga del botón que el usuario ha presionado.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetCastPickerButtonClick":::

En el controlador de eventos **CastingDeviceSelected** llama al método [**CreateCastingConnection**](/uwp/api/windows.media.casting.castingdevice.createcastingconnection) de la propiedad de los argumentos del evento [**SelectedCastingDevice**](/uwp/api/windows.media.casting.castingdeviceselectedeventargs.selectedcastingdevice), que representa el dispositivo de conversión seleccionado por el usuario. Registrar controladores para los eventos [**ErrorOccurred**](/uwp/api/windows.media.casting.castingconnection.erroroccurred) y [**StateChanged**](/uwp/api/windows.media.casting.castingconnection.statechanged). Por último, llama a [**RequestStartCastingAsync**](/uwp/api/windows.media.casting.castingconnection.requeststartcastingasync) para comenzar la transmisión al pasar el resultado al método [**GetAsCastingSource**](/uwp/api/windows.ui.xaml.controls.mediaelement.getascastingsource) del objeto **MediaPlayer** del control **MediaPlayerElement** para especificar que el contenido multimedia que se va a transmitir es el contenido del objeto **MediaPlayer** asociado al control **MediaPlayerElement**.

> [!NOTE] 
> La conexión de la transmisión se debe iniciar en el subproceso de interfaz de usuario. Dado que no se llama a **CastingDeviceSelected** desde el subproceso de interfaz de usuario, debes realizar estas llamadas dentro de una llamada a [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync), lo que hace que se les llame en el subproceso de interfaz de usuario.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetCastingDeviceSelected":::

En los controladores de eventos **ErrorOccurred** y **StateChanged**, debes actualizar la interfaz de usuario para informar al usuario del estado actual de conversión. Estos eventos se explican a detalle en la siguiente sección sobre la creación de un selector de dispositivos de conversión personalizadas.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetEmptyStateHandlers":::

## <a name="media-casting-with-a-custom-device-picker"></a>Conversión de multimedia con un selector de dispositivos personalizados

La siguiente sección describe cómo crear tu propio selector de dispositivos de conversión interfaz de usuario enumerando los dispositivos de conversión e iniciando la conexión desde el código.

Para enumerar los dispositivos de conversión disponibles, incluye el espacio de nombres [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration) en el proyecto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetEnumerationNamespace":::

Agrega los siguientes controles a la página XAML para implementar la interfaz de usuario rudimentaria para este ejemplo:

-   Un botón para iniciar el observador de dispositivo que busca dispositivos de conversión disponibles.
-   Un control [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) para proporcionar información al usuario de que la enumeración de conversión está en curso.
-   Una clase [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) para enumerar los dispositivos de conversión detectados. Definir una [**ItemTemplate**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) para el control, de modo que podamos asignar los objetos de dispositivo de conversión directamente al control y seguir mostrando la propiedad [**FriendlyName**](/uwp/api/windows.media.casting.castingdevice.friendlyname) .
-   Un botón para permitir que el usuario desconecte el dispositivo de conversión.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetCustomPickerXAML":::

En tu código subyacente, declara variables de miembro para la [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) y la [**CastingConnection**](/uwp/api/Windows.Media.Casting.CastingConnection).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareDeviceWatcher":::

En el controlador **Click** para el *startWatcherButton*, primero se actualiza la interfaz de usuario deshabilitando el botón y activando el círculo de progreso mientras la enumeración de dispositivos esté en curso. Desactiva la casilla de la lista de dispositivos de conversión.

A continuación, crea un observador de dispositivo mediante una llamada a [**DeviceInformation.CreateWatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher). Este método se puede usar para inspeccionar los diferentes tipos de dispositivos. Especifica lo que quieras observar en los dispositivos que admiten la conversión de vídeo mediante el uso de la cadena de selector de dispositivo devuelta por [**CastingDevice.GetDeviceSelector**](/uwp/api/windows.media.casting.castingdevice.getdeviceselector).

Por último, registra controladores de eventos para los eventos [**Added**](/uwp/api/windows.devices.enumeration.devicewatcher.added), [**Removed**](/uwp/api/windows.devices.enumeration.devicewatcher.removed), [**EnumerationCompleted**](/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) y [**Stopped**](/uwp/api/windows.devices.enumeration.devicewatcher.stopped).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetStartWatcherButtonClick":::

El evento **Added** se genera cuando el observador detecta un nuevo dispositivo. En el controlador para este evento, crea un nuevo objeto [**CastingDevice**](/uwp/api/Windows.Media.Casting.CastingDevice) llamando a [**CastingDevice.FromIdAsync**](/uwp/api/windows.media.casting.castingdevice.fromidasync) y pasando el identificador del dispositivo de conversión detectado, que está incluido en el objeto **DeviceInformation** pasado al controlador.

Agregar el **CastingDevice** al dispositivo de conversión **ListBox** para que el usuario pueda seleccionarlo. Debido a la propiedad [**ItemTemplate**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) definida en el XAML, la propiedad [**FriendlyName**](/uwp/api/windows.media.casting.castingdevice.friendlyname) se usará como el texto del elemento en el cuadro de lista. Dado que no se llama a este controlador de eventos en el subproceso de interfaz de usuario, debes actualizar la interfaz de usuario desde una llamada a [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherAdded":::

El evento **Removed** se genera cuando el observador detecta que un dispositivo de conversión ya no está presente. Compara la propiedad del identificador de objeto **Added** pasado al controlador para el identificador de cada **Added** en la colección [**Items**](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) del cuadro de lista. Si coincide con el identificador, quita ese objeto de la colección. De nuevo, debido a que la interfaz de usuario se está actualizando, esta llamada debe realizarse desde dentro de una llamada de **RunAsync**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherRemoved":::

El evento **EnumerationCompleted** se genera cuando el observador finaliza la detección de dispositivos. En el controlador para este evento, actualiza la interfaz de usuario para permitir al usuario saber que se ha completado la enumeración de dispositivos y detener el observador de dispositivo mediante una llamada a [**Stop**](/uwp/api/windows.devices.enumeration.devicewatcher.stop).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherEnumerationCompleted":::

Cuando el observador de dispositivo acaba de detenerse, se genera el evento Stopped. En el controlador para este evento, detén el control [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) y vuelve a habilitar el *startWatcherButton* para que el usuario pueda reiniciar el proceso de enumeración de dispositivos.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherStopped":::

Cuando el usuario selecciona uno de los dispositivos de conversión en el cuadro de lista, se genera el evento [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged). Es dentro de este controlador que se creará la conexión de conversión y se iniciará la conversión.

Primero, asegúrate de que el observador de dispositivo se detenga para que la enumeración de dispositivos no interfiera con la conversión de multimedia. Crea una conexión de conversión llamando a [**CreateCastingConnection**](/uwp/api/windows.media.casting.castingdevice.createcastingconnection) en el objeto **CastingDevice** seleccionado por el usuario. Añade los controladores de eventos para los eventos [**StateChanged**](/uwp/api/windows.media.casting.castingconnection.statechanged) y [**ErrorOccurred**](/uwp/api/windows.media.casting.castingconnection.erroroccurred) .

Inicia la transmisión de contenido multimedia con una llamada a [**RequestStartCastingAsync**](/uwp/api/windows.media.casting.castingconnection.requeststartcastingasync), al pasar el origen de transmisión devuelto mediante una llamada al método [**GetAsCastingSource**](/uwp/api/windows.ui.xaml.controls.mediaelement.getascastingsource) de **MediaPlayer**. Por último, haz que el botón de desconexión esté visible para permitir al usuario detener la transmisión multimedia.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetSelectionChanged":::

En el controlador de cambio de estado, la acción que realices depende el nuevo estado de la conexión de conversión:

-   Si el estado es **Connected** o **Rendering**, asegúrate de que el control **ProgressRing** esté inactivo y de que el botón desconectar esté visible.
-   Si el estado es **Disconnected**, desactiva el dispositivo de conversión actual en el cuadro de lista, desactiva el control **ProgressRing** y oculta el botón de desconexión.
-   Si el estado es **Connecting**, activa el control de **ProgressRing** y oculta el botón desconectar.
-   Si el estado es **Disconnecting**, activa el control de **ProgressRing** y oculta el botón desconectar.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetStateChanged":::

En el controlador para el evento **ErrorOccurred**, actualiza la interfaz de usuario para hacer saber al usuario que se ha producido un error de conversión y que anule la selección del objeto **CastingDevice** actual en el cuadro de lista.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetErrorOccurred":::

Por último, implementa el controlador para el botón Desconectar. Detén la conversión de tipos de medios y desconecta el dispositivo de conversión mediante una llamada al método [**DisconnectAsync**](/uwp/api/windows.media.casting.castingconnection.disconnectasync) del objeto **CastingConnection**. Esta llamada se debe enviar al subproceso de interfaz de usuario mediante una llamada a [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetDisconnectButton":::

 

 
