---
Description: Obtenga información sobre técnicas de diseño dinámico para adaptar la aplicación para dispositivos específicos
label: Responsive design techniques
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9e06131da5d7dd56354e1871867f9956ad13aa2c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624820"
---
# <a name="responsive-design-techniques"></a>Técnicas de diseño con capacidad de respuesta

Aplicaciones de UWP usan píxeles efectivos para garantizar que la interfaz de usuario serán legibles y se puede usar en todos los dispositivos con tecnología de Windows. Por lo tanto, ¿por qué querría personalizar la interfaz de usuario de la aplicación para una familia de dispositivos específicos?

- **Para hacer un uso más eficaz del espacio y reducir la necesidad de desplazarse**

    Si diseña una aplicación para mostrarse correctamente en un dispositivo que tenga una pantalla pequeña, como una tableta, la aplicación se podrá usar en un equipo con una pantalla mucho más grande, pero probablemente será algo de espacio desperdiciado. Puedes personalizar la aplicación para mostrar más contenido cuando la pantalla esté por encima de un determinado tamaño. Por ejemplo, una aplicación de compra podría mostrar una categoría de producto a la vez en una tableta, pero mostrar varias categorías y productos simultáneamente en un PC o portátil.

    Al colocar más contenido en la pantalla, se reduce la cantidad de navegación que el usuario necesita realizar.

- **Para aprovechar las capacidades de dispositivos**

    Determinados dispositivos tienen más probabilidad de contar con ciertas funcionalidades. Por ejemplo, equipos portátiles están probable que tenga un sensor de ubicación y una cámara, mientras que un Televisor no puede tener cualquiera. La aplicación puede detectar qué funcionalidades están disponibles y habilitar las funciones que las usan.

- **Para optimizar las entradas**

    La biblioteca de controles universales funciona con todos los tipos de entrada (táctil, lápiz, teclado, mouse), pero además se pueden optimizar ciertos tipos de entrada reorganizando los elementos de la interfaz de usuario. Por ejemplo, si se colocan elementos de navegación en la parte inferior de la pantalla, acceder a ellos será más fácil para los usuarios de teléfonos, pero la mayoría de los usuarios de equipos esperan ver los elementos de navegación en la parte superior de la pantalla.

Cuando se optimiza la interfaz de usuario de la aplicación para anchos de pantalla específicos, decimos que se está creando un diseño con capacidad de respuesta. Hay seis técnicas de diseño con capacidad de respuesta que puedes usar para personalizar la interfaz de usuario de la aplicación.

>[!TIP]
> Muchos controles UWP implementan automáticamente estos comportamientos con capacidad de respuesta. Para crear una interfaz de usuario con capacidad de respuesta, se recomienda desproteger el [controles UWP](../controls-and-patterns/index.md).

## <a name="reposition"></a>Cambiar la posición

Puede modificar la ubicación y la posición de elementos de interfaz de usuario para aprovechar al máximo el tamaño de ventana. En este ejemplo, la ventana más pequeña de pilas elementos verticalmente. Cuando la aplicación se convierte en una ventana más grande, los elementos pueden aprovechar el ancho de la ventana más amplio.

![Cambiar la posición](images/rsp-design/rspd-reposition2.gif)

En este diseño de ejemplo para una aplicación de fotos, la aplicación de fotos recoloca su contenido en las pantallas grandes.

## <a name="resize"></a>Cambiar el tamaño

Puede optimizar el tamaño de ventana ajustando los márgenes y el tamaño de los elementos de interfaz de usuario. Por ejemplo, esto podría aumentar la experiencia de lectura en una pantalla más grande aumentando simplemente el marco de contenido.

![Cambiar el tamaño de los elementos de diseño](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>Redistribuir

Al cambiar el flujo de elementos de la interfaz de usuario en función del dispositivo y la orientación, la aplicación puede ofrecer una visualización óptima del contenido. Por ejemplo, al pasar a una pantalla más grande, lo tendría sentido agregar columnas, usar contenedores más grandes o generar los elementos de lista de manera diferente.

En este ejemplo se muestra cómo una sola columna de contenido en una pantalla más pequeña que puede adaptarse en una pantalla más grande para mostrar dos columnas de texto que se desplaza verticalmente.

![Redistribuir los elementos de diseño](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>Mostrar/ocultar

Puedes mostrar u ocultar los elementos de una interfaz de usuario según la superficie de la pantalla o cuando el dispositivo admite funcionalidades adicionales, situaciones específicas u orientaciones de pantalla preferidas.

![Ocultar elementos de diseño](images/rsp-design/rspd-revealhide.gif)

Por ejemplo, los controles del Reproductor multimedia reducen el conjunto de botones en pantallas más pequeñas y expanden en pantallas más grandes. El Reproductor multimedia de una ventana más grande puede controlar en la pantalla mucho más funcionalidad que se puede en una ventana más pequeña.

Parte de la técnica de mostrar u ocultar incluye elegir cuándo se deben mostrar más metadatos. Con ventanas más pequeñas, es mejor mostrar una cantidad mínima de metadatos. Con windows de mayor tamaño, se puede mostrar una cantidad significativa de metadatos. Algunos ejemplos de cuándo se debe mostrar u ocultar los metadatos incluyen:

- En una aplicación de correo electrónico, donde puedes mostrar el avatar del usuario.
- En una aplicación de música, donde puedes mostrar más información acerca de un álbum o intérprete.
- En una aplicación de vídeo, donde puedes mostrar más información acerca de una película o un programa, como mostrar detalles del reparto y del equipo.
- En cualquier aplicación puedes desglosar columnas y mostrar más detalles.
- En cualquier aplicación puedes tomar elementos apilados verticalmente y distribuirlos horizontalmente. Al pasar desde un teléfono o tabléfono a dispositivos más grandes, los elementos de lista apilados pueden cambiar para mostrar filas de elementos de lista y columnas de metadatos.

## <a name="replace"></a>Reemplazar

Esta técnica le permite cambiar la interfaz de usuario de una interrupción específica. En este ejemplo, el panel de navegación y su diseño compacto, interfaz de usuario transitorio funciona bien para una pantalla más pequeña, pero en una pantalla más grande, las pestañas pueden ser una opción mejor.

![Reemplazar elementos de diseño](images/rsp-design/rspd-replace.gif)

El [NavigationView](../controls-and-patterns/navigationview.md) control es compatible con esta técnica con capacidad de respuesta, permitiendo a los usuarios que establece la posición del panel en la parte superior o izquierda.

## <a name="re-architect"></a>Rediseño

Puedes contraer o bifurcar la arquitectura de la aplicación para adaptarla mejor a dispositivos específicos. En este ejemplo, al expandir la ventana muestra todo el patrón principal-detalle.

![ejemplo de rediseñar una interfaz de usuario](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>Temas relacionados

- [Tamaños de pantalla y puntos de interrupción](screen-sizes-and-breakpoints-for-responsive-design.md)
- [Diseños con capacidad de respuesta con XAML](layouts-with-xaml.md)
- [Controles UWP y patrones](../controls-and-patterns/index.md)
