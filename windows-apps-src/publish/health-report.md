---
author: jnHs
Description: El informe Estado del panel del Centro de desarrollo de Windows te permite obtener los datos relacionados con el rendimiento y la calidad de la aplicación, incluidos los bloqueos y los eventos que no responden.
title: Informe Estado
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
---

# Informe Estado


El informe **Estado** del panel del Centro de desarrollo de Windows te permite obtener los datos relacionados con el rendimiento y la calidad de la aplicación, incluidos los bloqueos y los eventos que no responden. Puedes visualizar estos datos en tu panel o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión. Si corresponde, puedes ver seguimientos de la pila para una mayor depuración. Como alternativa, puedes recuperar mediante programación estos datos con la [API de REST de análisis de la Tienda Windows](../monetize/access-analytics-data-using-windows-store-services.md).

> **Nota**  Si ya habías publicado aplicaciones y habías visto datos de rendimiento anteriormente en los paneles previos, es posible que observes que se notifica un mayor número de bloqueos y eventos aquí. Esto se debe a que podemos incluir más datos en este informe para ofrecerle una perspectiva más completa.

## Aplicar filtros


Cerca de la parte superior de la página, puedes expandir **Aplicar filtros** para filtrar todos los datos de esta página por intervalo de fechas o por versión del paquete.

-   **Fecha**: el filtro predeterminado es **Últimas 72 horas**, pero puedes ampliarlo hasta **Últimos 6 meses**.
-   **Versión del paquete**: el valor predeterminado es **Todas las versiones**. Si la aplicación incluye más de una versión del paquete, puedes elegir uno concreto.

La información de todos los gráficos que aparecen a continuación reflejará el período de tiempo seleccionado en la sección **Aplicar filtros**. De manera predeterminada, se incluirán datos de todas las versiones de paquetes, a menos que hayas usado la opción **Aplicar filtros** para elegir solo una.

## Bloqueos y eventos totales


El gráfico **Bloqueos y eventos totales** muestra el número de bloqueos y eventos diarios que los clientes experimentaron al usar la aplicación durante el período de tiempo seleccionado. De cada tipo de evento que la aplicación experimentó se realiza un seguimiento por separado: bloqueos, cuelgues, excepciones de JavaScript o errores de memoria.

También puedes filtrar los resultados por mercado o por la versión del SO.

## Desglose de bloqueos y eventos


El gráfico **Desglose de bloqueos y eventos** permite ver gráficos que realizan un seguimiento de los detalles relacionados con las configuraciones de los clientes cuando se produjo un bloqueo o un evento sin respuesta. Haz clic en los encabezados de sección para ver detalles sobre:

-   Versión de SO
-   Tipo de dispositivo
-   Memoria (MB)
-   Almacenamiento masivo (GB)
-   Velocidad de la CPU (GHz)

También puedes filtrar los resultados por **Tipo de bloqueo**: bloqueos, cuelgues, excepciones de JavaScript o errores de memoria. (El valor predeterminado es mostrar todos los tipos de bloqueo).

## Mercado


El gráfico **Mercado** muestra el número total de bloqueos y eventos durante el período de tiempo seleccionado por mercado. De manera predeterminada, se muestra el mercado con más adquisiciones en la parte superior seguido del resto en orden descendente. Puedes invertir el orden alternando la flecha situada en la columna **Bloqueos** del gráfico.

## Registro de errores


Si has incluido los archivos de símbolos PDB, el gráfico **Registro de errores** mostrará los detalles relacionados con las apariciones de símbolos específicos, como el número total de bloqueos y el promedio diario de bloqueos por símbolo.

Esta información se basa en un porcentaje de los eventos totales. En la parte superior del gráfico, mostraremos qué porcentaje de eventos se muestreó para proporcionar los datos.

 

 


<!--HONumber=May16_HO2-->


