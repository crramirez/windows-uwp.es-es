---
description: Diseñe la aplicación para admitir los diseños y fuentes de varios idiomas, incluida la dirección de flujo RTL (derecha a izquierda).
title: Ajustar el diseño y las fuentes y admitir la escritura RTL
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.date: 05/11/2018
ms.topic: article
keywords: Windows 10, UWP, localizabilidad, localización, RTL, LTR
ms.localizationpriority: medium
ms.openlocfilehash: 0e4725f6d26cf1abf42effddd813c31e87926e89
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033798"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>Ajustar el diseño y las fuentes y admitir la escritura RTL
Diseñe la aplicación para admitir los diseños y fuentes de varios idiomas, incluida la dirección de flujo RTL (derecha a izquierda). La dirección del flujo es la dirección en la que se escribe y se muestra el script, y los elementos de la interfaz de usuario de la página se examinan en el ojo.

## <a name="layout-guidelines"></a>Directrices de diseño
Los idiomas como el alemán y el Finés suelen usar más caracteres que el inglés. Las fuentes del este de extremo suelen requerir más alto. Los lenguajes de y, como el árabe y el hebreo, requieren que los paneles de diseño y los elementos de texto se colocan en orden de lectura de derecha a izquierda (RTL).

Debido a estas variaciones en las métricas de texto traducido, se recomienda no integrar posicionamiento absoluto, ancho fijo o alto fijo en la interfaz de usuario (UI). En su lugar, aproveche los mecanismos de diseño dinámico integrados en los elementos de la interfaz de usuario de Windows. Por ejemplo, los controles de contenido (como los botones), los controles de elementos (como las vistas de cuadrícula y las vistas de lista) y los paneles de diseño (como cuadrículas y stackpanels) automáticamente se ajustan y retransmiten de forma predeterminada para ajustarse a su contenido. Localizar la aplicación para descubrir los casos extremos problemáticos en los que los elementos de la interfaz de usuario no se ajustan al contenido correctamente.

El diseño dinámico es la técnica recomendada y podrá usarla en la mayoría de los casos. Menos preferible, pero aún mejor que los tamaños de cocción en el marcado de la interfaz de usuario, es establecer los valores de diseño como una función del lenguaje. A continuación se muestra un ejemplo de cómo puede exponer una propiedad de ancho en la aplicación como un recurso que los localizadores pueden establecer adecuadamente por idioma. En primer lugar, en el archivo de recursos de la aplicación (. resw), agregue un identificador de propiedad con el nombre "TitleText. width" (en lugar de "TitleText", puede usar cualquier nombre que desee). A continuación, utilice **x:UID** para asociar uno o más elementos de la interfaz de usuario con este identificador de propiedad.

```xaml
<TextBlock x:Uid="TitleText">
```

Para obtener más información acerca de los archivos de recursos (. resw), los identificadores de propiedad y **x:UID** , consulte [localizar cadenas en la interfaz de usuario y el manifiesto del paquete de la aplicación](../../app-resources/localize-strings-ui-manifest.md).

## <a name="fonts"></a>Fuentes
Use la clase de asignación de fuentes [**LanguageFont**](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live) para acceder mediante programación a la familia de fuentes, el tamaño, el peso y el estilo recomendados para un idioma determinado. La clase **LanguageFont** proporciona acceso a la información de fuente correcta para varias categorías de contenido, incluidos los encabezados de la interfaz de usuario, las notificaciones, el cuerpo del texto y las fuentes del cuerpo del documento editables por el usuario.

## <a name="mirroring-images"></a>Creación de imágenes reflejadas
Si la aplicación tiene imágenes que deben reflejarse (es decir, se puede voltear la misma imagen) para RTL, puede usar **FlowDirection** .

```xaml
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

Si su aplicación requiere una imagen diferente para voltear la imagen correctamente, puede usar el sistema de administración de recursos con el `LayoutDirection` calificador (consulte la sección LayoutDirection de [adaptar los recursos para el idioma, la escala y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md#layoutdirection)). El sistema elige una imagen denominada `file.layoutdir-rtl.png` cuando el idioma de tiempo de ejecución de la aplicación (consulte [comprender los idiomas de Perfil de usuario y los idiomas del manifiesto de aplicación](manage-language-and-region.md)) se establece en un idioma RTL. Este enfoque puede ser necesario cuando alguna parte de la imagen se voltea, pero otra no.

## <a name="handling-right-to-left-rtl-languages"></a>Tratamiento de idiomas que se leen de derecha a izquierda (RTL)
Cuando la aplicación esté localizada para idiomas de derecha a izquierda (RTL), use la propiedad [**FrameworkElement. FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) y establezca el relleno y los márgenes simétricos. Los paneles de diseño, como la escala de [**cuadrícula**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live) y el volteo automáticamente con el valor de **FlowDirection** que establezca.

Establezca **FlowDirection** en el panel de diseño raíz (o marco) de la página o en la propia página. Esto hace que todos los controles contenidos en hereden esa propiedad.

> [!IMPORTANT]
> Sin embargo **FlowDirection** , FlowDirection *no* se establece automáticamente en función del idioma de visualización seleccionado del usuario en la configuración de Windows. tampoco cambia dinámicamente en respuesta al idioma de visualización de cambio de usuario. Si el usuario cambia la configuración de Windows de inglés a árabe, por ejemplo, **FlowDirection** la propiedad FlowDirection *no* cambiará automáticamente de izquierda a derecha a derecha a izquierda. Como desarrollador de la aplicación, debe establecer la configuración de **FlowDirection** correctamente para el idioma que está mostrando actualmente.

La técnica de programación consiste en usar la `LayoutDirection` propiedad del idioma de visualización del usuario preferido para establecer la propiedad [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) (vea el ejemplo de código siguiente). La mayoría de los controles incluidos en Windows ya usan **FlowDirection** . Si está implementando un control personalizado, debe usar **FlowDirection** para realizar los cambios de diseño adecuados para los idiomas RTL y LTR.

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

La técnica anterior convierte a **FlowDirection** en una función de la `LayoutDirection` propiedad del idioma de visualización del usuario preferido. Si, por cualquier motivo, no desea esa lógica, puede exponer una propiedad FlowDirection en la aplicación como un recurso que los localizadores pueden establecer adecuadamente para cada idioma en el que se traducen.

En primer lugar, en el archivo de recursos de la aplicación (. resw), agregue un identificador de propiedad con el nombre "MainPage. FlowDirection" (en lugar de "MainPage", puede usar cualquier nombre que desee). A continuación, utilice **x:UID** para asociar el elemento de **Página** principal (o su marco o panel de diseño raíz) con este identificador de propiedad.

```xaml
<Page x:Uid="MainPage">
```

En lugar de una sola línea de código para todos los lenguajes, esto depende del traductor que "traduce" este recurso de propiedad correctamente para cada lenguaje traducido. por lo tanto, tenga en cuenta que hay una oportunidad adicional para el error humano cuando se usa esta técnica.

## <a name="important-apis"></a>API importantes
* [FrameworkElement. FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>Temas relacionados
* [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](../../app-resources/localize-strings-ui-manifest.md)
* [Adapte los recursos para el idioma, la escala y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación](manage-language-and-region.md)
