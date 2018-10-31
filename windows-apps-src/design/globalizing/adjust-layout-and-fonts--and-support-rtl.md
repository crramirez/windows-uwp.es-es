---
author: stevewhims
Description: Design your app to support the layouts and fonts of multiple languages, including RTL (right-to-left) flow direction.
title: Ajustar el diseño y las fuentes y admitir RTL
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.author: stwhi
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, localizabilidad, localización, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: ac69701eca128d327dfd80cfddc7fad41c50c50e
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5868762"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>Ajustar el diseño y las fuentes, y admitir RTL
Diseña tu aplicación para admitir los diseños y fuentes de varios idiomas, incluida la dirección de flujo de derecha a izquierda (RTL). La dirección del flujo es la dirección en la que el script está escrito y se muestra y en la que los elementos de la interfaz de usuario se analizan por el ojo.

## <a name="layout-guidelines"></a>Directrices de diseño
Por lo general, idiomas como el alemán y el finlandés usan más caracteres que el inglés. Por lo general, las fuentes asiáticos requieren más altura. Y algunos idiomas, como el árabe y el hebreo, requieren que los paneles de diseño y los elementos de texto tengan el orden de lectura de derecha a izquierda (RTL).

Debido a estas variaciones en las métricas de texto traducido, te recomendamos que no utilices el posicionamiento absoluto, anchos fijos o altos fijos en la interfaz de usuario (UI). En su lugar, saca provecho de los mecanismos de diseño dinámico que están integradas en los elementos de interfaz de usuario de Windows. Por ejemplo, los controles de contenido (como botones), los controles de elementos (como vistas de cuadrícula y vistas de lista) y los paneles de diseño (como cuadrículas y elementos StackPanel) cambian automáticamente el tamaño y redistribuyen de forma predeterminada para que se ajusten a su contenido. Pseudolocaliza tu aplicación para revelar cualquier particularidad problemática en la que los elementos de la interfaz de usuario no se ajusten al tamaño adecuadamente.

El diseño dinámico es la técnica recomendada y podrás usarlo en la mayoría de los casos. Menos preferible, pero aún mejor que preparar tamaños en el marcado de interfaz de usuario, es establecer los valores de diseño en función del idioma. Este es un ejemplo de cómo puedes exponer una propiedad Width en tu aplicación como un recurso que los localizadores pueden establecer correctamente por idioma. En primer lugar, en tu archivo de recursos (.resw) de la aplicación, agrega un identificador de propiedad con el nombre "TitleText.Width" (en lugar de "TitleText" puedes usar el nombre que quieras). A continuación, usa un **x:Uid** para asociar uno o más elementos de la interfaz de usuario con este identificador de propiedad.

```xaml
<TextBlock x:Uid="TitleText">
```

Para obtener más información sobre archivos de recursos (.resw), identificadores de propiedades y **x:Uid**, consulta [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](../../app-resources/localize-strings-ui-manifest.md).

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

## <a name="handling-right-to-left-rtl-languages"></a>Control de idiomas de derecha a izquierda (RTL)
Cuando tu idioma está localizado para idiomas de derecha a izquierda (RTL), usa la propiedad [**FrameworkElement.FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) y fija márgenes y rellenos simétricos. Los paneles de diseño como [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live) se escalan y voltean automáticamente con el valor de **FlowDirection** que has fijado.

Establece **FlowDirection** en el panel de diseño raíz (o marco) de tu página o en la página en sí. Esto hace que todos los controles dentro hereden esa propiedad.

> [!IMPORTANT]
> Sin embargo, **FlowDirection** *no* se establece automáticamente según el idioma de visualización seleccionada por el usuario en la configuración de Windows; ni tampoco cambia dinámicamente en respuesta al cambio del idioma de visualización del usuario. Si el usuario cambia la configuración de Windows de inglés a árabe, por ejemplo, la propiedad **FlowDirection** *no* cambiará automáticamente de izquierda a derecha a de derecha a izquierda. Como desarrollador de aplicaciones, tienes que establecer **FlowDirection** de la forma apropiada para el idioma que se muestre actualmente.

La técnica programática es usar la propiedad `LayoutDirection` del idioma de visualización preferido del usuario para establecer la propiedad [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) (consulta el siguiente ejemplo de código). La mayoría de los controles incluidos en Windows ya usan **FlowDirection**. Si vas a implementar un control personalizado, este debería usar la propiedad **FlowDirection** para realizar los cambios de diseño adecuados para los idiomas RTL y LTR.

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

```cppwinrt
#include <winrt/Windows.ApplicationModel.Resources.Core.h>
#include <winrt/Windows.Globalization.h>
...

m_languageTag = Windows::Globalization::ApplicationLanguages::Languages().GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView().QualifierValues().Lookup(L"LayoutDirection");
if (flowDirectionSetting == L"LTR")
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::LeftToRight);
}
else
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::RightToLeft);
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

La técnica anterior convierte a **FlowDirection** en una función de la propiedad `LayoutDirection` del idioma de visualización preferido del usuario. Si por cualquier motivo no quieres esa lógica, puede exponer una propiedad FlowDirection en tu aplicación como un recurso que los localizadores pueden establecer correctamente para cada idioma al que traducen.

En primer lugar, en tu archivo de recursos (.resw) de la aplicación, agrega un identificador de propiedad con el nombre "MainPage.FlowDirection" (en lugar de "MainPage" puedes usar el nombre que quieras). A continuación, usa un **x:Uid** para asociar el elemento de **Página** principal (o su panel de diseño de raíz o marco) con este identificador de propiedad.

```xaml
<Page x:Uid="MainPage">
```

En lugar de una sola línea de código para todos los idiomas, depende del traductor "traducir" este recurso propiedad correctamente para cada idioma traducido; por lo tanto, ten en cuenta que hay esa oportunidad adicional para los errores humanos cuando usas esta técnica.

## <a name="important-apis"></a>API importantes
* [FrameworkElement.FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>Artículos relacionados
* [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](../../app-resources/localize-strings-ui-manifest.md)
* [Adaptar los recursos al idioma, la escala y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación](manage-language-and-region.md)