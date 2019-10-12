---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Animaciones de composición
description: Muchas de las propiedades de objetos y efectos de composición se pueden animar con animaciones de fotogramas y expresión clave, lo que permite a las propiedades de un elemento de la interfaz de usuario cambiar con el tiempo o según un cálculo concreto.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5991b5f9f3c63a25a7476cc35621488c6cb60b2c
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72281834"
---
# <a name="composition-animations"></a>Animaciones de composición

Las API de Windows.UI.Composition te permiten crear, animar, transformar y manipular objetos de compositor en una capa de API unificada. Las animaciones de composición proporcionan una forma eficaz y eficiente para ejecutar animaciones en tu interfaz de usuario de la aplicación. Se han diseñado desde cero para garantizar que las animaciones se ejecuten a 60 FPS independientemente del subproceso de la interfaz de usuario, así como para proporcionarte flexibilidad para crear experiencias increíbles no solo con tiempo, sino también con la entrada y otras propiedades, para controlar las animaciones.

## <a name="motion-in-windows"></a>Movimiento en Windows

Piensa en un diseño de movimiento similar al de una película. Las transiciones sin interrupciones te ayudan a concentrarte en la historia y a darle vida a tus experiencias. Podemos llevar esa sensación a nuestros diseños, llevando a los usuarios de una tarea a la siguiente con una calidad cinematográfica. El movimiento es a menudo el factor diferenciador entre una interfaz de usuario y una experiencia de usuario.

Como un bloque de creación fundamental de la plataforma de interfaz de usuario de Windows, CompositionAnimations proporciona una manera eficaz y eficaz de crear experiencias de movimiento en la interfaz de usuario de la aplicación. El motor de animación se ha diseñado desde el principio para asegurarse de que el movimiento se ejecuta a 60 FPS, independientemente del subproceso de la interfaz de usuario. Estas animaciones están diseñadas para proporcionar la flexibilidad necesaria para crear experiencias de movimiento innovadoras basadas en el tiempo, la entrada y otras propiedades.

### <a name="examples-of-motion"></a>Ejemplos de movimiento

Estos son algunos ejemplos de movimiento en una aplicación.

Aquí, una aplicación usa una animación conectada para animar la imagen de un elemento en la medida en que "continúa" para formar parte del encabezado de la página siguiente. El efecto te ayuda a mantener al usuario en contexto a través de la transición.

![Ejemplo de animación conectada](images/animation/connected-animation-example.gif)

Aquí, un efecto visual de paralaje mueve distintos objetos a diferentes velocidades cuando la interfaz de usuario se desplaza o realiza un movimiento panorámico para crear una sensación de profundidad, perspectiva y movimiento.

![Ejemplo de efecto parallax con la lista y una imagen en segundo plano](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>Uso de CompositionAnimations para crear movimiento

Para generar movimiento en la interfaz de usuario, los desarrolladores pueden tener acceso a animaciones en XAML o en la capa visual. Las animaciones en el nivel visual proporcionan a los desarrolladores una serie de ventajas:

- Rendimiento: en lugar de la animación tradicional enlazada a subprocesos de la interfaz de usuario, las animaciones de la plataforma de la interfaz de usuario de Windows operan en un subproceso independiente a 60 FPS, lo que permite experiencias de movimiento suave.
- Modelo de plantillas: las animaciones en la capa de la interfaz de usuario de Windows son plantillas, lo que significa que puede usar una sola animación en varios objetos y retocar propiedades o parámetros sin preocuparse por la obstrucción de usos anteriores.
- Personalización: el nivel de la interfaz de usuario de Windows no solo facilita la creación de una interfaz de usuario atractiva, sino que con una gama completa de tipos de animación, posible crear experiencias nuevas y increíbles con un degradado de personalizaciones

Como desarrollador que crea experiencias en el nivel de la interfaz de usuario de Windows, tiene acceso a diversos conceptos de animación para que los diseños cobren vida. Puede usar cualquiera de estos conceptos para animar una propiedad o un componente de subcanal (cuando proceda) de cualquier CompositionObject.

> [!NOTE]
> No todas las propiedades de un CompositionObject se pueden animar. Consulte la documentación de CompositionObject individuales para determinar si una propiedad se va a animar.

> [!NOTE]
> El término _subcanal_ hace referencia a una forma de componente de una propiedad. Por ejemplo, el subcanal X o XY de una propiedad de desplazamiento Vector3.

| Concepto de animación | Descripción |
| ----------------- | ----------- |
| [Movimiento basado en tiempo con KeyFrameAnimations](time-animations.md)  | KeyFrameAnimations se utilizan para controlar directamente la totalidad de una experiencia de movimiento durante un período de tiempo. Desarrolladores que describen el inicio, el final, la interpolación entre y la duración de un movimiento en un modo de fotogramas clave tradicional. |
| [Movimiento relativo con ExpressionAnimations](relation-animations.md)  | ExpressionAnimations se usan para describir cómo se debe controlar un movimiento de la propiedad de un objeto en relación con la propiedad de otro objeto. Los desarrolladores definen una ecuación matemática que define la relación basada en la referencia. |
| ImplicitAnimations | Estas animaciones se basan en desencadenadores y se definen por separado de la lógica de la aplicación principal. ImplicitAnimations se usan para describir cómo y cuándo se producen las animaciones como respuesta a los cambios de propiedad directos. |
| [Movimiento controlado por entrada con animaciones de entrada](input-driven-animations.md)  | Las animaciones de entrada cubren un conjunto de escenarios que permiten a los desarrolladores describir el movimiento basado en la manipulación a través de las modalidades de entrada táctil u otras. Estas animaciones se basan en movimientos o entradas de usuario activas. |
| [Movimiento basado en la física con NaturalMotionAnimations](natural-animations.md)  | NaturalMotionAnimations se usan para describir experiencias de movimiento naturales y familiares basadas en el movimiento controlado por la fuerza real. En lugar de definir el tiempo, los desarrolladores definen las características del movimiento (por ejemplo, la relación de amortiguación de los muelles) |