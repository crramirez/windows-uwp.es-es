---
title: Títulos comunes de EDS
assetID: 91297c9e-709d-7886-1da0-8a896c4953f4
permalink: en-us/docs/xboxlive/rest/edscommonheaders.html
description: " Títulos comunes de EDS"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51a85383ee75fa51cb8376451ac955dcc4fa2edf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630710"
---
# <a name="eds-common-headers"></a>Títulos comunes de EDS

<a id="ID4EO"></a>



## <a name="entertainment-discovery-services-eds-common-request-headers"></a>Encabezados de solicitud comunes (EDS) de servicios de detección de entretenimiento

| Nombre de encabezado| Descripción| ¿Obligatorio?| Notas|
| --- | --- | --- | --- |
| <b>x-xbl-contract-version</b>| Versión del servicio de EDS| sí| 3.2|
| <b>x-xbl-client-type</b>| Encabezado de tipo de cliente| sí| Hablar al equipo para obtener su propio tipo de cliente.|
| <b>x-xbl-client-version</b>| Versión del cliente de| sí| Cualquier cadena no vacía.|
| <b>x-xbl-parent-ig</b>| Guid de impresión| sí| Se usa para realizar un seguimiento de solicitud en los registros y a través de otras llamadas de servicio.|
| <b>x-xbl-device-type</b>| Tipo de dispositivo| sí| Dispositivo que representa el cliente.|
| <b>Accept</b>| Acepte el tipo| sí| XML o JSON.|
| <b>Autorización</b>| Encabezado de autenticación| sí|  |
| <b>x-xbl-autoSuggest-seed-text</b>| Texto de inicialización de sugerencias automáticas| no| Utilizado para BI y la relevancia|
| <b>x-xbl-search-term</b>| Término de búsqueda| no|  |
| <b>x-xbl-input-method</b>| Método de entrada utilizado por el usuario| no| Controlador, voz, Kinect.|
| <b>x-xbl-kinect-enabled</b>| Kinect habilitado| no| Sí/no.|
| <b>x-xbl-speech-session-id</b>| Id. de sesión de voz| no| Si se inició la sesión usa la voz.|
| <b>x-xbl-client-id</b>| Anonymous Client Id| no| Se usa para los informes de BI y la relevancia.|
| <b>x-xbl-device-id</b>| Id. de dispositivo| no| Se usa para los informes de BI y la relevancia.|
| <b>x-xbl-user-agent</b>| Agente de usuario de cliente| no| Se usa para BI. "&lt;nombre > /&lt;versión > (&lt;versión del sistema operativo >; &lt;plataforma >; &lt;capacidad >; &lt;fabricar >; &lt;modelo >) ".|
| <b>x-xbl-parent-ig</b>| Guid anterior de impresión para las llamadas de "Chained"| no (pero muy recomendado)| Es importante para la importancia de BI. Por ejemplo, IG de una llamada examinar es el elemento primario IG un siguientes llamada de detalle.|
| <b>delid</b>| Delegar la identidad| no| Usa los servicios internos para funcionar en nombre de un usuario.|

## <a name="common-response-headers"></a>Encabezados de respuesta comunes

| Nombre de encabezado| Descripción| ¿Obligatorio?| Notas|
| --- | --- | --- | --- | --- | --- | --- | --- |
| <b>Cache</b>| Encabezados de caché| sí| Consulte <a href="https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9"> http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9 </a>.|
| <b>x-xbl-errors</b>| Errores| no| Lista de errores de distintos proveedores de datos.|
| <b>x-xbl-traceid</b>| Id. de seguimiento de los registros| sí| Se usa para realizar seguimiento de los registros de solicitudes específicas.|
| <b>x-server-name</b>| Confuso nombre del servidor que controla la solicitud.| sí| Se utiliza para la depuración interna.|

<a id="ID4EECAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EGCAC"></a>


##### <a name="parent"></a>Primario  

[Referencia adicional](atoc-xboxlivews-reference-additional.md)


<a id="ID4ESCAC"></a>


##### <a name="further-information"></a>Para obtener más información

[URI de Marketplace](../uri/marketplace/atoc-reference-marketplace.md)
