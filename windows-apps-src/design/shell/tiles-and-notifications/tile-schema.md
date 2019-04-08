---
Description: En el siguiente artículo se describen todas las propiedades y elementos dentro del contenido de la ventana.
title: Esquema de contenido de icono
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Tile content schema
template: detail.hbs
ms.date: 07/28/2017
ms.topic: article
keywords: windows 10, uwp, ventana, notificación de ventana, contenido de ventana, esquema, carga de ventana
ms.localizationpriority: medium
ms.openlocfilehash: f12f1c2b6ac158b6f8e837fd3d6a64f96939ed99
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642990"
---
# <a name="tile-content-schema"></a>Esquema de contenido de icono

 

En el siguiente artículo se describen todas las propiedades y elementos dentro del contenido de la ventana.

Si prefieres utilizar XML sin formato en lugar de la [Biblioteca Notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), consulta [el esquema XML](../tiles-and-notifications/adaptive-tiles-schema.md).

[TileContent](#tilecontent)
* [TileVisual](#tilevisual)
  * [TileBinding](#tilebinding)
    * [TileBindingContentAdaptive](#TileBindingContentAdaptive)
    * [TileBindingContentIconic](#TileBindingContentIconic)
    * [TileBindingContentContact](#TileBindingContentContact)
    * [TileBindingContentPeople](#TileBindingContentPeople)
    * [TileBindingContentPhotos](#TileBindingContentPhotos)


## <a name="tilecontent"></a>TileContent
TileContent es el objeto de nivel superior que describe el contenido de una ventana, incluidos los elementos visuales.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Visual** | [ToastVisual](#tilevisual) | true | Describe la parte visual de la notificación de la ventana. |


## <a name="tilevisual"></a>TileVisual
La parte visual de las ventanas contiene las especificaciones visuales para todos los tamaños de la ventana así como otras propiedades visuales.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **TileSmall** | [TileBinding](#tilebinding) | falso | Proporciona un pequeño enlace opcional para especificar contenido del tamaño pequeño de la ventana. |
| **TileMedium** | [TileBinding](#tilebinding) | falso | Proporciona un enlace opcional mediano para especificar contenido del tamaño mediano de la ventana. |
| **TileWide** | [TileBinding](#tilebinding) | falso | Proporciona un enlace ancho opcional para especificar contenido del tamaño ancho de la ventana. |
| **TileLarge** | [TileBinding](#tilebinding) | falso | Proporciona un enlace grande opcional para especificar contenido del tamaño grande de la ventana. |
| **Personalización de marca** | [TileBranding](#tilebranding) | falso | La forma en la que debe usarse la ventana para mostrar la marca de la aplicación. De manera predeterminada, hereda la personalización de marca de la ventana predeterminada. |
| **DisplayName** | string | falso | Una cadena opcional para invalidar el nombre para mostrar de la ventana cuando se muestre esta notificación. |
| **Argumentos** | string | falso | Novedad en la actualización de aniversario de: Datos definidos en la aplicación que se pasan a la aplicación a través de la propiedad TileActivatedInfo en LaunchActivatedEventArgs cuando el usuario inicia la aplicación desde la ventana viva. Esto te permite saber las notificaciones de ventana que el usuario vio cuando pulsó tu Icono dinámico. En los dispositivos sin la actualización de aniversario, simplemente se omitirá. |
| **LockDetailedStatus1** | string | falso | Si se especifica, también debes proporcionar un enlace de TileWide. Se trata de la primera línea de texto que se mostrará en la pantalla de bloqueo, si el usuario ha seleccionado tu ventana como la aplicación de estado detallada. |
| **LockDetailedStatus2** | string | falso | Si se especifica, también debes proporcionar un enlace de TileWide. Se trata de la segunda línea de texto que se mostrará en la pantalla de bloqueo, si el usuario ha seleccionado tu ventana como la aplicación de estado detallada. |
| **LockDetailedStatus3** | string | falso | Si se especifica, también debes proporcionar un enlace de TileWide. Se trata de la tercera línea de texto que se mostrará en la pantalla de bloqueo, si el usuario ha seleccionado tu ventana como la aplicación de estado detallada. |
| **baseUri** | Uri | falso | URL base predeterminada que se combina con direcciones URL relativas en atributos de origen de imagen. |
| **AddImageQuery** | bool? | falso | Establece el valor en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación del sistema. Usa este atributo si el servidor aloja imágenes y puede controlar cadenas de consulta, ya sea recuperando una variante de imagen en función de las cadenas de consulta u omitiendo la cadena de consulta y devolviendo la imagen como se especificó sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us". |
| **Idioma**| string | falso | La configuración regional de destino de la carga visual al usar recursos localizados, especificados como etiquetas de idioma BCP-47 como "en-US" o "fr-FR". Esta configuración regional se reemplaza por cualquier configuración regional especificada en el enlace o texto. Si no se proporciona, se usará la configuración regional del sistema en su lugar. |


## <a name="tilebinding"></a>TileBinding
El objeto de enlace contiene el contenido visual de un tamaño específico de ventana.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Contenido** | [ITileBindingContent](#itilebindingcontent) | falso | El contenido visual para mostrar en la ventana. Uno de [TileBindingContentAdaptive](#tilebindingcontentadaptive), [TileBindingContentIconic](#TileBindingContentIconic), [TileBindingContentContact](#TileBindingContentContact), [TileBindingContentPeople](#TileBindingContentPeople), o [TileBindingContentPhotos](#TileBindingContentPhotos). |
| **Personalización de marca** | TileBranding | falso | La forma en la que debe usarse la ventana para mostrar la marca de la aplicación. De manera predeterminada, hereda la personalización de marca de la ventana predeterminada. |
| **DisplayName** | string | falso | Una cadena opcional para invalidar el nombre para mostrar de la ventana para este tamaño. |
| **Argumentos** | string | falso | Novedad en la actualización de aniversario de: Datos definidos en la aplicación que se pasan a la aplicación a través de la propiedad TileActivatedInfo en LaunchActivatedEventArgs cuando el usuario inicia la aplicación desde la ventana viva. Esto te permite saber las notificaciones de ventana que el usuario vio cuando pulsó tu Icono dinámico. En los dispositivos sin la actualización de aniversario, simplemente se omitirá. |
| **baseUri** | Uri | falso | URL base predeterminada que se combina con direcciones URL relativas en atributos de origen de imagen. |
| **AddImageQuery** | bool? | falso | Establece el valor en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación del sistema. Usa este atributo si el servidor aloja imágenes y puede controlar cadenas de consulta, ya sea recuperando una variante de imagen en función de las cadenas de consulta u omitiendo la cadena de consulta y devolviendo la imagen como se especificó sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us". |
| **Idioma**| string | falso | La configuración regional de destino de la carga visual al usar recursos localizados, especificados como etiquetas de idioma BCP-47 como "en-US" o "fr-FR". Esta configuración regional se reemplaza por cualquier configuración regional especificada en el enlace o texto. Si no se proporciona, se usará la configuración regional del sistema en su lugar. |


## <a name="itilebindingcontent"></a>ITileBindingContent
La interfaz de marcador para contenido de enlace de la ventana. Estos le permiten elegir dónde quieres especificar los elementos visuales del icono: adaptable, o en una de las plantillas especiales.

| Implementaciones |
| --- |
| [TileBindingContentAdaptive](#TileBindingContentAdaptive) |
| [TileBindingContentIconic](#TileBindingContentIconic) |
| [TileBindingContentContact](#TileBindingContentContact) |
| [TileBindingContentPeople](#TileBindingContentPeople) |
| [TileBindingContentPhotos](#TileBindingContentPhotos) |


## <a name="tilebindingcontentadaptive"></a>TileBindingContentAdaptive
Compatible con todos los tamaños. Esta es la manera recomendada de especificar el contenido de la ventana. Las plantillas de iconos adaptables es una novedad en Windows 10 y te permite crear una gran variedad de iconos personalizados mediante el diseño adaptable.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Elementos secundarios** | IList<ITileBindingContentAdaptiveChild> | falso | Elementos visuales alineados. Los objetos [AdaptiveText](#adaptivetext), [AdaptiveImage](#adaptiveimage), y [AdaptiveGroup](#adaptivegroup) pueden agregarse. Los elementos secundarios se muestran en una forma vertical de StackPanel. |
| **BackgroundImage** | [TileBackgroundImage](#tilebackgroundimage) | falso | Una imagen de fondo opcional que obtiene muestra detrás del contenido de la ventana, sin bordes. |
| **PeekImage** | [TilePeekImage](#tilepeekimage) | falso | Una animación de imagen que aparezca desde la parte superior de la ventana. |
| **TextStacking** | [TileTextStacking](#tiletextstacking) | falso | Controla el apilamiento de texto (alineación vertical) del contenido de los elementos secundarios como un todo. |


## <a name="adaptivetext"></a>AdaptiveText
Elemento de texto adaptable.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Texto** | string | falso | El texto que se va a mostrar. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | falso | El estilo controla el tamaño, el espesor y la opacidad de la fuente del texto. |
| **HintWrap** | bool? | falso | Establece esto en true para habilitar el ajuste de texto. El valor predeterminado es "false". |
| **HintMaxLines** | int? | falso | El número máximo de líneas que se permite que mostrar al elemento de texto. |
| **HintMinLines** | int? | falso | El número mínimo de líneas que debe mostrar el elemento de texto. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | falso | Alineación horizontal del texto. |
| **Idioma** | string | falso | La configuración regional de destino de la carga XML, especificada como una etiqueta de idioma BCP-47, como "en-US" o "fr-FR". La configuración regional especificada aquí reemplaza cualquier otra configuración regional especificada, como las de enlace o visual. Si este valor es una cadena literal, el valor predeterminado de este atributo es el idioma de la interfaz de usuario del usuario. Si este valor es una referencia de cadena, el valor predeterminado de este atributo será la configuración regional elegida por Windows Runtime en tiempo de ejecución en la resolución de la cadena. |


### <a name="adaptivetextstyle"></a>AdaptiveTextStyle
El estilo de texto controla el tamaño, el espesor y la opacidad de la fuente. La opacidad sutil es 60 % opaco.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | Valor predeterminado. El estilo viene determinado por el representador. |
| **Título** | De menor tamaño que el tamaño de la fuente del párrafo. |
| **captionSubtle** | Igual que Caption pero con opacidad sutil. |
| **Cuerpo** | Tamaño de fuente del párrafo. |
| **bodySubtle** | Igual que Body pero con opacidad sutil. |
| **base** | Tamaño de fuente de párrafo, espesor de negrita. Básicamente, la versión negrita de Body. |
| **baseSubtle** | Igual que Base pero con opacidad sutil. |
| **Subtítulo** | Tamaño de fuente H4 |
| **subtitleSubtle** | Igual que Subtitle pero con opacidad sutil. |
| **Título** | Tamaño de fuente H3 |
| **titleSubtle** | Igual que Title pero con opacidad sutil. |
| **titleNumeral** | Igual que Title pero con el espaciado superior o inferior quitado. |
| **Subencabezado** | Tamaño de fuente H2 |
| **subheaderSubtle** | Igual que Subheader pero con opacidad sutil. |
| **subheaderNumeral** | Igual que Subheader pero con el espaciado superior o inferior quitado. |
| **Encabezado** | Tamaño de fuente H1 |
| **headerSubtle** | Igual que Header pero con opacidad sutil. |
| **headerNumeral** | Igual que Header pero con el espaciado superior o inferior quitado. |


### <a name="adaptivetextalign"></a>AdaptiveTextAlign
Controla la alineación horizontal del texto.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | Valor predeterminado. La alineación la determina automáticamente el representador. |
| **Auto** | Alineación determinada por el idioma y la referencia cultural actuales. |
| **Izquierda** | Alinea horizontalmente el texto a la izquierda. |
| **Centro** | Alinea el texto horizontalmente en el centro. |
| **Correcto** | Alinea el texto horizontalmente a la derecha. |


## <a name="adaptiveimage"></a>AdaptiveImage
Imagen alineada.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Origen** | string | true | La dirección URL de la imagen. Se admiten ms-appx, ms-appdata y http. A partir de la actualización de Fall Creators Update, las imágenes web pueden tener un tamaño de hasta 3 MB, en conexiones normales y de 1 MB en conexiones de uso medido. En dispositivos que no ejecutan aún la actualización de Fall Creators Update, el tamaño de las imágenes web no debe ser superior a los 200 KB. |
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | falso | Controla el recorte deseado de la imagen. |
| **HintRemoveMargin** | bool? | falso | De manera predeterminada, las imágenes dentro de grupos o subgrupos tienen un margen de 8 píxeles alrededor de ellas. Puedes quitar este margen estableciendo esta propiedad en true. |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | falso | Alineación horizontal de la imagen. |
| **AlternateText** | string | falso | Texto alternativo que describe la imagen, que se usa para fines de accesibilidad. |
| **AddImageQuery** | bool? | falso | Establece el valor en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación de la ventana. Usa este atributo si el servidor aloja imágenes y puede controlar cadenas de consulta, ya sea recuperando una variante de imagen en función de las cadenas de consulta u omitiendo la cadena de consulta y devolviendo la imagen como se especificó sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us". |


### <a name="adaptiveimagecrop"></a>AdaptiveImageCrop
Especifica el recorte deseado de la imagen.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | Valor predeterminado. Comportamiento de recorte determinado por el representador. |
| **Ninguno** | No se recorta la imagen. |
| **Círculo** | Se recorta la imagen a una forma de círculo. |


### <a name="adaptiveimagealign"></a>AdaptiveImageAlign
Especifica la alineación horizontal para una imagen.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | Valor predeterminado. Comportamiento de alineación determinado por el representador. |
| **Stretch** | La imagen se amplía para rellenar el ancho disponible (y potencialmente también el alto disponible, en función de donde se coloque la imagen). |
| **Izquierda** | Alinea la imagen a la izquierda y la muestra en su resolución nativa. |
| **Centro** | Alinea la imagen en el centro de forma horizontal y la muestra en su resolución nativa. |
| **Correcto** | Alinea la imagen a la derecha y la muestra en su resolución nativa. |


## <a name="adaptivegroup"></a>AdaptiveGroup
Los grupos identifican semánticamente que el contenido del grupo debe mostrarse entero o no mostrarse si no cabe. Los grupos también permiten la creación de varias columnas.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Elementos secundarios** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | falso | Los subgrupos se muestran como columnas verticales. Debes usar subgrupos para proporcionar cualquier contenido dentro de un AdaptiveGroup. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
Los subgrupos son columnas verticales que pueden contener texto e imágenes.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Elementos secundarios** | IList<[IAdaptiveSubgroupChild](#iadaptivesubgroupchild)> | falso | [AdaptiveText](#adaptivetext) y [AdaptiveImage](#adaptiveimage) son elementos secundarios válidos de subgrupos. |
| **HintWeight** | int? | falso | Controla el ancho de esta columna de subgrupo especificando el grosor, en relación con los demás subgrupos. |
| **HintTextStacking** | [AdaptiveSubgroupTextStacking](#adaptivesubgrouptextstacking) | falso | Controla la alineación vertical del contenido de este subgrupo. |


### <a name="iadaptivesubgroupchild"></a>IAdaptiveSubgroupChild
Interfaz de marcador para elementos secundarios de subgrupo.

| Implementaciones |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |


### <a name="adaptivesubgrouptextstacking"></a>AdaptiveSubgroupTextStacking
TextStacking especifica la alineación vertical del contenido.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | Valor predeterminado. El representador selecciona automáticamente la alineación vertical predeterminada. |
| **Arriba** | Alineación vertical a la parte superior. |
| **Centro** | Alineación vertical al centro. |
| **parte inferior** | Alineación vertical a la parte inferior. |


## <a name="tilebackgroundimage"></a>TileBackgroundImage
Una imagen de fondo que se muestra sin bordes en la ventana.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Origen** | string | true | La dirección URL de la imagen. Se admiten ms-appx, ms-appdata y http. Las imágenes HTTP deben tener un tamaño de 200 KB o inferior. |
| **HintOverlay** | int? | falso | Una superposición negra en una imagen de fondo. Este valor controla la opacidad de la superposición negra, el cero 0 no supone superposición alguna y 100 es completamente negro. El valor predeterminado es 20. |
| **HintCrop** | [TileBackgroundImageCrop](#tilebackgroundimagecrop) | falso | Novedad en la versión 1511: Especifica cómo quieres que se recorte la imagen. En versiones anteriores a 1511, este función no existe y se mostrará la imagen de fondo sin ningún recorte. |
| **AlternateText** | string | falso | Texto alternativo que describe la imagen, que se usa para fines de accesibilidad. |
| **AddImageQuery** | bool? | falso | Establece el valor en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación de la ventana. Usa este atributo si el servidor aloja imágenes y puede controlar cadenas de consulta, ya sea recuperando una variante de imagen en función de las cadenas de consulta u omitiendo la cadena de consulta y devolviendo la imagen como se especificó sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us". |


### <a name="tilebackgroundimagecrop"></a>TileBackgroundImageCrop
Controla el recorte de la imagen de fondo.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | El recorte usa el comportamiento predeterminado del representador. |
| **Ninguno** | No se recorta la imagen, se muestra cuadrada. |
| **Círculo** | Se recorta la imagen en un círculo. |


## <a name="tilepeekimage"></a>TilePeekImage
Una animación de imagen que aparece desde la parte superior de la ventana.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Origen** | string | true | La dirección URL de la imagen. Se admiten ms-appx, ms-appdata y http. Las imágenes HTTP deben tener un tamaño de 200 KB o inferior. |
| **HintOverlay** | int? | falso | Novedad en la versión 1511: Una superposición de color negra en la imagen de peek. Este valor controla la opacidad de la superposición negra, el cero 0 no supone superposición alguna y 100 es completamente negro. El valor predeterminado es 20. En versiones anteriores, se omitirá este valor y se mostrará la imagen que aparece con superposición de 0. |
| **HintCrop** | [TilePeekImageCrop](#tilepeekimagecrop) | falso | Novedad en la versión 1511: Especifica cómo quieres que se recorte la imagen. En versiones anteriores a 1511, este función no existe y se mostrará la imagen que aparece sin ningún recorte. |
| **AlternateText** | string | falso | Texto alternativo que describe la imagen, que se usa para fines de accesibilidad. |
| **AddImageQuery** | bool? | falso | Establece el valor en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación de la ventana. Usa este atributo si el servidor aloja imágenes y puede controlar cadenas de consulta, ya sea recuperando una variante de imagen en función de las cadenas de consulta u omitiendo la cadena de consulta y devolviendo la imagen como se especificó sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us". |


### <a name="tilepeekimagecrop"></a>TilePeekImageCrop
Controla el recorte de la imagen que aparece.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | El recorte usa el comportamiento predeterminado del representador. |
| **Ninguno** | No se recorta la imagen, se muestra cuadrada. |
| **Círculo** | Se recorta la imagen en un círculo. |


### <a name="tiletextstacking"></a>TileTextStacking
El apilamiento de texto especifica la alineación vertical del contenido.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | Valor predeterminado. El representador selecciona automáticamente la alineación vertical predeterminada. |
| **Arriba** | Alineación vertical a la parte superior. |
| **Centro** | Alineación vertical al centro. |
| **parte inferior** | Alineación vertical a la parte inferior. |


## <a name="tilebindingcontenticonic"></a>TileBindingContentIconic
Compatible con el tamaño pequeño y mediano. Permite que una plantilla de ventana icónica, en la que se puede mostrar una insignia y un icono uno juntos, con el estilo clásico de Windows Phone. El número situado junto al icono se logra a través de una notificación de insignia independiente.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Icono** | [TileBasicImage](#tilebasicimage) | true | Para ser compatibles con ventanas pequeñas y medianas en versión de escritorio y móvil, propociona una imagen de relación de aspecto cuadrada con una resolución de 200 x 200 como mínimo, con formato PNG, transparente y de color blanco. Si quieres obtener más información, consulta: [Las plantillas de icono especial](../tiles-and-notifications/special-tile-templates-catalog.md). |


## <a name="tilebindingcontentcontact"></a>TileBindingContentContact
Solo para móviles. Compatible con el tamaño pequeño, mediano y ancho.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Image** | [TileBasicImage](#tilebasicimage) | true | Imagen para mostrar. |
| **Texto** | [TileBasicText](#tilebasictext) | falso | Una línea de texto que se muestra. No se muestra en la ventana pequeña. |


## <a name="tilebindingcontentpeople"></a>TileBindingContentPeople
Novedad en la versión 1511: Se admiten en medio de ancho y grande (escritorio y móvil). Anteriormente, solo era para móvil y solo para tamaños mediano y ancho.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Imágenes** | IList<[TileBasicImage](#tilebasicimage)> | true | Imágenes que se presentarán como círculos. |


## <a name="tilebindingcontentphotos"></a>TileBindingContentPhotos
Anima a través de una presentación de fotos. Compatible con todos los tamaños.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Imágenes** | IList<[TileBasicImage](#tilebasicimage)> | true | Pueden proporcionarse hasta 12 imágenes (la versión móvil solo mostrará hasta 9), que se usarán para la presentación. Agregar más de 12 se considerará una excepción. |


### <a name="tilebasicimage"></a>TileBasicImage
Una imagen que se usa en diferentes plantillas especiales.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Origen** | string | true | La dirección URL de la imagen. Se admiten ms-appx, ms-appdata y http. Las imágenes HTTP deben tener un tamaño de 200 KB o inferior. |
| **AlternateText** | string | falso | Texto alternativo que describe la imagen, que se usa para fines de accesibilidad. |
| **AddImageQuery** | bool? | falso | Establece el valor en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación de la ventana. Usa este atributo si el servidor aloja imágenes y puede controlar cadenas de consulta, ya sea recuperando una variante de imagen en función de las cadenas de consulta u omitiendo la cadena de consulta y devolviendo la imagen como se especificó sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us". |


### <a name="tilebasictext"></a>TileBasicText
Un elemento básico de texto que se usa en diferentes plantillas especiales.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Texto** | string | falso | El texto que se va a mostrar. |
| **Idioma** | string | falso | La configuración regional de destino de la carga XML, especificada como una etiqueta de idioma BCP-47, como "en-US" o "fr-FR". La configuración regional especificada aquí reemplaza cualquier otra configuración regional especificada, como las de enlace o visual. Si este valor es una cadena literal, el valor predeterminado de este atributo es el idioma de la interfaz de usuario del usuario. Si este valor es una referencia de cadena, el valor predeterminado de este atributo será la configuración regional elegida por Windows Runtime en tiempo de ejecución en la resolución de la cadena. |


## <a name="related-topics"></a>Temas relacionados

* [Inicio rápido: Enviar una notificación local del mosaico](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [Biblioteca de notificaciones en GitHub](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)