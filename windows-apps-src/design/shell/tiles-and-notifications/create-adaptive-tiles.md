---
description: Las plantillas de iconos adaptables son una nueva característica de Windows 10, que te permite diseñar tu propio contenido de notificación de icono con un lenguaje de marcado sencillo y flexible que se adapta a diferentes densidades de pantalla.
title: Crear iconos adaptables
ms.assetid: 1246B58E-D6E3-48C7-AD7F-475D113600F9
label: Create adaptive tiles
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b8160e8fa6adcfdba5cd33f1c8b80123cd539551
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032358"
---
# <a name="create-adaptive-tiles"></a>Crear iconos adaptables

Las plantillas de iconos adaptables son una nueva característica de Windows 10, que te permite diseñar tu propio contenido de notificación de icono con un lenguaje de marcado sencillo y flexible que se adapta a diferentes densidades de pantalla. En este artículo se explica cómo crear iconos dinámicos adaptables para la aplicación de Windows. Para obtener la lista completa de atributos y elementos adaptables, consulta el [Esquema de iconos adaptables](../tiles-and-notifications/tile-schema.md).

(Si quieres, puedes seguir usando las plantillas preestablecidas del [catálogo de plantillas de iconos de Windows 8](/previous-versions/windows/apps/hh761491(v=win.10)) al diseñar las notificaciones para Windows 10.)


## <a name="getting-started"></a>Introducción

**Instalar la biblioteca de notificaciones.** Si quieres usar C# en lugar de XML para generar notificaciones, instala el paquete de NuGet denominado [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) (buscar "notificaciones para UWP"). Los ejemplos de C# proporcionados en este artículo usan la versión 1.0.0 del paquete NuGet.

**Instala Notifications Visualizer** Esta aplicación Windows gratuita le ayuda a diseñar iconos dinámicos adaptables al proporcionar una vista previa visual instantánea del icono a medida que lo edita, similar a la vista de diseño o editor XAML de Visual Studio. Vea el [visualizador de notificaciones](notifications-visualizer.md) para obtener más información o [descargar el visualizador de notificaciones de la tienda](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="how-to-send-a-tile-notification"></a>Cómo enviar una notificación de icono

Lee nuestro artículo [Inicio rápido sobre cómo enviar notificaciones de icono locales](sending-a-local-tile-notification.md). En la documentación siguiente se explican todas las posibilidades de interfaz de usuario visual que tiene con iconos adaptables.


## <a name="usage-guidance"></a>Guía de uso


Las plantillas adaptables están diseñadas para funcionar en diferentes factores de forma y tipos de notificación. Los elementos, como el grupo y el subgrupo, vinculan entre sí el contenido y no implican un comportamiento visual determinado de forma indiscriminada. El aspecto final de una notificación debe basarse en el dispositivo específico en el que se va a mostrar, ya sea un teléfono, una tableta, un equipo de escritorio u otro dispositivo.

Las sugerencias son atributos opcionales que se pueden agregar a los elementos con el fin de lograr un comportamiento visual específico. Las sugerencias pueden ser específicas de un dispositivo o de una notificación.

## <a name="a-basic-example"></a>Un ejemplo básico


En este ejemplo se muestra lo que pueden generar las plantillas de iconos adaptables.

```xml
<tile>
  <visual>
  
    <binding template="TileMedium">
      ...
    </binding>
  
    <binding template="TileWide">
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </binding>
  
    <binding template="TileLarge">
      ...
    </binding>
  
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = ...
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "Jennifer Parker",
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },
  
                    new AdaptiveText()
                    {
                        Text = "Photos from our trip",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },
  
                    new AdaptiveText()
                    {
                        Text = "Check out these awesome photos I took while in New Zealand!",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        },
  
        TileLarge = ...
    }
};
```

**Resultado:**

![icono de muestra rápido](images/adaptive-tiles-quicksample.png)

## <a name="tile-sizes"></a>Tamaños de iconos


El contenido de cada tamaño de mosaico se especifica individualmente en elementos [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) independientes dentro de la carga XML. Elige el tamaño de destino estableciendo el atributo de plantilla a uno de los siguientes valores:

-   TileSmall
-   TileMedium
-   TileWide
-   TileLarge (solo para equipos de escritorio)

Para una única carga XML de notificaciones de icono, proporciona elementos &lt;binding&gt; para cada tamaño de icono que quieras admitir, tal como se muestra en este ejemplo:

```xml
<tile>
  <visual>
  
    <binding template="TileSmall">
      <text>Small</text>
    </binding>
  
    <binding template="TileMedium">
      <text>Medium</text>
    </binding>
  
    <binding template="TileWide">
      <text>Wide</text>
    </binding>
  
    <binding template="TileLarge">
      <text>Large</text>
    </binding>
  
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileSmall = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Small" }
                }
            }
        },
  
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Medium" }
                }
            }
        },
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Wide" }
                }
            }
        },
  
        TileLarge = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Large" }
                }
            }
        }
    }
};
```

**Resultado:**

![tamaños de iconos adaptables: pequeños, medianos, anchos y grandes](images/adaptive-tiles-sizes.png)

## <a name="branding"></a>Personalización de marca


Puedes controlar la personalización de marca en la parte inferior de un icono dinámico (el nombre para mostrar y el logotipo de esquina) mediante el atributo de personalización de marca de la carga de notificación. Puedes elegir mostrar "none", solo "name", solo "logo" o ambos con "nameAndLogo".

**Nota** Windows Mobile no admite el logotipo de esquina, por lo que "logo" y "nameAndLogo" se definen de manera predeterminada en "name" en Mobile.

 

```xml
<visual branding="logo">
  ...
</visual>
```

```csharp
new TileVisual()
{
    Branding = TileBranding.Logo,
    ...
}
```

**Resultado:**

![iconos adaptables, nombre y logotipo](images/adaptive-tiles-namelogo.png)

La personalización de marca se puede aplicar para determinados tamaños de iconos. Puedes hacerlo de dos formas:

1. Aplicando el atributo en el elemento [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding)
2. Al aplicar el atributo en el elemento [TileVisual](../tiles-and-notifications/tile-schema.md#tilevisual) , que afecta a la carga de notificación completa si no se especifica la personalización de marca para un enlace, se usará la personalización de marca que se proporciona en el elemento visual.

```xml
<tile>
  <visual branding="nameAndLogo">
 
    <binding template="TileMedium" branding="logo">
      ...
    </binding>
 
    <!--Inherits branding from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,

        TileMedium = new TileBinding()
        {
            Branding = TileBranding.Logo,
            ...
        },

        // Inherits branding from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**Resultado predeterminado de la personalización de marca:**

![personalización de marca predeterminada en iconos](images/adaptive-tiles-defaultbranding.png)

Si no especificas la personalización de marca en la carga de notificaciones, las propiedades del icono base determinarán la personalización de marca. Si en el icono base se muestra el nombre para mostrar, a continuación, la personalización de marca se definirá de manera predeterminada en "name". De lo contrario, la personalización de marca se definirá de manera predeterminada en "none" si no se muestra el nombre para mostrar.

**Nota** Este es un cambio de Windows 8.x, en el que la personalización de marca predeterminada era "logo".

 

## <a name="display-name"></a>Nombre para mostrar


Puedes invalidar el nombre para mostrar de una notificación escribiendo la cadena de texto que quieras con el atributo **displayName** . Al igual que con la personalización de marca, puede especificar esto en el elemento [TileVisual](../tiles-and-notifications/tile-schema.md#tilevisual) , que afecta a toda la carga de notificación, o en el elemento [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) , que solo afecta a los mosaicos individuales.

**Problema conocido**  En Windows Mobile, si especifica un ShortName para Tile, no se usará el nombre para mostrar proporcionado en la notificación (ShortName se muestra siempre). 

```xml
<tile>
  <visual branding="nameAndLogo" displayName="Wednesday 22">
 
    <binding template="TileMedium" displayName="Wed. 22">
      ...
    </binding>
 
    <!--Inherits displayName from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,
        DisplayName = "Wednesday 22",

        TileMedium = new TileBinding()
        {
            DisplayName = "Wed. 22",
            ...
        },

        // Inherits DisplayName from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**Resultado:**

![nombre para mostrar de los iconos adaptables](images/adaptive-tiles-displayname.png)

## <a name="text"></a>Texto


El elemento [AdaptiveText](../tiles-and-notifications/tile-schema.md#adaptivetext) se usa para mostrar el texto. Puedes usar sugerencias para modificar la apariencia del texto.

```xml
<text>This is a line of text</text>
```


```csharp
new AdaptiveText()
{
    Text = "This is a line of text"
};
```

**Resultado:**

![texto del icono adaptable](images/adaptive-tiles-text.png)

## <a name="text-wrapping"></a>Ajuste de texto


De manera predeterminada, el texto no se ajusta y continuará fuera del borde de la ventana. Usa el atributo **hint-wrap** para establecer el ajuste de texto en un elemento de texto. También puedes controlar el número mínimo y máximo de líneas mediante **hint-minLines** y **hint-maxLines** , que aceptan enteros positivos.

```xml
<text hint-wrap="true">This is a line of wrapping text</text>
```


```csharp
new AdaptiveText()
{
    Text = "This is a line of wrapping text",
    HintWrap = true
};
```

**Resultado:**

![icono adaptable con ajuste de texto](images/adaptive-tiles-textwrapping.png)

## <a name="text-styles"></a>Estilos de texto


Los estilos controlan el tamaño de fuente, el color y el espesor de los elementos de texto. Hay una serie de estilos disponibles, así como una variación "sutil" de cada estilo que establece la opacidad al 60 % y que, por lo general, hace que el color del texto tenga un tono gris claro.

```xml
<text hint-style="base">Header content</text>
<text hint-style="captionSubtle">Subheader content</text>
```

```csharp
new AdaptiveText()
{
    Text = "Header content",
    HintStyle = AdaptiveTextStyle.Base
},

new AdaptiveText()
{
    Text = "Subheader content",
    HintStyle = AdaptiveTextStyle.CaptionSubtle
}
```

**Resultado:**

![estilos de texto de iconos adaptables](images/adaptive-tiles-textstyles.png)

**Nota** El estilo se define de manera predeterminada en modo de subtítulo si no se especifica ninguna sugerencia de estilo.

 

**Estilos de texto básicos**

| &lt;sugerencia de texto: estilo = " \* "/&gt; | Alto de fuente               | Espesor de la fuente |
|--------------------------------|---------------------------|-------------|
| caption                        | 12 píxeles efectivos (epx) | Normal     |
| body                           | 15 epx                    | Normal     |
| base                           | 15 epx                    | Semibold    |
| subtitle                       | 20 epx                    | Normal     |
| title                          | 24 epx                    | Semilight   |
| subheader                      | 34 epx                    | Claro       |
| encabezado                         | 46 epx                    | Claro       |

 

**Variaciones de estilo de texto numérico**

Estas variaciones reducen el alto de línea para que el contenido de la parte superior e inferior se aproxime más al texto.

- titleNumeral

- subheaderNumeral

- headerNumeral

 

**Variaciones de estilo de texto sutiles**

Cada estilo tiene una variación sutil que proporciona al texto una opacidad del 60 % que, por lo general, hace que el color del texto tenga un tono gris claro.

- captionSubtle

- bodySubtle

- baseSubtle

- subtitleSubtle

- titleSubtle

- titleNumeralSubtle

- subheaderSubtle

- subheaderNumeralSubtle

- headerSubtle

- headerNumeralSubtle

 

## <a name="text-alignment"></a>Alineación del texto


El texto se puede ser alinear de forma horizontal a la izquierda, al centro o a la derecha. En el caso de los idiomas que se escriben de izquierda a derecha, como el inglés, el texto se alinea de manera predeterminada a la izquierda. En el caso de los idiomas que se escriben de derecha a izquierda, como el árabe, el texto se alinea de manera predeterminada a la derecha. Puedes establecer la alineación manualmente mediante el atributo **hint-align** en los elementos.

```xml
<text hint-align="center">Hello</text>
```


```csharp
new AdaptiveText()
{
    Text = "Hello",
    HintAlign = AdaptiveTextAlign.Center
};
```

**Resultado:**

![alineación de texto de iconos adaptables](images/adaptive-tiles-textalignment.png)

## <a name="groups-and-subgroups"></a>Grupos y subgrupos


Los grupos permiten declarar semánticamente que el contenido dentro del grupo está relacionado y debe mostrarse en su totalidad para que el contenido tenga sentido. Por ejemplo, puede que tengas dos elementos de texto, un encabezado y un subencabezado, y no tendría sentido que solo se mostrara el encabezado. Al agrupar estos elementos dentro de un subgrupo, se mostrarán todos los elementos (si caben) o no se mostrará ninguno (si no caben).

Con el fin de obtener la mejor experiencia en los dispositivos y las pantallas, proporciona varios grupos. El hecho de tener varios grupos permite que el icono se adapte a las pantallas más grandes.

**Nota**  El único elemento secundario válido de un grupo es un subgrupo.

 

```xml
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup>
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </subgroup>
  </group>
 
  <text />
 
  <group>
    <subgroup>
      <text hint-style="subtitle">Steve Bosniak</text>
      <text hint-style="captionSubtle">Build 2015 Dinner</text>
      <text hint-style="captionSubtle">Want to go out for dinner after Build tonight?</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            CreateGroup(
                from: "Jennifer Parker",
                subject: "Photos from our trip",
                body: "Check out these awesome photos I took while in New Zealand!"),

            // For spacing
            new AdaptiveText(),

            CreateGroup(
                from: "Steve Bosniak",
                subject: "Build 2015 Dinner",
                body: "Want to go out for dinner after Build tonight?")
        }
    }
}

...

private static AdaptiveGroup CreateGroup(string from, string subject, string body)
{
    return new AdaptiveGroup()
    {
        Children =
        {
            new AdaptiveSubgroup()
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
    };
}
```

**Resultado:**

![grupos y subgrupos de iconos adaptables](images/adaptive-tiles-groups-subgroups.png)

## <a name="subgroups-columns"></a>Subgrupos (columnas)


Los subgrupos también permiten dividir los datos en secciones semánticas dentro de un grupo. Para los iconos dinámicos, esto se traduce visualmente en columnas.

El atributo **hint-weight** te permite controlar el ancho de las columnas. El valor de **hint-weight** se expresa como una proporción ponderada del espacio disponible, que es idéntica al comportamiento de **GridUnitType.Star** . Las columnas que tienen el mismo ancho, asignan cada espesor en 1.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Sugerencia de espesor</td>
<td align="left">Porcentaje de ancho</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25 %</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25 %</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25 %</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25 %</td>
</tr>
<tr class="even">
<td align="left">Espesor total: 4</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![subgrupos, columnas pares](images/adaptive-tiles-subgroups01.png)

Para hacer que una columna sea dos veces más grande que otra columna, asigna a la columna pequeña un espesor de 1 y a la columna mayor un espesor de 2.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Sugerencia de espesor</td>
<td align="left">Porcentaje de ancho</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">33,3 %</td>
</tr>
<tr class="odd">
<td align="left">2</td>
<td align="left">66,7 %</td>
</tr>
<tr class="even">
<td align="left">Espesor total: 3</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![subgrupos, una columna que duplica el tamaño de otra](images/adaptive-tiles-subgroups02.png)

Si quieres que la primera columna ocupe un 20 % del ancho total y que la segunda columna ocupe un 80 % del ancho total, asigna el espesor de la primera en 20 y el de la segunda en 80. Si los valores totales del espesor suman 100, actúan como porcentajes.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Sugerencia de espesor</td>
<td align="left">Porcentaje de ancho</td>
</tr>
<tr class="even">
<td align="left">20</td>
<td align="left">20%</td>
</tr>
<tr class="odd">
<td align="left">80</td>
<td align="left">80 %</td>
</tr>
<tr class="even">
<td align="left">Espesor total: 100</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![subgrupos, con espesores que suman 100](images/adaptive-tiles-subgroups03.png)

**Nota**  Automáticamente se agrega un margen de 8 píxeles entre las columnas.

 

Cuando tengas más de dos subgrupos, debes especificar **hint-weight** , que solo acepta enteros positivos. Si no especificas ninguna sugerencia de espesor para el primer subgrupo, se le asignará un espesor de 50. Al siguiente subgrupo que no tenga ninguna sugerencia de espesor especificada se le asignará un espesor igual a 100 menos la suma de los espesores anteriores, o de 1 si el resultado es cero. En el caso de los subgrupos restantes que no tengan ninguna sugerencia de espesor especificada, se les asignará un grosor de 1.

Este es un código de ejemplo para un icono de información meteorológica que muestra cómo puedes lograr que un icono tenga cinco columnas con el mismo ancho:

```xml
<binding template="TileWide" displayName="Seattle" branding="name">
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Tue</text>
      <image src="Assets\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-align="center" hint-style="captionsubtle">38°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Wed</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">59°</text>
      <text hint-align="center" hint-style="captionsubtle">43°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Thu</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">62°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Fri</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">71°</text>
      <text hint-align="center" hint-style="captionsubtle">66°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°"),
                    CreateSubgroup("Wed", "Sunny.png", "59°", "43°"),
                    CreateSubgroup("Thu", "Sunny.png", "62°", "42°"),
                    CreateSubgroup("Fri", "Sunny.png", "71°", "66°")
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Resultado:**

![ejemplo de un icono de información meteorológica](images/adaptive-tiles-weathertile.png)

## <a name="images"></a>Imágenes


El elemento &lt;image&gt; se usa para mostrar imágenes en la notificación de icono. Las imágenes pueden colocarse alineadas dentro del contenido del icono (valor predeterminado), como una imagen de fondo detrás del contenido o como una animación de imagen que aparezca desde la parte superior de la notificación.

> [!NOTE]
> Las imágenes se pueden usar desde el paquete de la aplicación, el almacenamiento local de la aplicación o desde la Web. A partir de la actualización de Fall Creators, las imágenes Web pueden tener hasta 3 MB en las conexiones normales y 1 MB en las conexiones de uso medido. En los dispositivos que aún no ejecuten el Fall Creators Update, las imágenes web no deben tener más de 200 KB.

 

Si no se especifica ningún comportamiento adicional, las imágenes se reducirán o expandirán uniformemente para rellenar el ancho disponible. En este ejemplo se muestra un icono con dos columnas e imágenes insertadas. Las imágenes alineadas se ajustan para rellenar el ancho de la columna.

```xml
<binding template="TileMedium" displayName="Seattle" branding="name">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Apps\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Apps\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionSubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°")
                }
            }
        }
    }
}
...
private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Resultado:**

![ejemplo de imagen](images/adaptive-tiles-images01.png)

Las imágenes colocadas en la raíz del elemento &lt;binding&gt;, o en el primer grupo, también se ampliarán para ajustarse al alto disponible.

### <a name="image-alignment"></a>Alineación de la imagen

Las imágenes se pueden configurar para que se alineen a la izquierda, al centro o a la derecha con el atributo **hint-align** . Esto también hará que las imágenes se muestren en su resolución nativa en lugar de ampliarse para rellenar el ancho.

```xml
<binding template="TileLarge">
  <image src="Assets/fable.jpg" hint-align="center"/>
</binding>
```

```csharp
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveImage()
            {
                Source = "Assets/fable.jpg",
                HintAlign = AdaptiveImageAlign.Center
            }
        }
    }
}
```

**Resultado:**

![ejemplo de alineación de imagen (izquierda, centro, derecha)](images/adaptive-tiles-imagealignment.png)

### <a name="image-margins"></a>Márgenes de la imagen

De forma predeterminada, las imágenes alineadas tienen un margen de 8 píxeles entre cualquier contenido de la parte superior e inferior de la imagen. Este margen puede quitarse mediante el atributo **hint-removeMargin** de la imagen. Sin embargo, las imágenes siempre mantienen el margen de 8 píxeles desde el borde del icono, y los subgrupos (columnas) siempre mantienen el espaciado interno de 8 píxeles entre las columnas.

```xml
<binding template="TileMedium" branding="none">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Numbers\4.jpg" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Numbers\3.jpg" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionsubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Branding = TileBranding.None,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "4.jpg", "63°", "42°"),
                    CreateSubgroup("Tue", "3.jpg", "57°", "38°")
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Numbers/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

![ejemplo de sugerencia de eliminación de margen](images/adaptive-tiles-removemargin.png)

### <a name="image-cropping"></a>Recorte de imagen

Las imágenes se pueden recortar en forma de círculo mediante el atributo **hint-crop** , que actualmente solo admite valores "none" (valor predeterminado) o "circle".

```xml
<binding template="TileLarge" hint-textStacking="center">
  <group>
    <subgroup hint-weight="1"/>
    <subgroup hint-weight="2">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-weight="1"/>
  </group>
 
  <text hint-style="title" hint-align="center">Hi,</text>
  <text hint-style="subtitleSubtle" hint-align="center">MasterHip</text>
</binding>
```

```csharp
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    new AdaptiveSubgroup() { HintWeight = 1 },
                    new AdaptiveSubgroup()
                    {
                        HintWeight = 2,
                        Children =
                        {
                            new AdaptiveImage()
                            {
                                Source = "Assets/Apps/Hipstame/hipster.jpg",
                                HintCrop = AdaptiveImageCrop.Circle
                            }
                        }
                    },
                    new AdaptiveSubgroup() { HintWeight = 1 }
                }
            },
            new AdaptiveText()
            {
                Text = "Hi,",
                HintStyle = AdaptiveTextStyle.Title,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = "MasterHip",
                HintStyle = AdaptiveTextStyle.SubtitleSubtle,
                HintAlign = AdaptiveTextAlign.Center
            }
        }
    }
}
```

**Resultado:**

![ejemplo de recorte de imagen](images/adaptive-tiles-imagecropping.png)

### <a name="background-image"></a>Imagen de fondo

Para establecer una imagen de fondo, coloca un elemento de imagen en la raíz del elemento &lt;binding&gt; y establece el atributo de ubicación en "background".

```xml
<binding template="TileWide">
  <image src="Assets\Mostly Cloudy-Background.jpg" placement="background"/>
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    ...
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = "Assets/Mostly Cloudy-Background.jpg"
        },

        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°")
                    ...
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Resultado:**

![ejemplo de imagen de fondo](images/adaptive-tiles-backgroundimage.png)

### <a name="peek-image"></a>Imagen que aparece

Puedes especificar una imagen que "aparezca" desde la parte superior del icono. La imagen que aparece usa una animación para bajar y subir desde la parte superior del icono, detenerse para que se pueda leer y, a continuación, volver a deslizarse para desaparecer y así mostrar el contenido principal del icono. Para establecer una imagen que aparece, coloca un elemento de imagen en la raíz del elemento &lt;binding&gt; y establece el atributo de ubicación en "peek".

```xml
<binding template="TileMedium" branding="name">
  <image placement="peek" src="Assets/Apps/Hipstame/hipster.jpg"/>
  <text>New Message</text>
  <text hint-style="captionsubtle" hint-wrap="true">Hey, have you tried Windows 10 yet?</text>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        PeekImage = new TilePeekImage()
        {
            Source = "Assets/Apps/Hipstame/hipster.jpg"
        },
        Children =
        {
            new AdaptiveText()
            {
                Text = "New Message"
            },
            new AdaptiveText()
            {
                Text = "Hey, have you tried Windows 10 yet?",
                HintStyle = AdaptiveTextStyle.CaptionSubtle,
                HintWrap = true
            }
        }
    }
}
```

![ejemplos de imágenes que aparecen](images/adaptive-tiles-imagepeeking.png)

**Recorte por círculo para imágenes que aparecen e imágenes de fondo**

Usa el atributo "hint-crop" en imágenes que aparecen e imágenes de fondo para hacer un recorte por círculo:

```xml
<image placement="peek" hint-crop="circle" src="Assets/Apps/Hipstame/hipster.jpg"/>
```

```csharp
new TilePeekImage()
{
    HintCrop = TilePeekImageCrop.Circle,
    Source = "Assets/Apps/Hipstame/hipster.jpg"
}
```

El resultado tendrá este aspecto:

![recorte por círculo para imagen que aparece e imagen de fondo](images/circlecrop-image.png)

**Usar una imagen que aparece y una imagen de fondo al mismo tiempo**

Para usar tanto una imagen que aparece como una imagen de fondo en una notificación de icono, especifica una imagen que aparece y una imagen de fondo en la carga de notificación.

El resultado tendrá este aspecto:

![imagen que aparece e imagen de fondo en conjunto](images/peekandbackground.png)


### <a name="peek-and-background-image-overlays"></a>Superposiciones de la imagen que aparece y la imagen de fondo

Puedes establecer una superposición negra en la imagen de fondo y la imagen que aparece con **hint-overlay** , que acepta valores enteros de 0 a 100, en los que 0 no supone ninguna superposición y 100 es una superposición completa de color negro. Puedes usar la superposición para garantizar que el texto en el icono puede leerse.

**Usar el atributo "hint-overlay" en una imagen de fondo**

La imagen de fondo tendrá como valor predeterminado un 20 % de superposición mientras tengas algunos elementos de texto en la carga (de lo contrario el valor predeterminado de superposición será de un 0 %).

```xml
<binding template="TileWide">
  <image placement="background" hint-overlay="60" src="Assets\Mostly Cloudy-Background.jpg"/>
  ...
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = "Assets/Mostly Cloudy-Background.jpg",
            HintOverlay = 60
        },

        ...
    }
}
```

**Resultado de la sugerencia de superposición:**

![ejemplo de una superposición de sugerencia de imagen](images/adaptive-tiles-image-hintoverlay.png)

**Usar el atributo hint-overlay en una imagen que aparece**

En la versión 1511 de Windows 10, también se admite una superposición de la imagen que aparece, similar a la imagen de fondo. Especifica el atributo "hint-overlay" en el elemento de la imagen aparece como un número entero de 0 a 100. La superposición de forma predeterminada para las imágenes que aparecen es 0 (no hay superposición alguna).

```xml
<binding template="TileMedium">
  <image hint-overlay="20" src="Assets\Map.jpg" placement="peek"/>
  ...
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        PeekImage = new TilePeekImage()
        {
            Source = "Assets/Map.jpg",
            HintOverlay = 20
        },
        ...
    }
}
```

En este ejemplo se muestra una imagen que aparece con una opacidad del 20 % (izquierda) y una opacidad del 0 % (derecha):

![Atributo hint-overlay en una imagen que aparece](images/hintoverlay.png)

## <a name="vertical-alignment-text-stacking"></a>Alineación vertical (apilamiento de texto)


Puede controlar la alineación vertical del contenido en el icono mediante el atributo **Hint-textStacking** en el elemento [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) y en el elemento [AdaptiveSubgroup](../tiles-and-notifications/tile-schema.md#adaptivesubgroup) . De manera predeterminada, todo el contenido se alinea de forma vertical en la parte superior, pero también puedes alinear contenido en la parte inferior o en el centro.

### <a name="text-stacking-on-binding-element"></a>Apilamiento de texto en el elemento binding

Cuando se aplica en el nivel de [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) , la apilamiento de texto establece la alineación vertical del contenido de la notificación como un todo, alineando en el espacio vertical disponible por encima del área de personalización de marca/distintivo.

```xml
<binding template="TileMedium" hint-textStacking="center" branding="logo">
  <text hint-style="base" hint-align="center">Hi,</text>
  <text hint-style="captionSubtle" hint-align="center">MasterHip</text>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Branding = TileBranding.Logo,
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
        Children =
        {
            new AdaptiveText()
            {
                Text = "Hi,",
                HintStyle = AdaptiveTextStyle.Base,
                HintAlign = AdaptiveTextAlign.Center
            },

            new AdaptiveText()
            {
                Text = "MasterHip",
                HintStyle = AdaptiveTextStyle.CaptionSubtle,
                HintAlign = AdaptiveTextAlign.Center
            }
        }
    }
}
```

![apilamiento de texto en el elemento binding](images/adaptive-tiles-textstack-bindingelement.png)

### <a name="text-stacking-on-subgroup-element"></a>Apilamiento de texto en el elemento subgroup

Cuando se aplica en el nivel de [AdaptiveSubgroup](../tiles-and-notifications/tile-schema.md#adaptivesubgroup) , la apilamiento de texto establece la alineación vertical del contenido de subgrupo (columna), alineando en el espacio vertical disponible en todo el grupo.

```xml
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup hint-weight="33">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-textStacking="center">
      <text hint-style="subtitle">Hi,</text>
      <text hint-style="bodySubtle">MasterHip</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    // Image column
                    new AdaptiveSubgroup()
                    {
                        HintWeight = 33,
                        Children =
                        {
                            new AdaptiveImage()
                            {
                                Source = "Assets/Apps/Hipstame/hipster.jpg",
                                HintCrop = AdaptiveImageCrop.Circle
                            }
                        }
                    },

                    // Text column
                    new AdaptiveSubgroup()
                    {
                        // Vertical align its contents
                        TextStacking = TileTextStacking.Center,
                        Children =
                        {
                            new AdaptiveText()
                            {
                                Text = "Hi,",
                                HintStyle = AdaptiveTextStyle.Subtitle
                            },

                            new AdaptiveText()
                            {
                                Text = "MasterHip",
                                HintStyle = AdaptiveTextStyle.BodySubtle
                            }
                        }
                    }
                }
            }
        }
    }
}
```

## <a name="related-topics"></a>Temas relacionados
* [Esquema de contenido de icono](../tiles-and-notifications/tile-schema.md)
* [Enviar una notificación de icono local](sending-a-local-tile-notification.md)
* [Plantillas de iconos especiales](special-tile-templates-catalog.md)
* [Kit de herramientas de la comunidad de UWP: notificaciones](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [Notificaciones de Windows en GitHub](https://github.com/WindowsNotifications)

 

 
