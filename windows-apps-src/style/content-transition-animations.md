---
Description: Las animaciones de transición de contenido permiten cambiar el contenido de un área de la pantalla sin cambiar ni el contenedor ni el fondo. El nuevo contenido hace un fundido de entrada. Si hay contenido existente para sustituir, el contenido realiza un fundido de salida.
title: Directrices para animaciones de transición de contenido
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
---

# Animaciones de transición de contenido


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**Clase ContentThemeTransition (XAML)**](https://msdn.microsoft.com/library/windows/apps/br243104)
-   [**Función enterContent (HTML)**](https://msdn.microsoft.com/library/windows/apps/hh701582)

Las animaciones de transición de contenido permiten cambiar el contenido de un área de la pantalla sin cambiar ni el contenedor ni el fondo. El nuevo contenido hace un fundido de entrada. Si hay contenido existente para sustituir, el contenido realiza un fundido de salida.

## <span id="Recommendations"></span><span id="recommendations"></span><span id="RECOMMENDATIONS"></span>Lo que se debe y no se debe hacer


-   Usa una animación de entrada cuando haya un conjunto de elementos nuevos para introducir en un contenedor vacío. Por ejemplo, después de la carga inicial de una aplicación, parte del contenido de la aplicación puede no estar disponible inmediatamente para su visualización. Cuando ese contenido esté listo para mostrarse, usa una animación de transiciones de contenido para hacer aparecer dicho contenido tardío.
-   Usa transiciones de contenido para sustituir un conjunto de contenidos por otro que ya resida en el mismo contenedor de una vista.
-   Cuando introduzcas nuevo contenido, desliza hacia arriba (de abajo a arriba) ese contenido dentro de la vista en contra del flujo general de la página o el orden de lectura.
-   Introduce nuevo contenido de un modo lógico, por ejemplo, introduce el contenido más importante al final.
-   Si tienes más de un contenedor cuyo contenido tiene que actualizarse, desencadena todas las animaciones de transición de forma simultánea sin retraso ni escalonamiento.
-   No uses animaciones de transiciones de contenido cuando toda la página cambia. En ese caso, usa las animaciones de transición de página.
-   No uses animaciones de transiciones de contenido si el contenido solamente se está actualizando. Las animaciones de transición de contenido deberían mostrar movimiento. Para actualizaciones, usa animaciones de atenuación.

\[En este artículo se incluye información específica para aplicaciones de la Plataforma universal de Windows (UWP) y Windows 10. Para obtener instrucciones sobre Windows 8.1, descarga el [PDF sobre las directrices para Windows 8.1](https://go.microsoft.com/fwlink/p/?linkid=258743)\].

## <span id="related_topics"></span>Artículos relacionados

**Para desarrolladores (XAML)**
* [Información general sobre animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animación de transiciones de contenido](https://msdn.microsoft.com/library/windows/apps/xaml/jj649426)
* [Inicio rápido: animación de la interfaz de usuario con animaciones de la biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Clase ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br243104)

 

 






<!--HONumber=Mar16_HO3-->


