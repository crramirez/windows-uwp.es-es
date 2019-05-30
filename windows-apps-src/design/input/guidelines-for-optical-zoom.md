---
Description: En este tema se describen las características de zoom y cambio de tamaño de elementos en Windows. También se ofrecen instrucciones de experiencia de usuario para que uses estos nuevos mecanismos de interacción en tus aplicaciones.
title: Instrucciones de zoom óptico y cambio de tamaño
ms.assetid: 51a0007c-8a5d-4c44-ac9f-bbbf092b8a00
label: Optical zoom and resizing
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b63c9191489ecae54b17cb75b8aa1af32f09fcb8
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363600"
---
# <a name="optical-zoom-and-resizing"></a>Zoom óptico y cambio de tamaño



En este artículo se describen los elementos de zoom y cambio de tamaño de Windows. También se ofrecen instrucciones de experiencia de usuario para que uses estos nuevos mecanismos de interacción en las aplicaciones.

> **API importantes**: [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Input (XAML)** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

El zoom óptico permite a los usuarios ampliar la vista del contenido dentro de un área de contenido (se ejecuta sobre la propia área de contenido), mientras que el cambio de tamaño permite cambiar el tamaño relativo de uno o varios objetos sin cambiar la vista del área de contenido (se ejecuta sobre los objetos que están dentro del área de contenido).

Tanto la interacción de zoom óptico como la de cambio de tamaño se ejecutan con los gestos de reducir y ampliar (separar los dedos acerca el contenido y acercar los dedos lo aleja), manteniendo presionada la tecla Ctrl mientras se desplaza la rueda de desplazamiento del mouse o manteniendo presionada la tecla Ctrl (con la tecla Mayús, si no se dispone de teclado numérico) y presionando la tecla de signo más (+) o menos (-).

Los siguientes diagramas muestran las diferencias entre cambio de tamaño y zoom óptico.

**Zoom óptico**: El usuario selecciona un área y, a continuación, aumenta el tamaño de todo el área.

![juntar los dedos acerca el área de contenido, y separarlos la aleja.](images/areazoom.png)

**Cambiar el tamaño de**: El usuario selecciona un objeto dentro de un área y cambia el tamaño de ese objeto.

![juntar los dedos reduce el objeto, y alejarlos lo amplía.](images/objectresize.png)

**Tenga en cuenta**    zoom óptico no se deben confundir con [Zoom semántico](../controls-and-patterns/semantic-zoom.md). Aunque en las dos interacciones se usan los mismos gestos, el zoom semántico se refiere a la presentación y navegación de datos o contenido estructurados en una única vista (como la estructura de carpetas de un equipo, una biblioteca de documentos o un álbum de fotos).

 

## <a name="dos-and-donts"></a>Qué hacer y qué no hacer


Sigue las directrices que se indican a continuación para las aplicaciones que admitan el cambio de tamaño o zoom óptico:

-   Si defines restricciones o límites de tamaño máximo y mínimo, usa información visual para indicar al usuario cuándo ha alcanzado o superado esos límites.
-   Usa puntos de acoplamiento para influir en el comportamiento de zoom y cambio de tamaño, proporcionando puntos lógicos en los cuales detener la manipulación y garantizar así que en la ventanilla se muestre un subconjunto específico de contenido. Proporciona puntos de acoplamiento para niveles de zoom comunes o vistas lógicas para que al usuario le resulte más fácil seleccionar esos niveles. Por ejemplo, las aplicaciones de fotos podrían proporcionar un punto de acoplamiento de cambio de tamaño en el 100% o, en el caso de las aplicaciones de mapas, los puntos de acoplamiento podrían resultar útiles en las vistas de ciudad, estado y país.

    Los puntos de acoplamiento permiten que el usuario alcance sus objetivos, aunque carezca de precisión. Si usas XAML, consulta las propiedades de los puntos de acoplamiento de [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer). En JavaScript y HTML, usa [ **-ms-content-zoom-snap-points**](https://msdn.microsoft.com/library/hh771895).

    Existen dos tipos de puntos de acoplamiento:

    -   Proximidad: después de levantar el dedo, se selecciona un punto de acoplamiento si la inercia se detiene dentro del umbral de distancia del punto de acoplamiento. Los puntos de acoplamiento en proximidad aún permiten que el zoom y el cambio de tamaño finalicen entre los puntos de acoplamiento.
    -   Obligatorio: el punto de acoplamiento seleccionado es el que precede o sigue inmediatamente al último punto de acoplamiento que se cruzó antes de levantar el contacto (en función de la dirección y velocidad del gesto). Una manipulación debe finalizar en un punto de acoplamiento obligatorio.
-   Usa física de inercia. A continuación se enumeran algunas:
    -   Desaceleración: Se produce cuando el usuario deja de reducir o expandir. Es similar a la acción de dejar de deslizarse en una superficie resbaladiza.
    -   Rechazo: Un efecto de rebote back ligero se produce cuando una restricción de tamaño o se pasa el límite.
-   Distribuye los controles de acuerdo con las [directrices para destinos](guidelines-for-targeting.md).
-   Proporciona controladores de ajuste de escala para cambiar el tamaño de manera restringida. Si no se especifican controladores, el cambio de tamaño predeterminado es el isométrico o proporcional.
-   No uses el zoom para navegar por la interfaz de usuario ni exponer otros controles en la aplicación; en su lugar, usa una región de movimiento panorámico. Para obtener más información sobre el movimiento panorámico, consulta las [instrucciones del movimiento panorámico](guidelines-for-panning.md).
-   No coloques objetos redimensionables dentro de un área de contenido redimensionable. Las excepciones son:
    -   Las aplicaciones de dibujo en las que pueden aparecer elementos redimensionables en un elemento canvas redimensionable.
    -   Las páginas web con un objeto incrustado, como por ejemplo un mapa.

    **Tenga en cuenta**    en todos los casos, el área de contenido se cambia el tamaño a menos que todos los puntos táctiles que están dentro del objeto puede cambiar el tamaño.

     

## <a name="related-articles"></a>Artículos relacionados


**Ejemplos**
* [Ejemplo básico de entrada](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Ejemplo de entrada de baja latencia](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Ejemplo de modo de interacción del usuario](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Ejemplo de elementos visuales de foco](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Ejemplos de archivo**
* [Entrada: Ejemplo de eventos de entrada de usuario XAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: Ejemplo de las capacidades de dispositivo](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: Ejemplo de pruebas de posicionamiento táctil](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [Desplazamiento, panorámica y zoom de ejemplo XAML](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: Ejemplo de tinta simplificada](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrada: Ejemplo de gestos de Windows 8](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrada: Las manipulaciones y ejemplo de gestos (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [Ejemplo de entrada táctil de DirectX](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




