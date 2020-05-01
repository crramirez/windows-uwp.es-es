---
Description: Descubre técnicas de diseño con capacidad de respuesta para adaptar tu aplicación a dispositivos específicos.
title: Técnicas de diseño con capacidad de respuesta
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f688522ec8970b1e3570610663f5a3e6cae65793
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "68867410"
---
# <a name="responsive-design-techniques"></a>Técnicas de diseño con capacidad de respuesta

Las aplicaciones para UWP usan píxeles efectivos para garantizar que la interfaz de usuario sea legible y utilizable en todos los dispositivos con Windows. Por lo tanto, ¿por qué querría personalizar la interfaz de usuario de la aplicación para una familia de dispositivos específicos?

- **Para optimizar el uso del espacio y reducir la necesidad de desplazarse**

    Si diseñas una aplicación para que se vea bien en un dispositivo con una pantalla pequeña, como una tableta, la aplicación se podrá usar en un equipo con una pantalla mayor, pero probablemente se desperdiciará espacio. Puedes personalizar la aplicación para mostrar más contenido cuando la pantalla esté por encima de un determinado tamaño. Por ejemplo, una aplicación de compras podría mostrar una categoría de producto a la vez en una tableta, y varias categorías y productos al mismo tiempo en un equipo o en un portátil.

    Al colocar más contenido en la pantalla, se reduce la cantidad de navegación que el usuario necesita realizar.

- **Para aprovechar las funcionalidades de los dispositivos**

    Determinados dispositivos tienen más probabilidad de contar con ciertas funcionalidades. Por ejemplo, los portátiles suelen tener un sensor de ubicación y una cámara, mientras que un televisor puede que no tenga ninguna de las dos funcionalidades. La aplicación puede detectar qué funcionalidades están disponibles y habilitar las funciones que las usan.

- **Para optimizar la entrada**

    La biblioteca de controles universales funciona con todos los tipos de entrada (táctil, lápiz, teclado, mouse), pero además se pueden optimizar ciertos tipos de entrada reorganizando los elementos de la interfaz de usuario. Por ejemplo, si se colocan elementos de navegación en la parte inferior de la pantalla, acceder a ellos será más fácil para los usuarios de teléfonos, pero la mayoría de los usuarios de equipos esperan ver los elementos de navegación en la parte superior de la pantalla.

Cuando se optimiza la interfaz de usuario de la aplicación para anchos de pantalla específicos, decimos que se está creando un diseño con capacidad de respuesta. Hay seis técnicas de diseño con capacidad de respuesta que puedes usar para personalizar la interfaz de usuario de la aplicación.

>[!TIP]
> Muchos controles de UWP implementan automáticamente estos comportamientos con capacidad de respuesta. Para crear una interfaz de usuario con capacidad de respuesta, consulta los [controles de UWP](../controls-and-patterns/index.md).

## <a name="reposition"></a>Cambiar la posición

Puedes modificar la ubicación y la posición de los elementos de la interfaz de usuario para sacar el máximo partido al tamaño de la ventana. En este ejemplo, la ventana más pequeña apila los elementos verticalmente. Cuando la aplicación se convierte en una ventana de mayor tamaño, los elementos pueden aprovechar el ancho de ventana más amplio.

![Cambiar la posición](images/rsp-design/rspd-reposition2.gif)

En este diseño de ejemplo para una aplicación de fotos, la aplicación de fotos recoloca su contenido en las pantallas grandes.

## <a name="resize"></a>Cambiar el tamaño

Puedes optimizar el tamaño de la ventana ajustando los márgenes y el tamaño de los elementos de la interfaz de usuario. Por ejemplo, esto podría mejorar la experiencia de lectura en una pantalla más grande simplemente aumentado el tamaño del marco de contenido.

![Cambiar el tamaño de los elementos de diseño](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>Redistribuir

Al cambiar el flujo de elementos de la interfaz de usuario en función del dispositivo y la orientación, la aplicación puede ofrecer una visualización óptima del contenido. Por ejemplo, al ir a una pantalla más grande, es posible que tenga sentido agregar columnas, usar contenedores más grandes o generar los elementos de lista de manera diferente.

En este ejemplo se muestra cómo se puede adaptar a una única columna de contenido de desplazamiento vertical en una pantalla pequeña a una pantalla mayor para mostrar dos columnas de texto.

![Redistribuir los elementos de diseño](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>Mostrar u ocultar

Puedes mostrar u ocultar los elementos de una interfaz de usuario según la superficie de la pantalla o cuando el dispositivo admite funcionalidades adicionales, situaciones específicas u orientaciones de pantalla preferidas.

![Ocultar elementos de diseño](images/rsp-design/rspd-revealhide.gif)

Por ejemplo, los controles del reproductor multimedia reducen el botón en las pantallas más pequeñas y lo amplían en las pantallas más grandes. El reproductor multimedia en una pantalla mayor puede incluir más controles que en una pantalla más pequeña.

Parte de la técnica de mostrar u ocultar incluye elegir cuándo se deben mostrar más metadatos. En las ventanas más pequeñas, es mejor mostrar una cantidad mínima de metadatos. En las ventanas más grandes, puede aparecer una cantidad significativa de metadatos. Algunos ejemplos de cuándo mostrar u ocultar metadatos son:

- En una aplicación de correo electrónico, donde puedes mostrar el avatar del usuario.
- En una aplicación de música, donde puedes mostrar más información acerca de un álbum o intérprete.
- En una aplicación de vídeo, donde puedes mostrar más información acerca de una película o un programa, como mostrar detalles del reparto y del equipo.
- En cualquier aplicación puedes desglosar columnas y mostrar más detalles.
- En cualquier aplicación puedes tomar elementos apilados verticalmente y distribuirlos horizontalmente. Al pasar desde un teléfono o tabléfono a dispositivos más grandes, los elementos de lista apilados pueden cambiar para mostrar filas de elementos de lista y columnas de metadatos.

## <a name="replace"></a>Replace

Esta técnica permite cambiar la interfaz de usuario para un punto de interrupción específico. En este ejemplo, el panel de navegación y su interfaz de usuario compacta transitoria funcionan bien para una pantalla más pequeño, pero en una pantalla mayor, las pestañas pueden ser una elección mejor.

![Reemplazar elementos de diseño](images/rsp-design/rspd-replace.gif)

El control [NavigationView](../controls-and-patterns/navigationview.md) admite esta técnica con capacidad de respuesta, porque permite a los usuarios establecer la posición del panel en la parte superior o izquierda.

## <a name="re-architect"></a>Rediseño

Puedes contraer o bifurcar la arquitectura de la aplicación para adaptarla mejor a dispositivos específicos. En este ejemplo, al expandir la ventana se muestra todo el patrón de maestro y los detalles.

![ejemplo de rediseñar una interfaz de usuario](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>Temas relacionados

- [Tamaños de pantalla y puntos de interrupción](screen-sizes-and-breakpoints-for-responsive-design.md)
- [Diseños dinámicos con XAML](layouts-with-xaml.md)
- [Controles y patrones de UWP](../controls-and-patterns/index.md)
