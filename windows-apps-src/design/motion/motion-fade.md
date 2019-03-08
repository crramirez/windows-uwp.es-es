---
Description: Usa animaciones de atenuación para hacer aparecer o desaparecer elementos en la vista. Las dos animaciones de atenuación comunes son el fundido de entrada y de salida.
title: Animaciones de atenuación en aplicaciones para UWP
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6d3fee78f3608466f588a79d2811f1464e27a0ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620420"
---
# <a name="fade-animations"></a>Animaciones de atenuación



Usa animaciones de atenuación para hacer aparecer o desaparecer elementos en la vista. Las dos animaciones de atenuación comunes son el fundido de entrada y de salida.

> **API importantes**: [**Clase FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298), [ **FadeOutThemeAnimation clase**](https://msdn.microsoft.com/library/windows/apps/br210302)


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar


-   Cuando la aplicación realiza una transición entre elementos no relacionados o con mucho texto, usa el fundido de salida seguido de un fundido de entrada. Esto permite que el objeto saliente desaparezca completamente antes de que el objeto entrante esté visible.
-   Usa el fundido de entrada en los elementos entrantes o en los situados encima de los elementos salientes si su tamaño permanece constante y quieres que el usuario tenga la sensación de mirar al mismo elemento. Una vez completado el fundido, el elemento saliente se puede quitar. Esta opción solo es viable cuando el elemento saliente estará completamente cubierto por el elemento entrante.
-   Evita las animaciones de atenuación para agregar o eliminar elementos de una lista. En su lugar, usa las animaciones de lista creadas para ese propósito.
-   Evita las animaciones de atenuación para cambiar todo el contenido de una página. En su lugar, usa las animaciones de transición de página creadas para este propósito.
-   El fundido de salida es una forma sutil para quitar un elemento.
## <a name="related-articles"></a>Artículos relacionados

* [Información general sobre animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animación de fundido](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [Inicio rápido: Animar la interfaz de usuario mediante la biblioteca de animaciones](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Clase FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298)
* [**Clase FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)

 

 




