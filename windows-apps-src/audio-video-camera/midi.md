---
ms.assetid: 9146212C-8480-4C16-B74C-D7F08C7086AF
description: En este artículo se muestra cómo enumerar dispositivos MIDI (interfaz digital de instrumentos musicales), y enviar y recibir mensajes MIDI desde una aplicación universal de Windows.
title: MIDI
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 73806735401f53a73b1051f37c72119b45b574be
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318288"
---
# <a name="midi"></a>MIDI



En este artículo se muestra cómo enumerar dispositivos MIDI (interfaz digital de instrumentos musicales), y enviar y recibir mensajes MIDI desde una aplicación universal de Windows. Windows 10 es compatible con MIDI a través de USB (compatible con la clase y propiedad más controladores), MIDI a través de Bluetooth LE (Windows 10 Anniversary Edition y versiones posteriores) y los productos de terceros disponible de forma gratuita, MIDI a través de Ethernet y enrutado MIDI.

## <a name="enumerate-midi-devices"></a>Enumerar dispositivos MIDI

Antes de enumerar y usar dispositivos MIDI, agrega los siguientes espacios de nombres al proyecto.

[!code-cs[Using](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetUsing)]

Agrega un control [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) a la página XAML que permitirá que el usuario seleccione uno de los dispositivos de entrada de MIDI conectados al sistema. Agrega otro para enumerar los dispositivos de salida de MIDI.

[!code-xml[MidiListBoxes](./code/MIDIWin10/cs/MainPage.xaml#SnippetMidiListBoxes)]

El método [**FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) de clase [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) se usa para enumerar los diferentes tipos de dispositivos reconocidos por Windows. Para especificar que solo deseas el método para buscar dispositivos de entrada de MIDI, usa la cadena de selector devuelta por [**MidiInPort.GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midiinport.getdeviceselector). **FindAllAsync** devuelve una [**DeviceInformationCollection**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) que contiene una **DeviceInformation** para cada dispositivo de entrada MIDI registrado con el sistema. Si la colección devuelta no contiene ningún elemento, entonces no hay ningún dispositivo de entrada MIDI disponible. Si hay elementos en la colección, revisa los objetos **DeviceInformation** y agrega el nombre de cada dispositivo para el dispositivo de entrada MIDI **ListBox**.

[!code-cs[EnumerateMidiInputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiInputDevices)]

Enumerar dispositivos de salida MIDI funciona exactamente igual que la enumeración de dispositivos de entrada, excepto que se debe especificar la cadena de selector devuelta por [**MidiOutPort.GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midioutport.getdeviceselector) al llamar a **FindAllAsync**.

[!code-cs[EnumerateMidiOutputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiOutputDevices)]



## <a name="create-a-device-watcher-helper-class"></a>Crear una clase auxiliar de monitor de dispositivo

El espacio de nombres [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) proporciona la clase [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) que puede notificar a la aplicación si se agregan o eliminan dispositivos del sistema, o si se actualiza la información de un dispositivo. Dado que las aplicaciones MIDI habilitadas normalmente están interesadas en dispositivos de entrada y salida, este ejemplo crea una clase auxiliar que implementa el patrón **DeviceWatcher**, por lo que puede usarse el mismo código para ambos dispositivos de entrada MIDI y salida MIDI, sin necesidad de duplicación.

Agregar una nueva clase al proyecto para que actúe como el observador de dispositivo. En este ejemplo, la clase se llama **MyMidiDeviceWatcher**. El resto del código de esta sección se usa para implementar la clase auxiliar.

Agrega estas variables de miembro a la clase:

-   Un objeto [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) que va a supervisar los cambios de dispositivo.
-   Una cadena de selector de dispositivos que contendrá el MIDI en la cadena de selector de puerto para una instancia y la cadena de selector de puerto de salida MIDI para otra instancia.
-   Un control [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) que se rellene con los nombres de los dispositivos disponibles.
-   Una clase [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher), necesaria para actualizar la interfaz de usuario desde un subproceso distinto al subproceso de interfaz de usuario.

[!code-cs[WatcherVariables](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherVariables)]

Agrega una propiedad [**DeviceInformationCollection**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) que se use para acceder a la lista actual de dispositivos desde fuera de la clase auxiliar.

[!code-cs[DeclareDeviceInformationCollection](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetDeclareDeviceInformationCollection)]

En el constructor de clase, el llamador pasa en la cadena de selector de dispositivos MIDI, el objeto **ListBox** para enumerar los dispositivos y el objeto **Dispatcher** necesarios para actualizar la interfaz de usuario.

Llama a [**DeviceInformation.CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) para crear una nueva instancia de la clase **DeviceWatcher**, pasando la cadena de selector de dispositivos MIDI.

Registra controladores para los controladores de eventos del monitor.

[!code-cs[WatcherConstructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherConstructor)]

La clase **DeviceWatcher** tiene los eventos siguientes:

-   [**Agregar** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) -se genera cuando se agrega un nuevo dispositivo en el sistema.
-   [**Quitar** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed) -se genera cuando se quita un dispositivo desde el sistema.
-   [**Actualizar** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) -se genera cuando se actualiza la información asociada con un dispositivo existente.
-   [**EnumerationCompleted** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) -se genera cuando el monitor ha completado su enumeración del tipo solicitado del dispositivo.

En el controlador de eventos para cada uno de estos eventos, se llama a un método auxiliar, **UpdateDevices**, para actualizar la **ListBox** con la lista actual de dispositivos. Dado que no se llama a estos controladores de eventos y a los elementos de interfaz de usuario de las actualizaciones **UpdateDevices** en el subproceso de interfaz de usuario, cada llamada debe estar encapsulada en una llamada a [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync), lo que hace que el código se ejecute en el subproceso de interfaz de usuario especificado.

[!code-cs[WatcherEventHandlers](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherEventHandlers)]

El método auxiliar **UpdateDevices** llama a [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) y actualiza el objeto **ListBox** con los nombres de los dispositivos devueltos, tal como se describió anteriormente en este artículo.

[!code-cs[WatcherUpdateDevices](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherUpdateDevices)]

Agrega métodos para iniciar el monitor mediante el método [**Start**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start) del objeto **DeviceWatcher**, y detenerlo mediante el método [**Stop**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop).

[!code-cs[WatcherStopStart](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherStopStart)]

Proporciona un destructor para anular el registro de los controladores de eventos de monitor y establecer el monitor de dispositivo en null.

[!code-cs[WatcherDestructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherDestructor)]

## <a name="create-midi-ports-to-send-and-receive-messages"></a>Crear los puertos MIDI para enviar y recibir mensajes

En el código subyacente de la página, debes declarar variables de miembros para almacenar dos instancias de la clase auxiliar **MyMidiDeviceWatcher**: una para dispositivos de entrada y otra para dispositivos de salida.

[!code-cs[DeclareDeviceWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatchers)]

Crea una nueva instancia de las clases auxiliares del monitor, pasando la cadena de selector de dispositivo, la **ListBox** que se va a rellenar y el objeto **CoreDispatcher** al que se pueda acceder a través de la propiedad **Dispatcher** de la página. Después, llama al método para iniciar cada objeto **DeviceWatcher**.

Poco después de que se inicie cada objeto **DeviceWatcher**, finalizará la enumeración de los dispositivos actuales conectados al sistema y se generará su evento [**EnumerationCompleted**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted), lo que provocará que cada elemento **ListBox** se actualice con los dispositivos MIDI actuales.

[!code-cs[StartWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetStartWatchers)]

Cuando el usuario selecciona un elemento en la entrada MIDI **ListBox**, se genera el evento [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged). En el controlador para este evento, obtén acceso a la propiedad **DeviceInformationCollection** de la clase auxiliar para obtener la lista actual de dispositivos. Si hay entradas en la lista, selecciona el objeto **DeviceInformation** con el índice correspondiente a la [**SelectedIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) del control **ListBox**.

A continuación, crea el objeto [**MidiInPort**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiInPort) que representa el dispositivo de entrada seleccionado, llamando a [**MidiInPort.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midiinport.fromidasync) y pasando la propiedad [**Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) del dispositivo seleccionado.

Registra un controlador para el evento [**MessageReceived**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midiinport.messagereceived), el cual se genera siempre que recibas un mensaje de MIDI mediante el dispositivo especificado.

[!code-cs[DeclareMidiPorts](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareMidiPorts)]

[!code-cs[InPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetInPortSelectionChanged)]

Cuando se llama al controlador **MessageReceived**, el mensaje se guarda en la propiedad [**Message**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiMessageReceivedEventArgs) de la clase **MidiMessageReceivedEventArgs**. La propiedad [**Type**](https://docs.microsoft.com/uwp/api/windows.devices.midi.imidimessage.type) del mensaje objeto es un valor de la enumeración [**MidiMessageType**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiMessageType) que indica el tipo de mensaje que recibiste. Los datos del mensaje dependen del tipo de mensaje. En este ejemplo, se comprueba si el mensaje es una nota en el mensaje y, si es así, se envía el canal midi, la nota y la velocidad del mensaje.

[!code-cs[MessageReceived](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetMessageReceived)]

El controlador [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) del dispositivo de salida **ListBox** funciona igual que el controlador de dispositivos de entrada, salvo que no se registra ningún controlador de eventos.

[!code-cs[OutPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetOutPortSelectionChanged)]

Una vez creado el dispositivo de salida, puedes enviar un mensaje creando la nueva interfaz [**IMidiMessage**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.IMidiMessage) para el tipo de mensaje que quieras enviar. En este ejemplo, en mensaje es una clase [**NoteOnMessage**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiNoteOnMessage). El método [**SendMessage**](https://docs.microsoft.com/uwp/api/windows.devices.midi.imidioutport.sendmessage) del objeto [**IMidiOutPort**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.IMidiOutPort) se llama para enviar el mensaje.

[!code-cs[SendMessage](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetSendMessage)]

Cuando la aplicación se desactiva, asegúrate de limpiar los recursos de las aplicaciones. Anula el registro de los controladores de eventos y establece el MIDI del puerto y los objetos externos al puerto en "null". Detén los monitores de dispositivo y establécelos en "null".

[!code-cs[CleanUp](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetCleanUp)]

## <a name="using-the-built-in-windows-general-midi-synth"></a>Usar el sintetizador de MIDI general integrado en Windows

Cuando enumeres dispositivos MIDI de salida mediante la técnica descrita anteriormente, la aplicación detectará un dispositivo de MIDI denominado "Sintetizador por tabla de ondas GS de Microsoft". Este es un sintetizador MIDI general integrado que puedes reproducir desde la aplicación. Sin embargo, al intentar crear un puerto de salida MIDI en este dispositivo se producirá un error a menos que hayas incluido la extensión SDK para el sintetizador integrado en el proyecto.

**Para incluir la extensión de SDK de sintetizador MIDI General en el proyecto de aplicación**

1.  En el **Explorador de soluciones**, justo en tu proyecto, haz clic con el botón secundario en **Referencias** y selecciona **Agregar referencia...**
2.  Expande el nodo **Windows universal**.
3.  Selecciona **Extensiones**.
4.  En la lista de extensiones, selecciona **MIDI DLS general de Microsoft para aplicaciones universales de Windows**.
    > [!NOTE] 
    > Si existen varias versiones de la extensión, asegúrate de seleccionar la versión que coincida con el destino de la aplicación. Puedes ver a qué versión del SDK está orientada la aplicación en la pestaña **Aplicación** de las propiedades del proyecto.

 

 




