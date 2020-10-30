---
description: Las animaciones de listas te permiten insertar o quitar uno o varios elementos de una colección, como un álbum de fotos o una lista de resultados de búsqueda.
title: Agregar y eliminar animaciones
ms.assetid: A85006AE-4992-457a-B514-500B8BEF5DC8
label: Motion--add and delete animations
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cb01cbf3dab1ae8f1cdb055ad3fc04b94b1ff510
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034208"
---
# <a name="add-and-delete-animations"></a>Agregar y eliminar animaciones



Las animaciones de listas te permiten insertar o quitar uno o varios elementos de una colección, como un álbum de fotos o una lista de resultados de búsqueda.

> **API importantes** : [ **clase AddDeleteThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition)


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar


-   Usa animaciones de listas para agregar un solo elemento nuevo a un conjunto de elementos existente. Por ejemplo, úsalas cuando llega un nuevo correo electrónico o cuando se importa una fotografía nueva a un conjunto existente.
-   Usa animaciones de listas para agregar varios elementos a un conjunto al mismo tiempo. Por ejemplo, cuando importas un nuevo conjunto de fotografías a una colección existente. La adición o eliminación de varios elementos a la vez debe ocurrir al mismo tiempo, sin que transcurra tiempo entre la acción en los objetos individuales.
-   Usa las animaciones de listas de agregar y eliminar en pareja. Cuando uses una de estas animaciones, usa la otra para la acción opuesta.
-   Usa animaciones de listas en listas de elementos en las que quieras agregar o eliminar un elemento o grupo de elementos a la vez.
-   No uses las animaciones de listas para mostrar o quitar un contenedor. Estas animaciones son para miembros de una colección o conjunto que ya se está mostrando. Usa animaciones de elementos emergentes para mostrar u ocultar un contenedor transitorio en la parte superior de la superficie de la aplicación. Usa animaciones de transición de contenido para mostrar o cambiar un contenedor que forma parte de la superficie de la aplicación.
-   No uses animaciones de listas en un conjunto entero de elementos. Usa las animaciones de transición de contenido para agregar o quitar una colección entera de tu contenedor.



## <a name="related-articles"></a>Artículos relacionados

* [Información general sobre animaciones](./xaml-animation.md)
* [Animación de incorporaciones y eliminaciones de listas](/previous-versions/windows/apps/jj649430(v=win.10))
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](/previous-versions/windows/apps/hh452703(v=win.10))
* [**Clase AddDeleteThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition)

 

 
