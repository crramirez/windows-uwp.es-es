---
Description: El informe de mantenimiento en el centro de partners le permite obtener datos relacionados con el rendimiento y la calidad de la aplicación, incluidos los bloqueos y eventos que no responde.
title: Informe de estado
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, mantenimiento, bloqueos, eventos que no responden, estado de la aplicación, datos de estado, seguimiento de la pila, archivo cab, error, errores, pdb, símbolos
ms.localizationpriority: medium
ms.openlocfilehash: c547cc933247e69fd208e8d3c297572815a5f2ea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635840"
---
# <a name="health-report"></a>Informe de estado

El **mantenimiento** notificar en [centro de partners](https://partner.microsoft.com/dashboard) permite obtener los datos relacionados con el rendimiento y la calidad de la aplicación, incluidos los bloqueos y eventos que no responde. Puede ver estos datos en el centro de partners, o [descargar el informe](download-analytic-reports.md) ver sin conexión. Si corresponde, puedes ver seguimientos de la pila o archivos CAB para una mayor depuración.

Como alternativa, puedes recuperar mediante programación los datos de este informe con la [API de REST de análisis de la Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md).


## <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la página, puedes seleccionar el período de tiempo durante el que quieres mostrar los datos. La selección predeterminada es **72H** (72 horas), pero puedes elegir **30D** en su lugar para mostrar los datos de los últimos 30 días. Tenga en cuenta que los datos se muestran en la zona horaria local para el **72H** vista y en formato UTC para el **d. 30** vista.

También puedes expandir la opción **Filtros** para filtrar todos los datos de esta página por versión de paquete, mercado o tipo de dispositivo.

-   **Versión del paquete**: El valor predeterminado es **todas**. Si la aplicación incluye más de un paquete, puedes elegir uno concreto aquí.
-   **Mercado**: El filtro predeterminado es **todos los mercados**, pero puede limitar los datos a uno o varios de los mercados.
-   **Tipo de dispositivo**: El valor predeterminado es **todas**, pero puede optar por mostrar los datos de un único tipo de dispositivo específico. Ten en cuenta que en la categoría **Otros** se incluyen dispositivos en los que se reconoce la marca o modelo, pero no podemos incluirla en una de las categorías predefinidas que se muestran en este filtro. Para estos dispositivos, el modelo de dispositivo puede verse en la sección **Registro de errores** del informe **Detalles del error**.  
-   **Versión del sistema operativo**: El valor predeterminado es **versiones de todos los SO**, pero puede elegir una versión específica del sistema operativo.
-   **Versión de lanzamiento del SO**: El valor predeterminado es **OS todas las versiones de lanzamiento**, pero puede elegir una versión específica del seleccionado **versión del sistema operativo**.
-   **Espacio aislado**: El valor predeterminado es **Retail**, pero para los productos que usan varios espacios aislados de desarrollo (por ejemplo, los juegos que se integran con Xbox Live), puede elegir una en concreto aquí. (Si tu producto no usa espacios aislados, este filtro solo mostrará **Comercial** y no será aplicable.)
-   **Arquitectura**: El valor predeterminado es **todas las arquitecturas**, pero puede elegir un tipo de arquitectura de sistema específico. Este filtro solo está disponible cuando la opción **30D** está seleccionada.
-   **PRAID**: El valor predeterminado es **todas**, pero si define varios paquetes de aplicación relativa identificadores (PRAIDs) al crear el paquete de aplicación, puede elegir mostrar solo los datos relacionados con un PRAID. Este filtro no aparecerá si no se han definido varios PRAID.

La información de todos los gráficos que aparecen a continuación reflejará el intervalo de fechas y los filtros que hayas seleccionado. Algunas secciones también te permiten aplicar filtros adicionales.


## <a name="failure-hits"></a>Número de errores

El gráfico **Número de errores** muestra el número de bloqueos y eventos diarios que los clientes experimentaron al usar la aplicación durante el período de tiempo seleccionado. De cada tipo de evento que la aplicación experimentó se realiza un seguimiento por separado: bloqueos, cuelgues, excepciones de JavaScript y errores de memoria.

Cuando el **d. 30** está seleccionado el período de tiempo, puede ver los marcadores de círculo. Estos representan un aumento significativo o disminuyen en un valor determinado que creemos que desea conocer. La fecha en la que aparece el círculo representa el final de la semana en el que se ha detectado un significativo aumento o disminución en comparación con la semana antes de que. Para ver más detalles sobre lo que ha cambiado, mantenga el mouse sobre el círculo.  

> [!TIP]
> Puede ver más información relacionada con cambios significativos durante los últimos 30 días en el [informe Insights](insights-report.md).

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
> Los archivos CAB solo estarán disponibles cuando se ha producido un error en un equipo con una compilación de Windows Insider, por lo tanto, no todos los errores incluirán la opción de descarga de CAB. Para mostrar solo los errores que tienen archivos .cab, seleccione **errores con descargas** en el filtro de sección. También puede hacer clic en el **vínculos** encabezado en el **registro de errores de** para ordenar los resultados para que aparezcan errores que incluyen los archivos CAB en la parte superior de la lista.

En el **detalles del error** página, también verá el **prevalencia de la pila** gráfico, que muestra las pilas principales que han contribuido al error, ordenado por porcentaje, y el **dispositivo configuración (30D)** gráfico, que proporciona detalles sobre la configuración de dispositivos que presentó el error. 


## <a name="crash-free-sessions-and-devices-30d"></a>Sesiones sin bloqueos y los dispositivos (30D)

El **sesiones sin bloqueos y los dispositivos** gráfico muestra el porcentaje de dispositivos o las sesiones de usuario que no se produjo un bloqueo en los últimos 30 días. Esta información le ayudará a comprender cómo ampliamente los bloqueos están afectando a los usuarios. Por ejemplo, una aplicación podría tener 10.000 bloqueos en un día. Si el 90% de los dispositivos se ven afectado, probablemente sería clasificar como críticas y actuar para corregir este problema inmediatamente. Sin embargo, si sólo que representa el 5% de dispositivos mediante la aplicación, la prioridad podría ser inferior.

Este gráfico tiene dos pestañas:
- **Dispositivos sin bloqueos**: Muestra el porcentaje de dispositivos únicos que no se produjo un error en cada día (durante los últimos 30 días).
- **Las sesiones de bloqueo libre**: Muestra el porcentaje de sesiones de usuario único que no se produjo un error en cada día (durante los últimos 30 días).


 

 
