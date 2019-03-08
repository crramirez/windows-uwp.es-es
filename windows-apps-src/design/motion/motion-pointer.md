---
Description: Usa las animaciones de puntero para proporcionar comentarios visuales a los usuarios cuando presionen un elemento.
title: Animaciones para clic de puntero en aplicaciones para UWP
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1abd71cda8358e3f7e2fe36091f9c42f05bcb00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663800"
---
# <a name="pointer-click-animations"></a>Animaciones para clic de puntero



Usa las animaciones de puntero para proporcionar comentarios visuales a los usuarios cuando presionen un elemento. La animación de puntero abajo reduce e inclina ligeramente el elemento presionado y se reproduce cuando se pulsa un elemento por primera vez. La animación de puntero arriba, que restablece el elemento a su posición original, se reproduce cuando el usuario suelta el puntero.


> **API importantes**: [**Clase PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168), [ **PointerDownThemeAnimation clase**](https://msdn.microsoft.com/library/windows/apps/hh969164)


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

-   Cuando usas una animación de puntero arriba, se desencadena inmediatamente la animación cuando el usuario suelta el puntero. De esta manera, se informa inmediatamente al usuario de que la acción se reconoció, aunque la acción desencadenada al presionar (como, por ejemplo, la navegación a una página nueva) tarde más en responder.

## <a name="related-articles"></a>Artículos relacionados

* [Información general sobre animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animación de puntero de clics](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [Inicio rápido: Animar la interfaz de usuario mediante la biblioteca de animaciones](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Clase PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**Clase PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 




