---
Description: Buscar es una de las formas principales en que los usuarios pueden buscar contenido en tu aplicación. Las instrucciones de este artículo tratan los elementos de la experiencia de búsqueda, los ámbitos de búsqueda, la implementación y los ejemplos de la búsqueda en contexto.
title: Búsqueda y buscar en la página
ms.assetid: C328FAA3-F6AE-4970-8372-B413F1290C39
label: Search
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5c6eb22fbe0488fa9a36160ce9e704d10727e4c9
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "66364483"
---
# <a name="search-and-find-in-page"></a>Búsqueda y buscar en la página

 

Buscar es una de las formas principales en que los usuarios pueden buscar contenido en tu aplicación. Las instrucciones de este artículo tratan los elementos de la experiencia de búsqueda, los ámbitos de búsqueda, la implementación y los ejemplos de la búsqueda en contexto.

> **API importantes**: [Clase AutoSuggestBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox)

## <a name="elements-of-the-search-experience"></a>Elementos de la experiencia de búsqueda


**Entrada.**   Texto es el modo más común de la entrada de búsqueda y es el objetivo de estas instrucciones. Entre otros modos de entrada comunes se incluyen la voz y la cámara, pero estos por lo general requieren la capacidad de interactuar con el hardware del dispositivo y es posible que necesiten controles adicionales o una interfaz de usuario personalizada en la aplicación.

**Entrada cero.**   Una vez que el usuario ha activado el campo de entrada, pero antes de que haya escrito texto, puedes mostrar lo que se denomina un "lienzo de entrada cero". El lienzo de entrada cero aparecerá comúnmente en el lienzo de la aplicación para que [sugerencia automática](auto-suggest-box.md) sustituya este contenido cuando el usuario empiece a escribir su consulta. Historial de búsquedas recientes, tendencias de búsquedas, sugerencias de búsquedas contextuales, consejos y sugerencias son todos buenos candidatos para el estado de entrada cero.

![Ejemplo de Cortana en un lienzo de entrada cero](images/search-cortana-example.png)

 

**Formulación de consulta/sugerencia automática.**   Formulación de la consulta reemplaza el contenido de entrada cero en cuanto el usuario comienza a escribir. En cuando el usuario escribe una cadena de consulta, se le proporciona un conjunto de opciones de desambiguación o de sugerencias de consulta actualizadas continuamente que los ayudan a acelerar el proceso de entrada y a formular una consulta eficaz. Este comportamiento de sugerencias de consulta está integrado en el [control de sugerencias automáticas](auto-suggest-box.md)y también es una forma de mostrar el icono dentro de la búsqueda (como un micrófono o un icono de confirmación). Cualquier comportamiento distinto entra en la aplicación.

![ejemplo de consulta/formulación de sugerencia automática](images/search-autosuggest-example.png)

 

**Conjunto de resultados.**   Los resultados de búsqueda suelen aparecer directamente en el campo de entrada de búsqueda. Aunque esto no es un requisito, la yuxtaposición de la entrada y los resultados mantiene el contexto y proporciona al usuario acceso inmediato para editar la consulta anterior o escribir una consulta nueva. Esta conexión se puede comunicar reemplazando el texto de la sugerencia por la consulta que creó el conjunto de resultados.

Un método para habilitar el acceso eficaz tanto a la edición de consultas previas como a la creación de una nueva consulta es resaltar la consulta anterior cuando se reactiva el campo. De este modo, cualquier pulsación de tecla reemplazará a la cadena anterior, pero se mantiene la cadena de forma que el usuario pueda colocar un cursor para editar o anexar la cadena anterior.

El conjunto de resultados puede aparecer en la manera que mejor comunique el contenido. Una [vista de lista](lists.md) proporciona una gran cantidad de flexibilidad y es adecuada para la mayoría de las búsquedas. Una vista de cuadrícula funciona bien para imágenes u otros medios y un mapa puede usarse para comunicar la distribución espacial.

## <a name="search-scopes"></a>Ámbitos de búsqueda


La búsqueda es una función común y los usuarios encontrarán la interfaz de usuario de búsqueda en el shell y en muchas aplicaciones. Aunque los puntos de entrada de búsqueda tienden a visualizarse de modo similar, pueden proporcionar acceso a los resultados que van desde amplios (búsquedas web o de dispositivo) a restringidos (lista de contactos de un usuario). El punto de entrada de búsqueda debe yuxtaponerse al contenido que se está buscando.

Entre algunos ámbitos comunes de la búsqueda se incluyen:

**Global** y **contextual o refinar.**   Búsqueda en varias fuentes de contenido en la nube y local. Entre los resultados variados se incluyen las direcciones URL, documentos, medios, acciones, aplicaciones, etc.

**Web.**   Buscar en un índice web. Los resultados incluyen páginas, entidades y respuestas.

**Mis cosas.**   Búsqueda en dispositivos, en la nube, en gráficos sociales, etc. Los resultados son variados, pero están limitados por la conexión a cuentas de usuario.

Usa el texto de la sugerencia para comunicar el ámbito de búsqueda. Algunos ejemplos son:

"Buscar en la web y en Windows"

"Buscar en la lista de contactos"

"Buscar en el buzón"

"Configuración de búsqueda"

"Buscar un lugar"

![Ejemplo de texto de la sugerencia de búsqueda](images/search-windowsandweb.png)

 

Mediante una comunicación eficaz del ámbito de un punto de entrada de búsqueda, puedes ayudar a garantizar que se cumplirán las expectativas del usuario mediante las funcionalidades de la búsqueda que estás realizando y reducir la posibilidad de frustración a la hora de cumplir las expectativas del usuario.

## <a name="implementation"></a>Implementación


Para la mayoría de las aplicaciones, es mejor tener un campo de entrada de texto como punto de entrada de búsqueda, que proporciona una superficie visual destacada. Además, el texto de la sugerencia ayuda a detectar y comunicar el ámbito de búsqueda. Cuando la búsqueda es una acción más secundaria o cuando el espacio está limitado, el icono de búsqueda puede servir como punto de entrada sin el campo de entrada correspondiente. Cuando se visualiza como un icono, asegúrate de que hay espacio para un cuadro de búsqueda modal, como se muestra en los ejemplos a continuación.

Antes de hacer clic en el icono de búsqueda:

![Ejemplo de icono de búsqueda y cuadro de búsqueda contraído](images/search-icon-collapsed.png)

 

Después de hacer clic en el icono de búsqueda:

![Ejemplo de icono de búsqueda y cuadro de búsqueda expandido](images/search-icon-expanded.png)

 

La búsqueda siempre usa un glifo de lupa orientado hacia la derecha para el punto de entrada. El glifo que se debe usar es Segoe UI Symbol, el código de carácter hexadecimal 0xE0094, y normalmente con un tamaño de fuente de 15 epx.

El punto de entrada de la búsqueda se puede colocar en un número de diferentes áreas y su posición comunica el ámbito de búsqueda y el contexto. Las búsquedas que recopilan resultados de una experiencia o fuera de la aplicación se encuentran normalmente dentro de elementos visuales de la aplicación de nivel superior, como las barras de comandos globales o la navegación.

A medida que el ámbito de búsqueda se vuelve más estrecho o contextual, la colocación se asociará más directamente con el contenido a buscar, como en un lienzo, como un encabezado de lista o en barras de comandos contextuales. En todos los casos, la conexión entre la entrada de búsqueda y los resultados o el contenido filtrado debe quedar clara visualmente.

En el caso de las listas de desplazamiento, es útil tener siempre la entrada de búsqueda visible. Te recomendamos establecer la entrada de como permanente y desplazar el contenido detrás de ella.

La funcionalidad de entrada cero y de formulación de consulta es opcional para búsquedas contextuales o refinadas en las que se filtrará la lista en tiempo real mediante la entrada del usuario. Entre las excepciones se incluyen casos en los que puede haber sugerencias de formatos de consultas disponibles, como las opciones de filtrado de la bandeja de entrada (para: &lt;cadena de entrada&gt;, de: &lt;cadena de entrada&gt;, asunto: &lt;cadena de entrada&gt;, etc.).

## <a name="example"></a>Ejemplo


En los ejemplos de esta sección se muestran búsquedas en contexto.

Buscar como una acción en la barra de herramientas de Windows:

![Ejemplo de búsqueda como una acción en la barra de herramientas de Windows](images/search-toolbar-action.png)

 

Búsqueda como entrada en el lienzo de la aplicación:

![Ejemplo de búsqueda en un lienzo de la aplicación](images/search-canvas-contacts.png)

 

Buscar en un panel de navegación:

![Ejemplo de búsqueda en un menú de navegación](images/search-navmenu.png)

 

Búsqueda en línea se reserva para casos en los que se accede con poca frecuencia a la búsqueda o es muy contextual:

![Ejemplo de búsqueda en línea](images/patterns-search-results-desktop.png)


## <a name="guidelines-for-find-in-page"></a>Directrices sobre buscar en la página


Buscar en la página permite que los usuarios encuentren coincidencias de texto en el cuerpo de texto actual. Los lectores, visores y exploradores de documentos son las aplicaciones que ofrecen con mayor frecuencia Buscar en la página.

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar


-   Coloca una barra de comandos en la aplicación con la funcionalidad de buscar en la página para permitir que los usuarios puedan buscar texto en la página. Para obtener más detalles de colocación, consulta la sección Ejemplos.

    -   Las aplicaciones que proporcionan búsquedas en la página deben tener todos los controles necesarios en una barra de comandos.
    -   Si tu aplicación incluye muchas funcionalidades más allá de buscar en la página puedes proporcionar un botón **Buscar** en la barra de comandos de nivel superior como un punto de entrada a otra barra de comandos que contenga todos tus controles de buscar en la página.
    -   La barra de comandos de buscar en la página debe permanecer visible cuando el usuario interactúa con el teclado táctil. El teclado táctil aparece cuando un usuario pulsa en el cuadro de entrada. La barra de comandos de buscar en la página debe moverse hacia arriba para no quedar oculta tras el teclado táctil.

    -   Buscar en la página debe permanecer disponible cuando el usuario interactúa con la vista. Los usuarios tienen que interactuar con el texto a la vista mientras se usa buscar en la página. Por ejemplo, los usuarios probablemente quieran acercar o alejar un documento, o desplazarse lateralmente por la vista para leer el texto. Cuando el usuario comienza a usar buscar en la página, la barra de comandos debe permanecer disponible con un botón **Cerrar** para salir de buscar en la página.

    -   Habilita los métodos abreviados de teclado (CTRL+F). Implementa el método abreviado de teclado CTRL+F para permitir que el usuario invoque rápidamente la barra de comandos de buscar en la página.

    -   Incluye los conceptos básicos de la funcionalidad buscar en la página. Estos son los elementos de la interfaz de usuario que necesitas para poder implementar la búsqueda en la página:

        -   Cuadro de entrada
        -   Botones Anterior y Siguiente
        -   Un contador de coincidencias
        -   Cerrar (solo escritorio)
    -   La vista debe resaltar coincidencias y desplazarse para mostrar la siguiente coincidencia en pantalla. Los usuarios pueden moverse rápidamente por el documento mediante el uso de los botones **Anterior** y **Siguiente**, y mediante el uso de las barras de desplazamiento o la manipulación directa con la funcionalidad táctil.

    -   La funcionalidad de buscar y reemplazar debe funcionar conjuntamente con la funcionalidad básica de buscar en la página. Para las aplicaciones que tienen buscar y reemplazar, asegúrate de que buscar en la página no interfiera con dicha funcionalidad.

-   Incluye un contador de coincidencias para indicar al usuario cuántas coincidencias de texto hay en la página.
-   Habilita los métodos abreviados de teclado (CTRL+F).

## <a name="examples"></a>Ejemplos


Proporciona una manera sencilla de acceder a la función de búsqueda en la página. En este ejemplo de una interfaz de usuario móvil, "Buscar en la página" aparece después de dos comandos "Agregar a", en un menú desplegable:

![Ejemplo de Buscar en la página 1](images/findinpage-01.png)

 

Después de seleccionar Buscar en la página, el usuario escribe un término de búsqueda. Pueden aparecer sugerencias de texto mientras se escribe el término de búsqueda:

![Ejemplo de Buscar en la página 2](images/findinpage-02.png)

 

Si no hay una coincidencia de texto en la búsqueda, debería aparecer en el cuadro de texto una cadena del tipo "No hay resultados":

![Ejemplo de Buscar en la página 3](images/findinpage-03.png)

 

Si hay coincidencia de texto en la búsqueda, el primer término debería resaltarse en un color distinto y las coincidencias posteriores en un tono más sutil de la misma paleta de colores, como se muestra en este ejemplo:

![Ejemplo de Buscar en la página 4](images/findinpage-04.png)

 

Buscar en la página tiene un contador de coincidencias:

![Ejemplo de contador de coincidencias de Buscar en la página](images/findinpage-counter.png)




## <a name="implementing-find-in-page"></a>**Implementar la búsqueda en la página**

-   Los lectores, visores y exploradores de documentos son las aplicaciones que ofrecen con mayor frecuencia Buscar en la página y permiten al usuario disfrutar de una experiencia de lectura o visualización a pantalla completa.
-   La funcionalidad Buscar en la página es secundaria y debe ubicarse en una barra de comandos.

Para obtener más información sobre cómo agregar comandos a la barra de comandos, consulta [Barra de comandos](app-bars.md).

 


## <a name="related-articles"></a>Artículos relacionados

* [Cuadro de sugerencias automáticas](auto-suggest-box.md)


 

 
