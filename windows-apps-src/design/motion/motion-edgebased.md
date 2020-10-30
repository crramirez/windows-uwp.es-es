---
description: Las animaciones en el borde muestran u ocultan una interfaz de usuario que se origina en el borde de la pantalla.
title: Animaciones de UI en el borde
ms.assetid: 5A8F73B1-F4F6-424b-9EDF-A9766C5DEAE8
label: Motion--edge-based UI
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a49277a82a3bfffffb478496ab01d353c169463f
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034238"
---
# <a name="edge-based-ui-animations"></a>Animaciones de UI en el borde





Las animaciones en el borde muestran u ocultan una interfaz de usuario que se origina en el borde de la pantalla. Tanto el usuario como la aplicación pueden comenzar las acciones de mostrar u ocultar. La interfaz de usuario puede superponerse a la aplicación o formar parte de la superficie de la aplicación principal. Si la interfaz de usuario forma parte de la superficie de la aplicación, puede que sea necesario cambiar el tamaño del resto de la aplicación para que quepa.

> **API importantes** : [ **clase EdgeUIThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar


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
* [Información general sobre animaciones](./xaml-animation.md)
* [Animación de interfaz de usuario en el borde](/previous-versions/windows/apps/jj649428(v=win.10))
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](/previous-versions/windows/apps/hh452703(v=win.10))
* [**Clase EdgeUIThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)
* [**Clase PaneThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition)
* [Animación de atenuación](/previous-versions/windows/apps/jj649429(v=win.10))
* [Animación de reposiciones](/previous-versions/windows/apps/jj649434(v=win.10))

 

 
