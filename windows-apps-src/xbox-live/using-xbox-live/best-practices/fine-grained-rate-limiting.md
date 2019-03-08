---
title: Limitación de velocidad específica bien de Xbox Live
description: Obtenga información sobre cómo Xbox Live fine-grained limitación funciona y cómo evitar que el título de la que se va a velocidad limitada.
ms.assetid: ceca4784-9fe3-47c2-94c3-eb582ddf47d6
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, limitación, limitación de velocidad
ms.localizationpriority: medium
ms.openlocfilehash: b79a4f0a873ffaf5dd824a0c9832f61aa4a7d77f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628690"
---
# <a name="xbox-live-fine-grained-rate-limiting"></a>Limitación de velocidad específica bien de Xbox Live

## <a name="introduction"></a>Introducción

En este artículo se proporciona información general de Xbox Live bien específica la limitación. Además de resumir lo que la limitación es, en este documento también pretende ayudarle a determinar si se limitan y si es, qué herramientas y recursos se encuentran a su disposición.

## <a name="terminology-guide"></a>Guía de terminología

| Término         | Definición                                                  |
|--------------|-----------------------------------------------------------------------------
| FGRL         | Concreta de limitación de velocidad                                                  
| XSAPI        | Interfaz de programación de aplicación de servicios de Xbox                                 
| CU           | Actualización del contenido                                                              
| Ráfaga        | Representa un volumen de solicitudes recibidas en un breve período de tiempo          
| Admitir      | Representa un gran volumen de llamadas recibidas constantemente durante un período de tiempo
| Usuario + título | Representa el emparejamiento de un usuario y un título como una sola entidad                    
| XLTA         | Herramienta Analizador de seguimiento de Xbox Live, que se usa para determinar si el título está limitada

## <a name="xbox-live-fine-grained-rate-limiting"></a>Xbox Live bien específica la limitación

**Ajustar la limitación de velocidad más preciso** fue contratada para promover **justa uso** de los recursos compartidos entre diferentes títulos de Xbox Live. Esta solución es como sistemas de limitación más tradicionales que tienen un servicio que mantiene un recuento del número de solicitudes que se ha realizado una entidad en un determinado período de tiempo.

Las entidades que se alcanzan el límite especificado de servicios, a continuación, se mueven a un estado de rechazo donde todas las solicitudes entrantes de la entidad se activará inmediatamente. Las entidades solo pueden salir de este estado cuando el período de tiempo determinado expira el recuento de las entidades asociadas restablecer la causa.

Bien específico la limitación utiliza la misma mecánica core mencionada anteriormente sin embargo, en lugar de una entidad de seguimiento FGRL realiza un seguimiento de la combinación de **usuario y título** y compara el número asociado a **dos** diferentes **límites** como oponerse a uno. Se aplican límites duales FGRL en cada servicio lo que significa que el número de solicitudes para GameClips no afectará el número de solicitudes de presencia. Las siguientes secciones se pasará a más detalles sobre el usuario y emparejamiento de título, limitar dual y el objeto de respuesta HTTP 429 limitador.

## <a name="fair-usage"></a>Uso justo

Xbox Live, se considera que cada usuario debe tener la misma calidad no experiencia importar qué juego o aplicación que se está reproduciendo. FGRL está diseñado para resolver la situación siguiente:

Un desarrollador acaba de lanzar un título que sigue a la consola Xbox live garantiza el uso óptimo de los servicios mientras el desarrollador B también acaba de lanzar un título de prácticas recomendadas sin embargo esta uno tiene un error desconocido. Este error hace que el título y cada usuario enviar correo no deseado de presencia lo que resulta en el servicio va bajo una carga intensa. El servicio se ralentiza y, finalmente, detiene interrumpir la experiencia de desarrollador del usuarios incluso si se tratara de errores del desarrollador B que causó el problema. Si se implementara FGRL el servicio habría sido capaz de dejar de recibir las solicitudes de la pierda la oportunidad de comporta title, lo que le permite atender title del desarrollador su gran porción de este recurso.

## <a name="title-and-user-granularity"></a>Granularidad de título y el usuario

Título y el usuario se eligieron como la clave para garantizar un uso justo de los recursos de Xbox Live. Solo el usuario de seguimiento, se crearía un escenario en el que la experiencia del usuario sería a merced de cada integración títulos. Por ejemplo, la mayoría de los títulos usar el servicio de las personas todavía para este ejemplo, supongamos que bien más preciso limitación de velocidad se ha configurado en el servicio de las personas que permite a no más de 100 solicitudes en 5 minutos. Si un usuario a un juego que realizan 100 solicitudes en 1 minuto, se superará el límite y el usuario no pueda realizar más solicitudes al servicio de personas; Imagine que en el mismo período el usuario, a continuación, vuelve a la pantalla principal y hace clic en su lista de amigos: Puesto que el usuario ya habría superan el límite de la llamada de la lista de amigos produciría un error hasta que los 5 minutos, transcurrido el intervalo, aun cuando la pantalla de inicio no era responsable de poner el usuario en el estado limitado.

Limitación también se basa solo en el título, se producirá un resultado no equitativos igualmente. Establecer un límite por título omitiría la popularidad de los títulos y las solicitudes solo se primero entraría servir en primer lugar hasta que se alcance un límite.

El emparejamiento de usuario y el título se asegura de que ningún título utiliza más recursos que lo que sea adecuado, dado el número de usuarios activos mientras ofrece a cada usuario una porción coherente de los recursos.

![](../../images/FGRL.png)

El diagrama anterior muestra una vista de alto nivel de cómo se controla la solicitud. En primer lugar la solicitud se genera y, a continuación, recibe el servicio deseado. Al recibir la solicitud, el sistema se comprueba para ver el número de veces que el usuario y el título junto se ha tenido acceso el servicio. Si la solicitud esté por debajo del límite, a continuación, se procesará con normalidad. Si la solicitud se encuentra en o por encima del límite se coloca los servicios y en su lugar devolver una respuesta 429. La respuesta se indicará cuánto tiempo hasta el período revierte a través de y se pueden controlar las solicitudes de usuario y el título.

## <a name="burst-and-sustain-limits"></a>Ráfaga y a mantener los límites

Tradicionalmente, la limitación de velocidad consta de un límite por cada punto de conexión que se realiza un seguimiento durante un determinado período de tiempo. Este período representa la cantidad de tiempo que se realiza el seguimiento de un número de solicitudes de entidades. Al final del período, el recuento de las entidades se restablece en 0 para iniciar el seguimiento de nuevo.

Este enfoque funciona para la mayoría de API de pero no era lo suficientemente resistente para juegos y aplicaciones que llaman a Xbox live. La solución anterior se da por supuesto que están llamando las personas de una coherente manor predecible continuo. En el caso de Xbox Live, dependiendo del servicio y solicitar título o aplicación, los patrones que realiza la llamada son considerablemente diferentes.

Elección de un solo límite en este caso sería necesario poner en peligro en ambos extremos del espectro de patrón de llamada. La solución de Xbox Live utiliza dos puntos y los límites. El período más pequeño se denomina el período de ráfaga mientras lo más tiempo mayor se conoce como el período de Sostenerla. El tiempo de ráfaga de período para FGRL siempre es 15 segundos, mientras que la mantenga siempre es de 300 segundos (5 minutos) por lo que durante un período de sostenerla 5 minutos, hay 20 ráfaga períodos. Ambos ráfaga y a mantener los límites de seguimiento al mismo tiempo y, por tanto las solicitudes de recuento al mismo tiempo. Las ráfagas y el límite de sostenerla se establecen en el servicio lo que significa que cada servicio tiene su propio número de ráfagas y sostenerla. Juntos para ayudarle a entender cómo funcionan estos dos límites de la tabla siguiente muestra un usuario de reproducción de un título que se está realizando un número de solicitudes de servicio que ha implementado FGRL. En este caso es el límite de ráfaga de 30 solicitudes en 15 segundos y el límite de sostenerla es 100 solicitudes durante 5 minutos.

| Período de tiempo (segundos)  | Solicitudes por período de ráfaga  | Solicitudes por mantener el período  | \# de las solicitudes limitadas dentro del intervalo de 15 s | ¿El límite? (ráfaga, mantener, o ambos)  |
|-------------|--------------|----------------|-----------------------------------------------------|---------------------------|
| 0-15        | 35           | 35             | 5                                                   | Ráfaga                     |
| 15-30       | 28           | 63             | 0                                                   | N/D                       |
| 30-45       | 21           | 84             | 0                                                   | N/D                       |
| 45-60       | 36           | 120            | 20                                                  | Ambos                      |
| 60-75       | 24           | 144            | 24                                                  | Admitir                   |
| …           | …            | …              |                                                     | …                         |
| 285-300     | 4            | 148            |                                                     | Admitir                   |

La tabla muestra en los primeros 15 segundos los viajes de usuario limitar la ráfaga mediante solicitudes de 35. Se quitan esas 5 solicitudes adicionales y 5 429 respuestas se envían. Conviene tener en cuenta que esas 5 solicitud aunque limitada aún cuentan para el límite de sostenerla. Una vez que se desencadena cualquier límite ninguna solicitud se permite a través de tal como se muestra cuando tanto recorridos de los límites en la segunda marca de 45 y de nuevo cuando se realizan solo 4 solicitudes al segundo 285 marcan.

## <a name="http-429-response-object"></a>HTTP 429 objeto Response

Cuando el recuento de usuario y el título asociado es igual o superior ya sea la ráfaga o mantener el límite del servicio no controlará la solicitud y en su lugar, devolverá una respuesta HTTP 429. Representa el código del HTTP 429 "demasiadas solicitudes" irá acompañado de un encabezado que contiene un valor de "reintento después de X segundos". FGRL 429 contiene un reintento después de encabezado que especifica la cantidad de tiempo que las entidades que realiza la llamada deben esperar antes de intentarlo de nuevo. Los programadores que utilizan XSAPI no tendrá que preocuparse como XSAPI respeta y controla el encabezado Retry-After. La respuesta real contendrá los campos siguientes:

| Nombre del campo      | Tipo de valor | Ejemplo                | Definición                       |
|-----------------|------------|------------------------|----------------------------------|
| Versión         | Enteros    | `"version":1`          |                                  |
| currentRequests | Enteros    | `"currentRequests":13` | Número total de solicitudes enviadas    |
| maxRequests     | Enteros    | `"maxRequests":10`     | Número total de solicitudes permitidas |
| periodInSeconds | Enteros    | `“periodInSeconds”:15` | Ventana de tiempo                      |
| Tipo            | Cadena     | `“type”:”burst”`       | Tipo de límite de limitación              |

## <a name="implemented-limits"></a>Límites de implementada

Los siguientes servicios han implementado los límites de FGRL, con el cumplimiento de estos límites en su lugar desde **mayo de 2016**. Insistimos, estos límites será el mismo en todos los espacios aislados y títulos. **Se considera heredado y exentos, por tanto, cualquier título que se publican a través de la plataforma para desarrolladores de Xbox o centro de partners y distribuido antes de mayo de 2016.**

| **Nombre** | **Límite de ráfaga** (15 segundos por usuario y el título) | **Límite de soportar** (300 segundos por usuario y el título) | **Límite de certificación** (10 x sostenidas, 300 segundos por usuario y el título) |
|----------------------------|---------------------------|----------------------------|----------------------------|
| Lectura de estadísticas                 | 100                       | 300                        | 3000                       |
| Perfil                    | 10                        | 30                         | 300                        |
| MPSD                       | 30                        | 300                        | 3000                       |
| Presencia                   | Lectura de 10, 3 de escritura          | Lectura de 100, 30 de escritura         | 1000 lectura, escritura 300       |
| Redes sociales                     | 10                        | 30                         | 300                        |
| Marcadores               | 30                        | 100                        | 1000                       |
| Logros               | 100                       | 300                        | 3000                       |
| Coincidencia inteligente                | 10                        | 100                        | 1000                       |
| Entradas de usuario                 | 100                       | 300                        | 3000                       |
| Escritura de estadísticas                | 100                       | 300                        | 3000                       |
| Privacidad                    | 10                        | 30                         | 300                        |
| Clubes                      | 10                        | 30                         | 300                        |

La tabla anterior representa la lista actual de los servicios que se seleccionaron para FGRL. Esta lista no es final como nuevos servicios y los servicios existentes se pueden agregar. Cuando un servicio se va a agregar la tabla se actualizará y se realizará un anuncio. Los límites que se representa en la tabla también no deben interpretarse como finalizado. Como servicios, cambiar y evolucionar, evolucionará igualmente los límites, sin embargo, se le notificará y se realizarán las exenciones heredadas necesarias.

Como de **abril de 2018** títulos no pasará certificación si superan el sostenerla límite (límite de a qué velocidad limitar surte efecto) x 10.  Por ejemplo, si se establece el límite sostenido en el que FGRL surte efecto a 300 llamadas en 300 segundos, como se especifica en la tabla anterior, los títulos de o por encima de 3000 llamadas en 300 segundos se producirá un error certificación. 

## <a name="service-mapping-and-title-effects-of-rate-limiting"></a>Asignación de servicio y los efectos de título de la limitación de velocidad

| **Nombre** | **Punto de conexión de servicio** | **Impacto de juego Anticipiated de FGRL**
| --- | :---: | --- 
| Lectura de estadísticas | userstats.xboxlive.com | No actualizan entradas logros o tablas de clasificación o a recuperan.
| Perfil | profile.xboxlive.com | Datos del jugador no actualizan o se muestran correctamente.
| MPSD | sessiondirectory.xboxlive.com | Combinaciones/invitaciones no completaría correctamente, las sesiones que no se crean o actualizan correctamente que puede provocar errores de título.
| Presencia | presence.xboxlive.com | Presencia del jugador en el juego no sería precisa.
| Redes sociales | social.xboxlive.com | Afecta a todas las escrituras de amigos (por ejemplo, agregar a un amigo, nombra un favorito etc.) y puede afectar a las lecturas de confianza (por ejemplo, capturar mi lista de amigos). Los desarrolladores pueden realizar para llamar a la peoplehub para lectura, en lugar de social.xboxlive.com.
| Marcadores | leaderboards.xboxlive.com | En el juego para marcadores podría no rellenar/actualización de UX.
| Logros | achievements.xboxlive.com | No se actualizaría la experiencia del usuario en el juego para logros desbloqueado.
| Coincidencia inteligente | momatch.xboxlive.com | Coincidencias podrían no estar correctamente configuradas.
| Entradas de usuario | userposts.xboxlive.com | Entradas de usuario no aparecerán.
| Escritura de estadísticas | statswrite.xboxlive.com | Entradas de logros o marcadores no actualizadas.
| Privacidad | privacy.xboxlive.com | Acceso bloqueado pueden producir errores de privacidad para todos los llamadores.
| Clubes | Clubhub.xboxlive.com | Puede que el Reproductor no pueda ver sus gimnasios en juego.

**NOTA:** La asignación de API más reciente se actualiza periódicamente y pueden encontrarse en [Live asignación de API del analizador de seguimiento](https://github.com/Microsoft/xbox-live-trace-analyzer/blob/master/Source/XboxLiveTraceAnalyzer.APIMap.csv).

## <a name="faq"></a>Preguntas más frecuentes

## <a name="how-can-i-determine-i-am-being-throttled-and-what-steps-can-i-take"></a>¿Cómo puedo determinar estoy está limitando y ¿qué puedo hacer?

Consulte la [prácticas recomendadas de Xbox Live](best-practices-for-calling-xbox-live.md) documentar contendrán los pasos para mejorar su patrón de llamada, así como una explicación de cómo pueden usar para avisarle de que la aserción XSAPI y administradores XSAPI sociales y de varios jugadores y mitigar los problemas de limitación.

Otra opción consiste en registrar un seguimiento de las llamadas de Xbox Live y, a continuación, analizar dicho seguimiento mediante la [herramienta Analizador de seguimiento de Xbox Live](https://docs.microsoft.com/windows/uwp/xbox-live/tools/analyze-service-calls).  Para registrar un seguimiento, puede usar Fiddler para registrar un. Archivo SAZ, o utilizar el registro de seguimiento integrado de XSAPI. Para obtener más información, cómo usar activar seguimientos en XSAPI consulte la página de documentación de Xbox Live "Analizar las llamadas a servicios de Xbox Live". Una vez que un seguimiento, el analizador de Xbox Live seguimiento herramienta le avisará cuando detecta limita las llamadas.

Puede encontrar los procedimientos recomendados de papel en docs GDNP y SDK y XDK 1602 y versiones posteriores.

### <a name="can-limits-change"></a>¿Se pueden cambiar los límites?

La intención es que los límites publicados no cambiarán con el tiempo. Sin embargo, si fuera la necesidad de que surjan, es posible que algunos de los límites se volvió más estricta; en ese caso, los títulos que ya se liberaron Retail estarán exentos de los límites actualizados.

### <a name="are-more-services-going-to-get-limits"></a>¿Son más servicios que se van a obtener los límites?

Sí, más servicios y nuevos servicios puede y se pueden crear límites. Sin embargo, simplemente como esta primera versión FGRL se le notificará y se tomarán las precauciones adecuadas.

### <a name="when-will-these-changes-take-effect"></a>¿Cuándo apliquen estos cambios?

Límites de velocidad se han impuesto desde **mayo de 2016**.  Como de **abril de 2018**, títulos superior al especificado sostenida límites x 10 o más no pasará el proceso de certificación de Xbox.

### <a name="what-if-we-cant-adhere-to-the-limits"></a>¿Qué ocurre si no podemos cumplimos los límites?

Consulte la [prácticas recomendadas de Xbox Live](best-practices-for-calling-xbox-live.md) y asegúrese de que está siguiendo estos pasos.  También considere el uso de la [manager sociales](../../social-platform/intro-to-social-manager.md) si se están limitados con cualquiera de los servicios de redes sociales.

Si después de seguir estos pasos, todavía no puede permanecer dentro de los límites, póngase en contacto con su administrador de cuentas para desarrollador.

**NOTA: No se podrá pasar el certificado después de abril de 2018 títulos en o por encima de 10 veces el límite especificado sostenerla**.  Por ejemplo, si se establece el límite sostenido en el que FGRL surte efecto a 300 llamadas en 300 segundos, como se especifica en la tabla anterior, los títulos de o por encima de 3000 llamadas en 300 segundos se producirá un error certificación.

### <a name="what-about-my-existing-title"></a>¿Qué sucede con Mi título existente?

Todos los títulos en el sector MINORISTA antes de abril de 2018 se consideran heredado y están exentos.

### <a name="content-updates"></a>¿Actualizaciones de contenido?

Para un título heredado o exento, las actualizaciones de contenido también estará exentas, aunque se recomienda encarecidamente aprovechar las herramientas y recursos para optimizar los aspectos de la integración de servicio de su juego.

### <a name="can-i-get-an-exemption-for-my-game-until-i-can-make-a-content-update"></a>¿Puedo obtener una exención para mi juego hasta que puedo realizar una actualización de contenido?

Póngase en contacto con su administrador de cuentas de desarrollador.
