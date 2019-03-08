---
title: Plataforma de varios jugadores de Xbox Live
description: Obtenga información acerca de las plataformas de varios jugadores que son compatibles con el Xbox Live.
ms.assetid: 958b94b3-dccd-479a-bf52-54f7ff1656fa
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, varios jugadores
ms.localizationpriority: medium
ms.openlocfilehash: c71fc28a9bb8668d0253834c1a2c9c6ca7736fd1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651160"
---
# <a name="xbox-live-multiplayer-platform"></a>Plataforma de varios jugadores de Xbox Live

La plataforma de varios jugadores de Xbox Live mejora la capacidad de su juego para reunir los reproductores de Xbox Live a través de Internet y puede ampliar considerablemente la vida y el uso de un título más allá de la típica play solo.

Mediante la creación de una gran experiencia multijugador para el público, puede aprovechar la red social grande de jugadores de Xbox Live para aumentar la base para su juego de usuarios y promover a una comunidad sostenida de ventiladores dedicados reproducir juntos.


## <a name="what-is-the-xbox-live-multiplayer-platform"></a>¿Qué es la plataforma de varios jugadores de Xbox Live?

La plataforma de varios jugadores de Xbox Live es un conjunto de API que puede usar para implementar el juego multijugador en tiempo real de cliente. Los subsistemas principales en el conjunto de API son:

-   El **Xbox Live participan varios jugadores sesión directorio (MPSD)** service. El servicio MPSD funciona con experiencias integradas de la interfaz de usuario para facilitar a los usuarios buscar y atrayente entre sí para jugar. Integración con servicios de Xbox Live también permite a los clientes usar Xbox Live entidad Chat para ensamblar.
-   **Instalaciones de contactos simples y avanzadas.** Xbox Live proporciona capacidades de mecha rápida tradicionales, pero también examinar sesiones y soporte técnico para escenarios de contactos altamente personalizados. Xbox Live busca de grupo (LFG) también permite que los reproductores buscar entre sí, rally en Chat de entidad y, a continuación, reproduzca el juego.
-   **Elemento del mismo nivel del mismo nivel y cliente / servidor las API de red** proporcionan una comunicación segura en tiempo real aprovechar los estándares modernos de Internet y supervisa activamente Xbox Live. Normalización y la integración con la red de Xbox Live experiencias de solución de problemas que los usuarios solucionar rápidamente problemas de conectividad.  
-   **Soluciones de chat de voz integrado y texto** que facilitar la comunicación segura de en el juego aprovechando el grafo social de Xbox Live, media services y hardware especializado de codificación en Xbox One dispositivos.

Para obtener información general de algunos de los escenarios más comunes para varios jugadores y qué funcionalidad de Xbox Live puede ayudar a implementar estos escenarios, consulte [niveles multijugador](multiplayer-scenarios.md).

## <a name="how-can-i-implement-xbox-live-multiplayer-in-my-game"></a>¿Cómo puedo implementar varios jugadores de Xbox Live en mi juego?
Según el escenario, la plataforma de varios jugadores de Xbox Live proporciona varios enfoques para implementar varios jugadores de Xbox Live en tu juego.

### <a name="xbox-integrated-multiplayer-xim"></a>Xbox Integrated Multiplayer (XIM)
XIM es una solución llave en mano para mejorar la comunicación y red de varios jugadores en tiempo real para su juego a través de la eficacia de los servicios de Xbox Live. El objetivo de XIM es que sea más fácil para crear juegos multijugador de alta calidad punto a punto (P2P) en Xbox One y Windows 10.

XIM proporciona la funcionalidad siguiente:
- Compatibilidad con invitaciones al juego y contactos simple.
- Red de en tiempo real simple y segura que el servicio de retransmisión de Xbox Live participan varios jugadores aumenta de forma transparente.
- Voz y texto chat simple, con funciones para la transcripción y comentar la comunicación de una experiencia del usuario final sea más accesible.
- Impactos y ayuda para detectar y administrar la congestión de la red, así como para la migración del estado de juego.

XIM es la solución más sencilla de varios jugadores disponible a través de la plataforma de Xbox Live para varios jugadores, pero también la menos personalizable y solo es adecuado para los juegos de P2P. Para obtener más información sobre XIM, consulte [Multiplayer integrado Xbox](xbox-integrated-multiplayer.md).

### <a name="xbox-multiplayer-manager"></a>Administrador de varios jugadores de Xbox
El Administrador de varios jugadores de Xbox (MPM) es un cliente de API que proporciona un acceso flexible a directorio de sesión de varios jugadores del Xbox Live, invitación y servicios de contactos.

Muchos escenarios comunes de varios jugadores implementa de manera eficiente que sigue los procedimientos recomendados y también controla muchos de los requisitos de Xbox (XRs) que debe implementar su juego para conseguir la certificación.

Administrador de varios jugadores de Xbox no implementa una capa de chat o redes. MPM está diseñada como una flexible pero simplificada y consolidada con varios jugadores API de administración para su juego emparejado con una capa de red segura que se implementa a través de Windows.Networking.XboxLive. Se puede agregar en juego la comunicación con el [juego 2 Chat](chat/game-chat-2-overview.md) API o a través de las reservas de Chat de XIM. Las API de Chat 2 juegos y redes se documentan en el XDK una Xbox y Xbox Live extensiones de SDK de la plataforma.

Si va a hospedar servidores dedicados para su juego multijugador, MPM es la mejor opción. MPM también es ideal para escenarios avanzados, como la integración con torneos de Xbox Live. Para obtener más información sobre MPM, consulte [Introducción al administrador de varios jugadores](multiplayer-manager/multiplayer-manager-api-overview.md).

Para usar el Administrador de varios jugadores, debe configurar el servicio Xbox Live para sus escenarios multijugador. Para obtener más información sobre esta configuración, consulte [configurar el servicio de varios jugadores](service-configuration/configure-the-multiplayer-service.md).

>El Administrador de varios jugadores no admite actualmente examinar sesiones. Para obtener información, consulte [examinar de sesión de varios jugadores](session-browse.md).

### <a name="api-capabilites"></a>Funcionalidades de API

Funcionalidad | Varios jugadores integrada de Xbox| Administrador de varios jugadores
--  | -- | --
Visibilidad |  XIM se proporciona como una biblioteca compilada sin origen.  | MPM se proporciona con el origen, por lo que puede personalizar el comportamiento interactuando directamente con los servicios de Xbox Live o con XSAPI.
Sesión y contactos | XIM proporciona reglas simples contactos configurado previamente y no requiere configuración para varios jugadores. | MPM [requiere la configuración del servicio para varios jugadores](service-configuration/configure-the-multiplayer-service.md), que habilita la especificación sofisticada de comportamiento de contactos y la sesión.
Funciones de red | XIM proporciona una red de Reproductor al Reproductor simple y segura, respaldada por el servicio de retransmisión de Xbox Live cuando sea necesario. | MPM está diseñado para que puede conectar su propia solución de red segura mediante Windows.Networking.XboxLive.
Chat de juegos | XIM proporciona chat de voz y texto integrado. | Puede implementarse en juego la comunicación con la API de 2 juegos de Chat o mediante el uso de XIM reservas fuera de banda para habilitar el chat de un MPM administran lista.
