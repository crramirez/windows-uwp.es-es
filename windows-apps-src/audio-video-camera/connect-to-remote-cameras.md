---
ms.assetid: ''
description: En este artículo se muestra cómo conectarse a cámaras remotas y obtener un MediaFrameSourceGroup para recuperar fotogramas de cada cámara.
title: Conexión a cámaras remotas
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5924ad4b969fbf29021b2b48440ce071a516fe09
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175729"
---
# <a name="connect-to-remote-cameras"></a>Conexión a cámaras remotas

En este artículo se muestra cómo conectarse a una o varias cámaras remotas y cómo obtener un objeto [**MediaFrameSourceGroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) que le permita leer fotogramas de cada cámara. Para obtener más información sobre cómo leer fotogramas de un origen multimedia, consulte [procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md). Para obtener más información sobre el emparejamiento con dispositivos, consulte [Pair Devices](../devices-sensors/pair-devices.md).

> [!NOTE] 
> Las características que se describen en este artículo solo están disponibles a partir de Windows 10, versión 1903.

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>Cree una clase DeviceWatcher para ver las cámaras remotas disponibles

La clase [**DeviceWatcher**](/uwp/api/windows.devices.enumeration.devicewatcher) supervisa los dispositivos disponibles para la aplicación y notifica a la aplicación cuando se agregan o quitan dispositivos. Obtenga una instancia de **DeviceWatcher** llamando a [**DeviceInformation. CreateWatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_), pasando una cadena de sintaxis de consulta avanzada (AQS) que identifica el tipo de dispositivos que desea supervisar. La cadena AQS que especifica dispositivos de cámara de red es la siguiente:

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> El método auxiliar [**MediaFrameSourceGroup. GetDeviceSelector**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) devuelve una cadena AQS que supervisará las cámaras de red remotas y conectadas localmente. Para supervisar solo las cámaras de red, debe usar la cadena AQS mostrada anteriormente.


Al iniciar la **DeviceWatcher** devuelta llamando al método [**Start**](/uwp/api/windows.devices.enumeration.devicewatcher.start) , se generará el evento [**Added**](/uwp/api/windows.devices.enumeration.devicewatcher.added) para cada cámara de red que esté disponible actualmente. Hasta que detenga el monitor mediante una llamada a [**Stop**](/uwp/api/windows.devices.enumeration.devicewatcher.stop), el evento **agregado** se generará cuando haya nuevos dispositivos de cámara de red disponibles y el evento [**eliminado**](/uwp/api/windows.devices.enumeration.devicewatcher.removed) se generará cuando un dispositivo de cámara deje de estar disponible.

Los argumentos del evento que se pasan a los controladores de eventos **agregados** y **quitados** son un objeto [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) o [**DeviceInformationUpdate**](/uwp/api/windows.devices.enumeration.deviceinformationupdate) , respectivamente. Cada uno de estos objetos tiene una propiedad **ID** que es el identificador de la cámara de red para la que se activó el evento. Pase este identificador al método [**MediaFrameSourceGroup. FromIdAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) para obtener un objeto [**MediaFrameSourceGroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) que pueda usar para recuperar fotogramas de la cámara.

## <a name="remote-camera-pairing-helper-class"></a>Clase auxiliar de emparejamiento de cámara remoto

En el ejemplo siguiente se muestra una clase auxiliar que utiliza un objeto **DeviceWatcher** para crear y actualizar un **ObservableCollection** de objetos **MediaFrameSourceGroup** para admitir el enlace de datos a la lista de cámaras. Las aplicaciones típicas encapsularían **MediaFrameSourceGroup** en una clase de modelo personalizado. Tenga en cuenta que la clase auxiliar mantiene una referencia al [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) de la aplicación y actualiza la colección de cámaras dentro de las llamadas a [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) para asegurarse de que la interfaz de usuario enlazada a la colección se actualiza en el subproceso de la interfaz de usuario.

Además, este ejemplo controla el evento [**DeviceWatcher. Updated**](/uwp/api/windows.devices.enumeration.devicewatcher.updated) , además de los eventos **agregados** y **quitados** . En el controlador **actualizado** , el dispositivo de cámara remota asociado se quita de y, a continuación, se vuelve a agregar a la colección.

[!code-cs[SnippetRemoteCameraPairingHelper](./code/Frames_Win10/Frames_Win10/RemoteCameraPairingHelper.cs#SnippetRemoteCameraPairingHelper)]


## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Muestra de fotogramas de cámara](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md)
 

 