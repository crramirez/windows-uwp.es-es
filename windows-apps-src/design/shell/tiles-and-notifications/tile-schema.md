---
description: En el siguiente artículo se describen todas las propiedades y los elementos del contenido del mosaico.
title: Esquema de contenido de icono
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Tile content schema
template: detail.hbs
ms.date: 07/28/2017
ms.topic: article
keywords: Windows 10, UWP, icono, notificación de icono, contenido de mosaico, esquema, carga de iconos
ms.localizationpriority: medium
ms.openlocfilehash: 4d1953e6d745a41c3bdd85d5f4dd3c6c9df8b900
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034418"
---
# <a name="tile-content-schema"></a>Esquema de contenido de icono

 

A continuación se describen todas las propiedades y los elementos del contenido del mosaico.

Si prefiere usar XML sin formato en lugar de la [biblioteca de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), consulte [el esquema XML](../tiles-and-notifications/adaptive-tiles-schema.md).

[TileContent](#tilecontent)
* [TileVisual](#tilevisual)
  * [TileBinding](#tilebinding)
    * [TileBindingContentAdaptive](#tilebindingcontentadaptive)
    * [TileBindingContentIconic](#tilebindingcontenticonic)
    * [TileBindingContentContact](#tilebindingcontentcontact)
    * [TileBindingContentPeople](#tilebindingcontentpeople)
    * [TileBindingContentPhotos](#tilebindingcontentphotos)


## <a name="tilecontent"></a>TileContent
TileContent es el objeto de nivel superior que describe el contenido de una notificación de icono, incluidos los objetos visuales.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Visual** | [ToastVisual](#tilevisual) | true | Describe la parte visual de la notificación de icono. |


## <a name="tilevisual"></a>TileVisual
La parte visual de los mosaicos contiene las especificaciones visuales para todos los tamaños de mosaico y más propiedades relacionadas con visual.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **TileSmall** | [TileBinding](#tilebinding) | false | Proporcione un enlace pequeño opcional para especificar el contenido del tamaño de mosaico pequeño. |
| **TileMedium** | [TileBinding](#tilebinding) | false | Proporcione un enlace medio opcional para especificar el contenido del tamaño de mosaico medio. |
| **TileWide** | [TileBinding](#tilebinding) | false | Proporcione un enlace ancho opcional para especificar el contenido del tamaño de mosaico ancho. |
| **TileLarge** | [TileBinding](#tilebinding) | false | Proporcione un enlace grande opcional para especificar el contenido del tamaño de mosaico grande. |
| **Personalización de marca** | TileBranding | false | El formulario que el icono debe usar para mostrar la marca de la aplicación. De forma predeterminada, hereda la personalización de marca del mosaico predeterminado. |
| **DisplayName** | string | false | Una cadena opcional para invalidar el nombre para mostrar del mosaico mientras se muestra esta notificación. |
| **Argumentos** | string | false | Novedades de la actualización de aniversario: datos definidos por la aplicación que se devuelven a la aplicación a través de la propiedad TileActivatedInfo en LaunchActivatedEventArgs cuando el usuario inicia la aplicación desde el icono dinámico. Esto le permite saber qué notificaciones de icono vio su usuario al puntear en el icono dinámico. En los dispositivos sin la actualización de aniversario, esto simplemente se omitirá. |
| **LockDetailedStatus1** | string | false | Si especifica esto, también debe proporcionar un enlace TileWide. Esta es la primera línea de texto que se mostrará en la pantalla de bloqueo si el usuario ha seleccionado el icono como su aplicación de estado detallada. |
| **LockDetailedStatus2** | string | false | Si especifica esto, también debe proporcionar un enlace TileWide. Esta es la segunda línea de texto que se mostrará en la pantalla de bloqueo si el usuario ha seleccionado el icono como su aplicación de estado detallada. |
| **LockDetailedStatus3** | string | false | Si especifica esto, también debe proporcionar un enlace TileWide. Esta es la tercera línea de texto que se mostrará en la pantalla de bloqueo si el usuario ha seleccionado el icono como su aplicación de estado detallada. |
| **BaseUri** | Identificador URI | false | Una dirección URL base predeterminada que se combina con las direcciones URL relativas de los atributos de origen de la imagen. |
| **AddImageQuery** | booleano? | false | Establézcalo en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación del sistema. Utilice este atributo si el servidor hospeda imágenes y puede controlar las cadenas de consulta, ya sea mediante la recuperación de una variante de imagen basada en las cadenas de consulta o mediante la omisión de la cadena de consulta y la devolución de la imagen como se especifica sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&MS-lang = en-US" |
| **Lenguaje**| string | false | La configuración regional de destino de la carga visual cuando se usan recursos localizados, especificada como etiquetas de idioma BCP-47 como "en-US" o "fr-FR". Cualquier configuración regional especificada en el enlace o texto invalida esta configuración regional. Si no se proporciona, se usará la configuración regional del sistema en su lugar. |


## <a name="tilebinding"></a>TileBinding
El objeto de enlace contiene el contenido visual para un tamaño de mosaico específico.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Contenido** | [ITileBindingContent](#itilebindingcontent) | false | Contenido visual que se va a mostrar en el mosaico. Uno de [TileBindingContentAdaptive](#tilebindingcontentadaptive), [TileBindingContentIconic](#tilebindingcontenticonic), [TileBindingContentContact](#tilebindingcontentcontact), [TileBindingContentPeople](#tilebindingcontentpeople)o [TileBindingContentPhotos](#tilebindingcontentphotos). |
| **Personalización de marca** | TileBranding | false | El formulario que el icono debe usar para mostrar la marca de la aplicación. De forma predeterminada, hereda la personalización de marca del mosaico predeterminado. |
| **DisplayName** | string | false | Una cadena opcional para reemplazar el nombre para mostrar del mosaico para este tamaño de mosaico. |
| **Argumentos** | string | false | Novedades de la actualización de aniversario: datos definidos por la aplicación que se devuelven a la aplicación a través de la propiedad TileActivatedInfo en LaunchActivatedEventArgs cuando el usuario inicia la aplicación desde el icono dinámico. Esto le permite saber qué notificaciones de icono vio su usuario al puntear en el icono dinámico. En los dispositivos sin la actualización de aniversario, esto simplemente se omitirá. |
| **BaseUri** | Identificador URI | false | Una dirección URL base predeterminada que se combina con las direcciones URL relativas de los atributos de origen de la imagen. |
| **AddImageQuery** | booleano? | false | Establézcalo en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación del sistema. Utilice este atributo si el servidor hospeda imágenes y puede controlar las cadenas de consulta, ya sea mediante la recuperación de una variante de imagen basada en las cadenas de consulta o mediante la omisión de la cadena de consulta y la devolución de la imagen como se especifica sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&MS-lang = en-US" |
| **Lenguaje**| string | false | La configuración regional de destino de la carga visual cuando se usan recursos localizados, especificada como etiquetas de idioma BCP-47 como "en-US" o "fr-FR". Cualquier configuración regional especificada en el enlace o texto invalida esta configuración regional. Si no se proporciona, se usará la configuración regional del sistema en su lugar. |


## <a name="itilebindingcontent"></a>ITileBindingContent
Interfaz de marcador para el contenido de enlace de mosaicos. Estos permiten elegir lo que desea especificar como elementos visuales de icono en adaptable o una de las plantillas especiales.

| Implementaciones |
| --- |
| [TileBindingContentAdaptive](#tilebindingcontentadaptive) |
| [TileBindingContentIconic](#tilebindingcontenticonic) |
| [TileBindingContentContact](#tilebindingcontentcontact) |
| [TileBindingContentPeople](#tilebindingcontentpeople) |
| [TileBindingContentPhotos](#tilebindingcontentphotos) |


## <a name="tilebindingcontentadaptive"></a>TileBindingContentAdaptive
Se admite en todos los tamaños. Esta es la manera recomendada de especificar el contenido del icono. Plantillas de iconos adaptables nuevas en Windows 10, y puede crear una amplia variedad de mosaicos personalizados a través de Adaptive.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Children** | IList<ITileBindingContentAdaptiveChild> | false | Elementos visuales insertados. Se pueden agregar objetos [AdaptiveText](#adaptivetext), [AdaptiveImage](#adaptiveimage)y [AdaptiveGroup](#adaptivegroup) . Los elementos secundarios se muestran en un estilo vertical de StackPanel. |
| **BackgroundImage** | [TileBackgroundImage](#tilebackgroundimage) | false | Una imagen de fondo opcional que se muestra detrás de todo el contenido del icono, sangrado completo. |
| **PeekImage** | [TilePeekImage](#tilepeekimage) | false | Una imagen de lectura opcional que anima desde la parte superior del icono. |
| **TextStacking** | [TileTextStacking](#tiletextstacking) | false | Controla la pila de texto (alineación vertical) del contenido secundario en conjunto. |


## <a name="adaptivetext"></a>AdaptiveText
Elemento de texto adaptable.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Texto** | string | false | Texto que se va a mostrar. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | El estilo controla el tamaño de fuente, el peso y la opacidad del texto. |
| **HintWrap** | booleano? | false | Establézcalo en true para habilitar el ajuste de texto. El valor predeterminado es false. |
| **HintMaxLines** | int? | false | Número máximo de líneas que el elemento de texto puede mostrar. |
| **HintMinLines** | int? | false | Número mínimo de líneas que debe mostrar el elemento de texto. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | Alineación horizontal del texto. |
| **Lenguaje** | string | false | La configuración regional de destino de la carga XML, especificada como una etiqueta de idioma BCP-47 como "en-US" o "fr-FR". La configuración regional especificada aquí invalida cualquier otra configuración regional especificada, como la del enlace o el visual. Si este valor es una cadena literal, este atributo tiene como valor predeterminado el idioma de la interfaz de usuario del usuario. Si este valor es una referencia de cadena, este atributo tiene como valor predeterminado la configuración regional elegida por Windows Runtime en la resolución de la cadena. |


### <a name="adaptivetextstyle"></a>AdaptiveTextStyle
Estilo de texto controla el tamaño de fuente, el peso y la opacidad. La opacidad sutil es 60% opaca.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | Valor predeterminado. El representador determina el estilo. |
| **Caption** | Menor que el tamaño de fuente del párrafo. |
| **CaptionSubtle** | Igual que la leyenda, pero con una ligera opacidad. |
| **Cuerpo** | Tamaño de fuente del párrafo. |
| **BodySubtle** | Igual que el cuerpo, pero con una ligera opacidad. |
| **Básica** | Tamaño de fuente del párrafo, grosor de negrita. Esencialmente, la versión en negrita del cuerpo. |
| **BaseSubtle** | Igual que la base, pero con una opacidad sutil. |
| **Subtítulo** | Tamaño de fuente de H4. |
| **SubtitleSubtle** | Igual que el subtítulo, pero con una opacidad sutil. |
| **Título** | Tamaño de fuente H3. |
| **TitleSubtle** | Igual que el título, pero con una ligera opacidad. |
| **TitleNumeral** | Igual que el título, pero con el relleno superior/inferior quitado. |
| **Subencabezado** | Tamaño de fuente H2. |
| **SubheaderSubtle** | Igual que el subencabezado, pero con una ligera opacidad. |
| **SubheaderNumeral** | Igual que el subencabezado, pero se ha quitado el relleno superior/inferior. |
| **Encabezado** | Tamaño de fuente H1. |
| **HeaderSubtle** | Igual que el encabezado, pero con una ligera opacidad. |
| **HeaderNumeral** | Igual que el encabezado, pero se ha quitado el relleno superior/inferior. |


### <a name="adaptivetextalign"></a>AdaptiveTextAlign
Controla los enlineamientos horizontales del texto.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | Valor predeterminado. El representador determina automáticamente la alineación. |
| **Automático** | Alineación determinada por el idioma y la referencia cultural actuales. |
| **Left** | Alinear horizontalmente el texto a la izquierda. |
| **Centro** | Alinear horizontalmente el texto en el centro. |
| **Right** | Alinear horizontalmente el texto a la derecha. |


## <a name="adaptiveimage"></a>AdaptiveImage
Una imagen alineada.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Origen** | string | true | Dirección URL de la imagen. se admiten MS-appx, MS-AppData y http. A partir de la actualización de Fall Creators, las imágenes Web pueden tener hasta 3 MB en las conexiones normales y 1 MB en las conexiones de uso medido. En los dispositivos que aún no ejecuten el Fall Creators Update, las imágenes web no deben tener más de 200 KB. |
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | Controle el recorte deseado de la imagen. |
| **HintRemoveMargin** | booleano? | false | De forma predeterminada, las imágenes dentro de grupos o subgrupos tienen un margen 8px alrededor de ellas. Puede quitar este margen estableciendo esta propiedad en true. |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | Alineación horizontal de la imagen. |
| **AlternateText** | string | false | Texto alternativo que describe la imagen, que se usa para fines de accesibilidad. |
| **AddImageQuery** | booleano? | false | Establézcalo en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación de icono. Utilice este atributo si el servidor hospeda imágenes y puede controlar las cadenas de consulta, ya sea mediante la recuperación de una variante de imagen basada en las cadenas de consulta o mediante la omisión de la cadena de consulta y la devolución de la imagen como se especifica sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&MS-lang = en-US" |


### <a name="adaptiveimagecrop"></a>AdaptiveImageCrop
Especifica el recorte deseado de la imagen.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | Valor predeterminado. Comportamiento de recorte determinado por el representador. |
| **None** | La imagen no se recorta. |
| **Circle** | La imagen se recorta a una forma circular. |


### <a name="adaptiveimagealign"></a>AdaptiveImageAlign
Especifica la alineación horizontal de una imagen.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | Valor predeterminado. Comportamiento de alineación determinado por el representador. |
| **Stretch** | La imagen se ajusta para rellenar el ancho disponible (y posiblemente también el alto disponible, en función de dónde se coloque la imagen). |
| **Left** | Alinee la imagen a la izquierda y muestre la imagen en su resolución nativa. |
| **Centro** | Alinee la imagen en el centro horizontalmente, displayign en su resolución nativa. |
| **Right** | Alinee la imagen a la derecha y muestre la imagen en su resolución nativa. |


## <a name="adaptivegroup"></a>AdaptiveGroup
Los grupos identifican semánticamente que el contenido del grupo se debe mostrar como un todo, o no se muestra si no cabe. Los grupos también permiten crear varias columnas.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | Los subgrupos se muestran como columnas verticales. Debe usar subgrupos para proporcionar contenido dentro de un AdaptiveGroup. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
Los subgrupos son columnas verticales que pueden contener texto e imágenes.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Children** | IList<[IAdaptiveSubgroupChild](#iadaptivesubgroupchild)> | false | [AdaptiveText](#adaptivetext) y [AdaptiveImage](#adaptiveimage) son elementos secundarios válidos de subgrupos. |
| **HintWeight** | int? | false | Controle el ancho de esta columna de subgrupo especificando el peso con respecto a los demás subgrupos. |
| **HintTextStacking** | [AdaptiveSubgroupTextStacking](#adaptivesubgrouptextstacking) | false | Controlar la alineación vertical del contenido de este subgrupo. |


### <a name="iadaptivesubgroupchild"></a>IAdaptiveSubgroupChild
Interfaz de marcador para secundarios de subgrupo.

| Implementaciones |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |


### <a name="adaptivesubgrouptextstacking"></a>AdaptiveSubgroupTextStacking
TextStacking especifica la alineación vertical del contenido.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | Valor predeterminado. El representador selecciona automáticamente la alineación vertical predeterminada. |
| **Top** (Principales) | Alineación vertical en la parte superior. |
| **Centro** | Alineación vertical con el centro. |
| **Bottom** | Alineación vertical en la parte inferior. |


## <a name="tilebackgroundimage"></a>TileBackgroundImage
Una imagen de fondo que se muestra con sangrado completo en el icono.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Origen** | string | true | Dirección URL de la imagen. se admiten MS-appx, MS-AppData y http (s). Las imágenes http deben tener un tamaño de 200 KB o menos. |
| **HintOverlay** | int? | false | Una superposición de color negro en la imagen de fondo. Este valor controla la opacidad de la superposición de color negro, donde 0 no es ninguna superposición y 100 es completamente negro. El valor predeterminado es 20. |
| **HintCrop** | [TileBackgroundImageCrop](#tilebackgroundimagecrop) | false | Novedad en 1511: especifique cómo desea recortar la imagen. En las versiones anteriores a 1511, se omitirá y la imagen de fondo se mostrará sin recorte. |
| **AlternateText** | string | false | Texto alternativo que describe la imagen, que se usa para fines de accesibilidad. |
| **AddImageQuery** | booleano? | false | Establézcalo en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación de icono. Utilice este atributo si el servidor hospeda imágenes y puede controlar las cadenas de consulta, ya sea mediante la recuperación de una variante de imagen basada en las cadenas de consulta o mediante la omisión de la cadena de consulta y la devolución de la imagen como se especifica sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&MS-lang = en-US" |


### <a name="tilebackgroundimagecrop"></a>TileBackgroundImageCrop
Controla el recorte de la imagen de fondo.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | El recorte usa el comportamiento predeterminado del representador. |
| **None** | La imagen no se recorta, se muestra un cuadrado. |
| **Circle** | La imagen se recorta a un círculo. |


## <a name="tilepeekimage"></a>TilePeekImage
Imagen de inspección que anima desde la parte superior del icono.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Origen** | string | true | Dirección URL de la imagen. se admiten MS-appx, MS-AppData y http (s). Las imágenes http deben tener un tamaño de 200 KB o menos. |
| **HintOverlay** | int? | false | Novedad en 1511: superposición de color negro en la imagen de inspección. Este valor controla la opacidad de la superposición de color negro, donde 0 no es ninguna superposición y 100 es completamente negro. El valor predeterminado es 20. En versiones anteriores, este valor se omitirá y la imagen PEEK se mostrará con una superposición 0. |
| **HintCrop** | [TilePeekImageCrop](#tilepeekimagecrop) | false | Novedad en 1511: especifique cómo desea recortar la imagen. En las versiones anteriores a 1511, se omitirá y se mostrará la imagen de PEEK sin recortes. |
| **AlternateText** | string | false | Texto alternativo que describe la imagen, que se usa para fines de accesibilidad. |
| **AddImageQuery** | booleano? | false | Establézcalo en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación de icono. Utilice este atributo si el servidor hospeda imágenes y puede controlar las cadenas de consulta, ya sea mediante la recuperación de una variante de imagen basada en las cadenas de consulta o mediante la omisión de la cadena de consulta y la devolución de la imagen como se especifica sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&MS-lang = en-US" |


### <a name="tilepeekimagecrop"></a>TilePeekImageCrop
Controla el recorte de la imagen de inspección.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | El recorte usa el comportamiento predeterminado del representador. |
| **None** | La imagen no se recorta, se muestra un cuadrado. |
| **Circle** | La imagen se recorta a un círculo. |


### <a name="tiletextstacking"></a>TileTextStacking
El apilamiento de texto especifica la alineación vertical del contenido.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | Valor predeterminado. El representador selecciona automáticamente la alineación vertical predeterminada. |
| **Top** (Principales) | Alineación vertical en la parte superior. |
| **Centro** | Alineación vertical con el centro. |
| **Bottom** | Alineación vertical en la parte inferior. |


## <a name="tilebindingcontenticonic"></a>TileBindingContentIconic
Se admite en pequeñas y medianas. Habilita una plantilla de mosaico de iconos, donde puede tener un icono y una presentación de distintivos junto a los demás en el icono, en verdadero estilo de Windows Phone clásico. El número que aparece junto al icono se consigue mediante una notificación de notificación independiente.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Icono** | [TileBasicImage](#tilebasicimage) | true | Como mínimo, para admitir iconos de escritorio y móviles, pequeños y medianos, proporcione una imagen de relación de aspecto cuadrada con una resolución de 200 x 200, formato PNG, con transparencia y sin color que no sea blanco. Para obtener más información, consulte: [plantillas de iconos especiales](../tiles-and-notifications/special-tile-templates-catalog.md). |


## <a name="tilebindingcontentcontact"></a>TileBindingContentContact
Solo para dispositivos móviles. Se admite en pequeñas, medianas y anchas.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Imagen** | [TileBasicImage](#tilebasicimage) | true | Imagen que se va a mostrar. |
| **Texto** | [TileBasicText](#tilebasictext) | false | Línea de texto que se muestra. No se muestra en el icono pequeño. |


## <a name="tilebindingcontentpeople"></a>TileBindingContentPeople
Novedad en 1511: se admite en el nivel medio, ancho y grande (escritorio y móvil). Anteriormente, esto era solo móvil y solo es mediano y ancho.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Imágenes** | IList<[TileBasicImage](#tilebasicimage)> | true | Imágenes que se retirarán como círculos. |


## <a name="tilebindingcontentphotos"></a>TileBindingContentPhotos
Anima a través de una presentación de fotos. Se admite en todos los tamaños.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Imágenes** | IList<[TileBasicImage](#tilebasicimage)> | true | Se pueden proporcionar hasta 12 imágenes (Mobile solo mostrará hasta 9), que se usarán para la presentación. Al agregar más de 12 se producirá una excepción. |


### <a name="tilebasicimage"></a>TileBasicImage
Imagen utilizada en varias plantillas especiales.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Origen** | string | true | Dirección URL de la imagen. se admiten MS-appx, MS-AppData y http (s). Las imágenes http deben tener un tamaño de 200 KB o menos. |
| **AlternateText** | string | false | Texto alternativo que describe la imagen, que se usa para fines de accesibilidad. |
| **AddImageQuery** | booleano? | false | Establézcalo en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación de icono. Utilice este atributo si el servidor hospeda imágenes y puede controlar las cadenas de consulta, ya sea mediante la recuperación de una variante de imagen basada en las cadenas de consulta o mediante la omisión de la cadena de consulta y la devolución de la imagen como se especifica sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&MS-lang = en-US" |


### <a name="tilebasictext"></a>TileBasicText
Elemento de texto básico que se usa en varias plantillas especiales.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Texto** | string | false | Texto que se va a mostrar. |
| **Lenguaje** | string | false | La configuración regional de destino de la carga XML, especificada como una etiqueta de idioma BCP-47 como "en-US" o "fr-FR". La configuración regional especificada aquí invalida cualquier otra configuración regional especificada, como la del enlace o el visual. Si este valor es una cadena literal, este atributo tiene como valor predeterminado el idioma de la interfaz de usuario del usuario. Si este valor es una referencia de cadena, este atributo tiene como valor predeterminado la configuración regional elegida por Windows Runtime en la resolución de la cadena. |


## <a name="related-topics"></a>Temas relacionados

* [Inicio rápido: Enviar una notificación de icono local](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [Biblioteca de notificaciones en GitHub](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/dev/Notifications)
