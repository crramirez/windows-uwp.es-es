---
author: jnHs
Description: "El informe Estado del panel del Centro de desarrollo de Windows te permite obtener los datos relacionados con el rendimiento y la calidad de la aplicación, incluidos los bloqueos y los eventos que no responden."
title: Informe Estado
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.author: wdg-dev-content
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 67f87405f7738dec591c71678ecfa5122e673502
ms.sourcegitcommit: ae93435e1f9c010a054f55ed7d6bd2f268223957
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2017
---
# <a name="health-report"></a>Informe Estado


El informe **Mantenimiento** del panel del Centro de desarrollo de Windows te permite obtener los datos relacionados con el rendimiento y la calidad de la aplicación, incluidos los bloqueos y los eventos que no responden. Puedes visualizar estos datos en tu panel de información o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión. Si corresponde, puedes ver seguimientos de la pila o archivos CAB para una mayor depuración.

Como alternativa, puedes recuperar mediante programación los datos de este informe con la [API de REST de análisis de la Tienda Windows](../monetize/access-analytics-data-using-windows-store-services.md).


## <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la página, puedes seleccionar el período de tiempo durante el que quieres mostrar los datos. La selección predeterminada es **72H** (72 horas), pero puedes elegir **30D** en su lugar para mostrar los datos de los últimos 30 días.

También puedes expandir la opción **Filtros** para filtrar todos los datos de esta página por versión de paquete, mercado o tipo de dispositivo.

-   **Versión del paquete**: el valor predeterminado es **Todas**. Si la aplicación incluye más de un paquete, puedes elegir uno concreto aquí.
-   **Mercado**: el filtro predeterminado es **Todos los mercados**, pero puedes limitar los datos a adquisiciones de uno o varios mercados.
-   **Tipo de dispositivo**: el valor predeterminado es **Todos**, pero también puedes mostrar los datos de un determinado tipo de dispositivo.
-   **Versión de SO**: el valor predeterminado es **Todas las versiones de SO**, pero puedes elegir una versión específica de SO.

La información de todos los gráficos que aparecen a continuación reflejará el intervalo de fechas y los filtros que hayas seleccionado. Algunas secciones también te permiten aplicar filtros adicionales.


## <a name="failure-hits"></a>Número de errores

El gráfico **Número de errores** muestra el número de bloqueos y eventos diarios que los clientes experimentaron al usar la aplicación durante el período de tiempo seleccionado. De cada tipo de evento que la aplicación experimentó se realiza un seguimiento por separado: bloqueos, cuelgues, excepciones de JavaScript y errores de memoria.


## <a name="failure-hits-by-market"></a>Número de errores por mercado

El gráfico **Número de errores por mercado** muestra el número total de bloqueos y eventos durante el período de tiempo seleccionado por mercado.

Puedes ver estos datos en forma de **Mapa** visual o cambiar la configuración para verlos en forma de **Tabla**. La tabla mostrará cinco mercados a la vez, ordenados alfabéticamente o por mayor o menor número de sesiones de usuario. También puedes descargar los datos para ver la información de todos los mercados de forma conjunta.


## <a name="package-version"></a>Versión del paquete

El gráfico **Versión del paquete** muestra el número total de bloqueos y eventos durante el período de tiempo seleccionado por versión del paquete. De manera predeterminada, en la parte superior se muestra la versión del paquete con un mayor número seguido del resto en orden descendente. Puedes invertir el orden alternando la flecha situada en la columna **Aciertos** del gráfico.

## <a name="failures"></a>Errores

El gráfico **Errores** muestra el número total de bloqueos y eventos durante el período de tiempo seleccionado por nombre de error. De manera predeterminada, en la parte superior se muestra el error con un mayor número seguido del resto en orden descendente. Puedes invertir el orden alternando la flecha situada en la columna **Aciertos** del gráfico. También te mostramos el porcentaje de cada error del total de errores.

Para mostrar el informe **Detalles del error** de un error en concreto, selecciona el nombre del error. Si has incluido los archivos de símbolos de PDB, el informe **Detalles del error** incluirá el número de errores del último mes, así como un registro de errores que enumera los detalles de las repeticiones (fecha, versión del paquete, tipo de dispositivo, modelo de dispositivo y compilación del SO) y un vínculo al seguimiento de la pila o al archivo CAB, si estuviera disponible.

> [!TIP]
> Los archivos CAB solo estarán disponibles cuando se ha producido un error en un equipo con una compilación de WindowsInsider, por lo tanto, no todos los errores incluirán la opción de descarga de CAB. Puedes hacer clic en el título **Vínculos** de **Registro de errores** para ordenar los resultados de modo que aparezcan los errores que incluyan archivos CAB en la parte superior de la lista.

 

 
