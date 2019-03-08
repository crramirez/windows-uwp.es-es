---
title: Autenticación para los proyectos XDK
description: Obtenga información sobre cómo iniciar sesión en Xbox Live, los usuarios en un título en el Kit de desarrollo de Xbox (XDK).
ms.assetid: 713bb2e3-80c5-4ac9-8697-257525f243d3
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 597b3becfa2083955d8bd4e0adc91e4ae9b827a1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613500"
---
# <a name="authentication-for-xdk-projects"></a>Autenticación para los proyectos XDK

Para aprovechar las características de Xbox Live en los juegos, un usuario debe crear un perfil de Xbox Live para identificarse en la Comunidad de Xbox Live.  Servicios de Xbox Live realizar un seguimiento de juego relacionados con actividades mediante ese perfil de Xbox Live, como nombre de jugador y la imagen jugador, que son de amigos de juegos del usuario, el usuario lo que el usuario de juegos ha desempeñado, qué logros que el usuario ha desbloqueado, donde el usuario se encuentra en el marcador para un determinado etcetera juego.

En un nivel alto, usa las API de Xbox Live siguiendo estos pasos:
1. Identificar al usuario interactuar
2. Crear un contexto de Xbox Live en función del usuario
3. Use el contexto de Xbox Live para tener acceso a servicios de Xbox Live
4. Cuando el usuario o el programa sale del juego signos horizontal, liberar el objeto de XboxLiveContext si se establece en null

### <a name="creating-an-xboxliveuser-object"></a>Creación de un objeto XboxLiveUser
La mayoría de las actividades de Xbox Live está relacionados con los usuarios de Xbox Live.  Como desarrollador de juegos, deberá crear primero un objeto XboxLiveUser para representar al usuario local.

C++:
```
Windows::Xbox::System::User^ user; // the interacting user.  From User::Users, etc
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>( user );
```

WinRT:
```
Windows::Xbox::System::User^ user; // the interacting user.  From User::Users, etc
Microsoft::Xbox::Services::XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext( user );
```
