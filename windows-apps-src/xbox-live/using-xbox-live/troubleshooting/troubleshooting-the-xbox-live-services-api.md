---
title: Solución de problemas de la API de Xbox Live Services
description: Obtenga información sobre cómo registrar información de error adicional durante la solución de problemas con las API de Xbox Live.
ms.assetid: 3827bba1-902f-4f2d-ad51-af09bd9354c4
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, solución de problemas, errores, registro
ms.localizationpriority: medium
ms.openlocfilehash: 67735d83e5ff301ee4434a6917fce814cabe8309
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621420"
---
# <a name="troubleshooting-the-xbox-live-apis"></a>Solución de problemas de las API de Xbox Live

## <a name="code"></a>Código

Es difícil de diagnosticar un error con solo el error de la capa de API de servicios de Xbox Live. Información de error adicional, como el registro de todas las llamadas de RESTful, podría estar disponible para el servidor. Para escuchar a los datos, enlazar el registrador de respuesta y habilitar el seguimiento de depuración. Registro de respuesta permite ver tráfico y web service códigos de respuesta HTTP, que a menudo es tan útil como un seguimiento de Fiddler.

### <a name="c"></a>C++

En el ejemplo de código siguiente se habilita el registro de la respuesta y establece el nivel de error de depuración a detallado (también puede establecer el nivel de error de depuración para el Error para mostrar solo las llamadas con error de seguimiento o en Off para deshabilitar el seguimiento). La salida de depuración resultante se envía al panel de resultados cuando se ejecuta el proyecto en Visual Studio.  

```cpp

        // Set up debug tracing to the Output window in Visual Studio.
            xbox::services::system::xbox_live_services_settings::get_singleton_instance()->set_diagnostics_trace_level(
                xbox_services_diagnostics_trace_level::verbose
                );
```

También puede redirigir la salida de depuración en su propio archivo de registro de este modo:

```cpp

        // Set up debug tracing of the Xbox Live Services API traffic to the game UI.
        m_xboxLiveContext->Settings->EnableServiceCallRoutedEvents = true;
        m_xboxLiveContext->Settings->ServiceCallRouted += ref new
        Windows::Foundation::EventHandler<Microsoft::Xbox::Services::XboxServiceCallRoutedEventArgs^>(
            [=] ( Platform::Object^, Microsoft::Xbox::Services::XboxServiceCallRoutedEventArgs^ args )
            {
                gameUI->Log(L"[URL]: " + args->HttpMethod + " " + args->Url->AbsoluteUri);
                gameUI->Log(L"");
                gameUI->Log(L"[Response]: " + args->HttpStatus.ToString() + " " + args->ResponseBody);
            });

```
