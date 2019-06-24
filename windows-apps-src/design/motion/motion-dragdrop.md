---
Description: Usa animaciones de arrastrar y colocar cuando los usuarios muevan objetos, por ejemplo, cuando muevan un elemento dentro de una lista o coloquen un elemento encima de otro.
title: Animaciones de arrastre en aplicaciones para UWP
ms.assetid: 6064755F-6E24-4901-A4FF-263F05F0DFD6
label: Motion--Drag and drop
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d4aa6b0b1a7d0e4d805e43ba308730ee483927aa
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320899"
---
# <a name="drag-animations"></a>Animaciones de arrastre




Usa animaciones de arrastrar y colocar cuando los usuarios muevan objetos, por ejemplo, cuando muevan un elemento dentro de una lista o coloquen un elemento encima de otro.

> **API importantes**: [**Clase DragItemThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.dragitemthemeanimation)


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar


**Animación de inicio de arrastre**

-   Usa la animación de inicio de arrastre cuando el usuario empieza a mover un objeto.
-   Incluye los objetos afectados en la animación solo si existen otros objetos que puedan verse afectados por la operación de arrastrar y colocar.
-   Usa la animación de fin de arrastre para completar todas las secuencias de animaciones que empiecen con la animación de inicio de arrastre. Esto revierte el cambio de tamaño en el objeto arrastrado causado por la animación de inicio de arrastre.

**Animación de arrastre final**

-   Usa la animación de fin de arrastre cuando el usuario coloca un elemento arrastrado.
-   Usa la animación de fin de arrastre en combinación con las animaciones de agregar y eliminar para las listas.
-   Incluye los objetos afectados en la animación de fin de arrastre si, y solo si, incluiste los mismos objetos afectados en la animación de inicio de arrastre.
-   No uses la animación de fin de arrastre si no has usado antes la animación de inicio de arrastre. Necesitas usar ambas para que todos los objetos vuelvan a sus tamaños originales después de completar la secuencia de arrastrar.

**Arrastre entre escriba la animación**

-   Usa la animación de entrada de arrastre entre cuando el usuario arrastra el origen de arrastre hacia un área de colocación donde se pueda colocar entre otros dos objetos.
-   Elige una zona de destino para soltar razonable. Esta zona no debe ser demasiado pequeña ya que podría resultarle difícil al usuario colocar el origen de arrastre.
-   La dirección recomendada para mover los objetos afectados y mostrar el área de colocación es apartarlos entre sí. El hecho de que sea en vertical u horizontal depende de la orientación de los objetos afectados entre sí.
-   No uses la animación de entrada de arrastre entre si el origen de arrastre no se puede colocar en un área. La animación de entrada de arrastre entre le indica al usuario que el origen de arrastre se puede colocar entre los objetos afectados.

**Arrastre entre la animación deje**

-   Usa la animación de salida de arrastre entre cuando el usuario arrastra un objeto alejándolo de un área donde lo podría haber colocado entre otros dos objetos.
-   No uses la animación de salida de arrastre entre si no has usado antes la animación de entrada de arrastre entre.


## <a name="related-articles"></a>Artículos relacionados

**Para desarrolladores**
* [Información general sobre animaciones](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animación de secuencias de arrastrar y colocar](https://docs.microsoft.com/previous-versions/windows/apps/jj649427(v=win.10))
* [Inicio rápido: Animar la interfaz de usuario mediante la biblioteca de animaciones](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**Clase DragItemThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.dragitemthemeanimation)
* [**Clase DropTargetItemThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.droptargetitemthemeanimation)
* [**Clase DragOverThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.dragoverthemeanimation)


 




