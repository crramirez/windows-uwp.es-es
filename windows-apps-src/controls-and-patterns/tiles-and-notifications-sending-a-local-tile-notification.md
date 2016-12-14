---
author: mijacobs
Description: "En este artículo se describe cómo enviar una notificación de icono local a un icono principal y un icono secundario con el uso de plantillas de iconos adaptables."
title: "Enviar una notificación de icono local"
ms.assetid: D34B0514-AEC6-4C41-B318-F0985B51AF8A
label: TBD
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: d51aacb31f41cbd9c065b013ffb95b83a6edaaf4
ms.openlocfilehash: 8fc2fc007d14bd9c5d08ca4eb7e61a2dfdf04d3b

---
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 
# <a name="send-a-local-tile-notification"></a>Enviar una notificación de icono local





Los iconos de la aplicación principal de Windows 10 se definen en el manifiesto de la aplicación, mientras que los iconos secundarios se crean mediante programación y se definen mediante el código de la aplicación. En este artículo se describe cómo enviar una notificación de icono local a un icono principal y un icono secundario con el uso de plantillas de iconos adaptables. (Una notificación local es aquella que se envía desde el código de la aplicación frente a la que se envía o extrae de un servidor web).

![icono predeterminado e icono con notificación](images/sending-local-tile-01.png)

**Nota** Obtén información sobre cómo [crear iconos adaptables](tiles-and-notifications-create-adaptive-tiles.md) y sobre el [esquema de plantillas de iconos adaptables](tiles-and-notifications-adaptive-tiles-schema.md).

 

## <a name="install-the-nuget-package"></a>Instalación del paquete NuGet


Recomendamos instalar el [paquete de NuGet de la librería de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), que simplifica las cosas al generar cargas de iconos con objetos en lugar de XML sin formato.

Los ejemplos de código en línea de este artículo son para C# usando la biblioteca de notificaciones. (Si prefieres crear tu propio XML, puedes encontrar ejemplos de código sin la librería de notificaciones al final del artículo.)

## <a name="add-namespace-declarations"></a>Agregar declaraciones de espacios de nombres


Para acceder a las API del icono, incluye el espacio de nombres [**Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661). También recomendamos incluir el espacio de nombres **NotificationsExtensions.Tiles** para que puedas aprovechar nuestras API auxiliares de iconos (debes instalar el paquete NuGet [de la librería de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para acceder a estas API).

```CSharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```

## <a name="create-the-notification-content"></a>Crear el contenido de la notificación


En Windows 10, las cargas de iconos se definen mediante plantillas de iconos adaptables, que te permiten crear diseños visuales personalizados para tus notificaciones. (Para obtener información sobre las posibilidades de los iconos adaptables, consulta los artículos [Crear iconos adaptables](tiles-and-notifications-create-adaptive-tiles.md) y [Plantillas de iconos adaptables](tiles-and-notifications-adaptive-tiles-schema.md).)

Este ejemplo de código crea el contenido de iconos adaptables para iconos medianos y anchos.

```CSharp
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// Construct the tile content
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from
                    },

                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },

                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        },

        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from,
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },

                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },

                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        }
    }
};
```

El contenido de las notificaciones es similar al siguiente cuando se muestra en un icono mediano:

![contenido de las notificaciones en un icono mediano](images/sending-local-tile-02.png)

## <a name="create-the-notification"></a>Crear la notificación


Una vez que dispongas del contenido de la notificación, debes crear un nuevo [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616). El constructor **TileNotification** toma un objeto [**XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br208620) de Windows Runtime, que puedes obtener del método **TileContent.GetXml** si estás usando [la librería de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/).

Este ejemplo de código crea una notificación para un icono nuevo.

```CSharp
// Create the tile notification
var notification = new TileNotification(content.GetXml());
```

## <a name="set-an-expiration-time-for-the-notification-optional"></a>Establecer una fecha de expiración para la notificación (opcional)


De manera predeterminada, las notificaciones de iconos y distintivos locales no expiran, mientras que las notificaciones programadas, periódicas y de inserción expiran al cabo de de tres días. El contenido de los iconos no debe persistir más de lo necesario, por lo que se considera una buena práctica establecer una fecha de caducidad apropiada para la aplicación, especialmente en las notificaciones de iconos y distintivos locales.

Este ejemplo de código crea una notificación que expira y se eliminará del icono después de diez minutos.

```CSharp
tileNotification.ExpirationTime = DateTimeOffset.UtcNow.AddMinutes(10);
```

## <a name="send-the-notification"></a>Enviar la notificación


Aunque el envío local de una notificación de iconos es sencillo, enviar la notificación a un icono principal o secundario es un poco diferente.

**Icono principal**

Para enviar una notificación a un icono principal, usa [**TileUpdateManager**](https://msdn.microsoft.com/library/windows/apps/br208622) para crear un actualizador de iconos para el icono principal y envía la notificación llamando a "Update". Independientemente de si es visible, el icono principal de tu aplicación siempre existe, por lo que puedes enviarle notificaciones aunque no esté anclado. Si el usuario ancla tu icono principal más adelante, las notificaciones que enviaste aparecerán entonces.

Este ejemplo de código envía una notificación a un icono principal.


```CSharp
// Send the notification to the primary tile
TileUpdateManager.CreateTileUpdaterForApplication().Update(notification);
```

**Icono secundario**

Para enviar una notificación a un icono secundario, asegúrate primero de que el icono secundario existe. Si intentas crear un actualizador de iconos para un icono secundario que no existe (por ejemplo, si el usuario ha desanclado el icono secundario), se generará una excepción. Puedes usar [**SecondaryTile.Exists**](https://msdn.microsoft.com/library/windows/apps/br242205) (tileId) para averiguar si tu icono secundario está anclado después crear un actualizador para el icono secundario y enviar la notificación.

Este ejemplo de código envía una notificación a un icono secundario.

```CSharp
// If the secondary tile is pinned
if (SecondaryTile.Exists("MySecondaryTile"))
{
    // Get its updater
    var updater = TileUpdateManager.CreateTileUpdaterForSecondaryTile("MySecondaryTile");
 
    // And send the notification
    updater.Update(notification);
}
```

![icono predeterminado e icono con notificación](images/sending-local-tile-01.png)

## <a name="clear-notifications-on-the-tile-optional"></a>Borrar las notificaciones en el icono (opcional)


En la mayoría de los casos, debes borrar una notificación cuando el usuario haya interactuado con ese contenido. Por ejemplo, cuando el usuario inicia tu aplicación, es posible que quieras borrar todas las notificaciones del icono. Si tus notificaciones están controladas por tiempo, te recomendamos que establezcas una fecha de caducidad en la notificación en lugar de borrarla explícitamente.

Este ejemplo de código borra la notificación de icono para el icono principal. Se puede hacer lo mismo para los iconos secundarios creando un actualizador para el icono secundario.

```CSharp
TileUpdateManager.CreateTileUpdaterForApplication().Clear();
```

En un icono con la cola de notificaciones habilitada y con notificaciones en cola, la cola se vacía cuando se llama al método Clear. Sin embargo, no puedes borrar una notificación a través del servidor de la aplicación; solo el código de la aplicación local puede borrar las notificaciones.

Las notificaciones de inserción o periódicas solo pueden agregar nuevas notificaciones o reemplazar las notificaciones existentes. Una llamada local al método Clear borrará el icono independientemente de si las notificaciones fueron de inserción, periódicas o locales. Este método no borra las notificaciones programadas que aún no han aparecido.

![icono con notificación e icono después de ser borrado](images/sending-local-tile-03.png)

## <a name="next-steps"></a>Pasos siguientes


**Uso de la cola de notificaciones**

Ahora que has realizado tu primera actualización de iconos, puedes habilitar una [cola de notificaciones](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234) para expandir la funcionalidad del icono.

**Otros métodos de entrega de notificaciones**

Este artículo te muestra cómo enviar la actualización de iconos como una notificación. Para explorar otros métodos de entrega de notificaciones, incluidos los programados, periódicos y de inserción, consulta [Entrega de notificaciones](tiles-and-notifications-choosing-a-notification-delivery-method.md).

**Método de entrega XmlEncode**

Si no estás usando la [librería de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), este método de entrega de notificaciones es otra alternativa.


```CSharp
public string XmlEncode(string text)
{
    StringBuilder builder = new StringBuilder();
    using (var writer = XmlWriter.Create(builder))
    {
        writer.WriteString(text);
    }

    return builder.ToString();
}
```

## <a name="code-examples-without-notifications-library"></a>Ejemplos de código sin librería de notificaciones


Si prefieres trabajar con XML sin formato en lugar del paquete NuGet [de la librería de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), usa estos ejemplos de código alternativos para los tres primeros ejemplos proporcionados en este artículo. El resto de los ejemplos de código se puede usar o bien con [la librería de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) o bien con XML sin formato.

Agregar declaraciones de espacios de nombres

```CSharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
```

Crear el contenido de la notificación

```CSharp
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// TODO - all values need to be XML escaped
 
 
// Construct the tile content as a string
string content = $@"
<tile>
    <visual>
 
        <binding template='TileMedium'>
            <text>{from}</text>
            <text hint-style='captionSubtle'>{subject}</text>
            <text hint-style='captionSubtle'>{body}</text>
        </binding>
 
        <binding template='TileWide'>
            <text hint-style='subtitle'>{from}</text>
            <text hint-style='captionSubtle'>{subject}</text>
            <text hint-style='captionSubtle'>{body}</text>
        </binding>
 
    </visual>
</tile>";
```

Crear la notificación

```CSharp
// Load the string into an XmlDocument
XmlDocument doc = new XmlDocument();
doc.LoadXml(content);
 
// Then create the tile notification
var notification = new TileNotification(doc);
```

## <a name="related-topics"></a>Temas relacionados


* [Crear iconos adaptables](tiles-and-notifications-create-adaptive-tiles.md)
* [Plantillas de iconos adaptables: esquema y documentación](tiles-and-notifications-adaptive-tiles-schema.md)
* [Biblioteca Notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)
* [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-tile-win10)
* [**Espacio de nombres Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661)
* [Cómo usar la cola de notificaciones (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)
* [Entrega de notificaciones](tiles-and-notifications-choosing-a-notification-delivery-method.md)
 

 







<!--HONumber=Dec16_HO1-->


