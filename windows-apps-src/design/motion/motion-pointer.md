---
description: Usa las animaciones de puntero para proporcionar comentarios visuales a los usuarios cuando presionen un elemento.
title: Animaciones para clic de puntero
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1f2d6367ebd8e87cbb32f7a12a829dba7fa38ace
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034188"
---
# <a name="pointer-click-animations"></a>Animaciones para clic de puntero



Usa las animaciones de puntero para proporcionar comentarios visuales a los usuarios cuando presionen un elemento. La animación de puntero abajo reduce e inclina ligeramente el elemento presionado y se reproduce cuando se pulsa un elemento por primera vez. La animación de puntero arriba, que restablece el elemento a su posición original, se reproduce cuando el usuario suelta el puntero.


> **API importantes** : [**clase PointerUpThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation), [**clase PointerDownThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

-   Cuando usas una animación de puntero arriba, se desencadena inmediatamente la animación cuando el usuario suelta el puntero. De esta manera, se informa inmediatamente al usuario de que la acción se reconoció, aunque la acción desencadenada al presionar (como, por ejemplo, la navegación a una página nueva) tarde más en responder.

## <a name="related-articles"></a>Artículos relacionados

* [Información general sobre animaciones](./xaml-animation.md)
* [Animación de los clics del puntero](/previous-versions/windows/apps/jj649432(v=win.10))
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](/previous-versions/windows/apps/hh452703(v=win.10))
* [**Clase PointerUpThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)
* [**Clase PointerDownThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)

 

 
