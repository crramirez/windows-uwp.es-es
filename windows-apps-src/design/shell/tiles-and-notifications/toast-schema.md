---
author: andrewleader
Description: The following article describes all of the properties and elements within toast content.
title: Esquema de contenido de las notificaciones del sistema
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Toast content schema
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ad1700d58ab3568aa3aefa46b5950d0a8bf3c320
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "4612148"
---
# <a name="toast-content-schema"></a>Esquema de contenido de las notificaciones del sistema

 

En el siguiente artículo se describen todas las propiedades y elementos dentro del contenido de la notificación del sistema.

Si prefieres utilizar XML sin formato en lugar de la [Biblioteca Notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), consulta [el esquema XML](toast-xml-schema.md).

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
ToastContent es el objeto de nivel superior que describe el contenido de una notificación, incluidos los elementos visuales, las acciones y el audio.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Launch**| cadena | false | Una cadena que se pasa a la aplicación cuando la notificación del sistema la activa. El formato y el contenido de esta cadena los define la aplicación para su propio uso. Cuando el usuario pulse o haga clic en la notificación del sistema para iniciar la aplicación asociada, la cadena de inicio proporciona el contexto de la aplicación que le permite mostrar al usuario una vista relevante para el contenido de la notificación del sistema, en lugar de iniciarse de forma predeterminada. |
| **Visual** | [ToastVisual](#toastvisual) | true | Describe la parte visual de la notificación del sistema. |
| **Actions** | [IToastActions](#itoastactions) | false | De manera opcional, crea acciones personalizadas con botones y entradas. |
| **Audio** | [ToastAudio](#toastaudio) | false | Describe la parte de audio de la notificación del sistema. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Especifica qué tipo de activación se usará cuando el usuario haga clic en el cuerpo de esta notificación del sistema. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Novedad de la actualización Creators Update: opciones adicionales relacionadas con la activación de la notificación del sistema. |
| **Scenario** | [ToastScenario](#toastscenario) | false | Declara el escenario para el que se usa la notificación del sistema, como una alarma o un aviso. |
| **DisplayTimestamp** | DateTimeOffset? | false | Novedad de la actualización Creators Update: reemplaza la marca de tiempo predeterminada con una marca de tiempo personalizada que representa cuándo se entregó realmente el contenido de la notificación, en lugar de la hora en la que se recibió por la plataforma de Windows. |
| **Header** | [ToastHeader](#toastheader) | false | Novedad de la actualización Creators Update: agrega un encabezado personalizado a tu grupo de notificación para agrupar varias notificaciones en el centro de actividades. |


### <a name="toastscenario"></a>ToastScenario
Especifica qué escenario representa la notificación del sistema.

| Valor | Significado |
|---|---|
| **Default** | El comportamiento normal de la notificación del sistema. |
| **Reminder** | Notificación de recordatorio. Se mostrará previamente expandida y permanecerá en la pantalla del usuario hasta que se descarte. |
| **Alarm** | Notificación de alarma. Se mostrará previamente expandida y permanecerá en la pantalla del usuario hasta que se descarte. El audio se repetirá de manera predeterminada y usará el audio de alarma. |
| **IncomingCall** | Notificación de llamada entrante. Se mostrará previamente expandida en un formato de llamada especial y permanecerá en la pantalla del usuario hasta que se descarte. El audio se repetirá de manera predeterminada y usará el audio de tono. |


## <a name="toastvisual"></a>ToastVisual
La parte visual de las notificaciones del sistema contiene los enlaces, que contiene texto, imágenes, contenido adaptable y mucho más.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **BindingGeneric** | [ToastBindingGeneric](#toastbindinggeneric) | true | El enlace de notificación del sistema genérico, que se puede representar en todos los dispositivos. Este enlace es necesario y no puede ser null. |
| **BaseUri** | Uri | false | URL base predeterminada que se combina con direcciones URL relativas en atributos de origen de imagen. |
| **AddImageQuery** | bool? | false | Establece el valor en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación del sistema. Usa este atributo si el servidor aloja imágenes y puede controlar cadenas de consulta, ya sea recuperando una variante de imagen en función de las cadenas de consulta u omitiendo la cadena de consulta y devolviendo la imagen como se especificó sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us". |
| **Idioma**| cadena | false | La configuración regional de destino de la carga visual al usar recursos localizados, especificados como etiquetas de idioma BCP-47 como "en-US" o "fr-FR". Esta configuración regional se reemplaza por cualquier configuración regional especificada en el enlace o texto. Si no se proporciona, se usará la configuración regional del sistema en su lugar. |


## <a name="toastbindinggeneric"></a>ToastBindingGeneric
El enlace genérico es el enlace predeterminado para las notificaciones del sistema y es donde especificas el texto, las imágenes, el contenido adaptable y mucho más.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Children** | IList<[IToastBindingGenericChild](#itoastbindinggenericchild)> | false | El contenido del cuerpo de la notificación del sistema, que puede incluir texto, imágenes y grupos (agregados en la Actualización de aniversario). Los elementos de texto deben aparecer antes de cualquier otro elemento y solo se admiten 3 elementos de texto. Si un elemento de texto se coloca después de cualquier otro elemento, se llevará a la parte superior o se quitará. Por último, algunas no se admiten propiedades de texto como HintStyle en los elementos de texto secundarios raíz y solo funcionan dentro de un AdaptiveSubgroup. Si usas AdaptiveGroup en dispositivos sin la Actualización de aniversario, el contenido del grupo simplemente se quitará. |
| **AppLogoOverride** | [ToastGenericAppLogo](#toastgenericapplogo) | false | Logotipo opcional para reemplazar el logotipo de la aplicación. |
| **HeroImage** | [ToastGenericHeroImage](#toastgenericheroimage) | false | Imagen principal destacada opcional que se muestra en la notificación del sistema y en el centro de actividades. |
| **Attribution** | [ToastGenericAttributionText](#toastgenericattributiontext) | false | Texto de atribución opcional que se mostrará en la parte inferior de la notificación del sistema. |
| **BaseUri** | Uri | false | URL base predeterminada que se combina con direcciones URL relativas en atributos de origen de imagen. |
| **AddImageQuery** | bool? | false | Establece el valor en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación del sistema. Usa este atributo si el servidor aloja imágenes y puede controlar cadenas de consulta, ya sea recuperando una variante de imagen en función de las cadenas de consulta u omitiendo la cadena de consulta y devolviendo la imagen como se especificó sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us". |
| **Idioma**| cadena | false | La configuración regional de destino de la carga visual al usar recursos localizados, especificados como etiquetas de idioma BCP-47 como "en-US" o "fr-FR". Esta configuración regional se reemplaza por cualquier configuración regional especificada en el enlace o texto. Si no se proporciona, se usará la configuración regional del sistema en su lugar. |


## <a name="itoastbindinggenericchild"></a>IToastBindingGenericChild
Interfaz de marcador para elementos secundarios de notificación del sistema que incluyen texto, imágenes, grupos y mucho más.

| Implementaciones |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |
| [AdaptiveGroup](#adaptivegroup) |
| [AdaptiveProgressBar](#adaptiveprogressbar) |


## <a name="adaptivetext"></a>AdaptiveText
Elemento de texto adaptable. Si se coloca en el ToastBindingGeneric.Children de nivel superior, solo se aplicará HintMaxLines. Pero si se coloca como elemento secundario de un grupo o subgrupo, se admite el estilo de texto completo.

| Propiedad | Tipo | Obligatorio |Descripción |
|---|---|---|---|
| **Text** | cadena o [BindableString](#bindablestring) | false | El texto que se va a mostrar. Se ha agregado la compatibilidad de enlace de datos en la actualización Creators Update, pero solo funciona para los elementos de texto de nivel superior. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | El estilo controla el tamaño, el espesor y la opacidad de la fuente del texto. Solo funciona para elementos de texto dentro de un grupo o subgrupo. |
| **HintWrap** | bool? | false | Establece esto en true para habilitar el ajuste de texto. Los elementos de texto de nivel superior omiten esta propiedad y siempre ajustan (puedes usar HintMaxLines = 1 para deshabilitar el ajuste para elementos de texto de nivel superior). El valor predeterminado de los elementos de texto dentro de grupos o subgrupos es false para el ajuste. |
| **HintMaxLines** | int? | false | El número máximo de líneas que se permite que mostrar al elemento de texto. |
| **HintMinLines** | int? | false | El número mínimo de líneas que debe mostrar el elemento de texto. Solo funciona para elementos de texto dentro de un grupo o subgrupo. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | Alineación horizontal del texto. Solo funciona para elementos de texto dentro de un grupo o subgrupo. |
| **Language** | cadena | false | La configuración regional de destino de la carga XML, especificada como una etiqueta de idioma BCP-47, como "en-US" o "fr-FR". La configuración regional especificada aquí reemplaza cualquier otra configuración regional especificada, como las de enlace o visual. Si este valor es una cadena literal, el valor predeterminado de este atributo es el idioma de la interfaz de usuario del usuario. Si este valor es una referencia de cadena, el valor predeterminado de este atributo será la configuración regional elegida por Windows Runtime en tiempo de ejecución en la resolución de la cadena. |


### <a name="bindablestring"></a>BindableString
Valor de enlace para cadenas.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **BindingName** | cadena | true | Obtiene o establece el nombre que se asigna a su valor de datos de enlace. |


### <a name="adaptivetextstyle"></a>AdaptiveTextStyle
El estilo de texto controla el tamaño, el espesor y la opacidad de la fuente. La opacidad sutil es 60% opaco.

| Value | Significado |
|---|---|
| **Default** | Valor predeterminado. El estilo viene determinado por el representador. |
| **Caption** | De menor tamaño que el tamaño de la fuente del párrafo. |
| **CaptionSubtle** | Igual que Caption pero con opacidad sutil. |
| **Body** | Tamaño de fuente del párrafo. |
| **BodySubtle** | Igual que Body pero con opacidad sutil. |
| **Base** | Tamaño de fuente de párrafo, espesor de negrita. Básicamente, la versión negrita de Body. |
| **BaseSubtle** | Igual que Base pero con opacidad sutil. |
| **Subtitle** | Tamaño de fuente H4 |
| **SubtitleSubtle** | Igual que Subtitle pero con opacidad sutil. |
| **Title** | Tamaño de fuente H3 |
| **TitleSubtle** | Igual que Title pero con opacidad sutil. |
| **TitleNumeral** | Igual que Title pero con el espaciado superior o inferior quitado. |
| **Subheader** | Tamaño de fuente H2 |
| **SubheaderSubtle** | Igual que Subheader pero con opacidad sutil. |
| **SubheaderNumeral** | Igual que Subheader pero con el espaciado superior o inferior quitado. |
| **Header** | Tamaño de fuente H1 |
| **HeaderSubtle** | Igual que Header pero con opacidad sutil. |
| **HeaderNumeral** | Igual que Header pero con el espaciado superior o inferior quitado. |


### <a name="adaptivetextalign"></a>AdaptiveTextAlign
Controla la alineación horizontal del texto.

| Valor | Significado |
|---|---|
| **Default** | Valor predeterminado. La alineación la determina automáticamente el representador. |
| **Auto** | Alineación determinada por el idioma y la referencia cultural actuales. |
| **Left** | Alinea horizontalmente el texto a la izquierda. |
| **Center** | Alinea el texto horizontalmente en el centro. |
| **Derecha** | Alinea el texto horizontalmente a la derecha. |


## <a name="adaptiveimage"></a>AdaptiveImage
Imagen alineada.

| Propiedad | Tipo | Obligatorio |Descripción |
|---|---|---|---|
| **Source** | cadena | true | La dirección URL de la imagen. Se admiten ms-appx, ms-appdata y http. A partir de la actualización de Fall Creators Update, las imágenes web pueden tener un tamaño de hasta 3MB, en conexiones normales y de 1MB en conexiones de uso medido. En dispositivos que no ejecutan aún la actualización de Fall Creators Update, el tamaño de las imágenes web no debe ser superior a los 200KB. |
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | Novedad en la Actualización de aniversario: controlar el recorte deseado de la imagen. |
| **HintRemoveMargin** | bool? | false | De manera predeterminada, las imágenes dentro de grupos o subgrupos tienen un margen de 8 píxeles alrededor de ellas. Puedes quitar este margen estableciendo esta propiedad en true. |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | Alineación horizontal de la imagen. Solo funciona para las imágenes que se encuentran dentro de un grupo o subgrupo. |
| **AlternateText** | cadena | false | Texto alternativo que describe la imagen, que se usa para fines de accesibilidad. |
| **AddImageQuery** | bool? | false | Establece el valor en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación del sistema. Usa este atributo si el servidor aloja imágenes y puede controlar cadenas de consulta, ya sea recuperando una variante de imagen en función de las cadenas de consulta u omitiendo la cadena de consulta y devolviendo la imagen como se especificó sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us". |


### <a name="adaptiveimagecrop"></a>AdaptiveImageCrop
Especifica el recorte deseado de la imagen.

| Valor | Significado |
|---|---|
| **Default** | Valor predeterminado. Comportamiento de recorte determinado por el representador. |
| **None** | No se recorta la imagen. |
| **Circle** | Se recorta la imagen a una forma de círculo. |


### <a name="adaptiveimagealign"></a>AdaptiveImageAlign
Especifica la alineación horizontal para una imagen.

| Value | Significado |
|---|---|
| **Default** | Valor predeterminado. Comportamiento de alineación determinado por el representador. |
| **Stretch** | La imagen se amplía para rellenar el ancho disponible (y potencialmente también el alto disponible, en función de donde se coloque la imagen). |
| **Left** | Alinea la imagen a la izquierda y la muestra en su resolución nativa. |
| **Center** | Alinea la imagen en el centro de forma horizontal y la muestra en su resolución nativa. |
| **Right** | Alinea la imagen a la derecha y la muestra en su resolución nativa. |


## <a name="adaptivegroup"></a>AdaptiveGroup
Novedad en la Actualización de aniversario: los grupos identifican semánticamente que el contenido del grupo debe mostrarse entero o no mostrarse si no cabe. Los grupos también permiten la creación de varias columnas.

| Propiedad | Tipo | Obligatorio |Descripción |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | Los subgrupos se muestran como columnas verticales. Debes usar subgrupos para proporcionar cualquier contenido dentro de un AdaptiveGroup. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
Novedad en la Actualización de aniversario: los subgrupos son columnas verticales que pueden contener texto e imágenes.

| Propiedad | Tipo | Obligatorio |Descripción |
|---|---|---|---|
| **Children** | IList<[IAdaptiveSubgroupChild](#iadaptivesubgroupchild)> | false | [AdaptiveText](#adaptivetext) y [AdaptiveImage](#adaptiveimage) son elementos secundarios válidos de subgrupos. |
| **HintWeight** | int? | false | Controla el ancho de esta columna de subgrupo especificando el grosor, en relación con los demás subgrupos. |
| **HintTextStacking** | [AdaptiveSubgroupTextStacking](#adaptivesubgrouptextstacking) | false | Controla la alineación vertical del contenido de este subgrupo. |


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
| **Default** | Valor predeterminado. El representador selecciona automáticamente la alineación vertical predeterminada. |
| **Top** | Alineación vertical a la parte superior. |
| **Center** | Alineación vertical al centro. |
| **Bottom** | Alineación vertical a la parte inferior. |


## <a name="adaptiveprogressbar"></a>AdaptiveProgressBar
Novedad en Creators Update: una barra de progreso. Solo se admite en las notificaciones del sistema de Escritorio, compilación 15063 o posterior.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Título** | cadena o [BindableString](#bindablestring) | falso | Obtiene o establece una cadena de título opcional. Admite el enlace de datos. |
| **Valor** | doble o [AdaptiveProgressBarValue](#adaptiveprogressbarvalue) o [BindableProgressBarValue](#bindableprogressbarvalue) | falso | Obtiene o establece el valor de la barra de progreso. Admite el enlace de datos. El valor predeterminado es 0. |
| **ValueStringOverride** | cadena o [BindableString](#bindablestring) | falso | Obtiene o establece una cadena opcional que se mostrará en lugar de la cadena de porcentaje predeterminada. Si no se proporciona, se mostrará algo similar a "70%". |
| **Estado** | cadena o [BindableString](#bindablestring) | true | Obtiene o establece una cadena de estado (obligatoria), que se muestra debajo de la barra de progreso a la izquierda. Esta cadena debe reflejar el estado de la operación, como "Descargando…" o "Instalando..." |


### <a name="adaptiveprogressbarvalue"></a>AdaptiveProgressBarValue
Clase que representa el valor de la barra de progreso.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Valor** | double | falso | Obtiene o establece el valor (0,0 - 1,0) que representa el porcentaje completado. |
| **IsIndeterminate** | bool | falso | Obtiene o establece un valor que indica si la barra de progreso es indeterminada. Si es así, **Valor** se ignorará. |


### <a name="bindableprogressbarvalue"></a>BindableProgressBarValue
Valor de la barra de progreso enlazable.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **BindingName** | cadena | true | Obtiene o establece el nombre que se asigna a su valor de datos de enlace. |


## <a name="toastgenericapplogo"></a>ToastGenericAppLogo
Un logotipo que se mostrará en lugar del logotipo de la aplicación.

| Propiedad | Tipo | Obligatorio |Descripción |
|---|---|---|---|
| **Source** | cadena | true | La dirección URL de la imagen. Se admiten ms-appx, ms-appdata y http. Las imágenes HTTP deben tener un tamaño de 200KB o inferior. |
| **HintCrop** | [ToastGenericAppLogoCrop](#toastgenericapplogocrop) | false | Especifica cómo quieres que se recorte la imagen. |
| **AlternateText** | cadena | false | Texto alternativo que describe la imagen, que se usa para fines de accesibilidad. |
| **AddImageQuery** | bool? | false | Establece el valor en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación del sistema. Usa este atributo si el servidor aloja imágenes y puede controlar cadenas de consulta, ya sea recuperando una variante de imagen en función de las cadenas de consulta u omitiendo la cadena de consulta y devolviendo la imagen como se especificó sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us". |


### <a name="toastgenericapplogocrop"></a>ToastGenericAppLogoCrop
Controla el recorte de la imagen de logotipo de la aplicación.

| Valor | Significado |
|---|---|
| **Default** | El recorte usa el comportamiento predeterminado del representador. |
| **None** | No se recorta la imagen, se muestra cuadrada. |
| **Circle** | Se recorta la imagen en un círculo. |


## <a name="toastgenericheroimage"></a>ToastGenericHeroImage
Imagen principal destacada que se muestra en la notificación del sistema y en el centro de actividades.

| Propiedad | Tipo | Obligatorio |Descripción |
|---|---|---|---|
| **Source** | cadena | true | La dirección URL de la imagen. Se admiten ms-appx, ms-appdata y http. Las imágenes HTTP deben tener un tamaño de 200KB o inferior. |
| **AlternateText** | cadena | false | Texto alternativo que describe la imagen, que se usa para fines de accesibilidad. |
| **AddImageQuery** | bool? | false | Establece el valor en "true" para permitir que Windows anexe una cadena de consulta a la dirección URL de la imagen proporcionada en la notificación del sistema. Usa este atributo si el servidor aloja imágenes y puede controlar cadenas de consulta, ya sea recuperando una variante de imagen en función de las cadenas de consulta u omitiendo la cadena de consulta y devolviendo la imagen como se especificó sin la cadena de consulta. Esta cadena de consulta especifica la escala, la configuración de contraste y el idioma; por ejemplo, un valor de "www.website.com/images/hello.png" proporcionado en la notificación se convierte en "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us". |


## <a name="toastgenericattributiontext"></a>ToastGenericAttributionText
Texto de atribución que se muestra en la parte inferior de la notificación del sistema.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Text** | cadena | true | El texto que se va a mostrar. |
| **Language** | cadena | false | La configuración regional de destino de la carga visual al usar recursos localizados, especificados como etiquetas de idioma BCP-47 como "en-US" o "fr-FR". Si no se proporciona, se usará la configuración regional del sistema en su lugar. |


## <a name="itoastactions"></a>IToastActions
Interfaz de marcador para entradas o acciones de notificación del sistema.

| Implementaciones |
| --- |
| [ToastActionsCustom](#toastactionscustom) |
| [ToastActionsSnoozeAndDismiss](#toastactionssnoozeanddismiss) |


## <a name="toastactionscustom"></a>ToastActionsCustom
*Implementa [IToastActions](#itoastactions)*

Crea tus propias entradas y acciones personalizadas, mediante controles como botones, cuadros de texto y entradas de selección.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Inputs** | IList<[IToastInput](#itoastinput)> | false | Entradas como cuadros de texto y entradas de selección. Solo se permiten hasta 5 entradas. |
| **Buttons** | IList<[IToastButton](#itoastbutton)> | false | Los botones se muestran después de todas las entradas (o junto a una entrada si el botón se usa como botón de respuesta rápida). Solo se permite un máximo de 5 botones (o menos si también tienes elementos de menú contextual). |
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | Novedad en la Actualización de aniversario: elementos de menú contextual personalizados, que proporcionan acciones adicionales en caso de que el usuario haga clic con el botón derecho en la notificación. Solo puedes tener un máximo de 5 botones y elementos de menú contextual *combinados*. |


## <a name="itoastinput"></a>IToastInput
Interfaz de marcador para entradas de notificación del sistema.

| Implementaciones |
| --- |
| [ToastTextBox](#toasttextbox) |
| [ToastSelectionBox](#toastselectionbox) |


## <a name="toasttextbox"></a>ToastTextBox
*Implementa [IToastInput](#itoastinput)*

Control de cuadro de texto en el que puede escribir el usuario.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Id** | cadena | true | El identificador es necesario y se usa para asignar el texto introducido por el usuario en un par clave-valor de Id./valor que la aplicación consume posteriormente. |
| **Title** | cadena | false | Texto del título que se mostrará encima del cuadro de texto. |
| **PlaceholderContent** | cadena | false | Texto de marcador de posición que se mostrará en el cuadro de texto cuando el usuario no haya escrito ningún texto todavía. |
| **DefaultInput** | cadena | false | El texto inicial que se colocará en el cuadro de texto. Deja esto como null para un cuadro de texto en blanco. |


## <a name="toastselectionbox"></a>ToastSelectionBox
*Implementa [IToastInput](#itoastinput)*

Control de cuadro de selección, que permite a los usuarios elegir en una lista desplegable de opciones.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Id** | cadena | true | Se necesita el Id. Si el usuario ha seleccionado este elemento, este identificador se pasará de nuevo al código de la aplicación, que representa qué selección eligió. |
| **Content** | cadena | true | El contenido es necesario y es una cadena que se muestra en el elemento de la selección. |


### <a name="toastselectionboxitem"></a>ToastSelectionBoxItem
Elemento del cuadro de selección (un elemento que el usuario puede seleccionar en la lista desplegable).

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Id** | cadena | true | El identificador es necesario y se usa para asignar el texto introducido por el usuario en un par clave-valor de Id./valor que la aplicación consume posteriormente. |
| **Title** | cadena | false | Texto del título que se mostrará encima del cuadro de selección. |
| **DefaultSelectionBoxItemId** | cadena | false | Controla qué elemento está seleccionado de forma predeterminada y hace referencia a la propiedad del identificador del [ToastSelectionBoxItem ](#toastselectionboxitem). Si no proporcionas esto, la selección predeterminada estará vacía (el usuario no ve nada). |
| **Items** | IList<[ToastSelectionBoxItem](#toastselectionboxitem)> | false | Los elementos de la selección que el usuario puede seleccionar en este SelectionBox. Solo se pueden agregar 5 elementos. |


## <a name="itoastbutton"></a>IToastButton
Interfaz de marcador para botones de notificación del sistema.

| Implementaciones |
| --- |
| [ToastButton](#toastbutton) |
| [ToastButtonSnooze](#toastbuttonsnooze) |
| [ToastButtonDismiss](#toastbuttondismiss) |


## <a name="toastbutton"></a>ToastButton
*Implementa [IToastButton](#itoastbutton)*

Botón en el que el usuario puede hacer clic.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Contenido** | cadena | true | Obligatorio. Texto que se va a mostrar en el botón. |
| **Arguments** | cadena | true | Obligatorio. Cadena definida por la aplicación de argumentos que la aplicación recibirá más adelante si el usuario hace clic en este botón. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Controla qué tipo de activación se usará por este botón cuando se haga clic en él. El valor predeterminado es Primer plano. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | falso | Novedad de la actualización Creators Update: obtiene o establece opciones adicionales relacionadas con la activación del botón de la notificación del sistema. |


### <a name="toastactivationtype"></a>ToastActivationType
Decide el tipo de activación que se usará cuando el usuario interactúa con una acción específica.

| Value | Significado |
|---|---|
| **Foreground** | Valor predeterminado. Se inicia la aplicación en primer plano. |
| **Background** | Se desencadena la tarea en segundo plano correspondiente (suponiendo que configures todo) y puedes ejecutar código en segundo plano (por ejemplo, enviando el mensaje de respuesta rápida del usuario) sin interrumpir al usuario. |
| **Protocol** | Inicia otra aplicación mediante la activación de protocolo. |


### <a name="toastactivationoptions"></a>ToastActivationOptions
Novedad de la actualización Creators Update: opciones adicionales relacionadas con la activación.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **AfterActivationBehavior** | [ToastAfterActivationBehavior](#toastafteractivationbehavior) | falso | Novedad en la actualización de Fall Creators Update: obtiene o establece el comportamiento que debe usar la notificación del sistema cuando el usuario invoca esta acción. Esto funciona únicamente en Escritorio, para [ToastButton](#toastbutton) y [ToastContextMenuItem](#toastcontextmenuitem). |
| **ProtocolActivationTargetApplicationPfn** | cadena | false | Si estás usando *ToastActivationType.Protocol*, puedes especificar opcionalmente el PFN destino, por lo que, con independencia de si se registran varias aplicaciones para controlar el mismo URI de protocolo, se iniciará siempre la aplicación que quieras. |


### <a name="toastafteractivationbehavior"></a>ToastAfterActivationBehavior
Especifica el comportamiento que la notificación del sistema debe usar cuando el usuario realiza la acción en la notificación del sistema.

| Valor | Significado |
|---|---|
| **Predeterminado** | Comportamiento predeterminado. La notificación del sistema se descartará cuando el usuario realiza la acción en la notificación del sistema. |
| **PendingUpdate** | Después de que el usuario hace clic en un botón de la notificación del sistema, la notificación permanecerá presente en un estado visual de "actualización pendiente". Debes actualizar inmediatamente la notificación del sistema desde una tarea en segundo plano para que el usuario no vea este estado visual de "actualización pendiente" demasiado tiempo. |


## <a name="toastbuttonsnooze"></a>ToastButtonSnooze
*Implementa [IToastButton](#itoastbutton)*

Botón de posponer controlado por el sistema que controla automáticamente el aplazamiento de la notificación.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **CustomContent** | cadena | false | Texto personalizado opcional que se muestra en el botón que invalida el texto de "Posponer" localizado predeterminado. |


## <a name="toastbuttondismiss"></a>ToastButtonDismiss
*Implementa [IToastButton](#itoastbutton)*

Botón Descartar controlado por el sistema que descarta la notificación al hacer clic en ella.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **CustomContent** | cadena | false | Texto personalizado opcional que se muestra en el botón que invalida el texto de "Descartar" localizado predeterminado. |


## <a name="toastactionssnoozeanddismiss"></a>ToastActionsSnoozeAndDismiss
*Implementa [IToastActions](#itoastactions)

Construye automáticamente un cuadro de selección para intervalos de aplazamiento, y botones de posponer/descartar, todas localizadas automáticamente, y la lógica de aplazamiento se controla automáticamente por el sistema.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | Novedad en la Actualización de aniversario: elementos de menú contextual personalizados, que proporcionan acciones adicionales en caso de que el usuario haga clic con el botón derecho en la notificación. Solo puedes tener un máximo de 5 elementos. |


## <a name="toastcontextmenuitem"></a>ToastContextMenuItem
Entrada de elemento de menú contextual.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Contenido** | cadena | true | Obligatorio. El texto que se va a mostrar. |
| **Arguments** | cadena | true | Obligatorio. Cadena definida por la aplicación de argumentos que la aplicación puede recuperar más adelante una vez se activa cuando el usuario hace clic en el elemento de menú. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Controla qué tipo de activación se usará por este elemento de menú cuando se haga clic en él. El valor predeterminado es Primer plano. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Novedad de la actualización Creators Updates: opciones adicionales relacionadas con la activación del elemento de menú contextual de notificación del sistema. |


## <a name="toastaudio"></a>ToastAudio
Especifica audio que se reproducirá cuando se reciba la notificación del sistema.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Src** | uri | false | El archivo multimedia que se reproducirá en lugar del sonido predeterminado. Solo se admiten ms-appx y ms-appdata. |
| **Bucle** | boolean | false | Establece en true si el sonido se debe repetir mientras se muestra la notificación del sistema; false para reproducirlo solo una sola vez (predeterminado). |
| **Silencio** | boolean | false | True para silenciar el sonido; false para permitir que se reproduzca el sonido de notificación del sistema (predeterminado). |


## <a name="toastheader"></a>ToastHeader
Novedad de la actualización Creators Update: encabezado personalizado que agrupa varias notificaciones en el centro de actividades.

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Id** | cadena | true | Identificador creado por el desarrollador que identifica de manera exclusiva este encabezado. Si dos notificaciones tienen el mismo identificador de encabezado, se mostrarán debajo del mismo encabezado en el centro de actividades. |
| **Título** | cadena | true | Título para el encabezado. |
| **Argumentos**| cadena | true | Obtiene o establece una cadena de argumentos definida por el desarrollador que se devuelve a la aplicación cuando el usuario hace clic en este encabezado. No puede ser null. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | falso | Obtiene o establece el tipo de activación que este encabezado usará cuando se haga clic en él. El valor predeterminado es Primer plano. Ten en cuenta que solo se admiten en Primer plano y Protocolo. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | falso | Obtiene o establece opciones adicionales relacionadas con la activación del encabezado de la notificación del sistema. |


## <a name="related-topics"></a>Temas relacionados

* [Inicio rápido: Enviar una notificación del sistema local y administrar la aplicación](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10.aspx)
* [Biblioteca de notificaciones en GitHub](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)