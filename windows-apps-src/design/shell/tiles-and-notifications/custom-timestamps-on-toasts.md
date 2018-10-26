---
author: andrewleader
Description: Learn how to use custom timestamps on your toast notifications.
title: Marcas de tiempo personalizadas en notificaciones del sistema
label: Custom timestamps on toasts
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10, uwp, notificación del sistema, marca de tiempo personalizada, marca de tiempo, notificación, Centro de actividades
ms.localizationpriority: medium
ms.openlocfilehash: 290825fa079052b79fb2feaec8af928f8da93f95
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5557747"
---
# <a name="custom-timestamps-on-toasts"></a>Marcas de tiempo personalizadas en notificaciones del sistema

De manera predeterminada, la marca de tiempo de las notificaciones del sistema (visibles en el Centro de actividades)se establece en el momento en que se envió la notificación.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

Opcionalmente, puedes invalidar la marca de tiempo con tu propia fecha y hora personalizadas, de manera que la marca de tiempo represente la hora en que realmente se creó el mensaje, la información o el contenido, en lugar de la hora en que se envió la notificación. Esto también garantiza que las notificaciones aparecen en el orden correcto en el Centro de actividades (que se ordenan por hora). Se recomienda que la mayoría de las aplicaciones especifiquen una marca de tiempo personalizada.

> [!IMPORTANT]
> **Requiere CreatorsUpdate y 1.4.0 de la biblioteca de notificaciones**: debes estar ejecutando Escritorio compilación 15063 o superior para ver las marcas de hora personalizadas. Debes usar la versión 1.4.0 o superior de la [Biblioteca NuGet de notificaciones del Kit de herramientas de la comunidad de UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para asignar la marca de hora en el contenido de tu notificación del sistema.

Para usar una marca de tiempo personalizada, solo tienes que asignar la propiedad **DisplayTimestamp** en tu **ToastContent**.

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

Si usas XML, la fecha debe tener el formato en [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

> [!NOTE]
> Solo puedes usar 3 lugares decimales cómo máximo en los segundos (aunque en verdad, no hay ningún valor que proporcione nada tan detallado). Si proporcionas más, la carga no será válida y recibirás la notificación "Nueva notificación".


## <a name="usage-guidance"></a>Guía de uso

En general, se recomienda que la mayoría de las aplicaciones especifiquen una marca de tiempo personalizada. Esto garantiza que la marca de tiempo de la notificación representa de forma precisa cuándo se generó el mensaje, la información o el contenido, con independencia de retrasos en la red, modo avión o el intervalo fijo de tareas periódicas en segundo plano.

Por ejemplo, una aplicación nueva puede ejecutar una tarea en segundo plano cada 15 minutos que compruebe artículos nuevos y muestre notificaciones. Antes de las marcas de tiempo personalizadas, la marca de tiempo correspondía a cuando se generó la notificación del sistema (por lo tanto, siempre en intervalos de 15 minutos). Sin embargo, ahora la aplicación puede establecer la marca de tiempo en la hora en que el artículo se publicó realmente. De manera similar, las aplicaciones de correo electrónico y las de redes sociales pueden beneficiarse de esta característica si se usa un patrón similar de extracción periódica para sus notificaciones.

Además, al proporcionar una marca de tiempo personalizada se garantiza que la marca de tiempo es correcta, incluso si el usuario se desconectó de Internet. Por ejemplo, cuando el usuario enciende su equipo y se ejecuta tu tarea en segundo plano, por fin puedes garantizar que la marca de tiempo de las notificaciones representa la hora en que se enviaron los mensajes, en lugar de la hora en que el usuario encendió su equipo.


## <a name="default-timestamp"></a>Marca de tiempo predeterminada

Si no proporcionas una marca de tiempo personalizada, usamos la hora en que se envió la notificación.

Si envías una notificación de inserción a través de WNS, usamos la hora en que el servidor WNS recibió la notificación (de modo que ninguna latencia en la entrega de la notificación en el dispositivo afectará a la marca de tiempo).

Si has enviado una notificación local, usamos la hora en que la plataforma de notificaciones recibió la notificación (que debería ser inmediatamente).


## <a name="related-topics"></a>Temas relacionados

- [Enviar una notificación del sistema local](send-local-toast.md)
- [Documentación del contenido de la notificación del sistema](adaptive-interactive-toasts.md)