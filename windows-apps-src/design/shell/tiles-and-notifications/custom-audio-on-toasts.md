---
author: andrewleader
Description: Learn how to use custom audio on your toast notifications.
title: Audio personalizado en notificaciones del sistema
label: Custom audio on toasts
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, notificación del sistema, audio personalizado, notificación, audio, sonido
ms.localizationpriority: medium
ms.openlocfilehash: 24be148340366163fcab00ec75f7f26fac6c2d80
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "3987350"
---
# <a name="custom-audio-on-toasts"></a>Audio personalizado en notificaciones del sistema

Las notificaciones del sistema pueden usar audio personalizado, lo permite a tu aplicación expresar los efectos de sonido únicos de tu marca. Por ejemplo, una aplicación de mensajería puede usar su propio sonido de mensajería en sus notificaciones del sistema de manera que el usuario pueda saber al instante que ha recibido una notificación de la aplicación, en lugar de oír el sonido de notificación genérico.

## <a name="install-uwp-community-toolkit-nuget-package"></a>Instalar el paquete NuGet del Kit de herramientas de la comunidad de UWP

Para crear notificaciones mediante código, se recomienda encarecidamente usar la biblioteca de notificaciones del kit de herramientas de la comunidad de UWP, que proporciona un modelo de objeto para el contenido XML de notificación. Podrías crear manualmente el XML de notificación, pero es propenso a errores y resulta complicado. La biblioteca de notificaciones del Kit de herramientas de la comunidad de UWP se compila y se mantiene por el equipo que tiene la propiedad de las notificaciones de Microsoft.

Instala [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) desde NuGet (estamos usando la versión 1.0.0 en esta documentación).


## <a name="add-namespace-declarations"></a>Agregar declaraciones de espacios de nombres

`Windows.UI.Notifications` incluye las API de notificaciones del sistema e iconos. `Microsoft.Toolkit.Uwp.Notifications` incluye la biblioteca de notificaciones.

```csharp
using Microsoft.Toolkit.Uwp.Notifications;
using Windows.UI.Notifications;
```


## <a name="construct-the-notification"></a>Construir la notificación

El contenido de las notificaciones del sistema incluye texto e imágenes, además de botones y entradas. Consulta [enviar notificaciones del sistema local](send-local-toast.md) para ver un fragmento de código completo.

```csharp
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        ... (omitted)
    }
};
```


## <a name="add-the-custom-audio"></a>Agregar el audio personalizado

Windows Mobile siempre ha admitido audio personalizado en las notificaciones del sistema. Sin embargo, Escritorio solo ha agregado compatibilidad para audio personalizado en la versión 1511 (compilación 10586). Si envías una notificación del sistema que contenga audio personalizado a un dispositivo de escritorio anterior a la versión 1511, se silenciará la notificación del sistema. Por lo tanto, para Escritorio versión preliminar 1511, NO debes incluir el audio personalizado en la notificación del sistema, para que la notificación del sistema use al menos el sonido de la notificación predeterminado.

**Problema conocido**: Si estás usando Escritorio versión 1511, el audio personalizado de la notificación del sistema solo funcionará si tu aplicación se ha instalado a través de la Store. Eso significa que no puedes probar localmente tu audio personalizado en Escritorio antes de enviarlo a la Store, pero el audio funcionará correctamente una vez instalado desde la Store. Hemos corregido esto en la Actualización de aniversario, de manera que el audio personalizado desde tu aplicación implementada localmente funcionará correctamente.

```csharp
?
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
    toastContent.Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/Audio/CustomToastAudio.m4a")
    };
}
```

Los tipos de archivo de audio admitidos son...

- .aac
- .flac
- .m4a
- .mp3
- .wav
- .wma


## <a name="send-the-notification"></a>Enviar la notificación

Ahora que se ha finalizado el contenido de la notificación del sistema, el envío de la notificación es bastante sencillo.

```csharp
// Create the toast notification from the previous toast content
ToastNotification notification = new ToastNotification(toastContent.GetXml());
             
// And then send the toast
ToastNotificationManager.CreateToastNotifier().Show(notification);
```


## <a name="related-topics"></a>Temas relacionados

- [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/quickstart-toast-with-custom-audio)
- [Enviar una notificación del sistema local](send-local-toast.md)
- [Documentación del contenido de la notificación del sistema](adaptive-interactive-toasts.md)