---
Description: Usa las animaciones de puntero para proporcionar comentarios visuales a los usuarios cuando presionen un elemento.
title: Animaciones para clic de puntero en aplicaciones para UWP
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ee87a62796ed51d09d44cabecd0b49873145bd90
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366085"
---
# <a name="pointer-click-animations"></a>Animaciones para clic de puntero



Usa las animaciones de puntero para proporcionar comentarios visuales a los usuarios cuando presionen un elemento. La animación de puntero abajo reduce e inclina ligeramente el elemento presionado y se reproduce cuando se pulsa un elemento por primera vez. La animación de puntero arriba, que restablece el elemento a su posición original, se reproduce cuando el usuario suelta el puntero.


> **API importantes**: [**Clase PointerUpThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation), [ **PointerDownThemeAnimation clase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

-   Cuando usas una animación de puntero arriba, se desencadena inmediatamente la animación cuando el usuario suelta el puntero. De esta manera, se informa inmediatamente al usuario de que la acción se reconoció, aunque la acción desencadenada al presionar (como, por ejemplo, la navegación a una página nueva) tarde más en responder.

## <a name="related-articles"></a>Artículos relacionados

* [Información general sobre animaciones](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animación de puntero de clics](https://docs.microsoft.com/previous-versions/windows/apps/jj649432(v=win.10))
* [Inicio rápido: Animar la interfaz de usuario mediante la biblioteca de animaciones](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**Clase PointerUpThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)
* [**Clase PointerDownThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)

 

 




