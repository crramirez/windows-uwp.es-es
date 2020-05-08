---
Description: En este tema se describe la nueva interfaz de usuario de Windows para la rotación y se proporcionan instrucciones para la experiencia del usuario que deben tenerse en cuenta al usar este nuevo mecanismo de interacción en la aplicación de Windows.
title: Rotación
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 08d0eb18d59c9a5c19826eb7b6e8d4b65179b6fd
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970110"
---
# <a name="rotation"></a>Rotación


En este artículo se describe la nueva interfaz de usuario de Windows para la rotación y se proporcionan instrucciones para la experiencia del usuario que deben tenerse en cuenta al usar este nuevo mecanismo de interacción en la aplicación de Windows.

> **API importantes**: [**Windows. UI. Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows. UI. Xaml. Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="dos-and-donts"></a>Consejos

-   Usa la rotación para ayudar a los usuarios a girar directamente elementos de la interfaz de usuario.

## <a name="additional-usage-guidance"></a>Instrucciones de uso adicionales


**Introducción a la rotación**

La rotación es la técnica táctil y optimizada que usan las aplicaciones de Windows para que los usuarios puedan convertir un objeto en una dirección circular (en el sentido de las agujas del reloj o en el sentido contrario).

En función del dispositivo de entrada, la interacción de rotación se realiza con lo siguiente:

-   Un mouse o una pluma/lápiz activos para mover la barra de redimensionamiento para rotación de un objeto seleccionado.
-   Una pluma o un lápiz táctiles o pasivos para girar el objeto en la dirección deseada mediante el gesto de girar.

**Cuándo usar la rotación**

Usa la rotación para ayudar a los usuarios a girar directamente elementos de la interfaz de usuario. En los diagramas siguientes se muestran algunas de las posiciones de los dedos admitidas para la interacción de rotación.

![diagrama en el que se muestran varias posturas de los dedos admitidas por la rotación.](images/ux-rotate-positions.png)

**Nota**    de forma intuitiva y, en la mayoría de los casos, el punto de giro es uno de los dos puntos táctiles, a menos que el usuario pueda especificar un punto de rotación no relacionado con los puntos de contacto (por ejemplo, en una aplicación de dibujo o diseño). Las siguientes imágenes muestran cómo puede degradarse la experiencia del usuario si el punto de rotación no está restringido de esa manera.

En la primera imagen se muestran los puntos de contacto táctil inicial (pulgar) y secundario (índice): el índice está tocando un árbol y el pulgar toca un tronco.

![imagen en la que se muestran los dos puntos táctiles iniciales en el gesto de girar.](images/ux-rotate-points1.png)
En la segunda imagen, la rotación se ejecuta en torno del punto táctil inicial (pulgar). Después de la rotación, el índice sigue tocando el árbol y el pulgar sigue en contacto con el tronco (el punto de rotación).

![imagen en la que se muestra una ilustración girada en la que el punto de rotación está restringido a uno de los dos puntos táctiles iniciales.](images/ux-rotate-points2.png)
En la tercera imagen, la aplicación (o el usuario) ha definido que el centro de rotación sea el punto central de la ilustración. Después de la rotación, la ilusión de manipulación directa se ha roto porque la ilustración no giró en torno de uno de los dedos (a menos que fuera el usuario quien hubiera elegido esa configuración).

![imagen en la que se muestra una ilustración girada en la que el punto de rotación está restringido al centro de la ilustración en lugar de uno de los dos puntos táctiles iniciales.](images/ux-rotate-points3.png)
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
<td align="left"><p>La rotación libre permite a un usuario girar el contenido libremente en cualquier parte de un arco de 360 grados. Cuando el usuario suelta el objeto, el objeto permanece en la posición elegida. La rotación libre es útil para aplicaciones de dibujo y diseño como Microsoft PowerPoint, Word, Visio y Paint; y Photoshop, Illustrator y Flash de Adobe.</p></td>
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
<strong>Nota:</strong>  un raíl de la interfaz de usuario es una característica en la que un área alrededor de un destino limita el movimiento hacia algún valor o ubicación específicos para influir en su selección.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

## <a name="related-topics"></a>Temas relacionados

### <a name="samples"></a>Ejemplos

- [Ejemplo de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Ejemplo de entrada de latencia baja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Ejemplo de modo de interacción del usuario](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Ejemplos de archivo

- [Entrada: muestra de eventos de entrada de usuario de XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrada: muestra de funcionalidades del dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrada: muestra de prueba de acceso táctil](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Ejemplo de desplazamiento, panorámica y zoom de XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrada: ejemplo de entrada de lápiz simplificada](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Entrada: gestos y manipulaciones con GestureRecognizer](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [Entrada: ejemplo de manipulaciones y gestos](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Muestra de entrada táctil de DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
