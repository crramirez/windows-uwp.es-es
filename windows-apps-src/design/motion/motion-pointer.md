---
Description: Use pointer animations to provide users with visual feedback when the user taps on an item.
title: Animaciones para clic de puntero en aplicaciones para UWP
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 08/9/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8cedab3f589f41b791a6c387ec776dc4c8ee23a6
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8468259"
---
# <a name="pointer-click-animations"></a>Animaciones para clic de puntero



Usa las animaciones de puntero para proporcionar comentarios visuales a los usuarios cuando presionen un elemento. La animación de puntero abajo reduce e inclina ligeramente el elemento presionado y se reproduce cuando se pulsa un elemento por primera vez. La animación de puntero arriba, que restablece el elemento a su posición original, se reproduce cuando el usuario suelta el puntero.


> **API importantes**: [**clase PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168), [**clase PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)


## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer

-   Cuando usas una animación de puntero arriba, se desencadena inmediatamente la animación cuando el usuario suelta el puntero. De esta manera, se informa inmediatamente al usuario de que la acción se reconoció, aunque la acción desencadenada al presionar (como, por ejemplo, la navegación a una página nueva) tarde más en responder.

## <a name="related-articles"></a>Artículos relacionados

* [Introducción a las animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animación de los clics del puntero](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Clase PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**Clase PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 




