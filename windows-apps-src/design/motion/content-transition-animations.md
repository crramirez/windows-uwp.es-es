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
ms.openlocfilehash: 7703f2dfac2eed6df1f59b8e1cdfa6371e240eb4
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317099"
---
# <a name="content-transition-animations"></a>Animaciones de transición de contenido



Las animaciones de transición de contenido permiten cambiar el contenido de un área de la pantalla sin cambiar ni el contenedor ni el fondo. El nuevo contenido hace un fundido de entrada. Si hay contenido existente para sustituir, el contenido realiza un fundido de salida.

> **API importantes**: [**Clase ContentThemeTransition (XAML)** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.contentthemetransition)

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
* [Información general sobre animaciones](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animar las transiciones de contenido](https://docs.microsoft.com/previous-versions/windows/apps/jj649426(v=win.10))
* [Inicio rápido: Animar la interfaz de usuario mediante la biblioteca de animaciones](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**Clase ContentThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.contentthemetransition)

 

 




