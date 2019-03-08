---
title: Página de resumen de Xbox Live
description: Describe cómo puede aprovechar la vista de resumen de Xbox Live
ms.assetid: ''
ms.date: 10/19/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, Xbox, juegos, uwp, windows 10, Xbox one, xbox live resumen, resumen, publicar, historial de xbox live, barra de comandos, ficha Historial, tabla de resumen
ms.openlocfilehash: 289b472939c721e5bfb373d4de62ed800840bf57
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605990"
---
# <a name="the-xbox-live-configuration-summary-page"></a>La página Resumen de configuración de Xbox Live

Puede usar [centro de partners](https://developer.microsoft.com/dashboard) configurar Xbox Live del título. Centro de partners podrá administrar la configuración del servicio para cada uno de los espacios aislados del título.
Con el fin de configurar los aspectos de Xbox Live de su título, vaya a la sección de Xbox Live en su título, ubicado en **servicios** > **Xbox Live**. En esta página, encontrará una instantánea de la configuración actual en el recinto de seguridad seleccionado. También encontrará un análisis detallado de lo que se ha configurado y publicado en el recinto de seguridad.

## <a name="sandbox-selector"></a>Selector de espacio aislado

 [Los espacios aislados](../../xbox-live-sandboxes.md) ahora son un elemento de navegación de nivel superior, puede pasar a otro o expandir mediante la selección. La interfaz de usuario muestra los espacios aislados del título en la parte superior como pestañas. La información que se muestra en cada pestaña está en el contexto del espacio aislado asociado.  

![Imagen de cambiar de fichas de espacio aislado](../../images/summary/sandbox-tabs1.gif)

 Puede agregar espacios aislados adicionales si selecciona el "+" que se le presentará un cuadro de diálogo donde especifique qué recinto de seguridad que le gustaría copiar la configuración de y a qué espacio aislado que desea copiar la configuración.  

 ![Imagen de la expansión de una nueva pestaña del espacio aislado](../../images/summary/sandbox-tabs2.gif)

## <a name="command-bar"></a>Barra de comandos

Como se mencionó anteriormente la página se muestra siempre es dentro del contexto de un espacio aislado, por lo tanto la barra de comandos expuesta justo debajo de él muestra todas las acciones que puede realizar en su recinto de seguridad determinado. Los comandos disponibles son:  

* **Exportar** -que proporciona un archivo zip que contiene configurados todos los documentos en el espacio aislado.
* **Importación** -le permite proporcionar un archivo zip XBL válido que contiene los documentos que una vez cargado estará disponible en el espacio aislado.
* **Certificar** -publica la configuración actual en el espacio aislado de certificación.  *También puede usar el botón publish y cambiar el destino para el certificado para realizar esta acción.*
* **Historial de** -abre una pestaña que muestra información sobre quién creó qué y cuándo. Puede abrir esta ficha en cualquier página y se filtrará para los objetos creados en esa página.
* **Publicar** -le permite elegir el espacio aislado de origen y destino. Una vez seleccionado, ejecutará una validación que le permite saber si puede publicar la configuración. Si permite, seleccione Publicar will establecer la configuración para el espacio aislado para que puede probar esta configuración al usar el recinto de seguridad adecuado.  
  
  
![Imagen de la barra de comandos](../../images/summary/command-bar.png)  

## <a name="summary-table"></a>Tabla de resumen

La interfaz de usuario ahora proporciona un significativo de resumen de seguridad de todas las configuraciones diferentes, lo que permite una en una vista general de lo que se ha configurado, lo que es opcional y, lo que todavía se requiere antes de la publicación a la venta directa.  

* **Detalle** : se describe lo que se ha configurado para una característica determinada en el espacio aislado actual (Esto incluye los objetos que ha creado pero aún no se han publicado)
* **Desde la última publicación** : Esto le permitirá saber qué nuevas configuraciones que ha creado no se han publicado en su recinto de seguridad para las pruebas
* **Estado** – le informa de si esta característica está lista para publicarse en venta directa. Cualquier cosa con la etiqueta "No está listo para la venta minorista" debe solucionarse, filas marcadas como "Opcional" a discreción del desarrollador.

*Anteriormente, una vez seleccionado el botón"probar", ejecutaría la validación y solo entonces sabría si tenía un problema que es necesario corregir; Esto simplifica ese proceso y crea una mejor experiencia de usuario*  
  
![Imagen de la barra de comandos](../../images/summary/summary-table.png)  

## <a name="history-pane"></a>Panel del historial

El panel de historial muestra los objetos que se crearon en el recinto de seguridad e indica quién y cuándo. Cuando se encuentra en la página Resumen, el panel de historial mostrará todos los objetos crean y publicación las acciones realizadas en el espacio aislado. Sin embargo, al abrir este panel a una página concreta, como los logros verá solo el historial mención especial que permite filtrar fácilmente la búsqueda de historial.  

![Imagen del panel de historial](../../images/summary/history.png)  

## <a name="best-practices"></a>Procedimiento recomendado

* Publicar los cambios en su recinto de seguridad después de realizar algunos cambios para asegurarse de que son en directo en sus cuentas de prueba y los dispositivos.
* Use la nueva vista de ficha, la tabla de resumen y panel de historial para ayudarle a identificar rápidamente lo que está publicada donde.
* En casos extremos que necesita para realizar comparaciones XML entre espacios aislados que se puede usar la característica de exportación de ambos espacios aislados para obtener ambos documentos y, a continuación, abrirlos con una herramienta como más allá de comparar.
* Exportación puede usarse para obtener una versión local de los archivos que se puede asignar su propio control de código fuente. De este modo, si se cada pierde cualquier configuración que no pierda realmente. Puede sacar la configuración local del control de código fuente y vuelva a importarlo el espacio aislado.