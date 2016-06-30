---
author: mijacobs
Description: Estos son los elementos y atributos que usas para crear iconos adaptables.
title: Plantillas y esquema de iconos adaptables
ms.assetid: 858FB05E-87A2-49CF-BE48-570980AD36C8
label: Adaptive tile schema and templates
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: a5d061515eee1ab64f17e4f5aab8846adbd1c8f1

---

# Plantillas de iconos adaptables: esquema e instrucciones

Estos son los elementos y atributos que usas para crear iconos adaptables. Para obtener instrucciones y ejemplos, consulta [Crear iconos adaptables](tiles-and-notifications-create-adaptive-tiles.md).

## <span id="tile_element"></span><span id="TILE_ELEMENT"></span>icono de ventana


``` syntax
<tile>
  
  <!-- Child elements -->
  visual
  
</tile>
```

## <span id="visual_element"></span><span id="VISUAL_ELEMENT"></span>elemento visual


``` syntax
<visual
  version? = integer
  lang? = string
  baseUri? = anyURI
  branding? = "none" | "logo" | "name" | "nameAndLogo"
  addImageQuery? = boolean
  contentId? = string
  displayName? = string >
    
  <!-- Child elements -->
  binding+

</visual>
```

## <span id="binding_element"></span><span id="BINDING_ELEMENT"></span>elemento de enlace


``` syntax
<binding
  template = tileTemplateNameV3
  fallback? = tileTemplateNameV1
  lang? = string
  baseUri? = anyURI
  branding? = "none" | "logo" | "name" | "nameAndLogo"
  addImageQuery? = boolean
  contentId? = string
  displayName? = string
  hint-textStacking? = "top" | "center" | "bottom"
  hint-overlay? = [0-100] >

  <!-- Child elements -->
  ( image
  | text
  | group
  )*

</binding>
```

## <span id="image_element"></span><span id="IMAGE_ELEMENT"></span>elemento de imagen


``` syntax
<image
  src = string
  placement? = "inline" | "background" | "peek"
  alt? = string
  addImageQuery? = boolean
  hint-crop? = "none" | "circle"
  hint-removeMargin? = boolean
  hint-align? = "stretch" | "left" | "center" | "right" />
```

## <span id="text_element"></span><span id="TEXT_ELEMENT"></span>elemento de texto


``` syntax
<text
  lang? = string
  hint-style? = textStyle
  hint-wrap? = boolean
  hint-maxLines? = integer
  hint-minLines? = integer
  hint-align? = "left" | "center" | "right" >

  <!-- text goes here -->

</text>
```

Valores de textStyle: caption captionSubtle body bodySubtle base baseSubtle subtitle subtitleSubtle title titleSubtle titleNumeral subheader subheaderSubtle subheaderNumeral header headerSubtle headerNumber

## <span id="group_element"></span><span id="GROUP_ELEMENT"></span>elemento de grupo


``` syntax
<group>

  <!-- Child elements -->
  subgroup+

</group>
```

## <span id="subgroup_element"></span><span id="SUBGROUP_ELEMENT"></span>elemento de subgrupo


``` syntax
<subgroup
  hint-weight? = [0-100]
  hint-textStacking? = "top" | "center" | "bottom" >

  <!-- Child elements -->
  ( text
  | image
  )*

</subgroup>
```

## <span id="related_topics"></span>Temas relacionados


* [Crear iconos adaptables](tiles-and-notifications-create-adaptive-tiles.md)
 

 







<!--HONumber=Jun16_HO4-->


