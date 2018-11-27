---
Description: Edge-based animations show or hide UI that originates from the edge of the screen.
title: Animaciones de UI en el borde en aplicaciones para UWP
ms.assetid: 5A8F73B1-F4F6-424b-9EDF-A9766C5DEAE8
label: Motion--edge-based UI
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e07ac565fe2e223b2fb33573ad083edfdfbc888a
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7708885"
---
# <a name="edge-based-ui-animations"></a>Animaciones de UI en el borde





Las animaciones en el borde muestran u ocultan una interfaz de usuario que se origina en el borde de la pantalla. Tanto el usuario como la aplicación pueden comenzar las acciones de mostrar u ocultar. La interfaz de usuario puede superponerse a la aplicación o formar parte de la superficie de la aplicación principal. Si la interfaz de usuario forma parte de la superficie de la aplicación, puede que sea necesario cambiar el tamaño del resto de la aplicación para que quepa.

> **API importantes**: [**clase EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh702324)


## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer


-   Usa animaciones de interfaz de usuario de borde para mostrar u ocultar un mensaje personalizado o barra de error que no se extiende por toda la pantalla.
-   Usa animaciones de panel para mostrar una interfaz de usuario que se deslizará una distancia considerable a lo largo de la pantalla, tales como un panel de tareas o un teclado de pantalla personalizado.
-   Desliza la interfaz de usuario hacia dentro desde el borde al que quedará adjunta.
-   Desliza la interfaz de usuario hacia fuera, hasta el borde del que procede.
-   Si hay que cambiar el tamaño del contenido de la aplicación en respuesta al deslizamiento de la interfaz de usuario, usa animaciones de atenuación para cambiar el tamaño.
    -   Si la interfaz de usuario se desliza hacia dentro, usa una animación de atenuación después de la interfaz de usuario de borde o animación de panel.
    -   Si la interfaz de usuario se desliza hacia afuera, usa una animación de atenuación al mismo tiempo que una interfaz de usuario de borde o un panel de animación.
-   No uses estas animaciones con notificaciones. Las notificaciones no se deberían albergar en una interfaz de usuario en el borde.
-   No apliques las animaciones de panel ni las de interfaz de usuario de borde a ningún control o contenedor de interfaz de usuario que no se encuentre en el borde de la pantalla. Estas animaciones solo se usan para mostrar, cambiar el tamaño y descartar una interfaz de usuario en los bordes de la pantalla. Para mover otros tipos de interfaz de usuario, usa la animación de reposición.

    ![ilustra cuándo usar interfaz de usuario de borde o animaciones de panel y cuándo usar reposición.](images/edgevsreposition.png)

## <a name="related-articles"></a>Artículos relacionados


**Para desarrolladores**
* [Introducción a las animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animación de interfaz de usuario en el borde](https://msdn.microsoft.com/library/windows/apps/xaml/jj649428)
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Clase EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh702324)
* [**Clase PaneThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh969160)
* [Animación de atenuación](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [Animación de reposiciones](https://msdn.microsoft.com/library/windows/apps/xaml/jj649434)

 

 




