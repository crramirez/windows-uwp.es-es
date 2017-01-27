---
author: mijacobs
Description: "Usa animaciones de elementos emergentes para mostrar y ocultar interfaces de usuarios emergentes para controles flotantes o elementos personalizados emergentes de interfaz de usuario. Los elementos emergentes son contenedores que aparecen sobre el contenido de la aplicación y se descartan si el usuario presiona o hace clic fuera del elemento emergente."
title: Animaciones de elementos emergentes de interfaz de usuario en aplicaciones para UWP
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: cb5d70784e758b4e18092b75df9e0d77243af7fc

---

# <a name="pop-up-ui-animations"></a>Animaciones de interfaz de usuario emergente

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Usa animaciones de elementos emergentes para mostrar y ocultar interfaces de usuarios emergentes para controles flotantes o elementos personalizados emergentes de interfaz de usuario. Los elementos emergentes son contenedores que aparecen sobre el contenido de la aplicación y se descartan si el usuario presiona o hace clic fuera del elemento emergente.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Clase PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210383)</li>
<li>[**Clase PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh969172)</li>
</ul>
</div>


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar


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

 

 







<!--HONumber=Dec16_HO2-->


