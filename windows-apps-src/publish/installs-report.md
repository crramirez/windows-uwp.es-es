---
author: shawjohn
Description: "El informe Instalaciones del panel del Centro de desarrollo de Windows te permite ver cuántas veces se instaló correctamente la aplicación en dispositivos Windows10."
title: Informe Instalaciones
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, aplicación, instalaciones, instalación, informe, análisis, app, installs, installation, report, analytics"
ms.assetid: 46c08fd2-00bd-4be5-b29f-01a3b5fea4c2
ms.openlocfilehash: 7912775e17a70c1d6fe9810c780017dcfa2db60e
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="installs-report"></a>Informe Instalaciones

El informe **Instalaciones** del panel del Centro de desarrollo de Windows te permite ver cuántas veces los clientes han instalado correctamente la aplicación en dispositivos Windows10. Puedes visualizar estos datos en tu panel o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión. Como alternativa, puedes recuperar mediante programación estos datos siguiendo el método de [obtención de las instalaciones de la aplicación](../monetize/get-app-installs.md) en la [API de REST de análisis de la Tienda Windows](../monetize/access-analytics-data-using-windows-store-services.md).


## <a name="apply-filters"></a>Aplicar filtros


Cerca de la parte superior de la página, puedes expandir **Aplicar filtros** para filtrar todos los datos de esta página por fecha, dispositivo, tipo o versión del paquete.

-   **Fecha**: el filtro predeterminado es **Últimos 30 días**, pero puedes ampliarlo hasta **Últimos 12 meses**.
-   **Tipo de dispositivo**: el filtro predeterminado es **Todos los dispositivos**, pero puedes seleccionar un tipo de dispositivo específico (**Equipo**, **Teléfono**, **Tableta**, **Máquina Virtual**, **IoT**, **Holográfico**, **Consola**, **Otros** o **Desconocido**).
-   **Versión del paquete**: el filtro predeterminado es **Todas las versiones**, pero puedes seleccionar una versión específica del paquete.


## <a name="installs-daily"></a>Instalaciones diarias


El gráfico **Instalaciones diarias** muestra el número total de instalaciones diarias de la aplicación durante el período de tiempo seleccionado.

El total de instalaciones incluye lo siguiente:
-   **Instalaciones en varios dispositivos Windows10.** Por ejemplo, si un cliente instala tu aplicación en dos equipos con Windows10 y un teléfono con Windows10, cuenta como tres instalaciones.
-   **Reinstalaciones.** Por ejemplo, si un cliente instala la aplicación hoy, la desinstala mañana y la vuelve a instalar al mes siguiente, cuenta como dos instalaciones.

El total de instalaciones no incluye ni refleja lo siguiente:
-   **Instalaciones en dispositivos que no usan Windows10.** Por ejemplo, si un cliente instala la aplicación en un dispositivo que no ejecuta Windows10, no contamos esa instalación.
-   **Desinstalaciones.** Por ejemplo, si un cliente desinstala la aplicación, no la restamos al número total de instalaciones.
-   **Actualizaciones.** Por ejemplo, si un usuario instala la aplicación hoy e instala una actualización de la aplicación una semana después, solo se cuenta como una instalación (y no dos).
-   **Preinstalaciones.** Por ejemplo, si un cliente compra un dispositivo que tenga la aplicación preinstalada, no lo contamos como una instalación.
-   **Instalaciones iniciadas por el sistema.** Por ejemplo, si Windows instala la aplicación automáticamente por algún motivo, no lo contamos como una instalación.

> **Nota** actualmente, no puedes recuperar mediante programación los datos de **instalaciones diarias** con una API.

## <a name="markets"></a>Mercados


El gráfico **Mercados** muestra el número total de instalaciones durante el período de tiempo seleccionado por mercado. De manera predeterminada, mostramos datos para todos los mercados. Sin embargo, puedes filtrarlos por un mercado específico.


## <a name="package-version"></a>Versión del paquete


El gráfico **Versión del paquete** muestra el número total de instalaciones durante el período de tiempo filtrado por versión del paquete.



 

 
