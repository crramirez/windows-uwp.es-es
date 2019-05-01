---
ms.assetid: ''
description: Este artículo muestra cómo conectarse a cámaras remotas y obtener un MediaFrameSourceGroup para recuperar los marcos de cada cámara.
title: Conectarse a cámaras remotas
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bc719b8dad2adef0542edf284d257846052eac21
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63789588"
---
# <a name="connect-to-remote-cameras"></a>Conectarse a cámaras remotas

En este artículo se muestra cómo conectarse a uno o más cámaras remotas y obtener un [ **MediaFrameSourceGroup** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) objeto que permite leer los marcos de cada cámara. Para obtener más información en la lectura de marcos de un origen de medios, consulte [procesar fotogramas media con MediaFrameReader](process-media-frames-with-mediaframereader.md). Para obtener más información sobre el emparejamiento con dispositivos, consulte [emparejar los dispositivos](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices).

> [!NOTE] 
> Las características tratadas en este artículo solo están disponibles a partir de Windows 10, versión 1903.

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>Cree una clase DeviceWatcher para inspeccionar los cámaras remotas disponibles

El [ **DeviceWatcher** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) clase supervisa los dispositivos disponibles para la aplicación y notifica a la aplicación cuando se agregan o quitan dispositivos. Obtener una instancia de **DeviceWatcher** mediante una llamada a [ **DeviceInformation.CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_), pasando una cadena de sintaxis de consulta avanzada (AQS) que identifica el tipo de dispositivos que desea supervisar. La cadena AQS que especifica los dispositivos de cámara de red es la siguiente:

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> El método auxiliar [ **MediaFrameSourceGroup.GetDeviceSelector** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) devuelve una cadena AQS que supervisará las cámaras de red conectados de forma local y remota. Para supervisar sólo las cámaras de red, debe usar la cadena AQS mostrada anteriormente.


Al iniciar el valor devuelto **DeviceWatcher** mediante una llamada a la [ **iniciar** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start) método, se producirá la [ **Added** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) eventos para cada cámara de red que está disponible actualmente. Hasta que se detenga el monitor mediante una llamada a [ **detener**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop), **Added** evento se genera cuando surgen nuevos dispositivos de cámara de red y la [ **Quitado** ](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.devicewatcher.removed) evento se genera cuando un dispositivo de la cámara no está disponible.

Los argumentos del evento pasan a la **Added** y **quitado** controladores de eventos son un [ **DeviceInformation** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) o un [  **DeviceInformationUpdate** ](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.deviceinformationupdate) objeto, respectivamente. Cada uno de estos objetos tiene un **Id** propiedad que es el identificador de la cámara de red para el que se desencadenó el evento. Pase este identificador en el [ **MediaFrameSourceGroup.FromIdAsync** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) método para obtener un [ **MediaFrameSourceGroup** ](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) objeto que puede usar para recuperar los marcos de la cámara.

## <a name="remote-camera-pairing-helper-class"></a>Clase auxiliar de emparejamiento cámara remota

El ejemplo siguiente muestra una clase auxiliar que usa un **DeviceWatcher** para crear y actualizar una **ObservableCollection** de **MediaFrameSourceGroup** objetos para admitir enlace de datos a la lista de las cámaras. Las aplicaciones habituales se ajustan la **MediaFrameSourceGroup** en una clase de modelo personalizado. Tenga en cuenta que la clase auxiliar mantiene una referencia a la aplicación [ **CoreDispatcher** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) y actualiza la colección de las cámaras en las llamadas a [ **RunAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) para asegurarse de que se actualiza la interfaz de usuario enlaza a la colección en el subproceso de interfaz de usuario.

Además, en este ejemplo se atiende la [ **DeviceWatcher.Updated** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) eventos además el **Added** y **quitado** eventos. En el **Updated** controlador, el dispositivo de la cámara remota asociada se quitan y, a continuación, volver a agregar a la colección.

[!code-cs[SnippetRemoteCameraPairingHelper](./code/Frames_Win10/Frames_Win10/RemoteCameraPairingHelper.cs#SnippetRemoteCameraPairingHelper)]


## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Capturar básica de fotos, vídeo y audio con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Ejemplo de marcos de cámara](https://go.microsoft.com/fwlink/?LinkId=823230)
* [Marcos de procesamiento multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md)
 

 




