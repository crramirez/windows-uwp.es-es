---
ms.assetid: 2CC2E526-DACB-4008-9539-DA3D0C190290
description: Una introducción rápida de las tecnologías de redes disponibles para un desarrollador UWP, con sugerencias sobre cómo elegir las tecnologías que son adecuadas para la aplicación.
title: ¿Qué tecnología de redes?
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e418e5a159df44d6ff6e15e4faa972164447f5ee
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8944520"
---
# <a name="which-networking-technology"></a>¿Qué tecnología de redes?


Una introducción rápida de las tecnologías de redes disponibles para un desarrollador UWP, con sugerencias sobre cómo elegir las tecnologías que son adecuadas para la aplicación.

## <a name="sockets"></a>Sockets

Usa los [sockets](sockets.md) al comunicarte con otro dispositivo y quieras usar tu propio protocolo.

Hay dos implementaciones de sockets disponibles para los desarrolladores de la Plataforma universal de Windows (UWP): [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) y [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673). Si estás escribiendo código nuevo, Windows.Networking.Sockets tiene la ventaja de una API moderna, diseñada para los desarrolladores de UWP. Si estás usando bibliotecas de redes multiplataforma u otro código de Winsock existente, o prefieres la API de Winsock, úsalos.

### <a name="when-to-use-sockets"></a>Cuándo usar sockets

-   Ambas implementaciones de sockets te permiten comunicarte con otros dispositivos con los protocolos que elijas, con TCP o UDP.

-   Elige la API de sockets que mejor se adapte a tus necesidades en función de la experiencia y del código existente que puedas estar usando.

### <a name="when-not-to-use-sockets"></a>Cuándo no usar sockets

-   No implementes tu propia pila de HTTP(S) mediante sockets. Usa los [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639).
-   Si los WebSockets (las clases [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) y [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)) satisfacen tus necesidades de comunicaciones (TCP a/desde un servidor web), considera la posibilidad de usarlos en lugar de dedicar tus propios recursos de desarrollo y tiempo a implementar una funcionalidad similar con sockets.

## <a name="websockets"></a>Websockets

El protocolo [WebSockets](websockets.md) define un mecanismo para una comunicación bidireccional, rápida y segura entre un cliente y un servidor a través de Internet. Los datos se transfieren inmediatamente a través de una conexión de dúplex completo de un solo socket, lo que permite que ambos extremos reciban y envíen mensajes en tiempo real. WebSockets son ideales para juegos en tiempo real en los que las notificaciones instantáneas de redes sociales y la presentación actualizada de información (como las estadísticas de juegos) requieren una transferencia de datos segura y rápida. Los desarrolladores UWP pueden usar las clases [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) y [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) para conectar con los servidores que admiten el protocolo Websocket.

### <a name="when-to-use-websockets"></a>Cuándo usar Websockets

-   Si deseas enviar y recibir datos de manera continua entre un dispositivo y un servidor.

### <a name="when-not-to-use-websockets"></a>Cuándo no usar Websockets

-   Si va a enviar o recibir datos con poca frecuencia, puede que te resulte más fácil realizar solicitudes HTTP individuales desde el dispositivo al servidor, en lugar de establecer y mantener una conexión WebSocket.
-   Los WebSockets pueden no ser adecuados para situaciones de gran volumen. Considera la posibilidad de modelar tus flujos de datos y simular el tráfico a través de los WebSockets antes de confirmar su uso en el diseño.

## <a name="httpclient"></a>HttpClient

Usa [HttpClient](httpclient.md) (y el resto de la API de espacio de nombres [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)) cuando estés usando HTTP(S) para comunicarte con un servicio web o un servidor web.

### <a name="when-to-use-httpclient"></a>Cuándo usar HttpClient

-   Cuando usas HTTP(S) para comunicarte con servicios web.
-   Cuando se carga o se descarga una pequeña cantidad de archivos más pequeños.
-   Si los WebSockets (las clases [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) y [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)) cumplen tus necesidades de comunicaciones (TCP a/desde un servidor web) y el servidor web en cuestión admite WebSockets, considera la posibilidad de usarlos en lugar de dedicar tus propios recursos de desarrollo y tiempo implementar una funcionalidad similar con HttpClient.
-   Cuando estás transmitiendo en secuencias contenido en la red.

### <a name="when-not-to-use-httpclient"></a>Cuándo no usar HttpClient

-   Si vas a transferir archivos grandes o una gran cantidad de archivos, considera el uso de transferencias en segundo plano en su lugar.
-   Si deseas poder restringir los límites de carga y descarga en función del tipo de conexión, o si deseas guardar el progreso y reanudar la carga y descarga después de una interrupción, debes usar las transferencias en segundo plano.
-   Si vas a comunicarte entre dos dispositivos y ninguno está diseñado para que actúe como un servidor HTTP(S), debes usar sockets. No intentes implementar tu propio servidor HTTP y usar [HttpClient](httpclient.md) para comunicarte con él.

## <a name="background-transfers"></a>Transferencias en segundo plano

Usa la [API de transferencia en segundo plano](background-transfers.md) cuando quieras transferir archivos de forma confiable a través de la red. La API de transferencia en segundo plano proporciona funciones de carga y descarga avanzadas que se ejecutan en segundo plano durante la suspensión de la aplicación y que persisten tras la finalización de esta. La API supervisa el estado de red, y suspende y reanuda automáticamente las transferencias cuando se pierde la conexión. Además, las transferencias son compatibles con el sensor de datos y el de batería, lo que significa que la actividad de descarga se ajusta según la conectividad y el estado de la batería actuales del dispositivo. Estas funcionalidades son esenciales cuando la aplicación se ejecuta en dispositivos móviles o con batería. La API es ideal para cargar y descargar archivos grandes mediante HTTP(S). También se admite FTP, pero solo para descargas.

Una nueva característica de transferencia en segundo plano en Windows 10 es la capacidad de desencadenar un procesamiento posterior cuando una transferencia de archivos ha finalizado, por lo que puedes actualizar catálogos locales, activar otras aplicaciones o notificar al usuario cuando se completa una descarga.

### <a name="when-to-use-background-transfers"></a>Cuándo usar transferencias en segundo plano

-   Usa las transferencias en segundo plano para transferir de forma confiable archivos grandes o una gran cantidad de archivos.
-   Usa las transferencias en segundo plano con grupos de finalización de transferencia en segundo plano cuando quieras realizar un postprocesamiento de las transferencias de archivos con una tarea en segundo plano.
-   Usa las transferencias en segundo plano si deseas reanudar una transferencia en curso después de una interrupción de red.
-   Usa las transferencias en segundo plano si deseas poder cambiar el comportamiento de transferencia en función de las condiciones de red como estar en un plan de datos de uso medido.

### <a name="when-not-to-use-background-transfers"></a>Cuándo no usar transferencias en segundo plano

-   Si vas a transferir una pequeña cantidad de archivos pequeños y no es necesario hacer ningún posprocesamiento cuando se complete la transferencia, considera el uso de los métodos PUT o POST de [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639).
-   Si deseas transmitir en secuencia datos y usarlos localmente a medida que llegan, usa [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639).

## <a name="additional-network-related-technologies"></a>Otras tecnologías relacionadas con la red

### <a name="connection-quality"></a>Calidad de la conexión

La API [**Windows.Networking.Connectivity**](https://msdn.microsoft.com/library/windows/apps/br207308) te permite obtener acceso a información de uso, costo y conectividad de red. Para obtener más información sobre cómo usar esta API, consulta el tema [Acceso al estado de conexión de la red y administrar los costos de red](https://msdn.microsoft.com/library/windows/apps/hh452983)

### <a name="dns-service-discovery"></a>Detección de servicios DNS

La API [**Windows.Networking.ServiceDiscovery.Dnssd**](https://msdn.microsoft.com/library/windows/apps/dn895183) permite anunciar un servicio de red a otros dispositivos en la red mediante el protocolo DNS-SD descrito en IETF [RFC 2782](http://go.microsoft.com/fwlink/?LinkId=524158).

### <a name="communicating-over-bluetooth"></a>Comunicación a través de Bluetooth

Entre otras cosas, la API [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/dn263413) te permite usar Bluetooth para conectarte a otros dispositivos y transferir datos. Para obtener más información, consulta [Enviar o recibir archivos con RFCOMM](https://msdn.microsoft.com/library/windows/apps/mt270289).

### <a name="push-notifications-wns"></a>Notificaciones de inserción (WNS)

La API [**Windows.Networking.PushNotifications**](https://msdn.microsoft.com/library/windows/apps/br241307) te permite usar el servicio de notificaciones de Windows (WNS) para recibir notificaciones de inserción a través de la red. Para obtener más información sobre el uso de esta API, consulta la [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](https://msdn.microsoft.com/library/windows/apps/mt187203)

### <a name="near-field-communications"></a>Near Field Communications (NFC)

La API [**Windows.Networking.Proximity**](https://msdn.microsoft.com/library/windows/apps/br241250) te permite usar comunicaciones de transmisión de datos para las aplicaciones que usan proximidad o conectar con dispositivos para que la transferencia de datos sea más fácil. Para obtener más información sobre cómo usar esta API, consulta el tema [Compatibilidad con proximidad y pulsación](https://msdn.microsoft.com/library/windows/apps/hh465229).

### <a name="rssatom-feeds"></a>Fuentes RSS y Atom

La API [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) te permite administrar fuentes de sindicación con formatos RSS y Atom. Para obtener más información sobre cómo usar esta API, consulta [Fuentes RSS y Atom](web-feeds.md).

### <a name="wi-fi-enumeration-and-connection-control"></a>Control de enumeración y conexión Wi-Fi

La API [**Windows.Devices.WiFi**](https://msdn.microsoft.com/library/windows/apps/dn975224) te permite enumerar adaptadores Wi-Fi, buscar redes Wi-Fi disponibles y conectar un adaptador a una red.

### <a name="radio-control"></a>Control de radio

La API [**Windows.Devices.Radios**](https://msdn.microsoft.com/library/windows/apps/dn996447) te permite buscar y controlar las señales de radio en el dispositivo local, incluidos Wi-Fi y Bluetooth.

### <a name="wi-fi-direct"></a>Wi-Fi Direct

La API [**Windows.Devices.WiFiDirect**](https://msdn.microsoft.com/library/windows/apps/dn297687) permite conectarse y comunicarse con otros dispositivos locales mediante Wi-Fi Direct para crear redes inalámbricas locales ad-hoc.

### <a name="wi-fi-direct-services"></a>Servicios de Wi-Fi Direct

La API [**Windows.Devices.WiFiDirect.Services**](https://msdn.microsoft.com/library/windows/apps/dn996481) permite proporcionar servicios Wi-Fi Direct y conectarse a ellos. Los servicios de Wi-Fi Direct son la manera en que un dispositivo en una red Wi-Fi ad-hoc directa (un anunciante de servicio) ofrece capacidades a otro dispositivo (un solicitante de servicio) a través de una conexión Wi-Fi Direct.

### <a name="mobile-operators"></a>Operadores de telefonía móvil

Windows 10 expone a una audiencia amplia de desarrolladores algunas API que anteriormente solo se exponían a los fabricantes de dispositivos y operadores de telefonía móviles. Ten en cuenta que aunque estas API se exponen ahora, también están acompañadas de funcionalidades de aplicaciones específicas que debe aprobar Microsoft antes de que se pueda publicar una aplicación. El uso real de estas API se limita principalmente a los fabricantes de dispositivos y los operadores de telefonía móvil.

### <a name="network-operations"></a>Operaciones de red

La API [**Windows.Networking.NetworkOperators**](https://msdn.microsoft.com/library/windows/apps/br241148) trata principalmente la configuración y el aprovisionamiento de teléfonos. Como tal, el permiso para usar las funcionalidades que la controlan se limita a los fabricantes de dispositivos y los proveedores de telecomunicaciones.

### <a name="sms"></a>SMS

El espacio de nombres [**Windows.Devices.Sms**](https://msdn.microsoft.com/library/windows/apps/br206567) trata los SMS y los mensajes relacionados como entidades de bajo nivel. Se proporciona para su uso por parte de operadores de telefonía móvil para SMS dirigidos por aplicaciones, y se controla mediante una funcionalidad cuyo uso no estará aprobado para la mayoría de los desarrolladores de aplicaciones. Si estás escribiendo una aplicación para administrar mensajes, debes usar la API [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/dn642321) en su lugar, ya que está diseñada para controlar no solo los mensajes SMS, sino también los de otras fuentes, como las aplicaciones de chat en tiempo real, lo que permite una experiencia de mensajería o chat mucho más satisfactoria.

