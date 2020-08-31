---
title: Prioridades de notificación de WNS
description: Descripción de las distintas prioridades que puede establecer en una notificación
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, UWP, API de WinRT, WNS
localizationpriority: medium
ms.openlocfilehash: 3b6642054f9c63a03764267e5886b67fd4a9ac7d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169219"
---
# <a name="wns-notification-priorities"></a>Prioridades de notificación de WNS
Al establecer la prioridad de una notificación con un encabezado simple en los mensajes de WNS POST, puede controlar cómo se entregan las notificaciones en situaciones sensibles a la batería.

## <a name="power-on-windows"></a>Encender Windows
A medida que más usuarios trabajan solo en dispositivos con batería, la minimización del uso de energía se ha convertido en un requisito estándar para todas las aplicaciones. Si las aplicaciones consumen más energía que el valor que proporcionan, los usuarios pueden desinstalar las aplicaciones. Aunque el sistema operativo Windows reduce el uso de energía en la batería siempre que sea posible, es responsabilidad de la aplicación trabajar de forma eficaz. 

Las prioridades de WNS son una manera de trasladar el trabajo no crítico de la batería. Las prioridades de WNS indican al sistema las notificaciones que deben entregarse al instante y que pueden esperar hasta que el dispositivo esté conectado a una fuente de alimentación. Con estas sugerencias, el sistema puede entregar las notificaciones el tiempo exacto que son más valiosos tanto para el usuario como para la aplicación. 

## <a name="power-modes-on-the-device"></a>Modos de energía en el dispositivo
Cada dispositivo de Windows funciona a través de una variedad de modos de energía (batería, ahorro de batería y cargos) y los usuarios esperan distintos comportamientos de las aplicaciones en diferentes modos de energía. Cuando el dispositivo esté encendido, se deben entregar todas las notificaciones. En el modo de ahorro de batería, solo se deben entregar las notificaciones más importantes. Mientras el dispositivo está conectado, se pueden completar las operaciones de sincronización o no críticas.

Windows no sabe qué notificaciones son importantes para cualquier usuario o aplicación, por lo que el sistema se basa totalmente en las aplicaciones para establecer la prioridad adecuada para las notificaciones. 

## <a name="priorities"></a>Prioridades
Hay cuatro prioridades disponibles para que una aplicación la use al enviar notificaciones de envío. La prioridad se establece en las notificaciones individuales, lo que le permite elegir qué notificaciones deben entregarse al instante (por ejemplo, un mensaje de mensajería instantánea) y cuáles pueden esperar (por ejemplo, ponerse en contacto con las actualizaciones de fotos).

Las prioridades son las siguientes: 

|    Prioridad    |    Invalidación de usuario    |    Descripción    |    Ejemplo    |
|----------------|---------------------|-------------------|---------------|
|    Alto    |    Sí: el usuario puede bloquear todas las notificaciones desde una aplicación o puede impedir que una aplicación se limite en el modo de ahorro de batería.    |    Las notificaciones más importantes que se deben entregar de inmediato en cualquier circunstancia cuando el dispositivo pueda recibir notificaciones. Cosas como llamadas VoIP o alertas críticas que deben reactivar el dispositivo se encuentran en esta categoría.    |    Llamadas de VoIP, alertas críticas en el tiempo    |
|    Media    |    Sí: el usuario puede bloquear todas las notificaciones desde una aplicación o puede impedir que una aplicación se limite en el modo de ahorro de batería.    |    Se trata de cosas que no son tan importantes, lo que no es necesario que suceda de inmediato, pero los usuarios se molestarían si no se ejecutan en segundo plano.    |    Sincronización de cuentas de correo electrónico secundarias, actualizaciones de iconos dinámicos.    |
|    Bajo    |    Sí: el usuario puede bloquear todas las notificaciones desde una aplicación o puede impedir que una aplicación se limite en el modo de ahorro de batería.    |    Notificaciones que solo tienen sentido cuando el usuario usa el dispositivo o cuando la actividad en segundo plano tiene sentido. Estos se almacenan en caché y no se procesan hasta que el usuario inicia sesión o se conecta a su dispositivo.    |    Estado del contacto (en línea/sin conexión)    |
|    Muy bajo     |    No: no puede evitar que las notificaciones de prioridad muy baja se limiten en el modo de ahorro de batería.    |    Esto es prácticamente igual que la prioridad baja, salvo que los usuarios no pueden invalidar la Directiva de ahorro de batería. Estas notificaciones nunca se entregarán en el ahorro de batería.    |    Sincronizando archivos para un servicio de sincronización.    |

Tenga en cuenta que muchas aplicaciones tendrán notificaciones de prioridad diferente a lo largo de su ciclo de vida. Como la prioridad se establece en función de cada notificación, esto no supone un problema. Una aplicación de VoIP puede enviar una notificación de alta prioridad para una llamada entrante y, a continuación, seguirla con una prioridad baja una cuando un contacto se pone en línea. 

## <a name="setting-the-priority"></a>Establecimiento de la prioridad

La configuración de la prioridad de la solicitud de notificación se realiza a través de un encabezado adicional en la solicitud POST, `X-WNS-PRIORITY` . Se trata de un valor entero entre 1 y 4 que se asigna a una prioridad: 

| Nombre de prioridad | Valor de prioridad X-WNS- | Valor predeterminado para: |
|---------------|----------------------|------------------|
| Alto | 1 | Notificaciones del sistema |
| Meduim | 2 | Iconos y distintivos |
| Bajo | 3 | Raw |
| Muy bajo | 4 |  |

Para ser compatible con versiones anteriores, no es necesario establecer una prioridad. En caso de que una aplicación no establezca la prioridad de sus notificaciones, el sistema proporcionará una prioridad predeterminada. Los valores predeterminados se muestran en el gráfico anterior y coinciden con el comportamiento de las versiones existentes de Windows. 

## <a name="detailed-listing-of-desktop-behavior"></a>Lista detallada del comportamiento del escritorio 

Si va a enviar la aplicación en varias SKU diferentes de Windows, normalmente se recomienda seguir el gráfico de la sección anterior. 

A continuación se enumeran los comportamientos recomendados más específicos para cada prioridad. No se garantiza que cada dispositivo funcione exactamente según el gráfico. Los OEM pueden configurar el comportamiento de manera diferente, pero la mayoría están cerca de este gráfico. 

| Estado del dispositivo    | PRIORIDAD: alta    |    PRIORIDAD: medio        | PRIORIDAD: baja    |    PRIORIDAD: muy baja    |
|-------------------------------------------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|--------------------------|
|    Pantalla activada o conectada    |    Entrega    |    Entrega    |    Entrega    |    Entrega    |
|    Pantalla apagada y con batería    |    Entrega    |    Si el usuario está exento: entrega adicional: batch     |    Si el usuario se ha excluido: deliver Else: cache *    |    instancias y claves    |
|    Ahorro de batería habilitado    |    Si el usuario está exento: aportar la memoria caché    |    Si el usuario está exento: aportar la memoria caché    |    Si el usuario está exento: aportar la memoria caché    |    instancias y claves     |
|    Con batería + protector de batería habilitado + pantalla desactivada    |    Si el usuario está exento: aportar la memoria caché    |    Si el usuario está exento: aportar la memoria caché    |    Si el usuario está exento: aportar la memoria caché    |    instancias y claves    |

Tenga en cuenta que las notificaciones de prioridad baja se entregarán de forma predeterminada para la pantalla y la batería solo para dispositivos basados en Windows Phone. Esto es para maintian la compatibilidad con la Directiva de MPNS existente. Tenga en cuenta también que las filas cuarta y quinta son las mismas, simplemente llamando a distintos escenarios.

Para excluir una aplicación en el ahorro de batería, los usuarios deben ir a la "uso de la batería por aplicación" en configuración y seleccionar "permitir que la aplicación ejecute tareas en segundo plano". Esta selección de usuario exime a la aplicación del ahorro de batería para las notificaciones de prioridad alta, media y baja. También puede llamar a la [API BackgroundExecutionManager](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_) para solicitar el permiso del usuario mediante programación.  

## <a name="related-topics"></a>Temas relacionados
- [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](windows-push-notification-services--wns--overview.md)
- [Solicitando permiso para ejecutarse en segundo plano](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)