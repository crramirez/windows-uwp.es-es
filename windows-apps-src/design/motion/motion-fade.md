---
author: mijacobs
Description: Use fade animations to bring items into a view or to take items out of a view. The two common fade animations are fade-in and fade-out.
title: Animaciones de atenuación en aplicaciones para UWP
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d2a9745e35f19066b094b2be187620858166dbd5
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "7128646"
---
# <a name="fade-animations"></a>Animaciones de atenuación



Usa animaciones de atenuación para hacer aparecer o desaparecer elementos en la vista. Las dos animaciones de atenuación comunes son el fundido de entrada y de salida.

> **API importantes**: [**clase FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298), [**clase FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)


## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer


-   Cuando la aplicación realiza una transición entre elementos no relacionados o con mucho texto, usa el fundido de salida seguido de un fundido de entrada. Esto permite que el objeto saliente desaparezca completamente antes de que el objeto entrante esté visible.
-   Usa el fundido de entrada en los elementos entrantes o en los situados encima de los elementos salientes si su tamaño permanece constante y quieres que el usuario tenga la sensación de mirar al mismo elemento. Una vez completado el fundido, el elemento saliente se puede quitar. Esta opción solo es viable cuando el elemento saliente estará completamente cubierto por el elemento entrante.
-   Evita las animaciones de atenuación para agregar o eliminar elementos de una lista. En su lugar, usa las animaciones de lista creadas para ese propósito.
-   Evita las animaciones de atenuación para cambiar todo el contenido de una página. En su lugar, usa las animaciones de transición de página creadas para este propósito.
-   El fundido de salida es una forma sutil para quitar un elemento.
## <a name="related-articles"></a>Artículos relacionados

* [Introducción a las animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animación de atenuación](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Clase FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298)
* [**Clase FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)

 

 




