---
author: msatranjr
Description: "En el control de mapa se pueden mostrar mapas y vistas aéreas, indicaciones, resultados de búsqueda y el estado del tráfico."
title: Directrices para mapas
ms.assetid: 7B5B6BC9-D1EC-4978-8876-20B78EF44797
translationtype: Human Translation
ms.sourcegitcommit: 92285ce32548bd6035c105e35c2b152432f8575a
ms.openlocfilehash: e094e29220a0f8fcae6ca2af36ab86ecd22b520d

---

# Control de mapa


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En el control de mapa se pueden mostrar mapas de carreteras, vistas aéreas en 3D, indicaciones, resultados de la búsqueda y el estado del tráfico. En un mapa se pueden mostrar indicaciones, puntos de interés y la ubicación del usuario. También se pueden mostrar vistas aéreas en 3D, vistas Streetside, el estado del tráfico y del transporte público y negocios locales.

![ejemplo de un mapa, vista básica](./images/win10fa/controls-maps-basic.jpg)

## ¿Es este el control adecuado?


Usa un control de mapa si quieres un mapa dentro de la aplicación que permita a los usuarios ver información geográfica general o específica de la aplicación. Disponer de un control de mapa en la aplicación significa que los usuarios no tienen que salir de la misma para obtener esa información.

**Nota**  Si no te importa que los usuarios salgan de la aplicación para buscar información, considera la posibilidad de usar para ello la aplicación Mapas de Windows. Tu aplicación puede iniciar Mapas de Windows para que muestre mapas específicos, indicaciones y resultados de búsqueda. Para obtener más información, consulta [Iniciar la aplicación Mapas de Windows](https://msdn.microsoft.com/library/windows/apps/mt228341).

## Ejemplos


En este ejemplo se muestra un mapa con una vista Streetside:

![ejemplo de vista Streetside del control de mapa](./images/win10fa/controls-maps-streetside.jpg)

 

En este ejemplo se muestra un mapa con una vista aérea 3D:

![ejemplo de vista 3D del control de mapa](./images/win10fa/controls-maps-3dview.jpg)

 

En este ejemplo se muestra una aplicación con una vista aérea 3D y una vista Streetside:

![ejemplo de vista de mapa 3D con vista Streetside](./images/win10fa/controls-maps-3dstreetview.png)


## Recomendaciones


-   Usa suficiente espacio en pantalla (o toda ella) para mostrar el mapa, de modo que los usuarios no tengan que realizar demasiados movimientos panorámicos y zoom para ver información geográfica.

-   Si el mapa solo se usa para presentar una vista informativa estática, podría ser más apropiado usar uno menor. Si optas por un mapa estático menor, decide sus dimensiones según las necesidades: debe ser lo bastante pequeño para ahorrar espacio de pantalla y lo bastante grande para que sea legible.

-   Inserta los puntos de interés en la escena de mapa con [**elementos del mapa**](https://msdn.microsoft.com/library/windows/apps/dn637034); cualquier información adicional puede mostrarse como una interfaz de usuario transitoria superpuesta a la escena de mapa.

## Temas relacionados


* [Mostrar mapas con vistas 2D, 3D y Streetside](https://msdn.microsoft.com/library/windows/apps/mt219695)
* [Mostrar puntos de interés en un mapa](https://msdn.microsoft.com/library/windows/apps/mt219696)
* [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/)
* [Muestra de mapa de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [//Vídeo de compilación de 2015: Aprovechamiento de mapas y ubicación entre teléfonos, tabletas y equipos en tus aplicaciones de Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Iniciar la aplicación Mapas de Windows](https://msdn.microsoft.com/library/windows/apps/mt228341)
 

 







<!--HONumber=Aug16_HO3-->


