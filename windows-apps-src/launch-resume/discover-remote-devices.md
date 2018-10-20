---
author: PatrickFarley
title: Detectar dispositivos remotos
description: Obtén información sobre cómo detectar dispositivos remotos desde tu aplicación con Project Rome.
ms.assetid: 5b4231c0-5060-49e2-a577-b747e20cf633
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: dispositivos Windows 10, uwp, conectados, sistemas remotos, Roma, proyecto rome
ms.localizationpriority: medium
ms.openlocfilehash: 02d04074ece0033da8c3454a95bc35af201903f3
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2018
ms.locfileid: "5170315"
---
# <a name="discover-remote-devices"></a>Detectar dispositivos remotos
Tu aplicación puede usar la red inalámbrica, Bluetooth y conexión de nube para detectar dispositivos Windows en los que se inicie sesión con la misma cuenta de Microsoft que en el dispositivo detectado. Los dispositivos remotos no necesitan tener instalado ningún software especial para que se puedan reconocer.

> [!NOTE]
> Esta guía da por hecho que ya has concedido acceso a la característica de sistemas remotos siguiendo los pasos de [Iniciar una aplicación remota](launch-a-remote-app.md).

## <a name="filter-the-set-of-discoverable-devices"></a>Filtrar el conjunto de dispositivos reconocibles
Puedes reducir el conjunto de dispositivos reconocibles mediante el uso de [**RemoteSystemWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher) con filtros. Los filtros pueden detectar el tipo de detección (próxima frente a red local frente a conexión de nube), el tipo de dispositivo (escritorio, dispositivos móviles, Xbox, Hub y Holographic) y el estado de disponibilidad (el estado de disponibilidad de un dispositivo para usar características de sistema remoto).

Los objetos de filtro se deben crear antes de que se inicialice el objeto **RemoteSystemWatcher** o mientras se inicialice, ya que se pasan como un parámetro en su constructor. El siguiente código crea un filtro de cada tipo disponible y luego los agrega a una lista.

> [!NOTE]
> El código de estos ejemplos requiere que tengas una instrucción `using Windows.System.RemoteSystems` en el archivo.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

> [!NOTE]
> El valor de filtro de proximidad no garantiza el grado de proximidad física. Para escenarios que requieren proximidad física confiable, usa el valor [**RemoteSystemDiscoveryType.SpatiallyProximal**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemdiscoverytype) en el filtro. Actualmente, este filtro solo permite que los dispositivos se detecten por Bluetooth. Como se admiten nuevos mecanismos y protocolos de detección que garantizan la proximidad física, se incluirán aquí también.  
También hay una propiedad en la clase [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) que indica si un dispositivo detectado está en realidad próximo físicamente: [**RemoteSystem.IsAvailableBySpatialProximity**](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystem.IsAvailableByProximity).

> [!NOTE]
> Si quieres detectar dispositivos a través de una red local (determinado por la selección de filtro de tipo de detección), la red debe estar utilizando un perfil "privado" o de "dominio". El dispositivo no detectará otros dispositivos en una red "pública".

Una vez que se crea una lista de objetos [**IRemoteSystemFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.IRemoteSystemFilter), se puede pasar en el constructor de un **RemoteSystemWatcher**.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

Cuando se llama al método [**Inicio**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.Start) de este observador, se genera el evento [**RemoteSystemAdded**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.RemoteSystemAdded) solo si se detecta un dispositivo que cumpla con todos los criterios siguientes:
* Es reconocible por conexión de proximidad
* Es un escritorio o un teléfono
* Se clasifica como disponible

Desde allí, el procedimiento para controlar los eventos, recuperar objetos [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) y conectarse a dispositivos remotos es exactamente igual que el de [Iniciar una aplicación remota](launch-a-remote-app.md). En resumen, los objetos **RemoteSystem** se almacenan como propiedades de objetos [**RemoteSystemAddedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs), que se pasan con cada evento **RemoteSystemAdded**.

## <a name="discover-devices-by-address-input"></a>Detectar dispositivos mediante entrada de dirección
Es posible que algunos dispositivos no estén asociados a un usuario o no sean detectables mediante un examen, pero se puede llegar a ellos igualmente si la aplicación detectada usa una dirección directa. La clase [**HostName**](https://msdn.microsoft.com/library/windows/apps/windows.networking.hostname.aspx) se usa para representar la dirección de un dispositivo remoto. A menudo se almacena en forma de una dirección IP, pero se permiten varios otros formatos (consulta el [**constructor de HostName**](https://msdn.microsoft.com/library/windows/apps/br207118.aspx) para obtener información detallada).

Un objeto **RemoteSystem** se recupera si se proporciona un objeto **HostName** válido. Si los datos de la dirección no son válidos, se devuelve una referencia de objeto `null`.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## <a name="querying-a-capability-on-a-remote-system"></a>Consultar una funcionalidad en un sistema remoto

Aunque independiente del filtrado de detección, la consulta de las funciones de dispositivo puede constituir una parte importante del proceso de detección. Con el método [**RemoteSystem.GetCapabilitySupportedAsync**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystem.GetCapabilitySupportedAsync), puedes consultar sistemas remotos detectados para admitir determinadas funciones como la conectividad de sesión remota o el uso compartido de la entidad espacial (holográfica). Consulta la clase [**KnownRemoteSystemCapabilities**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.knownremotesystemcapabilities) para la lista de funcionalidades consultables.

```csharp
// Check to see if the given remote system can accept LaunchUri requests
bool isRemoteSystemLaunchUriCapable = remoteSystem.GetCapabilitySupportedAsync(KnownRemoteSystemCapabilities.LaunchUri);
```

## <a name="cross-user-discovery"></a>Detección entre usuarios

Los desarrolladores pueden especificar la detección de _todos_ los dispositivos próximos al dispositivo cliente, no solo los dispositivos registrados para el mismo usuario. Esto se implementa mediante un **IRemoteSystemFilter**, [**RemoteSystemAuthorizationKindFilter**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkindfilter) especial. Se implementa como los otros tipos de filtros:

```csharp
// Construct a user type filter that includes anonymous devices
RemoteSystemAuthorizationKindFilter authorizationKindFilter = new RemoteSystemAuthorizationKindFilter(RemoteSystemAuthorizationKind.Anonymous);
// then add this filter to the RemoteSystemWatcher
```

* Un valor de [**RemoteSystemAuthorizationKind**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkind) de **Anonymous** permitirá la detección de todos los dispositivos próximas, incluso de aquellos usuarios que no son de confianza.
* Un valor de **SameUser** filtra la detección únicamente a los dispositivos registrados para el mismo usuario que el dispositivo cliente. Este es el comportamiento predeterminado.

### <a name="checking-the-cross-user-sharing-settings"></a>Comprobar la configuración de uso compartido entre usuarios

Además del filtro anterior que se especifica en la aplicación de detección, el propio dispositivo cliente también debe configurarse para permitir experiencias compartidas desde dispositivos en los que se ha iniciado sesión con otros usuarios. Se trata de una configuración del sistema que se puede consultar con un método estático en la clase **RemoteSystem**:

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Anonymous".
}
```

Para cambiar esta configuración, el usuario debe abrir la aplicación **Configuración**. En el menú **Sistema** > **Experiencias compartidas** > **Compartir entre dispositivos**, hay un cuadro desplegable donde el usuario puede especificar con qué dispositivos puede compartir su sistema.

![página de configuración de experiencias compartidas](images/shared-experiences-settings.png)

## <a name="related-topics"></a>Temas relacionados
* [Aplicaciones y dispositivos conectados (Project Rome)](connected-apps-and-devices.md)
* [Iniciar una aplicación remota](launch-a-remote-app.md)
* [Referencia de API de sistemas remotos](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)
* [Muestra de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
