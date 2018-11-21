---
author: jwmsft
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Animaciones de composición
description: Muchas de las propiedades de objetos y efectos de composición se pueden animar con animaciones de fotogramas y expresión clave, lo que permite a las propiedades de un elemento de la interfaz de usuario cambiar con el tiempo o según un cálculo concreto.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 38f9d0daf230007d1d32a7d2187d54baa90986e5
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7428018"
---
# <a name="composition-animations"></a>Animaciones de composición

Las API de Windows.UI.Composition te permiten crear, animar, transformar y manipular objetos de compositor en una capa de API unificada. Las animaciones de composición proporcionan una forma eficaz y eficiente para ejecutar animaciones en tu interfaz de usuario de la aplicación. Se han diseñado desde cero para garantizar que las animaciones se ejecuten a 60 FPS independientemente del subproceso de la interfaz de usuario, así como para proporcionarte flexibilidad para crear experiencias increíbles no solo con tiempo, sino también con la entrada y otras propiedades, para controlar las animaciones.

## <a name="motion-in-windows"></a>Movimiento en Windows

Piensa en un diseño de movimiento similar al de una película. Las transiciones sin interrupciones te ayudan a concentrarte en la historia y a darle vida a tus experiencias. Podemos llevar esa sensación a nuestros diseños, los usuarios de una tarea a la siguiente con cinematográfica. A menudo, el movimiento es el factor diferenciador entre una interfaz de usuario y una experiencia de usuario.

Como un pilar fundamental de la plataforma de interfaz de usuario de Windows, CompositionAnimations proporciona una forma eficaz y eficiente para crear experiencias de movimiento en la interfaz de usuario de la aplicación. El motor de animación se ha diseñado desde el principio para garantizar que el movimiento se ejecuta a 60 FPS, independiente del subproceso de interfaz de usuario. Estas animaciones están diseñadas para proporcionar la flexibilidad para crear experiencias de movimiento innovadoras en función de tiempo, las entradas y otras propiedades.

### <a name="examples-of-motion"></a>Ejemplos de movimiento

Estos son algunos ejemplos de movimiento en una aplicación.

Aquí, una aplicación usa una animación conectada para animar la imagen de un elemento en la medida en que "continúa" para formar parte del encabezado de la página siguiente. El efecto te ayuda a mantener al usuario en contexto a través de la transición.

![Un ejemplo de animaciones conectadas](images/animation/connected-animation-example.gif)

Aquí, un efecto visual de paralaje mueve distintos objetos a diferentes velocidades cuando la interfaz de usuario se desplaza o realiza un movimiento panorámico para crear una sensación de profundidad, perspectiva y movimiento.

![Ejemplo de efecto de paralaje con la lista y una imagen en el fondo](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>Uso de CompositionAnimations para crear movimiento

Para generar el movimiento de la interfaz de usuario, los desarrolladores pueden acceder a las animaciones en XAML (vínculo a guiones gráficos aquí) o la capa Visual. Las animaciones en la capa Visual proporcionan a los desarrolladores con una serie de ventajas:

- Performance: en lugar de la animación de subproceso de interfaz de usuario enlazada tradicional, las animaciones en la plataforma de interfaz de usuario de Windows funcionan en un subproceso independiente a 60 FPS, la habilitación de experiencias de movimiento suave.
- Modelo de plantillas: las animaciones de la capa de interfaz de usuario de Windows son plantillas, significado puede utilizar una única animación en varios objetos y retocar las propiedades o utiliza parámetros sin preocuparse por obstruir anterior.
- Personalización: la capa de interfaz de usuario de Windows no solo facilita el proceso hacer que la interfaz de usuario atractivas, pero con una amplia gama de tipos de animación, sea posible para crear nuevas y magníficas experiencias con un degradado de personalizaciones

Como desarrollador de creación de experiencias en la capa de interfaz de usuario de Windows, tendrá acceso a una variedad de conceptos de animación para los diseños de dar vida a. Puedes usar cualquiera de estos conceptos para animar una propiedad o subchannel componente (si procede) de cualquier CompositionObject.

> [!NOTE]
> No todas las propiedades de un CompositionObject son que se pueden animar. Consulta la documentación del CompositionObject individual para determinar si es que se pueden animar una propiedad.

> [!NOTE]
> El término _subcanal_ hace referencia a un formulario de componentes de una propiedad. Por ejemplo, la X o XY subchannel de una propiedad Offset de tipo Vector3.

| Concepto de animación | Descripción |
| ----------------- | ----------- |
| [Movimiento basadas en tiempo con KeyFrameAnimations](time-animations.md)  | KeyFrameAnimations se usan para controlar directamente la totalidad de una experiencia de movimiento durante un período de tiempo. Desarrolladores que describe un movimiento inicio, fin, interpolación entre y duración en forma de fotogramas clave tradicionales. |
| [Movimiento relativo con ExpressionAnimations](relation-animations.md)  | Las ExpressionAnimations se usan para describir cómo debe se controladas por un movimiento de una propiedad de un objeto con respecto a la propiedad de otro objeto. Los desarrolladores definir una ecuación matemática que define la relación de referencia. |
| ImplicitAnimations | Estas animaciones están basados en desencadenador y se definen por separado de la lógica de la aplicación. ImplicitAnimations se usan para describir cómo y cuándo se producen animaciones como respuesta a los cambios de propiedad directo. |
| [Movimiento basado en entradas con animaciones de entrada](input-driven-animations.md)  | Animaciones de entrada describe un conjunto de escenarios que permiten a los desarrolladores describir el movimiento basado en la manipulación a través de la entrada táctil u otras modalidades de entrada. Estas animaciones son impulsadas basándose en entrada del usuario activo o gestos. |
| [Movimiento basada en física con NaturalMotionAnimations](natural-animations.md)  | NaturalMotionAnimations se usan para describir el movimiento natural y familiar movimiento controladas por forzar la experiencias basados en el mundo real. En lugar de definir el tiempo, los desarrolladores definir las características del movimiento (por ejemplo, dampingratio de los muelles) |