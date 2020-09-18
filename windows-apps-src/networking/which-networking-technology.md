---
ms.assetid: 2CC2E526-DACB-4008-9539-DA3D0C190290
description: Una introducción a las tecnologías de redes disponibles para un desarrollador UWP, con sugerencias sobre cómo elegir las que son adecuadas para la aplicación.
title: ¿Qué tecnología de redes?
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b3f14e06f5e6f7508c90df9f04265740daaccb49
ms.sourcegitcommit: b99e2f4dffa603b68c2a8273fe6313432f91b353
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90569379"
---
# <a name="which-networking-technology"></a>¿Qué tecnología de red?

Una introducción a las tecnologías de redes disponibles para un desarrollador UWP, con sugerencias sobre cómo elegir las que son adecuadas para la aplicación.

## <a name="sockets"></a>Sockets

Usa los [sockets](sockets.md) al comunicarte con otro dispositivo y quieras usar tu propio protocolo.

Hay dos implementaciones de sockets disponibles para los desarrolladores de la Plataforma universal de Windows (UWP): [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets) y [Winsock](/windows/desktop/WinSock/windows-sockets-start-page-2). Si estás escribiendo código nuevo, Windows.Networking.Sockets tiene la ventaja de una API moderna, diseñada para los desarrolladores de UWP. Si estás usando bibliotecas de redes multiplataforma u otro código de Winsock existente, o prefieres la API de Winsock, úsalos.

### <a name="when-to-use-sockets"></a>Cuándo usar sockets

-   Ambas implementaciones de sockets te permiten comunicarte con otros dispositivos con los protocolos que elijas, con TCP o UDP.

-   Elige la API de sockets que mejor se adapte a tus necesidades en función de la experiencia y del código existente que puedas estar usando.

### <a name="when-not-to-use-sockets"></a>Cuándo no usar sockets

-   No implementes tu propia pila de HTTP(S) mediante sockets. Usa los [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient).
-   Si los WebSockets (las clases [**StreamWebSocket**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket) y [**MessageWebSocket**](/uwp/api/Windows.Networking.Sockets.MessageWebSocket)) satisfacen tus necesidades de comunicaciones (TCP a/desde un servidor web), considera la posibilidad de usarlos en lugar de dedicar tus propios recursos de desarrollo y tiempo a implementar una funcionalidad similar con sockets.

## <a name="websockets"></a>Websockets

El protocolo [WebSockets](websockets.md) define un mecanismo para una comunicación bidireccional, rápida y segura entre un cliente y un servidor a través de Internet. Los datos se transfieren inmediatamente a través de una conexión de dúplex completo de un solo socket, lo que permite que ambos extremos reciban y envíen mensajes en tiempo real. WebSockets son ideales para juegos en tiempo real en los que las notificaciones instantáneas de redes sociales y la presentación actualizada de información (como las estadísticas de juegos) requieren una transferencia de datos segura y rápida. Los desarrolladores UWP pueden usar las clases [**StreamWebSocket**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket) y [**MessageWebSocket**](/uwp/api/Windows.Networking.Sockets.MessageWebSocket) para conectar con los servidores que admiten el protocolo Websocket.

### <a name="when-to-use-websockets"></a>Cuándo usar Websockets

-   Si deseas enviar y recibir datos de manera continua entre un dispositivo y un servidor.

### <a name="when-not-to-use-websockets"></a>Cuándo no usar Websockets

-   Si va a enviar o recibir datos con poca frecuencia, puede que te resulte más fácil realizar solicitudes HTTP individuales desde el dispositivo al servidor, en lugar de establecer y mantener una conexión WebSocket.
-   Los WebSockets pueden no ser adecuados para situaciones de gran volumen. Considera la posibilidad de modelar tus flujos de datos y simular el tráfico a través de los WebSockets antes de confirmar su uso en el diseño.

## <a name="httpclient"></a>HttpClient

Usa [HttpClient](httpclient.md) (y el resto de la API de espacio de nombres [**Windows.Web.Http**](/uwp/api/Windows.Web.Http)) cuando estés usando HTTP(S) para comunicarte con un servicio web o un servidor web.

### <a name="when-to-use-httpclient"></a>Cuándo usar HttpClient

-   Cuando usas HTTP(S) para comunicarte con servicios web.
-   Cuando se carga o se descarga una pequeña cantidad de archivos más pequeños.
-   Si los WebSockets (las clases [**StreamWebSocket**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket) y [**MessageWebSocket**](/uwp/api/Windows.Networking.Sockets.MessageWebSocket)) cumplen tus necesidades de comunicaciones (TCP a/desde un servidor web) y el servidor web en cuestión admite WebSockets, considera la posibilidad de usarlos en lugar de dedicar tus propios recursos de desarrollo y tiempo implementar una funcionalidad similar con HttpClient.
-   Cuando estás transmitiendo en secuencias contenido en la red.

### <a name="when-not-to-use-httpclient"></a>Cuándo no usar HttpClient

-   Si vas a transferir archivos grandes o una gran cantidad de archivos, considera el uso de transferencias en segundo plano en su lugar.
-   Si deseas poder restringir los límites de carga y descarga en función del tipo de conexión, o si deseas guardar el progreso y reanudar la carga y descarga después de una interrupción, debes usar las transferencias en segundo plano.
-   Si vas a comunicarte entre dos dispositivos y ninguno está diseñado para que actúe como un servidor HTTP(S), debes usar sockets. No intentes implementar tu propio servidor HTTP y usar [HttpClient](httpclient.md) para comunicarte con él.

## <a name="background-transfers"></a>Transferencias en segundo plano

Usa la [API de transferencia en segundo plano](background-transfers.md) cuando quieras transferir archivos de forma confiable a través de la red. La API de transferencia en segundo plano proporciona funciones de carga y descarga avanzadas que se ejecutan en segundo plano durante la suspensión de la aplicación y que persisten tras la finalización de esta. La API supervisa el estado de red, y suspende y reanuda automáticamente las transferencias cuando se pierde la conexión. Además, las transferencias son compatibles con el sensor de datos y el de batería, lo que significa que la actividad de descarga se ajusta según la conectividad y el estado de la batería actuales del dispositivo. Estas funcionalidades son esenciales cuando la aplicación se ejecuta en dispositivos móviles o con batería. La API es ideal para cargar y descargar archivos grandes mediante HTTP(S). También se admite FTP, pero solo para descargas.

Una nueva característica de transferencia en segundo plano en Windows 10 es la capacidad de desencadenar un procesamiento posterior cuando una transferencia de archivos ha finalizado, por lo que puedes actualizar catálogos locales, activar otras aplicaciones o notificar al usuario cuando se completa una descarga.

### <a name="when-to-use-background-transfers"></a>Cuándo usar transferencias en segundo plano

-   Usa las transferencias en segundo plano para transferir de forma confiable archivos grandes o una gran cantidad de archivos.
-   Usa las transferencias en segundo plano con grupos de finalización de transferencia en segundo plano cuando quieras realizar un postprocesamiento de las transferencias de archivos con una tarea en segundo plano.
-   Usa las transferencias en segundo plano si deseas reanudar una transferencia en curso después de una interrupción de red.
-   Usa las transferencias en segundo plano si deseas poder cambiar el comportamiento de transferencia en función de las condiciones de red como estar en un plan de datos de uso medido.

### <a name="when-not-to-use-background-transfers"></a>Cuándo no usar transferencias en segundo plano

-   Si vas a transferir una pequeña cantidad de archivos pequeños y no es necesario hacer ningún posprocesamiento cuando se complete la transferencia, considera el uso de los métodos PUT o POST de [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient).
-   Si deseas transmitir en secuencia datos y usarlos localmente a medida que llegan, usa [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient).

## <a name="additional-network-related-technologies"></a>Otras tecnologías relacionadas con la red

### <a name="connection-quality"></a>Calidad de la conexión

Las API en el espacio de nombres [**Windows.Networking.Connectivity**](/uwp/api/Windows.Networking.Connectivity) permiten obtener acceso a información de uso, costo y conectividad de red. Para obtener más información sobre cómo usar esta API, consulte el tema [Acceso al estado de conexión de la red y administración de los costos de red](/previous-versions/windows/apps/hh452985(v=win.10)).

### <a name="dns-service-discovery"></a>Detección de servicios DNS

La API [**Windows.Networking.ServiceDiscovery.Dnssd**](/uwp/api/Windows.Networking.ServiceDiscovery.Dnssd) permite anunciar un servicio de red a otros dispositivos en la red mediante el protocolo DNS-SD descrito en IETF [RFC 2782](https://www.rfc-archive.org/getrfc.php?rfc=2782).

### <a name="communicating-over-bluetooth"></a>Comunicación a través de Bluetooth

Entre otras cosas, la API [**Windows.Devices.Bluetooth**](/uwp/api/Windows.Devices.Bluetooth) te permite usar Bluetooth para conectarte a otros dispositivos y transferir datos. Para obtener más información, consulta [Enviar o recibir archivos con RFCOMM](../devices-sensors/send-or-receive-files-with-rfcomm.md).

### <a name="push-notifications-wns"></a>Notificaciones de inserción (WNS)

La API [**Windows.Networking.PushNotifications**](/uwp/api/Windows.Networking.PushNotifications) te permite usar el servicio de notificaciones de Windows (WNS) para recibir notificaciones de inserción a través de la red. Para obtener más información sobre el uso de esta API, consulta la [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)

### <a name="near-field-communications"></a>Near Field Communications (NFC)

La API [**Windows.Networking.Proximity**](/uwp/api/Windows.Networking.Proximity) te permite usar comunicaciones de transmisión de datos para las aplicaciones que usan proximidad o conectar con dispositivos para que la transferencia de datos sea más fácil. Para obtener más información sobre cómo usar esta API, consulte el tema [Proximidad y pulsación](/previous-versions/windows/apps/hh465221(v=win.10)).

### <a name="rssatom-feeds"></a>Fuentes RSS y Atom

La API [**Windows.Web.Syndication**](/uwp/api/Windows.Web.Syndication) te permite administrar fuentes de sindicación con formatos RSS y Atom. Para obtener más información sobre cómo usar esta API, consulta [Fuentes RSS y Atom](web-feeds.md).

### <a name="wi-fi-enumeration-and-connection-control"></a>Control de enumeración y conexión Wi-Fi

La API [**Windows.Devices.WiFi**](/uwp/api/Windows.Devices.WiFi) te permite enumerar adaptadores Wi-Fi, buscar redes Wi-Fi disponibles y conectar un adaptador a una red.

### <a name="radio-control"></a>Control de radio

La API [**Windows.Devices.Radios**](/uwp/api/Windows.Devices.Radios) te permite buscar y controlar las señales de radio en el dispositivo local, incluidos Wi-Fi y Bluetooth.

### <a name="wi-fi-direct"></a>Wi-Fi Direct

La API [**Windows.Devices.WiFiDirect**](/uwp/api/Windows.Devices.WiFiDirect) permite conectarse y comunicarse con otros dispositivos locales mediante Wi-Fi Direct para crear redes inalámbricas locales ad-hoc.

### <a name="wi-fi-direct-services"></a>Servicios de Wi-Fi Direct

La API [**Windows.Devices.WiFiDirect.Services**](/uwp/api/Windows.Devices.WiFiDirect.Services) permite proporcionar servicios Wi-Fi Direct y conectarse a ellos. Los servicios de Wi-Fi Direct son la manera en que un dispositivo en una red Wi-Fi ad-hoc directa (un anunciante de servicio) ofrece capacidades a otro dispositivo (un solicitante de servicio) a través de una conexión Wi-Fi Direct.

### <a name="mobile-operators"></a>Operadores de telefonía móvil

Windows 10 expone a una audiencia amplia de desarrolladores algunas API que anteriormente solo se exponían a los fabricantes de dispositivos y a los operadores de telefonía móvil. Ten en cuenta que aunque estas API se exponen ahora, también están acompañadas de funcionalidades de aplicaciones específicas que debe aprobar Microsoft antes de que se pueda publicar una aplicación. El uso real de estas API se limita principalmente a los fabricantes de dispositivos y los operadores de telefonía móvil.

### <a name="network-operations"></a>Operaciones de red

La API [**Windows.Networking.NetworkOperators**](/uwp/api/Windows.Networking.NetworkOperators) trata principalmente la configuración y el aprovisionamiento de teléfonos. Como tal, el permiso para usar las funcionalidades que la controlan se limita a los fabricantes de dispositivos y los proveedores de telecomunicaciones.

### <a name="sms"></a>SMS

El espacio de nombres [**Windows.Devices.Sms**](/uwp/api/Windows.Devices.Sms) trata los SMS y los mensajes relacionados como entidades de bajo nivel. Se proporciona para su uso por parte de operadores de telefonía móvil para SMS dirigidos por aplicaciones, y se controla mediante una funcionalidad cuyo uso no estará aprobado para la mayoría de los desarrolladores de aplicaciones. Si estás escribiendo una aplicación para administrar mensajes, debes usar la API [**Windows.ApplicationModel.Chat**](/uwp/api/Windows.ApplicationModel.Chat) en su lugar, ya que está diseñada para controlar no solo los mensajes SMS, sino también los de otras fuentes, como las aplicaciones de chat en tiempo real, lo que permite una experiencia de mensajería o chat mucho más satisfactoria.