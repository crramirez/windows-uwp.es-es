---
author: v-angraf
title: API de REST del Portal de dispositivos para Xbox
description: Referencia de API de UWP en Xbox One.
ms.author: v-angraf
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 5ae8e953-0465-487b-81dd-54a85c904daf
ms.localizationpriority: medium
ms.openlocfilehash: 894bc6f657f4a65072056a14171bf86b92cced38
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6841716"
---
# <a name="xbox-device-portal-rest-api"></a>API de REST del Portal de dispositivos para Xbox

Esta sección contiene temas de referencia para la API de REST del Portal de dispositivos para Xbox, que se usa para configurar y administrar tu consola de forma remota.

| URI        | Descripción |
|------------|-------------|
|[/api/app/packagemanager/register](wdp-loose-folder-register-api.md)| Registra una aplicación que se encuentre en una carpeta débil. |
|[/api/app/packagemanager/upload](wdp-folder-upload.md)| Carga una carpeta completa a la consola. |
|[/ext/app/sshpins](uwp-sshpins-api.md)| Borrar todas las anclas SSH de confianza SSH de manera remota. Será necesario realizar el emparejamiento de anclas de nuevo para el desarrollo de UWP de Visual Studio. |
|[/ext/app/deployinfo](uwp-deployinfo-api.md)| Solicita la información de implementación para uno o más paquetes instalados. |
|[/ext/fiddler](wdp-fiddler-api.md)| Habilitar y deshabilitar el seguimiento de la red de Fiddler. |
|[/ext/httpmonitor/sessions](wdp-httpMonitor-api.md)| Obtiene el tráfico de HTTP desde la aplicación centrada en Xbox. |
|[/ext/networkcredential](uwp-networkcredentials-api.md)| Agrega, quita o actualiza las credenciales de red. |
|[/ext/remoteinput](uwp-remoteinput-api.md)| Envía de manera remota la entrada del mouse, del teclado o de la controladora a una Xbox. |
|[/ext/remoteinput/controllers](uwp-remoteinput-controllers-api.md)| Obtiene el número de controladoras físicas conectadas o desactiva todas las controladoras físicas. |
|[/ext/screenshot](wdp-media-capture-api.md)| Captura una representación de PNG de la pantalla que se muestra actualmente en la consola. |
|[/ext/settings](wdp-xboxsettings-api.md)| Accede a la configuración del desarrollador de Xbox One. |
|[/ext/smb/developerfolder](wdp-smb-api.md)| Accede a la carpeta de desarrollador en la consola a través del Explorador de archivos en el equipo de desarrollo. |
|[/ext/user](wdp-user-management.md)| Administra los usuarios en la consola Xbox One. |
|[/ext/xbox/info](wdp-xboxinfo-api.md)| Proporciona información acerca del dispositivo Xbox One. |
|[/ext/xboxlive/sandbox](wdp-sandbox-api.md)| Administra tu espacio aislado de Xbox Live. |

## <a name="see-also"></a>Consulta también

- [UWP en Xbox One](index.md)
- [Portal de dispositivos Windows](../debug-test-perf/device-portal.md)