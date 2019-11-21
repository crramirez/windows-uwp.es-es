---
Description: En este tema se describe la nueva interfaz de usuario de Windows para la rotación y se proporcionan instrucciones para la experiencia del usuario que deben tenerse en cuenta al usar este nuevo mecanismo de interacción en la aplicación para UWP.
title: Rotación
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 71a09d304b0d68bf01012166173360ec6304fb2c
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257925"
---
# <a name="rotation"></a>Rotación


En este artículo se describe la nueva interfaz de usuario de Windows para realizar una rotación. También se ofrecen directrices sobre la experiencia del usuario que debes tener presente cuando uses este nuevo mecanismo de interacción en una aplicación para UWP.

> **API importantes**: [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="dos-and-donts"></a>Qué hacer y qué no hacer

-   Usa la rotación para ayudar a los usuarios a girar directamente elementos de la interfaz de usuario.

## <a name="additional-usage-guidance"></a>Instrucciones de uso adicionales


**Información general sobre la rotación**

La rotación es la técnica optimizada para entrada táctil que utilizan las aplicaciones para UWP para permitir que los usuarios giren un objeto en dirección circular (en el sentido de las agujas del reloj o en el sentido contrario).

En función del dispositivo de entrada, la interacción de rotación se realiza con lo siguiente:

-   Un mouse o una pluma/lápiz activos para mover la barra de redimensionamiento para rotación de un objeto seleccionado.
-   Una pluma o un lápiz táctiles o pasivos para girar el objeto en la dirección deseada mediante el gesto de girar.

**Cuándo usar la rotación**

Usa la rotación para ayudar a los usuarios a girar directamente elementos de la interfaz de usuario. En los diagramas siguientes se muestran algunas de las posiciones de los dedos admitidas para la interacción de rotación.

![diagrama en el que se muestran varias posturas de los dedos admitidas por la rotación.](images/ux-rotate-positions.png)

**Tenga en cuenta**   intuitivamente y, en la mayoría de los casos, el punto de rotación es uno de los dos puntos táctiles, a menos que el usuario pueda especificar un punto de rotación no relacionado con los puntos de contacto (por ejemplo, en una aplicación de diseño o dibujo). Las siguientes imágenes muestran cómo puede degradarse la experiencia del usuario si el punto de rotación no está restringido de esa manera.

En la primera imagen se muestran los puntos de contacto táctil inicial (pulgar) y secundario (índice): el índice está tocando un árbol y el pulgar toca un tronco.

![imagen que muestra los dos puntos de toque iniciales para el gesto de giro.](images/ux-rotate-points1.png)
En la segunda imagen, la rotación se ejecuta en torno del punto táctil inicial (pulgar). Después de la rotación, el índice sigue tocando el árbol y el pulgar sigue en contacto con el tronco (el punto de rotación).

![imagen que muestra una imagen girada con el punto de giro restringido a uno de los dos puntos de toque iniciales.](images/ux-rotate-points2.png)
En la tercera imagen, la aplicación (o el usuario) ha definido que el centro de rotación sea el punto central de la ilustración. Después de la rotación, la ilusión de manipulación directa se ha roto porque la ilustración no giró en torno de uno de los dedos (a menos que fuera el usuario quien hubiera elegido esa configuración).

![imagen que muestra una imagen girada con el punto de giro restringido al centro de la imagen en lugar de a uno de los dos puntos táctiles iniciales.](images/ux-rotate-points3.png)
En esta última imagen, la aplicación (o el usuario) ha definido que el centro de rotación sea un punto en el medio del borde izquierdo de la ilustración. De nuevo, a menos que sea el usuario quien haya elegido esa configuración, la ilusión de manipulación directa se rompe.

![imagen en la que se muestra una ilustración girada en la que el punto de rotación está restringido al centro del borde izquierdo de la ilustración en lugar de uno de los dos puntos táctiles iniciales.](images/ux-rotate-points4.png)

 

Windows 10 admite tres tipos de rotación: gratis, restringido y combinado.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tipo</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Rotación libre</td>
<td align="left"><p>La rotación libre permite que el usuario rote el contenido libremente dentro de un arco de 360 grados. Cuando el usuario suelta el objeto, el objeto permanece en la posición elegida. La rotación libre es útil para aplicaciones de dibujo y diseño como Microsoft PowerPoint, Word, Visio y Paint; y Photoshop, Illustrator y Flash de Adobe.</p></td>
</tr>
<tr class="even">
<td align="left">Rotación restringida</td>
<td align="left"><p>La rotación restringida admite la rotación libre durante la manipulación pero impone puntos de acoplamiento con incrementos de 90 grados (0, 90, 180 y 270) cuando se suelta el contenido. Cuando el usuario suelta el objeto, el objeto rota de manera automática hasta el punto de acoplamiento más cercano.</p>
<p>La rotación restringida es el método más común de rotación y funciona de manera similar al desplazamiento de contenido. Los puntos de acoplamiento permiten que el usuario alcance sus objetivos, aunque carezca de precisión. La rotación restringida es útil para aplicaciones como exploradores web y álbumes de fotos.</p></td>
</tr>
<tr class="odd">
<td align="left">Rotación combinada</td>
<td align="left"><p>La rotación combinada admite la rotación libre pero con zonas (similares a las guías de las <a href="guidelines-for-panning.md">Instrucciones del movimiento panorámico</a>) en cada uno de los puntos de acoplamiento situados cada 90 grados impuestos en la rotación restringida. Si el usuario libera el objeto fuera de una de las zonas de 90 grados, el objeto permanece en esa posición; de otro modo, el objeto gira de manera automática hasta un punto de acoplamiento.</p>
<div class="alert">
<strong>Tenga en cuenta</strong>  un raíl de la interfaz de usuario es una característica en la que un área alrededor de un destino limita el movimiento hacia algún valor o ubicación específicos para influir en su selección.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>Temas relacionados


**Ejemplos**
* [Ejemplo de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Ejemplo de entrada de baja latencia](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [Ejemplo de modo de interacción del usuario](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**Ejemplos de archivo**
* [Entrada: ejemplo de eventos de entrada de usuario de XAML](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [Entrada: ejemplo de funcionalidades del dispositivo](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [Entrada: ejemplo de prueba de posicionamiento táctil](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [Ejemplo de desplazamiento, panorámica y zoom de XAML](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [Entrada: ejemplo de entrada de lápiz simplificada](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [Entrada: gestos y manipulaciones con GestureRecognizer](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Entrada: ejemplo de manipulaciones y gestos (C++)](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [Ejemplo de entrada táctil de DirectX](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 




