---
author: mijacobs
Description: "Usa animaciones de atenuación para hacer aparecer o desaparecer elementos en la vista. Las dos animaciones de atenuación comunes son el fundido de entrada y de salida."
title: "Animaciones de atenuación en aplicaciones para UWP"
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 028e3462a0bf34af0486864508b1ac049fdf60ed

---

# Animaciones de atenuación

Usa animaciones de atenuación para hacer aparecer o desaparecer elementos en la vista. Las dos animaciones de atenuación comunes son el fundido de entrada y de salida.

**API importantes**

-   [**Clase FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298)
-   [**Clase FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)


## Cosas que hacer y cosas que evitar


-   Cuando la aplicación realiza una transición entre elementos no relacionados o con mucho texto, usa el fundido de salida seguido de un fundido de entrada. Esto permite que el objeto saliente desaparezca completamente antes de que el objeto entrante esté visible.
-   Usa el fundido de entrada en los elementos entrantes o en los situados encima de los elementos salientes si su tamaño permanece constante y quieres que el usuario tenga la sensación de mirar al mismo elemento. Una vez completado el fundido, el elemento saliente se puede quitar. Esta opción solo es viable cuando el elemento saliente estará completamente cubierto por el elemento entrante.
-   Evita las animaciones de atenuación para agregar o eliminar elementos de una lista. En su lugar, usa las animaciones de lista creadas para ese propósito.
-   Evita las animaciones de atenuación para cambiar todo el contenido de una página. En su lugar, usa las animaciones de transición de página creadas para este propósito.
-   El fundido de salida es una forma sutil para quitar un elemento.
## Artículos relacionados

**Para desarrolladores (XAML)**
* [Información general sobre animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animación de atenuación](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Clase FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298)
* [**Clase FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)

 

 







<!--HONumber=Jun16_HO4-->


