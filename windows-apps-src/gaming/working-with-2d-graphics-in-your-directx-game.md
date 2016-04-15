---
title: Elementos gráficos 2D básicos para juegos DirectX
description: Se analiza el uso de efectos y elementos gráficos de mapas de bits 2D y cómo usarlos en un juego.
ms.assetid: ad69e680-d709-83d7-4a4c-7bbfe0766bc7
---

# Elementos gráficos 2D para juegos DirectX


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Se analiza el uso de efectos y elementos gráficos de mapas de bits 2D y cómo usarlos en un juego.

Los gráficos 2D son un subconjunto de gráficos 3D que se ocupan de los primitivos 2D o mapas de bits. De forma más general, no usan una coordenada Z del mismo modo en que lo haría un juego en 3D, dado que el juego suele limitarse al plano X-Y. A veces emplean técnicas de gráficos 3D para crear sus componentes visuales y, por lo general, son más sencillos de desarrollar. Si acabas de llegar al mundo de los juegos, un juego en 2D es el punto de partida ideal, y el desarrollo de gráficos 2D puede ser un buen entrenamiento para familiarizarte con DirectX.

Puedes desarrollar gráficos para juegos en 2D en DirectX usando Direct2D o Direct3D, o alguna combinación. Muchas de las clases más útiles para desarrollar juegos en 2D se encuentran en Direct3D, como la clase [**Sprite**](https://msdn.microsoft.com/library/windows/desktop/bb205601). Direct2D es un conjunto de API dirigidas principalmente a interfaces de usuario y aplicaciones que requieren soporte para dibujar primitivos (como formas de círculos, líneas y polígonos planos). Teniendo esto en cuenta, sigue proporcionando un conjunto potente y con buen rendimiento de clases y métodos para crear gráficos para juegos, especialmente al crear superposiciones, interfaces y pantallas de visualización frontal (HUD, del inglés heads-up display), o para crear diversos juegos en 2D, desde uno simple hasta uno con un nivel razonable de detalle. Sin embargo, el método más eficaz para crear juegos en 2D es usar elementos de ambas bibliotecas, y es la forma en que vamos a tratar el desarrollo de gráficos 2D en este tema.

## Breve explicación de conceptos


Antes de la aparición de los gráficos 3D modernos y el hardware que los hace posibles, los juegos eran principalmente en 2D, y muchas de las técnicas para gráficos implicaban mover bloques de memoria, normalmente matrices de datos de color que se traducían o transformaban en píxeles en la pantalla con una relación 1:1.

En DirectX, los gráficos 2D son parte de la canalización 3D. La variedad de resoluciones de pantalla y hardware gráfico disponibles es mucho mayor, y tu motor de gráficos 2D debe ser compatible con ellos sin que la fidelidad sufra cambios significativos.

A continuación presentamos varios conceptos básicos que deberías conocer al empezar a desarrollar gráficos 2D.

-   Coordenadas de píxeles y tramas. Un píxel es un único punto de una pantalla de trama. Tiene su propio par de coordenadas (x, y), que indica su ubicación en la pantalla. (El término "píxel" se suele usar indistintamente para los píxeles físicos que conforman la pantalla y los elementos de memoria direccionables que se usan para contener los valores de color y alfa de los píxeles antes de enviarlos a la pantalla). La API trata la trama como una cuadrícula rectangular de elementos de píxeles, a menudo con una correspondencia 1:1 con la cuadrícula de píxeles físicos de una pantalla. Los sistemas de coordenadas de trama empiezan a contar desde la esquina superior izquierda, con el píxel en la posición (0, 0) en la esquina superior izquierda de la cuadrícula.
-   Los gráficos de mapa de bits (en ocasiones denominados gráficos de trama) son elementos gráficos representados como una cuadrícula rectangular de valores de píxeles. Los sprites (matrices de píxeles calculadas con independencia de la trama) son un tipo de gráfico de mapa de bits, y normalmente se usan para los caracteres activos o los objetos animados que no forman parte del fondo de un juego. Las diferentes tramas de animación de un sprite se representan como colecciones de mapas de bits llamadas "hojas" o "lotes". Los fondos son objetos de mapa de bits de mayor tamaño con una resolución igual o mayor que la de la trama de pantalla, y a menudo actúan como telón de fondo para el campo de juego.
-   Los gráficos vectoriales son gráficos que usan primitivos geométricos, como puntos, líneas, círculos y polígonos, para definir objetos 2D. En vez de con matrices de píxeles, se representan con las ecuaciones matemáticas que los definen en un espacio 2D. No tienen por qué tener una correspondencia 1:1 con la cuadrícula de píxeles de la pantalla, y es necesario transformarlos del sistema de coordenadas donde los representas al sistema de coordinadas de trama de la pantalla.
-   Una traslación consiste en tomar un punto o un vértice y calcular su nueva ubicación en el mismo sistema de coordenadas.
-   Ajustar la escala es aumentar o reducir un objeto aplicando un factor de escala especificado. En una imagen de vectores, puedes reducir y aumentar los vértices de sus componentes; en un mapa de bits, puedes aumentar o disminuir los elementos de píxeles. En las imágenes de mapa de bits, se pierden datos de píxeles al reducir la imagen, y los píxeles individuales aumentan de tamaño cuando se aumenta la escala de la imagen. En este último caso, puedes usar operaciones de interpolación de colores de píxeles, como el filtro bilinear, para suavizar las diferencias bruscas de color entre los píxeles aumentados.
-   La rotación consiste en rotar un objeto sobre un eje o ejes especificados. En una imagen vectorial, los vértices de la geometría se multiplican por una matriz de rotación para obtener el vértice rotado; en una imagen de mapa de bits, se pueden emplear diferentes algoritmos, cada uno de ellos con resultados de mayor o menor grado de fidelidad. Al igual que para el ajuste de escala y la traslación, existen API específicas para las operaciones de rotación.
-   La transformación consiste en tomar un punto o vértice en un sistema de coordenadas y calcular su punto o vértice correspondiente en otro sistema de coordenadas. Esto incluye la traslación, el ajuste de escala y la rotación, así como otras operaciones de cálculo de coordenadas.
-   El recorte consiste en quitar partes de mapas de bits o de geometría que se encuentran fuera del área visible de la pantalla, o están ocultas por objetos con una mayor prioridad de vista.
-   El búfer de tramas es un área de la memoria (a menudo situada en la memoria del propio hardware gráfico) que contiene el mapa de tramas final que dibujarás en la pantalla. La cadena de intercambio es una colección de búferes, en la que dibujas en un búfer de reserva y, cuando la imagen está lista, la "intercambias" poniéndola en la parte frontal para mostrarla.

## Consideraciones de diseño


El desarrollo de gráficos 2D es una magnífica forma de acostumbrarse a desarrollar con Direct3D, y te permitirá dedicar más tiempo a otros aspectos críticos del desarrollo de un juego: audio, controles y mecánicas de juego.

Dibuja siempre en un búfer de reserva. Dibujar directamente en el búfer de tramas implica que tu imagen se mostrará cuando se reciba la señal para mostrar (normalmente, cada 1/60 de segundo), incluso si la operación de dibujo no ha finalizado.

Diseña tu motor gráfico para que sea compatible con una buena selección de resoluciones, desde 1024x600 a 1920x1080 (o más). Tus destinatarios te lo agradecerán si admites la resolución nativa de sus monitores LCD, especialmente con gráficos 2D.

Unas buenas ilustraciones serán tu activo más importante cuando se trate de efectos visuales. Aunque tus gráficos de mapa de bits pueden resultar menos atractivos que los efectos fotorrealistas en 3D que usan las últimas características de modelos de sombreador, una buena ilustración con una resolución alta puede transmitir el mismo o más estilo y personalidad, y penaliza mucho menos el rendimiento.

## Referencia


-   [Introducción a Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370987)
-   [Inicio rápido de Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd535473)
-   [Introducción a la interoperabilidad de Direct2D y Direct3D](https://msdn.microsoft.com/library/windows/desktop/dd370966)

> **Note**  
Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

 

 






<!--HONumber=Mar16_HO1-->


