---
author: Jwmsft
Description: "Un patrón con un área de contenido y un área de comando para aplicaciones de vista única o experiencias modales, como visores o editores de fotos, visores de documentos, mapas, dibujo u otras aplicaciones que usan una vista de desplazamiento libre."
title: "Patrón de diseño de lienzo activo"
ms.assetid: 4D768472-64D6-406C-9E87-F750F6B981A0
label: TBD
template: detail.hbs
op-migration-status: ready
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: b38d7664a8a874743c5307e44e81104ce512454e

---
# <a name="active-canvas-layout-pattern"></a>Patrón de diseño de lienzo activo

Un lienzo activo es un patrón con un área de contenido y un área de comandos. Está destinado para aplicaciones de vista única o experiencias modales, como visores o editores de fotos, visores de documentos, mapas, dibujo u otras aplicaciones que usan una vista de desplazamiento libre. Para realizar acciones, un lienzo activo puede combinarse con una barra de comandos o simplemente botones, según el número y los tipos de acciones que necesitas.

## <a name="examples"></a>Ejemplos

Este diseño de una aplicación de edición de fotos ofrece un patrón de lienzo activo, con un ejemplo de dispositivo móvil a la izquierda y un ejemplo de equipo de escritorio a la derecha. La superficie de edición de imágenes es un lienzo y la barra de comandos de la parte inferior contiene todas las acciones contextuales necesarias para la aplicación.

![Ejemplo de un editor de fotos con el patrón de lienzo activo](images/uap-photo-pc-phone-700.png)

Este diseño de una aplicación de mapa de metro hace uso de un lienzo activo con una franja de interfaz de usuario simple en la parte superior que solo tiene dos acciones y un cuadro de búsqueda. Las acciones contextuales se muestran en el menú contextual, como se aprecia en la imagen derecha.

![Ejemplo de una aplicación de mapas con el patrón de lienzo activo](images/uap-subway-pc-phone-700.png)


## <a name="implementing-this-pattern"></a>Implementación de este patrón

El patrón de lienzo activo consta de un área de contenido y un área de comando.

**Área de contenido.**  El área de contenido suele ser un lienzo de desplazamiento libre. Es posible que existan varias áreas de contenido dentro de una aplicación.

**Área de comandos.**  Si colocas muchos comandos, entonces la solución podría ser una barra de comandos que responda según el tamaño de la pantalla. Si no vas a colocar tantos comandos y no te preocupa la capacidad de respuesta de la interfaz de usuario, puedes usar botones para ahorrar espacio.



## <a name="related-articles"></a>Artículos relacionados

-   [**Barra de la aplicación y barra de comandos**](../controls-and-patterns/app-bars.md)



<!--HONumber=Dec16_HO2-->


