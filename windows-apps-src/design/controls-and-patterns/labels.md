---
Description: Usa una etiqueta para indicar al usuario lo que debe escribir en un control adyacente. También puedes etiquetar un grupo de controles relacionados o mostrar un texto de instrucciones junto a un grupo de controles relacionados.
title: Etiquetas
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 68b062dc4bd70c81b1b8b57808fad8e9c7498d75
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172619"
---
# <a name="labels"></a>Etiquetas

 

Una etiqueta es el nombre o título de un control o un grupo de controles relacionados.

> **API importantes**: propiedad Header, [Clase TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

En XAML, muchos controles tienen una propiedad Header integrada que sirve para mostrar la etiqueta. En los controles sin una propiedad Header, o para etiquetar grupos de controles, puedes usar un [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) en su lugar.

![Captura de pantalla que muestra un control de etiqueta estándar](images/label-standard.png)

## <a name="recommendations"></a>Recomendaciones


-   Usa una etiqueta para indicar al usuario lo que debe escribir en un control adyacente. También puedes etiquetar un grupo de controles relacionados o mostrar un texto de instrucciones junto a un grupo de controles relacionados.
-   Al etiquetar controles, escribe una etiqueta que sea un sustantivo o una frase nominal breve, no una oración completa, y sin tono de instrucción. Evita los dos puntos u otro signo de puntuación.
-   Cuando haya un texto de instrucciones en una etiqueta, la cadena de texto puede ser más larga y también puedes usar signos de puntuación.


## <a name="get-the-sample-code"></a>Obtención del código de ejemplo
* [Ejemplo de conceptos básicos de interfaz de usuario de XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>Temas relacionados
* [Controles de texto](text-controls.md)
* [Propiedad TextBox.Header](/uwp/api/windows.ui.xaml.controls.textbox.header)
* [Propiedad PasswordBox.Header](/uwp/api/windows.ui.xaml.controls.passwordbox.header)
* [Propiedad ToggleSwitch.Header](/uwp/api/windows.ui.xaml.controls.toggleswitch.header)
* [Propiedad DatePicker.Header](/uwp/api/windows.ui.xaml.controls.datepicker.header)
* [Propiedad TimePicker.Header](/uwp/api/windows.ui.xaml.controls.timepicker.header)
* [Propiedad Slider.Header](/uwp/api/windows.ui.xaml.controls.slider.header)
* [Propiedad ComboBox.Header](/uwp/api/windows.ui.xaml.controls.combobox.header)
* [Propiedad RichEditBox.Header](/uwp/api/windows.ui.xaml.controls.richeditbox.header)
* [Clase TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

 

 