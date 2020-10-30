---
description: El informe de mantenimiento del centro de Partners le permite obtener datos relacionados con el rendimiento y la calidad de la aplicación, incluidos los bloqueos y los eventos que no responden.
title: Informe de estado
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, estado, bloqueos, eventos que no responden, estado de la aplicación, datos de mantenimiento, seguimiento de la pila, archivo. cab, error, errores, PDB, símbolos
ms.localizationpriority: medium
ms.openlocfilehash: a345ad9fb6f2ed5c540c6e52ef0b95e43d1adc6d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032828"
---
# <a name="health-report"></a>Informe de estado

El informe de **mantenimiento** del [centro de Partners](https://partner.microsoft.com/dashboard) le permite obtener datos relacionados con el rendimiento y la calidad de la aplicación, incluidos los bloqueos y los eventos que no responden. Puede ver estos datos en el centro de Partners o [descargar el informe](download-analytic-reports.md) para verlo sin conexión. Si procede, puede ver los seguimientos de la pila y/o los archivos. CAB para una depuración posterior.

Como alternativa, puede recuperar mediante programación los datos de este informe mediante el uso de la [API de REST de Microsoft Store Analytics](../monetize/access-analytics-data-using-windows-store-services.md).


## <a name="apply-filters"></a>Aplicación de filtros

Cerca de la parte superior de la página, puede seleccionar el período de tiempo para el que desea mostrar los datos. La selección predeterminada es **72H** (72 horas), pero puede elegir **30D** en su lugar para mostrar los datos en los últimos 30 días. Tenga en cuenta que los datos se muestran en la zona horaria local para la vista **72H** y en UTC para la vista **30D** .

También puede expandir **filtros** para filtrar todos los datos de esta página por la versión del paquete, el mercado o el tipo de dispositivo.

-   **Versión del paquete** : el valor predeterminado es **Todas** . Si la aplicación incluye más de un paquete, puedes elegir uno concreto aquí.
-   **Market** : el filtro predeterminado es **todos los mercados** , pero puede limitar los datos a uno o varios mercados.
-   **Tipo de dispositivo** : el valor predeterminado es **Todos** , pero también puedes mostrar los datos de un determinado tipo de dispositivo. Tenga en cuenta que la **otra** categoría incluye los dispositivos donde se reconoce la marca o el modelo, pero no se puede incluir en una de las categorías predefinidas que se muestran en este filtro. Para estos dispositivos, el modelo de dispositivo se puede ver en la sección **registro de errores** del informe **detalles del error** .  
-   **Versión de SO** : el valor predeterminado es **Todas las versiones de SO** , pero puedes elegir una versión específica de SO.
-   **Versión de lanzamiento del sistema operativo** : el valor predeterminado es **todas las versiones** de la versión del sistema operativo, pero puede elegir una versión de lanzamiento específica de la **versión del sistema operativo** seleccionada.
-   **Espacio aislado** : el valor predeterminado es **comercial** , pero para los productos que usan varios espacios aislados de desarrollo (como los juegos que se integran con Xbox Live), puede elegir uno específico aquí. (Si el producto no usa espacios aislados, este filtro solo mostrará el **distribuidor** y no será aplicable).
-   **Arquitectura** : el valor predeterminado es **todas las arquitecturas** , pero puede elegir un tipo de arquitectura de sistema específico. Este filtro solo está disponible cuando se selecciona **30D** .
-   **PRAID** : la configuración predeterminada es **All** , pero si definió varios identificadores de aplicación relativos de paquete (PRAIDs) al crear el paquete de la aplicación, puede elegir mostrar solo los datos relacionados con un PRAID. Este filtro no aparecerá si no ha definido varios PRAIDs.

La información de todos los gráficos que se enumeran a continuación reflejará el intervalo de fechas y los filtros que haya seleccionado. Algunas secciones también permiten aplicar filtros adicionales.


## <a name="failure-hits"></a>Aciertos de errores

El gráfico de **aciertos de errores** muestra el número de bloqueos y eventos diarios que los clientes experimentaron al usar la aplicación durante el período de tiempo seleccionado. Se realiza un seguimiento de cada tipo de evento experimentado por separado: bloqueos, bloqueos, excepciones de JavaScript y errores de memoria.

Cuando se selecciona el período de tiempo de **30D** , es posible que vea marcadores de círculo. Representan un aumento o una disminución significativos en un valor determinado que creemos que querrá conocer. La fecha en la que aparece el círculo representa el final de la semana en el que se ha detectado un aumento o una disminución significativos en comparación con la semana anterior. Para ver más detalles sobre lo que ha cambiado, mantenga el mouse sobre el círculo.  

> [!TIP]
> Puede ver más información relacionada con los cambios significativos en los últimos 30 días del [Informe Insights](insights-report.md).

## <a name="failure-hits-by-market"></a>Aciertos de errores por mercado

El gráfico de **resultados de errores por mercado** muestra el número total de bloqueos y eventos durante el período de tiempo seleccionado por mercado.

Puede ver estos datos en un formulario de **mapa** visual o alternar la configuración para verlo en un formulario de **tabla** . El formulario de tabla mostrará cinco mercados a la vez, ordenados alfabéticamente o por el número más alto o más bajo de sesiones de usuario. También puede descargar los datos para ver la información de todos los mercados juntos.


## <a name="package-version"></a>Versión del paquete

El gráfico **Versión del paquete** muestra el número total de bloqueos y eventos durante el período de tiempo seleccionado por versión del paquete. De manera predeterminada, en la parte superior se muestra la versión del paquete con un mayor número seguido del resto en orden descendente. Puedes invertir el orden alternando la flecha situada en la columna **Aciertos** del gráfico.

## <a name="failures"></a>Errores

El gráfico **Errores** muestra el número total de bloqueos y eventos durante el período de tiempo seleccionado por nombre de error. Cada nombre de error consta de cuatro partes: una o varias clases problemáticas, un código de comprobación de excepción/error, el nombre de la imagen o el controlador donde se produjo el error y el nombre de función asociado. De manera predeterminada, en la parte superior se muestra el error con un mayor número seguido del resto en orden descendente. Puedes invertir el orden alternando la flecha situada en la columna **Aciertos** del gráfico. También te mostramos el porcentaje de cada error del total de errores.

> [!TIP]
> En ocasiones, es posible que vea una entrada **desconocida** en esta sección. Esto sucede cuando, a pesar de nuestros mejores esfuerzos, no se pueden recopilar detalles completos de uno o más errores, que se agruparán en un grupo **desconocido** . Lo más frecuente es que esto se debe a las restricciones de almacenamiento, pero también puede deberse a la configuración de privacidad de un dispositivo, a problemas de conexión de red, a volcados de memoria parciales o incorrectos, y a otros factores.
>
> Si ve **! Unknown** como parte de un nombre de error, significa que los símbolos no estaban presentes, por lo que no pudimos identificar el nombre del error. Asegúrese de incluir símbolos en el paquete para obtener un análisis de errores preciso. Consulte [configurar un paquete de aplicación](/windows/msix/package/packaging-uwp-apps#configure-an-app-package). Por el contrario, los nombres de error que incluyen **! unknown_error_in_** y **! unknown_function** significan que no pudimos recopilar detalles completos por otras razones.

Para mostrar el informe **Detalles del error** de un error en concreto, selecciona el nombre del error. Si ha incluido los archivos de símbolos, el informe **detalles del error** incluye el número de aciertos de error durante el último mes, así como un registro de errores que muestra los detalles de las repeticiones (fecha, versión del paquete, tipo de dispositivo, modelo de dispositivo, compilación del sistema operativo) y un vínculo al seguimiento de la pila y/o archivo. cab, si está disponible.

> [!TIP]
> Los archivos. CAB solo estarán disponibles cuando se produzca el error en un equipo que use una compilación de Windows Insider, por lo que no todos los errores incluirán la opción de descarga de CAB. Para mostrar solo los errores que tienen archivos. CAB, seleccione **errores con descargas** en el filtro de la sección. También puede hacer clic en el encabezado **Links** en el **registro de errores** para ordenar los resultados de modo que los errores que incluyen los archivos. cab aparezcan en la parte superior de la lista.

En la página **detalles del error** , también verá el gráfico de **prevale** de la pila, que muestra las pilas principales que han contribuido al error, ordenados por porcentaje y el gráfico de **configuración del dispositivo (30D)** , que proporciona detalles sobre la configuración de los dispositivos que experimentaron el error. 


## <a name="crash-free-sessions-and-devices-30d"></a>Sesiones y dispositivos sin bloqueos (30D)

El gráfico **sesiones sin bloqueos y dispositivos** muestra el porcentaje de dispositivos o sesiones de usuario que no experimentaron un bloqueo en los últimos 30 días. Esta información le ayudará a comprender el grado en que los bloqueos afectan a los usuarios. Por ejemplo, una aplicación podría tener 10.000 bloqueos en un día. Si el 90% de los dispositivos se ven afectados, es probable que lo clasifique como crítico y actúe para corregirlo inmediatamente. Sin embargo, si solo representa el 5% de los dispositivos que usan su aplicación, la prioridad podría ser menor.

Este gráfico tiene dos pestañas:
- **Dispositivos sin bloqueo** : muestra el porcentaje de dispositivos únicos que no experimentaron un error cada día (en los últimos 30 días).
- **Sesiones sin bloqueos** : muestra el porcentaje de sesiones de usuario únicas que no experimentaron un error cada día (en los últimos 30 días).


 

 
