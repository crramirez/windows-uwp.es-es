---
author: mijacobs
Description: "Un movimiento intencionado y bien diseñado da vida a las aplicaciones y ofrece una experiencia que se percibe elaborada y elegante. Ayuda a los usuarios a comprender los cambios contextuales y relaciona las experiencias con transiciones visuales."
title: "Movimiento y animación en las aplicaciones para UWP"
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.openlocfilehash: d24edf5eaacb65ca28f2f2727f441cb833141dcf
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2017
---
# <a name="motion-for-uwp-apps"></a>Movimiento para las aplicaciones para UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Un movimiento intencionado y bien diseñado da vida a las aplicaciones y ofrece una experiencia que se percibe elaborada y elegante. El movimiento ayuda a los usuarios a comprender cambios contextuales y dónde se encuentran dentro de la jerarquía de navegación de la aplicación. Relaciona experiencias con transiciones visuales. El movimiento aporta una sensación de ritmo y dimensionalidad a la experiencia.

## <a name="benefits-of-motion"></a>Ventajas del movimiento

El movimiento es algo más que hacer que las cosas se muevan. El movimiento es una herramienta que permite crear un ecosistema físico donde el usuario vive y manipula mediante una variedad de tipos de entrada, como el mouse, el teclado, la entrada táctil y el lápiz. La calidad de la experiencia depende de cómo responda la aplicación al usuario y de qué tipo de personalidad transmita la interfaz de usuario.

Asegúrate de que el movimiento sirva para un objetivo de la aplicación. Las mejores aplicaciones para la Plataforma universal de Windows(UWP) usan el movimiento para dar vida a la interfaz de usuario. El movimiento debe:

- Dar información en función del comportamiento del usuario.
- Enseñar al usuario a interactuar con la interfaz.
- Indicar cómo desplazarse a vistas anteriores o posteriores.

Es cada vez más importante que, a medida que los usuarios pasan más tiempo en tu aplicación o que las tareas de la aplicación son más complejas, el movimiento sea de gran calidad: puede usarse para cambiar la manera en que el usuario percibe la carga cognitiva y la facilidad de uso de la aplicación. El movimiento tiene, además, muchas ventajas directas más:

- **El movimiento admite la interacción y la forma de orientación.**

    El movimiento es direccional: avanza y retrocede, entra y sale del contenido, dejando pistas mentales para indicar la ruta de navegación que el usuario tomó para llegar a la vista actual. Las transiciones pueden ayudar a los usuarios a aprender a manejar aplicaciones nuevas estableciendo analogías con tareas con las que el usuario ya está familiarizado.

- **El movimiento puede dar la impresión de un mejor rendimiento.**

    Cuando la velocidad de red es lenta o el sistema se detiene para trabajar, las animaciones pueden hacer que la espera parezca inferior. Se pueden usar animaciones para que el usuario sepa que la aplicación está procesando y no congelado, y también pueden mostrar de forma pasiva información nueva que puede interesar al usuario.

- **El movimiento aporta personalidad.**

    A menudo, el movimiento es el subproceso común que comunica la personalidad de la aplicación a medida que un usuario se desplaza por una experiencia.

- **El movimiento aporta elegancia.**

    Un movimiento fluido y dinámico hace que las experiencias se sientan naturales, ya que se crean conexiones emocionales con la experiencia.

## <a name="examples-of-motion"></a>Ejemplos de movimiento

Estos son algunos ejemplos de movimiento en una aplicación.

Aquí, una aplicación usa una animación conectada para animar la imagen de un elemento en la medida en que "continúa" para formar parte del encabezado de la página siguiente. El efecto te ayuda a mantener al usuario en contexto a través de la transición.

![Animación conectada](images/connected-animations/example.gif)

Aquí, un efecto visual de paralaje mueve distintos objetos a diferentes velocidades cuando la interfaz de usuario se desplaza o realiza un movimiento panorámico para crear una sensación de profundidad, perspectiva y movimiento.

![Ejemplo de efecto de paralaje con la lista y una imagen en el fondo](images/_Parallax_v2.gif)


## <a name="types-of-motion"></a>Tipos de movimiento

<table>
    <tr>
        <th align="left">Tipo de movimiento</th>
        <th align="left">Descripción</th>
    </tr>
    <tr>
        <td>[Agregar y eliminar](motion-list.md)
        </td>
        <td>Las animaciones de listas te permiten insertar o quitar uno o varios elementos de una colección, como un álbum de fotos o una lista de resultados de búsqueda.
        </td>
    </tr>
    <tr>
        <td>[Animación conectada](connected-animation.md)
        </td>
        <td>Las animaciones conectadas te permiten crear una experiencia de navegación dinámica y atractiva al animar la transición de un elemento entre dos vistas distintas. Esto ayuda al usuario a mantener el contexto y ofrece continuidad entre las vistas. En una animación conectada, un elemento parece "continuar" entre dos vistas durante un cambio en el contenido de la interfaz de usuario, volando por la pantalla desde su ubicación en la vista de origen hasta su destino en la nueva vista. Esto enfatiza el contenido común entre las vistas y crea un efecto atractivo y dinámico como parte de una transición. 
        </td>
    </tr>
    <tr>
        <td>[Transición de contenido](content-transition-animations.md)
        </td>
        <td>Las animaciones de transición de contenido permiten cambiar el contenido de un área de la pantalla sin cambiar ni el contenedor ni el fondo. El nuevo contenido hace un fundido de entrada. Si hay contenido existente para sustituir, el contenido realiza un fundido de salida.
        </td>
    </tr>
    <tr>
        <td>[Drill](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinthemeanimation)
        </td>
        <td>Usa una animación de ampliación cuando un usuario avanza en una jerarquía lógica, como de una página maestra a una página de detalles. Usa una animación de reducción cuando un usuario retroceda en una jerarquía lógica, como de una página de detalles a una página maestra.
        </td>
    </tr>
    <tr>
        <td>[Atenuación](motion-fade.md)
        </td>
        <td>Usa animaciones de atenuación para hacer aparecer o desaparecer elementos en la vista. Las dos animaciones de atenuación comunes son el fundido de entrada y de salida.
        </td>
    </tr>
        <tr>
        <td>[Parallax](parallax.md)
        </td>
        <td>Un efecto visual de paralaje ayuda a crear una sensación de profundidad, perspectiva y movimiento. Este efecto se consigue al mover distintos objetos a diferentes velocidades cuando la interfaz de usuario se desplaza o realiza un movimiento panorámico.
        </td>
    </tr> 
    <tr>
        <td>[Press feedback](motion-pointer.md)
        </td>
        <td>Las animaciones de presión del puntero proporcionan comentarios visuales a los usuarios cuando presionan un elemento. La animación de puntero abajo reduce e inclina ligeramente el elemento presionado y se reproduce cuando se pulsa un elemento por primera vez. La animación de puntero arriba, que restablece el elemento a su posición original, se reproduce cuando el usuario suelta el puntero.
        </td>
    </tr>
</table>