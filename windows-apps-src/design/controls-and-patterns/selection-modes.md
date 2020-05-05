---
Description: El modo de selección permite a los usuarios seleccionar uno o varios elementos y actuar sobre ellos.
title: Modo de selección
label: Selection mode
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d7b781e6074468fbe73446e4057e36ff31266d05
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "72165102"
---
# <a name="selection-mode-overview"></a>Introducción al modo de selección

El modo de selección permite a los usuarios seleccionar uno o varios elementos y actuar sobre ellos. Se puede invocar a través de un menú contextual, mediante el uso de CTRL+clic o MAYÚS+clic en un elemento, o pasando sobre un elemento en una vista de galería. Cuando el modo de selección está activado, aparecen casillas junto a cada elemento de lista y pueden aparecer acciones en la parte superior o inferior de la pantalla.

## <a name="different-selection-modes"></a>Diferentes modos de selección
Existen tres modos de selección diferentes:

- Único: el usuario puede seleccionar un solo elemento a la vez.
- Múltiple: el usuario puede seleccionar varios elementos sin usar un modificador.
- Extendido: el usuario puede seleccionar varios elementos con un modificador, por ejemplo, manteniendo presionada la tecla MAYÚS.

## <a name="selection-mode-examples"></a>Ejemplos de modo de selección
### <a name="here-is-a-listview-with-single-selection-mode-enabled"></a>Esta es una vista de lista con el modo de selección única habilitado.
![Vista de lista con el modo de selección única](images/listview-selection-single.png)

El elemento Art Venere está seleccionado y, en este momento, se mueve el puntero sobre el elemento Mitsue Tollner.

### <a name="here-is-a-listview-with-multiple-selection-mode-enabled"></a>Esta es una vista de lista con el modo de selección múltiple habilitado:
![Vista de lista con el modo de selección múltiple](images/listview-selection-multiple.png)

Hay tres elementos seleccionados y se mueve el puntero sobre el elemento Donette Foller.

### <a name="here-is-a-gridview-with-single-selection-mode-enabled"></a>Esta es una vista de cuadrícula con el modo de selección única habilitado:
![Vista de cuadrícula con el modo de selección única](images/gridview-selection-single.png)

El elemento 7 está seleccionado y, en este momento, se pasa el puntero sobre el elemento 3.

### <a name="here-is-a-gridview-with-multiple-selection-mode-enabled"></a>Esta es una vista de cuadrícula con el modo de selección múltiple habilitado:
![Vista de cuadrícula con el modo de selección múltiple](images/gridview-selection-multiple.png)

Los elementos 2, 4 y 5 están seleccionados y, en este momento, se pasa el puntero sobre el elemento 7.

## <a name="behavior-and-interaction"></a>Comportamiento e interacción
Al tocar en cualquier parte de un elemento, este se selecciona. Tocar sobre la acción de la barra de comandos afecta a todos los elementos seleccionados. Si no se selecciona ningún elemento, las acciones de la barra de comandos deberían estar desactivadas, excepto "Seleccionar todo".

El modo de selección no tiene un modelo de cierre del elemento por cambio de foco; al presionar fuera del marco en el que el modo de selección está activo, no se cancelará el modo. De este modo, se evita la desactivación fortuita del modo. Al hacer clic en el botón Atrás se descarta el modo de selección múltiple.

Se muestra una confirmación visual al seleccionar una acción. Considera la posibilidad de mostrar un cuadro de diálogo de confirmación para determinadas acciones, especialmente las destructivas, como la eliminación.

El modo de selección se limita a la página en la que está activo y no puede afectar a los elementos fuera de esa página.

El punto de entrada para el modo de selección debe estar yuxtapuesto en relación con el contenido sobre el que incide.

Para obtener recomendaciones sobre la barra de comandos, consulta [Directrices para barras de comandos](app-bars.md).

Para obtener más información sobre los modos de selección y el control de los eventos de selección para determinados controles, visita la página de instrucciones de ese control.
