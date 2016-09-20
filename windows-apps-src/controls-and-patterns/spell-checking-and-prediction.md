---
author: Jwmsft
Description: "Durante la edición y entrada de texto, la corrección ortográfica informa al usuario que una palabra está escrita erróneamente resaltándola con una línea en zigzag roja y brindando una forma en la que el usuario puede corregir el error."
title: "Revisión ortográfica y predicción de texto"
ms.assetid: B867C956-5AB2-4207-A8DE-179CE7871180
label: Spell checking and text prediction
template: detail.hbs
redirect_url: text-controls
ms.sourcegitcommit: 08f982f5c54d596a7624fe63528f91375e7761ca
ms.openlocfilehash: 8954b14a84241686a7c784c3d250f197bed7c8b6

---

# Directrices sobre revisión ortográfica

Durante la edición y entrada de texto, la revisión ortográfica informa al usuario de que una palabra está escrita de forma errónea resaltándola con una línea en zigzag roja y permite que el usuario pueda corregir el error.

**API importantes**

-   [**Propiedad IsSpellCheckEnabled (XAML)**](https://msdn.microsoft.com/library/windows/apps/br209688)


## <span id="checklist_section"></span><span id="CHECKLIST_SECTION"></span>Recomendaciones


-   Usa la revisión ortográfica para ayudar a los usuarios a medida que escriben palabras u oraciones en controles de escritura de texto. Revisa la ortografía de las palabras con entradas táctil, de mouse y de teclado.
-   No uses la revisión ortográfica si es probable que una palabra no esté en el diccionario o si no aporta valor a los usuarios. Por ejemplo, no la actives para cuadros de entrada de contraseñas, números de teléfono o nombres. La revisión ortográfica está deshabilitada de forma predeterminada para esos controles.
-   No deshabilites la revisión ortográfica solo porque el motor de revisión ortográfica actual no admite el idioma de tu aplicación. Si el corrector ortográfico no admite un idioma, no hace nada y dejar la opción activada no causa ningún trastorno. Además, es posible que algunos usuarios usen un editor de métodos de entrada (IME) para incorporar otro idioma a tu aplicación y puede que el corrector sí admita ese idioma. Por ejemplo, al crear una aplicación en japonés, no desactives la revisión ortográfica aunque el motor de revisión no reconozca todavía este idioma. El usuario puede cambiar a un IME en inglés y empezar a escribir en inglés en la aplicación; si la revisión ortográfica está habilitada, se revisará la ortografía de los textos en inglés.

## <span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Instrucciones de uso adicionales


Las aplicaciones de la Tienda Windows proporcionan un corrector ortográfico integrado para los cuadros de entrada de texto en una línea o multilínea, así como elementos cuya propiedad **contentEditable** está establecida en **true**. Este es un ejemplo del corrector ortográfico integrado:

![el corrector ortográfico integrado](images/spellchecking.png)

Para obtener más información, consulta [**Clase TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683).

Usa la revisión ortográfica con controles de entrada de texto para estos dos propósitos:

-   **Para una corrección automática de los errores ortográficos**

    El motor de revisión ortográfica corrige de forma automática las palabras mal escritas cuando está seguro de la corrección. Por ejemplo, el motor cambia automáticamente "dle" por "del".

-   **Para mostrar una ortografía alternativa**

    Cuando el motor de revisión ortográfica no está seguro de las correcciones, agrega una línea roja debajo de la palabra mal escrita y muestra las alternativas en un menú contextual cuando pulsas o haces clic con el botón secundario en la palabra.

Para los controles de JavaScript, la revisión ortográfica está activada de forma predeterminada para los controles de escritura de texto multilínea, y desactivada para los controles de texto de una línea. Puedes activarla manualmente para los controles de texto en una línea estableciendo la propiedad **revisión ortográfica** del control en **true**. Puedes deshabilitar la revisión ortográfica para un control estableciendo su propiedad **revisión ortográfica** en **false**.

Para los controles TextBox de XAML, la revisión ortográfica está desactivada de forma predeterminada. Para activarla, establece la propiedad **IsSpellCheckEnabled** en **true**.



## <span id="related_topics"></span>Artículos relacionados

* [Texto y controles de texto](text-controls.md)
* [Directrices para la entrada de texto](https://msdn.microsoft.com/library/windows/apps/hh750315)
* 
            [Directrices sobre texto y tipografía](https://msdn.microsoft.com/library/windows/apps/hh700394)
            
          
            **Para desarrolladores (XAML)**
          
* [**Propiedad TextBox.IsSpellCheckEnabled**](https://msdn.microsoft.com/library/windows/apps/br209688)
* [**Clase TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)

 







<!--HONumber=Jun16_HO5-->


