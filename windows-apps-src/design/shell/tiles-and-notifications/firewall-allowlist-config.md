---
author: mijacobs
Description: Muchas empresas usan los servidores de seguridad para bloquear el tráfico no deseado. Este documento describe cómo permitir el tráfico WNS pasar a través de firewalls.
title: Adición de WNS tráfico a la lista de permitidos del Firewall
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, WNS, el servicio de notificaciones de windows, notificación, windows, firewall, solución de problemas, IP, el tráfico, enterprise, red, IPv4, VIP, FQDN, dirección IP pública
ms.localizationpriority: medium
ms.openlocfilehash: 466463bfc984707af4cb30618f9cbfa47d78703c
ms.sourcegitcommit: fd7d358aad3a5b7112f5a587bb6ea86805dc8a4d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976255"
---
# <a name="allowing-windows-notification-traffic-through-enterprise-firewalls"></a>Permitir el tráfico de notificación de Windows a través de firewalls empresariales

## <a name="background"></a>Background
Muchas empresas usan los servidores de seguridad para bloquear el tráfico de red no deseado; Lamentablemente, esto puede bloquear también cosas importantes como las comunicaciones de servicios de notificación de Windows. Esto significa que se eliminarán todas las notificaciones enviadas a través de WNS. Para evitar esto, los administradores de red pueden agregar la lista de canales WNS aprobadas a su lista de exención para permitir el tráfico WNS pasar a través del firewall. A continuación se muestran más detalles sobre cómo y qué se va a agregar. 


## <a name="what-information-should-be-added-to-the-allowlist"></a>¿Qué información debe agregarse a la lista de permitidos
A continuación es una lista que contiene el FQDN, la VIP y IP intervalos de direcciones usados por el servicio de notificación de Windows. 

> [!IMPORTANT]
> Se recomienda encarecidamente que permite a la lista por FQDN, ya que estos no cambiará. Si permite que la lista por FQDN, no es necesario para también permitir que los intervalos de direcciones IP.

> [!IMPORTANT]
> Los intervalos de direcciones IP cambiará periódicamente; por este motivo, no se incluyen en esta página. Si desea ver la lista de intervalos de direcciones IP, puede descargar el archivo desde el centro de descarga: [Servicio de notificación de Windows (WNS) VIP y la dirección IP a intervalos](https://www.microsoft.com/download/details.aspx?id=44238). Compruebe back con regularidad para asegurarse de que tiene la información más actualizada. 


### <a name="fqdns-vips-and-ips"></a>Las direcciones IP, FQDN y VIP
Cada uno de los elementos en el siguiente documento XML se explica en la tabla siguiente se (en [términos y las notaciones](#terms-and-notations). Los intervalos IP intencionadamente quedaron fuera de este documento para que se recomienda que use solo los FQDN como el FQDN permanecerá constantes. Sin embargo, puede descargar el archivo XML que contiene una lista completa del centro de descarga: [Servicio de notificación de Windows (WNS) VIP y la dirección IP a intervalos](https://www.microsoft.com/download/details.aspx?id=44238). Nuevo VIP o los intervalos IP will ser **efectiva una semana después de que se cargan**.

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

### <a name="terms-and-notations"></a>Los términos y las notaciones
A continuación se muestran explicaciones sobre las notaciones y elementos que se usan en el fragmento XML anterior.

| Término | Explicación |
|---|---|
| **Notación punto-decimal (es decir, 64.4.28.0/26)** | Notación punto-decimal es una forma de describir el intervalo de direcciones IP. Por ejemplo, 64.4.28.0/26 significa que los primeros bits 26 de 64.4.28.0 son constantes, mientras que los últimos 6 bits son variables.  En este caso, el intervalo de IPv4 es 64.4.28.0 - 64.4.28.63. |
| **ClientDNS** | Estos son los filtros de nombre de dominio completo (FQDN) para los dispositivos cliente (Windows PC, equipos de escritorio) recibir notificaciones de WNS. Estos se deben permitir a través del firewall para que los clientes WNS usar la funcionalidad de WNS.  Se recomienda a la lista de permitidos por el FQDN, en lugar de los intervalos de IP/VIP, ya que estos no cambia nunca. |
| **ClientIPsIPv4** | Estas son las direcciones IPv4 de los servidores de acceso a los dispositivos cliente (Windows PC, equipos de escritorio) recibir notificaciones de WNS. |
| **CloudServiceDNS** | Estos son los filtros de nombre de dominio completo (FQDN) para los servidores WNS su servicio en la nube se comunicará con enviar notificatios a WNS. Estos se deben permitir a través del firewall para los servicios enviar notificaciones de WNS.  Se recomienda a la lista de permitidos por el FQDN, en lugar de los intervalos de IP/VIP, ya que estos no cambia nunca.|
| **CloudServiceIPs** | CloudServiceIPs son las direcciones IPv4 de los servidores usados para cloud services para enviar notificaciones a WNS  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Intervalos IP públicos de servicio de notificaciones Push de Microsoft (MPNS)
Si usa el servicio de notificación heredado, MPNS, los intervalos de direcciones IP que necesitará para agregar a la lista de permitidos están disponibles desde el centro de descarga: [Intervalos de direcciones IP públicas del servicio de notificaciones Push de Microsoft (MPNS)](https://www.microsoft.com/download/details.aspx?id=44535).


## <a name="related-topics"></a>Temas relacionados

* [Inicio rápido: Enviar una notificación de inserción](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)
* [Cómo solicitar, crear y guardar un canal de notificación](https://msdn.microsoft.com/library/windows/apps/hh465412)
* [Cómo interceptar las notificaciones para la ejecución de aplicaciones](https://msdn.microsoft.com/library/windows/apps/xaml/jj709907.aspx)
* [Cómo realizar la autenticación con el servicio de notificaciones de inserción de Windows (WNS)](https://msdn.microsoft.com/library/windows/apps/hh465407)
* [Encabezados de solicitud y respuesta del servicio de notificación de inserción](https://msdn.microsoft.com/library/windows/apps/hh465435)
* [Directrices y lista de comprobación para las notificaciones de inserción](https://msdn.microsoft.com/library/windows/apps/hh761462)
 
