---
author: mijacobs
Description: Muchas empresas usan firewalls para bloquear el tráfico no deseado. Este documento describe cómo permitir que el tráfico de WNS pase a través de firewalls.
title: Agregar tráfico de WNS al firewall permitidos
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.date: 05/20/2019
ms.topic: article
keywords: Windows 10, UWP, WNS, servicio de notificaciones de Windows, notificación, Windows, firewall, solución de problemas, IP, tráfico, empresa, red, IPv4, VIP, FQDN, dirección IP pública
ms.localizationpriority: medium
ms.openlocfilehash: 7f87dc0cc174a22f474c91a58f3ffeb738822fa8
ms.sourcegitcommit: 963316e065cf36c17b6360c3f89fba93a1a94827
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82868907"
---
# <a name="enterprise-firewall-and-proxy-configurations-to-support-wns-traffic"></a>Configuraciones de firewall y proxy de empresa para admitir el tráfico de WNS

## <a name="background"></a>Información previa
Muchas empresas usan firewalls para bloquear el tráfico de red y los puertos no deseados; Desafortunadamente, esto también puede bloquear aspectos importantes como las comunicaciones del servicio de notificaciones de Windows. Esto significa que todas las notificaciones enviadas a través de WNS se quitarán en determinadas configuraciones de red. Para evitar esto, los administradores de red pueden agregar la lista de FQDN o VIP de WNS aprobados a su lista de exenciones para permitir que el tráfico de WNS pase a través del firewall. A continuación se muestran más detalles sobre cómo y qué agregar, así como la compatibilidad con distintos tipos de proxy.

## <a name="proxy-support"></a>Compatibilidad con proxy

> [!Note]
> Los clientes de Windows **no** admiten todos los servidores proxy, la conexión a WNS debe ser una conexión directa.

**¡Próximamente!** Estamos investigando activamente diferentes configuraciones de red, servidores proxy y firewalls. Esta página se actualizará con más detalles sobre escenarios empresariales comunes y la compatibilidad con WNS en breve.


## <a name="what-information-should-be-added-to-the-allowlist"></a>Información que se debe agregar a permitidos
A continuación se muestra una lista que contiene los FQDN, las VIP y los intervalos de direcciones IP que usa el servicio de notificaciones de Windows. 

> [!IMPORTANT]
> Se recomienda encarecidamente que permita la lista por FQDN, ya que estos no cambiarán. Si permite la lista por FQDN, no es necesario permitir también los intervalos de direcciones IP.

> [!IMPORTANT]
> Los intervalos de direcciones IP cambiarán periódicamente; por este motivo, no se incluyen en esta página. Si desea ver la lista de intervalos IP, puede descargar el archivo desde el centro de descarga: [VIP e intervalos IP de servicio de notificaciones de Windows (WNS)](https://www.microsoft.com/download/details.aspx?id=44238). Consulte periódicamente para asegurarse de que dispone de la información más actualizada. 


### <a name="fqdns-vips-ips-and-ports"></a>FQDN, direcciones VIP, direcciones IP y puertos
Independientemente del método que elija, deberá permitir el tráfico de red a los destinos enumerados a través del **puerto 443**. Cada uno de los elementos del siguiente documento XML se explica en la tabla siguiente (en [términos y notaciones](#terms-and-notations)). Los intervalos IP se dejaron intencionadamente en este documento para que se le recomiende usar solo los FQDN, ya que los FQDN permanecerán constantes. Sin embargo, puede descargar el archivo XML que contiene la lista completa desde el centro de descarga: [VIP e intervalos IP de servicio de notificaciones de Windows (WNS)](https://www.microsoft.com/download/details.aspx?id=44238). Las nuevas VIP o intervalos IP entrarán en **vigor una semana después de cargarse**.

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
    <IdentityServiceDNS>
        <DNS FQDN="login.microsoftonline.com"/>
        <DNS FQDN="login.live.com"/>
    </IdentityServiceDNS>
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
Si usa el servicio de notificación heredado, MPNS, los intervalos de direcciones IP que necesitará agregar a la lista de permitidos están disponibles en el centro de descarga: [intervalos de IP pública del servicio de notificaciones de envío de Microsoft (MPNS)](https://www.microsoft.com/download/details.aspx?id=44535).


## <a name="related-topics"></a>Temas relacionados

* [Inicio rápido: Envío de una notificación de inserción](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [Cómo solicitar, crear y guardar un canal de notificación](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [Cómo interceptar notificaciones para aplicaciones en ejecución](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [Cómo autenticar con los Servicios de notificaciones de inserción de Windows (WNS)](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [Solicitud de servicio de notificaciones de inserción y encabezados de respuesta](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [Directrices y lista de comprobación de notificaciones de inserción](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
 
