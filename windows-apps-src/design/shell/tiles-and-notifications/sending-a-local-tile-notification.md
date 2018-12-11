---
Description: This article describes how to send a local tile notification to a primary tile and a secondary tile using adaptive tile templates.
title: Enviar una notificación de icono local
ms.assetid: D34B0514-AEC6-4C41-B318-F0985B51AF8A
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5752a7bf18d785121258ea3fe75afe8383be2aff
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8880486"
---
# <a name="send-a-local-tile-notification"></a>Enviar una notificación de icono local
 

Iconos de la aplicación principal en Windows 10 se definen en el manifiesto de la aplicación, mientras que los iconos secundarios se crean y se definen mediante el código de aplicación mediante programación. En este artículo se describe cómo enviar una notificación de icono local a un icono principal y un icono secundario con el uso de plantillas de iconos adaptables. (Una notificación local es aquella que se envía desde el código de la aplicación frente a la que se envía o extrae de un servidor web).

![icono predeterminado e icono con notificación](images/sending-local-tile-01.png)

> [!NOTE] 
>Obtén información sobre cómo [crear iconos adaptables](create-adaptive-tiles.md) y el [esquema de contenido de ventana](../tiles-and-notifications/tile-schema.md).

 

## <a name="install-the-nuget-package"></a>Instalación del paquete NuGet


Recomendamos instalar el [paquete de NuGet de la librería de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), que simplifica las cosas al generar cargas de iconos con objetos en lugar de XML sin formato.

Los ejemplos de código en línea de este artículo son para C# usando la biblioteca de notificaciones. (Si prefieres crear tu propio XML, puedes encontrar ejemplos de código sin la librería de notificaciones al final del artículo.)

## <a name="add-namespace-declarations"></a>Agregar declaraciones de espacios de nombres


Para acceder a las API del icono, incluye el espacio de nombres [**Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications). También recomendamos incluir el espacio de nombres **Microsoft.Toolkit.Uwp.Notifications** para que puedas aprovechar nuestras API auxiliares de iconos (debes instalar el paquete NuGet [de la librería de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para acceder a estas API).

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```

## <a name="create-the-notification-content"></a>Crear el contenido de la notificación


En Windows 10, las cargas de iconos se definen mediante plantillas de iconos adaptables, que te permiten crear diseños visuales personalizados para las notificaciones. (Para obtener información sobre las posibilidades de los iconos adaptables, consulta los artículos [Crear iconos adaptables](create-adaptive-tiles.md).)

Este ejemplo de código crea el contenido de iconos adaptables para iconos medianos y anchos.

```csharp
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


Una vez que dispongas del contenido de la notificación, debes crear un nuevo [**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification). El constructor **TileNotification** toma un objeto [**XmlDocument**](https://docs.microsoft.com/uwp/api/windows.data.xml.dom.xmldocument) de Windows Runtime, que puedes obtener del método **TileContent.GetXml** si estás usando [la librería de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/).

Este ejemplo de código crea una notificación para un icono nuevo.

```csharp
// Create the tile notification
var notification = new TileNotification(content.GetXml());
```

## <a name="set-an-expiration-time-for-the-notification-optional"></a>Establecer una fecha de expiración para la notificación (opcional)


De manera predeterminada, las notificaciones de iconos y distintivos locales no expiran, mientras que las notificaciones programadas, periódicas y de inserción expiran al cabo de de tres días. El contenido de los iconos no debe persistir más de lo necesario, por lo que se considera una buena práctica establecer una fecha de caducidad apropiada para la aplicación, especialmente en las notificaciones de iconos y distintivos locales.

Este ejemplo de código crea una notificación que expira y se eliminará del icono después de diez minutos.

```csharp
tileNotification.ExpirationTime = DateTimeOffset.UtcNow.AddMinutes(10);
```

## <a name="send-the-notification"></a>Enviar la notificación


Aunque el envío local de una notificación de iconos es sencillo, enviar la notificación a un icono principal o secundario es un poco diferente.

**Icono principal**

Para enviar una notificación a un icono principal, usa [**TileUpdateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdateManager) para crear un actualizador de iconos para el icono principal y envía la notificación llamando a "Update". Independientemente de si es visible, el icono principal de tu aplicación siempre existe, por lo que puedes enviarle notificaciones aunque no esté anclado. Si el usuario ancla tu icono principal más adelante, las notificaciones que enviaste aparecerán entonces.

Este ejemplo de código envía una notificación a un icono principal.


```csharp
// Send the notification to the primary tile
TileUpdateManager.CreateTileUpdaterForApplication().Update(notification);
```

**Icono secundario**

Para enviar una notificación a un icono secundario, asegúrate primero de que el icono secundario existe. Si intentas crear un actualizador de iconos para un icono secundario que no existe (por ejemplo, si el usuario ha desanclado el icono secundario), se generará una excepción. Puedes usar [**SecondaryTile.Exists**](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile#Windows_UI_StartScreen_SecondaryTile_Exists_System_String_) (tileId) para averiguar si tu icono secundario está anclado después crear un actualizador para el icono secundario y enviar la notificación.

Este ejemplo de código envía una notificación a un icono secundario.

```csharp
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

```csharp
TileUpdateManager.CreateTileUpdaterForApplication().Clear();
```

En un icono con la cola de notificaciones habilitada y con notificaciones en cola, la cola se vacía cuando se llama al método Clear. Sin embargo, no puedes borrar una notificación a través del servidor de la aplicación; solo el código de la aplicación local puede borrar las notificaciones.

Las notificaciones de inserción o periódicas solo pueden agregar nuevas notificaciones o reemplazar las notificaciones existentes. Una llamada local al método Clear borrará el icono independientemente de si las notificaciones fueron de inserción, periódicas o locales. Este método no borra las notificaciones programadas que aún no han aparecido.

![icono con notificación e icono después de ser borrado](images/sending-local-tile-03.png)

## <a name="next-steps"></a>Pasos siguientes


**Uso de la cola de notificaciones**

Ahora que has realizado tu primera actualización de iconos, puedes habilitar una [cola de notificaciones](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234) para expandir la funcionalidad del icono.

**Otros métodos de entrega de notificaciones**

Este artículo te muestra cómo enviar la actualización de iconos como una notificación. Para explorar otros métodos de entrega de notificaciones, incluidos los programados, periódicos y de inserción, consulta [Entrega de notificaciones](choosing-a-notification-delivery-method.md).

**Método de entrega XmlEncode**

Si no estás usando la [librería de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), este método de entrega de notificaciones es otra alternativa.


```csharp
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

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
```

Crear el contenido de la notificación

```csharp
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

```csharp
// Load the string into an XmlDocument
XmlDocument doc = new XmlDocument();
doc.LoadXml(content);
 
// Then create the tile notification
var notification = new TileNotification(doc);
```

## <a name="related-topics"></a>Temas relacionados

* [Crear iconos adaptables](create-adaptive-tiles.md)
* [Esquema de contenido de ventana](../tiles-and-notifications/tile-schema.md)
* [Biblioteca Notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)
* [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-tile-win10)
* [**Espacio de nombres Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications)
* [Cómo usar la cola de notificaciones (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)
* [Entrega de notificaciones](choosing-a-notification-delivery-method.md)
 

 




