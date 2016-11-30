---
author: jnHs
Description: "El informe Estado del panel del Centro de desarrollo de Windows te permite obtener los datos relacionados con el rendimiento y la calidad de la aplicación, incluidos los bloqueos y los eventos que no responden."
title: Informe Estado
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
translationtype: Human Translation
ms.sourcegitcommit: 2481343772a6cf8af0667dc0796ed455dba1eaee
ms.openlocfilehash: a92a2ab8f11c46217d1d7b63f86c5bf68757d3fb

---

# Informe Estado


El informe **Mantenimiento** del panel del Centro de desarrollo de Windows te permite obtener los datos relacionados con el rendimiento y la calidad de la aplicación, incluidos los bloqueos y los eventos que no responden. Puedes visualizar estos datos en tu panel (**Análisis** > **Mantenimiento**) o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión. Si corresponde, puedes ver seguimientos de la pila para una mayor depuración. Como alternativa, puedes recuperar mediante programación estos datos con la [API de REST de análisis de la Tienda Windows](../monetize/access-analytics-data-using-windows-store-services.md).


> **Nota** Si ya habías publicado aplicaciones y habías visto datos de rendimiento, es posible que observes que se notifica un mayor número de bloqueos y eventos aquí. Esto se debe a que podemos incluir más datos en este informe para ofrecerle una perspectiva más completa.

## Aplicar filtros


Cerca de la parte superior de la página, puedes expandir **Aplicar filtros** para filtrar todos los datos de esta página por intervalo de fechas, versión del paquete, tipo de dispositivo o versión de SO.

-   **Fecha**: el filtro predeterminado es **Últimas 72 horas**, pero puedes ampliarlo hasta **Últimos 30 días**.
-   **Versión del paquete**: el valor predeterminado es **Todas las versiones**. Si la aplicación incluye más de una versión del paquete, puedes elegir uno concreto.
-   **Tipo de dispositivo**: el valor predeterminado es **Todos los dispositivos**, pero puedes elegir un dispositivo específico (**PC**, **Teléfono**, **Consola**, **IoT**, **Holográfico** o **Desconocido**).
-   **Versión de SO**: el valor predeterminado es **Todas las versiones de SO**, pero puedes elegir una versión específica de SO.

La información de todos los gráficos que aparecen a continuación reflejará el período de tiempo seleccionado en la sección **Aplicar filtros**. De manera predeterminada, se incluirán datos de todas las versiones de paquetes, a menos que hayas usado la opción **Aplicar filtros** para elegir solo una.

## Bloqueos y eventos totales


El gráfico **Failure hits (Número de errores)** (antes conocido como **Bloqueos y eventos totales**) muestra el número de bloqueos y eventos diarios que los clientes experimentaron al usar la aplicación durante el período de tiempo seleccionado. De cada tipo de evento que la aplicación experimentó se realiza un seguimiento por separado: bloqueos, cuelgues, excepciones de JavaScript o errores de memoria.

También puedes filtrar los resultados por mercado o por la versión del SO.

## Mercados


El gráfico **Mercados** muestra el número total de bloqueos y eventos durante el período de tiempo seleccionado por mercado. De manera predeterminada, en la parte superior se muestra el mercado con un mayor número seguido del resto en orden descendente. Puedes invertir el orden alternando la flecha situada en la columna **Aciertos** del gráfico.

## Versión del paquete


El gráfico **Versión del paquete** muestra el número total de bloqueos y eventos durante el período de tiempo seleccionado por versión del paquete. De manera predeterminada, en la parte superior se muestra la versión del paquete con un mayor número seguido del resto en orden descendente. Puedes invertir el orden alternando la flecha situada en la columna **Aciertos** del gráfico.

## Errores


El gráfico **Errores** muestra el número total de bloqueos y eventos durante el período de tiempo seleccionado por nombre de error. De manera predeterminada, en la parte superior se muestra el error con un mayor número seguido del resto en orden descendente. Puedes invertir el orden alternando la flecha situada en la columna **Aciertos** del gráfico. También te mostramos el porcentaje de cada error del total de errores.

Para mostrar el informe **Detalles del error** de un error en concreto, selecciona el nombre del error. Si has incluido los archivos de símbolos de PDB, el informe **Detalles del error** incluirá el número de errores del último mes, así como un registro de errores que enumera los detalles de las repeticiones (fecha, versión del paquete, tipo de dispositivo, modelo de dispositivo y compilación del SO) y un vínculo al seguimiento de la pila.

 

 



<!--HONumber=Nov16_HO1-->


