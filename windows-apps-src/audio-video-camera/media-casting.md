---
author: drewbatgit
ms.assetid: 40B97E0C-EB1B-40C2-A022-1AB95DFB085E
description: En este artículo se muestra cómo transmitir contenido multimedia a dispositivos remotos desde una aplicación universal de Windows.
title: Transmitir contenido multimedia
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: da0bb4d25166dd62372d5902ff89221d20189c22
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6974760"
---
# <a name="media-casting"></a>Transmitir contenido multimedia



En este artículo se muestra cómo transmitir contenido multimedia a dispositivos remotos desde una aplicación universal de Windows.

## <a name="built-in-media-casting-with-mediaplayerelement"></a>Transmisión de contenido multimedia integrada con MediaPlayerElement

La forma más sencilla de transmitir contenido multimedia desde una aplicación universal de Windows es usar la característica de transmisión integrada del control [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement).

Para permitir que el usuario abra un archivo de vídeo para reproducirlo en el control **MediaPlayerElement**, agrega los siguientes espacios de nombres al proyecto.

[!code-cs[BuiltInCastingUsing](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetBuiltInCastingUsing)]

En el archivo XAML de la aplicación, agrega una clase **MediaPlayerElement** y establece [**AreTransportControlsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298977) como true.

[!code-xml[MediaElement](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetMediaElement)]

Agrega un botón para permitir al usuario que inicie seleccionando un archivo.

[!code-xml[OpenButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetOpenButton)]

En el controlador de eventos [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) para el botón, crea una nueva instancia de la [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847), agrega tipos de archivo de vídeo a la colección [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850) y establece la ubicación inicial para la biblioteca de vídeos del usuario.

Llama a [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) para iniciar el cuadro de diálogo del selector de archivos. Cuando se devuelve este método, el resultado es un objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que representa el archivo de vídeo. Comprueba que el archivo no es null, que sí lo será si el usuario cancela la operación de selección. Llama al método [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227221.aspx) del archivo para obtener una interfaz [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731) para el archivo. Por último, crea un nuevo objeto **MediaSource** a partir del archivo seleccionado mediante una llamada a [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909) y asígnalo a la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.Source) del objeto **MediaPlayerElement** para que el archivo de vídeo sea el origen de vídeo para el control.

[!code-cs[OpenButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetOpenButtonClick)]

Una vez cargado el vídeo en **MediaPlayerElement**, el usuario simplemente puede presionar el botón de transmisión en los controles de transporte para iniciar un cuadro de diálogo integrado que le permite elegir un dispositivo al que se transmitirá el contenido multimedia cargado.

![botón de transmisión de mediaelement](images/media-element-casting-button.png)

> [!NOTE] 
> A partir de Windows10, versión 1607, se recomienda usar la clase **MediaPlayer** para reproducir elementos multimedia. **MediaPlayerElement** es un control XAML ligero que se usa para representar el contenido de un objeto **MediaPlayer** en una página XAML. El control **MediaElement** se sigue admitiendo para la compatibilidad con versiones anteriores. Para obtener más información sobre el uso de **MediaPlayer** y **MediaPlayerElement** para reproducir contenido multimedia, consulta [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md). Para obtener información sobre el uso de **MediaSource** y las API relacionadas para trabajar con contenido multimedia, consulta [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md).

## <a name="media-casting-with-the-castingdevicepicker"></a>Transmisión de contenido multimedia con el CastingDevicePicker

Una segunda forma de transmitir contenido multimedia a un dispositivo es usar la clase [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/dn972525). Para usar esta clase, se incluye el espacio de nombres de [**Windows.Media.Casting**](https://msdn.microsoft.com/library/windows/apps/dn972568) en el proyecto.

[!code-cs[CastingNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingNamespace)]

Declarar una variable de miembro para el objeto **CastingDevicePicker**.

[!code-cs[DeclareCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareCastingPicker)]

Cuando tu página se inicia, crea una nueva instancia del selector de conversión y establece el [**Filter**](https://msdn.microsoft.com/library/windows/apps/dn972540) a la propiedad [**SupportsVideo**](https://msdn.microsoft.com/library/windows/apps/dn972526) para indicar que los dispositivos de conversión enumerados por el selector deben admitir el vídeo. Registrar un controlador para el evento [**CastingDeviceSelected**](https://msdn.microsoft.com/library/windows/apps/dn972539), que se genera cuando el usuario elige un dispositivo de conversión.

[!code-cs[InitCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetInitCastingPicker)]

Agrega un botón en el archivo XAML para permitir al usuario iniciar el selector.

[!code-xml[CastPickerButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCastPickerButton)]

En el controlador de eventos **Click** para el botón, llama a [**TransformToVisual**](https://msdn.microsoft.com/library/windows/apps/br208986) para obtener la transformación de un elemento de interfaz de usuario con respecto a otro elemento. En este ejemplo, la transformación es la posición del botón Selector en relación a la raíz visual de la ventana de la aplicación. Llama al método [**Show**](https://msdn.microsoft.com/library/windows/apps/dn972542) del objeto [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/dn972525) para iniciar el cuadro de diálogo del selector de conversión. Especifica la ubicación y las dimensiones del botón Selector de conversión de tipos para que el sistema pueda hacer que el cuadro de diálogo salga del botón que el usuario ha presionado.

[!code-cs[CastPickerButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastPickerButtonClick)]

En el controlador de eventos **CastingDeviceSelected** llama al método [**CreateCastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972547) de la propiedad de los argumentos del evento [**SelectedCastingDevice**](https://msdn.microsoft.com/library/windows/apps/dn972546), que representa el dispositivo de conversión seleccionado por el usuario. Registrar controladores para los eventos [**ErrorOccurred**](https://msdn.microsoft.com/library/windows/apps/dn972519) y [**StateChanged**](https://msdn.microsoft.com/library/windows/apps/dn972523). Por último, llama a [**RequestStartCastingAsync**](https://msdn.microsoft.com/library/windows/apps/dn972520) para comenzar la transmisión al pasar el resultado al método [**GetAsCastingSource**](https://msdn.microsoft.com/library/windows/apps/dn920012) del objeto **MediaPlayer** del control **MediaPlayerElement** para especificar que el contenido multimedia que se va a transmitir es el contenido del objeto **MediaPlayer** asociado al control **MediaPlayerElement**.

> [!NOTE] 
> La conexión de la transmisión se debe iniciar en el subproceso de interfaz de usuario. Dado que no se llama a **CastingDeviceSelected** desde el subproceso de interfaz de usuario, debes realizar estas llamadas dentro de una llamada a [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317), lo que hace que se les llame en el subproceso de interfaz de usuario.

[!code-cs[CastingDeviceSelected](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingDeviceSelected)]

En los controladores de eventos **ErrorOccurred** y **StateChanged**, debes actualizar la interfaz de usuario para informar al usuario del estado actual de conversión. Estos eventos se explican a detalle en la siguiente sección sobre la creación de un selector de dispositivos de conversión personalizadas.

[!code-cs[EmptyStateHandlers](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEmptyStateHandlers)]

## <a name="media-casting-with-a-custom-device-picker"></a>Conversión de multimedia con un selector de dispositivos personalizados

La siguiente sección describe cómo crear tu propio selector de dispositivos de conversión interfaz de usuario enumerando los dispositivos de conversión e iniciando la conexión desde el código.

Para enumerar los dispositivos de conversión disponibles, incluye el espacio de nombres [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459) en el proyecto.

[!code-cs[EnumerationNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

Agrega los siguientes controles a la página XAML para implementar la interfaz de usuario rudimentaria para este ejemplo:

-   Un botón para iniciar el observador de dispositivo que busca dispositivos de conversión disponibles.
-   Un control [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) para proporcionar información al usuario de que la enumeración de conversión está en curso.
-   Una clase [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) para enumerar los dispositivos de conversión detectados. Definir una [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830) para el control, de modo que podamos asignar los objetos de dispositivo de conversión directamente al control y seguir mostrando la propiedad [**FriendlyName**](https://msdn.microsoft.com/library/windows/apps/dn972549) .
-   Un botón para permitir que el usuario desconecte el dispositivo de conversión.

[!code-xml[CustomPickerXAML](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCustomPickerXAML)]

En tu código subyacente, declara variables de miembro para la [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) y la [**CastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972510).

[!code-cs[DeclareDeviceWatcher](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatcher)]

En el controlador **Click** para el *startWatcherButton*, primero se actualiza la interfaz de usuario deshabilitando el botón y activando el círculo de progreso mientras la enumeración de dispositivos esté en curso. Desactiva la casilla de la lista de dispositivos de conversión.

A continuación, crea un observador de dispositivo mediante una llamada a [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427). Este método se puede usar para inspeccionar los diferentes tipos de dispositivos. Especifica lo que quieras observar en los dispositivos que admiten la conversión de vídeo mediante el uso de la cadena de selector de dispositivo devuelta por [**CastingDevice.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn972551).

Por último, registra controladores de eventos para los eventos [**Added**](https://msdn.microsoft.com/library/windows/apps/br225450), [**Removed**](https://msdn.microsoft.com/library/windows/apps/br225453), [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451) y [**Stopped**](https://msdn.microsoft.com/library/windows/apps/br225457).

[!code-cs[StartWatcherButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStartWatcherButtonClick)]

El evento **Added** se genera cuando el observador detecta un nuevo dispositivo. En el controlador para este evento, crea un nuevo objeto [**CastingDevice**](https://msdn.microsoft.com/library/windows/apps/dn972524) llamando a [**CastingDevice.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn972550) y pasando el identificador del dispositivo de conversión detectado, que está incluido en el objeto **DeviceInformation** pasado al controlador.

Agregar el **CastingDevice** al dispositivo de conversión **ListBox** para que el usuario pueda seleccionarlo. Debido a la propiedad [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830) definida en el XAML, la propiedad [**FriendlyName**](https://msdn.microsoft.com/library/windows/apps/dn972549) se usará como el texto del elemento en el cuadro de lista. Dado que no se llama a este controlador de eventos en el subproceso de interfaz de usuario, debes actualizar la interfaz de usuario desde una llamada a [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317).

[!code-cs[WatcherAdded](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherAdded)]

El evento **Removed** se genera cuando el observador detecta que un dispositivo de conversión ya no está presente. Compara la propiedad del identificador de objeto **Added** pasado al controlador para el identificador de cada **Added** en la colección [**Items**](https://msdn.microsoft.com/library/windows/apps/br242823) del cuadro de lista. Si coincide con el identificador, quita ese objeto de la colección. De nuevo, debido a que la interfaz de usuario se está actualizando, esta llamada debe realizarse desde dentro de una llamada de **RunAsync**.

[!code-cs[WatcherRemoved](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherRemoved)]

El evento **EnumerationCompleted** se genera cuando el observador finaliza la detección de dispositivos. En el controlador para este evento, actualiza la interfaz de usuario para permitir al usuario saber que se ha completado la enumeración de dispositivos y detener el observador de dispositivo mediante una llamada a [**Stop**](https://msdn.microsoft.com/library/windows/apps/br225456).

[!code-cs[WatcherEnumerationCompleted](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherEnumerationCompleted)]

Cuando el observador de dispositivo acaba de detenerse, se genera el evento Stopped. En el controlador para este evento, detén el control [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) y vuelve a habilitar el *startWatcherButton* para que el usuario pueda reiniciar el proceso de enumeración de dispositivos.

[!code-cs[WatcherStopped](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherStopped)]

Cuando el usuario selecciona uno de los dispositivos de conversión en el cuadro de lista, se genera el evento [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776). Es dentro de este controlador que se creará la conexión de conversión y se iniciará la conversión.

Primero, asegúrate de que el observador de dispositivo se detenga para que la enumeración de dispositivos no interfiera con la conversión de multimedia. Crea una conexión de conversión llamando a [**CreateCastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972547) en el objeto **CastingDevice** seleccionado por el usuario. Añade los controladores de eventos para los eventos [**StateChanged**](https://msdn.microsoft.com/library/windows/apps/dn972523) y [**ErrorOccurred**](https://msdn.microsoft.com/library/windows/apps/dn972519) .

Inicia la transmisión de contenido multimedia con una llamada a [**RequestStartCastingAsync**](https://msdn.microsoft.com/library/windows/apps/dn972520), al pasar el origen de transmisión devuelto mediante una llamada al método [**GetAsCastingSource**](https://msdn.microsoft.com/library/windows/apps/dn920012) de **MediaPlayer**. Por último, haz que el botón de desconexión esté visible para permitir al usuario detener la transmisión multimedia.

[!code-cs[SelectionChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetSelectionChanged)]

En el controlador de cambio de estado, la acción que realices depende el nuevo estado de la conexión de conversión:

-   Si el estado es **Connected** o **Rendering**, asegúrate de que el control **ProgressRing** esté inactivo y de que el botón desconectar esté visible.
-   Si el estado es **Disconnected**, desactiva el dispositivo de conversión actual en el cuadro de lista, desactiva el control **ProgressRing** y oculta el botón de desconexión.
-   Si el estado es **Connecting**, activa el control de **ProgressRing** y oculta el botón desconectar.
-   Si el estado es **Disconnecting**, activa el control de **ProgressRing** y oculta el botón desconectar.

[!code-cs[StateChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStateChanged)]

En el controlador para el evento **ErrorOccurred**, actualiza la interfaz de usuario para hacer saber al usuario que se ha producido un error de conversión y que anule la selección del objeto **CastingDevice** actual en el cuadro de lista.

[!code-cs[ErrorOccurred](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetErrorOccurred)]

Por último, implementa el controlador para el botón Desconectar. Detén la conversión de tipos de medios y desconecta el dispositivo de conversión mediante una llamada al método [**DisconnectAsync**](https://msdn.microsoft.com/library/windows/apps/dn972518) del objeto **CastingConnection**. Esta llamada se debe enviar al subproceso de interfaz de usuario mediante una llamada a [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317).

[!code-cs[DisconnectButton](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDisconnectButton)]

 

 




