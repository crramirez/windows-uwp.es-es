---
author: mijacobs
Description: Content transition animations let you change the content of an area of the screen while keeping the container or background constant. New content fades in. If there is existing content to be replaced, that content fades out.
title: Directrices para animaciones de transición de contenido
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 7dc34d15ed7f623b564101f4731beb72bb4bfbcc
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652599"
---
# <a name="content-transition-animations"></a>Animaciones de transición de contenido



Las animaciones de transición de contenido permiten cambiar el contenido de un área de la pantalla sin cambiar ni el contenedor ni el fondo. El nuevo contenido hace un fundido de entrada. Si hay contenido existente para sustituir, el contenido realiza un fundido de salida.

> **API importantes**: [**clase ContentThemeTransition (XAML)**](https://msdn.microsoft.com/library/windows/apps/br243104)

## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer


-   Usa una animación de entrada cuando haya un conjunto de elementos nuevos para introducir en un contenedor vacío. Por ejemplo, después de la carga inicial de una aplicación, parte del contenido de la aplicación puede no estar disponible inmediatamente para su visualización. Cuando ese contenido esté listo para mostrarse, usa una animación de transiciones de contenido para hacer aparecer dicho contenido tardío.
-   Usa transiciones de contenido para sustituir un conjunto de contenidos por otro que ya resida en el mismo contenedor de una vista.
-   Cuando introduzcas nuevo contenido, desliza hacia arriba (de abajo a arriba) ese contenido dentro de la vista en contra del flujo general de la página o el orden de lectura.
-   Introduce nuevo contenido de un modo lógico, por ejemplo, introduce el contenido más importante al final.
-   Si tienes más de un contenedor cuyo contenido tiene que actualizarse, desencadena todas las animaciones de transición de forma simultánea sin retraso ni escalonamiento.
-   No uses animaciones de transiciones de contenido cuando toda la página cambia. En ese caso, usa las animaciones de transición de página.
-   No uses animaciones de transiciones de contenido si el contenido solamente se está actualizando. Las animaciones de transición de contenido deberían mostrar movimiento. Para actualizaciones, usa animaciones de atenuación.



## <a name="related-articles"></a>Artículos relacionados

**Para desarrolladores (XAML)**
* [Introducción a las animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animación de transiciones de contenido](https://msdn.microsoft.com/library/windows/apps/xaml/jj649426)
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Clase ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br243104)

 

 



