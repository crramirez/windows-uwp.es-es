---
title: Introducción a las API de Xbox Live
description: Obtenga información sobre los distintos modelos de API que puede usar para interactuar con el servicio Xbox Live.
ms.assetid: 5918c3a2-6529-4f07-b44d-51f9861f91ec
ms.date: 06/05/2018
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1f52e379b524952c3361b432a577a7137b02155b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657700"
---
# <a name="introduction-to-xbox-live-apis"></a>Introducción a las API de Xbox Live

## <a name="use-xbox-live-services"></a>Usar servicios de Xbox Live

Hay dos maneras de obtener información de los servicios de Xbox Live:

- Usar una API del lado cliente llama a la API de servicios de Xbox Live (**XSAPI**)
- Llame a la **extremos REST de Xbox Live** directamente

Las ventajas de usar la API de servicios de Xbox Live (**XSAPI**) incluyen:

- Detalles de autenticación, codificación y envío y recepción de HTTP son controlados por usted.
- Argumentos y los datos devuelven, que la API de contenedor se controla en datos nativos tipos; por lo tanto, no es necesario realizar la codificación y descodificación JSON.
- Llamar directamente a los servicios web implica varios pasos asincrónicos, que encapsula la API de contenedor; Esto facilita Título código leer y escribir.
- Algunas funciones, como escribir eventos de juegos, solo está disponible en XSAPI.

Las ventajas de utilizar el **extremos REST de Xbox Live** directamente incluyen:

- La capacidad de llamar a puntos de conexión de Xbox Live desde un servicio web
- La posibilidad de llamar a puntos de conexión que no están incluidos en XSAPI.  XSAPI solo incluye las API que creemos juegos va a usar, por lo que si hay algo que faltan permiten saber a través de los foros.
- Alguna funcionalidad disponible a través de los puntos de conexión REST no puede tener un contenedor XSAPI correspondiente.

Los juegos y aplicaciones no están limitadas a usar solo uno de estos métodos. Puede usar el contenedor XSAPI y seguir llamando a los puntos de conexión REST directamente si es necesario.

## <a name="xbox-live-services-api-overview"></a>Introducción a la API de Xbox Live Services ##

La API de servicios de Xbox Live (**XSAPI**) expone tres conjuntos de API que admiten una amplia gama de escenarios de clientes del lado cliente:

- [XSAPI WinRT API](#xsapi-winrt-based-api)
- [API basada en C ++ 11 XSAPI](#xsapi-c11-based-api)
- [API basada en XSAPI C](#xsapi-c-based-api) (**nuevo a partir de junio de 2018**)

Comparación de las API:

### <a name="xsapi-winrt-based-api"></a>API basadas en WinRT XSAPI

- Admite aplicaciones escritas con C++ / c++ / CX, C#y JavaScript.
    - C++ / c++ / CX es una extensión de C++ de Microsoft para facilitar la programación de WinRT mediante por ejemplo ^ como punteros de WinRT.
- Es compatible con las aplicaciones destinadas a la plataforma Xbox uno XDK y Universal Windows Platform (UWP) x86, x64 y arquitecturas ARM.
- Los errores se controlan a través de las excepciones en todos los idiomas, incluido C++ / c++ / CX.
- C++ / c++ / WinRT también se admite.  Para obtener más información acerca de C++ / c++ / WinRT puede encontrarse en [https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

Este es un ejemplo de una llamada a la API de WinRT XSAPI con C++ / c++ / WinRT:

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```

Si desea mezclar C++ / c++ / CX y c++ / WinRT como a migrar código puede hacer esto demasiado, pero es un poco más complejo.  
Este es un ejemplo de una llamada a la API de WinRT XSAPI con C++ / c++ / WinRT proporcionado C++ / c++ / CX usuario ^ objeto.

```c++
::Windows::Xbox::System::User^ user1 = ::Windows::Xbox::System::User::Users->GetAt(0);
winrt::Windows::Xbox::System::User cppWinrtUser(nullptr);
winrt::copy_from(cppWinrtUser, reinterpret_cast<winrt::ABI::Windows::Xbox::System::IUser*>(user1));
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```


### <a name="xsapi-c11-based-api"></a>API basada en C ++ 11 XSAPI

- Usa diversas plataformas ISO C ++ 11 estándar
- Soporta aplicaciones escritas con C++
- Es compatible con las aplicaciones destinadas a la plataforma Xbox uno XDK y Universal Windows Platform (UWP) x86, x64 y arquitecturas ARM.
- Los errores se controlan mediante std::error_code.
- La API de basado en 11 de C ++ es la recomendada que se usará para los motores de juegos de C++ para mejorar el rendimiento y la mejor de depuración.
- Si se encuentra en el programa de creadores de Xbox Live, antes de incluir el encabezado XSAPI definir XBOX_LIVE_CREATORS_SDK. Esto limita el área expuesta de API a solo aquellos que se pueden usar los desarrolladores en el programa de creadores de Xbox Live y cambia el método de inicio de sesión para que funcione para los títulos en el programa de creadores.  Por ejemplo:

```c++
#define XBOX_LIVE_CREATORS_SDK
#include "xsapi\services.h"
```

- C++ / c++ / WinRT también se admite.  Para obtener más información acerca de C++ / c++ / WinRT puede encontrarse en [https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

Para usar C++ / c++ / WinRT con la API de C++ XSAPI, antes de incluir el encabezado XSAPI definir XSAPI_CPPWINRT.  Por ejemplo:

```c++
#define XSAPI_CPPWINRT
#include "xsapi\services.h"
```

Este es un ejemplo de una llamada a la API de C++ XSAPI con C++ / c++ / WinRT:

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(cppWinrtUser);
```

### <a name="xsapi-c-based-api"></a>API basada en XSAPI C

- Permite que los títulos controlar las asignaciones de memoria al llamar a XSAPI.
- Permite que los títulos tener control total del subproceso que controla al llamar a XSAPI.
- Usa una nueva biblioteca HTTP, libHttpClient, diseñada para desarrolladores de juegos.

Para obtener más información, consulte [Introducción a las API de C de Xbox Live](xsapi-flat-c.md).
