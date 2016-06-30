---
author: jnHs
Description: "El informe Críticas del panel del Centro de desarrollo de Windows te permite ver los comentarios que los clientes proporcionaron al clasificar tu aplicación en la Tienda."
title: "Informe Críticas"
ms.assetid: E50C3A4D-1D8A-4E5B-8182-3FAD049F2A2D
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 7d1a768ce558718b43a4d124f7c88868e999fb93

---

# Informe Críticas


El informe **Críticas** del panel del Centro de desarrollo de Windows te permite ver los comentarios que los clientes proporcionaron al clasificar tu aplicación en la Tienda. Puedes visualizar estos datos en tu panel o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión. Como alternativa, puedes recuperar mediante programación estos datos con la [API de REST de análisis de la Tienda Windows](../monetize/access-analytics-data-using-windows-store-services.md).

> **Nota** También puedes [responder a las críticas de los clientes](respond-to-customer-reviews.md) desde esta página.

Este informe muestra el número de estrellas que un cliente asignó a tu aplicación al escribir una crítica, pero no analiza las clasificaciones por estrellas en toda la aplicación; para obtener las estadísticas de las clasificaciones, consulta el [informe Críticas](ratings-report.md).

Ten en cuenta que los clientes pueden dejar una clasificación de la aplicación sin agregar comentarios, por lo que normalmente verás menos críticas que clasificaciones. De manera predeterminada, esta página también muestra las clasificaciones que no incluyen contenido de críticas pero puedes usar los **filtros de página** para mostrar solo las clasificaciones que incluyen críticas, como se describe a continuación.

Cada crítica del cliente contiene lo siguiente:

-   El título y el texto de la crítica proporcionados por el cliente. (Las críticas escritas por clientes en Windows Phone 8.1 y versiones anteriores no tendrán título).
-   La fecha de la crítica.
-   El nombre del revisor tal y como aparece en la Tienda Windows.
-   El país o región del revisor.
-   La versión del paquete de la aplicación en el dispositivo del cliente en el momento en que se escribió la crítica. (Esta información no está disponible para críticas enviadas en línea o enviadas por los clientes en Windows 8.1 y versiones anteriores).
-   La versión del SO del dispositivo que estaba usando el cliente cuando se escribió la crítica.
-   El nombre del dispositivo que estaba usando el cliente cuando se escribió la crítica. (Esta información no está disponible para críticas enviadas en línea o enviadas por los clientes en Windows 8.1 y versiones anteriores).
-   El "recuento de utilidad" de la crítica, según las clasificaciones de otros clientes al leerla. Estas clasificaciones se muestran como una serie de dos números: el primer número muestra cuántos clientes la clasificaron como útil y el segundo número es el número total de clientes que clasificaron la crítica. Por ejemplo, un recuento de utilidad de 4/10 significa que de 10 clasificadores, 4 opinaron que la crítica era útil y 6, que no lo era. (Si no hay ningún voto de utilidad para una crítica, no se mostrará ningún recuento de utilidad.)

## Aplicar filtros


Cerca de la parte superior de la página, puedes expandir **Aplicar filtros** para filtrar todos los datos de esta página.

>**Sugerencia** Si no ves ninguna crítica en la página, comprueba que los filtros no hayan excluido todas tus críticas. Por ejemplo, si filtras por un SO de destino que no es compatible con la aplicación, no verás ninguna crítica.

-   **Clasificación**: de manera predeterminada se comprueban todas las clasificaciones por estrellas, pero puedes activar o desactivar clasificaciones específicas (de 1 a 5 estrellas) si quieres ver solo las críticas asociadas a una clasificación por estrellas particular.
-   **Fecha**: el filtro predeterminado es **Últimos 30 días**, pero puedes ampliarlo hasta **Últimos 12 meses**.
-   **Contenido de la crítica**: el valor predeterminado es **Todos**, que incluye clasificaciones sin texto de crítica agregado. Puedes seleccionar **Clasificaciones con contenido de crítica** para mostrar solo las clasificaciones que incluyen contenido de crítica escrita.
-   **SO de destino**: el valor predeterminado es **Todos**. Puedes elegir un determinado sistema operativo de destino si quieres que esta página muestre solo clasificaciones de clientes con paquetes cuyo destino es el SO en cuestión.
-   **Respuestas**: el valor predeterminado es **Todas**. Puedes optar por filtrar las críticas para mostrar solamente aquellas donde has [respondido a los clientes](respond-to-customer-reviews.md), o solo aquellas donde aún ha no ha respondido.
-   **Actualizaciones**: el valor predeterminado es **Todas**. Puedes optar por filtrar las críticas para mostrar solamente las críticas que ha actualizado el cliente desde que [respondiste a una crítica](respond-to-customer-reviews.md) o solo aquellas que el cliente aún no ha actualizado.
-   **Mercado**: el valor predeterminado es **Todos los mercados**. Puedes elegir un mercado específico si quieres que esta página solo muestre críticas de clientes en ese mercado.
-   **Tipo de dispositivo**: el filtro predeterminado es **Todos los dispositivos**. Puedes elegir un tipo de dispositivo específico si quieres que esta página solo muestre críticas aportadas por clientes que usen ese tipo de dispositivo.
-   **Versión del paquete**: el filtro predeterminado es **Todos los paquetes**. Puedes elegir un paquete específico si deseas que esta página muestre solamente críticas realizadas por los clientes que tenían ese paquete cuando revisaron la aplicación.

La información contenida en todos los gráficos enumerados a continuación reflejará el período de tiempo seleccionado en la sección **Aplicar filtros** y todos los filtros que hayas elegido aquí.

> **Nota** La clasificación media que un cliente ve en la Tienda tiene en cuenta el mercado y el tipo de dispositivo del cliente, y considera las clasificaciones durante el último año. Por lo tanto, puede ser diferente de lo que aparece en este informe. Para ver cómo la clasificación media aparecerá en la Tienda para un cliente determinado, tendrás que aplicar filtros para seleccionar un mercado y tipo de dispositivo específicos, así como para establecer la opción **Fecha** en **Últimos 12 meses**.

## Traducción de críticas


De manera predeterminada, se traducen las críticas que no se escribieron en tu idioma preferido. Si lo prefieres, la traducción de las críticas se puede deshabilitar desactivando la casilla **Traducir críticas** situada en la esquina superior derecha, encima de la lista de críticas.

Ten en cuenta que las críticas las traduce un sistema de traducción automática y que la traducción resultante no siempre será precisa. Se te proporciona el texto original por si quieres compararlo con la traducción o traducirlo por otros medios.

## Ordenación de críticas


Puedes ordenar las críticas de la página por fecha o por clasificación en orden ascendente o descendente. Haz clic en el vínculo **Ordenar por** para ver las opciones para ordenar por fecha o clasificación. Al hacer clic en un botón de radio en la sección de Clasificación o Fecha, se aplicarán los criterios de ordenación y verás la etiqueta de ordenación junto al encabezado **Ordenar por**. Puedes quitar por completo el criterio de ordenación haciendo clic en la **X** que aparece en cada etiqueta.

## Responder a las críticas de los clientes


Puedes usar el panel del Centro de desarrollo de la Tienda Windows para enviar respuestas a muchas de las críticas de los clientes. Para más información, consulta [Responder a las críticas de los clientes](respond-to-customer-reviews.md).

Estas son algunas acciones adicionales que puedes considerar según las críticas y clasificaciones que estás viendo.

-   Si notas que hay muchas revisiones que sugieren agregar o modificar una función, o que se quejan sobre un problema, piensa en lanzar una nueva versión que contemple lo indicado en comentarios específicos. (Asegúrate de actualizar la [descripción](create-app-descriptions.md) de tu aplicación para indicar que el problema se ha corregido).
-   Si el promedio de la clasificación es alto, pero el nombre de descargas es bajo, quizás quieras buscar maneras de [exponer la aplicación a más personas](app-promotion-and-customer-engagement.md), dado que es bien recibida por los usuarios que la han probado.

> **Nota** Es probable que veas un número diferente de opiniones al comparar el informe **Críticas** del Centro de desarrollo de Windows con el informe Críticas de la aplicación móvil más antigua del Centro de desarrollo. Esto se debe a que la aplicación solo muestra los datos de las opiniones que dejaron los clientes en Windows Phone 8.1 y versiones anteriores. También puede ser que Microsoft haya eliminado opiniones de la Tienda Windows que se identificaron como spam, inadecuadas, ofensivas o que infringían las directivas de otra forma. Esperamos que esta acción dé como resultado una mejor experiencia del cliente.

 

 

 



<!--HONumber=Jun16_HO4-->


