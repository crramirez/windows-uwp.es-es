---
Description: El informe de mantenimiento del centro de Partners le permite obtener datos relacionados con el rendimiento y la calidad de la aplicación, incluidos los bloqueos y los eventos que no responden.
title: Informe de estado
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, mantenimiento, bloqueos, eventos que no responden, estado de la aplicación, datos de estado, seguimiento de la pila, archivo cab, error, errores, pdb, símbolos
ms.localizationpriority: medium
ms.openlocfilehash: 9b6795673959510d0e4f5452a68ffced6c43ced1
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682500"
---
# <a name="health-report"></a>Informe de estado

El informe de **mantenimiento** del [centro de Partners](https://partner.microsoft.com/dashboard) le permite obtener datos relacionados con el rendimiento y la calidad de la aplicación, incluidos los bloqueos y los eventos que no responden. Puede ver estos datos en el centro de Partners o [descargar el informe](download-analytic-reports.md) para verlo sin conexión. Si corresponde, puedes ver seguimientos de la pila o archivos CAB para una mayor depuración.

Como alternativa, puedes recuperar mediante programación los datos de este informe con la [API de REST de análisis de la Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md).


## <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la página, puedes seleccionar el período de tiempo durante el que quieres mostrar los datos. La selección predeterminada es **72H** (72 horas), pero puedes elegir **30D** en su lugar para mostrar los datos de los últimos 30 días. Tenga en cuenta que los datos se muestran en la zona horaria local para la vista **72H** y en UTC para la vista **30D** .

También puedes expandir la opción **Filtros** para filtrar todos los datos de esta página por versión de paquete, mercado o tipo de dispositivo.

-   **Versión del paquete**: La configuración predeterminada es **All**. Si la aplicación incluye más de un paquete, puedes elegir uno concreto aquí.
-   **Mercado**: El filtro predeterminado es **todos los mercados**, pero puede limitar los datos a uno o varios mercados.
-   **Tipo de dispositivo**: La configuración predeterminada es **All**, pero puede elegir Mostrar datos solo para un tipo de dispositivo específico. Ten en cuenta que en la categoría **Otros** se incluyen dispositivos en los que se reconoce la marca o modelo, pero no podemos incluirla en una de las categorías predefinidas que se muestran en este filtro. Para estos dispositivos, el modelo de dispositivo puede verse en la sección **Registro de errores** del informe **Detalles del error**.  
-   **Versión del sistema operativo**: El valor predeterminado es **todas las versiones del sistema operativo**, pero puede elegir una versión específica del sistema operativo.
-   **Versión de lanzamiento del sistema operativo**: El valor predeterminado es **todas las versiones de lanzamiento del sistema operativo**, pero puede elegir una versión de lanzamiento específica de la **versión del sistema operativo**seleccionada.
-   **Espacio aislado**: El valor predeterminado es **comercial**, pero para los productos que usan varios espacios aislados de desarrollo (como los juegos que se integran con Xbox Live), puede elegir uno específico aquí. (Si tu producto no usa espacios aislados, este filtro solo mostrará **Comercial** y no será aplicable.)
-   **Arquitectura**: El valor predeterminado es **todas**las arquitecturas, pero puede elegir un tipo de arquitectura de sistema específico. Este filtro solo está disponible cuando la opción **30D** está seleccionada.
-   **PRAID**: La configuración predeterminada es **All**, pero si definió varios identificadores de aplicación relativos de paquetes (PRAIDs) al crear el paquete de la aplicación, puede elegir mostrar solo los datos relacionados con un PRAID. Este filtro no aparecerá si no se han definido varios PRAID.

La información de todos los gráficos que aparecen a continuación reflejará el intervalo de fechas y los filtros que hayas seleccionado. Algunas secciones también te permiten aplicar filtros adicionales.


## <a name="failure-hits"></a>Número de errores

El gráfico **Número de errores** muestra el número de bloqueos y eventos diarios que los clientes experimentaron al usar la aplicación durante el período de tiempo seleccionado. De cada tipo de evento que la aplicación experimentó se realiza un seguimiento por separado: bloqueos, cuelgues, excepciones de JavaScript y errores de memoria.

Cuando se selecciona el período de tiempo de **30D** , es posible que vea marcadores de círculo. Representan un aumento o una disminución significativos en un valor determinado que creemos que querrá conocer. La fecha en la que aparece el círculo representa el final de la semana en el que se ha detectado un aumento o una disminución significativos en comparación con la semana anterior. Para ver más detalles sobre lo que ha cambiado, mantenga el mouse sobre el círculo.  

> [!TIP]
> Puede ver más información relacionada con los cambios significativos en los últimos 30 días del [Informe Insights](insights-report.md).

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
> Si ves **!unknown** como parte del nombre de un error, esto significa que los símbolos no estaban presentes, por lo que no pudimos identificar el nombre del error. Asegúrate de incluir los símbolos en el paquete para obtener análisis de errores precisos. Consulta [Configurar un paquete de la aplicación](/windows/msix/package/packaging-uwp-apps#configure-an-app-package). En cambio, los nombres de errores que incluyen **!unknown_error_in_** y **!unknown_function** significan que no pudimos recopilar los detalles completos por varios otros motivos.

Para mostrar el informe **Detalles del error** de un error en concreto, selecciona el nombre del error. Si has incluido los archivos de símbolos, el informe **Detalles del error** incluirá el número de errores del último mes, así como un registro de errores que enumera los detalles de las repeticiones (fecha, versión del paquete, tipo de dispositivo, modelo de dispositivo y compilación del SO) y un vínculo al seguimiento de la pila o al archivo CAB, si estuviera disponible.

> [!TIP]
> Los archivos CAB solo estarán disponibles cuando se ha producido un error en un equipo con una compilación de Windows Insider, por lo tanto, no todos los errores incluirán la opción de descarga de CAB. Para mostrar solo los errores que tienen archivos. CAB, seleccione **errores con descargas** en el filtro de la sección. También puede hacer clic en el encabezado **Links** en el **registro de errores** para ordenar los resultados de modo que los errores que incluyen los archivos. cab aparezcan en la parte superior de la lista.

En la página **detalles del error** , también verá el gráfico de prevale de la **pila** , que muestra las pilas principales que han contribuido al error, ordenados por porcentaje y el gráfico de **configuración del dispositivo (30D)** , que proporciona detalles sobre el configuración de los dispositivos que experimentaron el error. 


## <a name="crash-free-sessions-and-devices-30d"></a>Sesiones y dispositivos sin bloqueos (30D)

El gráfico **sesiones sin bloqueos y dispositivos** muestra el porcentaje de dispositivos o sesiones de usuario que no experimentaron un bloqueo en los últimos 30 días. Esta información le ayudará a comprender el grado en que los bloqueos afectan a los usuarios. Por ejemplo, una aplicación podría tener 10.000 bloqueos en un día. Si el 90% de los dispositivos se ven afectados, es probable que lo clasifique como crítico y actúe para corregirlo inmediatamente. Sin embargo, si solo representa el 5% de los dispositivos que usan su aplicación, la prioridad podría ser menor.

Este gráfico tiene dos pestañas:
- **Dispositivos sin bloqueos**: Muestra el porcentaje de dispositivos únicos que no experimentaron un error cada día (en los últimos 30 días).
- **Sesiones sin bloqueos**: Muestra el porcentaje de sesiones de usuario únicas que no experimentaron un error cada día (en los últimos 30 días).


 

 
