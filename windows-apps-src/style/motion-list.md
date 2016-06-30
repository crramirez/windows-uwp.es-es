---
author: mijacobs
Description: "Las animaciones de listas te permiten insertar o quitar uno o varios elementos de una colección, como un álbum de fotos o una lista de resultados de búsqueda."
title: Agregar y eliminar animaciones en aplicaciones para UWP
ms.assetid: A85006AE-4992-457a-B514-500B8BEF5DC8
label: Motion--add and delete animations
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: a795cf52c3d062d2131832ac3bc765f45097e849

---

# Agregar y eliminar animaciones




Las animaciones de listas te permiten insertar o quitar uno o varios elementos de una colección, como un álbum de fotos o una lista de resultados de búsqueda.

**API importantes**

-   [**Clase AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br243048)


## Cosas que hacer y cosas que evitar


-   Usa animaciones de listas para agregar un solo elemento nuevo a un conjunto de elementos existente. Por ejemplo, úsalas cuando llega un nuevo correo electrónico o cuando se importa una fotografía nueva a un conjunto existente.
-   Usa animaciones de listas para agregar varios elementos a un conjunto al mismo tiempo. Por ejemplo, cuando importas un nuevo conjunto de fotografías a una colección existente. La adición o eliminación de varios elementos a la vez debe ocurrir al mismo tiempo, sin que transcurra tiempo entre la acción en los objetos individuales.
-   Usa las animaciones de listas de agregar y eliminar en pareja. Cuando uses una de estas animaciones, usa la otra para la acción opuesta.
-   Usa animaciones de listas en listas de elementos en las que quieras agregar o eliminar un elemento o grupo de elementos a la vez.
-   No uses las animaciones de listas para mostrar o quitar un contenedor. Estas animaciones son para miembros de una colección o conjunto que ya se está mostrando. Usa animaciones de elementos emergentes para mostrar u ocultar un contenedor transitorio en la parte superior de la superficie de la aplicación. Usa animaciones de transición de contenido para mostrar o cambiar un contenedor que forma parte de la superficie de la aplicación.
-   No uses animaciones de listas en un conjunto entero de elementos. Usa las animaciones de transición de contenido para agregar o quitar una colección entera de tu contenedor.



## Artículos relacionados


**Para desarrolladores (XAML)**
* [Información general sobre animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animación de incorporaciones y eliminaciones de listas](https://msdn.microsoft.com/library/windows/apps/xaml/jj649430)
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Clase AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br243048)

 

 







<!--HONumber=Jun16_HO4-->


