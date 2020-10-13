---
description: Aprende a usar iconos, distintivos, notificaciones del sistema y notificaciones para proporcionar puntos de entrada en la aplicación y mantener actualizados a los usuarios.
title: Notificaciones de distintivos para aplicaciones de Windows
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a49d771b7efdbb7e787db0cbadea45c255a1120e
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984601"
---
# <a name="badge-notifications-for-windows-apps"></a>Notificaciones de distintivos para aplicaciones de Windows

 

<div style="float:left; font-size:80%; text-align:left; margin: 0px 15px 15px 0px;">
<img src="images/badge-example.png" alt="A tile with a numeric badge displaying the number 63 to indicate 63 unread mails." style="padding-bottom:0.0em; margin-bottom: 2px" /><br/>Un icono con una notificación numérica que muestra<br/> el número 63 para indicar que hay 63 correos electrónicos sin leer.</div>

Las notificaciones transmiten información de estado o resumen de la aplicación y son específicos de la aplicación. Las notificaciones ser numéricas (1-99) o pueden formar parte de un conjunto de glifos proporcionados por el sistema. Algunos ejemplos de la información que se transmite mejor mediante una notificación son el estado de conexión de red de un juego en línea, el estado de un usuario de una aplicación de mensajería, la cantidad de correos electrónicos sin leer en una aplicación de correo electrónico y la cantidad de entradas nuevas en una aplicación de medios sociales. 

Las notificaciones aparecen en el icono de barra de tareas de la aplicación y en la esquina inferior derecha de su ventana de Inicio, independientemente de si la aplicación está en ejecución. Las notificaciones se pueden mostrar en ventanas de todos los tamaños.  

> [!NOTE]
> No puedes proporcionar tu propia imagen de notificación (solo se pueden usar las imágenes de notificación proporcionadas por el sistema).


## <a name="numeric-badges"></a>Notificaciones numéricas

Value | Distintivo | XML
--|--|--
Un número de 1 a 99. Un valor de 0 es equivalente al valor de glifo "ninguno" y borrará la notificación. | <img src="images/badges/badge-numeric.png" alt="A numeric badge less than 100." /> | `<badge value="1"/>`
Cualquier número superior a 99. | <img src="images/badges/badge-numeric-greater.png" alt="A numeric badge greater than 99." /></td> | `<badge value="100"/>`

## <a name="glyph-badges"></a>Notificaciones de glifo
En lugar de un número, una notificación puede mostrar un elemento de un conjunto no extensible de glifos de estado. 

Estado | Glifo | XML
--|--|--
ninguno | (No se muestran notificaciones.) | `<badge value="none"/>`
activity | <img src="images/badges/badge-activity.png" alt="Glyph" /> | `<badge value="activity"/>`
alarma | <img src="images/badges/badge-alarm.png" alt="Glyph" /> | `<badge value="alarm"/>`
alerta | <img src="images/badges/badge-alert.png" alt="Glyph" /> | `<badge value="alert"/>`
atención | <img src="images/badges/badge-attention.png" alt="Glyph" /> | `<badge value="attention"/>`
disponible | <img src="images/badges/badge-available.png" alt="Glyph" /> | `<badge value="available"/>`
ausente | <img src="images/badges/badge-away.png" alt="Glyph" /> | `<badge value="away"/>`
ocupado | <img src="images/badges/badge-busy.png" alt="Glyph" /> | `<badge value="busy"/>`
error | <img src="images/badges/badge-error.png" alt="Glyph" /> | `<badge value="error"/>`
newMessage | <img src="images/badges/badge-newMessage.png" alt="Glyph" /> | `<badge value="newMessage"/>`
en pausa | <img src="images/badges/badge-paused.png" alt="Glyph" /> | `<badge value="paused"/>`
reproducción | <img src="images/badges/badge-playing.png" alt="Glyph" /> | `<badge value="playing"/>`
no disponible | <img src="images/badges/badge-unavailable.png" alt="Glyph" /> | `<badge value="unavailable"/>`</td>

## <a name="create-a-badge"></a>Crear una notificación

En estos ejemplos se muestra cómo crear una actualización de distintivo.

### <a name="create-a-numeric-badge"></a>Crear una notificación numérica

````csharp
private void setBadgeNumber(int num)
{

    // Get the blank badge XML payload for a badge number
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeNumber);

    // Set the value of the badge in the XML to our number
    XmlElement badgeElement = badgeXml.SelectSingleNode("/badge") as XmlElement;
    badgeElement.SetAttribute("value", num.ToString());

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### <a name="create-a-glyph-badge"></a>Crear una notificación de glifo
````csharp
private void updateBadgeGlyph()
{
    string badgeGlyphValue = "alert";

    // Get the blank badge XML payload for a badge glyph
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeGlyph);

    // Set the value of the badge in the XML to our glyph value
    Windows.Data.Xml.Dom.XmlElement badgeElement = 
        badgeXml.SelectSingleNode("/badge") as Windows.Data.Xml.Dom.XmlElement;
    badgeElement.SetAttribute("value", badgeGlyphValue);

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### <a name="clear-a-badge"></a>Borrar una notificación

````csharp
private void clearBadge()
{
    BadgeUpdateManager.CreateBadgeUpdaterForApplication().Clear();
}
````

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

* [Muestra de notificaciones](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)<br/> Muestra cómo crear iconos dinámicos, enviar actualizaciones de notificación y mostrar notificaciones del sistema. 

## <a name="related-articles"></a>Artículos relacionados

* [Notificaciones del sistema interactivas y adaptables](adaptive-interactive-toasts.md)
* [Crear iconos](creating-tiles.md)
* [Crear iconos adaptables](create-adaptive-tiles.md)
