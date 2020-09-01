---
Description: Los iconos y las notificaciones del sistema pueden cargar cadenas e imágenes personalizadas para el idioma de visualización, mostrar el factor de escala, el contraste alto y otros contextos en tiempo de ejecución.
title: Compatibilidad con las notificaciones de icono y del sistema para el idioma, la escala y el contraste alto
template: detail.hbs
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: 88bcd5d6ce59d0561e76f46f6291f58ad03ddf3c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156739"
---
# <a name="tile-and-toast-notification-support-for-language-scale-and-high-contrast"></a>Compatibilidad con las notificaciones de icono y del sistema para el idioma, la escala y el contraste alto

Los iconos y las notificaciones del sistema pueden cargar cadenas e imágenes personalizadas para el idioma de visualización, [Mostrar el factor de escala](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md), el contraste alto y otros contextos en tiempo de ejecución. Para obtener información sobre cómo usar calificadores en los nombres de los archivos de recursos, consulte [adaptar los recursos para el idioma, la escala y otros calificadores](../../../app-resources/tailor-resources-lang-scale-contrast.md) y los [iconos y logotipos](../../style/app-icons-and-logos.md)de la aplicación.

Para más información sobre la propuesta de valor de localizar la aplicación, consulta [Globalización y localización](../../globalizing/globalizing-portal.md).

## <a name="refer-to-a-string-resource-from-a-template"></a>Referencia a un recurso de cadena de una plantilla

En la plantilla de icono o del sistema, puede hacer referencia a un recurso de cadena mediante el `ms-resource` esquema URI (identificador uniforme de recursos) seguido de un identificador de recurso de cadena simple. Por ejemplo, si tiene un archivo resources. resx que contiene una entrada de recurso cuyo nombre es "despedida", tiene un recurso de cadena con el identificador "despedida". Para obtener más información sobre los identificadores de recursos de cadena y los archivos de recursos (. resw), consulte [localizar cadenas en la interfaz de usuario y el manifiesto del paquete de la aplicación](../../../app-resources/localize-strings-ui-manifest.md).

Este es el aspecto de una referencia al identificador de recurso de cadena "despedida" en el cuerpo del [texto](/uwp/schemas/tiles/tilesschema/element-text?branch=live) del contenido de la plantilla, mediante `ms-resource` .

```xml
<text id="1">ms-resource:Farewell</text>
```

Si omite el `ms-resource` esquema de URI, el cuerpo del texto es simplemente un literal de cadena y *no* una referencia a un identificador.

```xml
<text id="1">Farewell</text>
```

## <a name="refer-to-an-image-resource-from-a-template"></a>Referencia a un recurso de imagen desde una plantilla

En la plantilla de icono o del sistema, puede hacer referencia a un recurso de imagen mediante el `ms-appx` esquema URI (identificador uniforme de recursos) seguido del nombre del recurso de imagen. Esta es la misma manera en que se hace referencia a un recurso de imagen en el marcado XAML (para obtener más información, vea [hacer referencia a una imagen u otro recurso desde el marcado y el código XAML](../../../app-resources/images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)).

Por ejemplo, puede asignar un nombre a carpetas como este.

```
\Assets\Images\contrast-standard\welcome.png
\Assets\Images\contrast-high\welcome.png
```

En ese caso, tiene un recurso de imagen único y su nombre (como una ruta de acceso absoluta) es `/Assets/Images/welcome.png` . Aquí se muestra cómo usar ese nombre en la plantilla.

```xml
<image id="1" src="ms-appx:///Assets/Images/welcome.png"/>
```

Observe cómo en este URI de ejemplo el esquema (" `ms-appx` ") va seguido de " `://` ", que va seguido de una ruta de acceso absoluta (una ruta de acceso absoluta comienza por " `/` ").

## <a name="hosting-and-loading-images-in-the-cloud"></a>Hospedaje y carga de imágenes en la nube

Los `ms-resource` `ms-appx` esquemas de URI y realizan la coincidencia automática del calificador para buscar el recurso más adecuado para el contexto actual. Los esquemas de URI web (por ejemplo, `http` , `https` y `ftp` ) no realizan ninguna coincidencia automática.

En su lugar, anexe al URI de la imagen una cadena de consulta que describa el valor o los valores del calificador solicitados.

```xml
<image id="1" src="http://www.contoso.com/Assets/Images/welcome.png?ms-lang=en-US"/>
```

A continuación, en el servicio de aplicaciones que proporciona las imágenes, implemente un controlador HTTP que inspeccione y use la cadena de consulta para determinar la imagen que se va a devolver.

También debe establecer el atributo [**addImageQuery**](/uwp/schemas/tiles/tilesschema/element-visual?branch=live) en `true` en la carga XML del [icono](/uwp/schemas/tiles/tilesschema/schema-root?branch=live) o la notificación del [sistema](/uwp/schemas/tiles/toastschema/schema-root?branch=live) . El atributo **addImageQuery** aparece en los `visual` `binding` elementos, y `image` de los esquemas de icono y del sistema. Al establecer **addImageQuery** explícitamente en un elemento, se invalida cualquier valor establecido en un antecesor. Por ejemplo, un valor **addImageQuery** de `true` en un `image` elemento invalida un **addImageQuery** de `false` en su `binding` elemento primario.

Estas son las cadenas de consulta que puede usar.

| Calificador: | Cadena de consulta | Ejemplo |
| --------- | ------------ | ------- |
| Escala | a escala de MS | ? MS-Scale = 400 |
| Idioma | MS-lang | ? MS-lang = en-US |
| Compare | MS-Contrast | ? MS-Contrast = alto |

Para obtener una tabla de referencia de todos los posibles valores de calificador que puede usar en las cadenas de consulta, vea [ResourceContext. QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

## <a name="important-apis"></a>API importantes

* [ResourceContext. QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)

## <a name="related-topics"></a>Temas relacionados

* [Tamaños de pantalla y puntos de interrupción de diseño adaptativo](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Adapte los recursos para el idioma, la escala y otros calificadores](../../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Directrices para los recursos de iconos e iconos](../../style/app-icons-and-logos.md).
* [Globalización y localización](../../globalizing/globalizing-portal.md)
* [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](../../../app-resources/localize-strings-ui-manifest.md)
* [Referencia a una imagen u otro recurso desde el marcado y el código XAML](../../../app-resources/images-tailored-for-scale-theme-contrast.md)
* [addImageQuery](/uwp/schemas/tiles/tilesschema/element-visual?branch=live)
* [Esquema de iconos](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)
* [Esquema de notificaciones del sistema](/uwp/schemas/tiles/toastschema/schema-root?branch=live)