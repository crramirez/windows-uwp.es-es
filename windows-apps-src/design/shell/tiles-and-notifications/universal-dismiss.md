---
author: andrewleader
Description: Learn how to use Universal Dismiss on your toast notifications.
title: Descartar universal
label: Universal Dismiss
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10, uwp, notificación del sistema, centro de actividades en la nube, descartar universal, notificación, entre dispositivos, descartar una vez descartar en todas partes
ms.localizationpriority: medium
ms.openlocfilehash: 40a9c446172b25f2430a3f75014c8e168a91c233
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5811653"
---
# <a name="universal-dismiss"></a>Descartar universal

Descartar universal, con tecnología del Centro de actividades en la nube, significa que, al descartar una notificación de un dispositivo, también se descarta la misma notificación en tus otros dispositivos.

> [!IMPORTANT]
> **Requiere la Actualización de aniversario**: debes utilizar el SDK 14393 y estar ejecutando la compilación 14393 o superior para usar Descartar universal.

El ejemplo habitual de este escenario es avisos de calendario... tienes una aplicación de calendario en ambos dispositivos... obtienes un aviso en el teléfono y el escritorio... haces clic en descartar en el escritorio... gracias a Descartar universal, ¡también se descarta el aviso en el teléfono! **¡Habilitar Descartar universal solo requiere una línea de código!**

<img alt="Diagram of Universal Dismiss" src="images/universal-dismiss.gif" width="406"/>

En este escenario, el hecho de clave es que **la misma aplicación se instala en varios dispositivos**, lo que significa que **cada dispositivo ya está recibiendo notificaciones**. Una aplicación de calendario es el ejemplo icónico, ya que normalmente se instala la misma aplicación de calendario tanto en tu PC con Windows como en tu teléfono, y cada instancia de la aplicación ya te envía recordatorios en cada dispositivo. Al agregar compatibilidad con Descartar universal, esas instancias de los mismos recordatorios se pueden vincular en todos los dispositivos.


## <a name="how-to-enable-universal-dismiss"></a>Cómo habilitar Descartar universal

Como desarrollador, es muy fácil habilitar Descartar universal. Solo tienes que proporcionar un id. que nos permita vincular cada notificación en todos los dispositivos, para que cuando el usuario descarte una notificación de un dispositivo, se descarte la notificación vinculada correspondiente desde el otro dispositivo.

![Diagrama de RemoteId de Descartar universal](images/universal-dismiss-remoteid.jpg)

> **RemoteId**: identificador que identifica de manera única una notificación *en todos los dispositivos*.

T solo toma una línea de código para agregar RemoteId, ¡habilitando la compatibilidad para Descartar universal! La manera de generar tu RemoteId depende de ti. Sin embargo, debes asegurarte de que identifica de manera única tu notificación en todos los dispositivos y que se puede generar el mismo identificador desde distintas instancias de la aplicación que se ejecuta en diferentes dispositivos.

Por ejemplo, en mi aplicación de planificador de deberes, genero mi RemoteId diciendo que es del tipo "recordatorio" y, a continuación, incluyo el id. de la cuenta en línea y el identificador en línea del elemento de deberes. Puedo generar de forma coherente el mismo RemoteId exacto, con independencia del dispositivo que envía la notificación, dado que estos id. en línea se comparten entre los dispositivos.

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

El siguiente código se ejecuta tanto en mi teléfono como en la aplicación de escritorio, lo que significa que la notificación en ambos dispositivos tendrán el mismo RemoteId.

¡Eso es todo lo que tienes que hacer! Cuando el usuario descarte (o haga clic en) una notificación, comprobaremos si tiene un RemoteId y, de ser así, distribuiremos un descarte de ese RemoteId en todos los dispositivos del usuario.

**Problema conocido**: la recuperación del **RemoteId** a través de la API de `ToastNotificationHistory.GetHistory()` siempre devolverá una cadena vacía en lugar del **RemoteId** especificado. No te preocupes, todo es funcional; solo se trata de recuperar el valor que se ha roto.

> [!NOTE]
> Si el usuario o la empresa deshabilita la [creación de reflejo de notificación](notification-mirroring.md) para tu aplicación (o deshabilita por completo la creación de reflejo de notificación), Descartar universal no funcionará, puesto que no tenemos tus notificaciones en la nube.


## <a name="supported-devices"></a>Dispositivos compatibles

Desde la Actualización de aniversario, Descartar universal se admite en Windows Mobile y Escritorio de Windows. Descartar universal funciona en ambas direcciones, entre PC y PC, PC y teléfono, y teléfono y teléfono.