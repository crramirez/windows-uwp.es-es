---
Description: Las animaciones en el borde muestran u ocultan una interfaz de usuario que se origina en el borde de la pantalla.
title: Animaciones de UI en el borde en aplicaciones para UWP
ms.assetid: 5A8F73B1-F4F6-424b-9EDF-A9766C5DEAE8
label: Motion--edge-based UI
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fd7071092a66f46a81095a5cb6aff8b623a774a5
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366623"
---
# <a name="edge-based-ui-animations"></a>Animaciones de UI en el borde





Las animaciones en el borde muestran u ocultan una interfaz de usuario que se origina en el borde de la pantalla. Tanto el usuario como la aplicación pueden comenzar las acciones de mostrar u ocultar. La interfaz de usuario puede superponerse a la aplicación o formar parte de la superficie de la aplicación principal. Si la interfaz de usuario forma parte de la superficie de la aplicación, puede que sea necesario cambiar el tamaño del resto de la aplicación para que quepa.

> **API importantes**: [**Clase EdgeUIThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)


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
* [Información general sobre animaciones](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animar la interfaz de usuario basada en el perímetro](https://docs.microsoft.com/previous-versions/windows/apps/jj649428(v=win.10))
* [Inicio rápido: Animar la interfaz de usuario mediante la biblioteca de animaciones](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**Clase EdgeUIThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)
* [**Clase PaneThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition)
* [Animación de fundido](https://docs.microsoft.com/previous-versions/windows/apps/jj649429(v=win.10))
* [Vuelve a colocar la animación](https://docs.microsoft.com/previous-versions/windows/apps/jj649434(v=win.10))

 

 




