---
title: Configuración de la búsqueda Contextual
description: Obtenga información sobre cómo configurar la búsqueda contextual para etiquetar los clips de juegos y las difusiones.
ms.assetid: 6cb2cb10-811a-4b20-9b9b-a3fc59a033c2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, configuración del servicio de búsqueda contextual, clip de juego, de difusión
ms.localizationpriority: medium
ms.openlocfilehash: c8c78f115a160c82a28881a3c551a958cdf4b68c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607930"
---
# <a name="configuring-contextual-search"></a>Configuración de la búsqueda Contextual

## <a name="configuration-info"></a>Información de configuración

### <a name="designing-your-data"></a>Diseño de los datos
Al configurar el título para la búsqueda Contextual, es importante pensar acerca de las cosas que hacer que sea el título de la única.  ¿Si un usuario está buscando el contenido de vídeo, lo que sería la parte superior de la cuenta para buscar?  ¿Nivel de granularidad necesitan los datos sea?

Búsqueda contextual admite la capacidad de filtrar y ordenar contenido, por lo que los metadatos deben reflejar estas construcciones de búsqueda para asegurarse de encontrar una gran cobertura.  Una buena reglas general es pensar en los datos de su juego en dos ejes:
1. Juegos de estado - p. ej.  Nivel de carácter, arma, mapa, rol, el punto de punto de control o un artículo, DLC
2. Habilidades de usuario - p. ej. Proporción de K/D, prestigio, Rank, velocidad de tiempo de ejecución

El primer pivot es muy útil para filtrar el contenido, el segundo es excelente para la ordenación.  Además, según el tipo de contenido que aparece, diferentes niveles de granularidad stat son importantes.  Para las difusiones, suponiendo que no hay un número limitado de difusiones activos en un momento dado, los datos de granularidad inferior garantizará una mayor probabilidad de una búsqueda con éxito.  Los clips de juegos, aumenta el número continuamente, por lo que los datos de granularidad más fino garantizará resultados más precisos.

Es bastante probable que ya dispone de estos datos creados para características de Xbox Live, como la presencia de enriquecido o estadísticas Hero, por lo que esto es una excelente oportunidad para aprovechar ya completado el trabajo.

Si utiliza estadísticas para filtrar el contenido, tenga en cuenta que todas las estadísticas de búsqueda contextuales están disponibles para consultar en cualquier momento.  Debe diseñar sus eventos en consecuencia para admitir esto.

Por ejemplo, si usas estadísticas, tales como SinglePlayerMap y MultiplayerMap para filtrar el contenido, el Reproductor sólo va a estar en uno de ellos a la vez.  Sin embargo, ambos valores estarán disponibles para consultar desde el servicio en cualquier momento.  Es importante que a medida que establezca uno, desactive la otra.  Para las estadísticas en función de cadena, una cadena vacía es excelente (asegúrese de no incluir los datos en la configuración de la interfaz de usuario como una opción).

### <a name="configuring-a-stat-for-contextual-search"></a>Configuración de una estadística para la búsqueda Contextual
Configurar el título para la búsqueda Contextual es fácil una vez que haya configurado los eventos y estadísticas que impulsan el etiquetado.  Vea otro XDP o centro de partners de documentación sobre cómo configurar la búsqueda Contextual si ya no está familiarizado.

![](../images/contextual_search/config02.png)

La imagen anterior es un ejemplo de la página principal después de hacer clic en el icono de búsqueda Contextual en la parte de configuración de servicio principal de XDP.

Incluso si se trata de la primera vez que ha visitado esta página, es posible que vea las estadísticas que ya ha configurado para la búsqueda Contextual.  Esto es porque todas las estadísticas de imagen prominente se configuran automáticamente para la búsqueda Contextual. No es necesario mantener estas estadísticas héroe en la configuración, pero son generalmente excelente para la ordenación (y ya listo para usted).

Actualmente, el número máximo de instancias stat que puede configurar para la búsqueda Contextual es 10.

En esta página, cada una de las estadísticas se representa en la siguiente manera: Prioridad: nombre para mostrar (nombre de instancia Stat)

Cada una de ellas se explicará con más detalle en la sección siguiente.

![](../images/contextual_search/config01.png)

La imagen anterior es la pantalla que aparecerá si opta por crear una nueva búsqueda Contextual stat o editar y uno ya existente.

Para configurar correctamente una estadística para la búsqueda Contextual, complete los pasos siguientes:
1. Elija la instancia stat.

  ![](../images/contextual_search/config03.png)

  Tenga en cuenta que stat solo se admiten instancias - stat plantillas no se aceptan.  También debe tener en cuenta la visibilidad que se haya establecido para la instancia stat (configurada en la parte de las estadísticas de XDP o centro de partners).  Solo las estadísticas que están marcadas como **abierto** aparecerá en las experiencias de otros fabricantes.

2. Elija su prioridad stat. Esta es una manera para delinear la importancia de este estado en relación con otros usuarios para experiencias de búsqueda y los algoritmos.  Los valores aceptables son 1 a 10 (1 es el más alto).  Dejar como 0 o en blanco para que esto se omiten.
3. Agregue su nombre para mostrar.  Se trata de una cadena localizable que se expone al usuario final.
4. Compruebe si se usará este estado para filtrar u ordenar los resultados.  Puede comprobar ambos en determinados casos (si el valor de la estadística es un número - preferiblemente en un intervalo definido por).
5. Elija cómo se representa el valor stat.  En el caso tiene 3 opciones.
   * Valor sin formato - la estadística se representa como está y no tiene ningún requisito de intervalo.  Esto se utiliza mejor para la ordenación.
   * Set - los valores se asignan a cadenas localizables individuales.  Esto se utiliza mejor para el filtrado.
   * Intervalo de - los valores que se encuentran dentro de un mínimo y máximo del intervalo.  Este tipo es fantástico si desea filtrar y ordenar desde estos datos.

#### <a name="explaining-sets"></a>Explicación de conjuntos
Si no está familiarizado con los conjuntos, son una forma de asignar los posibles valores stat a cadenas localizables que se pueden exponer a un usuario final.  Son muy importantes para las estadísticas de la búsqueda Contextual que desea filtrar con.

{% sin procesar %} Como un ejemplo, supongamos que tengo un número entero denominado Stat SinglePlayerMap.  Que muestra una serie de números a los usuarios finales no tiene sentido.  Por lo que en su lugar, creo un conjunto de asignaciones como ```{{0, “Blood Gulch”}, {1, “Lockout”}, {2, “Zanzibar”}}```.  El usuario verá la cadena, pero se usa el número como parte de la consulta de búsqueda de búsqueda Contextual.  Puede asignar números enteros o cadenas para cadenas localizadas en conjuntos.
{% endraw %}

### <a name="updating-your-contextual-search-document"></a>Actualizando el documento de la búsqueda Contextual
En este momento, puede cambiar fácilmente las estadísticas en el documento de la búsqueda Contextual como solo se admite la difusión en este momento (que es datos volátiles).  Una vez que comenzamos admitir juegos clip etiquetado (septiembre de 2015), si decide agregar o quitar estadísticas, se verán afectados los clips que ya se han indizado.  Si agrega un nuevo estado, clips sólo juegos y difusiones creadas después de que el cambio entra en vigor se etiquetan y aparece en las consultas que utilizan el estado nueva. Clips juegos antiguos no será posible para la consulta de este estado. Si quita un stat desde el documento de la búsqueda Contextual, el antiguo stat adjunta al clip juego será inútil y no se devolverá.
