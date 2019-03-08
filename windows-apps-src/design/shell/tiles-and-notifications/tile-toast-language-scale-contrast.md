---
Description: Las ventanas y notificaciones del sistema pueden cargar cadenas y archivos adaptados al idioma, el factor de escala de visualización, el contraste alto y otros contextos de tiempo de ejecución.
title: Compatibilidad de ventanas y notificaciones del sistema para el idioma, la escala y el contraste alto.
template: detail.hbs
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: aa6e93196d30c15374129eee7714604cfab7b82e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601480"
---
# <a name="tile-and-toast-notification-support-for-language-scale-and-high-contrast"></a>Compatibilidad de ventanas y notificaciones del sistema para el idioma, la escala y el contraste alto.

Las ventanas y notificaciones del sistema pueden cargar cadenas y archivos adaptados al idioma, el [factor de escala de visualización](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md), el contraste alto y otros contextos de tiempo de ejecución. Para obtener información sobre cómo usar calificadores en los nombres de los archivos de recursos, consulte [adaptar los recursos de idioma, la escala y otros calificadores](../../../app-resources/tailor-resources-lang-scale-contrast.md) y [iconos de aplicación y los logotipos](/windows/uwp/design/style/app-icons-and-logos).

Para obtener más información sobre la propuesta de valor de localizar tu aplicación, consulta [Globalización y localización](../../globalizing/globalizing-portal.md).

## <a name="refer-to-a-string-resource-from-a-template"></a>Referirse a un recurso de cadena desde una plantilla

En tu ventana o plantilla de notificación del sistema, puedes consultar un recurso de cadena con el esquema de URI (identificador uniforme de recursos) `ms-resource` seguido de un identificador de recursos de cadena simple. Por ejemplo, si tienes un archivo Resources.resx que contenga una entrada de recursos cuyo nombre es "Farewell", en ese caso deberás tener un recurso de cadena con el identificador "Farewell". Para obtener más información sobre identificadores de recursos de cadena y archivos de recursos (.resw), consulta [Localizar las cadenas de la interfaz de usuario y el manifiesto de paquete de la aplicación](../../../app-resources/localize-strings-ui-manifest.md).

Esta es la forma en que aparecería una referencia al identificador de recursos de cadena "Farewell" en el cuerpo [texto](/uwp/schemas/tiles/tilesschema/element-text?branch=live) de la plantilla de contenido, usando `ms-resource`.

```xml
<text id="1">ms-resource:Farewell</text>
```

Si se omite el esquema de URI `ms-resource`, el cuerpo del texto es tan solo una cadena literal, y *no* una referencia a un identificador.

```xml
<text id="1">Farewell</text>
```

## <a name="refer-to-an-image-resource-from-a-template"></a>Referirse a un recurso de imagen desde una plantilla

En tu ventana o plantilla de notificación del sistema, puedes hacer referencia a un recurso de imagen usando el esquema de URI (identificador uniforme de recursos) `ms-appx`, seguido del nombre del recurso de imagen. Se trata de la misma manera en que se hace referencia a un recurso de imagen en el marcado XAML (para obtener más información, consulta [Hacer referencia a una imagen u otro activo de marcado y código XAML](../../../app-resources/images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)).

Por ejemplo, podrías denominar a las carpetas de esta manera.

```
\Assets\Images\contrast-standard\welcome.png
\Assets\Images\contrast-high\welcome.png
```

En ese caso, tiene un recurso de imagen único y su nombre (como ruta de acceso absoluta) es `/Assets/Images/welcome.png`. Esta es la manera de usar este nombre en la plantilla.

```xml
<image id="1" src="ms-appx:///Assets/Images/welcome.png"/>
```

Observa cómo en este URI de ejemplo, el esquema ("`ms-appx`") va seguido de "`://`", seguido a su vez por una ruta absoluta (una ruta absoluta comienza por "`/`").

## <a name="hosting-and-loading-images-in-the-cloud"></a>Alojamiento y carga de imágenes en la nube

Los esquemas de URI `ms-resource` y `ms-appx` llevan a cabo la comparación automática de calificadores para buscar el recurso que sea más adecuado para el contexto actual. Los esquemas de URI web (por ejemplo, `http`, `https` y `ftp`) no realizan ninguna comparación automática de esta clase.

En su lugar, anexa al URI de la imagen una cadena de consulta que describa el valor o valores de calificador solicitados.

```xml
<image id="1" src="http://www.contoso.com/Assets/Images/welcome.png?ms-lang=en-US"/>
```

A continuación, en el servicio de aplicación que proporciona las imágenes, implementa un controlador HTTP que inspeccione y use la cadena de consulta para determinar qué imagen devolver.

También debes establecer el atributo [**addImageQuery**](/uwp/schemas/tiles/tilesschema/element-visual?branch=live) en `true` en la carga de trabajo XML de [ventana](/uwp/schemas/tiles/tilesschema/schema-root?branch=live) o [notificación del sistema](/uwp/schemas/tiles/toastschema/schema-root?branch=live). El atributo **addImageQuery** aparece en los elementos `visual`, `binding` e `image` de los esquemas de ventanas y notificaciones del sistema. Establecer explícitamente **addImageQuery** en un elemento invalida cualquier valor establecido en un elemento anterior. Por ejemplo, un valor **addImageQuery** de `true` en un elemento `image` invalida un **addImageQuery** de `false` en su elemento primario de `binding`.

Estas son las cadenas de consulta que puedes usar.

| Calificador | Cadena de consulta | Ejemplo |
| --------- | ------------ | ------- |
| Escala | ms-scale | ?ms-scale=400 |
| Idioma | ms-lang | ?ms-lang=en-US |
| Compare | ms-contrast | ?ms-contrast=high |

Para ver una tabla de referencia de todos los valores de calificador posibles que puedes usar en las cadenas de consulta, atiende a [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

## <a name="important-apis"></a>API importantes

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)

## <a name="related-topics"></a>Temas relacionados

* [Tamaños de pantalla y los puntos de interrupción para el diseño dinámico](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Adaptar los recursos de idioma, la escala y otros calificadores](../../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Directrices de activos de ventanas e iconos](app-assets.md).
* [Globalización y localización](../../globalizing/globalizing-portal.md)
* [Localizar cadenas en el manifiesto de paquete de interfaz de usuario y la aplicación](../../../app-resources/localize-strings-ui-manifest.md)
* [Hacer referencia a una imagen u otros activos de código y marcado XAML](../../../app-resources/images-tailored-for-scale-theme-contrast.md)
* [addImageQuery](/uwp/schemas/tiles/tilesschema/element-visual?branch=live)
* [Esquema de icono](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)
* [Esquema de notificación del sistema](/uwp/schemas/tiles/toastschema/schema-root?branch=live)
