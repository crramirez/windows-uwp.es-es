---
description: Obtenga información sobre cómo usar el audio personalizado en las notificaciones del sistema para que la aplicación exprese los efectos de sonido únicos de la marca.
title: Audio personalizado en notificaciones del sistema
label: Custom audio on toasts
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, notificación del sistema, audio personalizado, notificación, audio, sonido
ms.localizationpriority: medium
ms.openlocfilehash: d2d32b9545cccfb25790d394aec028fd29904ca5
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984411"
---
# <a name="custom-audio-on-toasts"></a>Audio personalizado en notificaciones del sistema

Las notificaciones del sistema pueden usar el audio personalizado, que permite a la aplicación expresar los efectos de sonido únicos de la marca. Por ejemplo, una aplicación de mensajería puede usar su propio sonido de mensajería en sus notificaciones del sistema, por lo que el usuario puede saber al instante que ha recibido una notificación de la aplicación, en lugar de oír el sonido de notificación genérico.

## <a name="install-uwp-community-toolkit-nuget-package"></a>Instalar el paquete NuGet de UWP Community Toolkit

Para crear notificaciones a través de código, se recomienda encarecidamente usar la biblioteca de notificaciones del kit de herramientas de la comunidad de UWP, que proporciona un modelo de objetos para el contenido XML de notificación. Podría construir manualmente el XML de notificación, pero esto es propenso a errores y es confuso. El equipo que posee notificaciones en Microsoft crea y mantiene la biblioteca de notificaciones dentro del kit de herramientas de la comunidad de UWP.

Instale [Microsoft. Toolkit. UWP. notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) desde NuGet.


## <a name="add-namespace-declarations"></a>Incorporación de declaraciones de espacio de nombres

`Windows.UI.Notifications` incluye el icono y la API del sistema. `Microsoft.Toolkit.Uwp.Notifications` incluye la biblioteca de notificaciones.

```csharp
using Microsoft.Toolkit.Uwp.Notifications;
using Windows.UI.Notifications;
```


## <a name="add-the-custom-audio"></a>Adición del audio personalizado

Windows Mobile siempre ha admitido el audio personalizado en las notificaciones del sistema. Sin embargo, Desktop solo agregó compatibilidad con audio personalizado en la versión 1511 (compilación 10586). Si envía una notificación del sistema que contiene audio personalizado a un dispositivo de escritorio antes de la versión 1511, la notificación del sistema será silenciosa. Por lo tanto, para la versión de escritorio anterior a la 1511, no debe incluir el audio personalizado en la notificación del sistema, por lo que la notificación usará al menos el sonido de notificación predeterminado.

**Problema conocido**: Si usa la versión de escritorio 1511, el audio del sistema de notificación personalizado solo funcionará si la aplicación se instala a través de la tienda. Esto significa que no puede probar localmente el audio personalizado en el escritorio antes de enviarlo a la tienda, pero el audio funcionará correctamente una vez que se haya instalado desde la tienda. Este problema se ha corregido en la actualización de aniversario, de modo que el audio personalizado de la aplicación implementada localmente funcione correctamente.

```csharp
var contentBuilder = new ToastContentBuilder()
    .AddText("New message");

    
bool supportsCustomAudio = true;
 
// If we're running on Desktop before Version 1511, do NOT include custom audio
// since it was not supported until Version 1511, and would result in a silent toast.
if (AnalyticsInfo.VersionInfo.DeviceFamily.Equals("Windows.Desktop")
    && !ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 2))
{
    supportsCustomAudio = false;
}
 
if (supportsCustomAudio)
{
    contentBuilder.AddAudio(new Uri("ms-appx:///Assets/Audio/CustomToastAudio.m4a"));
}

// TODO: Send the toast
```

Entre los tipos de archivo de audio admitidos se incluyen...

- .aac
- . flac
- .m4a
- .mp3
- .wav
- .wma


## <a name="send-the-notification"></a>Enviar la notificación

Enviar un notificaciones con audio es lo mismo que enviar una notificación normal. Consulte [envío de una notificación del sistema local](send-local-toast.md) para obtener más información.


## <a name="related-topics"></a>Temas relacionados

- [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/quickstart-toast-with-custom-audio)
- [Enviar una notificación del sistema local](send-local-toast.md)
- [Documentación del contenido del sistema](adaptive-interactive-toasts.md)