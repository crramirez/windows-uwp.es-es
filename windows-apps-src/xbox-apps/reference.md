---
title: API de REST del Portal de dispositivos para Xbox
description: Referencia de API para UWP en Xbox One.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 5ae8e953-0465-487b-81dd-54a85c904daf
ms.localizationpriority: medium
ms.openlocfilehash: d8fdcf01d7d1f72624d73acf2d10ce28dfb75e04
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657000"
---
# <a name="xbox-device-portal-rest-api"></a>API de REST del Portal de dispositivos para Xbox

Esta sección contiene temas de referencia para la API de REST del Portal de dispositivos para Xbox, que se usa para configurar y administrar tu consola de forma remota.

| URI        | Descripción |
|------------|-------------|
|[/API/App/PackageManager/Register](wdp-loose-folder-register-api.md)| Registra una aplicación que se encuentre en una carpeta débil. |
|[/API/App/PackageManager/Upload](wdp-folder-upload.md)| Carga una carpeta completa a la consola. |
|[/ext/App/sshpins](uwp-sshpins-api.md)| Borrar todas las anclas SSH de confianza SSH de manera remota. Será necesario realizar el emparejamiento de anclas de nuevo para el desarrollo de UWP de Visual Studio. |
|[/ext/App/deployinfo](uwp-deployinfo-api.md)| Solicita la información de implementación para uno o más paquetes instalados. |
|[/ ext/fiddler](wdp-fiddler-api.md)| Habilitar y deshabilitar el seguimiento de la red de Fiddler. |
|[/ext/httpmonitor/Sessions](wdp-httpMonitor-api.md)| Obtiene el tráfico de HTTP desde la aplicación centrada en Xbox. |
|[/ ext/networkcredential](uwp-networkcredentials-api.md)| Agrega, quita o actualiza las credenciales de red. |
|[/ ext/remoteinput](uwp-remoteinput-api.md)| Envía de manera remota la entrada del mouse, del teclado o de la controladora a una Xbox. |
|[/ext/remoteinput/Controllers](uwp-remoteinput-controllers-api.md)| Obtiene el número de controladoras físicas conectadas o desactiva todas las controladoras físicas. |
|[captura de pantalla/ext /](wdp-media-capture-api.md)| Captura una representación de PNG de la pantalla que se muestra actualmente en la consola. |
|[/ ext/configuración](wdp-xboxsettings-api.md)| Accede a la configuración del desarrollador de Xbox One. |
|[/ext/SMB/developerfolder](wdp-smb-api.md)| Accede a la carpeta de desarrollador en la consola a través del Explorador de archivos en el equipo de desarrollo. |
|[/ ext/usuario](wdp-user-management.md)| Administra los usuarios en la consola Xbox One. |
|[/ext/Xbox/Info](wdp-xboxinfo-api.md)| Proporciona información acerca del dispositivo Xbox One. |
|[/ext/xboxlive/Sandbox](wdp-sandbox-api.md)| Administra tu espacio aislado de Xbox Live. |

## <a name="see-also"></a>Consulte también

- [UWP en Xbox One](index.md)
- [Portal de dispositivos Windows](../debug-test-perf/device-portal.md)