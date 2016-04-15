---
Description: Usa las animaciones de puntero para proporcionar comentarios visuales a los usuarios cuando presionen un elemento.
title: Animaciones para clic de puntero en aplicaciones para UWP
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
label: Motion--Pointer animations
template: detail.hbs
---

# Animaciones para clic de puntero

Usa las animaciones de puntero para proporcionar comentarios visuales a los usuarios cuando presionen un elemento. La animación de puntero abajo reduce e inclina ligeramente el elemento presionado y se reproduce cuando se pulsa un elemento por primera vez. La animación de puntero arriba, que restablece el elemento a su posición original, se reproduce cuando el usuario suelta el puntero.

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**Clase PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168)
-   [**Clase PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)



## Cosas que hacer y cosas que evitar


-   Cuando usas una animación de puntero arriba, se desencadena inmediatamente la animación cuando el usuario suelta el puntero. De esta manera, se informa inmediatamente al usuario de que la acción se reconoció, aunque la acción desencadenada al presionar (como, por ejemplo, la navegación a una página nueva) tarde más en responder.

## Artículos relacionados

**Para desarrolladores (XAML)**
* [Información general sobre animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animación de los clics del puntero](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Clase PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**Clase PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 






<!--HONumber=Mar16_HO3-->


