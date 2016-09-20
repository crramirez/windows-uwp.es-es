---
author: mijacobs
Description: "En este artículo se describe cómo enviar una notificación de icono local a un icono principal y un icono secundario con el uso de plantillas de iconos adaptables."
title: "Enviar una notificación de icono local"
ms.assetid: D34B0514-AEC6-4C41-B318-F0985B51AF8A
label: TBD
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: cc2f86f2a56aae5ee9e3019dafa3417a25e7d610

---

# Enviar una notificación de icono local





Los iconos de la aplicación principal de Windows 10 se definen en el manifiesto de la aplicación, mientras que los iconos secundarios se crean mediante programación y se definen mediante el código de la aplicación. En este artículo se describe cómo enviar una notificación de icono local a un icono principal y un icono secundario con el uso de plantillas de iconos adaptables. (Una notificación local es aquella que se envía desde el código de la aplicación frente a la que se envía o extrae de un servidor web).

![icono predeterminado e icono con notificación](images/sending-local-tile-01.png)


            **Nota** Obtén información sobre cómo [crear iconos adaptables](tiles-and-notifications-create-adaptive-tiles.md) y sobre el [esquema de plantillas de iconos adaptables](tiles-and-notifications-adaptive-tiles-schema.md).

 

## <span id="Install_the_NuGet_package"></span><span id="install_the_nuget_package"></span><span id="INSTALL_THE_NUGET_PACKAGE"></span>Instalación del paquete NuGet


Recomendamos instalar el [paquete NotificationsExtensions NuGet](https://www.nuget.org/packages/NotificationsExtensions.Win10/), que simplifica las cosas al generar cargas de iconos con objetos en lugar de XML sin formato.

Los ejemplos de código en línea en este artículo son para C# con el paquete NuGet [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) instalado. (Si prefieres crear tu propio XML, puedes encontrar ejemplos de código sin [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) al final del artículo.)

## <span id="Add_namespace_declarations"></span><span id="add_namespace_declarations"></span><span id="ADD_NAMESPACE_DECLARATIONS"></span>Agregar declaraciones de espacios de nombres


Para acceder a las API del icono, incluye el espacio de nombres [**Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661). También recomendamos incluir el espacio de nombres **NotificationsExtensions.Tiles** para que puedas aprovechar nuestras API auxiliares de iconos (debes instalar el paquete NuGet [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) para acceder a estas API).

```
using Windows.UI.Notifications;
using NotificationsExtensions.Tiles; // NotificationsExtensions.Win10
```

## <span id="Create_the_notification_content"></span><span id="create_the_notification_content"></span><span id="CREATE_THE_NOTIFICATION_CONTENT"></span>Crear el contenido de la notificación


En Windows 10, las cargas de iconos se definen mediante plantillas de iconos adaptables, que te permiten crear diseños visuales personalizados para tus notificaciones. (Para obtener información sobre las posibilidades de los iconos adaptables, consulta los artículos [Crear iconos adaptables](tiles-and-notifications-create-adaptive-tiles.md) y [Plantillas de iconos adaptables](tiles-and-notifications-adaptive-tiles-schema.md).)

Este ejemplo de código crea el contenido de iconos adaptables para iconos medianos y anchos.

```
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
                    new TileText()
                    {
                        Text = from
                    },
 
                    new TileText()
                    {
                        Text = subject,
                        Style = TileTextStyle.CaptionSubtle
                    },
 
                    new TileText()
                    {
                        Text = body,
                        Style = TileTextStyle.CaptionSubtle
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
                    new TileText()
                    {
                        Text = from,
                        Style = TileTextStyle.Subtitle
                    },
 
                    new TileText()
                    {
                        Text = subject,
                        Style = TileTextStyle.CaptionSubtle
                    },
 
                    new TileText()
                    {
                        Text = body,
                        Style = TileTextStyle.CaptionSubtle
                    }
                }
            }
        }
    }
};
```

El contenido de las notificaciones es similar al siguiente cuando se muestra en un icono mediano:

![contenido de las notificaciones en un icono mediano](images/sending-local-tile-02.png)

## <span id="Create_the_notification"></span><span id="create_the_notification"></span><span id="CREATE_THE_NOTIFICATION"></span>Crear la notificación


Una vez que dispongas del contenido de la notificación, debes crear un nuevo [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616). El constructor **TileNotification** toma un objeto [**XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br208620) de Windows Runtime, que puedes obtener del método **TileContent.GetXml** si estás usando [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki).

Este ejemplo de código crea una notificación para un icono nuevo.

```
// Create the tile notification
var notification = new TileNotification(content.GetXml());
```

## <span id="Set_an_expiration_time_for_the_notification__optional_"></span><span id="set_an_expiration_time_for_the_notification__optional_"></span><span id="SET_AN_EXPIRATION_TIME_FOR_THE_NOTIFICATION__OPTIONAL_"></span>Establecer una fecha de expiración para la notificación (opcional)


De manera predeterminada, las notificaciones de iconos y distintivos locales no expiran, mientras que las notificaciones programadas, periódicas y de inserción expiran al cabo de de tres días. El contenido de los iconos no debe persistir más de lo necesario, por lo que se considera una buena práctica establecer una fecha de caducidad apropiada para la aplicación, especialmente en las notificaciones de iconos y distintivos locales.

Este ejemplo de código crea una notificación que expira y se eliminará del icono después de diez minutos.

```
tileNotification.ExpirationTime = DateTimeOffset.UtcNow.AddMinutes(10);</code></pre></td>
</tr>
</tbody>
</table>
```

## <span id="Send_the_notification"></span><span id="send_the_notification"></span><span id="SEND_THE_NOTIFICATION"></span>Enviar la notificación


Aunque el envío local de una notificación de iconos es sencillo, enviar la notificación a un icono principal o secundario es un poco diferente.

**Icono principal**

Para enviar una notificación a un icono principal, usa [**TileUpdateManager**](https://msdn.microsoft.com/library/windows/apps/br208622) para crear un actualizador de iconos para el icono principal y envía la notificación llamando a "Update". Independientemente de si es visible, el icono principal de tu aplicación siempre existe, por lo que puedes enviarle notificaciones aunque no esté anclado. Si el usuario ancla tu icono principal más adelante, las notificaciones que enviaste aparecerán entonces.

Este ejemplo de código envía una notificación a un icono principal.

<span codelanguage=""></span>
```
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
// And send the notification
TileUpdateManager.CreateTileUpdaterForApplication().Update(notification);
```

**Icono secundario**

Para enviar una notificación a un icono secundario, asegúrate primero de que el icono secundario existe. Si intentas crear un actualizador de iconos para un icono secundario que no existe (por ejemplo, si el usuario ha desanclado el icono secundario), se generará una excepción. Puedes usar [**SecondaryTile.Exists**](https://msdn.microsoft.com/library/windows/apps/br242205) (tileId) para averiguar si tu icono secundario está anclado después crear un actualizador para el icono secundario y enviar la notificación.

Este ejemplo de código envía una notificación a un icono secundario.

```
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

## <span id="Clear_notifications_on_the_tile__optional_"></span><span id="clear_notifications_on_the_tile__optional_"></span><span id="CLEAR_NOTIFICATIONS_ON_THE_TILE__OPTIONAL_"></span>Borrar las notificaciones en el icono (opcional)


En la mayoría de los casos, debes borrar una notificación cuando el usuario haya interactuado con ese contenido. Por ejemplo, cuando el usuario inicia tu aplicación, es posible que quieras borrar todas las notificaciones del icono. Si tus notificaciones están controladas por tiempo, te recomendamos que establezcas una fecha de caducidad en la notificación en lugar de borrarla explícitamente.

Este ejemplo de código borra la notificación de icono.

```
TileUpdateManager.CreateTileUpdaterForApplication().Clear();</code></pre></td>
</tr>
</tbody>
</table>
```

En un icono con la cola de notificaciones habilitada y con notificaciones en cola, la cola se vacía cuando se llama al método Clear. Sin embargo, no puedes borrar una notificación a través del servidor de la aplicación; solo el código de la aplicación local puede borrar las notificaciones.

Las notificaciones de inserción o periódicas solo pueden agregar nuevas notificaciones o reemplazar las notificaciones existentes. Una llamada local al método Clear borrará el icono independientemente de si las notificaciones fueron de inserción, periódicas o locales. Este método no borra las notificaciones programadas que aún no han aparecido.

![icono con notificación e icono después de ser borrado](images/sending-local-tile-03.png)

## <span id="Next_steps"></span><span id="next_steps"></span><span id="NEXT_STEPS"></span>Pasos siguientes


**Uso de la cola de notificaciones**

Ahora que has realizado tu primera actualización de iconos, puedes habilitar una [cola de notificaciones](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234) para expandir la funcionalidad del icono.

**Otros métodos de entrega de notificaciones**

Este artículo te muestra cómo enviar la actualización de iconos como una notificación. Para explorar otros métodos de entrega de notificaciones, incluidos los programados, periódicos y de inserción, consulta [Entrega de notificaciones](tiles-and-notifications-choosing-a-notification-delivery-method.md).

**Método de entrega XmlEncode**

Si no estás usando [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki), este método de entrega de notificaciones es otra alternativa.

<span codelanguage=""></span>
```
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
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

## <span id="Code_examples_without_NotificationsExtensions"></span><span id="code_examples_without_notificationsextensions"></span><span id="CODE_EXAMPLES_WITHOUT_NOTIFICATIONSEXTENSIONS"></span>Ejemplos de código sin NotificationsExtensions


Si prefieres trabajar con XML sin formato en lugar del paquete NuGet [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki), usa estos ejemplos de código alternativos para los tres primeros ejemplos proporcionados en este artículo. El resto de los ejemplos de código se puede usar o bien con [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) o bien con XML sin formato.

Agregar declaraciones de espacios de nombres

```
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
```

Crear el contenido de la notificación

```
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// TODO - all values need to be XML escaped
 
 
// Construct the tile content as a string
string content = $@"
<tile>
    <visual>
 
        <binding template=&#39;TileMedium&#39;>
            <text>{from}</text>
            <text hint-style=&#39;captionSubtle&#39;>{subject}</text>
            <text hint-style=&#39;captionSubtle&#39;>{body}</text>
        </binding>
 
        <binding template=&#39;TileWide&#39;>
            <text hint-style=&#39;subtitle&#39;>{from}</text>
            <text hint-style=&#39;captionSubtle&#39;>{subject}</text>
            <text hint-style=&#39;captionSubtle&#39;>{body}</text>
        </binding>
 
    </visual>
</tile>";
```

Crear la notificación

```
// Load the string into an XmlDocument
XmlDocument doc = new XmlDocument();
doc.LoadXml(content);
 
// Then create the tile notification
var notification = new TileNotification(doc);
```

## <span id="related_topics"></span>Temas relacionados


* [Crear iconos adaptables](tiles-and-notifications-create-adaptive-tiles.md)
* [Plantillas de iconos adaptables: esquema y documentación](tiles-and-notifications-adaptive-tiles-schema.md)
* [NotificationsExtensions.Win10 (paquete NuGet)](https://www.nuget.org/packages/NotificationsExtensions.Win10/)
* [NotificationsExtensions en GitHub](https://github.com/WindowsNotifications/NotificationsExtensions/wiki)
* [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-tile-win10)
* [**Espacio de nombres Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661)
* [Cómo usar la cola de notificaciones (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)
* [Entrega de notificaciones](tiles-and-notifications-choosing-a-notification-delivery-method.md)
 

 







<!--HONumber=Jun16_HO4-->


