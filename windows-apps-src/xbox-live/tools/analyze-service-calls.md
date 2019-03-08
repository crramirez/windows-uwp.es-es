---
title: Analizador de seguimiento activo de Xbox
description: Obtenga información sobre cómo usar el analizador de seguimiento de Xbox Live para revisar las llamadas de servicio realizadas por su título.
ms.assetid: b4490fae-d554-403d-bbbc-601af38af0ef
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, las llamadas, las pruebas de servicio, analizador de seguimiento
ms.localizationpriority: medium
ms.openlocfilehash: fa8ca37842edfbeaab0063cd953f3a34358a82da
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623470"
---
# <a name="xbox-live-trace-analyzer"></a>Analizador de seguimiento activo de Xbox

La API de servicios de Xbox Live ahora permite a los desarrolladores de título capturar todas las llamadas de servicio y, a continuación, analizarlos sin conexión para las infracciones en la llamada a patrones. Mediante el uso de nuevas funciones disponibles en la herramienta de línea de comandos xbtrace o a través de la activación de protocolos para escenarios más avanzados, se puede activar el seguimiento de llamadas de servicio. También se admite la activación de la llamada al servicio de seguimiento directamente desde el código de título. La herramienta de análisis sin conexión, denominada el analizador de seguimiento del servicio de Xbox Live (XBLTraceAnalyzer.exe) puede encontrarse como parte del paquete de herramientas de Xbox Live [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools).


## <a name="gather-logs-and-analyze-the-service-calls"></a>Recopilar registros y analizar las llamadas de servicio

Los pasos siguientes son necesarios para recopilar los registros que contienen el registro de las llamadas de servicio y analizan con el analizador de seguimiento de Xbox Live.

1.  Compile el título con la versión de la Xbox Live Services API que se incluye en el de julio de 2015 o una versión más reciente de la Xbox Developer Kit (XDK).
2.  Modificar el título para habilitar el seguimiento como se describe a continuación.
3.  Implemente su título.
4.  Inicie el título y realice al menos una llamada a servicios de Xbox Live para inicializar la API de servicios de Xbox Live.
5.  Iniciar el seguimiento en el momento en el título de la que desea analizar.
6.  Detener el seguimiento.
7.  Ejecute la herramienta de analizador de seguimiento de Xbox Live en el equipo de desarrollo y ver la salida.

## <a name="starting-and-stopping-tracing"></a>Iniciar y Detener seguimiento

Hay tres formas de iniciar y detener el seguimiento:

1.  Puede llamar a un conjunto de API de servicios de Xbox Live directamente desde su título.
2.  Puede usar el *xbtrace* herramienta de línea de comandos.
3.  Puede utilizar la activación de protocolos a través de la *Application Management (xbapp.exe)* herramienta de línea de comandos.


### <a name="starting-and-stopping-tracing-directly-from-your-title"></a>Iniciar y Detener seguimiento directamente desde su título

Para comenzar el seguimiento directamente desde su título, haga lo siguiente:

1.  En el `Microsoft::Xbox::Services::Experimental` espacio de nombres, el `EnableServiceCallTracking` propiedad de la `ServiceCallTrackerSettings` clase en true.
2.  Llame a `StartServiceCallTracking()` para iniciar el seguimiento de llamadas de servicio.
3.  Llame a `StopServiceCallTracking()` para detener el seguimiento de llamadas de servicio.
4.  Cuando se haya detenido el seguimiento, copie el archivo de seguimiento resultante de la unidad temporal de desarrollador en la consola de vuelva a su equipo mediante el uso *(xbcp.exe) del archivo de copia* o *Xbox una vecindad* para analizarlos con el analizador de seguimiento de Xbox Live.

### <a name="starting-and-stopping-tracing-by-using-the-xbtrace-command-line-tool"></a>Iniciar y detener el seguimiento mediante la herramienta de línea de comandos xbtrace

La manera más sencilla y cómoda de iniciar el seguimiento es usar la herramienta de línea de comandos xbtrace con el tipo de seguimiento xboxliveservices. Cuando usas xbTrace, se copia el archivo de seguimiento resultante a su PC para usted.

Iniciar y detener seguimientos mediante xbtrace se basa en la activación de protocolos. Antes de usar xbtrace para iniciar y detener el seguimiento, debe inicializar activación de protocolos mediante una llamada a la `RegisterForProtcolActivation` método en el `ServiceCallTrackerSettings` clase.

El ejemplo siguiente muestra cómo iniciar y detener un seguimiento de servicios de Xbox Live mediante xbTrace:

    xbtrace start xboxliveservices
    xbtrace stop


Recuerde que debe ejecutarse el título y activación de protocolos debe inicializarse antes de poder iniciar y detener el seguimiento con xbtrace. Cuando se haya detenido el seguimiento, xbtrace copia el archivo de seguimiento en el equipo de desarrollo y la coloca en un directorio cuyo nombre incluya "xbtrace" y una marca de tiempo. El nombre de este directorio puede invalidarse mediante \[etlfile\] opción xbtrace.

<a name="starting-and-stopping-tracing-by-using-protocol-activation"></a>Iniciar y detener el seguimiento mediante la activación de protocolo
----------------------------------------------------------
El seguimiento también puede controlarse mediante el uso de las características de activación de protocolo de "xbApp inicio". Debe conocer titleid del título para iniciar y detener el seguimiento mediante la activación de protocolos. Puede encontrar el identificador de título en el archivo de manifiesto de su título. El seguimiento se controla a través de URI que contienen el parámetro "serviceCallTracking". Los ejemplos siguientes muestran cómo iniciar y detener el seguimiento para un título cuyo identificador de título es 12345678:

    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=start"
    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=stop"

Cuando se usa la activación de protocolos, el archivo de seguimiento resultante se almacena en la unidad temporal de desarrollador en la consola. Deberá copiar el archivo en su PC con xbcp o el vecindario una Xbox. El archivo no se copia automáticamente a lo PC ya que es cuando se usa xbtrace.

Activación de protocolos le permite configurar parámetros de seguimiento adicionales, como un nivel de detalle. Se admiten cuatro niveles de detalle: quiet, diagnóstico, detallado y mínimo. El ejemplo siguiente muestra cómo establecer un nivel de detalle:

    xbapp launch "ms-xbl-12345678://serviceCallTracking?verbosity=diagnostic"

## <a name="analyze-the-trace-file"></a>Analizar el archivo de seguimiento

Después de que se ha copiado el archivo de seguimiento a su PC, puede usar la herramienta Analizador de seguimiento de Xbox Live en GNDP para analizar el uso de su título de servicios de Xbox Live. Consulte la documentación incluida con la herramienta Analizador de seguimiento de Xbox Live en juego Developer Network para obtener una descripción de cómo invocar la herramienta e interpretar sus resultados. ¿También puede ejecutar XBLTraceAnalyzer.exe con la opción de línea de comandos? o -h para ver la Ayuda de línea de comandos.
