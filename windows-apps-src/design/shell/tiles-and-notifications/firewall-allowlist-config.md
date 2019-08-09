---
author: mijacobs
Description: Muchas empresas usan firewalls para bloquear el tráfico no deseado. Este documento describe cómo permitir que el tráfico de WNS pase a través de firewalls.
title: Agregar tráfico de WNS al firewall permitidos
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/20/2019
ms.topic: article
keywords: Windows 10, UWP, WNS, servicio de notificaciones de Windows, notificación, Windows, firewall, solución de problemas, IP, tráfico, empresa, red, IPv4, VIP, FQDN, dirección IP pública
ms.localizationpriority: medium
ms.openlocfilehash: 1f8a72eec46971fa27a4bd0dec112430f2eb3535
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867304"
---
# <a name="allowing-windows-notification-traffic-through-enterprise-firewalls"></a>Permitir el tráfico de notificaciones de Windows a través de firewalls empresariales

## <a name="background"></a>Background
Muchas empresas usan firewalls para bloquear el tráfico de red no deseado; Desafortunadamente, esto también puede bloquear aspectos importantes como las comunicaciones del servicio de notificaciones de Windows. Esto significa que se quitarán todas las notificaciones enviadas a través de WNS. Para evitar esto, los administradores de red pueden agregar la lista de canales WNS aprobados a su lista de exenciones para permitir que el tráfico de WNS pase a través del firewall. A continuación se muestran más detalles sobre cómo y qué agregar. 

> [!Note] 
A partir de 6/24/2019, los clientes de Windows **no** admiten servidores proxy, la conexión a WNS debe ser una conexión directa.

## <a name="what-information-should-be-added-to-the-allowlist"></a>Información que se debe agregar a permitidos
A continuación se muestra una lista que contiene los FQDN, las VIP y los intervalos de direcciones IP que usa el servicio de notificaciones de Windows. 

> [!IMPORTANT]
> Se recomienda encarecidamente que permita la lista por FQDN, ya que estos no cambiarán. Si permite la lista por FQDN, no es necesario permitir también los intervalos de direcciones IP.

> [!IMPORTANT]
> Los intervalos de direcciones IP cambiarán periódicamente; por este motivo, no se incluyen en esta página. Si desea ver la lista de intervalos IP, puede descargar el archivo desde el centro de descarga: [Intervalos IP y VIP del servicio de notificaciones de Windows (WNS)](https://www.microsoft.com/download/details.aspx?id=44238). Consulte periódicamente para asegurarse de que dispone de la información más actualizada. 


### <a name="fqdns-vips-and-ips"></a>FQDN, VIP e IP
Cada uno de los elementos del siguiente documento XML se explica en la tabla siguiente (en [términos y notaciones](#terms-and-notations)). Los intervalos IP se dejaron intencionadamente en este documento para que se le recomiende usar solo los FQDN, ya que los FQDN permanecerán constantes. Sin embargo, puede descargar el archivo XML que contiene la lista completa del centro de descarga: [Intervalos IP y VIP del servicio de notificaciones de Windows (WNS)](https://www.microsoft.com/download/details.aspx?id=44238). Las nuevas VIP o intervalos IP entrarán en **vigor una semana después**de cargarse.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<WNSPublicIpAddresses xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <!-- This file contains the FQDNs, VIPs, and IP address ranges used by the Windows Notification Service. A new text file will be uploaded every time a new VIP or IP range is released in production.  Please copy the below information and perform the necessary changes on your site. Endpoints in CloudService nodes are used for cloud services to send notifications to WNS. Endpoints in Client nodes are used by devices to receive notifications from WNS. --> 
    <CloudServiceDNS>
    <DNS FQDN="*.notify.windows.com"/>
    </CloudServiceDNS>
    <ClientDNS>
        <DNS FQDN="*.wns.windows.com"/>
        <DNS FQDN="*.notify.live.net"/>
    </ClientDNS>
    <CloudServiceIPs>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </CloudServiceIPs>
    <ClientIPsIPv4>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </ClientIPsIPv4>
</WNSPublicIpAddresses>

```

### <a name="terms-and-notations"></a>Términos y notaciones
A continuación se muestran explicaciones sobre las notaciones y los elementos utilizados en el fragmento de código XML anterior.

| Término | Explicación |
|---|---|
| **Notación de punto-decimal (es decir, 64.4.28.0/26)** | La notación punto-decimal es una manera de describir el intervalo de direcciones IP. Por ejemplo, 64.4.28.0/26 significa que los primeros 26 bits de 64.4.28.0 son constantes, mientras que los últimos 6 bits son variables.  En este caso, el intervalo IPv4 es 64.4.28.0-64.4.28.63. |
| **ClientDNS** | Estos son los filtros de nombre de dominio completo (FQDN) para los dispositivos cliente (PC de Windows, equipos de escritorio) que reciben notificaciones de WNS. Deben permitirse a través del firewall para que los clientes de WNS usen la funcionalidad de WNS.  Se recomienda permitir la lista de los FQDN en lugar de los intervalos IP/VIP, ya que nunca cambiarán. |
| **ClientIPsIPv4** | Estas son las direcciones IPv4 de los servidores a los que tienen acceso los dispositivos cliente (PC de Windows, equipos de escritorio) que reciben notificaciones de WNS. |
| **CloudServiceDNS** | Estos son los filtros de nombre de dominio completo (FQDN) de los servidores WNS a los que se comunicará el servicio en la nube para enviar notificatios a WNS. Deben permitirse a través del firewall para que los servicios envíen notificaciones WNS.  Se recomienda permitir la lista de los FQDN en lugar de los intervalos IP/VIP, ya que nunca cambiarán.|
| **CloudServiceIPs** | CloudServiceIPs son las direcciones IPv4 de los servidores que se usan para que los servicios en la nube envíen notificaciones a WNS.  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Intervalos IP públicos del servicio de notificaciones de envío de Microsoft (MPNS)
Si usa el servicio de notificación heredado, MPNS, los intervalos de direcciones IP que necesitará agregar a la lista de permitidos están disponibles en el centro de descarga: [Intervalos IP públicos del servicio de notificaciones de envío de Microsoft (MPNS)](https://www.microsoft.com/download/details.aspx?id=44535).


## <a name="related-topics"></a>Temas relacionados

* [Inicio rápido: Envío de una notificación de extracción](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [Solicitud, creación y guardado de un canal de notificación](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [Cómo interceptar notificaciones para la ejecución de aplicaciones](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [Cómo autenticarse con el servicio de notificaciones de extracción de Windows (WNS)](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [Encabezados de solicitud y respuesta del servicio de notificaciones de extracción](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [Instrucciones y lista de comprobación para las notificaciones de envío](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
 
