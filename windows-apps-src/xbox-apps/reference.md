---
title: API REST del Portal de dispositivos para Xbox
description: Vea una lista de temas de referencia para la API de REST del portal de dispositivos de Xbox, que se usa para configurar y administrar la consola de forma remota.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 5ae8e953-0465-487b-81dd-54a85c904daf
ms.localizationpriority: medium
ms.openlocfilehash: be8558daa39b126061caaa132fb134c8bdef15c1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172779"
---
# <a name="xbox-device-portal-rest-api"></a>API REST del Portal de dispositivos para Xbox

Esta sección contiene temas de referencia de la API de REST del portal de dispositivos Xbox, que se usa para configurar y administrar la consola de forma remota.

| URI        | Descripción |
|------------|-------------|
|[/api/app/packagemanager/register](wdp-loose-folder-register-api.md)| Registra una aplicación que se encuentre en una carpeta débil. |
|[/api/app/packagemanager/upload](wdp-folder-upload.md)| Carga una carpeta completa a la consola. |
|[/ext/app/sshpins](uwp-sshpins-api.md)| Borre todos los pin SSH de confianza de forma remota. Necesitará volver a realizar el emparejamiento del PIN para el desarrollo de Visual Studio UWP. |
|[/ext/app/deployinfo](uwp-deployinfo-api.md)| Solicita información de implementación de uno o más paquetes instalados. |
|[/ext/fiddler](wdp-fiddler-api.md)| Habilitar y deshabilitar el seguimiento de red de Fiddler. |
|[/ext/httpmonitor/sessions](wdp-httpMonitor-api.md)| Obtenga tráfico HTTP desde la aplicación que tiene el foco en Xbox. |
|[/ext/networkcredential](uwp-networkcredentials-api.md)| Agregar, quitar o actualizar las credenciales de red. |
|[/ext/remoteinput](uwp-remoteinput-api.md)| Enviar la entrada del teclado, del mouse o del controlador de forma remota a una consola Xbox. |
|[/ext/remoteinput/controllers](uwp-remoteinput-controllers-api.md)| Obtenga el número de controladores físicos conectados o desactive todas las controladoras físicas. |
|[/ext/screenshot](wdp-media-capture-api.md)| Captura una representación de PNG de la pantalla que se muestra actualmente en la consola. |
|[/ext/settings](wdp-xboxsettings-api.md)| Accede a la configuración del desarrollador de Xbox One. |
|[/ext/smb/developerfolder](wdp-smb-api.md)| Accede a la carpeta de desarrollador en la consola a través del Explorador de archivos en el equipo de desarrollo. |
|[/ext/user](wdp-user-management.md)| Administra los usuarios en la consola Xbox One. |
|[/ext/xbox/info](wdp-xboxinfo-api.md)| Proporciona información sobre el dispositivo Xbox One. |
|[/ext/xboxlive/sandbox](wdp-sandbox-api.md)| Administra tu espacio aislado de Xbox Live. |

## <a name="see-also"></a>Vea también

- [UWP en Xbox One](index.md)
- [Portal de dispositivos Windows](../debug-test-perf/device-portal.md)