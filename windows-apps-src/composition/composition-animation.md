---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Animaciones de composición
description: Muchas de las propiedades de objetos y efectos de composición se pueden animar con animaciones de fotogramas y expresión clave, lo que permite a las propiedades de un elemento de la interfaz de usuario cambiar con el tiempo o según un cálculo concreto.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 18208986d7d07e4d437e52dce844deecc03cf1f6
ms.sourcegitcommit: 681c1e3836d2a51cd3b31d824ece344281932bcd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59240033"
---
# <a name="composition-animations"></a>Animaciones de composición

Las API de Windows.UI.Composition te permiten crear, animar, transformar y manipular objetos de compositor en una capa de API unificada. Las animaciones de composición proporcionan una forma eficaz y eficiente para ejecutar animaciones en tu interfaz de usuario de la aplicación. Se han diseñado desde cero para garantizar que las animaciones se ejecuten a 60 FPS independientemente del subproceso de la interfaz de usuario, así como para proporcionarte flexibilidad para crear experiencias increíbles no solo con tiempo, sino también con la entrada y otras propiedades, para controlar las animaciones.

## <a name="motion-in-windows"></a>Movimiento en Windows

Piensa en un diseño de movimiento similar al de una película. Las transiciones sin interrupciones te ayudan a concentrarte en la historia y a darle vida a tus experiencias. Podemos llevar esa sensación a nuestros diseños, llevando a los usuarios de una tarea a la siguiente con una calidad cinematográfica. Movimiento a menudo es el factor diferenciador entre una interfaz de usuario y una experiencia de usuario.

Como un bloque de creación fundamental de la plataforma de interfaz de usuario de Windows, CompositionAnimations proporcionan una manera eficiente y eficaz para crear experiencias de movimiento en la interfaz de usuario de la aplicación. El motor de animación se ha diseñado desde cero para asegurarse de que el movimiento se ejecuta a 60 FPS, independiente del subproceso de interfaz de usuario. Estas animaciones están diseñadas para proporcionar la flexibilidad necesaria para crear experiencias innovadoras movimiento basadas en el tiempo, las entradas y otras propiedades.

### <a name="examples-of-motion"></a>Ejemplos de movimiento

Estos son algunos ejemplos de movimiento en una aplicación.

Aquí, una aplicación usa una animación conectada para animar la imagen de un elemento en la medida en que "continúa" para formar parte del encabezado de la página siguiente. El efecto te ayuda a mantener al usuario en contexto a través de la transición.

![Un ejemplo de animación conectada](images/animation/connected-animation-example.gif)

Aquí, un efecto visual de paralaje mueve distintos objetos a diferentes velocidades cuando la interfaz de usuario se desplaza o realiza un movimiento panorámico para crear una sensación de profundidad, perspectiva y movimiento.

![Ejemplo de efecto parallax con la lista y una imagen en segundo plano](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>Usar CompositionAnimations para crear el movimiento

Para generar el movimiento de la interfaz de usuario, los desarrolladores pueden acceder a las animaciones en XAML o en la capa Visual. Las animaciones en la capa Visual proporcionan a los desarrolladores con una serie de ventajas:

- Rendimiento: en lugar de la animación enlazado a subprocesos de interfaz de usuario tradicionales, las animaciones de la plataforma de interfaz de usuario de Windows funcionan en un subproceso independiente a 60 FPS, permitir experiencias de movimiento suave.
- : Modelo de plantillas de las animaciones en la capa de interfaz de usuario de Windows son plantillas, significado puede usar una animación única en varios objetos y modificar las propiedades o parámetros sin preocuparse de obstrucción anterior utiliza.
- Personalización: la capa de interfaz de usuario de Windows no sólo hace fácil crear la interfaz de usuario atractiva, pero con una amplia gama de tipos de animación, sea posible para crear nuevas e increíbles experiencias con un degradado de personalizaciones

Como desarrollador crear experiencias en la capa de interfaz de usuario de Windows, tendrá acceso a una serie de conceptos de animación para que sus diseños cobren vida. Puede usar cualquiera de estos conceptos para animar una propiedad o subchannel componente (si procede) de cualquier CompositionObject.

> [!NOTE]
> No todas las propiedades de CompositionObject son que se pueden animar. Consulte la documentación de la CompositionObject individual para determinar si es que se pueden animar una propiedad.

> [!NOTE]
> El término _subcanal_ hace referencia a un formulario de componentes de una propiedad. Por ejemplo, la X o XY subchannel de una propiedad de desplazamiento Vector3.

| Concepto de animación | Descripción |
| ----------------- | ----------- |
| [Movimiento de duración definida con KeyFrameAnimations](time-animations.md)  | KeyFrameAnimations sirven para controlar directamente la totalidad de una experiencia de movimiento en un período de tiempo. Desarrolladores que describe un movimiento inicio, end, interpolación entre y duración de forma tradicional de fotogramas clave. |
| [Movimiento relativo con ExpressionAnimations](relation-animations.md)  | ExpressionAnimations se utilizan para describir cómo un movimiento de una propiedad de objeto debe basarse en relación con la propiedad de otro objeto. Los desarrolladores definen una ecuación matemática que define la relación basada en referencias. |
| ImplicitAnimations | Estas animaciones están basados en desencadenador y se definen por separado de la lógica de aplicación principal. ImplicitAnimations se utilizan para describir cómo y cuándo se producen las animaciones como respuesta a los cambios de propiedad directa. |
| [Controlado por la entrada de movimiento con animaciones de entrada](input-driven-animations.md)  | Entrada animaciones abarca un conjunto de escenarios que permiten a los desarrolladores describir movimiento basado en la manipulación a través de toque u otras modalidades de entrada. Estas animaciones controladas basándose en proporcionados por el usuario activo o de gestos. |
| [Movimiento basado en física con NaturalMotionAnimations](natural-animations.md)  | NaturalMotionAnimations se utilizan para describir el movimiento natural y familiar, forzar el movimiento controlado por experiencias basadas en el mundo real. En lugar de definir el tiempo, los desarrolladores definen características del movimiento (por ejemplo, la amortiguación proporción para Springs) |