---
title: Descartar Universal
description: Obtenga información sobre cómo usar el descartado universal para descartar una notificación del sistema de un dispositivo y hacer que se descarte la misma notificación en otros dispositivos.
label: Universal Dismiss
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, notificación del sistema, centro de actividades en la nube, descarte universal, notificación, dispositivo cruzado, descartar una vez descartadas
ms.localizationpriority: medium
ms.openlocfilehash: fff9315b9a5645d8a981ca899c1c37acb04de07c
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043457"
---
# <a name="universal-dismiss"></a>Descartar Universal

Descartado universal, con el centro de actividades en la nube, significa que cuando se descarta una notificación de un dispositivo, también se descarta la misma notificación en los demás dispositivos.

> [!IMPORTANT]
> **Requiere actualización de aniversario**: debe tener como destino el SDK 14393 y ejecutar la compilación 14393 o superior para usar el descartado universal.

El ejemplo común de este escenario son los recordatorios de calendario... tiene una aplicación de calendario en ambos dispositivos... recibirá un recordatorio en el teléfono y en el escritorio... Haga clic en descartar en el escritorio... Gracias al descarte universal, el recordatorio del teléfono también se descarta. **La habilitación del descartado universal solo requiere una línea de código.**

<img alt="Diagram of Universal Dismiss" src="images/universal-dismiss.gif" width="406"/>

En este escenario, el hecho clave es que **la misma aplicación se instala en varios dispositivos**, lo que significa que **cada dispositivo ya recibe notificaciones**. Una aplicación de calendario es el ejemplo de iconos, ya que normalmente tiene la misma aplicación de calendario instalada en el equipo Windows y en el teléfono, y cada instancia de la aplicación ya le envía recordatorios en cada dispositivo. Al agregar compatibilidad con descartado universal, esas instancias de los mismos recordatorios se pueden vincular entre dispositivos.


## <a name="how-to-enable-universal-dismiss"></a>Cómo habilitar el descartado universal

Como desarrollador, la habilitación del descarte universal es muy fácil. Solo tiene que proporcionar un identificador que nos permita vincular cada notificación entre los dispositivos, de modo que cuando el usuario descarte una notificación de un dispositivo, se descartará la notificación vinculada correspondiente del otro dispositivo.

![Diagrama RemoteId de descarte universal](images/universal-dismiss-remoteid.jpg)

> **RemoteId**: un identificador que identifica de forma única una notificación *en todos los dispositivos*.

t solo toma una línea de código para agregar RemoteId, lo que habilita la compatibilidad con el descartado universal. No obstante, el modo en que se genera el RemoteId depende de usted, por lo que debe asegurarse de que identifica de forma única la notificación en todos los dispositivos y de que se puede generar el mismo identificador a partir de diferentes instancias de la aplicación que se ejecutan en distintos dispositivos.

Por ejemplo, en mi aplicación de agenda de trabajos, geneme mi RemoteId diciendo que es de tipo "recordatorio" y, a continuación, he incluido el identificador de cuenta en línea y el identificador en línea del elemento de deberes. Puedo generar de forma coherente exactamente el mismo RemoteId, independientemente del dispositivo que envíe la notificación, ya que estos identificadores en línea se comparten entre los dispositivos.

```csharp
var toast = new ScheduledToastNotification(content.GetXml(), startTime);
 
// If the RemoteId property is present
if (ApiInformation.IsPropertyPresent(typeof(ScheduledToastNotification).FullName, nameof(ScheduledToastNotification.RemoteId)))
{
    // Assign the RemoteId to add support for Universal Dismiss
    toast.RemoteId = $"reminder_{account.AccountId}_{homework.Identifier}"
}
  
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```

El código siguiente se ejecuta en mi aplicación de escritorio y teléfono, lo que significa que la notificación en ambos dispositivos tendrá el mismo RemoteId.

Eso es todo lo que tiene que hacer. Cuando el usuario descarte (o haga clic en) una notificación, se comprobará si tiene un RemoteId y, en caso afirmativo, se descartará un descartado de ese RemoteId en todos los dispositivos del usuario.

**Problema conocido**: la recuperación de **RemoteId** a través de la `ToastNotificationHistory.GetHistory()` API siempre devolverá una cadena vacía en lugar de la **RemoteId** especificada. No se preocupe, todo es funcional, solo recupera el valor que se ha interrumpido.

> [!NOTE]
> Si el usuario o la empresa deshabilita la [creación de reflejo](notification-mirroring.md) de la notificación de la aplicación (o deshabilita por completo la creación de reflejo de la notificación), el descartado universal no funcionará, ya que no tenemos las notificaciones en la nube.


## <a name="supported-devices"></a>Dispositivos compatibles

Desde la actualización de aniversario, se admite el descartado universal en Windows Mobile y el escritorio de Windows. El descarte universal funciona en ambas direcciones, entre PC PC, PC-Phone y teléfono.