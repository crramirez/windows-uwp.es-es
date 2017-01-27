---
author: DelfCo
Description: "Desarrolla tu aplicación para admitir los diseños y fuentes de varios idiomas, incluida la dirección de flujo de derecha a izquierda."
title: "Ajustar el diseño y las fuentes, y admitir la escritura RTL"
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: 05208607b148a48e8a680691de90161feeea7700

---

# <a name="adjust-layout-and-fonts-and-support-rtl"></a>Ajustar el diseño y las fuentes, y admitir la escritura RTL
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Desarrolla la aplicación para que admita los diseños y fuentes de varios idiomas, incluida la dirección de flujo RTL (de derecha a izquierda).

## <a name="layout-guidelines"></a>Directrices de diseño


Algunos idiomas, como el alemán y el finés, requieren más espacio para el texto que el inglés. Las fuentes para algunos idiomas, como el japonés, requieren más altura. Algunos idiomas, como el árabe y el hebreo, requieren que el diseño de texto y el diseño de la aplicación tengan el orden de lectura de derecha a izquierda.

Usa mecanismos de diseño flexibles en lugar del posicionamiento absoluto, los anchos de longitud fija o los altos de longitud fija. Cuando sea necesario, determinados elementos de interfaz de usuario pueden ajustarse según el idioma.

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


## <a name="mirroring-images"></a>Creación de imágenes reflejadas

Si la aplicación tiene imágenes que deben reflejarse (es decir, se puede voltear la misma imagen) para la entrada de derecha a izquierda, puedes aplicar la propiedad [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716):

```XML
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```


Si tu aplicación requiere de una imagen diferente para voltear la imagen correctamente, puedes usar el sistema de administración de recursos con el [calificador layoutdir](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324). El sistema elige una imagen llamada file.layoutdir-rtl.png cuando el [idioma de la aplicación](manage-language-and-region.md) se establece en un idioma de derecha a izquierda. Este enfoque puede ser necesario cuando alguna parte de la imagen se voltea, pero otra no.

## <a name="fonts"></a>Fuentes

Usa las API de asignación de fuentes [**LanguageFont**](https://msdn.microsoft.com/library/windows/apps/br206864) para obtener acceso mediante programación al estilo, el peso, el tamaño y la familia de fuentes recomendados para un idioma en particular. El objeto **LanguageFont** proporciona acceso a la información de fuente correcta para diversas categorías de contenido, como encabezados de interfaz de usuario, notificaciones, cuerpo y fuentes de cuerpo de documento que los usuarios pueden editar.

## <a name="best-practices-for-handling-right-to-left-rtl-languages"></a>Procedimientos recomendados para controlar los idiomas de lectura de derecha a izquierda (RTL)

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
```
    // Get preferred app language
    m_language = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);
     
    // Set flow direction accordingly
    m_flowDirection = ResourceManager::Current->DefaultContext->QualifierValues->Lookup("LayoutDirection") != "LTR" ? 
       FlowDirection::RightToLeft : FlowDirection::LeftToRight;
```


### <a name="rtl-faq"></a>Preguntas más frecuentes sobre los idiomas de derecha a izquierda (RTL) 

<dl>
  <dt> <p><b>P: ¿</b><b>FlowDirection</b> se establece automáticamente en función de la selección de idioma actual? Por ejemplo, ¿si se selecciona el idioma inglés, ¿se mostrará de izquierda a derecha y si se selecciona el árabe se mostrará de derecha a izquierda?</p></dt>

  <dd><p><b>R:</b> <b>FlowDirection</b> no tiene en cuenta el idioma. <b>FlowDirection</b> se establece de la forma apropiada para el idioma que se muestre actualmente. Consulta el ejemplo de código anterior.</p></dd> 

  <dt> <p><b>P:</b> No tengo muchos conocimientos sobre la localización. ¿Los recursos ya contienen la dirección del flujo? ¿Es posible determinar la dirección del flujo a partir del idioma actual?</p></dt>

  <dd> <p><b>R:</b> Si usas los procedimientos recomendados actuales, los recursos no contienen la dirección del flujo directamente. La dirección del flujo correspondiente al idioma actual la debes determinar tú. Hay dos maneras de hacerlo: </p>
   <p>La manera preferida es usar LayoutDirection para el idioma preferido con el fin de establecer la propiedad FlowDirection de RootFrame. Todos los controles de RootFrame heredan FlowDirection de RootFrame.</p>
   <p>Otra forma es establecer la propiedad FlowDirection en el archivo resw para los idiomas de derecha a izquierda para los que se esté localizando. Por ejemplo, podrías un archivo resw de árabe y un archivo resw de hebreo. En estos archivos, podría usar x: UID para establecer la propiedad FlowDirection. Sin embargo, este método es más propenso a errores que el método mediante programación.</p></dd>
</dl>


## <a name="related-topics"></a>Temas relacionados
[FlowDirection](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.flowdirection.aspx)



<!--HONumber=Dec16_HO2-->


