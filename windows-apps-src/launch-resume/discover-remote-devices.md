---
title: Detectar dispositivos remotos
description: Obtenga información sobre cómo detectar dispositivos remotos desde su aplicación mediante el proyecto Roma.
ms.assetid: 5b4231c0-5060-49e2-a577-b747e20cf633
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, dispositivos conectados, sistemas remotos, Roma, proyecto Roma
ms.localizationpriority: medium
ms.openlocfilehash: 01c13a30c8869643badc69c546b0a5212308956f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155929"
---
# <a name="discover-remote-devices"></a>Detectar dispositivos remotos
La aplicación puede usar la red inalámbrica, el Bluetooth y la conexión en la nube para detectar dispositivos Windows que han iniciado sesión con el mismo cuenta de Microsoft que el dispositivo de detección. Los dispositivos remotos no necesitan tener instalado ningún software especial para que se puedan reconocer.

> [!NOTE]
> Esta guía da por hecho que ya has concedido acceso a la característica de sistemas remotos siguiendo los pasos de [Iniciar una aplicación remota](launch-a-remote-app.md).

## <a name="filter-the-set-of-discoverable-devices"></a>Filtrar el conjunto de dispositivos reconocibles
Puede restringir el conjunto de dispositivos que se pueden detectar mediante el uso de un [**RemoteSystemWatcher**](/uwp/api/Windows.System.RemoteSystems.RemoteSystemWatcher) con filtros. Los filtros pueden detectar el tipo de detección (proximal frente a red local frente a conexión a la nube), tipo de dispositivo (escritorio, dispositivo móvil, Xbox, Hub y Holographic) y estado de disponibilidad (el estado de la disponibilidad de un dispositivo para usar características del sistema remoto).

Los objetos de filtro se deben construir antes o mientras se inicializa el objeto **RemoteSystemWatcher** , ya que se pasan como un parámetro en su constructor. El siguiente código crea un filtro de cada tipo disponible y luego los agrega a una lista.

> [!NOTE]
> El código de estos ejemplos requiere que tenga una `using Windows.System.RemoteSystems` instrucción en el archivo.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

> [!NOTE]
> El valor de filtro "proximal" no garantiza el grado de proximidad física. En escenarios que requieren proximidad física confiable, use el valor [**RemoteSystemDiscoveryType. SpatiallyProximal**](/uwp/api/windows.system.remotesystems.remotesystemdiscoverytype) en el filtro. Actualmente, este filtro solo permite dispositivos detectados por Bluetooth. A medida que se admitan nuevos mecanismos de detección y protocolos que garanticen la proximidad física, también se incluirán aquí.  
También hay una propiedad en la clase [**RemoteSystem**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem) que indica si un dispositivo detectado está en realidad en la proximidad física: [**RemoteSystem. IsAvailableBySpatialProximity**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem.IsAvailableByProximity).

> [!NOTE]
> Si tiene previsto detectar dispositivos a través de una red local (determinada por la selección de filtro de tipo de detección), la red debe usar un perfil "privado" o "dominio". El dispositivo no detectará otros dispositivos a través de una red "pública".

Una vez que se crea una lista de objetos [**IRemoteSystemFilter**](/uwp/api/Windows.System.RemoteSystems.IRemoteSystemFilter), se puede pasar en el constructor de un **RemoteSystemWatcher**.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

Cuando se llama al método [**Inicio**](/uwp/api/windows.system.remotesystems.remotesystemwatcher.start) de este observador, se genera el evento [**RemoteSystemAdded**](/uwp/api/windows.system.remotesystems.remotesystemwatcher.remotesystemadded) solo si se detecta un dispositivo que cumpla con todos los criterios siguientes:
* Es reconocible por conexión de proximidad
* Es un escritorio o un teléfono
* Se clasifica como disponible

Desde allí, el procedimiento para controlar los eventos, recuperar objetos [**RemoteSystem**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem) y conectarse a dispositivos remotos es exactamente igual que el de [Iniciar una aplicación remota](launch-a-remote-app.md). En Resumen, los objetos **RemoteSystem** se almacenan como propiedades de objetos [**RemoteSystemAddedEventArgs**](/uwp/api/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs) , que se pasan con cada evento **RemoteSystemAdded** .

## <a name="discover-devices-by-address-input"></a>Detectar dispositivos mediante entrada de dirección
Es posible que algunos dispositivos no estén asociados a un usuario o no sean detectables mediante un examen, pero se puede llegar a ellos igualmente si la aplicación detectada usa una dirección directa. La clase [**HostName**](/uwp/api/windows.networking.hostname) se usa para representar la dirección de un dispositivo remoto. A menudo se almacena en forma de una dirección IP, pero se permiten varios otros formatos (consulta el [**constructor de HostName**](/uwp/api/windows.networking.hostname.-ctor) para obtener información detallada).

Un objeto **RemoteSystem** se recupera si se proporciona un objeto **HostName** válido. Si los datos de la dirección no son válidos, se devuelve una referencia de objeto `null`.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## <a name="querying-a-capability-on-a-remote-system"></a>Consultar una funcionalidad en un sistema remoto

Aunque es independiente del filtrado de detección, la consulta de las capacidades del dispositivo puede ser una parte importante del proceso de detección. Mediante el método [**RemoteSystem. GetCapabilitySupportedAsync**](/uwp/api/windows.system.remotesystems.remotesystem.GetCapabilitySupportedAsync) , puede consultar sistemas remotos detectados para la compatibilidad con determinadas funcionalidades, como la conectividad de sesión remota o el uso compartido de entidades espaciales (Holographic). Vea la clase [**KnownRemoteSystemCapabilities**](/uwp/api/windows.system.remotesystems.knownremotesystemcapabilities) para ver la lista de funcionalidades consultables.

```csharp
// Check to see if the given remote system can accept LaunchUri requests
bool isRemoteSystemLaunchUriCapable = remoteSystem.GetCapabilitySupportedAsync(KnownRemoteSystemCapabilities.LaunchUri);
```

## <a name="cross-user-discovery"></a>Detección de usuarios cruzados

Los desarrolladores pueden especificar la detección de _todos los_ dispositivos en proximidad al dispositivo cliente, no solo a los dispositivos registrados en el mismo usuario. Esto se implementa a través de un **IRemoteSystemFilter**especial, [**RemoteSystemAuthorizationKindFilter**](/uwp/api/windows.system.remotesystems.remotesystemauthorizationkindfilter). Se implementa como otros tipos de filtro:

```csharp
// Construct a user type filter that includes anonymous devices
RemoteSystemAuthorizationKindFilter authorizationKindFilter = new RemoteSystemAuthorizationKindFilter(RemoteSystemAuthorizationKind.Anonymous);
// then add this filter to the RemoteSystemWatcher
```

* Un valor de [**RemoteSystemAuthorizationKind**](/uwp/api/windows.system.remotesystems.remotesystemauthorizationkind) de **Anonymous** permitirá la detección de todos los dispositivos proximal, incluso los de los usuarios que no son de confianza.
* Un valor de **SameUser** filtra la detección solo por los dispositivos registrados en el mismo usuario que el dispositivo cliente. Este es el comportamiento predeterminado.

### <a name="checking-the-cross-user-sharing-settings"></a>Comprobar la configuración de uso compartido entre usuarios

Además del filtro anterior que se especifica en la aplicación de detección, el propio dispositivo cliente también debe estar configurado para permitir experiencias compartidas de los dispositivos que han iniciado sesión con otros usuarios. Se trata de una configuración del sistema que se puede consultar con un método estático en la clase **RemoteSystem** :

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Anonymous".
}
```

Para cambiar esta configuración, el usuario debe abrir la aplicación de **configuración** . En el **System**  >  menú recursos**compartidos de experiencias compartidas**en  >  **varios dispositivos** , hay un cuadro desplegable en el que el usuario puede especificar los dispositivos con los que el sistema puede compartir.

![Página de configuración de experiencias compartidas](images/shared-experiences-settings.png)

## <a name="related-topics"></a>Temas relacionados
* [Aplicaciones y dispositivos conectados (Proyecto Roma)](connected-apps-and-devices.md)
* [Iniciar una aplicación remota](launch-a-remote-app.md)
* [Referencia de API de sistemas remotos](/uwp/api/Windows.System.RemoteSystems)
* [Muestra de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)