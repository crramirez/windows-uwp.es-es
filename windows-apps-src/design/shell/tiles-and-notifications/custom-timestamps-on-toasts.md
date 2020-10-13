---
title: Marcas de tiempo personalizadas en notificaciones del sistema
description: Obtenga información sobre cómo invalidar la marca de tiempo predeterminada en una notificación del sistema con una marca de tiempo personalizada que indique cuándo se generó el mensaje, la información y el contenido.
label: Custom timestamps on toasts
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, notificación del sistema, marca de tiempo personalizada, marca de tiempo, notificación, centro de actividades
ms.localizationpriority: medium
ms.openlocfilehash: 23e5337fe2ce30e1172aedc034a8b4eb3821a615
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984431"
---
# <a name="custom-timestamps-on-toasts"></a>Marcas de tiempo personalizadas en notificaciones del sistema

De forma predeterminada, la marca de tiempo en las notificaciones del sistema (visibles dentro del centro de actividades) se establece en el momento en que se envió la notificación.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

Opcionalmente, puede invalidar la marca de tiempo con su propia fecha y hora personalizadas, de modo que la marca de tiempo represente la hora en que se creó realmente el mensaje/información/contenido, en lugar de la hora a la que se envió la notificación. Esto también garantiza que las notificaciones aparezcan en el orden correcto dentro del centro de actividades (que están ordenadas por tiempo). Se recomienda que la mayoría de las aplicaciones especifiquen una marca de tiempo personalizada.

> [!IMPORTANT]
> **Requiere Creators Update y 1.4.0 de la biblioteca de notificaciones**: debe ejecutar la compilación 15063 o superior para ver las marcas de tiempo personalizadas. Debe usar la versión 1.4.0 o superior de la [biblioteca NuGet de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) de la comunidad de UWP para asignar la marca de tiempo en el contenido de la notificación del sistema.

Para usar una marca de tiempo personalizada, basta con asignar la propiedad **DisplayTimestamp** en **ToastContent**.

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
var content = new ToastContent()
    .AddCustomTimeStamp(new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc))
    ...
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

---

Si usa XML, la fecha debe tener el formato [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

> [!NOTE]
> Solo puede usar como máximo 3 posiciones decimales en los segundos (aunque de forma realista no hay ningún valor para proporcionar todo lo que sea granular). Si proporciona más, la carga no será válida y recibirá la notificación "nueva notificación".


## <a name="usage-guidance"></a>Guía de uso

En general, se recomienda que la mayoría de las aplicaciones especifiquen una marca de tiempo personalizada. Esto garantiza que la marca de tiempo de la notificación representa con precisión cuándo se generó el mensaje/información/contenido, independientemente de los retrasos de la red, el modo de avión o el intervalo fijo de tareas en segundo plano periódicas.

Por ejemplo, una aplicación de noticias puede ejecutar una tarea en segundo plano cada 15 minutos, que busca nuevos artículos y muestra notificaciones. Antes de las marcas de tiempo personalizadas, la marca de tiempo se correspondía a cuando se generó la notificación del sistema (por lo tanto, siempre en intervalos de 15 minutos). Sin embargo, ahora la aplicación puede establecer la marca de tiempo en el momento en que se publicó el artículo. Del mismo modo, las aplicaciones de correo electrónico y las aplicaciones de red social pueden beneficiarse de esta característica si se usa un patrón similar de extracción periódica para sus notificaciones.

Además, si se proporciona una marca de tiempo personalizada, se asegura de que la marca de tiempo es correcta aunque el usuario se haya desconectado de Internet. Por ejemplo, cuando el usuario activa el equipo y se ejecuta la tarea en segundo plano, puede asegurarse de que la marca de tiempo de las notificaciones representa la hora a la que se enviaron los mensajes, en lugar de la hora en que el usuario encendió el equipo.


## <a name="default-timestamp"></a>Marca de tiempo predeterminada

Si no proporciona una marca de tiempo personalizada, usamos la hora a la que se envió la notificación.

Si ha enviado una notificación de envío a través de WNS, usamos el tiempo en el que el servidor WNS recibió la notificación (por lo que cualquier latencia en la entrega de la notificación al dispositivo no afectará a la marca de tiempo).

Si ha enviado una notificación local, usamos el momento en que la plataforma de notificación ha recibido la notificación (que debe ser inmediatamente).


## <a name="related-topics"></a>Temas relacionados

- [Enviar una notificación del sistema local](send-local-toast.md)
- [Documentación del contenido del sistema](adaptive-interactive-toasts.md)