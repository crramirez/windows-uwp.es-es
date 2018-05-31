---
author: Xansky
Description: Learn to develop accessible Windows 10 UWP apps that include keyboard navigation, color and contrast settings, and support for assistive technologies.
ms.assetid: 9311D23A-B340-42F0-BEFE-9261442AF108
title: Desarrollo de aplicaciones inclusivas de Windows10
label: Developing inclusive Windows 10 apps
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4a8bafb02baf4d70c41cd7a75c283903c05680b0
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
ms.locfileid: "1394304"
---
# <a name="developing-inclusive-windows-apps"></a>Desarrollo de aplicaciones inclusivas de Windows  

En este artículo se describe cómo desarrollar aplicaciones accesibles para la Plataforma universal de Windows (UWP). En concreto, se supone que conoces cómo diseñar la jerarquía lógica de la aplicación para tu aplicación. Aprende a desarrollar aplicaciones para UWP de Windows 10 accesibles que incluyan navegación por teclado, configuración de color y contraste y compatibilidad con tecnologías de asistencia.

Si no lo has hecho aún, empieza por leer [Diseño de software inclusivo](designing-inclusive-software.md).

Hay tres cosas que debes hacer para asegurarte de que tu aplicación sea accesible:

1. Exponer tus elementos de interfaz de usuario al [acceso mediante programación](#programmatic-access).
2. Asegurarte de que tu aplicación admita [navegación por teclado](#keyboard-navigation) para personas que no pueden usar un mouse o una pantalla táctil.
3. Asegurarte de que tu aplicación admita configuraciones de [color y contraste](#color-and-contrast) accesibles.

## <a name="programmatic-access"></a>Acceso mediante programación  
El acceso mediante programación es fundamental para la creación de accesibilidad en aplicaciones. Esto se logra estableciendo el nombre accesible (obligatorio) y la descripción accesible (opcional) para los elementos de la interfaz de usuario interactivos y de contenido de la aplicación. Esto garantiza que los controles de interfaz de usuario estén expuestos a la tecnología de asistencia (AT) tales como lectores de pantalla (por ejemplo, Narrador) o dispositivos de salida alternativa (tales como pantallas Braille). Sin acceso mediante programación, las API de tecnología de asistencia no pueden interpretar la información correctamente, impidiendo que el usuario pueda usar los productos lo suficiente o forzando a la AT a usar interfaces o técnicas de programación no documentadas que nunca fueron pensadas para usarse como una interfaz de accesibilidad. Cuando los controles de interfaz de usuario se exponen a la tecnología de asistencia, la AT es capaz de determinar qué acciones y opciones están disponibles para el usuario.  

Para obtener más información sobre cómo hacer que los elementos de interfaz de usuario de tu aplicación estén disponibles para las tecnologías de asistencia (AT), consulta el tema [Exponer la información de accesibilidad básica](basic-accessibility-information.md).

## <a name="keyboard-navigation"></a>Navegación por teclado  
Para los usuarios ciegos o con problemas de movilidad, es muy importante poder navegar por la interfaz de usuario con un teclado. Sin embargo, solo se debe dar foco del teclado a los controles de interfaz de usuario que requieren interacción del usuario para funcionar. Los componentes que no requieren una acción, tales como las imágenes estáticas, no necesitan foco del teclado.  

Es importante recordar que, a diferencia de la navegación con un mouse o entrada táctil, la navegación por teclado es lineal. Al considerar la navegación por teclado, piensa en cómo el usuario interactúa con tu producto y cuál será la lógica de navegación. En las culturas occidentales, las personas leen de izquierda a derecha y de arriba abajo. Por lo tanto, es una práctica habitual seguir este patrón en la navegación por teclado.  

Al diseñar la navegación por teclado, examina tu interfaz de usuario y piensa en estas preguntas:
* ¿Cómo están dispuestos o agrupados los controles en la interfaz de usuario?
* ¿Hay algunos grupos de controles significativos?
    * En caso afirmativo, ¿contienen esos grupos otro nivel de grupos?
*   Entre los controles del mismo nivel, ¿se debe navegar mediante tabulación, a través de una navegación especial (tales como las teclas de dirección) o ambas?

El objetivo es ayudar al usuario a entender cómo se dispone la interfaz de usuario e identificar los controles que requieren acción. Si encuentra que hay que hacer demasiadas tabulaciones antes de que el usuario complete el bucle de navegación, considere la posibilidad de agrupar controles relacionados. Algunos de los controles que están relacionados, tales como un control híbrido, pueden tener que abordarse en esta fase temprana de exploración. Después de comenzar a desarrollar tu producto, es difícil de rediseñar la navegación por teclado, por lo tanto, ¡planea cuidadosamente y durante una fase temprana!  

Para obtener más información sobre la navegación por teclado entre elementos de la interfaz de usuario, consulta [Accesibilidad del teclado](keyboard-accessibility.md).  

Además, el libro electrónico sobre [diseño de software para accesibilidad](https://www.microsoft.com/download/details.aspx?id=19262) tiene un excelente capítulo sobre este tema titulado _Designing the Logical Hierarchy_ (Diseño de la jerarquía lógica).

## <a name="color-and-contrast"></a>Color y contraste  
Una de las características de accesibilidad integradas en Windows es el modo de contraste alto, lo que aumenta el contraste de color del texto y las imágenes en la pantalla del equipo. Para algunas personas, aumentar el contraste de los colores reduce la fatiga ocular y facilita la lectura. Cuando compruebas tu interfaz de usuario en contraste alto, es buena idea comprobar que los controles se hayan codificado de forma coherente y con los colores del sistema (no con colores codificados de forma rígida) para garantizar que podrán ver todos los controles de la pantalla que vería un usuario que no está usando contraste alto.  

XAML
```xaml
<Button Background="{ThemeResource ButtonBackgroundThemeBrush}">OK</Button>
```
Para obtener más información sobre el uso de recursos y colores del sistema, consulte [Recursos de temas de XAML](../controls-and-patterns/xaml-theme-resources.md).

Hasta que no hayas invalidado los colores del sistema, una aplicación para UWP admite temas de contraste alto de manera predeterminada. Si un usuario elige que el sistema use un tema de contraste alto en la configuración del sistema o en las herramientas de accesibilidad, el marco usa automáticamente la configuración de colores y estilos que producen un diseño y una representación de contraste alto para los controles y los componentes de la interfaz de usuario.   

Para obtener más información, consulta [Temas de contraste alto](high-contrast-themes.md).  

Si decides usar tu propio tema de color en lugar de los colores del sistema, ten en cuenta estas directrices:  

**Relación de contraste de color:** La sección 508 actualizada de la Ley sobre Estadounidenses con Discapacidades (ADA), al igual que otras legislaciones, requieren que el contraste de color predeterminado entre el texto y su fondo sea de 5:1. Para el texto grande (tamaños de 18 puntos o de 14 puntos y en negrita), el contraste necesario predeterminado es de 3:1.  

**Combinaciones de colores:** Aproximadamente el 7% de los hombres (y menos del 1% de las mujeres) padecen alguna forma de daltonismo. Los usuarios con daltonismo tienen problemas para distinguir entre algunos colores, así que es importante que nunca se use el color por sí solo para transmitir el estado o el significado en una aplicación. Para obtener imágenes decorativas (tales como iconos o fondos), las combinaciones de colores se deberían elegirse de manera que se maximice la percepción de la imagen por parte de los usuarios daltónicos.  

## <a name="accessibility-checklist"></a>Lista de comprobación de accesibilidad  
La siguiente es una versión abreviada de la lista de comprobación de accesibilidad:

1. Establece el nombre accesible (obligatorio) y la descripción accesible (opcional) para los elementos de la interfaz de usuario interactivos y de contenido de la aplicación.
2. Implementa la accesibilidad de teclado.
3. Comprueba visualmente tu interfaz de usuario para asegurarte de que el contraste de texto sea suficiente, que los elementos se presenten correctamente en los temas de contraste alto y que los colores se usen correctamente.
4. Ejecuta herramientas de accesibilidad, soluciona problemas notificados y comprueba la experiencia de lectura de pantalla. (Consulta el tema sobre pruebas de accesibilidad).
5. Asegúrate de que la configuración del manifiesto de la aplicación siga las instrucciones de accesibilidad.
6. Declara que tu aplicación es accesible en Microsoft Store. (Consulta el tema [Accesibilidad en la tienda](accessibility-in-the-store.md).)

Para obtener más información, consulta el tema completo [Lista de comprobación de accesibilidad](accessibility-checklist.md).

## <a name="related-topics"></a>Temas relacionados  
* [Diseño de software inclusivo](designing-inclusive-software.md)  
* [Diseño inclusivo](http://design.microsoft.com/inclusive)
* [Procedimientos de accesibilidad que deben evitarse](practices-to-avoid.md)
* [Diseño de software para accesibilidad](https://www.microsoft.com/download/details.aspx?id=19262)
* [Centro de Microsoft para desarrolladores de accesibilidad](https://msdn.microsoft.com/enable)
* [Accesibilidad](accessibility.md)
