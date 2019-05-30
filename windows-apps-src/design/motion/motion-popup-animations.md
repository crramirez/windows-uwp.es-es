---
Description: Usa animaciones de elementos emergentes para mostrar y ocultar interfaces de usuarios emergentes para controles flotantes o elementos personalizados emergentes de interfaz de usuario. Los elementos emergentes son contenedores que aparecen sobre el contenido de la aplicación y se descartan si el usuario presiona o hace clic fuera del elemento emergente.
title: Animaciones de elementos emergentes de interfaz de usuario en aplicaciones para UWP
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ee3d6a7fc29ec2adfeb149a3bc84f27c482c3be7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366699"
---
# <a name="pop-up-ui-animations"></a>Animaciones de interfaz de usuario emergente



Usa animaciones de elementos emergentes para mostrar y ocultar interfaces de usuarios emergentes para controles flotantes o elementos personalizados emergentes de interfaz de usuario. Los elementos emergentes son contenedores que aparecen sobre el contenido de la aplicación y se descartan si el usuario presiona o hace clic fuera del elemento emergente.

> **API importantes**: [**Clase PopInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation), [ **PopupThemeTransition clase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar


-   Usa animaciones de elementos emergentes para mostrar u ocultar elementos de interfaz de usuario emergentes personalizados que no son parte de la página de la aplicación. Los controles comunes proporcionados por Windows ya tienen esas animaciones integradas.
-   No uses las animaciones de elementos emergentes en informaciones sobre herramientas o cuadros de diálogo.
-   No uses animaciones de elementos emergentes para mostrar u ocultar la interfaz de usuario dentro del contenido principal de la aplicación; solo usa animaciones de elementos emergentes para mostrar u ocultar un contenedor emergente que muestra sobre el contenido principal de la aplicación.

## <a name="related-articles"></a>Artículos relacionados

* [Información general sobre animaciones](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animar la interfaz de usuario emergente](https://docs.microsoft.com/previous-versions/windows/apps/jj649433(v=win.10))
* [Inicio rápido: Animar la interfaz de usuario mediante la biblioteca de animaciones](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**Clase PopInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)
* [**Clase PopOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation)
* [**Clase PopupThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)

 

 




