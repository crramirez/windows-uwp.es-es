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
ms.openlocfilehash: d79c369e14236b827bdc18aba6c74349528728b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635180"
---
# <a name="pop-up-ui-animations"></a>Animaciones de interfaz de usuario emergente



Usa animaciones de elementos emergentes para mostrar y ocultar interfaces de usuarios emergentes para controles flotantes o elementos personalizados emergentes de interfaz de usuario. Los elementos emergentes son contenedores que aparecen sobre el contenido de la aplicación y se descartan si el usuario presiona o hace clic fuera del elemento emergente.

> **API importantes**: [**Clase PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210383), [ **PopupThemeTransition clase**](https://msdn.microsoft.com/library/windows/apps/hh969172)


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar


-   Usa animaciones de elementos emergentes para mostrar u ocultar elementos de interfaz de usuario emergentes personalizados que no son parte de la página de la aplicación. Los controles comunes proporcionados por Windows ya tienen esas animaciones integradas.
-   No uses las animaciones de elementos emergentes en informaciones sobre herramientas o cuadros de diálogo.
-   No uses animaciones de elementos emergentes para mostrar u ocultar la interfaz de usuario dentro del contenido principal de la aplicación; solo usa animaciones de elementos emergentes para mostrar u ocultar un contenedor emergente que muestra sobre el contenido principal de la aplicación.

## <a name="related-articles"></a>Artículos relacionados

* [Información general sobre animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animar la interfaz de usuario emergente](https://msdn.microsoft.com/library/windows/apps/xaml/jj649433)
* [Inicio rápido: Animar la interfaz de usuario mediante la biblioteca de animaciones](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Clase PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210383)
* [**Clase PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210391)
* [**Clase PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh969172)

 

 




