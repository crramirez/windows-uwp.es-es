---
Description: Learn how to use tiles, badges, toasts, and notifications to provide entry points into your app and keep users up-to-date.
title: Notificaciones de distintivo para aplicaciones para UWP
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b41069f1ca964f978ba0cff73f5c34a9db133a10
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8487227"
---
# <a name="badge-notifications-for-uwp-apps"></a>Notificaciones de distintivo para aplicaciones para UWP

 

<div style="float:left; font-size:80%; text-align:left; margin: 0px 15px 15px 0px;">
<img src="images/badge-example.png" alt="A tile with a numeric badge displaying the number 63 to indicate 63 unread mails." style="padding-bottom:0.0em; margin-bottom: 2px" /><br/>Un icono con una notificación numérica que muestra<br/> el número 63 para indicar que hay 63 correos electrónicos sin leer.</div>

Las notificaciones transmiten información de estado o resumen de la aplicación y son específicos de la aplicación. Las notificaciones ser numéricas (1-99) o pueden formar parte de un conjunto de glifos proporcionados por el sistema. Algunos ejemplos de la información que se transmite mejor mediante una notificación son el estado de conexión de red de un juego en línea, el estado de un usuario de una aplicación de mensajería, la cantidad de correos electrónicos sin leer en una aplicación de correo electrónico y la cantidad de entradas nuevas en una aplicación de medios sociales. 

Las notificaciones aparecen en el icono de barra de tareas de la aplicación y en la esquina inferior derecha de su ventana de Inicio, independientemente de si la aplicación está en ejecución. Las notificaciones se pueden mostrar en ventanas de todos los tamaños.  

> [!NOTE]
> No puedes proporcionar tu propia imagen de notificación (solo se pueden usar las imágenes de notificación proporcionadas por el sistema).


## <a name="numeric-badges"></a>Notificaciones numéricas

<table>
    <tr>
        <th>Valor</th>
        <th>Notificación</th>
        <th>XML</th>
    </tr>
    <tr>
        <td>Un número de 1 a 99. Un valor de 0 es equivalente al valor de glifo "ninguno" y borrará la notificación.</td>
        <td><img src="images/badges/badge-numeric.png" alt="A numeric badge less than 100." /></td>
        <td>`<badge value="1"/>`</td>
    </tr>
    <tr>
        <td>Cualquier número superior a 99.</td>
        <td><img src="images/badges/badge-numeric-greater.png" alt="A numeric badge greater than 99." /></td></td>
        <td>`<badge value="100"/>`</td>
    </tr>    
</table>

## <a name="glyph-badges"></a>Notificaciones de glifo
En lugar de un número, una notificación puede mostrar un elemento de un conjunto no extensible de glifos de estado. 

<table>
<tr>
    <th>Estado</th>
    <th>Glifo</th>
    <th>XML</th>
</tr>
<tr>
    <td>ninguno</td>
    <td>(No se muestran notificaciones.)</td>
    <td>`<badge value="none"/>`</td>
</tr>
<tr>
    <td>actividad</td>
    <td><img src="images/badges/badge-activity.png" alt="Glyph" /></td>
    <td>`<badge value="activity"/>`</td>
</tr>
<tr>
    <td>alarma</td>
    <td><img src="images/badges/badge-alarm.png" alt="Glyph" /></td>
    <td>`<badge value="alarm"/>`</td>
</tr>
<tr>
    <td>alerta</td>
    <td><img src="images/badges/badge-alert.png" alt="Glyph" /></td>
    <td>`<badge value="alert"/>`</td>
</tr>
<tr>
    <td>atención</td>
    <td><img src="images/badges/badge-attention.png" alt="Glyph" /></td>
    <td>`<badge value="attention"/>`</td>
</tr>
<tr>
    <td>disponible</td>
    <td><img src="images/badges/badge-available.png" alt="Glyph" /></td>
    <td>`<badge value="available"/>`</td>
</tr>
<tr>
    <td>ausente</td>
    <td><img src="images/badges/badge-away.png" alt="Glyph" /></td>
    <td>`<badge value="away"/>`</td>
</tr>
<tr>
    <td>ocupado</td>
    <td><img src="images/badges/badge-busy.png" alt="Glyph" /></td>
    <td>`<badge value="busy"/>`</td>
</tr>
<tr>
    <td>error</td>
    <td><img src="images/badges/badge-error.png" alt="Glyph" /></td>
    <td>`<badge value="error"/>`</td>
</tr>
<tr>
    <td>newMessage</td>
    <td><img src="images/badges/badge-newMessage.png" alt="Glyph" /></td>
    <td>`<badge value="newMessage"/>`</td>
</tr>
<tr>
    <td>en pausa</td>
    <td><img src="images/badges/badge-paused.png" alt="Glyph" /></td>
    <td>`<badge value="paused"/>`</td>
</tr>
<tr>
    <td>reproducción</td>
    <td><img src="images/badges/badge-playing.png" alt="Glyph" /></td>
    <td>`<badge value="playing"/>`</td>
</tr>
<tr>
    <td>no disponible</td>
    <td><img src="images/badges/badge-unavailable.png" alt="Glyph" /></td>
    <td>`<badge value="unavailable"/>`</td>
</tr>
</table>

## <a name="create-a-badge"></a>Crear una notificación

Estos ejemplos muestran cómo crear una actualización de notificación.

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

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

* [Muestra de notificaciones](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/Notifications)<br/> Muestra cómo crear iconos dinámicos, enviar actualizaciones de notificación y mostrar notificaciones del sistema. 

## <a name="related-articles"></a>Artículos relacionados

* [Notificaciones del sistema interactivas y adaptables](adaptive-interactive-toasts.md)
* [Crear iconos](creating-tiles.md)
* [Crear iconos adaptables](create-adaptive-tiles.md)
