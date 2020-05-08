---
Description: Las animaciones de listas te permiten insertar o quitar uno o varios elementos de una colección, como un álbum de fotos o una lista de resultados de búsqueda.
title: Agregar y eliminar animaciones
ms.assetid: A85006AE-4992-457a-B514-500B8BEF5DC8
label: Motion--add and delete animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4264a9a3a75c076fc033bb98dad45ff8dc99f2ef
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970290"
---
# <a name="add-and-delete-animations"></a>Agregar y eliminar animaciones



Las animaciones de listas te permiten insertar o quitar uno o varios elementos de una colección, como un álbum de fotos o una lista de resultados de búsqueda.

> **API importantes**: [ **clase AddDeleteThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition)


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar


-   Usa animaciones de listas para agregar un solo elemento nuevo a un conjunto de elementos existente. Por ejemplo, úsalas cuando llega un nuevo correo electrónico o cuando se importa una fotografía nueva a un conjunto existente.
-   Usa animaciones de listas para agregar varios elementos a un conjunto al mismo tiempo. Por ejemplo, cuando importas un nuevo conjunto de fotografías a una colección existente. La adición o eliminación de varios elementos a la vez debe ocurrir al mismo tiempo, sin que transcurra tiempo entre la acción en los objetos individuales.
-   Usa las animaciones de listas de agregar y eliminar en pareja. Cuando uses una de estas animaciones, usa la otra para la acción opuesta.
-   Usa animaciones de listas en listas de elementos en las que quieras agregar o eliminar un elemento o grupo de elementos a la vez.
-   No uses las animaciones de listas para mostrar o quitar un contenedor. Estas animaciones son para miembros de una colección o conjunto que ya se está mostrando. Usa animaciones de elementos emergentes para mostrar u ocultar un contenedor transitorio en la parte superior de la superficie de la aplicación. Usa animaciones de transición de contenido para mostrar o cambiar un contenedor que forma parte de la superficie de la aplicación.
-   No uses animaciones de listas en un conjunto entero de elementos. Usa las animaciones de transición de contenido para agregar o quitar una colección entera de tu contenedor.



## <a name="related-articles"></a>Artículos relacionados

* [Introducción a las animaciones](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animación de incorporaciones y eliminaciones de listas](https://docs.microsoft.com/previous-versions/windows/apps/jj649430(v=win.10))
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**Clase AddDeleteThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition)

 

 




