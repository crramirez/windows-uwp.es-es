---
Description: En este tema se describe la nueva interfaz de usuario de Windows para seleccionar y manipular texto, imágenes y controles, y se proporcionan instrucciones para la experiencia del usuario que deben tenerse en cuenta al usar estos nuevos mecanismos de selección y manipulación en la aplicación de Windows.
title: Seleccionar texto e imágenes
ms.assetid: d973ffd8-602e-47b5-ab0b-4b2a964ec53d
label: Selecting text and images
template: detail.hbs
keywords: teclado,texto,entrada,interacciones del usuario
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 179eeffd014cb614fb5314826068d9690fc29807
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216978"
---
# <a name="selecting-text-and-images"></a>Seleccionar texto e imágenes


En este artículo se describen la selección y la manipulación de texto, imágenes y controles, y se ofrecen directrices sobre la experiencia del usuario que debes tener en cuenta al usar estos mecanismos en las aplicaciones.

> **API importantes**: [**Windows. UI. Xaml. Input**](/uwp/api/Windows.UI.Xaml.Input), [**Windows. UI. Input**](/uwp/api/Windows.UI.Input)
 


## <a name="dos-and-donts"></a>Consejos


-   Usa glifos de fuentes al implementar una interfaz de usuario de barra de redimensionamiento propia. La barra de redimensionamiento es una combinación de dos fuentes Segoe UI que se encuentran disponibles en todo el sistema. El uso de recursos de fuente simplifica los problemas de representación a diferentes ppp y da resultado con las diversas mesetas de escalado de la interfaz de usuario. Cuando implementes tus propias barras de redimensionamiento, estas deben compartir los siguientes rasgos de interfaz de usuario:

    -   Forma circular
    -   Visible con cualquier fondo
    -   Tamaño coherente
-   Proporciona un margen alrededor del contenido seleccionable para acomodar la interfaz de usuario de la barra de redimensionamiento. Si la aplicación permite la selección de texto en una región en la que no hay disponible desplazamiento ni desplazamiento lateral, deja un margen de media barra de redimensionamiento en los lados izquierdo y derecho del área de texto y la altura de una barra de redimensionamiento en los lados superior e inferior del área de texto (como se muestra en las siguientes imágenes). Esto garantiza que el usuario vea toda la interfaz de usuario de la barra de redimensionamiento y minimiza las interacciones accidentales con otras interfaces de usuario en el borde.

    ![márgenes de la barra de redimensionamiento de selección de texto](images/textselection-gripper-margins.png)

-   Oculta la interfaz de usuario de las barras de redimensionamiento durante la interacción. Elimina la oclusión producida por las barras de redimensionamiento durante la interacción. Esto resulta útil cuando el dedo no oculta completamente la barra de redimensionamiento o cuando hay varias barras de redimensionamiento de selección de texto. Esto elimina los artefactos visuales al mostrar ventanas secundarias.

-   No permitas la selección de elementos de interfaz de usuario, como controles, etiquetas, imágenes, contenido registrado, etc. Por lo general, las aplicaciones de Windows solo permiten seleccionar dentro de controles específicos. Los controles como los botones, las etiquetas y los logotipos no son seleccionables. Evalúa si la selección puede ser un problema para la aplicación y, si es así, identifica las áreas de la interfaz de usuario donde debes prohibir la selección. 

## <a name="additional-usage-guidance"></a>Instrucciones de uso adicionales


La selección y la manipulación de texto son particularmente sensibles frente a los retos que pueden presentar las interacciones táctiles en la experiencia del usuario. Las entradas mediante mouse, pluma o lápiz y teclado tienen un alto grado de detalle: el clic de un mouse o el contacto de una pluma o lápiz por lo general se asignan a un solo píxel, y una tecla se considera presionada o no presionada. La entrada táctil no es precisa: es difícil asignar toda la superficie de la punta de un dedo a una ubicación x-y específica en la pantalla para situar un símbolo de intercalación de texto de manera precisa.

**Consideraciones y recomendaciones**

Usa los controles integrados que se exponen a través de los marcos de lenguaje de Windows para crear aplicaciones que proporcionen toda la experiencia de interacción del usuario de la plataforma, incluidos los comportamientos de selección y manipulación. Encontrará la funcionalidad de interacción de los controles integrados suficientes para la mayoría de las aplicaciones de Windows.

Al usar controles de texto de Windows estándar, los comportamientos de selección y los objetos visuales que se describen en este tema no se pueden personalizar.

**Selección de texto**

Si la aplicación requiere una interfaz de usuario personalizada que admita la selección de texto, es recomendable seguir los comportamientos de selección de Windows que se describen en este artículo.

**Contenido modificable y no modificable**


Con la entrada táctil, las interacciones de selección se ejecutan básicamente mediante gestos, como pulsar para establecer un cursor de inserción o seleccionar una palabra y deslizar para modificar una selección. Como ocurre con otras interacciones táctiles en Windows, las interacciones con intervalo están limitadas al gesto de pulsar y sostener para mostrar una interfaz de usuario informativa. Para obtener más información, consulta las [directrices para información visual](guidelines-for-visualfeedback.md).

Windows reconoce dos estados posibles para las interacciones de selección, modificables y no modificables, y ajusta la interfaz de usuario, la información y la funcionalidad de selección en función de esa distinción.

**Contenido modificable**

Al pulsar en la mitad izquierda de una palabra, el cursor aparece inmediatamente a la izquierda de la palabra, mientras que al pulsar dentro de la mitad derecha, el cursor aparece inmediatamente a la derecha de la palabra.

En la imagen siguiente se muestra cómo colocar un cursor de inserción inicial con barra de redimensionamiento pulsando cerca del comienzo o el final de una palabra.

![pulsa (o pulsa y sostiene) el lado izquierdo de una palabra para colocar un símbolo de intercalación y una barra de redimensionamiento al comienzo de esa palabra. pulsa (o pulsa y sostiene) el lado derecho de una palabra para colocar un símbolo de intercalación y una barra de redimensionamiento al final de esa palabra.](images/textselection-place-caret.png)

En la imagen siguiente se muestra cómo ajustar una selección arrastrando la barra de redimensionamiento.

![arrastra la barra de redimensionamiento en cualquier dirección para ajustar la selección (la primera barra de redimensionamiento queda anclada y se muestra una segunda barra de redimensionamiento). arrastra cualquiera de las dos para hacer más ajustes.](images/adjust-selection.png)

En las imágenes siguientes se muestra cómo invocar el menú contextual pulsando dentro de la selección o sobre una barra de redimensionamiento (también puedes usar el gesto de pulsar y sostener).

![pulsa (o pulsa y sostén) dentro de la selección o en una barra de redimensionamiento para invocar el menú contextual.](images/textselection-show-context.png)

**Nota:**    Estas interacciones varían en cierto modo en el caso de una palabra mal escrita. Si pulsas una palabra marcada por tener errores ortográficos, la palabra se resalta y se invoca el menú contextual con la ortografía sugerida.

 

**Contenido no modificable**

En la imagen siguiente se muestra cómo seleccionar una palabra pulsando dentro de la palabra (no se incluyen espacios en la selección inicial).

![pulsa dentro de una palabra para seleccionarla (no se incluyen espacios en la selección inicial).](images/select-word.png)

Para ajustar la selección y mostrar el menú contextual, sigue los mismos procedimientos que con el texto modificable.

**Manipulación de objetos**

Siempre que sea posible, use los mismos recursos de agarrador (o similares) que la selección de texto al implementar la manipulación de objetos personalizada en una aplicación de Windows. Esto ayuda a ofrecer una experiencia de interacción coherente en toda la plataforma.

Por ejemplo, las barras de redimensionamiento también se pueden usar en aplicaciones de procesamiento de imágenes que admitan cambio de tamaño y recorte, o bien en aplicaciones de reproductor multimedia que proporcionen barras de progreso ajustables, como se muestra en las siguientes imágenes.

![reproductor multimedia con barra de redimensionamiento de progreso](images/gripper-mediaplayer.png)

*Reproductor multimedia con barra de redimensionamiento ajustable.*

![imagen con barra de redimensionamiento de recorte](images/gripper-imagemanip.png)

*Editor de imágenes con barras de redimensionamiento de recorte.*

## <a name="related-articles"></a>Artículos relacionados

### <a name="for-developers"></a>Para desarrolladores

- [Interacciones del usuario personalizadas](../layout/index.md)

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
- [Entrada: muestra de gestos de Windows 8](/samples/browse/?redirectedfrom=MSDN-samples)
- [Entrada: ejemplo de manipulaciones y gestos](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Muestra de entrada táctil de DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
