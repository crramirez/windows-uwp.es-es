---
Description: These guidelines describe how to design effective Help content for your app.
title: Directrices para la ayuda de la aplicación
label: Guidelines for app help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: c3e73f9b-4839-4804-b379-c95b0ca4fbe8
ms.localizationpriority: medium
ms.openlocfilehash: bd2174c6bbfb84a3ea6c6956e1d0b02ed5c9be33
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7977777"
---
# <a name="guidelines-for-app-help"></a>Directrices para la ayuda de la aplicación



Las aplicaciones pueden ser complejas, por lo que proporcionar una ayuda eficaz a los usuarios puede mejorar enormemente su experiencia. No todas las aplicaciones necesitan proporcionar ayuda para los usuarios y el tipo de ayuda puede variar mucho en función de la aplicación.

Si decides ofrecer ayuda, sigue estas directrices al crearla. Es preferible nu ofrecer ninguna antes que proporcionar una ayuda que no sirva para nada.

## <a name="intuitive-design"></a>Diseño intuitivo

El contenido de la ayuda es muy útil, pero tu aplicación no puede depender de ella para proporcionar una buena experiencia para el usuario. Si este no puede detectar y usar inmediatamente las funciones fundamentales de la aplicación, no la usará. No importa la cantidad o calidad de la ayuda que proporciones, no cambiarán esa primera impresión.

Un diseño intuitivo y fácil de usar es el primer paso para crear una ayuda útil. No solo mantendrá a los usuarios en la aplicación el tiempo suficiente como para que usen características más avanzadas, si no que además les proporcionará conocimientos sobre las funciones principales de la aplicación, que podrán desarrollar a medida que aprendan mediante su uso.

## <a name="general-instructions"></a>Instrucciones generales

Un usuario no buscará el contenido de ayuda a menos que ya tenga un problema, de modo que la ayuda debe proporcionar una respuesta rápida y eficaz. Si no es útil de manera inmediata o es demasiado complicada, lo más probable es que los usuarios la pasen por alto.

Toda ayuda, independientemente del tipo, debe seguir estos principios:

-   **Fácil de entender:** una ayuda que confunda al usuario será peor que no ofrecer ninguna ayuda.

-   **Sencilla:** los usuarios que buscan ayuda quieren respuestas claras presentadas de manera directa.

-   **Pertinente:** los usuarios no quieren ponerse a buscar el problema específico. Quieren que se les presente directamente la ayuda apropiada (esto se denomina "ayuda contextual") o una interfaz fácil de navegar.

-   **Directa:** cuando un usuario busca ayuda, quiere ver algo que le ayude. Es decir, si la aplicación incluye páginas de informes de errores, comentarios, condiciones del servicio o funciones similares, es adecuado que la ayuda contenga vínculos a esas páginas, pero deberían incluirse de manera adicional en la página principal de ayuda y no como elementos de igual o mayor importancia.

-   **Coherente:** independientemente de su tipo, la ayuda forma parte de la aplicación y debe tratarse como cualquier otra parte de la interfaz de usuario. Los mismos principios de diseño, facilidad de uso, accesibilidad y estilo que se usan en el resto de la aplicación también deben estar presentes en la ayuda que se ofrece.

## <a name="types-of-help"></a>Tipos de ayuda

Hay tres categorías principales de ayuda, cada una con varios niveles y adecuada para objetivos distintos. Usa la combinación más adecuada para las necesidades de tu aplicación.

#### <a name="instructional-ui"></a>UI informativa

Normalmente, los usuarios deberían poder usar todas las funciones principales de la aplicación sin instrucciones. Sin embargo, en ocasiones la aplicación dependerá del uso de gestos específicos o es posible que haya características secundarias de la aplicación que no sean perceptibles de manera inmediata. En estos casos, se debe usar la interfaz de usuario informativa para enseñar a los usuarios mediante instrucciones cómo realizar tareas específicas.

[Consulta las directrices para la interfaz de usuario informativa](instructional-ui.md)

#### <a name="in-app-help"></a>Ayuda desde la aplicación

El método estándar para presentar ayuda es mostrándola en la misma aplicación cuando el usuario lo solicite. Hay varias maneras para implementar esto, por ejemplo, en las páginas de ayuda o en descripciones informativas. Este método es ideal para obtener ayuda de propósito general, que responde directamente preguntas de los usuarios sin complejidad.

[Consulta las directrices para la ayuda desde la aplicación](in-app-help.md)

#### <a name="external-help"></a>Ayuda externa

Para obtener tutoriales detallados, funciones avanzadas o bibliotecas de temas de ayuda que son demasiado grandes para la aplicación, lo mejor es incluir vínculos a páginas web externas. Estos vínculos deben usarse con moderación si es posible, ya que alejan al usuario de la experiencia de la aplicación.

[Consulta las directrices para la ayuda externa](external-help.md)


