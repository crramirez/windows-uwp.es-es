---
author: PatrickFarley
title: Detectar dispositivos remotos
description: "Obtén información sobre cómo detectar dispositivos remotos desde tu aplicación con el proyecto &quot;Roma&quot;."
ms.assetid: 5b4231c0-5060-49e2-a577-b747e20cf633
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: c655ebf4d02f0de6e07d70ac0d81e33f20388741
ms.lasthandoff: 02/07/2017

---

# <a name="discover-remote-devices"></a>Detectar dispositivos remotos
Tu aplicación puede usar la red inalámbrica, Bluetooth y una conexión de nube para detectar dispositivos Windows en los que se inicie sesión con la misma cuenta de Microsoft que en el dispositivo detectado. Los dispositivos comunes que pueden aceptar conexiones anónimas, como Surface Hub y Xbox One, también son reconocibles. Los dispositivos remotos no necesitan tener instalado ningún software especial para que se puedan reconocer.

> [!NOTE]
> Esta guía da por hecho que ya has concedido acceso a la característica de sistemas remotos siguiendo los pasos de [Iniciar una aplicación remota](launch-a-remote-app.md).

## <a name="filter-the-set-of-discoverable-devices"></a>Filtrar el conjunto de dispositivos reconocibles
Puedes reducir el conjunto de dispositivos reconocibles mediante el uso de [**RemoteSystemWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher) con filtros. Los filtros pueden detectar el tipo de detección (red local frente a conexión de nube), el tipo de dispositivo (escritorio, dispositivos móviles, Xbox, Hub y Holographic) y el estado de disponibilidad (el estado de disponibilidad de un dispositivo para usar características de sistema remoto).

Los objetos de filtro se deben crear antes de que se inicialice el objeto **RemoteSystemWatcher**, ya que se pasan como un parámetro en su constructor. El siguiente código crea un filtro de cada tipo disponible y luego los agrega a una lista.

> [!NOTE]
> El código de estos ejemplos supone que tienes una instrucción `using Windows.System.RemoteSystems` en el archivo.

[!code-cs[Principal](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

Una vez que se crea una lista de objetos [**IRemoteSystemFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.IRemoteSystemFilter), se puede pasar en el constructor de un **RemoteSystemWatcher**.

[!code-cs[Principal](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

Cuando se llama al método [**Inicio**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.Start) de este observador, se genera el evento [**RemoteSystemAdded**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.RemoteSystemAdded) solo si se detecta un dispositivo que cumpla con todos los criterios siguientes:
* Es reconocible por conexión de proximidad
* Es un escritorio o un teléfono
* Se clasifica como disponible

Desde allí, el procedimiento para controlar los eventos, recuperar objetos [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) y conectarse a dispositivos remotos es exactamente igual que el de [Iniciar una aplicación remota](launch-a-remote-app.md). En resumen, los objetos **RemoteSystem** se almacenan como propiedades de objetos [**RemoteSystemAddedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs), que son parámetros de cada evento **RemoteSystemAdded**.

## <a name="discover-devices-by-address-input"></a>Detectar dispositivos mediante entrada de dirección
Es posible que algunos dispositivos no estén asociados a un usuario o no sean detectables mediante un examen, pero se puede llegar a ellos igualmente si la aplicación detectada usa una dirección directa. La clase [**HostName**](https://msdn.microsoft.com/library/windows/apps/windows.networking.hostname.aspx) se usa para representar la dirección de un dispositivo remoto. A menudo se almacena en forma de una dirección IP, pero se permiten varios otros formatos (consulta el [**constructor de HostName**](https://msdn.microsoft.com/library/windows/apps/br207118.aspx) para obtener información detallada).

Un objeto **RemoteSystem** se recupera si se proporciona un objeto **HostName** válido. Si los datos de la dirección no son válidos, se devuelve una referencia de objeto `null`.

[!code-cs[Principal](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## <a name="related-topics"></a>Temas relacionados
[Aplicaciones y dispositivos conectados (proyecto "Roma")](connected-apps-and-devices.md)  
[Iniciar una aplicación remota](launch-a-remote-app.md)  
[Referencia de API de sistemas remotos](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[Muestra de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) indica cómo detectar un sistema remoto, iniciar una aplicación en un sistema remoto y usar los servicios de aplicaciones para enviar mensajes entre aplicaciones que se ejecuten en dos sistemas.

