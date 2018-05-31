---
author: stevewhims
Description: Design your app to support the layouts and fonts of multiple languages, including RTL (right-to-left) flow direction.
title: Ajustar el diseño y las fuentes y admitir RTL
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.author: stwhi
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, localizabilidad, localización, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: cf3a2d781dc916fbda9a9d6386dee4e2e6144873
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
ms.locfileid: "1673982"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>Ajustar el diseño y las fuentes, y admitir RTL

Diseña tu aplicación para admitir los diseños y fuentes de varios idiomas, incluida la dirección de flujo de derecha a izquierda (RTL). La dirección del flujo es la dirección en la que el script está escrito y se muestra y en la que los elementos de la interfaz de usuario se analizan por el ojo.

## <a name="layout-guidelines"></a>Directrices de diseño

Por lo general, idiomas como el alemán y el finlandés usan más caracteres que el inglés. Por lo general, las fuentes asiáticos requieren más altura. Y algunos idiomas, como el árabe y el hebreo, requieren que los paneles de diseño y los elementos de texto tengan el orden de lectura de derecha a izquierda (RTL).

Debido a la duración de la variable del texto traducido, debes usar los mecanismos de diseño de interfaz de usuario dinámicos en lugar del posicionamiento absoluto, los anchos fijos o las alturas fijos. Una pseudolocalización de tu aplicación va a revelar cualquier particularidad problemática en la que los elementos de la interfaz de usuario no se ajustarán al tamaño adecuadamente.

Para los lenguajes RTL, usa la propiedad [**FrameworkElement.FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) y fija márgenes y rellenos simétricos. Los paneles de diseño como [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live) se escalan y voltean automáticamente con el valor de **FlowDirection** que has fijado.

Este es un ejemplo de cómo puedes exponer una propiedad FlowDirection en tu aplicación como un recurso que los localizadores pueden establecer correctamente.

En primer lugar, en tu archivo de recursos (.resw) de la aplicación, agrega un identificador de propiedad con el nombre "MainPage.FlowDirection" (en lugar de "MainPage" puedes usar el nombre que quieras).

A continuación, usa un **x:Uid** para asociar el elemento de **Página** principal con este identificador de propiedad.

```xaml
<Page x:Uid="MainPage">
```

Para obtener más información sobre archivos de recursos (.resw), identificadores de propiedades y **x:Uid**, consulta [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](../../app-resources/localize-strings-ui-manifest.md).

Debes evitar establecer los valores de diseño absolutos en cualquier elemento de interfaz de usuario en función del idioma. Sin embargo, si es absolutamente inevitable, puedes crear un identificador de propiedad del formulario "TitleText.Width".

```xaml
<TextBlock x:Uid="TitleText">
```

## <a name="fonts"></a>Fuentes

Usa la clase de asignación de fuentes [**LanguageFont**](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live) para acceder mediante programación al estilo, el espesor, el tamaño y la familia de fuentes recomendados para un idioma en particular. La clase **LanguageFont** proporciona acceso a la información de fuente correcta para diversas categorías de contenido, como encabezados de interfaz de usuario, notificaciones, texto del cuerpo y fuentes de cuerpo de documento que los usuarios pueden editar.

## <a name="mirroring-images"></a>Creación de imágenes reflejadas

Si la aplicación tiene imágenes que deben reflejarse (es decir, se puede voltear la misma imagen) para RTL, puedes usar **FlowDirection**.

```xaml
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

Si la aplicación requiere una imagen diferente para voltear la imagen correctamente, puedes usar el sistema de administración de recursos con el calificador `LayoutDirection` (ver la sección LayoutDirection de [Adaptar los recursos al idioma, la escala y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md#layoutdirection)). El sistema elige una imagen llamada `file.layoutdir-rtl.png` cuando el idioma de tiempo de ejecución de la aplicación (ver [Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación](manage-language-and-region.md)) se establece en un idioma RTL. Este enfoque puede ser necesario cuando alguna parte de la imagen se voltea, pero otra no.

## <a name="best-practices-for-handling-right-to-left-rtl-languages"></a>Procedimientos recomendados para controlar los idiomas de lectura de derecha a izquierda (RTL)

Si la aplicación está localizada para idiomas de derecha a izquierda (RTL), usa API para establecer la dirección predeterminada del texto para el panel de diseño raíz de tu página. Esto hará que todos los controles contenidos en el panel raíz respondan adecuadamente a la dirección del texto predeterminada. Si se admite más de un idioma, usa `LayoutDirection` para el idioma preferido de tiempo de ejecución de la aplicación a fin de configurar la propiedad [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) (ver ejemplo de código a continuación). La mayoría de los controles incluidos en Windows ya usan **FlowDirection**. Si vas a implementar controles personalizados, estos deberían usar la propiedad **FlowDirection** para realizar los cambios de diseño adecuados para los idiomas RTL y LTR.

```csharp    
this.languageTag = Windows.Globalization.ApplicationLanguages.Languages[0];

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

var flowDirectionSetting = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["LayoutDirection"];
if (flowDirectionSetting == "LTR")
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.LeftToRight;
}
else
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.RightToLeft;
}
```

```cpp
this->languageTag = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView()->QualifierValues->Lookup("LayoutDirection");
if (flowDirectionSetting == "LTR")
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::LeftToRight;
}
else
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::RightToLeft;
}
```

### <a name="rtl-faq"></a>Preguntas más frecuentes sobre los idiomas de derecha a izquierda (RTL) 

**P:** ¿Se establece **FlowDirection** automáticamente en función de la selección de idioma actual? Por ejemplo, ¿si se selecciona el idioma inglés, ¿se mostrará de izquierda a derecha y si se selecciona el árabe se mostrará de derecha a izquierda?

> **A:** **FlowDirection** no tiene en cuenta el idioma. **FlowDirection** se establece de la forma apropiada para el idioma que se muestre actualmente. Consulta el ejemplo de código anterior.

**P:** No tengo muchos conocimientos sobre la localización. ¿Los recursos ya contienen la dirección del flujo? ¿Es posible determinar la dirección del flujo a partir del idioma actual?

> **R:** Si usas los procedimientos recomendados actuales, los recursos no contendrán la dirección del flujo directamente. La dirección del flujo correspondiente al idioma actual la debes determinar tú. Hay dos maneras de hacerlo.
> 
> La manera preferida es usar **LayoutDirection** para el idioma preferido con el fin de establecer la propiedad **FlowDirection** de RootFrame. Todos los controles de RootFrame heredan FlowDirection de RootFrame.
> 
> Otra forma es establecer la propiedad FlowDirection en el archivo .resw para los idiomas RTL para los que estás localizando. Por ejemplo, podrías tener un archivo resw en árabe y un archivo resw en hebreo. En estos archivos, podría usar x: UID para establecer la propiedad FlowDirection. Sin embargo, este método es más propenso a errores que el método de programación.

## <a name="important-apis"></a>API importantes

* [FrameworkElement.FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>Artículos relacionados

* [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](../../app-resources/localize-strings-ui-manifest.md)
* [Adaptar los recursos al idioma, la escala y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación](manage-language-and-region.md)