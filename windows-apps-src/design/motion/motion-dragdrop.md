---
Description: Use drag-and-drop animations when users move objects, such as moving an item within a list, or dropping an item on top of another.
title: Animaciones de arrastre en aplicaciones para UWP
ms.assetid: 6064755F-6E24-4901-A4FF-263F05F0DFD6
label: Motion--Drag and drop
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c12acd2148c8c85d69354a71c016d7a7230e590b
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8892117"
---
# <a name="drag-animations"></a>Animaciones de arrastre




Usa animaciones de arrastrar y colocar cuando los usuarios muevan objetos, por ejemplo, cuando muevan un elemento dentro de una lista o coloquen un elemento encima de otro.

> **API importantes**: [**clase DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243174)


## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer


**Animación de inicio de arrastre**

-   Usa la animación de inicio de arrastre cuando el usuario empieza a mover un objeto.
-   Incluye los objetos afectados en la animación solo si existen otros objetos que puedan verse afectados por la operación de arrastrar y colocar.
-   Usa la animación de fin de arrastre para completar todas las secuencias de animaciones que empiecen con la animación de inicio de arrastre. Esto revierte el cambio de tamaño en el objeto arrastrado causado por la animación de inicio de arrastre.

**Animación de fin de arrastre**

-   Usa la animación de fin de arrastre cuando el usuario coloca un elemento arrastrado.
-   Usa la animación de fin de arrastre en combinación con las animaciones de agregar y eliminar para las listas.
-   Incluye los objetos afectados en la animación de fin de arrastre si, y solo si, incluiste los mismos objetos afectados en la animación de inicio de arrastre.
-   No uses la animación de fin de arrastre si no has usado antes la animación de inicio de arrastre. Necesitas usar ambas para que todos los objetos vuelvan a sus tamaños originales después de completar la secuencia de arrastrar.

**Animación de entrada de arrastre entre**

-   Usa la animación de entrada de arrastre entre cuando el usuario arrastra el origen de arrastre hacia un área de colocación donde se pueda colocar entre otros dos objetos.
-   Elige una zona de destino para soltar razonable. Esta zona no debe ser demasiado pequeña ya que podría resultarle difícil al usuario colocar el origen de arrastre.
-   La dirección recomendada para mover los objetos afectados y mostrar el área de colocación es apartarlos entre sí. El hecho de que sea en vertical u horizontal depende de la orientación de los objetos afectados entre sí.
-   No uses la animación de entrada de arrastre entre si el origen de arrastre no se puede colocar en un área. La animación de entrada de arrastre entre le indica al usuario que el origen de arrastre se puede colocar entre los objetos afectados.

**Animación de salida de arrastre entre**

-   Usa la animación de salida de arrastre entre cuando el usuario arrastra un objeto alejándolo de un área donde lo podría haber colocado entre otros dos objetos.
-   No uses la animación de salida de arrastre entre si no has usado antes la animación de entrada de arrastre entre.


## <a name="related-articles"></a>Artículos relacionados

**Para desarrolladores**
* [Introducción a las animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animación de secuencias de arrastrar y colocar](https://msdn.microsoft.com/library/windows/apps/xaml/jj649427)
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Clase DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243174)
* [**Clase DropTargetItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243186)
* [**Clase DragOverThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243180)


 




