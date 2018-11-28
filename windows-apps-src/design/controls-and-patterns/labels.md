---
Description: Use a label to indicate to the user what they should enter into an adjacent control. You can also label a group of related controls, or display instructional text near a group of related controls.
title: Etiquetas
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4345daf5b879fed7ba9805e4a448c473299031d7
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7993311"
---
# <a name="labels"></a>Etiquetas

 

Una etiqueta es el nombre o título de un control o un grupo de controles relacionados.

> **API importantes**: propiedad Header, [Clase TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)

En XAML, muchos controles tienen una propiedad Header integrada que sirve para mostrar la etiqueta. En los controles sin propiedad Header o para etiquetar grupos de controles, puedes usar un [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) en su lugar.

![Captura de pantalla que muestra un control de etiqueta estándar](images/label-standard.png)

## <a name="recommendations"></a>Recomendaciones


-   Usa una etiqueta para indicar al usuario lo que debe escribir en un control adyacente. También puedes etiquetar un grupo de controles relacionados o mostrar un texto de instrucciones junto a un grupo de controles relacionados.
-   Al etiquetar controles, escribe una etiqueta que sea un sustantivo o una frase nominal breve, no una oración completa, y sin tono de instrucción. Evita los dos puntos u otro signo de puntuación.
-   Cuando haya un texto de instrucciones en una etiqueta, la cadena de texto puede ser más larga y también puedes usar signos de puntuación.


## <a name="get-the-sample-code"></a>Obtener el código de ejemplo
* [Muestra de conceptos básicos de una interfaz de usuario de XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>Temas relacionados
* [Controles de texto](text-controls.md)
* [Propiedad TextBox.Header](https://msdn.microsoft.com/library/windows/apps/dn252861)
* [Propiedad PasswordBox.Header](https://msdn.microsoft.com/library/windows/apps/dn299051)
* [Propiedad ToggleSwitch.Header](https://msdn.microsoft.com/library/windows/apps/br209713)
* [Propiedad DatePicker.Header](https://msdn.microsoft.com/library/windows/apps/dn279460)
* [Propiedad TimePicker.Header](https://msdn.microsoft.com/library/windows/apps/dn299286)
* [Propiedad Slider.Header](https://msdn.microsoft.com/library/windows/apps/dn252829)
* [Propiedad ComboBox.Header](https://msdn.microsoft.com/library/windows/apps/dn279416)
* [Propiedad RichEditBox.Header](https://msdn.microsoft.com/library/windows/apps/dn252726)
* [Clase TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)

 

 




