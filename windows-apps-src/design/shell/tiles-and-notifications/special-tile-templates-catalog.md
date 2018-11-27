---
Description: Special tile templates are unique templates that are either animated, or just allow you to do things that aren't possible with adaptive tiles.
title: Plantillas de iconos especiales
ms.assetid: 1322C9BA-D5B2-45E2-B813-865884A467FF
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 09647347134463c8dd2d93f6b869796c8def44e2
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7698144"
---
# <a name="special-tile-templates"></a>Plantillas de iconos especiales
 

Las plantillas de iconos especiales son plantillas únicas que, o bien están animadas, o bien simplemente te permiten hacer cosas que no son posibles con los iconos adaptables. Cada plantilla de iconos especial fue compilada específicamente para Windows 10, excepto la plantilla de iconos icónica, una plantilla especial clásica que se ha actualizado para Windows 10. En este artículo se abordan tres plantillas de iconos especiales: icónicas, de fotos y de contactos.

## <a name="iconic-tile-template"></a>Plantillas de iconos icónicas


La plantilla icónica (también conocida como plantilla "IconWithBadge") te permite mostrar una imagen pequeña en el centro del icono. Windows 10 admite la plantilla en el teléfono y el escritorio o tableta.

![iconos de correo pequeños y medianos](images/iconic-template-mail-2sizes.png)

### <a name="how-to-create-an-iconic-tile"></a>Cómo crear un icono de iconos

Los siguientes pasos incluyen todo lo que necesitas saber para crear un icono de iconos para Windows 10. En un nivel alto, necesitas el activo de imagen icónico, a continuación envías una notificación al icono con la plantilla icónica y, finalmente, envías una notificación de distintivos que proporcione el número que debe mostrarse en el icono.

![flujo de desarrolladores del icono de iconos](images/iconic-template-dev-flow.png)

**Paso 1: crear tus activos de imagen en formato PNG**

Crear los activos de icono para tu icono y situarlos en tus recursos del proyecto con tus otros activos. Como mínimo, crea un icono de 200 x 200 píxeles, que funcione tanto para iconos pequeños como medianos en teléfonos y en el escritorio. Para proporcionar la mejor experiencia de usuario, crea un icono para cada tamaño. Estos activos no requieren relleno. Consulta los detalles de ajuste de tamaño en la imagen que viene a continuación.

Guarda activos de iconos en formato PNG y con transparencia. En Windows Phone, se muestra cada píxel no transparente en blanco (RGB 255, 255, 255). Por razones de coherencia y simplicidad, usar el blanco también para los iconos de escritorio.

Windows 10 en escritorio, portátil y tableta solo es compatible con activos icónicos cuadrados. El teléfono es compatible tanto con activos cuadrados como con activos que son más altos que anchos, hasta una proporción de ancho:alto 2:3, que es útil para imágenes como un icono de teléfono.

![ajuste de tamaño de los iconos en iconos pequeños y medianos, en teléfonos y en el escritorio](images/iconic-template-sizing-info.png)

![tamaño para los activos con y sin distintivo](images/assetguidance24.png)

En los activos cuadrados, se produce el centrado automático dentro del contenedor:

![tamaño del activo cuadrado, con y sin distintivo](images/assetguidance25.png)

Para activos no cuadrados, se produce el centrado horizontal o vertical automático y el ajuste del ancho o alto del contenedor:

![tamaño del recurso no cuadrado, con y sin distintivo](images/assetguidance26a.png)

![tamaño del recurso no cuadrado, con y sin distintivo](images/assetguidance26b.png)

**Paso 2: crear el icono de base**

Puedes usar la plantilla icónica tanto en el icono principal como en los secundarios. Si estás usándola en un icono secundario, primero tendrás que crear el icono secundario o usar un icono secundario ya anclado. Los iconos principales están anclados implícitamente y siempre se les puede enviar notificaciones.

**Paso 3: enviar una notificación a tu icono**

Aunque este paso puede variar en función de si la notificación se envía de forma local o a través de inserción del servidor, la carga de XML que envíes se mantiene. Para enviar una notificación de icono local, crea [**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater) para tu icono (icono principal o secundario) y después envía una notificación al icono que usa la plantilla de iconos icónica tal como se muestra a continuación. En teoría, también debes incluir enlaces para tamaños de icono grande y ancho con [plantillas de iconos adaptables](create-adaptive-tiles.md).

Este es un código de muestra para la carga de XML:

```xml
<tile>
  <visual>

    <binding template="TileSquare150x150IconWithBadge">
      <image id="1" src="Iconic.png" alt="alt text"/>
    </binding>
    
    <binding template="TileSquare71x71IconWithBadge">
      <image id="1" src="Iconic.png" alt="alt text"/>
    </binding>

  </visual>
</tile>
```

Esta carga de XML de plantilla de iconos icónica usa un elemento de imagen que apunta a la imagen que creaste en el paso 1. Ahora tu icono está listo para mostrar el distintivo junto a tu icono; lo único que queda es enviar las notificaciones de los distintivos.

**Paso 4: enviar una notificación de distintivo a tu icono**

Del mismo modo que en el paso 3, este paso puede variar en función de si la notificación se envía de forma local o a través de inserción del servidor, pero la carga de XML que envíes sigue siendo la misma. Para enviar una notificación local, crea un [**BadgeUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.BadgeUpdater) para tu icono (ya sea un icono principal o secundario) y después envía una notificación con tu valor deseado (o borra el distintivo).

Este es un código de muestra para la carga de XML:

```xml
<badge value="2"/>
```

El distintivo del icono se actualizará acordemente.

**Paso 5: implementación**

La siguiente imagen ilustra cómo se asocian las distintas API y cargas con cada uno de los aspectos de la plantilla de iconos icónica. Una [notificación de icono](https://msdn.microsoft.com/library/windows/apps/hh779724) (que contenga esos elementos &lt;binding&gt;) se usa para especificar la plantilla icónica y el activo de imagen; una [notificación de distintivo](https://msdn.microsoft.com/library/windows/apps/hh779719) especifica el valor numérico; las propiedades del icono controlan el nombre para mostrar, el color y otros datos de tu icono.

![API y cargas asociadas con la plantilla de iconos icónica](images/iconic-template-properties-info.png)

## <a name="photos-tile-template"></a>Plantilla de iconos de fotos


La plantilla de iconos de Fotos te permite mostrar una presentación de fotos en tu icono dinámico. La plantilla es compatible con todos los tamaños de icono, incluido el pequeño, y se comporta del mismo modo en cada tamaño de icono. En el ejemplo siguiente se muestran cinco fotogramas de un icono mediano que usa la plantilla de fotos. La plantilla tiene un zoom y una animación de fundido que recorre las fotos seleccionadas y se repite indefinidamente.

![presentación de la imagen con la plantilla de iconos de Fotos](images/photo-tile-template-image01.jpg)

### <a name="how-to-use-the-photos-template"></a>Cómo usar la plantilla de fotos

Usar la plantilla de fotos es fácil si has instalado la [biblioteca de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/). Aunque puedes usar XML sin formato, se recomienda encarecidamente usar la librería para que no tengas que preocuparte de generar contenido XML o XML de escape válido.

Windows Phone muestra hasta 9 fotos en una presentación; en tabletas, portátiles o el escritorio se mostrarán hasta 12.

Para obtener información sobre cómo enviar la notificación de iconos, consulta el [artículo Enviar notificaciones](index.md).


```xml
<!--
 
To use the Photos template...
 
 - On any adaptive tile binding (like TileMedium or TileWide)
   - Set the hint-presentation attribute to "photos"
   - Add up to 12 images as children of the binding.
    
-->
 
<tile>
  <visual>
     
    <binding template="TileMedium" hint-presentation="photos">
       
      <image src="Assets/1.jpg" />
      <image src="ms-appdata:///local/Images/2.jpg"/>
      <image src="http://msn.com/images/3.jpg"/>
       
      <!--TODO: Can have 12 images total-->
       
    </binding>
     
    <!--TODO: Add bindings for other tile sizes-->
     
  </visual>
</tile>
```

```csharp
/*
 
To use the Photos template...
 
 - On any TileBinding object
   - Set Content property to new instance of TileBindingContentPhotos
   - Add up to 12 images to Images property of TileBindingContentPhotos.
 
*/
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentPhotos()
            {
                Images =
                {
                    new TileBasicImage() { Source = "Assets/1.jpg" },
                    new TileBasicImage() { Source = "ms-appdata:///local/Images/2.jpg" },
                    new TileBasicImage() { Source = "http://msn.com/images/3.jpg" }
 
                    // TODO: Can have 12 images total
                }
            }
        }
 
        // TODO: Add other tile sizes
    }
};
```

## <a name="people-tile-template"></a>Plantilla de iconos de contactos


La aplicación de contactos en Windows 10 usa una plantilla de iconos especial que muestra una colección de imágenes en círculos que se deslizan vertical u horizontalmente en el icono. Esta plantilla de iconos ha estado disponible desde Windows 10 compilación 10572 y cualquier persona es bienvenida a usarla en su aplicación.

Las plantilla de iconos de Contactos funciona en los iconos de estos tamaños:

**Icono mediano** (TileMedium)

![icono de contactos mediano](images/people-tile-medium.png)

 

**Icono ancho** (TileWide)

![icono de contactos ancho](images/people-tile-wide.png)

 

**Icono grande (solo escritorio)** (TileLarge)

![icono de contactos grande](images/people-tile-large.png)

 

Si estás usando la [biblioteca Notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), todo lo que tienes que hacer para usar la plantilla de iconos de Contactos es un nuevo objeto *TileBindingContentPeople* para tu contenido *TileBinding*. La clase *TileBindingContentPeople* tiene una propiedad de Imágenes donde agregar tus imágenes.

Si usas XML sin formato, establece la *presentación de sugerencias* para "contactos" y agrega tus imágenes como elementos secundarios del elemento de enlace.

En la siguiente muestra de código de C#, se da por hecho que estás usando [biblioteca Notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/).

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentPeople()
            {
                Images =
                {
                    new TileBasicImage() { Source = "Assets/ProfilePics/1.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/2.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/3.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/4.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/5.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/6.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/7.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/8.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/9.jpg" }
                }
            }
        }
    }
};
```

```xml
<tile>
  <visual>
 
    <binding template="TileMedium" hint-presentation="people">
      <image src="Assets/ProfilePics/1.jpg"/>
      <image src="Assets/ProfilePics/2.jpg"/>
      <image src="Assets/ProfilePics/3.jpg"/>
      <image src="Assets/ProfilePics/4.jpg"/>
      <image src="Assets/ProfilePics/5.jpg"/>
      <image src="Assets/ProfilePics/6.jpg"/>
      <image src="Assets/ProfilePics/7.jpg"/>
      <image src="Assets/ProfilePics/8.jpg"/>
      <image src="Assets/ProfilePics/9.jpg"/>
    </binding>
 
  </visual>
</tile>
```

Para la mejor experiencia de usuario, te recomendamos que proporciones el siguiente número de fotos para cada tamaño de icono:

-   Icono mediano: 9 fotos
-   Icono ancho: 15 fotos
-   Icono grande: 20 fotos

Este número de fotos permite que haya algunos círculos vacíos, lo que significa que el icono no estará demasiado ocupado visualmente. No dudes en retocar el número de fotos para conseguir la mejor apariencia para ti.

Para enviar la notificación, consulta [Elegir un método de entrega de notificaciones](choosing-a-notification-delivery-method.md).

## <a name="related-topics"></a>Temas relacionados


* [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/quickstart-people-tile-template)
* [Biblioteca Notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)
* [Iconos, distintivos y notificaciones](index.md)
* [Crear iconos adaptables](create-adaptive-tiles.md)
* [Esquema de contenido de ventana](../tiles-and-notifications/tile-schema.md)
 

 




