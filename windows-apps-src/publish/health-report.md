---
author: jnHs
Description: The Health report in Partner Center lets you get data related to the performance and quality of your app, including crashes and unresponsive events.
title: Informe Mantenimiento
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, mantenimiento, bloqueos, eventos que no responden, estado de la aplicación, datos de estado, seguimiento de la pila, archivo cab, error, errores, pdb, símbolos
ms.localizationpriority: medium
ms.openlocfilehash: 06cf6a7050f7598e86582393a92b92d1bdd877d1
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "6994771"
---
# <a name="health-report"></a>Informe Mantenimiento

El informe de **estado** en [El centro de partners](https://partner.microsoft.com/dashboard) te permite obtener los datos relacionados con el rendimiento y la calidad de la aplicación, incluidos los bloqueos y eventos que no responden. Puedes ver estos datos en el centro de partners, o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión. Si corresponde, puedes ver seguimientos de la pila o archivos CAB para una mayor depuración.

Como alternativa, puedes recuperar mediante programación los datos de este informe con la [API de REST de análisis de la Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md).


## <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la página, puedes seleccionar el período de tiempo durante el que quieres mostrar los datos. La selección predeterminada es **72H** (72 horas), pero puedes elegir **30D** en su lugar para mostrar los datos de los últimos 30 días. Ten en cuenta que los datos se muestran en la zona horaria local para la vista de **72 H** y en UTC para la vista **30D** .

También puedes expandir la opción **Filtros** para filtrar todos los datos de esta página por versión de paquete, mercado o tipo de dispositivo.

-   **Versión del paquete**: el valor predeterminado es **Todas**. Si la aplicación incluye más de un paquete, puedes elegir uno concreto aquí.
-   **Mercado**: el filtro predeterminado es **Todos los mercados**, pero puedes limitar los datos a uno o varios mercados.
-   **Tipo de dispositivo**: el valor predeterminado es **Todos**, pero también puedes mostrar los datos de un determinado tipo de dispositivo. Ten en cuenta que en la categoría **Otros** se incluyen dispositivos en los que se reconoce la marca o modelo, pero no podemos incluirla en una de las categorías predefinidas que se muestran en este filtro. Para estos dispositivos, el modelo de dispositivo puede verse en la sección **Registro de errores** del informe **Detalles del error**.  
-   **Versión de SO**: el valor predeterminado es **Todas las versiones de SO**, pero puedes elegir una versión específica de SO.
-   **Versión de SO**: el valor predeterminado es **Todas las versiones de SO**, pero puedes elegir una versión específica de la **Versión de SO** seleccionada.
-   **Espacio aislado**: el valor predeterminado es **Comercial**, pero para productos que usan varios espacios aislados de desarrollo (como los juegos que se integran con Xbox Live), puedes elegir uno específico aquí. (Si tu producto no usa espacios aislados, este filtro solo mostrará **Comercial** y no será aplicable.)
-   **Arquitectura**: el valor predeterminado es **Todas las arquitecturas**, pero puedes elegir un tipo de arquitectura de sistema específico. Este filtro solo está disponible cuando la opción **30D** está seleccionada.
-   **PRAID**: el ajuste predeterminado es **Todos**, pero si has definido varios id. de la aplicación relativos del paquete (PRAID) al crear el paquete de la aplicación, puedes elegir mostrar solo los datos relacionados con un PRAID. Este filtro no aparecerá si no se han definido varios PRAID.

La información de todos los gráficos que aparecen a continuación reflejará el intervalo de fechas y los filtros que hayas seleccionado. Algunas secciones también te permiten aplicar filtros adicionales.


## <a name="failure-hits"></a>Número de errores

El gráfico **Número de errores** muestra el número de bloqueos y eventos diarios que los clientes experimentaron al usar la aplicación durante el período de tiempo seleccionado. De cada tipo de evento que la aplicación experimentó se realiza un seguimiento por separado: bloqueos, cuelgues, excepciones de JavaScript y errores de memoria.

Cuando la **30D** se selecciona el período de tiempo, es posible que veas marcadores de círculo. Estos representan un aumento significativo o disminución un valor determinado que creemos que querrás saber sobre. La fecha en el que se muestra el círculo representa el final de la semana en el que hemos detectado un aumento significativo o una disminución en comparación con la semana anterior. Para ver más detalles sobre qué ha cambiado, mantén el puntero encima del círculo.  

> [!TIP]
> Puedes ver más detalles relacionados con los cambios importantes a través de los últimos 30 días en el [informe de información](insights-report.md).

## <a name="failure-hits-by-market"></a>Número de errores por mercado

El gráfico **Número de errores por mercado** muestra el número total de bloqueos y eventos durante el período de tiempo seleccionado por mercado.

Puedes ver estos datos en forma de **Mapa** visual o cambiar la configuración para verlos en forma de **Tabla**. La tabla mostrará cinco mercados a la vez, ordenados alfabéticamente o por mayor o menor número de sesiones de usuario. También puedes descargar los datos para ver la información de todos los mercados de forma conjunta.


## <a name="package-version"></a>Versión del paquete

El gráfico **Versión del paquete** muestra el número total de bloqueos y eventos durante el período de tiempo seleccionado por versión del paquete. De manera predeterminada, en la parte superior se muestra la versión del paquete con un mayor número seguido del resto en orden descendente. Puedes invertir el orden alternando la flecha situada en la columna **Aciertos** del gráfico.

## <a name="failures"></a>Errores

El gráfico **Errores** muestra el número total de bloqueos y eventos durante el período de tiempo seleccionado por nombre de error. Cada nombre de error se compone de cuatro partes: una o varias clases de problemas, un código de comprobación de errores o excepciones, el nombre de la imagen o del controlador donde se produjo el error y el nombre de función asociada. De manera predeterminada, en la parte superior se muestra el error con un mayor número seguido del resto en orden descendente. Puedes invertir el orden alternando la flecha situada en la columna **Aciertos** del gráfico. También te mostramos el porcentaje de cada error del total de errores.

> [!TIP]
> En ocasiones, es posible que veas una entrada para **Desconocido** en esta sección. Esto ocurre cuando a pesar de nuestros mejores esfuerzos; no podemos recopilar detalles completos para uno o más errores, que se agruparán de manera conjunta en **Desconocido**. A menudo, esto se produce debido a restricciones de almacenamiento, pero también puede ser el resultado de la configuración de privacidad de un dispositivo, los problemas de conexión de red, los volcados de memoria parciales o erróneos, y otros factores.
>
> Si ves **!unknown** como parte del nombre de un error, esto significa que los símbolos no estaban presentes, por lo que no pudimos identificar el nombre del error. Asegúrate de incluir los símbolos en el paquete para obtener análisis de errores precisos. Consulta [Configurar un paquete de la aplicación](../packaging/packaging-uwp-apps.md#configure-an-app-package). En cambio, los nombres de errores que incluyen **!unknown_error_in_** y **!unknown_function** significan que no pudimos recopilar los detalles completos por varios otros motivos.

Para mostrar el informe **Detalles del error** de un error en concreto, selecciona el nombre del error. Si has incluido los archivos de símbolos, el informe **Detalles del error** incluirá el número de errores del último mes, así como un registro de errores que enumera los detalles de las repeticiones (fecha, versión del paquete, tipo de dispositivo, modelo de dispositivo y compilación del SO) y un vínculo al seguimiento de la pila o al archivo CAB, si estuviera disponible.

> [!TIP]
> Los archivos CAB solo estarán disponibles cuando se ha producido un error en un equipo con una compilación de WindowsInsider, por lo tanto, no todos los errores incluirán la opción de descarga de CAB. Para mostrar solo los errores que tienen archivos CAB, selecciona los **errores con las descargas** en el filtro de sección. También puedes hacer clic en el encabezado de **vínculos** en el **registro de errores** para ordenar los resultados para que aparezcan los errores que incluyan archivos CAB en la parte superior de la lista.

En la página de **Detalles del error** , también verás el gráfico de **prevalencia de la pila** , que muestra la parte superior apila que contribuyeron a error, ordenados por el porcentaje y el gráfico de **Configuración del dispositivo (30D)** , que proporciona información detallada sobre la configuración de dispositivos que funcionan con el error. 


## <a name="crash-free-sessions-and-devices-30d"></a>Sesiones libre de bloqueo y dispositivos (30D)

El gráfico de **dispositivos y sesiones libre de bloqueo** , muestra el porcentaje de dispositivos o sesiones de usuario que no se produjo un error en los últimos 30 días. Esta información ayuda a comprender cómo ampliamente los bloqueos afectan a los usuarios. Por ejemplo, una aplicación podría tener 10.000 bloqueos en un día. Si se ven afectado de un 90% de los dispositivos, probablemente haría clasificar como crítica y actuar para corregir esto inmediatamente. Sin embargo, si solo que represente el 5% de dispositivos con la aplicación, la prioridad podría ser inferior.

Este gráfico tiene dos fichas:
- **Libre de bloqueo de dispositivos**: muestra el porcentaje de dispositivos únicos que no se produjo un error en cada día (durante los últimos 30 días).
- **Sesiones libre de bloqueo**: muestra el porcentaje de sesiones de usuario único que no se produjo un error en cada día (durante los últimos 30 días).


 

 
