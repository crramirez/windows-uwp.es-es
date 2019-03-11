---
title: Información general sobre juegos de Chat 2
description: Obtenga información sobre cómo agregar la comunicación por voz a su juego mediante el uso de Xbox Live Game Chat 2, una versión actualizada de Chat, juegos.
ms.date: 10/20/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, juegos chat, juegos chat 2, comunicación por voz
ms.localizationpriority: medium
ms.openlocfilehash: 9f013f8b80cc7bca367c3ef5cd2c0d1da86cc98c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615830"
---
# <a name="game-chat-2-overview"></a>Información general de Chat juego 2

Juego 2 Chat le permite agregar fácilmente la comunicación de chat de voz y texto a la aplicación mientras respetando la configuración de privacidad de los jugadores y que cumplen los requisitos de Xbox para uno de los juegos Xbox y aplicaciones de concentrador relativas a la charla de voz y texto. Para reproductores que han habilitado la conversión de texto a voz o texto a voz a través de la facilidad de acceso - configuración de la transcripción del Chat de juego, 2 de Chat de juego de forma transparente realizará traducciones para crear mensajes de texto que representa la entrada de audio de voz y play de chat sintetizada de audio para los mensajes de texto salientes de chat, voz, respectivamente.

> [!NOTE]
> Si desea obtener una referencia de API específica, puede encontrar en el archivo descargable de Xbox Live API ayuda HTML compilado (.chm) [aquí](https://aka.ms/xboxliveuwpdocs).

- **Una relación de comunicación** -2 de Chat de juego le ofrece un mayor control sobre cómo los jugadores pueden comunicarse con los demás. En vez de especificar los equipos o los canales, 2 de Chat de juego requiere que se defina relaciones explícitas entre cada par de usuarios. Una relación de comunicación 2 Chat juego admite uni y la comunicación bidireccional entre cualquier par de jugadores. Una relación de comunicación de voz y texto se puede establecer independientemente entre sí.

- **Accesibilidad** -2 de Chat de juego admite texto a voz y texto a voz. Cuando se habilita, 2 de Chat de juego respetará la preferencia "Transcripción del Chat de juego" de los jugadores y realizará de forma transparente traducciones para crear mensajes de texto que representa el audio de voz entrante y sintetizada de reproducción de audio de voz para texto saliente de chat de chat mensajes, respectivamente.

- **Integración de Xbox Live** -2 de Chat de juego utiliza servicios de Xbox Live para garantizar que se respeten las preferencias de cada jugador y los privilegios.

- **Detección de actividad de voz** -2 de juego Chat realiza la detección de actividad de voz para determinar si los datos de audio incluyen actividad de voz.

- **Control automático de ganancia** -2 de Chat de juego realiza el Control automático de ganancia para minimizar la variación en la salida del micrófono de un usuario.

- **Los códecs** -2 de Chat de juego codifica los datos de audio que se deben entregar en instancias remotas de la aplicación. En Xbox One, esta codificación (y descodificación en el extremo receptor) es la aceleración de hardware.

- `chat_manager::start/finish_processing_state_changes` -El par de métodos de la aplicación llama cada fotograma de la interfaz de usuario para realizar operaciones asincrónicas, para recuperar los resultados que se tratan de forma de `game_chat_state_change` estructuras y, a continuación para liberar los recursos asociados cuando termine.

- `chat_manager::start/finish_processing_data_frames` -El par de métodos que se usan para conectar el 2 de Chat de juego en la capa de transporte de la aplicación. Estos métodos son invocados por la aplicación en cada fotograma de la red para recuperar y distribuir `game_chat_data_frame` objetos a las instancias de la aplicación en dispositivos remotos y, a continuación para liberar los recursos asociados cuando termine.

- `chat_manager::process_incoming_data` -El método utilizado para proporcionar datos a 2 Chat de juego que se ha entregado a través de capa de transporte de la aplicación desde una instancia remota de juego Chat 2.

- **Manipulación de Audio en tiempo real** -2 de Chat de juego permite que la aplicación se inserte en la canalización de audio de chat, por lo que puede inspeccionar o manipular los datos de audio de chat. Consulte [manipulación de audio en tiempo real](real-time-audio-manipulation.md) para obtener más información.

La aplicación informa a la biblioteca de los usuarios en el dispositivo local y en dispositivos remotos que se esperan que hablar. La aplicación, a continuación, configura las relaciones entre cada usuario.

Una vez configurado, 2 de Chat de juego sondea el audio de microphone(s) los usuarios locales, realiza la detección de actividad de voz y el control automático de ganancia, codifica los datos y luego expone los datos de audio que se entregará a instancias remotas de juego Chat de 2 a través de `chat_manager::start/finish_processing_data_frames`. La aplicación debe entregar los datos contenidos en el `game_chat_state_change` objeto para las instancias remotas del 2 de Chat de juego especificada en el mismo objeto. Al recibir los datos, las instancias remotas de la aplicación deben enviar los datos a su propia instancia de juego Chat de 2 a través de `chat_manager::process_incoming_data`. Juego 2 Chat descodifica los datos entrantes y, a continuación, se presenta al dispositivo de audio del usuario local.

Juego 2 Chat aplica requisitos de privacidad y con privilegios Xbox Live. Los usuarios que no tienen privilegios de las comunicaciones no generará datos de audio y datos de audio no se representará de los usuarios que están bloqueados a través de la configuración de privacidad.

Para empezar, vea [con juego de charla 2](using-game-chat-2.md). Si usas C#, consulte [utilizando juego Chat 2 WinRT proyecciones](using-game-chat-2-winrt.md). Si ya tiene una implementación de Chat de juego, use el [Guía de migración de juego Chat 2](game-chat-2-migration.md) para asignar el juego Chat conceptos y patrones que realiza la llamada a análogas 2 de Chat de juego.

Para obtener información acerca de la API del 2 de Chat de juego, descargue la referencia de API de Xbox: [XboxAPIs.zip](https://aka.ms/xboxliveuwpdocs)