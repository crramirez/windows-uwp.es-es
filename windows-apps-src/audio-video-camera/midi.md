---
author: drewbatgit
ms.assetid: 9146212C-8480-4C16-B74C-D7F08C7086AF
description: En este artículo se muestra cómo enumerar dispositivos MIDI (interfaz digital de instrumentos musicales), y enviar y recibir mensajes MIDI desde una aplicación universal de Windows.
title: MIDI
---

# MIDI

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En este artículo se muestra cómo enumerar dispositivos MIDI (interfaz Digital de instrumentos musicales) y enviar y recibir mensajes MIDI desde una aplicación universal de Windows.

## Enumerar dispositivos MIDI

Antes de enumerar y usar dispositivos MIDI, agrega los siguientes espacios de nombres al proyecto.

[!code-cs[Uso](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetUsing)]

Agrega un control [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) a la página XAML que permitirá que el usuario seleccione uno de los dispositivos de entrada de MIDI conectados al sistema. Agrega otro para enumerar los dispositivos de salida de MIDI.

[!code-xml[MidiListBoxes](./code/MIDIWin10/cs/MainPage.xaml#SnippetMidiListBoxes)]

El método [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) de clase [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) se usa para enumerar los diferentes tipos de dispositivos reconocidos por Windows. Para especificar que sólo quieres el método para buscar dispositivos de entrada de MIDI, usa la cadena de selector devuelta por [**MidiInPort.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894779). **FindAllAsync** devuelve una clase [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) que contiene la clase **DeviceInformation** para cada dispositivo de entrada MIDI registrado con el sistema. Si la colección devuelta no contiene ningún elemento, entonces no hay ningún dispositivo de entrada MIDI disponible. Si hay elementos en la colección, repite el proceso con los objetos **DeviceInformation** y agrega el nombre de cada dispositivo al dispositivo de entrada MIDI **ListBox**. EnumerateMidiInputDevices

[!code-cs[Si enumeras los dispositivos MIDI de salida, verás que podrás hacerlo de la misma forma que al enumerar los dispositivos de entrada, excepto que se debe especificar la cadena de selector que devuelve el método [**MidiOutPort.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894845) al llamar a **FindAllAsync**.](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiInputDevices)]

EnumerateMidiOutputDevices

[!code-cs[Crear una clase auxiliar de monitor de dispositivo](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiOutputDevices)]

## El espacio de nombres [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459) proporciona la clase [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) que puede notificar a la aplicación si se agregan o eliminan dispositivos del sistema, o si se actualiza la información de un dispositivo.

Dado que las aplicaciones MIDI habilitadas normalmente están interesadas en dispositivos de entrada y salida, este ejemplo crea una clase auxiliar que implementa el patrón **DeviceWatcher**, por lo que puede usarse el mismo código para ambos dispositivos de entrada MIDI y salida MIDI, sin necesidad de duplicación. Agregar una nueva clase al proyecto para que actúe como el observador de dispositivo.

En este ejemplo, la clase se llama **MyMidiDeviceWatcher**. El resto del código de esta sección se usa para implementar la clase auxiliar. Agrega estas variables de miembro a la clase:

Un objeto [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) que va a supervisar los cambios de dispositivo.

-   Una cadena de selector de dispositivos que contendrá el MIDI en la cadena de selector de puerto para una instancia y la cadena de selector de puerto de salida MIDI para otra instancia.
-   Un control [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) que se rellene con los nombres de los dispositivos disponibles.
-   Una clase [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211), necesaria para actualizar la interfaz de usuario desde un subproceso distinto al subproceso de interfaz de usuario.
-   WatcherVariables

[!code-cs[Agrega una propiedad [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) que se use para acceder a la lista actual de dispositivos desde fuera de la clase auxiliar.](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherVariables)]

DeclareDeviceInformationCollection

[!code-cs[En el constructor de clase, el llamador pasa en la cadena de selector de dispositivos MIDI, el objeto **ListBox** para enumerar los dispositivos y el objeto **Dispatcher** necesarios para actualizar la interfaz de usuario.](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetDeclareDeviceInformationCollection)]

Llama a [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427) para crear una nueva instancia de la clase **DeviceWatcher**, pasando la cadena de selector de dispositivos MIDI.

Registra controladores para los controladores de eventos del monitor.

WatcherConstructor

[!code-cs[La clase **DeviceWatcher** tiene los eventos siguientes:](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherConstructor)]

[
              **Added**
            ](https://msdn.microsoft.com/library/windows/apps/br225450): se genera cuando se agrega un nuevo dispositivo al sistema.

-   [
              **Removed**
            ](https://msdn.microsoft.com/library/windows/apps/br225453): se genera cuando un dispositivo se quita del sistema.
-   [
              **Updated**
            ](https://msdn.microsoft.com/library/windows/apps/br225458): se genera cuando se actualiza la información asociada a un dispositivo existente.
-   [
              **EnumerationCompleted**
            ](https://msdn.microsoft.com/library/windows/apps/br225451): se genera cuando el monitor completa la enumeración del tipo de dispositivo solicitado.
-   En el controlador de eventos para cada uno de estos eventos, se llama a un método auxiliar, **UpdateDevices**, para actualizar la **ListBox** con la lista actual de dispositivos.

Dado que no se llama a estos controladores de eventos y a los elementos de interfaz de usuario de las actualizaciones **UpdateDevices** en el subproceso de interfaz de usuario, cada llamada debe estar encapsulada en una llamada a [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317), lo que hace que el código se ejecute en el subproceso de interfaz de usuario especificado. WatcherEventHandlers

[!code-cs[El método auxiliar **UpdateDevices** llama a [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) y actualiza el objeto **ListBox** con los nombres de los dispositivos devueltos, tal como se describió anteriormente en este artículo.](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherEventHandlers)]

WatcherUpdateDevices

[!code-cs[Agrega métodos para iniciar el monitor mediante el método [**Start**](https://msdn.microsoft.com/library/windows/apps/br225454) del objeto **DeviceWatcher**, y detenerlo mediante el método [**Stop**](https://msdn.microsoft.com/library/windows/apps/br225456).](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherUpdateDevices)]

WatcherStopStart

[!code-cs[Proporciona un destructor para anular el registro de los controladores de eventos de monitor y establecer el monitor de dispositivo en null.](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherStopStart)]

WatcherDestructor

[!code-cs[Crear los puertos MIDI para enviar y recibir mensajes](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherDestructor)]

## En el código subyacente de la página, debes declarar variables de miembros para almacenar dos instancias de la clase auxiliar **MyMidiDeviceWatcher**: una para dispositivos de entrada y otra para dispositivos de salida.

DeclareDeviceWatchers

[!code-cs[Crea una nueva instancia de las clases auxiliares del monitor, pasando la cadena de selector de dispositivo, la **ListBox** que se va a rellenar y el objeto **CoreDispatcher** al que se pueda acceder a través de la propiedad **Dispatcher** de la página.](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatchers)]

A continuación, llama al método para iniciar cada objeto **DeviceWatcher**. Poco después de que cada objeto **DeviceWatcher** se inicie, finalizará la enumeración de los dispositivos que estén conectados al sistema y se generará su evento [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451); esto hace que cada elemento **ListBox** se actualice con los dispositivos MIDI actuales.

StartWatchers

[!code-cs[Poco después de que cada objeto **DeviceWatcher** se inicie, finalizará la enumeración de los dispositivos que estén conectados al sistema y se generará su evento [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451); esto hace que cada elemento **ListBox** se actualice con los dispositivos MIDI actuales.](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetStartWatchers)]

Cuando el usuario selecciona un elemento en la entrada MIDI **ListBox**, se genera el evento [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776).

En el controlador para este evento, obtén acceso a la propiedad **DeviceInformationCollection** de la clase auxiliar para obtener la lista actual de dispositivos. Si hay entradas en la lista, selecciona el objeto **DeviceInformation** con el índice correspondiente a la propiedad [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/br209768) del control **ListBox**. A continuación, crea el objeto [**MidiInPort**](https://msdn.microsoft.com/library/windows/apps/dn894770) que representa el dispositivo de entrada seleccionado, llamando a [**MidiInPort.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn894776) y pasando la propiedad [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) del dispositivo seleccionado.

Registra un controlador para el evento [**MessageReceived**](https://msdn.microsoft.com/library/windows/apps/dn894781), el cual se genera siempre que recibas un mensaje de MIDI mediante el dispositivo especificado.

InPortSelectionChanged

[!code-cs[Cuando se llama al controlador **MessageReceived**, el mensaje se guarda en la propiedad [**Message**](https://msdn.microsoft.com/library/windows/apps/dn894783) de la clase **MidiMessageReceivedEventArgs**.](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetInPortSelectionChanged)]

La propiedad [**Type**](https://msdn.microsoft.com/library/windows/apps/dn894726) del mensaje objeto es un valor de la enumeración [**MidiMessageType**](https://msdn.microsoft.com/library/windows/apps/dn894786) que indica el tipo de mensaje que recibiste. Los datos del mensaje dependen del tipo de mensaje. En este ejemplo, se comprueba si el mensaje es una nota en el mensaje y, si es así, se envía el canal midi, la nota y la velocidad del mensaje. MessageReceived

[!code-cs[El controlador [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) del dispositivo de salida **ListBox** funciona igual que el controlador de dispositivos de entrada, salvo que no se registra ningún controlador de eventos.](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetMessageReceived)]

OutPortSelectionChanged

[!code-cs[Una vez creado el dispositivo de salida, puedes enviar un mensaje creando la nueva interfaz [**IMidiMessage**](https://msdn.microsoft.com/library/windows/apps/dn911508) para el tipo de mensaje que quieras enviar.](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetOutPortSelectionChanged)]

En este ejemplo, en mensaje es una clase [**NoteOnMessage**](https://msdn.microsoft.com/library/windows/apps/dn894817). El método [**SendMessage**](https://msdn.microsoft.com/library/windows/apps/dn894730) del objeto [**IMidiOutPort**](https://msdn.microsoft.com/library/windows/apps/dn894727) se llama para enviar el mensaje. SendMessage

[!code-cs[Cuando la aplicación se desactiva, asegúrate de limpiar los recursos de las aplicaciones.](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetSendMessage)]

Anula el registro de los controladores de eventos y establece el MIDI del puerto y los objetos externos al puerto en "null". Detén los monitores de dispositivo y establécelos en "null". CleanUp

[!code-cs[Usar el sintetizador de MIDI general integrado en Windows](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetCleanUp)]

## Cuando enumeres dispositivos MIDI de salida mediante la técnica descrita anteriormente, la aplicación detectará un dispositivo de MIDI denominado "Sintetizador por tabla de ondas GS de Microsoft".

Este es un sintetizador MIDI general integrado que puedes reproducir desde la aplicación. Sin embargo, al intentar crear un puerto de salida MIDI en este dispositivo se producirá un error a menos que hayas incluido la extensión SDK para el sintetizador integrado en el proyecto. Para incluir la extensión del SDK de sintetizador MIDI general en el proyecto de aplicación

**En el **Explorador de soluciones**, justo en tu proyecto, haz clic con el botón secundario en **Referencias** y selecciona **Agregar referencia...****

1.  Expande el nodo **Windows universal**.
2.  Selecciona **Extensiones**.
3.  En la lista de extensiones, selecciona **MIDI DLS general de Microsoft para aplicaciones universales de Windows**.
4.  **Nota** Si existen varias versiones de la extensión, asegúrate de seleccionar la versión que coincida con el destino de la aplicación.
    Puedes ver a qué versión del SDK está orientada la aplicación en la pestaña **Aplicación** de las propiedades del proyecto. You can see which SDK version your app is targeting on the <bpt id="p1">**</bpt>Application<ept id="p1">**</ept> tab of the project Properties.

 

 






<!--HONumber=May16_HO2-->


