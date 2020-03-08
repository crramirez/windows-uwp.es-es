---
Description: El informe de revisiones del centro de Partners le permite ver las revisiones (comentarios) que los clientes escribieron al clasificar la aplicación en la tienda.
title: Informe Críticas
ms.assetid: E50C3A4D-1D8A-4E5B-8182-3FAD049F2A2D
ms.date: 08/16/2018
ms.topic: article
keywords: Windows 10, UWP, revisión, comentario, revisor
ms.localizationpriority: medium
ms.openlocfilehash: 7ec883e7bcb98d69673b520df918e085182d35ec
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853242"
---
# <a name="reviews-report"></a>Informe Críticas


El informe de **revisiones** del [centro de Partners](https://partner.microsoft.com/dashboard) le permite ver las revisiones (comentarios) que los clientes escribieron al clasificar la aplicación en la tienda.

Puede ver estos datos en el centro de Partners o [descargar el informe](download-analytic-reports.md) para verlo sin conexión. Como alternativa, puede recuperar estos datos mediante programación con el método [Get App Reviews](../monetize/get-app-reviews.md) en la [API de REST de Microsoft Store Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

También puede responder a las opiniones de los clientes [directamente desde esta página](respond-to-customer-reviews.md) o mediante programación [a través de la API de Microsoft Store Reviews](../monetize/submit-responses-to-app-reviews.md).

> [!TIP]
> Para obtener una vista rápida de las valoraciones, clasificaciones y comentarios de los usuarios de todas las aplicaciones en los últimos 30 días, expande **Interactuar** en el menú de navegación izquierdo y selecciona **Valoraciones y comentarios**. 


## <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la página, puedes seleccionar el período de tiempo durante el que quieres mostrar las valoraciones. La selección predeterminada es **Duración**, pero también puedes mostrar las valoraciones durante 30 días, 3 meses, 6 meses o 12 meses o durante un intervalo de fechas personalizado que especifiques.

Puedes expandir la opción **Filtros** para filtrar las valoraciones mostradas en esta página mediante las siguientes opciones. Estos filtros no se aplicarán a los gráficos **Desglose de clasificaciones** y **Clasificación promedio con el tiempo**.

-   **Clasificación**: de manera predeterminada se comprueban las valoraciones con clasificaciones por estrellas, pero puedes activar o desactivar clasificaciones específicas (de 1 a 5 estrellas) si quieres ver solo las valoraciones asociadas a una clasificación por estrellas particular.
- **Revisar contenido**: la configuración predeterminada es **clasificaciones con contenido de revisión**, lo que significa que solo se mostrarán las clasificaciones con contenido de revisión. Puede seleccionar **todo** para mostrar todas las clasificaciones, incluso las que no incluyen ningún texto de revisión escrita. Tenga en cuenta que el gráfico de **desglose de clasificaciones** siempre mostrará todas las revisiones, independientemente de la selección.
-   **Versión de SO**: el valor predeterminado es **Todos**. Puedes elegir un tipo de versión de SO específica si quieres que esta página solo muestre valoraciones aportadas por clientes que usen esa versión de SO.
-   **Versión del paquete**: el valor predeterminado es **Todas**. Si la aplicación incluye más de un paquete, aquí puedes elegir un paquete específico para mostrar solamente valoraciones realizadas por los clientes que tenían ese paquete cuando revisaron la aplicación.
-   **Respuestas**: el valor predeterminado es **Todas**. Puedes optar por filtrar las críticas para mostrar solamente aquellas donde has [respondido a los clientes](respond-to-customer-reviews.md), o solo aquellas donde aún ha no ha respondido.
-   **Actualizaciones**: el valor predeterminado es **Todas**. Puedes optar por filtrar las valoraciones para mostrar solamente las que ha actualizado el cliente desde que [respondiste a una valoración](respond-to-customer-reviews.md) o solo aquellas que el cliente aún no ha actualizado.
-   **Mercado**: el valor predeterminado es **Todos los mercados**. Puedes elegir un mercado específico si quieres que esta página solo muestre críticas de clientes en ese mercado.
-   **Tipo de dispositivo**: el filtro predeterminado es **Todos los dispositivos**. Puedes elegir un tipo de dispositivo específico si quieres que esta página solo muestre críticas aportadas por clientes que usen ese tipo de dispositivo.
-   **Nombre de la categoría**: el filtro predeterminado es **Todos**. Puede elegir una categoría de [información de revisión](#review-insight-categories) específica para mostrar solo las revisiones que hemos asociado a esa categoría. 

> [!TIP]
> Si no ves las valoraciones en la página, comprueba que los filtros no han excluido todas tus valoraciones. Por ejemplo, si filtras por un SO de destino que no es compatible con la aplicación, no verás ninguna crítica.


## <a name="ratings-breakdown"></a>Desglose de clasificaciones

El gráfico de **desglose de clasificaciones** aparece en la parte superior de este informe para que pueda ver rápidamente lo siguiente: 
- La clasificación promedio por estrellas de la aplicación.
- El número total de clasificaciones de la aplicación en los últimos 12 meses.
- El número total de clasificaciones para cada clasificación por estrellas.
- El número de clasificaciones para cada tipo de clasificación (nueva o revisada) por clasificación por estrellas en los últimos 12 meses.
 - **Clasificaciones nuevas** son clasificaciones que los clientes han enviado, pero que no han cambiado.
 - **Clasificaciones revisadas** son clasificaciones que el cliente ha cambiado de cualquier forma, incluso cambiar el texto de la valoración.

> [!TIP]
> La clasificación promedio que un cliente ve en la Tienda tiene en cuenta el mercado y el tipo de dispositivo del cliente, de modo que puede ser diferente de lo que aparece en este informe.

Tenga en cuenta que este gráfico siempre incluye todas las revisiones, aunque haya seleccionado **clasificación con revisar contenido** en el filtro página **revisar contenido** .

Este gráfico también se puede ver en el [Informe de clasificaciones](ratings-report.md), junto con más detalles sobre las clasificaciones de la aplicación.


<span id = "review-insight-categories" />

## <a name="insight-categories"></a>Categorías de información

En el gráfico de **categorías de información** se agrupan las revisiones en función de las categorías que se han determinado que se pueden asociar a la revisión.

> [!NOTE]
> Valoraciones con menos de 24 horas o en un idioma distinto del inglés no se incluyen al ver valoraciones por categorías.

Cerca de la parte superior de la página, verás los bloques de colores que representan valoraciones por categoría. Selecciona una de estas categorías para ver solo las valoraciones que hemos asociado a esa categoría. También puedes usar los [filtros de página](#apply-filters) para filtrar por categoría.

Para ver un desglose del número de valoraciones por categoría, selecciona **Mostrar detalles**. 


## <a name="reviews"></a>Valoraciones

Cada crítica del cliente contiene lo siguiente:

-   El título y el texto de la crítica proporcionados por el cliente. (Las críticas escritas por clientes en Windows Phone 8.1 y versiones anteriores no tendrán título).
-   La fecha de la crítica.
-   Nombre del revisor tal como aparece en el Microsoft Store.
-   El país o región del revisor.
-   La versión del paquete de la aplicación en el dispositivo del cliente en el momento en que se escribió la crítica. (Esta información no está disponible para las revisiones enviadas en línea o enviadas por los clientes en Windows 8.1 y versiones anteriores).
-   La versión del SO del dispositivo que estaba usando el cliente cuando se escribió la crítica.
-   El nombre del dispositivo que estaba usando el cliente cuando se escribió la crítica. (Esta información no está disponible para las revisiones enviadas en línea o enviadas por los clientes en Windows 8.1 y versiones anteriores).
-   El "recuento de utilidad" de la crítica, según las clasificaciones de otros clientes al leerla. Estas clasificaciones se muestran como una serie de dos números: el primer número muestra cuántos clientes la clasificaron como útil y el segundo número es el número total de clientes que clasificaron la crítica. Por ejemplo, un recuento de utilidad de 4/10 significa que de 10 clasificadores, 4 opinaron que la crítica era útil y 6, que no lo era. (Si no hay ningún voto de utilidad para una crítica, no se mostrará ningún recuento de utilidad.)

Ten en cuenta que los clientes pueden dejar una clasificación de la aplicación sin agregar comentarios, por lo que normalmente verás menos críticas que clasificaciones.

Puedes ordenar las críticas de la página por fecha o por clasificación en orden ascendente o descendente. Haga clic en el vínculo **ordenar por** para ver las opciones de ordenación por **fecha** y/o **clasificación**.

También puede usar el cuadro Buscar para buscar palabras o frases específicas en las revisiones de la aplicación. Tenga en cuenta que solo se busca en el texto de revisión original escrito por el cliente, incluso si la revisión se escribió en otro idioma. No se busca el texto de revisión traducido.

> [!NOTE]
> Puede que, ocasionalmente, observes que las valoraciones desaparecen de este informe. Esto puede ocurrir porque Microsoft quita opiniones de la Tienda que estén escritas por clientes que ejecutan ciertas compilaciones de Insider y versiones preliminares de Windows 10. Lo hacemos para reducir la posibilidad de que una opinión negativa que se deba a un problema en una compilación de una versión preliminar de Windows. También podremos eliminar opiniones de la Tienda que se hayan identificado como spam, inadecuadas, ofensivas o que infringían las directivas de otra forma. Esperamos que esta acción dé como resultado una mejor experiencia del cliente.


## <a name="translating-reviews"></a>Traducción de críticas

De manera predeterminada, se traducen las críticas que no se escribieron en tu idioma preferido. Si lo prefieres, la traducción de las críticas se puede deshabilitar desactivando la casilla **Traducir críticas** situada en la esquina superior derecha, encima de la lista de críticas.

Ten en cuenta que las críticas las traduce un sistema de traducción automática y que la traducción resultante no siempre será precisa. Se te proporciona el texto original por si quieres compararlo con la traducción o traducirlo por otros medios.

Como se indicó anteriormente, al buscar en las revisiones, solo se busca en el texto original que deja el cliente (y no en cualquier texto traducido), incluso si se activa la casilla **traducir revisiones** .


## <a name="responding-to-customer-reviews"></a>Responder a las críticas de los clientes

Puede usar el [centro de Partners](https://partner.microsoft.com/dashboard) o la [API de Microsoft Store Reviews](../monetize/submit-responses-to-app-reviews.md) para enviar respuestas a muchas de las revisiones de sus clientes. Para más información, consulta [Responder a las críticas de los clientes](respond-to-customer-reviews.md).

Estas son algunas acciones adicionales que puedes considerar según las críticas y clasificaciones que estás viendo.

-   Si notas que hay muchas revisiones que sugieren agregar o modificar una función, o que se quejan sobre un problema, piensa en lanzar una nueva versión que contemple lo indicado en comentarios específicos. (Asegúrate de actualizar la [descripción](create-app-descriptions.md) de tu aplicación para indicar que el problema se ha corregido).
-   Si el promedio de la clasificación es alto, pero el nombre de descargas es bajo, quizás quieras buscar maneras de [exponer la aplicación a más personas](attract-customers-and-promote-your-apps.md), dado que es bien recibida por los usuarios que la han probado.


 

 

 
