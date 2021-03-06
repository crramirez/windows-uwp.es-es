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
ms.openlocfilehash: 64d5595bcc315b24228401ff23a1a59d29282eae
ms.sourcegitcommit: cbdfac0e2d8bead6c225e815e7d6dffe1f5ef864
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344967"
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

Status | Glifo | XML
--|--|--
ninguno | (No se muestran notificaciones.) | `<badge value="none"/>`
activity | :::image type="icon" source="images/badges/badge-activity.png"::: | `<badge value="activity"/>`
alarma | :::image type="icon" source="images/badges/badge-alarm.png"::: | `<badge value="alarm"/>`
alerta | :::image type="icon" source="images/badges/badge-alert.png"::: | `<badge value="alert"/>`
atención | :::image type="icon" source="images/badges/badge-attention.png"::: | `<badge value="attention"/>`
disponible | :::image type="icon" source="images/badges/badge-available.png"::: | `<badge value="available"/>`
ausente | :::image type="icon" source="images/badges/badge-away.png"::: | `<badge value="away"/>`
ocupado | :::image type="icon" source="images/badges/badge-busy.png"::: | `<badge value="busy"/>`
error | :::image type="icon" source="images/badges/badge-error.png"::: | `<badge value="error"/>`
newMessage | :::image type="icon" source="images/badges/badge-newMessage.png"::: | `<badge value="newMessage"/>`
en pausa | :::image type="icon" source="images/badges/badge-paused.png"::: | `<badge value="paused"/>`
reproducción | :::image type="icon" source="images/badges/badge-playing.png"::: | `<badge value="playing"/>`
no disponible | :::image type="icon" source="images/badges/badge-unavailable.png"::: | `<badge value="unavailable"/>`</td>

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
