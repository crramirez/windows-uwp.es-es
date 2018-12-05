---
ms.assetid: 9899F6A0-7EDD-4988-A76E-79D7C0C58126
title: Componentes de la Plataforma universal de Windows y optimización de la interoperabilidad
description: Crea aplicaciones para la Plataforma universal de Windows (UWP) que usen componentes UWP e interactúen con tipos administrados y nativos al mismo tiempo que evitan problemas de rendimiento de interoperabilidad.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 37bcf2ff6eee6c272339fdc997ee7bbb046f85e9
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8706793"
---
# <a name="uwp-components-and-optimizing-interop"></a>Componentes de UWP y optimización de la interoperabilidad


Crea aplicaciones para la Plataforma universal de Windows (UWP) que usen componentes UWP e interactúen con tipos administrados y nativos al mismo tiempo que evitan problemas de rendimiento de interoperabilidad.

## <a name="best-practices-for-interoperability-with-uwp-components"></a>Procedimientos recomendados de interoperabilidad con los componentes de UWP

Si no eres cuidadoso, el uso de componentes de UWP puede afectar considerablemente al rendimiento de tu aplicación. En esta sección se describe cómo conseguir un buen rendimiento cuando la aplicación usa componentes de UWP.

### <a name="introduction"></a>Introducción

La interoperabilidad puede afectar considerablemente al rendimiento y es posible que la estés usando sin darte cuenta. UWP controla gran parte de la interoperabilidad para que puedas ser más productivo y para que puedas volver a usar el código escrito en otros lenguajes. Te animamos a que aproveches lo que UWP hace por ti, pero ten en cuenta que puede afectar al rendimiento. En esta sección se describen algunos métodos que puedes usar para reducir el efecto de la interoperabilidad en el rendimiento de tu aplicación.

UWP cuenta con una biblioteca de tipos a los que se puede tener acceso en cualquier lenguaje en el que se pueda escribir una aplicación para UWP. Los tipos de UWP en C# o MicrosoftVisual Basic se usan del mismo modo que los objetos de .NET. No es necesario que realices llamadas de método de invocación de plataforma para tener acceso a los componentes de UWP. Esto facilita mucho la escritura de las aplicaciones, pero es importante que sepas que puede haber más interoperabilidad de la que esperas. Si un componente de UWP está escrito en un lenguaje diferente de C# o Visual Basic, al usar ese componente estás cruzando el límite de la interoperabilidad. Cruzar los límites de interoperabilidad puede afectar al rendimiento de las aplicaciones.

Cuando desarrollas una aplicación para UWP en C# o Visual Basic, los dos conjuntos más comunes de API que se usan son las API de UWP y las API de .NET para aplicaciones para UWP. En general, los tipos que se definen en la UWP están en espacios de nombres que comienzan con "Windows". Los tipos .NET están en espacios de nombres que empiezan con "System". Sin embargo, existen algunas excepciones. Los tipos de .NET para aplicaciones para UWP no necesitan interoperabilidad cuando están en uso. Si ves que el rendimiento es deficiente en un área que usa componentes de UWP, puedes usar las API de .NET para aplicaciones para UWP para mejorar el rendimiento.

**Nota**  la mayoría de los componentes de UWP que se incluyen con Windows 10 se implementa en C++, por lo que estás cruzando el límite de interoperabilidad cuando usas desde C# o Visual Basic. Como siempre, antes de dedicarte a modificar tu código, asegúrate de evaluar tu aplicación para averiguar si el uso de componentes de UWP afecta al rendimiento.

En este tema, cuando hablamos de "componentes de UWP", nos referimos a los componentes que se escriben en un lenguaje diferente de C# o Visual Basic.

 

Cada vez que tienes acceso a una propiedad o llamas a un método de un componente de UWP, incurres en un coste de interoperabilidad. De hecho, crear un objeto de UWP es más costeso que crear un objeto de .NET. Esto se debe a que UWP debe ejecutar código que realiza una transición del lenguaje de tu aplicación al lenguaje del componente. Además, si pasas datos al componente, estos deben convertirse entre tipos administrados y no administrados.

### <a name="using-uwp-components-efficiently"></a>Uso eficaz de los componentes de UWP

Si te das cuenta de que necesitas mejorar el rendimiento, puedes asegurarte de que tu código usa los componentes de UWP con la mayor eficacia posible. En esta sección se proporcionan algunas sugerencias para mejorar el rendimiento cuando usas componentes de UWP.

Hace falta una cantidad considerable de llamadas en un período breve para que se aprecien los efectos sobre el rendimiento. Una aplicación bien diseñada que encapsula llamadas a los componentes de UWP desde la lógica de negocios y otros códigos administrados no debería implicar un gran coste de interoperabilidad. Pero si tus pruebas indican que usar componentes de UWP afecta al rendimiento de la aplicación, las sugerencias que se describen en esta sección pueden ayudarte a mejorar el rendimiento.

### <a name="consider-using-net-for-uwp-apps"></a>Considera el uso de .NET para aplicaciones UWP

Existen determinados casos en los que puedes realizar una tarea mediante el uso de aplicaciones para UWP o .NET para UWP. Te aconsejamos que no trates de combinar tipos de .NET con tipos de UWP. Trata de mantenerte en uno de los dos tipos. Por ejemplo, puedes analizar un flujo de xml con el tipo [**Windows.Data.Xml.Dom.XmlDocument**](https://msdn.microsoft.com/library/windows/apps/BR206173) (un tipo de UWP) o con el tipo [**System.Xml.XmlReader**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.xmlreader.aspx) (un tipo de .NET). Usa la API que pertenezca a la misma tecnología que el flujo. Por ejemplo, si lees xml desde [**MemoryStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.memorystream.aspx), usa el tipo **System.Xml.XmlReader** porque los dos son tipos de .NET. Si lees desde un archivo, usa el tipo **Windows.Data.Xml.Dom.XmlDocument** porque las API del archivo y **XmlDocument** son componentes de UWP.

### <a name="copy-window-runtime-objects-to-net-types"></a>Copiar objetos de Windows Runtime a tipos de .NET

Cuando un componente de UWP devuelve un objeto de UWP, puede resultar útil copiar el objeto devuelto en un objeto de .NET. Cuando trabajas con colecciones y flujos, esto resulta especialmente importante.

Si llamas a una API de UWP que devuelve una colección y después guardas dicha colección y tienes acceso a ella varias veces, puede resultar útil copiarla en una colección de .NET y usar la versión de .NET de ahí en adelante.

### <a name="cache-the-results-of-calls-to-uwp-components-for-later-use"></a>Almacenar en la caché los resultados de las llamadas a los componentes de UWP para usarlos adelante

Es posible mejorar el rendimiento guardando valores en variables locales en lugar de tener acceso a un tipo de UWP varias veces. Esto puede ser particularmente beneficioso si usas un valor dentro de un bucle. Evalúa tu aplicación para ver si el uso de variables locales mejora su rendimiento. Usar valores en caché puede aumentar la velocidad de la aplicación, porque dedicará menos tiempo a la interoperabilidad.

### <a name="combine-calls-to-uwp-components"></a>Combinar llamadas en componentes de UWP

Trata de completar las tareas con la menor cantidad posible de llamadas a objetos de UWP. Por ejemplo, suele ser mejor leer una gran cantidad de datos desde un flujo que leer pequeñas cantidades de una vez.

Usa las API que agrupan trabajo en la menor cantidad posible de llamadas, en lugar de usar las API que realizan menos trabajo y necesitan más llamadas. Por ejemplo, crear un objeto llamando constructores que inicializan varias propiedades es preferible a llamar al constructor predeterminado y asignar propiedades una a una.

### <a name="building-a-uwp-components"></a>Compilar componentes de UWP

Si escribes un componente de UWP que puedan usar las aplicaciones escritas en C++ o JavaScript, asegúrate de que esté diseñado para lograr un buen rendimiento. Todas las sugerencias para lograr el buen rendimiento de las aplicaciones se aplican a los componentes. Evalúa tu componente para saber qué API tienen modelos de alta densidad de tráfico. Para esas áreas, considera proporcionar aquellas API que permitan que los usuarios realicen trabajo con pocas llamadas.

## <a name="keep-your-app-fast-when-you-use-interop-in-managed-code"></a>Mantener la rapidez de la aplicación al usar interoperabilidad en código administrado

UWP facilita la interoperabilidad entre el código administrado y nativo, pero si no eres cuidadoso puede verse afectado el rendimiento. Aquí te mostramos cómo obtener un buen rendimiento al usar la interoperabilidad en tus aplicaciones para UWP administradas.

UWP permite a los desarrolladores escribir aplicaciones con XAML en el lenguaje que prefieran gracias a las proyecciones de las API de UWP disponibles en cada lenguaje. Al escribir aplicaciones en C# o Visual Basic, esta comodidad implica un coste de interoperabilidad porque las API de UWP suelen implementarse en código nativo, mientras que las invocaciones de UWP desde C# o Visual Basic requiere que CLR se transforme de un marco de pila administrado en uno nativo y calcule las referencias de los parámetros de función en representaciones accesibles por el código nativo. Esta sobrecarga es mínima en la mayoría de las aplicaciones. Pero cuando se realizan muchas llamadas (cientos de miles a millones) a las API de UWP en la ruta crítica de una aplicación, este coste puede llegar a ser evidente. En general quieres garantizar que el tiempo invertido en la transición de idiomas sea escaso en relación con la ejecución del resto del código. Esto se muestra en el siguiente diagrama.

![Las transiciones de interoperabilidad no deben regir el tiempo de ejecución del programa.](images/interop-transitions.png)

Los tipos enumerados en [**.NET para las aplicaciones de Windows**](https://msdn.microsoft.com/library/windows/apps/xaml/br230232.aspx) no incurren en este costo de interoperabilidad cuando se usan desde C# o Visual Basic. Como regla general, puedes suponer que los tipos de espacios de nombres que empiezan con “Windows.” son parte de UWP y que los tipos de espacios de nombres que empiezan con “System.” son tipos de .NET. Ten en cuenta que hasta los usos más sencillos de los tipos de UWP, como la asignación o el acceso a la propiedad, conllevan un coste de interoperabilidad.

Debes medir tu aplicación y determinar si la interoperabilidad está consumiendo gran parte de su tiempo de ejecución antes de optimizar los costes. Cuando analizas el rendimiento de la aplicación con Visual Studio, puedes obtener con facilidad un límite máximo de costes de interoperabilidad usando la vista **Funciones** y viendo el tiempo transcurrido en métodos que llaman a UWP.

Si tu aplicación es lenta debido a una sobrecarga de interoperabilidad, para mejorar su rendimiento puedes reducir las llamadas a las API de UWP en las rutas de código activas. Por ejemplo, el motor de un juego que realiza una gran cantidad de cálculos de física al consultar constantemente la posición y las dimensiones de [**UIElements**](https://msdn.microsoft.com/library/windows/apps/BR208911) puede ahorrar mucho tiempo si almacena la información necesaria de **UIElements** en variables locales, realiza los cálculos en estos valores almacenados en caché y vuelve a asignar el resultado final a **UIElements** una vez acaba. Por poner otro ejemplo, si el código de C# o Visual Basic obtiene acceso de manera constante a una colección, es más eficaz usar una colección del espacio de nombres [**System.Collections**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.aspx) que otra del espacio de nombres [**Windows.Foundation.Collections**](https://msdn.microsoft.com/library/windows/apps/BR206657). También puedes considerar combinar llamadas a componentes de UWP; por ejemplo, al usar las API de [**Windows.Storage.BulkAccess**](https://msdn.microsoft.com/library/windows/apps/BR207676).

### <a name="building-a-uwp-component"></a>Compilar componentes de UWP

Si escribes un componente de UWP para usar en aplicaciones escritas en C++ o JavaScript, asegúrate de que esté diseñado para lograr un buen rendimiento. La superficie de API define el límite de interoperabilidad y define el grado hasta el que los usuarios deberán usar la guía de este tema. Si distribuyes componentes a otras partes, entonces este tema es especialmente importante.

Todas las sugerencias para lograr el buen rendimiento de las aplicaciones se aplican a los componentes. Evalúa tu componente para saber qué API tienen modelos de alta densidad de tráfico. Para esas áreas, considera proporcionar aquellas API que permitan que los usuarios realicen trabajo con pocas llamadas. Se ha invertido mucho esfuerzo para diseñar UWP para que las aplicaciones puedan usarla sin cruzar repetidas veces el límite de interoperabilidad.

 

