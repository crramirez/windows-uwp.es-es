---
description: Usa animaciones de elementos emergentes para mostrar y ocultar interfaces de usuarios emergentes para controles flotantes o elementos personalizados emergentes de interfaz de usuario. Los elementos emergentes son contenedores que aparecen sobre el contenido de la aplicación y se descartan si el usuario presiona o hace clic fuera del elemento emergente.
title: Animaciones de interfaz de usuario emergente
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5d09363d6ecd2eb5909c98da316c7c1395ebec03
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034128"
---
# <a name="pop-up-ui-animations"></a>Animaciones de interfaz de usuario emergente



Usa animaciones de elementos emergentes para mostrar y ocultar interfaces de usuarios emergentes para controles flotantes o elementos personalizados emergentes de interfaz de usuario. Los elementos emergentes son contenedores que aparecen sobre el contenido de la aplicación y se descartan si el usuario presiona o hace clic fuera del elemento emergente.

> **API importantes** : [**clase PopInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation), [**clase PopupThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar


-   Usa animaciones de elementos emergentes para mostrar u ocultar elementos de interfaz de usuario emergentes personalizados que no son parte de la página de la aplicación. Los controles comunes proporcionados por Windows ya tienen esas animaciones integradas.
-   No uses las animaciones de elementos emergentes en informaciones sobre herramientas o cuadros de diálogo.
-   No uses animaciones de elementos emergentes para mostrar u ocultar la interfaz de usuario dentro del contenido principal de la aplicación; solo usa animaciones de elementos emergentes para mostrar u ocultar un contenedor emergente que muestra sobre el contenido principal de la aplicación.

## <a name="related-articles"></a>Artículos relacionados

* [Información general sobre animaciones](./xaml-animation.md)
* [Animación de interfaz de usuario emergente](/previous-versions/windows/apps/jj649433(v=win.10))
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](/previous-versions/windows/apps/hh452703(v=win.10))
* [**Clase PopInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)
* [**Clase PopOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation)
* [**Clase PopupThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)

 

 
