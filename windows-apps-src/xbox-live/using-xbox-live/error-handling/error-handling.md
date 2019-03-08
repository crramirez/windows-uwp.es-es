---
title: Control de errores
description: Obtenga información sobre cómo controlar los errores cuando pasa a llamar a un servicio de Xbox Live.
ms.assetid: e433dfbd-488b-44ff-8333-1dcf0329cd60
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, error, la llamada de servicio
ms.localizationpriority: medium
ms.openlocfilehash: 637b9da84ef3ae50ea6235ca86e6318ae62acd11
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637010"
---
# <a name="error-handling"></a>Control de errores

Los desarrolladores deben tener especial cuidado al realizar una llamada de servicio. El título siempre debe controlar errores al realizar llamadas de red. Incluso para las llamadas de servicio que han estado trabajando de forma coherente durante el desarrollo, las llamadas al servicio pueden fallar en entornos del mundo real para una serie de motivos diferentes. Por ejemplo, la llamada puede producir un error debido a:

* La red no está disponible.
* El servicio está demasiado ocupado, devuelve un código de estado HTTP 503.
* La solicitud no es válida, devuelve un código de estado HTTP 400.
* El usuario no tiene los permisos adecuados, para devolver un código de estado HTTP 403.
* El usuario ha sido prohibido, devuelve un código de estado HTTP 401.
IXMLHTTPRequest2, que es invocado por el servicio API de WinRT, no puede enviar la solicitud (ERROR_TIMEOUT, RPC_S_CALL_FAILED_DNE, etcetera.)
* La lista anterior no es exhaustiva. La mayoría de estos problemas producirá una excepción de la capa de API de servicios. El título debe capturar estas excepciones y controlarlas adecuadamente.

Hay una dos estilos diferentes de la API de servicios de Xbox (XSAPI) dependiendo de la API de control de errores está usando:

Consulte [control de errores de la API de C++](error-handling-cpp.md).

Consulte [control de errores de la API de WinRT](error-handling-winrt.md).
