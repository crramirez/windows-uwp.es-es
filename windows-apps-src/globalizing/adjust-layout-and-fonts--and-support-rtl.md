---
author: DelfCo
Description: "Desarrolla tu aplicación para admitir los diseños y fuentes de varios idiomas, incluida la dirección de flujo de derecha a izquierda."
title: "Ajustar el diseño y las fuentes, y admitir la escritura RTL"
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 5255da14ccdd0aed3852c41fa662de63a7160fba
ms.openlocfilehash: b45029156a28afdb37d7ac1402d1e6ae845b0e63

---

# Ajustar el diseño y las fuentes, y admitir la escritura RTL





Desarrolla la aplicación para que admita los diseños y fuentes de varios idiomas, incluida la dirección de flujo RTL (de derecha a izquierda).

## Directrices de diseño


Algunos idiomas, como el alemán y el finés, requieren más espacio para el texto que el inglés. Las fuentes para algunos idiomas, como el japonés, requieren más altura. Algunos idiomas, como el árabe y el hebreo, requieren que el diseño de texto y el diseño de la aplicación tengan el orden de lectura de derecha a izquierda.

Usa mecanismos de diseño flexibles en lugar del posicionamiento absoluto, los anchos de longitud fija o los altos de longitud fija. Cuando sea necesario, determinados elementos de interfaz de usuario pueden ajustarse según el idioma.

### XAML

Especifica un objeto **Uid** para un elemento:

```XML
<TextBlock x:Uid="Block1">
```

Asegúrate de que el archivo ResW de tu aplicación tenga un recurso para Block1.Width, que puedes establecer para cada idioma al que localices.

Para las aplicaciones de la Tienda Windows con C++, C\# o Visual Basic, usa la propiedad [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716), con márgenes y espaciado simétrico, para permitir la localización para otras direcciones de diseño.

Los controles de diseño de lenguaje XAML, como [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704), se escalan y voltean automáticamente con la propiedad [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716). Expón tu propia propiedad **FlowDirection** en la aplicación como un recurso para los localizadores.

Especifica un objeto **Uid** para la página principal de tu aplicación:

```XML
<Page x:Uid="MainPage">
```

Asegúrate de que el archivo **ResW** de tu aplicación tenga un recurso para MainPage.FlowDirection, que puedes establecer para cada idioma al que localices.

### HTML

Para las aplicaciones de la Tienda Windows con JavaScript, usa [mecanismos de diseño de hojas de estilo CSS](https://msdn.microsoft.com/library/ms531209), como [-ms-grid](https://msdn.microsoft.com/en-us/library/windows/apps/hh465453.aspx#g_section) y [–ms-box](https://msdn.microsoft.com/en-us/library/windows/apps/hh465453.aspx#f_section). Usa márgenes y relleno simétricos para permitir la localización de diversas direcciones de diseño.

Tu aplicación también puede usar el selector de seudoclase [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) para ajustar las propiedades de CSS, como el ancho, en elementos concretos según el idioma de la aplicación. Para habilitarlo, el host de aplicaciones establece el atributo **lang** del elemento raíz en el idioma de la aplicación.

**CSS**
```CSS
.item:-ms-lang(de, fi) { width: 350px; }
```

La dirección de diseño de cuerpo de las aplicaciones de la Tienda Windows con JavaScript que usan las hojas de estilo ui-light.css o ui-dark.css se establece automáticamente según el idioma de la aplicación. Las CSS siguientes están establecidas en ui-light y ui-dark.css, y no es necesario que las escribas.

**CSS**
```CSS
body:-ms-lang(ar,he…) { direction: rtl;}
```

Esto significa que la mayoría de los diseños de aplicación se establece correctamente cuando el sistema usa un idioma de derecha a izquierda.

Al igual que los controles [WinJS.UI](https://msdn.microsoft.com/library/windows/apps/br229782), tu aplicación puede usar el selector de seudoclase [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) para ajustar las propiedades de CSS físicas, como **margin** y **padding**. No necesitas ajustar las propiedades de CSS lógicas que usan palabras clave como **after** y **before**.

No uses el atributo ni la propiedad **align** en HTML. En cambio, usa la propiedad **direction** para controlar la alineación de componentes particulares.

Usa la propiedad [**writing-mode**](https://msdn.microsoft.com/library/ms531187) para admitir diseños de texto vertical en CSS.

## Creación de imágenes reflejadas


### XAML

Si la aplicación tiene imágenes que deben reflejarse (es decir, se puede voltear la misma imagen) para la entrada de derecha a izquierda, puedes aplicar la propiedad [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716):

```XML
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

### HTML

Si tu aplicación tiene imágenes que deben reflejarse (es decir, se puede voltear la misma imagen) para la entrada de derecha a izquierda, puedes usar transformaciones de CSS para reflejar tus imágenes en el momento de la representación si agregas una clase .mirrorable a tus elementos y agregas la siguiente clase CSS:

```CSS
.mirrorable { transform: scaleX(-1); }
```

**Para XAML y HTML:**: Si la aplicación requiere una imagen diferente para invertir la imagen correctamente, puedes usar el Sistema de administración de recursos con el [calificador layoutdir](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324). El sistema elige una imagen llamada file.layoutdir-rtl.png cuando el [idioma de la aplicación](manage-language-and-region.md) se establece en un idioma de derecha a izquierda. Este método puede ser necesario cuando alguna parte de la imagen se voltea, pero otra no.

## Fuentes


**Para XAML y HTML:** Usa las API de asignación de fuentes [**LanguageFont**](https://msdn.microsoft.com/library/windows/apps/br206864) para acceder mediante programación a la familia, el tamaño, el peso y el estilo de fuente recomendados para un idioma en concreto. El objeto **LanguageFont** proporciona acceso a la información de fuente correcta para diversas categorías de contenido, como encabezados de interfaz de usuario, notificaciones, cuerpo y fuentes de cuerpo de documento que los usuarios pueden editar.

### HTML

Las fuentes de las aplicaciones de la Tienda Windows con JavaScript que usan las hojas de estilo ui-light.css o ui-dark.css se establecen automáticamente en la fuente más apropiada según el idioma de la aplicación. El host de aplicaciones establece el atributo **lang** del elemento raíz en el idioma de la aplicación.

Las aplicaciones que muestran varios idiomas en una única página deben establecer el atributo **lang** para la sección en cada idioma. El selector de seudoclase [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) elige la fuente correcta para cada sección de la página.

## Procedimientos recomendados para controlar los idiomas de lectura de derecha a izquierda (RTL)

Si la aplicación está localizada para idiomas de derecha a izquierda (RTL), usa API para establecer la dirección predeterminada del texto para la propiedad RootFrame. Esto hará que todos los controles contenidos en la propiedad RootFrame respondan adecuadamente a la dirección del texto predeterminada.  Si se admite más de un idioma, usa LayoutDirection para el idioma preferido con el fin de establecer la propiedad FlowDirection. La mayoría de los controles incluidos en Windows usan ya la propiedad FlowDirection. Si vas a implementar controles personalizados, estos deberían usar la propiedad FlowDirection para realizar los cambios de diseño adecuados para los idiomas de derecha a izquierda y de izquierda a derecha.

C#
```csharp    
// For bidirectional languages, determine flow direction for RootFrame and all derived UI.

    string resourceFlowDirection = ResourceContext.GetForCurrentView().QualifierValues["LayoutDirection"];
    if (resourceFlowDirection == "LTR")
    {
       RootFrame.FlowDirection = FlowDirection.LeftToRight;
    }
    else
    {
       RootFrame.FlowDirection = FlowDirection.RightToLeft;
    }
```
C++:
```cpp
    // Get preferred app language
    m_language = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);
     
    // Set flow direction accordingly
    m_flowDirection = ResourceManager::Current->DefaultContext->QualifierValues->Lookup("LayoutDirection") != "LTR" ? 
       FlowDirection::RightToLeft : FlowDirection::LeftToRight;
```


### Preguntas más frecuentes sobre los idiomas de derecha a izquierda (RTL) 

<dl>
  <dt> <p><b>P: ¿</b><b>FlowDirection</b> se establece automáticamente en función de la selección de idioma actual? Por ejemplo, ¿si se selecciona el idioma inglés, ¿se mostrará de izquierda a derecha y si se selecciona el árabe se mostrará de derecha a izquierda?</p></dt>

  <dd><p><b>R:</b> <b>FlowDirection</b> no tiene en cuenta el idioma. <b>FlowDirection</b> se establece de la forma apropiada para el idioma que se muestre actualmente. Consulta el ejemplo de código anterior.</p></dd> 

  <dt> <p><b>P:</b> No tengo muchos conocimientos sobre la localización. ¿Los recursos ya contienen la dirección del flujo? ¿Es posible determinar la dirección del flujo a partir del idioma actual?</p></dt>

  <dd> <p><b>R:</b> Si usas los procedimientos recomendados actuales, los recursos no contienen la dirección del flujo directamente. La dirección del flujo correspondiente al idioma actual la debes determinar tú. Hay dos maneras de hacerlo: </p>
   <p>La manera preferida es usar LayoutDirection para el idioma preferido con el fin de establecer la propiedad FlowDirection de RootFrame. Todos los controles de RootFrame heredan FlowDirection de RootFrame.</p>
   <p>Otra forma es establecer la propiedad FlowDirection en el archivo resw para los idiomas de derecha a izquierda para los que se esté localizando. Por ejemplo, podrías un archivo resw de árabe y un archivo resw de hebreo. En estos archivos, podría usar x: UID para establecer la propiedad FlowDirection. Sin embargo, este método es más propenso a errores que el método mediante programación.</p></dd>
</dl>


## Temas relacionados
[FlowDirection](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.flowdirection.aspx)



<!--HONumber=Aug16_HO3-->


