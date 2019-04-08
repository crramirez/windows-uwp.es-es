---
title: Prioridades de notificación de WNS
description: Descripción de las diversas prioridades que se pueden establecer en una notificación
ms.date: 01/10/2017
ms.topic: article
keywords: API de Windows 10, uwp, WinRT, WNS
localizationpriority: medium
ms.openlocfilehash: 2c297a04786c6fbf1eb0600e63a04a6d88585864
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648710"
---
# <a name="wns-notification-priorities"></a>Prioridades de notificación de WNS
Al establecer la prioridad de una notificación con un encabezado simple en mensajes POST WNS, puede controlar cómo se entregan las notificaciones en situaciones confidenciales de la batería.

## <a name="power-on-windows"></a>Energía en Windows
Usuarios que trabaja solamente en los dispositivos con tecnología de la batería, minimizar el uso de energía se ha convertido en un requisito para todas las aplicaciones estándar. Si las aplicaciones consumen más energía que el valor que proporcionan, los usuarios pueden desinstalar las aplicaciones. Si bien el sistema operativo de Windows reduce el uso de energía de la batería siempre que sea posible, es responsabilidad de la aplicación funcione de forma eficaz. 

Las prioridades WNS es una manera de mover el trabajo no críticas fuera de la batería. Las prioridades WNS indican al sistema de las notificaciones que se deben entregar al instante y que puede esperar a que el dispositivo está conectado a una fuente de alimentación. Con estas sugerencias, el sistema puede entregar las notificaciones de la hora exacta son más valiosos para el usuario y la aplicación. 

## <a name="power-modes-on-the-device"></a>Modos de energía en el dispositivo
Todos los dispositivos Windows funciona a través de diversos modos de energía (la batería, ahorro de batería y cargo), y los usuarios esperan distintos comportamientos de las aplicaciones en los modos de energía diferente. Cuando el dispositivo está activado, se deben entregar todas las notificaciones. En modo de ahorro de batería, se deben entregar solo las notificaciones más importantes. Mientras el dispositivo está conectado, se pueden completar sincronización o que no sea de tiempos operaciones críticas.

Windows no conoce las notificaciones que son importantes para cualquier usuario o aplicación, por lo que el sistema se basa totalmente en las aplicaciones para establecer la prioridad adecuada para sus notificaciones. 

## <a name="priorities"></a>Prioridades
Hay cuatro prioridades disponibles para una aplicación que se utilizará al enviar notificaciones de inserción. La prioridad se establece en las notificaciones individuales, que le permite elegir las notificaciones que deben entregarse al instante (por ejemplo, un mensaje de mensajería instantánea) y las que pueden esperar (por ejemplo, póngase en contacto con las actualizaciones de la foto).

Las prioridades son: 

|    Prioridad    |    Reemplazo de usuario    |    Descripción    |    Ejemplo    |
|----------------|---------------------|-------------------|---------------|
|    Alto    |    Sí: el usuario puede bloquear todas las notificaciones desde una aplicación o puede impedir que una aplicación se está limitando en modo de ahorro de batería.    |    Las notificaciones más importantes que deben entregarse inmediatamente en ninguna circunstancia, cuando el dispositivo puede recibir notificaciones. Cosas como las llamadas de VoIP o alertas críticas que deben reactivar el dispositivo está dentro de esta categoría.    |    Llamadas de VoIP, las alertas críticas de tiempo:    |
|    Medio    |    Sí: el usuario puede bloquear todas las notificaciones desde una aplicación o puede impedir que una aplicación se está limitando en modo de ahorro de batería.    |    Estas son cosas que no son tan importantes, lo que no debe producirse inmediatamente, pero los usuarios se molestaría si no se están ejecutando en segundo plano.    |    Sincronización de cuentas de correo electrónico secundaria, actualizaciones de icono dinámico.    |
|    Bajo    |    Sí: el usuario puede bloquear todas las notificaciones desde una aplicación o puede impedir que una aplicación se está limitando en modo de ahorro de batería.    |    Notificaciones que solo tienen sentido cuando el usuario está utilizando el dispositivo o cuando la actividad en segundo plano tiene sentido. Estos se almacenan en caché y no se procesan hasta que el usuario inicie sesión en o se conecta en su dispositivo.    |    Póngase en contacto con el estado (en línea o sin conexión)    |
|    Muy baja     |    No: no puede impedir que las notificaciones de muy baja prioridad desde que se está limitando en modo de ahorro de batería.    |    Esto es prácticamente el mismo como de prioridad baja, excepto los usuarios no puede invalidar la directiva de protector de la batería. Estas notificaciones no se enviarán nunca en ahorro de batería.    |    Sincronización de archivos para un servicio de sincronización.    |

Tenga en cuenta que muchas aplicaciones tendrán las notificaciones de prioridad diferentes a lo largo de su ciclo de vida. Puesto que la prioridad se establece en una base por cada notificación, esto no supone un problema. Una aplicación de VoIP puede enviar una notificación de alta prioridad para una llamada entrante y, a continuación, un seguimiento con una prioridad baja uno cuando se conecte un contacto. 

## <a name="setting-the-priority"></a>Establecer la prioridad

Establecer la prioridad en la solicitud de notificación se realiza a través de un encabezado adicional en la solicitud POST, `X-WNS-PRIORITY`. Se trata de un valor entero comprendido entre 0 y 3 que se asigna a una prioridad: 

| Nombre de prioridad | Valor de X-WNS-prioridad | Valor predeterminado para: |
|---------------|----------------------|------------------|
| Alto | 1 | Notificaciones del sistema |
| Meduim | 2 | Iconos y notificaciones |
| Bajo | 3 | Notificación sin procesar |
| Muy baja | 4 |  |

Para que sea con versiones anteriores compatibles, una prioridad no es necesario establecer. En caso de una aplicación no establece la prioridad de sus notificaciones, el sistema proporcionará una prioridad predeterminada. Los valores predeterminados se muestran en el gráfico por encima y emular el comportamiento de las versiones existentes de Windows. 

## <a name="detailed-listing-of-desktop-behavior"></a>Lista detallada de comportamiento del escritorio 

Si va a entregar la aplicación a través de muchos SKU diferentes de Windows, es normalmente es mejor seguir el gráfico en la sección anterior. 

Comportamientos recomendados más específicos para cada prioridad se enumeran a continuación. Esto no es una garantía de que cada dispositivo funcionará exactamente según el gráfico. Los OEM son gratuitos configurar el comportamiento de manera diferente, pero están más cerca de este gráfico. 

| Estado del dispositivo    | PRIORIDAD: Alto    |    PRIORIDAD: Medio        | PRIORIDAD: Bajo    |    PRIORIDAD: Muy baja    |
|-------------------------------------------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|--------------------------|
|    Pantalla o conectado    |    Entregar    |    Entregar    |    Entregar    |    Entregar    |
|    Pantalla desactivada y funciona con batería    |    Entregar    |    Si el usuario exento: entregar Else: lote     |    Si el usuario exento: entregar Else: caché *    |    Memoria caché    |
|    Ahorro de batería habilitado    |    Si el usuario exento: entregar Else: memoria caché    |    Si el usuario exento: entregar Else: memoria caché    |    Si el usuario exento: entregar Else: memoria caché    |    Memoria caché     |
|    Habilitada en la batería y ahorro de batería + pantalla    |    Si el usuario exento: entregar Else: memoria caché    |    Si el usuario exento: entregar Else: memoria caché    |    Si el usuario exento: entregar Else: memoria caché    |    Memoria caché    |

Tenga en cuenta que se entregarán las notificaciones de baja prioridad predeterminada para la pantalla desactivada y la batería sólo para Windows Phone dispositivos basados en. Esto es maintian compatibilidad con la directiva MPNS preexistente. Tenga en cuenta también que las filas de la cuarta y quinta son los mismos, simplemente llamar a escenarios diferentes.

Para excluir una aplicación de ahorro de batería, los usuarios deben ir a la "batería uso mediante la aplicación" en la configuración y seleccione "Permitir la aplicación para ejecutar tareas en segundo plano". Esta selección del usuario exime a la aplicación de ahorro de batería para las notificaciones de prioridad baja, Media y alta. También puede llamar a [BackgroundExecutionManager API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_) para solicitar el permiso del usuario mediante programación.  

## <a name="related-topics"></a>Temas relacionados
- [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](windows-push-notification-services--wns--overview.md)
- [Solicitando permiso para ejecutar en segundo plano](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)
- 
