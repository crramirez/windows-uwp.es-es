---
Description: Usa animaciones de atenuación para hacer aparecer o desaparecer elementos en la vista. Las dos animaciones de atenuación comunes son el fundido de entrada y de salida.
title: Animaciones de atenuación
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6b937ba4174d01096a09a98f7efd4e7e3dce9632
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970310"
---
# <a name="fade-animations"></a>Animaciones de atenuación



Usa animaciones de atenuación para hacer aparecer o desaparecer elementos en la vista. Las dos animaciones de atenuación comunes son el fundido de entrada y de salida.

> **API importantes**: [**clase FadeInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation), [**clase FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar


-   Cuando la aplicación realiza una transición entre elementos no relacionados o con mucho texto, usa el fundido de salida seguido de un fundido de entrada. Esto permite que el objeto saliente desaparezca completamente antes de que el objeto entrante esté visible.
-   Usa el fundido de entrada en los elementos entrantes o en los situados encima de los elementos salientes si su tamaño permanece constante y quieres que el usuario tenga la sensación de mirar al mismo elemento. Una vez completado el fundido, el elemento saliente se puede quitar. Esta opción solo es viable cuando el elemento saliente estará completamente cubierto por el elemento entrante.
-   Evita las animaciones de atenuación para agregar o eliminar elementos de una lista. En su lugar, usa las animaciones de lista creadas para ese propósito.
-   Evita las animaciones de atenuación para cambiar todo el contenido de una página. En su lugar, usa las animaciones de transición de página creadas para este propósito.
-   El fundido de salida es una forma sutil para quitar un elemento.
## <a name="related-articles"></a>Artículos relacionados

* [Introducción a las animaciones](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animación de atenuación](https://docs.microsoft.com/previous-versions/windows/apps/jj649429(v=win.10))
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**Clase FadeInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation)
* [**Clase FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)

 

 




