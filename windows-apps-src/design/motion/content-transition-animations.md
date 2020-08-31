---
Description: Las animaciones de transición de contenido permiten cambiar el contenido de un área de la pantalla sin cambiar ni el contenedor ni el fondo. El nuevo contenido hace un fundido de entrada. Si hay contenido existente para sustituir, el contenido realiza un fundido de salida.
title: Directrices para animaciones de transición de contenido
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8d498c3214effd64895c6a894802a9ccc82ff357
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169269"
---
# <a name="content-transition-animations"></a>Animaciones de transición de contenido



Las animaciones de transición de contenido permiten cambiar el contenido de un área de la pantalla sin cambiar ni el contenedor ni el fondo. El nuevo contenido hace un fundido de entrada. Si hay contenido existente para sustituir, el contenido realiza un fundido de salida.

> **API importantes**: [ **clase ContentThemeTransition (XAML)**](/uwp/api/windows.ui.xaml.media.animation.contentthemetransition)

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar


-   Usa una animación de entrada cuando haya un conjunto de elementos nuevos para introducir en un contenedor vacío. Por ejemplo, después de la carga inicial de una aplicación, parte del contenido de la aplicación puede no estar disponible inmediatamente para su visualización. Cuando ese contenido esté listo para mostrarse, usa una animación de transiciones de contenido para hacer aparecer dicho contenido tardío.
-   Usa transiciones de contenido para sustituir un conjunto de contenidos por otro que ya resida en el mismo contenedor de una vista.
-   Cuando introduzcas nuevo contenido, desliza hacia arriba (de abajo a arriba) ese contenido dentro de la vista en contra del flujo general de la página o el orden de lectura.
-   Introduce nuevo contenido de un modo lógico, por ejemplo, introduce el contenido más importante al final.
-   Si tienes más de un contenedor cuyo contenido tiene que actualizarse, desencadena todas las animaciones de transición de forma simultánea sin retraso ni escalonamiento.
-   No uses animaciones de transiciones de contenido cuando toda la página cambia. En ese caso, usa las animaciones de transición de página.
-   No uses animaciones de transiciones de contenido si el contenido solamente se está actualizando. Las animaciones de transición de contenido deberían mostrar movimiento. Para actualizaciones, usa animaciones de atenuación.



## <a name="related-articles"></a>Artículos relacionados

**Para desarrolladores (XAML)**
* [Introducción a las animaciones](./xaml-animation.md)
* [Animación de transiciones de contenido](/previous-versions/windows/apps/jj649426(v=win.10))
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](/previous-versions/windows/apps/hh452703(v=win.10))
* [**Clase ContentThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.contentthemetransition)

 

 