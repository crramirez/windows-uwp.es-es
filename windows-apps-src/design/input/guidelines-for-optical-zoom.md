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
ms.openlocfilehash: 0de537ec8b3b1fde0692234f7b4f39350459b7fe
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82558796"
---
# <a name="optical-zoom-and-resizing"></a>Zoom óptico y cambio de tamaño



En este artículo se describen los elementos de zoom y cambio de tamaño de Windows. También se ofrecen instrucciones de experiencia de usuario para que uses estos nuevos mecanismos de interacción en las aplicaciones.

> **API importantes**: [**Windows. UI. Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Input (XAML)**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

El zoom óptico permite a los usuarios ampliar la vista del contenido dentro de un área de contenido (se ejecuta sobre la propia área de contenido), mientras que el cambio de tamaño permite cambiar el tamaño relativo de uno o varios objetos sin cambiar la vista del área de contenido (se ejecuta sobre los objetos que están dentro del área de contenido).

Tanto la interacción de zoom óptico como la de cambio de tamaño se ejecutan con los gestos de reducir y ampliar (separar los dedos acerca el contenido y acercar los dedos lo aleja), manteniendo presionada la tecla Ctrl mientras se desplaza la rueda de desplazamiento del mouse o manteniendo presionada la tecla Ctrl (con la tecla Mayús, si no se dispone de teclado numérico) y presionando la tecla de signo más (+) o menos (-).

Los siguientes diagramas muestran las diferencias entre cambio de tamaño y zoom óptico.

**Zoom óptico**: el usuario selecciona un área y después acerca o aleja el área completa.

![juntar los dedos acerca el área de contenido, y separarlos la aleja.](images/areazoom.png)

**Cambio de tamaño**: el usuario selecciona un objeto dentro de un área y cambia el tamaño del objeto.

![juntar los dedos reduce el objeto, y alejarlos lo amplía.](images/objectresize.png)

**Nota**    el zoom óptico no se debe confundir con el [zoom semántico](../controls-and-patterns/semantic-zoom.md). Aunque en las dos interacciones se usan los mismos gestos, el zoom semántico se refiere a la presentación y navegación de datos o contenido estructurados en una única vista (como la estructura de carpetas de un equipo, una biblioteca de documentos o un álbum de fotos).

 

## <a name="dos-and-donts"></a>Consejos


Sigue las directrices que se indican a continuación para las aplicaciones que admitan el cambio de tamaño o zoom óptico:

-   Si defines restricciones o límites de tamaño máximo y mínimo, usa información visual para indicar al usuario cuándo ha alcanzado o superado esos límites.
-   Usa puntos de acoplamiento para influir en el comportamiento de zoom y cambio de tamaño, proporcionando puntos lógicos en los cuales detener la manipulación y garantizar así que en la ventanilla se muestre un subconjunto específico de contenido. Proporciona puntos de acoplamiento para niveles de zoom comunes o vistas lógicas para que al usuario le resulte más fácil seleccionar esos niveles. Por ejemplo, las aplicaciones de fotos podrían proporcionar un punto de acoplamiento de cambio de tamaño en el 100% o, en el caso de las aplicaciones de mapas, los puntos de acoplamiento podrían resultar útiles en las vistas de ciudad, estado y país.

    Los puntos de acoplamiento permiten que el usuario alcance sus objetivos, aunque carezca de precisión. Si usas XAML, consulta las propiedades de los puntos de acoplamiento de [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer). En JavaScript y HTML, usa [**-ms-content-zoom-snap-points**](https://msdn.microsoft.com/library/hh771895).

    Existen dos tipos de puntos de acoplamiento:

    -   Proximidad: después de levantar el dedo, se selecciona un punto de acoplamiento si la inercia se detiene dentro del umbral de distancia del punto de acoplamiento. Los puntos de acoplamiento en proximidad aún permiten que el zoom y el cambio de tamaño finalicen entre los puntos de acoplamiento.
    -   Obligatorio: el punto de acoplamiento seleccionado es el que precede o sigue inmediatamente al último punto de acoplamiento que se cruzó antes de levantar el contacto (en función de la dirección y velocidad del gesto). Una manipulación debe finalizar en un punto de acoplamiento obligatorio.
-   Usa física de inercia. Entre ellas se incluyen las siguientes:
    -   Desaceleración: ocurre cuando el usuario deja de reducir o ampliar. Es similar a la acción de dejar de deslizarse en una superficie resbaladiza.
    -   Rebote: se produce un ligero efecto de rebote cuando se supera un límite o una restricción de cambio de tamaño.
-   Distribuye los controles de acuerdo con las [directrices para destinos](guidelines-for-targeting.md).
-   Proporciona controladores de ajuste de escala para cambiar el tamaño de manera restringida. Si no se especifican controladores, el cambio de tamaño predeterminado es el isométrico o proporcional.
-   No uses el zoom para navegar por la interfaz de usuario ni exponer otros controles en la aplicación; en su lugar, usa una región de movimiento panorámico. Para obtener más información sobre el movimiento panorámico, consulta las [instrucciones del movimiento panorámico](guidelines-for-panning.md).
-   No coloques objetos redimensionables dentro de un área de contenido redimensionable. Las excepciones son:
    -   Las aplicaciones de dibujo en las que pueden aparecer elementos redimensionables en un elemento canvas redimensionable.
    -   Las páginas web con un objeto incrustado, como por ejemplo un mapa.

    **Tenga en cuenta**    que, en todos los casos, el área de contenido cambia de tamaño a menos que todos los puntos táctiles estén dentro del objeto de tamaño variable.

## <a name="related-articles"></a>Artículos relacionados

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
- [Entrada: muestra de gestos de Windows 8](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [Entrada: ejemplo de manipulaciones y gestos](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Muestra de entrada táctil de DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
