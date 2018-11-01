---
author: Xansky
Description: Learn about evolving inclusive design with Universal Windows Platform (UWP) apps for Windows 10.  Design and build inclusive software with accessibility in mind.
ms.assetid: A6393A57-53F2-4F06-89AF-0D806FD76DB0
title: Diseño de software inclusivo en Windows10
label: Designing inclusive software
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f2a28d2a401ae132772c3948b3c4299184d4f858
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5870311"
---
# <a name="designing-inclusive-software-for-windows-10"></a>Diseño de software inclusivo para Windows 10  

Aprende sobre la evolución del diseño inclusivo con aplicaciones de la Plataforma universal de Windows (UWP) para Windows 10.  Diseña y crea software inclusivo teniendo en cuenta la accesibilidad.

En Microsoft, estamos mejorando nuestros principios y prácticas de diseño. Estos informan cómo se ven, sienten, funcionan y comportan nuestras experiencias. Estamos elevando nuestra perspectiva.

Esta nueva filosofía de diseño se denomina diseño inclusivo. La idea consiste en diseñar software con todos los usuarios en mente desde el principio. Esto difiere de considerar la accesibilidad como una tecnología que se añade al final del proceso de desarrollo para satisfacer a los requerimientos de un pequeño grupo de usuarios.

"Definimos discapacidad como un desajuste entre las necesidades de la persona y las del servicio, producto o entorno que se ofrece. Cualquier persona puede experimentar una discapacidad. Es un rasgo humano común y no debe excluirse."  \- desde el vídeo [Inclusivo](https://www.microsoft.com/design/inclusive)  

El diseño inclusivo crea mejores productos para todos. Se trata de considerar la gama completa de diversidad humana. Considera las rampas que se encuentran ahora en la mayoría de las esquinas de las aceras. Claramente fueron pensadas para ser usadas por personas en sillas de ruedas. Pero ahora casi todo el mundo las usa, incluso personas con sillitas para bebés, ciclistas y patinadores en monopatín. Incluso los peatones usarán dichas rampas a menudo porque están ahí y mejoran la experiencia. El control remoto de la televisión podría considerarse una tecnología de asistencia (AT) para una persona con limitaciones físicas. E incluso hoy es prácticamente imposible comprar un televisor sin uno de ellos. Antes de que los niños aprendan a atarse los zapatos, pueden llevar zapatos sin cordones o de cierre fácil. Los zapatos que son fáciles de poner y quitar suelen preferirse en culturas donde los zapatos se quitan antes de entrar en una casa. También son mejores para aquellas personas con problemas de habilidad, como artritis o una fractura de muñeca.

## <a name="inclusive-design-principles"></a>Principios de diseño inclusivo  
Los siguientes 4 principios están impulsando el cambio de Microsoft hacia el diseño inclusivo:

**Piensa de forma universal**: Nos centramos en lo que unifica a las personas: motivaciones, relaciones y capacidades humanas. Esto nos lleva a tener en cuenta el impacto social más amplio de nuestro trabajo. El resultado es una experiencia que tiene una diversidad de formas para que todas las personas participen.

**Haz que sea personal**: Después, nos retamos a crear conexiones emocionales. Las interacciones de persona a persona pueden inspirar una mejor interacción de la persona con la tecnología. Las circunstancias particulares de una persona pueden mejorar el diseño para todos los usuarios. El resultado es una experiencia que parece creada especialmente para una persona.

**Mantenlo simple**: Partimos de la simplicidad como el unificador fundamental. Cuando se reduce el desorden, las personas saben qué hacer a continuación. Están inspiradas a avanzar hacia espacios que sean limpios, livianos y abiertos. El resultado es una experiencia realista e intemporal.

**Crea deleite**: Las experiencias agradables provocan fascinación y descubrimiento. Algunas veces es mágico. Algunas veces es un detalle que es simplemente perfecto. Diseñamos estos momentos para que se sientan como un ansiado cambio de ritmo. El resultado es una experiencia que tiene impulso y flujo.

## <a name="inclusive-design-users"></a>Usuarios de diseño inclusivo  
Existen básicamente dos tipos de usuarios de la tecnología de asistencia (AT):

1. Aquellos que la necesitan, debido a discapacidades o dificultades, condiciones temporales (como la movilidad limitada a causa de un miembro roto) o relacionadas con la edad.  
2. Los que la usan por preferencia, para una experiencia informática más cómoda o conveniente.

La mayoría de los usuarios de PC (54 por ciento) son conscientes de la existencia de alguna forma de tecnología de asistencia y el 44 por ciento de dichos usuarios usan alguna forma de ella, pero muchos de ellos no usan AT que los beneficiaría (Forrester 2004).  

Un estudio de 2003-2004 encargado por Microsoft y llevado a cabo por Forrester Research encontró que más de la mitad (57 por ciento) de los usuarios de PC en los Estados Unidos entre los 18 y 64 años de edad podrían beneficiarse de la tecnología de asistencia. La mayoría de estos usuarios no se identificaron a sí mismos como discapacitados o con algún tipo de deficiencia, pero expresaron ciertas dificultades relacionadas con la tarea cuando usan un equipo. Forrester (2003) también encontró la siguiente proporción de usuarios con estas dificultades específicas: uno de cada cuatro experimenta dificultades visuales. Uno de cada cuatro experimenta dolor en las muñecas o manos. Uno de cada cinco experimenta dificultades auditivas.  

Además de las discapacidades permanentes, la gravedad y los tipos de dificultades que experimenta un individuo pueden variar a lo largo su vida. No existen las personas "normales". Nuestras capacidades siempre están cambiando. Margaret Meade dijo: "Todos somos únicos. Ser únicos nos hace iguales."  

Microsoft está comprometido con la investigación sobre las ciencias de la computación y la ingeniería de software con el objetivo de mejorar la experiencia informática y crear nuevas tecnologías informáticas. Consulta [Proyectos actuales de investigación y desarrollo de Microsoft](https://www.microsoft.com/enable/microsoft/research.aspx) dirigidos a hacer que el equipo sea más accesible y fácil de ver, escuchar e interactuar.  

## <a name="practical-design-steps"></a>Pasos prácticos de diseño  
Si estás preparado, esta sección es para ti. Se describen los pasos prácticos de diseño a tener en cuenta al implementar el diseño inclusivo para tu aplicación.  

### <a name="describe-the-target-audience"></a>Describir el público objetivo  
Define los usuarios potenciales de tu aplicación. Imagínate todas sus características y capacidades diferentes. Por ejemplo, la edad, sexo, idioma, sordera o problemas auditivos, impedimentos visuales, capacidades cognitivas, estilo de aprendizaje, restricciones de movilidad y así sucesivamente. ¿Satisface tu diseño sus necesidades individuales?  

### <a name="talk-to-actual-humans-with-specific-needs"></a>Habla con personas reales con necesidades específicas  
Reúnete con los usuarios potenciales que tengan distintas características. Asegúrate de considerar todas sus necesidades al diseñar tu aplicación. Por ejemplo, Microsoft descubrió que los usuarios sordos se desactivaban las notificaciones del sistema en sus consolas Xbox. Cuando se le preguntó a usuarios sordos reales sobre esto, nos dimos cuenta de que las notificaciones del sistema oscurecían una sección de subtítulos. La solución fue mostrar la notificación del sistema ligeramente más arriba en la pantalla. Esta fue una solución sencilla que no fue necesariamente evidente a partir de los datos de telemetría que inicialmente revelaron el comportamiento.  

### <a name="choose-a-development-framework-wisely"></a>Elige un entorno de desarrollo sabiamente  
En la fase de diseño, el entorno de desarrollo que usarás (es decir, UWP, Win32, web) es fundamental para el desarrollo del producto. Si tienes el lujo de poder elegir el entorno, piensa cuánto esfuerzo requerirá crear tus controles dentro de ese entorno. ¿Cuáles son las propiedades de accesibilidad integradas o predeterminadas que vienen con él? ¿Qué controles necesitarás personalizar? Al elegir tu entorno de desarrollo, esencialmente estás eligiendo la cantidad de controles de accesibilidad que obtendrás "gratuitamente" (es decir, qué proporción de los controles ya están integrados) y cuántos requerirán costos de desarrollo adicionales debido a las personalizaciones de los mismos.   

Usa los controles estándar de Windows siempre que sea posible. Estos controles ya están habilitados con la tecnología necesaria para interactuar con las tecnologías de asistencia.

### <a name="design-a-logical-hierarchy-for-your-controls"></a>Diseña una jerarquía lógica para tus controles  
Una vez que tienes tu entorno de desarrollo, diseña una jerarquía lógica para asignar tus controles. La jerarquía lógica de tu aplicación incluye la distribución y el orden de tabulación de los controles. Cuando los programas de tecnología de asistencia (AT), tales como los lectores de pantalla, leen tu interfaz de usuario, la presentación visual no es suficiente; debes proporcionar una alternativa de programación que tenga sentido estructural para los usuarios. Una jerarquía lógica puede ayudarte a hacerlo. Es una manera de estudiar la distribución de tu interfaz de usuario y estructurar cada elemento para que los usuarios puedan interpretarla. Principalmente se usa una jerarquía lógica:  

1.  Para proporcionar un contexto a los programas para el orden lógico (lectura) de los elementos de la interfaz de usuario  
2.  Para identificar claramente los límites entre los controles personalizados y los controles estándar en la interfaz de usuario  
3.  Para determinar cómo interactúan entre sí las partes de la interfaz de usuario  

Una jerarquía lógica es una excelente manera de solucionar posibles problemas de facilidad de uso. Si no puedes estructurar la interfaz de usuario de una manera relativamente sencilla, puede que tengas problemas con la facilidad de uso. Una representación lógica de un cuadro de diálogo sencillo no debe dar como resultado páginas de diagramas. En el caso de jerarquías lógicas que se vuelvan demasiado profundas o anchas, puede que tengas que volver a diseñar tu interfaz de usuario. Para obtener más información, descarga el libro electrónico sobre [diseño de software para accesibilidad](https://www.microsoft.com/download/details.aspx?id=19262).  

### <a name="design-appropriate-visual-ui-settings"></a>Diseña los ajustes de interfaz de usuario visuales adecuados  
Al diseñar la interfaz de usuario visual, asegúrate de que tu producto tenga una configuración de contraste alto, use las fuentes y opciones de suavizado predeterminadas del sistema, se escale correctamente según la configuración de puntos por pulgada (PPP) de la pantalla, tenga texto predeterminado con una relación de contraste de al menos 5:1 con el fondo y que tenga combinaciones de colores que resultarán fáciles de diferenciar a los usuarios con deficiencias en la percepción del color.  

#### <a name="high-contrast-setting"></a>Configuración de contraste alto  
Una de las características de accesibilidad integradas en Windows es el modo de contraste alto, el cual aumenta el contraste del color de texto e imágenes. Para algunas personas, aumentar el contraste de los colores reduce la fatiga ocular y facilita la lectura. Cuando inspeccionas tu interfaz de usuario en modo de contraste alto, es buena idea comprobar que los controles, tales como los vínculos, se hayan codificado de forma coherente y con los colores del sistema (no con colores codificados de forma rígida) para garantizar que podrán ver todos los controles de la pantalla que vería un usuario que no está usando contraste alto.  

#### <a name="system-font-settings"></a>Configuración de la fuente del sistema  
Para garantizar la legibilidad y minimizar cualquier distorsión inesperada en el texto, asegúrate de que tu producto se adhiera siempre a las fuentes predeterminadas del sistema y use las opciones de suavizado de contorno y suavizado. Si tu producto usa fuentes personalizadas, los usuarios pueden enfrentan distracciones y problemas de legibilidad significativos cuando personalicen la presentación de su interfaz de usuario (mediante el uso de un lector de pantalla o mediante distintos estilos de fuentes para ver tu interfaz de usuario, por ejemplo).  

#### <a name="high-dpi-resolutions"></a>Resoluciones con valores altos de PPP  
Para usuarios con dificultades visuales, es importante tener una interfaz de usuario escalable. Las interfaces de usuario que no se escalen correctamente en resoluciones con valores altos de puntos por pulgada (PPP) pueden hacer que los componentes importantes se superpongan o que tapen otros componentes y los hagan inaccesibles.  

#### <a name="color-contrast-ratio"></a>Relación de contraste de color  
La sección 508 actualizada de la Ley estadounidense sobre la discapacidad (ADA), al igual que otras legislaciones, requiere que el contraste de color predeterminado entre el texto y su fondo sea de 5:1. Para los textos grandes (tamaños de 18 puntos o de 14 puntos y en negrita) el contraste necesario predeterminado es de 3:1.  

#### <a name="color-combinations"></a>Combinaciones de colores  
Aproximadamente el 7 por ciento de los hombres (y menos del 1 por ciento de las mujeres) tienen alguna forma de daltonismo. Los usuarios con daltonismo tienen problemas para distinguir entre algunos colores, así que es importante que nunca se use el color por sí solo para transmitir el estado o el significado en una aplicación. Para obtener imágenes decorativas (tales como iconos o fondos), las combinaciones de colores se deberían elegirse de manera que se maximice la percepción de la imagen por parte de los usuarios daltónicos. Si diseñas usando estas recomendaciones de color desde el principio, tu aplicación ya estará dando pasos importantes hacia ser inclusiva.  

## <a name="summary-mdash-seven-steps-for-inclusive-design"></a>Resumen: siete pasos para un diseño inclusivo  
En resumen, sigue estos siete pasos para garantizar que tu software sea inclusivo.  
1.  Decide si un diseño inclusivo es un aspecto importante para tu software. Si lo es, aprende y aprecia cómo le permite a los usuarios reales vivir, trabajar y jugar, para ayudarte a guiar tu diseño.  
2.  A medida que diseñes soluciones para tus requisitos, usa los controles proporcionados por tu entorno de desarrollo (controles estándar) tanto como sea posible y evita los esfuerzos y costos innecesarios de los controles personalizados.  
3.  Diseña una jerarquía lógica para tu producto, tomando nota de dónde están los controles estándar, los controles personalizados y el foco del teclado en la interfaz de usuario.  
4.  Diseña configuraciones del sistema útiles (tales como la navegación por teclado, el contraste alto y los valores altos de PPP) en tu producto.  
5.  Implementa tu diseño mediante el [Centro de Microsoft para desarrolladores de accesibilidad](https://developer.microsoft.com/windows/accessible-apps) y la especificación de accesibilidad de tu marco como punto de referencia.  
6.  Prueba tu producto en usuarios con necesidades especiales para garantizar que podrán aprovechar las técnicas de diseño inclusivo implementadas en él.  
7.  Entrega tu producto final y documenta tu implementación para aquellas otras personas que trabajen en el proyecto después de ti.  

## <a name="related-topics"></a>Temas relacionados  
* [Diseño inclusivo](http://design.microsoft.com/inclusive)
* [Diseño de software para accesibilidad](https://www.microsoft.com/download/details.aspx?id=19262)
* [Centro de Microsoft para desarrolladores de accesibilidad](https://developer.microsoft.com/windows/accessible-apps)
* [Desarrollo de aplicaciones inclusivas de Windows](developing-inclusive-windows-apps.md) 
* [Accesibilidad](accessibility.md)
