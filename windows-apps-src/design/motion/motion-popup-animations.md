---
author: mijacobs
Description: Use pop-up animations to show and hide pop-up UI for flyouts or custom pop-up UI elements. Pop-up elements are containers that appear over the app's content and are dismissed if the user taps or clicks outside of the pop-up element.
title: Animaciones de elementos emergentes de interfaz de usuario en aplicaciones para UWP
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1a304df30986c904f19cc2401c9a1fb468514f6f
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5834976"
---
# <a name="pop-up-ui-animations"></a>Animaciones de interfaz de usuario emergente



Usa animaciones de elementos emergentes para mostrar y ocultar interfaces de usuarios emergentes para controles flotantes o elementos personalizados emergentes de interfaz de usuario. Los elementos emergentes son contenedores que aparecen sobre el contenido de la aplicación y se descartan si el usuario presiona o hace clic fuera del elemento emergente.

> **API importantes**: [**clase PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210383), [**clase PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh969172)


## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer


-   Usa animaciones de elementos emergentes para mostrar u ocultar elementos de interfaz de usuario emergentes personalizados que no son parte de la página de la aplicación. Los controles comunes proporcionados por Windows ya tienen esas animaciones integradas.
-   No uses las animaciones de elementos emergentes en informaciones sobre herramientas o cuadros de diálogo.
-   No uses animaciones de elementos emergentes para mostrar u ocultar la interfaz de usuario dentro del contenido principal de la aplicación; solo usa animaciones de elementos emergentes para mostrar u ocultar un contenedor emergente que muestra sobre el contenido principal de la aplicación.

## <a name="related-articles"></a>Artículos relacionados

* [Introducción a las animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animación de interfaz de usuario emergente](https://msdn.microsoft.com/library/windows/apps/xaml/jj649433)
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Clase PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210383)
* [**Clase PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210391)
* [**Clase PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh969172)

 

 




