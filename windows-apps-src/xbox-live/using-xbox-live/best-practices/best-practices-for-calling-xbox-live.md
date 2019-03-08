---
title: Prácticas recomendadas para llamar a Xbox Live
description: Obtenga información sobre las prácticas recomendadas para llamar a las API de Xbox Live.
ms.assetid: f4c7156b-7736-41e5-9b3d-e87cc8dd2531
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, procedimientos recomendados
ms.localizationpriority: medium
ms.openlocfilehash: 55e05ef7de2e2981f9f5af86623a8d8413ce2c99
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613710"
---
# <a name="best-practices-for-calling-xbox-live"></a>Prácticas recomendadas para llamar a Xbox Live

Los servicios de Xbox Live se pueden llamar desde dos formas principales: uso de la API de servicios de Xbox (XSAPI), o llamar directamente a los puntos de conexión REST. Independientemente de cómo el código llama a Xbox Live, es importante tener patrones de llamada correcta y la lógica de reintento.

Para aprender a escribir la lógica de reintento adecuada, es necesario saber acerca de los dos tipos de puntos de conexión REST: **idempotente** y **no idempotentes**. Analizaremos cada una de ellas aparece a continuación
 
## <a name="non-idempotent-endpoints"></a>Puntos de conexión que no sean idempotentes

Repita los métodos HTTP que tienen efectos tras se consideran llamadas **no idempotentes**. Esto significa que si un cliente se llamase al punto de conexión y se produce un tiempo de espera de la red, no es seguro volver a intentar el método porque el recurso se han actualizado pero que no la red notificar al llamador que fue correcta. Tras el error, en lugar de volver a intentar, el cliente debe consultar primero para ver si la llamada fue correcta. Solo si la llamada no se realizó correctamente, a continuación, debe lo vuelva a intentar.

En la API de servicios de Xbox, algunas API internamente se marca como llamar a extremos que no sean idempotentes. Esto significa que si se producen errores al llamar a estos puntos de conexión, las API no reintentará automáticamente el punto de conexión.

La lista completa de las API que no sean idempotentes son:

* game\_server\_platform\_service::allocate\_cluster()
<br>
* game\_server\_platform\_service::allocate\_cluster\_inline()
<br>
* game\_server\_platform\_service::allocate\_session\_host()
<br>
* matchmaking\_service::create\_match\_ticket()
<br>
* multiplayer\_service::write\_session()
<br>
* multiplayer\_service::write\_session\_by\_handle()
<br>
* multiplayer\_service::send\_invites()
<br>
* reputation\_service::submit\_batch\_reputation\_feedback()
<br>
* reputation\_service::submit\_reputation\_feedback()
 

## <a name="idempotent-methods"></a>Métodos idempotente

**Idempotente** métodos HTTP, por otro lado, no dejen efectos secundarios. A su vez, esto significa que sean seguros de que se vuelva a intentar. En la API de servicios de Xbox, todos los métodos idempotente se reintentan automáticamente en determinadas condiciones.

La lista completa de idempotente API son todas las API que no se han indicado anteriormente como que no sean idempotentes.


## <a name="retry-logic-best-practices"></a>Procedimientos recomendados de la lógica de reintento

Para las llamadas idempotente que Reintentar automáticamente estas condiciones:

* Todos los errores de red
* 401: Sin autorización
* 408: RequestTimeout
* 429: Demasiadas solicitudes
* 500: InternalError
* 502: BadGateway
* 503: ServiceUnavailable
* 504: GatewayTimeout


En UWP, 401: No autorizado es tratamiento especial. Indica que expiró el token de autenticación de Xbox Live, por lo que la API de servicios de Xbox llama en el sistema operativo para el token de actualización y, a continuación, se realiza como un reintento único.

Cuando se realiza un reintento, es recomendable no llamar al servicio hasta que se ha alcanzado el tiempo de encabezado "Retry-After". XSAPI ahora implementa esta práctica recomendada. Si se devolvió un código de estado de error HTTP y el encabezado "Retry-After" para cualquier API, las llamadas adicionales a esa misma API antes de la hora Retry-After devolverá inmediatamente con el error original sin alcanzar el servicio.

Al volver a intentar una llamada, es recomendable realizar retroceso exponencial con una vibración aleatoria para distribuir la carga en el servicio. XSAPI comienza con un retraso predeterminado de 2 segundos que se controla mediante xbox\_live\_contexto\_settings::set\_http\_vuelva a intentar\_delay(). Esto significa que de forma predeterminada cada reintento realiza un retroceso exponencial de 2, 4, 8, segundos etcetera y jitters el retraso entre el actual y siguiente retroceso valor basándose en el tiempo de respuesta para la propagación carga en el conjunto de dispositivos al intentar el reintento.

Títulos deben estar en el control de la larga dedicará a reintentar una llamada. Con XSAPI, los desarrolladores tienen control directo de este mediante el uso de la consola xbox función\_live\_contexto\_settings::set\_http\_tiempo de espera\_window(). De forma predeterminada, se establece en 20 segundos. Si se establece en 0 segundos eficazmente se desactivará lógica de reintento. XSAPI ahora también ajusta dinámicamente el tiempo de espera interno de HTTP en función de cuánto tiempo queda izquierdo en el http\_tiempo de espera\_window(). Los controles de tiempo de espera HTTP internos cuánto tiempo el sistema operativo dedica a realizar la operación de red HTTP antes de anular. No se reintentará la llamada a menos que queda al menos 5 segundos en http\_tiempo de espera\_window() para dar un tiempo suficientemente razonable para que se complete la llamada. Esta regla no se aplica a la primera llamada, por lo que establecer el http\_tiempo de espera\_window() en 0 es aceptable y dará como resultado una única llamada. Esta lógica tiene el efecto que http\_tiempo de espera\_window() es mucho más determinista cuando se devolverá la llamada API. Si se devolvió un encabezado "Retry-After", no se realizará ninguna reties hasta después de que se ha alcanzado el tiempo "Retry-After". Si la hora "Retry-After" es posterior http\_tiempo de espera\_window() y, a continuación, la llamada se devuelve al final del http\_tiempo de espera\_window().


## <a name="error-handling"></a>Control de errores

Los desarrolladores de título deben **siempre** usar correcto control de errores para **cada** llamada de servicio, deben asegurarse de que está controlando las respuestas de error correctamente.
 
Hay muchas condiciones reales que pueden provocar una solicitud a Xbox Live para devolver los códigos de error, como

1.  Red no está disponible. Por ejemplo, el dispositivo perdido 4G, pierde Wi-Fi, o la red ha fallado.
2.  Demasiada carga en los servicios de control de carga (503)
3.  Se produjo un error en el servicio (500)
4.  Demasiadas solicitudes donde se envían al servicio (429)
5.  Conflicto de operación (412) de escritura. Por ejemplo, otro reproductor en una sesión de varios jugadores en primer lugar envía un cambio
6.  El usuario no se admite o no tiene permiso
7.  Usuario ha iniciado horizontal

Controlador de errores adecuado es fundamental para garantizar que el juego funciona correctamente en estas condiciones.

XSAPI tiene dos tipos de patrones de control de errores. Un patrón al usar las APIs WinRT de C++ / c++ / CX, C\#, Javascript y otro patrón al usar las nuevas APIs C++. Obtener detalles completos sobre los procedimientos recomendados de control de errores, vea la página de documentación de Xbox Live "Control de errores" y para ver un vídeo que se abarca esto, consulte la charla en [ *Xfest 2015 vídeos* ](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx) llamado *XSAPI: ¡No hay excepciones de C++!*


## <a name="best-calling-patterns"></a>Patrones de llamadas de procedimientos

### <a name="usebatching-requests"></a>Uso de las solicitudes de procesamiento por lotes

Algunos puntos de conexión compatible con procesamiento por lotes o la agregación de un conjunto de solicitudes en una sola llamada. Por ejemplo, con el servicio de perfil de Xbox Live, puede pedir un único perfil de usuario o un conjunto de perfiles de los usuarios. Por lo que si necesita un perfiles de usuario para un conjunto de usuarios, sería muy ineficiente para llamar al punto de conexión o API uno a la vez para cada perfil de usuario. Cada llamada agrega una gran sobrecarga de autenticación. Por lo que en su lugar, pase todos los usuarios que desea obtener información acerca de a la vez a la API, para que el punto de conexión puede procesar todos los perfiles de usuario al mismo tiempo y devolver una única respuesta.

### <a name="use-the-real-time-activity-rta-service-instead-of-polling"></a>Usar el servicio de la actividad en tiempo Real (ATR) en lugar de sondeo

Otra práctica recomendada es usar el sondeo en su lugar periódico del servicio de actividad en tiempo Real (ATR). Este servicio expone un websocket que envía una notificación a los clientes cuando cambian los recursos de destino en el servicio. El servicio de ATR ofrece notificaciones de cambios de presencia, los cambios de estadística, los cambios del documento de sesión de varios jugadores y relación sociales. Para saber lo que está interesado en el cliente de información, el cliente primero debe suscribirse al elemento a través del socket Web. Esto evita que sondeen el servicio para detectar los cambios desde que se le indica exactamente cuando cambia el elemento.

Expone XSAPI la ATR de servicio como un conjunto de API que los clientes pueden usar la suscripción. Cada una de estas API tienen sus correspondientes \* \_cambiado\_controlador de API que realizar en una función de devolución de llamada que se llama cuando cambia un elemento.

* presence\_service::subscribe\_to\_device\_presence\_change
<br>
* presence\_service::subscribe\_to\_title\_presence\_change
<br>
* user\_statistics\_service::subscribe\_to\_statistic\_change
<br>
* social\_service::subscribe\_to\_social\_relationship\_change<br>
 

## <a name="use-xbox-live-client-side-managers"></a>Utilizan los administradores de lado cliente de Xbox Live

Nuevo en XSAPI ahora tenemos un conjunto de administradores que actúan como la memoria caché y el estado de las máquinas que lo hacen a todas la pesada levantamiento para determinados escenarios.


### <a name="social-manager"></a>Administrador de redes sociales

El Administrador de redes sociales no todo el trabajo pesado en torno a las listas de amigos y perfiles. Mantendrá a sus amigos lista, sus perfiles y sus datos de presencia actualizado mediante el servicio de ATR. El administrador expone una API sincrónica que es el motor de juego muy descriptivo. Juegos pueden llamar a su API con la frecuencia que mantiene una caché en memoria de la información más reciente desde el servicio. Para obtener más información, consulte la página de documentación de Xbox Live "Introducción al administrador de redes sociales"

### <a name="multiplayer-manager"></a>Administrador de varios jugadores

Para la administración de la sesión de varios jugadores, el Administrador de varios jugadores es una solución de orden para juegos multijugador tradicionales. La API del Administrador de varios jugadores incluye administración de sesión y la lista de jugadores, controla las invitaciones de juegos, combinación en curso, contactos y se conecta a la solución de red existente. Lo hace todo el trabajo pesado en torno a implementar flujos de varios jugadores tradicionales. Para obtener más información, consulte la página de documentación "Introducción al administrador de varios jugadores" Xbox Live


## <a name="throttling-fine-grained-rate-limiting"></a>Limitación (limitación de velocidad específica bien)

Servicios de Xbox Live tienen limitaciones en su lugar para impedir que cualquier dispositivo único colocando extreme carga en el servicio. Es importante saber cuándo se limitó el título. Puede indicar si se limitó el título de varias maneras diferentes:


### <a name="monitor-for-http-status-code-429"></a>Supervisión de código de estado HTTP 429

Puede usar Fiddler y ver si se devuelve un código de estado HTTP 429. La respuesta JSON contendrá los detalles acerca de cómo se limitó el punto de conexión. Por ejemplo:

```json
{
  "version":1,
  "currentRequests":13,
  "maxRequests":10,
  "periodInSeconds":120,
  "limitType":"Rate"
}
```

Si usas XSAPI, las API devolverán un http\_estado\_429\_demasiado\_muchos\_solicitudes de error y establece el mensaje de error como detalles acerca de cómo se limitó la API.

### <a name="debug-asserts"></a>Depurar aserciones

Cuando se usa XSAPI, si la llamada está limitada en un espacio aislado para desarrolladores y con una compilación de depuración del título, impondrá inmediatamente que el desarrollador sepan se ha producido una limitación. Esto es para evitar que faltan de forma no intencionada error 429 limitación debido al código escrito incorrectamente. Si quiere deshabilitar estas aserciones para seguir trabajando sin corregir el código incorrecto, puede llamar a esta API:


> xboxLiveContext -&gt;settings() -&gt;deshabilitar\_aserciones\_para\_xbox\_live\_limitación\_en\_dev\_ los espacios aislados (xbox\_live\_contexto\_limitación\_setting::this\_código\_necesita\_a\_ser\_cambiado\_a\_evitar\_limitación);

pero tenga en cuenta que esta API no impedirá que el título de está limitando. Todavía se limitará su título. Esto deshabilita simplemente las aserciones en espacios aislados de desarrollo al usar una compilación de depuración. 

### <a name="xbox-live-trace-analyzer-tool"></a>Herramienta de analizador de seguimiento de Xbox Live

Otra opción consiste en registrar un seguimiento de las llamadas de Xbox Live y, a continuación, analizar dicho seguimiento mediante la [herramienta Analizador de seguimiento de Xbox Live.](https://docs.microsoft.com/windows/uwp/xbox-live/tools/analyze-service-calls)

Para registrar un seguimiento, puede usar Fiddler para registrar un. Archivo SAZ, o mediante el registro de seguimiento integrado de XSAPI. Para obtener más información, cómo usar activar seguimientos en XSAPI consulte la página de documentación de Xbox Live "Analizar las llamadas a servicios de Xbox Live". Una vez que un seguimiento, el analizador de Xbox Live seguimiento herramienta le avisará cuando detecta limita las llamadas.

## <a name="is-xbox-live-up"></a>¿Es de Xbox Live?

Xbox Live es una colección de microservicios que exponen las características de Xbox Live, como el perfil, amigos y presencia, estadísticas, marcadores, logros, multijugador y contactos. No hay un solo servidor o punto de conexión que define si Xbox Live está activo. Si un único servidor deja de funcionar, el resto de los microservicios de Xbox Live sean independiente y debe estar operativo.

Si un único servicio sufre una interrupción temporal, es importante saber si esta llamada de servicio es de vital importancia para su juego. Intente proporcionar experiencia razonable mientras hay intermitente de la red o problemas de servicio. Por ejemplo, si el servicio de presencia devuelve errores que llaman a probablemente no es crítica para su juego. Así que simplemente notificar al usuario la presencia último conocido en lugar de informar que Xbox Live está inactivo.

Xbox Live, también se sigue el modelo de coherencia de la coherencia final. Esto significa que si no hay nuevas actualizaciones se realizan, que finalmente todas las solicitudes para ese recurso notificará el último valor actualizado. En efecto, esto significa que se propaga un período pequeño donde la información está obsoleta que los datos.
