---
author: mijacobs
Description: "Las animaciones de transición de contenido permiten cambiar el contenido de un área de la pantalla sin cambiar ni el contenedor ni el fondo. El nuevo contenido hace un fundido de entrada. Si hay contenido existente para sustituir, el contenido realiza un fundido de salida."
title: "Directrices para animaciones de transición de contenido"
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
translationtype: Human Translation
ms.sourcegitcommit: d7236006f2c620a4ff0de4e0f413f32a2eaf5687
ms.openlocfilehash: 2b3e0196b573fb426c9cd71fc613819a2dd2d615

---

# Animaciones de transición de contenido





**API importantes**

-   [**Clase ContentThemeTransition (XAML)**](https://msdn.microsoft.com/library/windows/apps/br243104)
-   [**Función enterContent (HTML)**](https://msdn.microsoft.com/library/windows/apps/hh701582)

Las animaciones de transición de contenido permiten cambiar el contenido de un área de la pantalla sin cambiar ni el contenedor ni el fondo. El nuevo contenido hace un fundido de entrada. Si hay contenido existente para sustituir, el contenido realiza un fundido de salida.

## Cosas que hacer y cosas que evitar


-   Usa una animación de entrada cuando haya un conjunto de elementos nuevos para introducir en un contenedor vacío. Por ejemplo, después de la carga inicial de una aplicación, parte del contenido de la aplicación puede no estar disponible inmediatamente para su visualización. Cuando ese contenido esté listo para mostrarse, usa una animación de transiciones de contenido para hacer aparecer dicho contenido tardío.
-   Usa transiciones de contenido para sustituir un conjunto de contenidos por otro que ya resida en el mismo contenedor de una vista.
-   Cuando introduzcas nuevo contenido, desliza hacia arriba (de abajo a arriba) ese contenido dentro de la vista en contra del flujo general de la página o el orden de lectura.
-   Introduce nuevo contenido de un modo lógico, por ejemplo, introduce el contenido más importante al final.
-   Si tienes más de un contenedor cuyo contenido tiene que actualizarse, desencadena todas las animaciones de transición de forma simultánea sin retraso ni escalonamiento.
-   No uses animaciones de transiciones de contenido cuando toda la página cambia. En ese caso, usa las animaciones de transición de página.
-   No uses animaciones de transiciones de contenido si el contenido solamente se está actualizando. Las animaciones de transición de contenido deberían mostrar movimiento. Para actualizaciones, usa animaciones de atenuación.



## Artículos relacionados

**Para desarrolladores (XAML)**
* [Introducción a las animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animación de transiciones de contenido](https://msdn.microsoft.com/library/windows/apps/xaml/jj649426)
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Clase ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br243104)

 

 







<!--HONumber=Aug16_HO3-->


