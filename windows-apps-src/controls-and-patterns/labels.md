---
author: Jwmsft
Description: "Usa una etiqueta para indicar al usuario lo que debe escribir en un control adyacente. También puedes etiquetar un grupo de controles relacionados o mostrar un texto de instrucciones junto a un grupo de controles relacionados."
title: Etiquetas
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: b14c775896984095f580aed8cf41d010f408f67e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="labels"></a>Etiquetas

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Una etiqueta es el nombre o título de un control o un grupo de controles relacionados.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>Propiedad Header</li>
<li>[**Clase TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)</li>
</ul>
</div>


En XAML, muchos controles tienen una propiedad Header integrada que sirve para mostrar la etiqueta. En los controles sin propiedad Header o para etiquetar grupos de controles, puedes usar un [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) en su lugar.


## <a name="example"></a>Ejemplo


![Captura de pantalla que muestra un control de etiqueta estándar](images/label-standard.png)

## <a name="recommendations"></a>Recomendaciones


-   Usa una etiqueta para indicar al usuario lo que debe escribir en un control adyacente. También puedes etiquetar un grupo de controles relacionados o mostrar un texto de instrucciones junto a un grupo de controles relacionados.
-   Al etiquetar controles, escribe una etiqueta que sea un sustantivo o una frase nominal breve, no una oración completa, y sin tono de instrucción. Evita los dos puntos u otro signo de puntuación.
-   Cuando haya un texto de instrucciones en una etiqueta, la cadena de texto puede ser más larga y también puedes usar signos de puntuación.


## <a name="get-the-sample-code"></a>Obtener el código de ejemplo
* [Muestra de conceptos básicos de una interfaz de usuario de XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>Temas relacionados
* [Controles de texto](text-controls.md)

**Para desarrolladores**
* [**Propiedad TextBox.Header**](https://msdn.microsoft.com/library/windows/apps/dn252861)
* [**Propiedad PasswordBox.Header**](https://msdn.microsoft.com/library/windows/apps/dn299051)
* [**Propiedad ToggleSwitch.Header**](https://msdn.microsoft.com/library/windows/apps/br209713)
* [**Propiedad DatePicker.Header**](https://msdn.microsoft.com/library/windows/apps/dn279460)
* [**Propiedad TimePicker.Header**](https://msdn.microsoft.com/library/windows/apps/dn299286)
* [**Propiedad Slider.Header**](https://msdn.microsoft.com/library/windows/apps/dn252829)
* [**Propiedad ComboBox.Header**](https://msdn.microsoft.com/library/windows/apps/dn279416)
* [**Propiedad RichEditBox.Header**](https://msdn.microsoft.com/library/windows/apps/dn252726)
* [**Clase TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)

 

 




