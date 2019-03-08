---
title: API de EDS auxiliares
assetID: 5729ab80-e88d-0190-fb61-bd0cc4f134f6
permalink: en-us/docs/xboxlive/rest/eds-apis.html
description: " API de EDS auxiliares"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3f2f729359f52b09879e7227ede71e238fe63801
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603240"
---
# <a name="auxiliary-eds-apis"></a>API de EDS auxiliares

Existen servicios de detección de entretenimiento varias (EDS) las API que directamente no proporcionan información sobre el contenido, pero proporcionan información general acerca de cómo usar el servicio o ayudar a los modelos de interfaz de usuario comunes de unidad.

<a id="ID4EQ"></a>


## <a name="auxiliary-apis"></a>API auxiliares

| API| URI| Descripción|
| --- | --- | --- |
| Valores de parámetro de la API| /{locale}/metadata| Enumeración de posibles valores de parámetros que se pueden utilizar en llamadas al servicio|
| Combina el generador de clasificación de contenido| /{locale}/contentRating| Crea un valor que se puede usar en otras API para el filtrado de contenido potencialmente ofensivo o explícito. Para obtener más información, consulte a continuación.|
| Generador de nombres de campo combinados| /{locale}/fields| Crea un valor que se puede usar en el detalle de la API para controlar qué campos se devuelven. Para obtener más información, consulte a continuación.|

<a id="ID4EBC"></a>


### <a name="api-parameter-values"></a>Valores de parámetro de la API

Esta API describe los parámetros que se pueden usar con el servicio. La información devuelta es utilizable por el cliente de la interfaz de usuario porque el texto localizado acompaña a cada parámetro.

Ninguna de las API siguientes acepta los parámetros de consulta.

| API| URI| Descripción|
| --- | --- | --- | --- | --- | --- |
| Tipos| /{locale}/metadata/mediaGroups| La lista completa de los grupos de medios|
| Tipos por grupo de medios de elemento de medios| /{locale}/metadata/mediaGroups/{mediaItemTypeGroup}/mediaItemTypes| Tipos incluidos en el grupo de medios dado de elemento de la lista de los medios.|
| Todos los tipos de elemento multimedia| /{locale}/metadata/mediaItemTypes| La lista completa de tipos de elemento multimedia|
| Campos por tipo de elemento multimedia| /{locale}/metadata/mediaItemTypes/{mediaItemType}/fields| La lista de campos de un tipo de elemento de medios|
| Consulta refinadores| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners| La lista de refinadores consulta compatible con el tipo de elemento de medios dado|
| Todos los valores de la matriz de consulta| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners/{queryRefiner}| Los valores de la matriz de consulta especificada para el medio de determinado tipo de elemento|
| Todos los valores de matriz secundaria de consulta| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners/{queryRefiner}/subQueryRefinerValues| La lista de valores secundario para un valor de matriz de consulta determinada (por ejemplo, "subgenres de un género determinado"). El valor de matriz de la consulta se pasa como un parámetro de cadena de consulta denominado "queryRefinerValue", que se hace para permitir que consulta los valores de matriz con los caracteres que se prohíbe en recursos de identificador URI que se pasará.|
| Ordenaciones| /{locale}/metadata/mediaItemTypes/{mediaItemType}/sortOrders| La lista de criterios de ordenación para el tipo de elemento de medios dado|

<a id="ID4EEF"></a>


### <a name="combined-content-rating-generator"></a>Combina el generador de clasificación de contenido

Aplicar controles parentales sobre el contenido que los niños pueden ver es una tarea complicada. No sólo cada tipo de elemento multimedia tiene su propio sistema de clasificación, pero los sistemas de clasificación pueden variar de un país a otro. Esto significa que hay distintas partes de datos que deben especificarse para filtrar correctamente todos los elementos.

En lugar de especificar todos los parámetros en todas las llamadas de API, esta API genera un valor para pasar parámetros combinedContentRating en otras API y comunicarse todavía la misma información. Esto está diseñado para facilitar las API y mantenimiento, como los varios parámetros pasados a esta API se contraen en un único valor reutilizable para las otras API.

Aunque finalmente pueden cambiar los valores exactos devueltos por esta API, debe cambiar con muy poca frecuencia (por ejemplo, entre las versiones de EDS) y, por tanto, podría almacenarse en caché durante largos períodos de tiempo. Cualquier API que acepte que un parámetro combinedContentRating proporcionará un mensaje de error descriptivo si el valor pasado no es válido, que es un valor que indica el autor de llamada simplemente necesita llamar a esta API para obtener un valor actualizado. Si una API acepta un parámetro combinedContentRating, pero no se proporciona uno, ningún filtrado de contenido se llevará a cabo según el control parental

> [!NOTE]
> Esto no significa que se devuelve solo "seguro" contenido--significa que se devuelve todo el contenido, incluido contenido potencialmente explícito).



<a id="ID4EWF"></a>


### <a name="combined-field-name"></a>Nombre del campo combinado

Las API de EDS, de forma predeterminada, devolver un conjunto muy pequeño mínimo de campos de cada elemento:

   * Tipo de elemento multimedia
   * Grupo de medios
   * Id
   * Nombre

Para obtener más información, las API de aceptan un parámetro de "campos" que especifica qué partes de datos adicionales se deben devolver. Porque hay muchos campos posibles, especificando su nombre en su totalidad para cada llamada de API sería en gran medida a la saturación de la solicitud. En su lugar, los nombres se pueden pasar a esta API, lo que generará un valor mucho más pequeño que se puede pasar a las otras API.

Para cualquier API que acepte este parámetro, el valor proporcionado debe ser el superconjunto de todos los campos en todos los tipos de elemento de medio especificado. No es posible especificar distintos conjuntos de campos para tipos de elemento de medios diferentes. Sin embargo, si un campo que se aplica a un elemento tipo de medio, pero no otra, es sólo aparecerán en el medio de tipos de elementos que existen datos (por ejemplo, si se incluye "AvatarBodyType" en la llamada a la API de nombre de campo combinados, AvatarItems solo contendrá el campo).

Los valores devueltos por esta API son altamente almacenable en caché, en realidad, no debería cambiar, excepto entre las implementaciones de EDS. Se recomienda que, si se desea el almacenamiento en caché, la memoria caché no durar más de la sesión del usuario.

Además de aceptar los nombres de campo en Sí, esta API acepta "all" como un valor válido. Esto generará un valor que contiene cada campo que es posible especificar. El valor "all" es probable que sólo se puede utilizar para el desarrollo, depuración y pruebas.

<a id="ID4ERG"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4ETG"></a>


##### <a name="parent"></a>Primario  

[Referencia adicional](atoc-xboxlivews-reference-additional.md)


<a id="ID4E6G"></a>


##### <a name="further-information"></a>Para obtener más información

[URI de Marketplace](../uri/marketplace/atoc-reference-marketplace.md)
