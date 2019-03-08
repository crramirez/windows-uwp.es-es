---
title: Parámetros de EDS
assetID: 9475b427-53bc-697b-6d24-1787320260b7
permalink: en-us/docs/xboxlive/rest/edsparameters.html
description: " Parámetros de EDS"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5412bcebc73dfd25d81b2073527e64e81de1bba4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655420"
---
# <a name="eds-parameters"></a>Parámetros de EDS

<a id="ID4EO"></a>


## <a name="query-string-parameters"></a>Parámetros de cadena de consulta

Estos parámetros de consulta no se aceptan necesariamente todos los [entretenimiento detección de servicios (EDS) API](../uri/marketplace/atoc-reference-marketplace.md), pero todos son aceptados por más de una API.

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| combinedContentRating| string| Opcional. Consulte [obtener (/media/ {marketplaceId} / contentRating)](../uri/marketplace/uri-medialocalecontentratingget.md).|
| continuationToken| string| Opcional. El token de continuación es un objeto binario opaco que contiene información que necesita el servicio para la paginación en determinados escenarios. Si se omite el valor, la primera página de resultados se devuelve (donde el tamaño de página viene determinado por el parámetro maxItems), junto con un token de continuación que puede utilizarse para obtener la segunda página de resultados. La segunda página podría contener el token de continuación para la tercera página de resultados y así sucesivamente.|
| deseado| string| Opcional. Consulte la API de nombre de campo combinado.|
| desiredMediaItemTypes| matriz de cadena| Opcional. Este parámetro determina los tipos de elementos que se devolverán en la respuesta.|
| Dominio| string| Opcional. El parámetro de dominio determina el contexto de marketplace de juego y la aplicación en el cliente que se llama para. De forma predeterminada, el dominio es "Modern", que indica que el cliente solo puede solicitar contenido Xbox One. Si el cliente desea cambiar el dominio para exponer el contenido de Xbox 360, debe especificar el dominio como "Xbox360". Actualmente, no se admiten resultados entre dominios. Los valores posibles son: <ul><li>Xbox360</li><li>Moderno</li></ul> El valor "Xbox360" solo se admite para singleMediaGroupSearch. Se admiten la exploración y los detalles. crossMediaGroupSearch no se admite y se devolverá un error 400.|
| campos| string| Opcional. Consulte [obtener (/media/ {marketplaceId} / campos)](../uri/marketplace/uri-medialocalefieldsget.md).|
| firstPartyOnly| Valor booleano| Parámetro de filtro opcional. Determina si el contenido de terceros solo se devuelve o si se devuelven ambos primero y terceros contenido de la consulta. |
| freeOnly| Valor booleano| Parámetro de filtro opcional. Restringe los resultados para liberar solo el contenido.|
| GroupBy| TK| Se usa el parámetro groupBy para ayudar a clasificar el conjunto de resultados en grupos, en lugar de un conjunto de resultados único. Si se especifica este parámetro, se modificará el conjunto de resultados para devolver varias listas de elementos, donde el número de elementos de cada depósito viene determinada por el parámetro de maxItems. <ul><li>MediaGroup - los resultados se agrupan por MediaGroup.</li></ul> |
| hasTrailer| Valor booleano| Parámetro de filtro opcional. Determina si los elementos devueltos deben contener un finalizador o si tiene un finalizador es opcional. Si el valor es true, todos los elementos deben tener finalizadores.|
| id| string| Opcional. Si se proporciona, restringe los resultados a sólo los elementos secundarios del elemento con el identificador especificado. Si se proporciona este parámetro, también debe especificarse MediaItemType. |
| Id.| matriz de cadena| Obligatorio. Todos los identificadores (hasta 10) para el que se devolverán los detalles. Tenga en cuenta que cualquier identificador que contiene caracteres no válidos para colocar en una dirección URL (los identificadores de tipo ProviderContentId son direcciones URL completas normalmente a sí mismos y, por tanto, contienen caracteres no válidos) debe codificarse para URL correctamente se envíen a EDS.|
| idType| string| Opcional. El tipo de los identificadores que se pasan al parámetro de identificadores. Los valores válidos se incluyen: <ul><li>Canónica (Bing/Marketplace)</li><li>XboxHexTitle (aplicación de reproducción en curso en la consola)</li></ul>  Todas se proporcionan que identificadores deben compartir el mismo Tipo_id. Si este valor se omite, se supone que todos los identificadores de ser Canonical.|
| latestOnly| Valor booleano| Parámetro de filtro opcional. Restringe los resultados a solo aquellos con la fecha de lanzamiento más reciente.|
| maxItems| entero de 32 bits con signo| Opcional. Determina el número máximo de elementos que se deben devolver en una llamada. Los valores válidos son números entre 1 y 25, ambos inclusive. El parámetro de valor predeterminado es 25 si se omite.|
| mediaGroup| string| Opcional. El grupo de medios de los identificadores. Todas se proporcionan que identificadores deben compartir el mismo grupo de medios.|
| MediaItemType| string| Opcional. El tipo del elemento cuyo identificador se especifica en el parámetro id. Si se proporciona el parámetro id, también se debe especificar este parámetro.|
| orderBy| string| Obligatorio. El parámetro orderBy determina cómo se deben ordenar los elementos que se devuelven. Los valores comunes para este campo se muestran aquí, pero algunas API puede admitir valores adicionales.<ul><li>playCountDaily - por número de veces que reproduce multimedia, para el día más reciente.</li><li>freeAndPaidCountDaily - por número de compras gratuitos y de pagadas, para el día más reciente.</li><li>paidCountAllTime - por recuento de las compras pagadas únicas de todo el tiempo.</li><li>paidCountDaily - por número de pago compras, para el día más reciente.</li><li>digitalReleaseDate - por fecha disponible para su descarga.</li><li>releaseDate - por fecha disponible en los almacenes, revirtiendo a la fecha de lanzamiento digital (si está disponible).</li><li>userRatings - por clasificaciones de usuario medio.</li></ul> |
| preferredProvider| string| Opcional. Si el usuario tiene un proveedor de contenido preferido, como Comcast Xfinity o Verizon FIOS, se puede pasar Id. de dicho proveedor. Mientras que el orden real de los elementos no cambiará, para cada elemento, se mostrará información del proveedor especificado en la parte superior de la lista de proveedores (si el proveedor de contenido preferido tiene el elemento está disponible).|
| q| string| Obligatorio. Término que se usa en la búsqueda de consulta.|
| queryRefiners| matriz de cadena| Opcional. Ver la lista de [EDS consulta refinadores](edsqueryrefiners.md).|
| relación| string| Opcional. Filtro que utiliza el parámetro ID como base para buscar otros productos que coinciden con el tipo de relación especificado: <ul><li>bundledWith - productos de agrupación de búsqueda donde el parámetro ID se incluye como parte de esos paquetes.</li><li>bundledProducts - buscar los productos que se incluyen en el paquete especificado por el parámetro de identificador.</li></ul>  Con este parámetro, se devolverá solo los productos que están visibles en el marketplace (pueden devolverse en una llamada de exploración). Si un paquete tiene un producto oculto, sigue siendo parte de la agrupación pero no se devuelve en estos resultados.|
  | ScopeId | string | Este parámetro se usa en escenarios de búsqueda inversa para multimedia de vídeo. |
  | ScopeIdType | string | Este parámetro se usa en escenarios de búsqueda inversa para multimedia de vídeo. Posibles valores: Título. |
  | skipItems | entero de 32 bits con signo | Opcional. Para la paginación en escenarios entre grupos, el parámetro skipItems se utiliza para determinar cuántos elementos ya se han visto (y, por tanto, ¿qué elemento debe mostrarse en primer lugar en el conjunto de resultados). El valor está basado en 0, por lo que skipItems = 0 (o simplemente no suministra skipItems) comienza la recuperación al principio de la lista. skipItems = 3 podría omitir los tres primeros elementos en la lista y comenzar una recuperación del cuarto elemento. |
  | subscriptionLevel | matriz de cadena | Parámetro de filtro opcional. El parámetro subscriptionLevel determina que el tipo de suscripción, el usuario tiene (por ejemplo, si un usuario tiene una suscripción de pago o una suscripción gratuita). Los valores posibles son los siguientes. <ul><li>Gold: El usuario tiene una suscripción de pago</li><li>Silver: El usuario tiene una suscripción gratuita.</li></ul>  |
| TargetDevices| string| EDS proporciona la flexibilidad para filtrar ofrece para los dispositivos de destino. Las ofertas (ProviderContent o disponibilidad) devueltos para el elemento puede restringirse a un dispositivo de destino.|
| topRatedOnly| Valor booleano| Parámetro de filtro opcional. Restringe los resultados a solo el contenido mejor calificado.|

<a id="ID4EGEAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EIEAC"></a>


### <a name="parent"></a>Primario

[Referencia adicional](atoc-xboxlivews-reference-additional.md)


<a id="ID4EUEAC"></a>


### <a name="further-information"></a>Para obtener más información

[URI de Marketplace](../uri/marketplace/atoc-reference-marketplace.md)
