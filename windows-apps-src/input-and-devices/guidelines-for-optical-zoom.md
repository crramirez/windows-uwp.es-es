---
Description: En este tema se describen los elementos de zoom y cambio de tamaño de Windows. y se ofrecen instrucciones de experiencia de usuario para que uses estos nuevos mecanismos de interacción en las aplicaciones.
title: Instrucciones de zoom óptico y cambio de tamaño
ms.assetid: 51a0007c-8a5d-4c44-ac9f-bbbf092b8a00
label: Zoom óptico y cambio de tamaño
template: detail.hbs
---

# Zoom óptico y cambio de tamaño


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)
-   [**Input (XAML)**](https://msdn.microsoft.com/library/windows/apps/br227994)

En este artículo se describen los elementos de zoom y cambio de tamaño de Windows. y se ofrecen instrucciones de experiencia de usuario para que uses estos nuevos mecanismos de interacción en las aplicaciones.



El zoom óptico permite a los usuarios ampliar la vista del contenido dentro de un área de contenido (se ejecuta sobre la propia área de contenido), mientras que el cambio de tamaño permite cambiar el tamaño relativo de uno o varios objetos sin cambiar la vista del área de contenido (se ejecuta sobre los objetos que están dentro del área de contenido).

Tanto la interacción de zoom óptico como la de cambio de tamaño se ejecutan con los gestos de reducir y ampliar (separar los dedos acerca el contenido y acercar los dedos lo aleja), manteniendo presionada la tecla Ctrl mientras se desplaza la rueda de desplazamiento del mouse o manteniendo presionada la tecla Ctrl (con la tecla Mayús, si no se dispone de teclado numérico) y presionando la tecla de signo más (+) o menos (-).

Los siguientes diagramas muestran las diferencias entre cambio de tamaño y zoom óptico.

**Zoom óptico**: el usuario selecciona un área y después acerca o aleja el área completa.

![juntar los dedos acerca el área de contenido, y separarlos la aleja.](images/areazoom.png)

**Cambio de tamaño**: el usuario selecciona un objeto dentro de un área y cambia el tamaño del objeto.

![juntar los dedos reduce el objeto, y alejarlos lo amplía.](images/objectresize.png)

**Nota**  
El zoom óptico no debe confundirse con el [zoom semántico](../controls-and-patterns/semantic-zoom.md). Aunque en las dos interacciones se usan los mismos gestos, el zoom semántico se refiere a la presentación y navegación de datos o contenido estructurados en una única vista (como la estructura de carpetas de un equipo, una biblioteca de documentos o un álbum de fotos).

 

## <span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>Qué hacer y qué no hacer


Sigue las directrices que se indican a continuación para las aplicaciones que admitan el cambio de tamaño o zoom óptico:

-   Si defines restricciones o límites de tamaño máximo y mínimo, usa información visual para indicar al usuario cuándo ha alcanzado o superado esos límites.
-   Usa puntos de acoplamiento para influir en el comportamiento de zoom y cambio de tamaño, proporcionando puntos lógicos en los cuales detener la manipulación y garantizar así que en la ventanilla se muestre un subconjunto específico de contenido. Proporciona puntos de acoplamiento para niveles de zoom comunes o vistas lógicas para que al usuario le resulte más fácil seleccionar esos niveles. Por ejemplo, las aplicaciones de fotos podrían proporcionar un punto de acoplamiento de cambio de tamaño en el 100% o, en el caso de las aplicaciones de mapas, los puntos de acoplamiento podrían resultar útiles en las vistas de ciudad, estado y país.

    Los puntos de acoplamiento permiten que el usuario alcance sus objetivos, aunque carezca de precisión. Si usas XAML, consulta las propiedades de los puntos de acoplamiento de [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527). En JavaScript y HTML, usa [**-ms-content-zoom-snap-points**](https://msdn.microsoft.com/library/hh771895).

    Existen dos tipos de puntos de acoplamiento:

    -   Proximidad: después de levantar el dedo, se selecciona un punto de acoplamiento si la inercia se detiene dentro del umbral de distancia del punto de acoplamiento. Los puntos de acoplamiento en proximidad aún permiten que el zoom y el cambio de tamaño finalicen entre los puntos de acoplamiento.
    -   Obligatorio: el punto de acoplamiento seleccionado es el que precede o sigue inmediatamente al último punto de acoplamiento que se cruzó antes de levantar el contacto (en función de la dirección y velocidad del gesto). Una manipulación debe finalizar en un punto de acoplamiento obligatorio.
-   Usa física de inercia. Incluye:
    -   Desaceleración: ocurre cuando el usuario deja de reducir o ampliar. Es similar a la acción de dejar de deslizarse en una superficie resbaladiza.
    -   Rebote: se produce un ligero efecto de rebote cuando se supera un límite o una restricción de cambio de tamaño.
-   Distribuye los controles de acuerdo con las [directrices para destinos](guidelines-for-targeting.md).
-   Proporciona controladores de ajuste de escala para cambiar el tamaño de manera restringida. Si no se especifican controladores, el cambio de tamaño predeterminado es el isométrico o proporcional.
-   No uses el zoom para navegar por la interfaz de usuario ni exponer otros controles en la aplicación; en su lugar, usa una región de movimiento panorámico. Para obtener más información sobre el movimiento panorámico, consulta las [instrucciones del movimiento panorámico](guidelines-for-panning.md).
-   No coloques objetos redimensionables dentro de un área de contenido redimensionable. Las excepciones son:
    -   Las aplicaciones de dibujo en las que pueden aparecer elementos redimensionables en un elemento canvas redimensionable.
    -   Las páginas web con un objeto incrustado, como por ejemplo un mapa.

    **Nota**  
    En todos los casos, el área de contenido cambia de tamaño a menos que todos los puntos táctiles se encuentren dentro del objeto redimensionable.

     

## <span id="related_topics"></span>Artículos relacionados


**Ejemplos**
* [Ejemplo de entrada básica](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Ejemplo de entrada de latencia baja](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Ejemplo de modo de interacción del usuario](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Ejemplo de elementos visuales de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)
**Ejemplos de archivo**
* [Entrada: muestra de eventos de entrada de usuario de XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: muestra de funcionalidades del dispositivo](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: muestra de prueba de acceso táctil](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Muestra de desplazamiento, movimiento panorámico y zoom XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: ejemplo de entrada de lápiz simplificada](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrada: muestra de gestos de Windows 8](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrada: muestra de manipulaciones y gestos (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Muestra de entrada táctil de DirectX](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 






<!--HONumber=Mar16_HO1-->


