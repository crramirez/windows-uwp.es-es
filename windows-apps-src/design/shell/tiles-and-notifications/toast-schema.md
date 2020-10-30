---
description: En el siguiente artículo se describen todas las propiedades y los elementos del contenido del sistema.
title: Esquema de contenido de notificaciones del sistema
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Toast content schema
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7116f1aa6f06eda1351183963ea8169625a8df70
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033028"
---
# <a name="toast-content-schema"></a>Esquema de contenido de notificaciones del sistema

 

A continuación se describen todas las propiedades y los elementos del contenido del sistema.

Si prefiere usar XML sin formato en lugar de la [biblioteca de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), consulte [el esquema XML](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/schema-root).

[ToastContent](#toastcontent)
* [ToastVisual](#toastvisual)
  * [ToastBindingGeneric](#toastbindinggeneric)
    * [IToastBindingGenericChild](#itoastbindinggenericchild)
    * [ToastGenericAppLogo](#toastgenericapplogo)
    * [ToastGenericHeroImage](#toastgenericheroimage)
    * [ToastGenericAttributionText](#toastgenericattributiontext)
* [IToastActions](#itoastactions)
* [ToastAudio](#toastaudio)
* [ToastHeader](#toastheader)


## <a name="toastcontent"></a>ToastContent
ToastContent es el objeto de nivel superior que describe el contenido de una notificación, incluidos los objetos visuales, las acciones y el audio.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Launch**| string | false | Una cadena que se pasa a la aplicación cuando está activada por la notificación del sistema. El formato y el contenido de esta cadena los define la aplicación para su propio uso. Cuando el usuario puntea o hace clic en la notificación del sistema para iniciar su aplicación asociada, la cadena de inicio proporciona el contexto a la aplicación que le permite mostrar al usuario una vista pertinente para el contenido del sistema, en lugar de iniciarse de forma predeterminada. |
| **Visual** | [ToastVisual](#toastvisual) | true | Describe la parte visual de la notificación del sistema. |
| **Acciones** | [IToastActions](#itoastactions) | false | Opcionalmente, puede crear acciones personalizadas con botones y entradas. |
| **Audio** | [ToastAudio](#toastaudio) | false | Describe la parte de audio de la notificación del sistema. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Especifica qué tipo de activación se usará cuando el usuario haga clic en el cuerpo de esta notificación del sistema. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Novedad en Creators Update: opciones adicionales relacionadas con la activación de la notificación del sistema. |
| **Escenario** | [ToastScenario](#toastscenario) | false | Declara el escenario en el que se usa la notificación del sistema, como una alarma o un recordatorio. |
| **DisplayTimestamp** | DateTimeOffset? | false | Nuevo en Creators Update: invalide la marca de tiempo predeterminada con una marca de tiempo personalizada que representa Cuándo se entregó realmente el contenido de la notificación, en lugar del momento en que la plataforma Windows recibió la notificación. |
| **Encabezado** | [ToastHeader](#toastheader) | false | Novedad en Creators Update: Agregue un encabezado personalizado a la notificación para agrupar varias notificaciones en el centro de actividades. |


### <a name="toastscenario"></a>ToastScenario
Especifica el escenario que representa la notificación del sistema.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | El comportamiento normal del sistema. |
| **Reminder** | Una notificación de recordatorio. Esto se mostrará previamente expandido y permanecerá en la pantalla del usuario hasta que se descarte. |
| **Alarma** | Una notificación de alarma. Esto se mostrará previamente expandido y permanecerá en la pantalla del usuario hasta que se descarte. Audio se repite de forma predeterminada y utilizará el audio de alarma. |
| **IncomingCall** | Una notificación de llamada entrante. Esto se mostrará previamente expandido en un formato de llamada especial y permanecerá en la pantalla del usuario hasta que se descarte. Audio se repetirá de forma predeterminada y usará audio de tonos. |


## <a name="toastvisual"></a>ToastVisual
La parte visual de la notificación del sistema contiene los enlaces, que contienen texto, imágenes, contenido adaptable, etc.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **BindingGeneric** | [ToastBindingGeneric](#toastbindinggeneric) | true | El enlace de notificación genérico, que se puede representar en todos los dispositivos. Este enlace es obligatorio y no puede ser null. |
| **BaseUri** | Identificador URI | false | Una dirección URL base predeterminada que se combina con las direcciones URL relativas de los atributos de origen de la imagen. |
| **AddImageQuery** | booleano? | false | Establézcalo en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación del sistema. Utilice este atributo si el servidor hospeda imágenes y puede controlar las cadenas de consulta, ya sea mediante la recuperación de una variante de imagen basada en las cadenas de consulta o mediante la omisión de la cadena de consulta y la devolución de la imagen como se especifica sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&MS-lang = en-US" |
| **Lenguaje**| string | false | La configuración regional de destino de la carga visual cuando se usan recursos localizados, especificada como etiquetas de idioma BCP-47 como "en-US" o "fr-FR". Cualquier configuración regional especificada en el enlace o texto invalida esta configuración regional. Si no se proporciona, se usará la configuración regional del sistema en su lugar. |


## <a name="toastbindinggeneric"></a>ToastBindingGeneric
El enlace genérico es el enlace predeterminado para las notificaciones del sistema y es donde se especifica el texto, las imágenes, el contenido adaptable, etc.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Children** | IList<[IToastBindingGenericChild](#itoastbindinggenericchild)> | false | Contenido del cuerpo de la notificación del sistema, que puede incluir texto, imágenes y grupos (agregada en la actualización de aniversario). Los elementos de texto deben aparecer antes que cualquier otro elemento y solo se admiten tres elementos de texto. Si se coloca un elemento de texto después de cualquier otro elemento, se extraerá a la parte superior o se quitará. Y, por último, algunas propiedades de texto como HintStyle no se admiten en los elementos de texto de elementos secundarios raíz y solo funcionan dentro de un AdaptiveSubgroup. Si usa AdaptiveGroup en dispositivos sin la actualización de aniversario, el contenido del grupo se quitará simplemente. |
| **AppLogoOverride** | [ToastGenericAppLogo](#toastgenericapplogo) | false | Un logotipo opcional para invalidar el logotipo de la aplicación. |
| **HeroImage** | [ToastGenericHeroImage](#toastgenericheroimage) | false | Una imagen "prominente" destacada opcional que se muestra en la notificación del sistema y dentro del centro de actividades. |
| **Atribución** | [ToastGenericAttributionText](#toastgenericattributiontext) | false | Texto opcional de atribución que se mostrará en la parte inferior de la notificación del sistema. |
| **BaseUri** | Identificador URI | false | Una dirección URL base predeterminada que se combina con las direcciones URL relativas de los atributos de origen de la imagen. |
| **AddImageQuery** | booleano? | false | Establézcalo en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación del sistema. Utilice este atributo si el servidor hospeda imágenes y puede controlar las cadenas de consulta, ya sea mediante la recuperación de una variante de imagen basada en las cadenas de consulta o mediante la omisión de la cadena de consulta y la devolución de la imagen como se especifica sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&MS-lang = en-US" |
| **Lenguaje**| string | false | La configuración regional de destino de la carga visual cuando se usan recursos localizados, especificada como etiquetas de idioma BCP-47 como "en-US" o "fr-FR". Cualquier configuración regional especificada en el enlace o texto invalida esta configuración regional. Si no se proporciona, se usará la configuración regional del sistema en su lugar. |


## <a name="itoastbindinggenericchild"></a>IToastBindingGenericChild
Interfaz de marcador para los elementos secundarios del sistema que incluyen texto, imágenes, grupos, etc.

| Implementaciones |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |
| [AdaptiveGroup](#adaptivegroup) |
| [AdaptiveProgressBar](#adaptiveprogressbar) |


## <a name="adaptivetext"></a>AdaptiveText
Elemento de texto adaptable. Si se coloca en el nivel superior ToastBindingGeneric. Children, solo se aplicará HintMaxLines. Pero si se coloca como elemento secundario de un grupo o subgrupo, se admite el estilo de texto completo.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Texto** | string o [BindableString](#bindablestring) | false | Texto que se va a mostrar. Compatibilidad de enlace de datos agregada en Creators Update, pero solo funciona para los elementos de texto de nivel superior. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | El estilo controla el tamaño de fuente, el peso y la opacidad del texto. Solo funciona para los elementos de texto dentro de un grupo o subgrupo. |
| **HintWrap** | booleano? | false | Establézcalo en true para habilitar el ajuste de texto. Los elementos de texto de nivel superior omiten esta propiedad y siempre se ajustan (puede usar HintMaxLines = 1 para deshabilitar el ajuste de los elementos de texto de nivel superior). Los elementos de texto dentro de grupos o subgrupos tienen como valor predeterminado False para el ajuste. |
| **HintMaxLines** | int? | false | Número máximo de líneas que el elemento de texto puede mostrar. |
| **HintMinLines** | int? | false | Número mínimo de líneas que debe mostrar el elemento de texto. Solo funciona para los elementos de texto dentro de un grupo o subgrupo. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | Alineación horizontal del texto. Solo funciona para los elementos de texto dentro de un grupo o subgrupo. |
| **Lenguaje** | string | false | La configuración regional de destino de la carga XML, especificada como una etiqueta de idioma BCP-47 como "en-US" o "fr-FR". La configuración regional especificada aquí invalida cualquier otra configuración regional especificada, como la del enlace o el visual. Si este valor es una cadena literal, este atributo tiene como valor predeterminado el idioma de la interfaz de usuario del usuario. Si este valor es una referencia de cadena, este atributo tiene como valor predeterminado la configuración regional elegida por Windows Runtime en la resolución de la cadena. |


### <a name="bindablestring"></a>BindableString
Un valor de enlace para las cadenas.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **BindingName** | string | true | Obtiene o establece el nombre que se asigna al valor de datos de enlace. |


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
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | Novedades de la actualización de aniversario: controle el recorte deseado de la imagen. |
| **HintRemoveMargin** | booleano? | false | De forma predeterminada, las imágenes dentro de grupos o subgrupos tienen un margen 8px alrededor de ellas. Puede quitar este margen estableciendo esta propiedad en true. |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | Alineación horizontal de la imagen. Solo funciona para las imágenes dentro de un grupo o subgrupo. |
| **AlternateText** | string | false | Texto alternativo que describe la imagen, que se usa para fines de accesibilidad. |
| **AddImageQuery** | booleano? | false | Establézcalo en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación del sistema. Utilice este atributo si el servidor hospeda imágenes y puede controlar las cadenas de consulta, ya sea mediante la recuperación de una variante de imagen basada en las cadenas de consulta o mediante la omisión de la cadena de consulta y la devolución de la imagen como se especifica sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&MS-lang = en-US" |


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
Novedad en la actualización de aniversario: los grupos identifican semánticamente que el contenido del grupo se debe mostrar como un todo, o no se muestra si no cabe. Los grupos también permiten crear varias columnas.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | Los subgrupos se muestran como columnas verticales. Debe usar subgrupos para proporcionar contenido dentro de un AdaptiveGroup. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
Novedades de la actualización de aniversario: los subgrupos son columnas verticales que pueden contener texto e imágenes.

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


## <a name="adaptiveprogressbar"></a>AdaptiveProgressBar
Nuevo en Creators Update: barra de progreso. Solo se admite en la notificación del sistema en el escritorio, compilación 15063 o posterior.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Título** | string o [BindableString](#bindablestring) | false | Obtiene o establece una cadena de título opcional. Admite el enlace de datos. |
| **Valor** | Double o [AdaptiveProgressBarValue](#adaptiveprogressbarvalue) o [BindableProgressBarValue](#bindableprogressbarvalue) | false | Obtiene o establece el valor de la barra de progreso. Admite el enlace de datos. El valor predeterminado es 0. |
| **ValueStringOverride** | string o [BindableString](#bindablestring) | false | Obtiene o establece una cadena opcional que se va a mostrar en lugar de la cadena de porcentaje predeterminada. Si no se proporciona, se mostrará algo como "70%". |
| **Estado** | string o [BindableString](#bindablestring) | true | Obtiene o establece una cadena de estado (obligatorio), que se muestra debajo de la barra de progreso de la izquierda. Esta cadena debe reflejar el estado de la operación, como "descargando..." o "instalando..." |


### <a name="adaptiveprogressbarvalue"></a>AdaptiveProgressBarValue
Una clase que representa el valor de la barra de progreso.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Valor** | double | false | Obtiene o establece el valor (0,0-1,0) que representa el porcentaje completado. |
| **IsIndeterminate** | bool | false | Obtiene o establece un valor que indica si la barra de progreso es indeterminada. Si es true, el **valor** se omitirá. |


### <a name="bindableprogressbarvalue"></a>BindableProgressBarValue
Valor de la barra de progreso que se va A enlazar.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **BindingName** | string | true | Obtiene o establece el nombre que se asigna al valor de datos de enlace. |


## <a name="toastgenericapplogo"></a>ToastGenericAppLogo
Un logotipo que se mostrará en lugar del logotipo de la aplicación.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Origen** | string | true | Dirección URL de la imagen. se admiten MS-appx, MS-AppData y http. Las imágenes http deben tener un tamaño de 200 KB o menos. |
| **HintCrop** | [ToastGenericAppLogoCrop](#toastgenericapplogocrop) | false | Especifique cómo desea recortar la imagen. |
| **AlternateText** | string | false | Texto alternativo que describe la imagen, que se usa para fines de accesibilidad. |
| **AddImageQuery** | booleano? | false | Establézcalo en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación del sistema. Utilice este atributo si el servidor hospeda imágenes y puede controlar las cadenas de consulta, ya sea mediante la recuperación de una variante de imagen basada en las cadenas de consulta o mediante la omisión de la cadena de consulta y la devolución de la imagen como se especifica sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&MS-lang = en-US" |


### <a name="toastgenericapplogocrop"></a>ToastGenericAppLogoCrop
Controla el recorte de la imagen del logotipo de la aplicación.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | El recorte usa el comportamiento predeterminado del representador. |
| **None** | La imagen no se recorta, se muestra un cuadrado. |
| **Circle** | La imagen se recorta a un círculo. |


## <a name="toastgenericheroimage"></a>ToastGenericHeroImage
Una imagen "prominente" destacada que se muestra en la notificación del sistema y dentro del centro de actividades.

| Propiedad | Tipo | Requerido |Descripción |
|---|---|---|---|
| **Origen** | string | true | Dirección URL de la imagen. se admiten MS-appx, MS-AppData y http. Las imágenes http deben tener un tamaño de 200 KB o menos. |
| **AlternateText** | string | false | Texto alternativo que describe la imagen, que se usa para fines de accesibilidad. |
| **AddImageQuery** | booleano? | false | Establézcalo en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación del sistema. Utilice este atributo si el servidor hospeda imágenes y puede controlar las cadenas de consulta, ya sea mediante la recuperación de una variante de imagen basada en las cadenas de consulta o mediante la omisión de la cadena de consulta y la devolución de la imagen como se especifica sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&MS-lang = en-US" |


## <a name="toastgenericattributiontext"></a>ToastGenericAttributionText
Texto de atribución mostrado en la parte inferior de la notificación del sistema.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Texto** | string | true | Texto que se va a mostrar. |
| **Lenguaje** | string | false | La configuración regional de destino de la carga visual cuando se usan recursos localizados, especificada como etiquetas de idioma BCP-47 como "en-US" o "fr-FR". Si no se proporciona, se usará la configuración regional del sistema en su lugar. |


## <a name="itoastactions"></a>IToastActions
Interfaz de marcador para entradas o acciones del sistema.

| Implementaciones |
| --- |
| [ToastActionsCustom](#toastactionscustom) |
| [ToastActionsSnoozeAndDismiss](#toastactionssnoozeanddismiss) |


## <a name="toastactionscustom"></a>ToastActionsCustom
*Implementa [IToastActions](#itoastactions)*

Cree sus propias acciones y entradas personalizadas mediante controles como botones, cuadros de texto y entradas de selección.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Entradas** | IList<[IToastInput](#itoastinput)> | false | Entradas como cuadros de texto y entradas de selección. Solo se permiten hasta 5 entradas. |
| **Botones** | IList<[IToastButton](#itoastbutton)> | false | Los botones se muestran después de todas las entradas (o adyacentes a una entrada si el botón se usa como botón de respuesta rápida). Solo se permiten hasta 5 botones (o menos si también tiene elementos de menú contextual). |
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | Novedades de la actualización de aniversario: elementos del menú contextual personalizado, que proporcionan acciones adicionales si el usuario hace clic con el botón derecho en la notificación. Solo puede tener hasta 5 botones y elementos de menú contextual *combinados* . |


## <a name="itoastinput"></a>IToastInput
Interfaz de marcador para entradas del sistema.

| Implementaciones |
| --- |
| [ToastTextBox](#toasttextbox) |
| [ToastSelectionBox](#toastselectionbox) |


## <a name="toasttextbox"></a>ToastTextBox
*Implementa [IToastInput](#itoastinput)*

Control de cuadro de texto en el que el usuario puede escribir texto.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Id** | string | true | El identificador es obligatorio y se usa para asignar el texto de la incrustación del usuario a un par clave-valor de identificador/valor que la aplicación consume posteriormente. |
| **Título** | string | false | Texto del título que se va a mostrar sobre el cuadro de texto. |
| **PlaceholderContent** | string | false | Texto de marcador de posición que se mostrará en el cuadro de texto cuando el usuario no haya escrito todavía ningún texto. |
| **DefaultInput** | string | false | Texto inicial que se va a colocar en el cuadro de texto. Deje este valor null para un cuadro de texto en blanco. |


## <a name="toastselectionbox"></a>ToastSelectionBox
*Implementa [IToastInput](#itoastinput)*

Control de cuadro de selección, que permite a los usuarios elegir en una lista desplegable de opciones.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Id** | string | true | El identificador es obligatorio. Si el usuario seleccionó este elemento, este identificador se devolverá al código de la aplicación, que representa la selección que han elegido. |
| **Contenido** | string | true | El contenido es obligatorio y es una cadena que se muestra en el elemento de selección. |


### <a name="toastselectionboxitem"></a>ToastSelectionBoxItem
Un elemento del cuadro de selección (un elemento que el usuario puede seleccionar en la lista desplegable).

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Id** | string | true | El identificador es obligatorio y se usa para asignar el texto de la incrustación del usuario a un par clave-valor de identificador/valor que la aplicación consume posteriormente. |
| **Título** | string | false | Texto del título que se va a mostrar sobre el cuadro de selección. |
| **DefaultSelectionBoxItemId** | string | false | Controla qué elemento está seleccionado de forma predeterminada y hace referencia a la propiedad ID de [ToastSelectionBoxItem](#toastselectionboxitem). Si no proporciona esta opción, la selección predeterminada estará vacía (el usuario no podrá ver nada). |
| **Elementos** | IList<[ToastSelectionBoxItem](#toastselectionboxitem)> | false | Los elementos de selección de los que el usuario puede elegir en este SelectionBox. Solo se pueden agregar 5 elementos. |


## <a name="itoastbutton"></a>IToastButton
Interfaz de marcador para los botones del sistema.

| Implementaciones |
| --- |
| [ToastButton](#toastbutton) |
| [ToastButtonSnooze](#toastbuttonsnooze) |
| [ToastButtonDismiss](#toastbuttondismiss) |


## <a name="toastbutton"></a>ToastButton
*Implementa [IToastButton](#itoastbutton)*

Botón en el que el usuario puede hacer clic.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Contenido** | string | true | Necesario. Texto que se va a mostrar en el botón. |
| **Argumentos** | string | true | Necesario. Cadena de argumentos definida por la aplicación que recibirá posteriormente la aplicación si el usuario hace clic en este botón. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Controla qué tipo de activación usará este botón cuando se haga clic en él. El valor predeterminado es Foreground. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Novedad en Creators Update: obtiene o establece opciones adicionales relacionadas con la activación del botón del sistema. |


### <a name="toastactivationtype"></a>ToastActivationType
Decide el tipo de activación que se utilizará cuando el usuario interactúe con una acción específica.

| Valor | Significado |
|---|---|
| **Primer plano** | Valor predeterminado. Se iniciará la aplicación en primer plano. |
| **Información preliminar** | La tarea en segundo plano correspondiente (suponiendo que se establece todo) se desencadena y puede ejecutar código en segundo plano (como enviar el mensaje de respuesta rápida del usuario) sin interrumpir al usuario. |
| **Protocolo** | Inicie una aplicación diferente mediante la activación de protocolo. |


### <a name="toastactivationoptions"></a>ToastActivationOptions
Novedad en Creators Update: opciones adicionales relacionadas con la activación.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **AfterActivationBehavior** | [ToastAfterActivationBehavior](#toastafteractivationbehavior) | false | Novedad en Fall Creators Update: obtiene o establece el comportamiento que la notificación del sistema debe usar cuando el usuario invoca esta acción. Esto solo funciona en el escritorio, para [ToastButton](#toastbutton) y [ToastContextMenuItem](#toastcontextmenuitem). |
| **ProtocolActivationTargetApplicationPfn** | string | false | Si usa *ToastActivationType. Protocol* , también puede especificar el pfn de destino, de modo que, independientemente de si hay varias aplicaciones registradas para administrar el mismo URI de protocolo, siempre se iniciará la aplicación deseada. |


### <a name="toastafteractivationbehavior"></a>ToastAfterActivationBehavior
Especifica el comportamiento que debe usar la notificación del sistema cuando el usuario realiza una acción en la notificación del sistema.

| Valor | Significado |
|---|---|
| **Valor predeterminado** | Comportamiento predeterminado. La notificación del sistema se descartará cuando el usuario realice la acción en la notificación del sistema. |
| **PendingUpdate** | Una vez que el usuario hace clic en un botón de la notificación del sistema, la notificación permanecerá presente, en un estado visual "pendiente de actualización". Debe actualizar inmediatamente la notificación del sistema desde una tarea en segundo plano para que el usuario no vea el estado visual "pendiente de actualización" durante demasiado tiempo. |


## <a name="toastbuttonsnooze"></a>ToastButtonSnooze
*Implementa [IToastButton](#itoastbutton)*

Botón de posponer controlado por el sistema que controla automáticamente la posposición de la notificación.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **CustomContent** | string | false | Texto personalizado opcional que se muestra en el botón que invalida el texto "posponer" adaptado predeterminado. |


## <a name="toastbuttondismiss"></a>ToastButtonDismiss
*Implementa [IToastButton](#itoastbutton)*

Un botón descartado controlado por el sistema que descarta la notificación cuando se hace clic en ella.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **CustomContent** | string | false | Texto personalizado opcional que se muestra en el botón que reemplaza el texto "descartado" adaptado predeterminado. |


## <a name="toastactionssnoozeanddismiss"></a>ToastActionsSnoozeAndDismiss
* Implementa [IToastActions](#itoastactions)

Crea automáticamente un cuadro de selección para los intervalos de aplazamiento, y los botones para posponer y descartar, todo el sistema controla automáticamente la lógica de posposición y de posposición.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | Novedades de la actualización de aniversario: elementos del menú contextual personalizado, que proporcionan acciones adicionales si el usuario hace clic con el botón derecho en la notificación. Solo puede tener hasta 5 elementos. |


## <a name="toastcontextmenuitem"></a>ToastContextMenuItem
Una entrada de elemento de menú contextual.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Contenido** | string | true | Necesario. Texto que se va a mostrar. |
| **Argumentos** | string | true | Necesario. Cadena de argumentos definida por la aplicación que la aplicación puede recuperar más tarde una vez que se activa cuando el usuario hace clic en el elemento de menú. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Controla qué tipo de activación usará este elemento de menú cuando se haga clic en él. El valor predeterminado es Foreground. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Novedad en Creators Update: opciones adicionales relacionadas con la activación del elemento de menú contextual del sistema. |


## <a name="toastaudio"></a>ToastAudio
Especifique el audio que se reproducirá cuando se reciba la notificación del sistema.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Diez** | uri | false | Archivo multimedia que se va a reproducir en lugar del sonido predeterminado. Solo se admiten MS-appx y MS-appdata. |
| **Realizar** | boolean | false | Establézcalo en true si el sonido se debe repetir siempre que se muestre la notificación del sistema; false para reproducir solo una vez (valor predeterminado). |
| **Manera** | boolean | false | True para silenciar el sonido; false para permitir que se reproduzca el sonido de notificación del sistema (valor predeterminado). |


## <a name="toastheader"></a>ToastHeader
Novedad en Creators Update: un encabezado personalizado que agrupa varias notificaciones en el centro de actividades.

| Propiedad | Tipo | Requerido | Descripción |
|---|---|---|---|
| **Id** | string | true | Un identificador creado por el desarrollador que identifica de forma única este encabezado. Si dos notificaciones tienen el mismo identificador de encabezado, se mostrarán debajo del mismo encabezado en el centro de actividades. |
| **Título** | string | true | Título para el encabezado. |
| **Argumentos**| string | true | Obtiene o establece una cadena de argumentos definida por el desarrollador que se devuelve a la aplicación cuando el usuario hace clic en este encabezado. No puede ser NULL. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Obtiene o establece el tipo de activación que usará este encabezado cuando se haga clic en él. El valor predeterminado es Foreground. Tenga en cuenta que solo se admiten el primer y el protocolo. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Obtiene o establece opciones adicionales relacionadas con la activación del encabezado del sistema. |


## <a name="related-topics"></a>Temas relacionados

* [Inicio rápido: Enviar una notificación del sistema local y administrar la aplicación](/archive/blogs/tiles_and_toasts/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10)
* [Biblioteca de notificaciones en GitHub](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/dev/Notifications)
