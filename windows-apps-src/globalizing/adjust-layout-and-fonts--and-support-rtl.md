---
Description: Desarrolla tu aplicación para admitir los diseños y fuentes de varios idiomas, incluida la dirección de flujo de derecha a izquierda.
title: Ajustar el diseño y las fuentes, y admitir la escritura de derecha a izquierda
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
---

# Ajustar el diseño y las fuentes, y admitir la escritura de derecha a izquierda


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Desarrolla tu aplicación para admitir los diseños y fuentes de varios idiomas, incluida la dirección de flujo de derecha a izquierda.

## <span id="Layout_guidelines"></span><span id="layout_guidelines"></span><span id="LAYOUT_GUIDELINES"></span>Directrices de diseño


Algunos idiomas, como el alemán y el finés, requieren más espacio para el texto que el inglés. Las fuentes para algunos idiomas, como el japonés, requieren más altura. Algunos idiomas, como el árabe y el hebreo, requieren que el diseño de texto y el diseño de la aplicación tengan el orden de lectura de derecha a izquierda.

Usa mecanismos de diseño flexibles en lugar del posicionamiento absoluto, los anchos de longitud fija o los altos de longitud fija. Cuando sea necesario, determinados elementos de interfaz de usuario pueden ajustarse según el idioma.

### <span id="XAML"></span><span id="xaml"></span>XAML

Especifica un objeto **Uid** para un elemento:

```XAML
<TextBlock x:Uid="Block1">
```

Asegúrate de que el archivo ResW de tu aplicación tenga un recurso para Block1.Width, que puedes establecer para cada idioma al que localices.

Para las aplicaciones de la Tienda Windows con C++, C\# o Visual Basic, usa la propiedad [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716), con márgenes y espaciado simétrico, para permitir la localización para otras direcciones de diseño.

Los controles de diseño de lenguaje XAML, como [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704), se escalan y voltean automáticamente con la propiedad [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716). Expón tu propia propiedad **FlowDirection** en la aplicación como un recurso para los localizadores.

Especifica un objeto **Uid** para la página principal de tu aplicación:

```XAML
<Page x:Uid="MainPage">
```

Asegúrate de que el archivo **ResW** de tu aplicación tenga un recurso para MainPage.FlowDirection, que puedes establecer para cada idioma al que localices.

### <span id="HTML"></span><span id="html"></span>HTML

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

## <span id="Mirroring_images"></span><span id="mirroring_images"></span><span id="MIRRORING_IMAGES"></span>Creación de espejos de imágenes


### <span id="XAML"></span><span id="xaml"></span>XAML

Si la aplicación tiene imágenes que deben reflejarse (es decir, se puede voltear la misma imagen) para la entrada de derecha a izquierda, puedes aplicar la propiedad [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716):

```XAML
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

### <span id="HTML"></span><span id="html"></span>HTML

Si tu aplicación tiene imágenes que deben reflejarse (es decir, se puede voltear la misma imagen) para la entrada de derecha a izquierda, puedes usar transformaciones de CSS para reflejar tus imágenes en el momento de la representación si agregas una clase .mirrorable a tus elementos y agregas la siguiente clase CSS:

```CSS
.mirrorable { transform: scaleX(-1); }
```

**Para XAML y HTML:**: si tu aplicación requiere de una imagen diferente para voltear la imagen correctamente, puedes usar el sistema de administración de recursos con el [calificador layoutdir](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324). El sistema elige una imagen llamada file.layoutdir-rtl.png cuando el [idioma de la aplicación](manage-language-and-region.md) se establece en un idioma de derecha a izquierda. Este método puede ser necesario cuando alguna parte de la imagen se voltea, pero otra no.

## <span id="Fonts"></span><span id="fonts"></span><span id="FONTS"></span>Fuentes


**Para XAML y HTML:** usa las API de asignación de fuentes [**LanguageFont**](https://msdn.microsoft.com/library/windows/apps/br206864) para acceder mediante programación a la familia, tamaño, peso y estilo de fuentes recomendados para un idioma en particular. El objeto **LanguageFont** proporciona acceso a la información de fuente correcta para diversas categorías de contenido, como encabezados de interfaz de usuario, notificaciones, cuerpo y fuentes de cuerpo de documento que los usuarios pueden editar.

### <span id="HTML"></span><span id="html"></span>HTML

Las fuentes de las aplicaciones de la Tienda Windows con JavaScript que usan las hojas de estilo ui-light.css o ui-dark.css se establecen automáticamente en la fuente más apropiada según el idioma de la aplicación. El host de aplicaciones establece el atributo **lang** del elemento raíz en el idioma de la aplicación.

Las aplicaciones que muestran varios idiomas en una única página deben establecer el atributo **lang** para la sección en cada idioma. El selector de seudoclase [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) elige la fuente correcta para cada sección de la página.

 

 





<!--HONumber=Mar16_HO4-->


