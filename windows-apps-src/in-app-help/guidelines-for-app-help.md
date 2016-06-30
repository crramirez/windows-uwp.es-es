---
author: QuinnRadich
Description: "En estas directrices se describe cómo diseñar un contenido de la ayuda que resulte eficaz para una aplicación."
title: "Directrices para la ayuda de la aplicación"
label: Guidelines for app help
template: detail.hbs
ms.sourcegitcommit: 0aa2db498ab7ec25839da259dd0026b0a7cd2b13
ms.openlocfilehash: f2522afa91abe26303a85cbfbabd5ec5b3dba59c

---

# Directrices para la ayuda de la aplicación

\ [Actualizado para aplicaciones para la Plataforma universal de Windows (UWP) en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Las aplicaciones pueden ser complejas, por lo que proporcionar una ayuda eficaz a los usuarios puede mejorar enormemente su experiencia. No todas las aplicaciones necesitan proporcionar ayuda para los usuarios, y el tipo de ayuda que se proporciona puede variar mucho en función de la aplicación y de cómo la van a emplear los usuarios.

Si decides ofrecer ayuda, ten en cuenta las directrices que describimos en este tema. Y, recuerda: es preferible no ofrecer ninguna ayuda antes que proporcionar una que no sirva para nada.

## <span id="intuitive_design"></span><span id="INTUITIVE_DESIGN"></span>Diseño intuitivo

El contenido de la ayuda es muy útil, pero tu aplicación no puede depender de ella para proporcionar una buena experiencia para el usuario. Si alguien no puede detectar y usar inmediatamente las funciones fundamentales de la aplicación, esa persona no la usará. El contenido de la ayuda no cambiará esa primera impresión.

Un diseño intuitivo y fácil de usar es el primer paso para crear una ayuda útil. No solo mantendrá a los usuarios en la aplicación el tiempo suficiente como para que usen características más avanzadas, si no que, además, les proporcionará el conocimiento sobre las funciones principales de la aplicación, que podrán desarrollar a medida que la sigan usando.

## <span id="general_instructions"></span><span id="GENERAL_INSTRUCTIONS"></span>Instrucciones generales

Un usuario no buscará en el contenido de ayuda a menos que ya tenga un problema, de modo que la ayuda debe proporcionar una respuesta rápida y eficaz. Si no es útil de manera inmediata o es demasiado complicada, lo más probable es que los usuarios la pasen por alto.

Toda ayuda, independientemente del tipo, debe seguir estos principios básicos:

-   **Fácil de entender:** Una ayuda que confunda al usuario será peor que no ofrecer ninguna ayuda.

-   **Sencilla:** los usuarios que buscan ayuda quieren respuestas claras presentadas de manera directa.

-   **Pertinente:** Los usuarios no quieren ponerse a buscar el problema específico. Quieren que primero se presente la ayuda apropiada (esto se denomina "ayuda contextual") o una interfaz fácil de navegar.

-   **Directa:** Cuando un usuario busca ayuda, quiere ver algo que le ayude. Es decir, si la aplicación incluye páginas sobre informes de errores, cómo proporcionar comentarios, ver las condiciones del servicio o funciones similares, estas deben incluirse como enlaces o pies de página en la página de la ayuda principal y no como puntos importantes,

-   **Coherente:** Independientemente de su tipo, la ayuda forma parte de la aplicación y debe tratarse como cualquier otra parte de la interfaz de usuario. Los mismos principios de diseño, facilidad de uso, accesibilidad y estilo que se usan en el resto de la aplicación también deben estar presentes en la ayuda que se ofrece.

## <span id="types_of_help"></span><span id="TYPES_OF_HELP"></span>Tipos de ayuda

Hay tres categorías principales de ayuda, cada una con varios niveles y adecuada para objetivos distintos. Puedes usar la combinación más adecuada para las necesidades de tu aplicación.

### <span id="instructional_ui"></span><span id="INSTRUCTIONAL_UI"></span>Interfaz de usuario informativa

Lo ideal es que los usuarios puedan usar todas las funciones principales de la aplicación sin ninguna instrucción, pero, en ocasiones, la aplicación puede depender del uso de un gesto específico o puede contar con características secundarias que no sean obvias a primera vista. En estos casos, se debe usar una interfaz de usuario informativa para enseñar a los usuarios mediante indicaciones sobre cómo realizar tareas específicas.

[Consulta las directrices respecto a la interfaz de usuario informativa.](instructional-ui.md)

### <span id="in_app_help"></span><span id="IN_APP_HELP"></span>Ayuda desde la aplicación

El método estándar para presentar ayuda es mostrándola en la misma aplicación cuando el usuario lo solicite. Esto se puede implementar de varias maneras, como, por ejemplo, mediante páginas de ayuda o descripciones informativas. Este método es ideal para proporcionar ayuda de propósito general que responda directamente a preguntas de los usuarios sin complejidad.

[Consulta las directrices para la ayuda desde la aplicación](in-app-help.md)

### <span id="external_help"></span><span id="EXTERNAL_HELP"></span>Ayuda externa

Para obtener tutoriales detallados, funciones avanzadas o bibliotecas de temas de ayuda que son demasiado grandes para la aplicación, lo mejor es incluir vínculos a páginas web externas. Estos vínculos deben usarse con moderación si es posible, ya que alejan al usuario de la experiencia de la aplicación.

[Consulta las directrices para la ayuda externa.](external-help.md)

\[Este artículo contiene información específica sobre las aplicaciones para la Plataforma universal de Windows (UWP) y Windows 10. Para obtener instrucciones sobre Windows 8.1, descarga el [PDF sobre las directrices para Windows 8.1](https://go.microsoft.com/fwlink/p/?linkid=258743)\].



<!--HONumber=Jun16_HO4-->


