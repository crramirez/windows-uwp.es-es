---
Description: Aprenda técnicas de diseño con capacidad de respuesta para adaptar la aplicación a dispositivos específicos.
title: Técnicas de diseño con capacidad de respuesta
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f688522ec8970b1e3570610663f5a3e6cae65793
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867410"
---
# <a name="responsive-design-techniques"></a>Técnicas de diseño con capacidad de respuesta

Las aplicaciones para UWP usan píxeles eficaces para garantizar que la interfaz de usuario será legible y utilizable en todos los dispositivos con Windows. Por lo tanto, ¿por qué querría personalizar la interfaz de usuario de la aplicación para una familia de dispositivos específicos?

- **Para hacer el uso más eficaz del espacio y reducir la necesidad de navegar**

    Si diseña una aplicación para que tenga un aspecto correcto en un dispositivo que tiene una pantalla pequeña, como una tableta, la aplicación se podrá usar en un equipo con una pantalla mucho más grande, pero es probable que haya algún espacio desaprovechado. Puedes personalizar la aplicación para mostrar más contenido cuando la pantalla esté por encima de un determinado tamaño. Por ejemplo, una aplicación de compras podría mostrar una categoría de mercancía a la vez en una tableta, pero mostrar varias categorías y productos simultáneamente en un equipo o portátil.

    Al colocar más contenido en la pantalla, se reduce la cantidad de navegación que el usuario necesita realizar.

- **Para aprovechar las capacidades de los dispositivos**

    Determinados dispositivos tienen más probabilidad de contar con ciertas funcionalidades. Por ejemplo, es probable que los equipos portátiles tengan un sensor de ubicación y una cámara, mientras que un televisor podría no tener ninguno. La aplicación puede detectar qué funcionalidades están disponibles y habilitar las funciones que las usan.

- **Para optimizar para la entrada**

    La biblioteca de controles universales funciona con todos los tipos de entrada (táctil, lápiz, teclado, mouse), pero además se pueden optimizar ciertos tipos de entrada reorganizando los elementos de la interfaz de usuario. Por ejemplo, si se colocan elementos de navegación en la parte inferior de la pantalla, acceder a ellos será más fácil para los usuarios de teléfonos, pero la mayoría de los usuarios de equipos esperan ver los elementos de navegación en la parte superior de la pantalla.

Cuando se optimiza la interfaz de usuario de la aplicación para anchos de pantalla específicos, decimos que se está creando un diseño con capacidad de respuesta. Hay seis técnicas de diseño con capacidad de respuesta que puedes usar para personalizar la interfaz de usuario de la aplicación.

>[!TIP]
> Muchos controles de UWP implementan automáticamente estos comportamientos de capacidad de respuesta. Para crear una interfaz de usuario con capacidad de respuesta, se recomienda desproteger los [controles de UWP](../controls-and-patterns/index.md).

## <a name="reposition"></a>Cambiar la posición

Puede modificar la ubicación y la posición de los elementos de la interfaz de usuario para sacar el máximo partido del tamaño de la ventana. En este ejemplo, la ventana más pequeña apila verticalmente los elementos. Cuando la aplicación se convierte en una ventana de mayor tamaño, los elementos pueden aprovechar el ancho de ventana más amplio.

![Cambiar la posición](images/rsp-design/rspd-reposition2.gif)

En este diseño de ejemplo para una aplicación de fotos, la aplicación de fotos recoloca su contenido en las pantallas grandes.

## <a name="resize"></a>Cambiar el tamaño

Puede optimizar el tamaño de la ventana ajustando los márgenes y el tamaño de los elementos de la interfaz de usuario. Por ejemplo, esto podría aumentar la experiencia de lectura en una pantalla más grande aumentando simplemente el marco de contenido.

![Cambiar el tamaño de los elementos de diseño](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>Redistribuir

Al cambiar el flujo de elementos de la interfaz de usuario en función del dispositivo y la orientación, la aplicación puede ofrecer una visualización óptima del contenido. Por ejemplo, al ir a una pantalla más grande, es posible que tenga sentido Agregar columnas, usar contenedores más grandes o generar elementos de lista de manera diferente.

Este ejemplo muestra cómo una sola columna de contenido de desplazamiento vertical en una pantalla más pequeña que se puede refluir en una pantalla más grande para mostrar dos columnas de texto.

![Redistribuir los elementos de diseño](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>Mostrar/ocultar

Puedes mostrar u ocultar los elementos de una interfaz de usuario según la superficie de la pantalla o cuando el dispositivo admite funcionalidades adicionales, situaciones específicas u orientaciones de pantalla preferidas.

![Ocultar elementos de diseño](images/rsp-design/rspd-revealhide.gif)

Por ejemplo, los controles del reproductor de media reducen el botón establecido en pantallas más pequeñas y se expanden en pantallas más grandes. El reproductor de media de una ventana más grande puede controlar mucho más funcionalidad en pantalla que en una ventana más pequeña.

Parte de la técnica de mostrar u ocultar incluye elegir cuándo se deben mostrar más metadatos. Con ventanas más pequeñas, es mejor mostrar una cantidad mínima de metadatos. Con las ventanas más grandes, puede aparecer una cantidad significativa de metadatos. Algunos ejemplos de Cuándo Mostrar u ocultar metadatos son:

- En una aplicación de correo electrónico, donde puedes mostrar el avatar del usuario.
- En una aplicación de música, donde puedes mostrar más información acerca de un álbum o intérprete.
- En una aplicación de vídeo, donde puedes mostrar más información acerca de una película o un programa, como mostrar detalles del reparto y del equipo.
- En cualquier aplicación puedes desglosar columnas y mostrar más detalles.
- En cualquier aplicación puedes tomar elementos apilados verticalmente y distribuirlos horizontalmente. Al pasar desde un teléfono o tabléfono a dispositivos más grandes, los elementos de lista apilados pueden cambiar para mostrar filas de elementos de lista y columnas de metadatos.

## <a name="replace"></a>Reemplazar

Esta técnica permite cambiar la interfaz de usuario para un punto de interrupción concreto. En este ejemplo, el panel de navegación y su interfaz de usuario de la solución compacta y transitoria funcionan bien para una pantalla más pequeña, pero en una pantalla más grande, las pestañas pueden ser una mejor opción.

![Reemplazar elementos de diseño](images/rsp-design/rspd-replace.gif)

El control [NavigationView](../controls-and-patterns/navigationview.md) admite esta técnica de capacidad de respuesta, ya que permite a los usuarios establecer la posición del panel en la parte superior o izquierda.

## <a name="re-architect"></a>Rediseño

Puedes contraer o bifurcar la arquitectura de la aplicación para adaptarla mejor a dispositivos específicos. En este ejemplo, al expandir la ventana se muestra todo el patrón de maestro y detalles.

![ejemplo de rediseñar una interfaz de usuario](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>Temas relacionados

- [Tamaños de pantalla y puntos de interrupción](screen-sizes-and-breakpoints-for-responsive-design.md)
- [Diseños dinámicos con XAML](layouts-with-xaml.md)
- [Controles y patrones de UWP](../controls-and-patterns/index.md)
