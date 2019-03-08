---
title: Configuración del servicio de multijugador
description: Obtenga información sobre cómo configurar el servicio de varios jugadores de Xbox Live.
ms.assetid: d042d4d5-1c75-4257-8a6f-07eddd39ca7e
ms.date: 07/12/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, varios jugadores, configuración del servicio, plantilla de sesión, cadena de invitación personalizado, hopper smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: bf829069824443cdc1c8c0658fcfdfcbe72d0b93
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655880"
---
# <a name="multiplayer-service-configuration"></a>Configuración del servicio de multijugador
En el orden del título aprovechar las ventajas de los servicios brindados Xbox Live, primero debe definir la configuración del servicio. Esta configuración de servicio existe en el servicio de nube de Xbox Live y define cómo interactúa el servicio Xbox Live con todos los dispositivos que ejecutan el título y de juegos.

Para los servicios multijugador, hay tres aspectos de varios jugadores que puede configurar:
* Plantillas de sesión
* SmartMatch Hoppers
* Cadenas de invitación personalizado

## <a name="session-templates"></a>Plantillas de sesión
El servicio de varios jugadores de Xbox permite jugadores crear y unir las sesiones, para intercambiar mensajes de la sesión con otros jugadores en la misma sesión y para registrar los resultados de su juego a la sesión. (Registro de los resultados limpia la sesión y también actualiza las tablas de clasificación para todos los jugadores en la sesión).

Por ejemplo, una sesión de varios jugadores podría ser un solo juego de ajedrez entre dos jugadores. Como alternativa, podría estar una sesión continua de una acción y título desempeñado por un número mucho mayor de jugadores de adventure.

Cuando un juego, crea una nueva sesión, crea la sesión que se basa en una plantilla de sesión predefinido. Esta plantilla es esencialmente un objeto JSON que contiene atributos que describen la sesión.

Cuando se crea una plantilla nueva sesión, debe definir lo siguiente:

| Campo | Descripción |
| --- | --- |
| Nombre de la sesión | Escriba un nombre que caracteriza a la plantilla de sesión de varios jugadores, y que recordará y reconocer fácilmente. El nombre debe ser una cadena de texto con un máximo de 100 caracteres. |
| Versión del contrato | Este valor está rellena automáticamente por el sistema y denota la versión actual del sistema del contrato JSON. No lo edites. |
| Plantilla de sesión (texto JSON) | Especifique los datos JSON que describen los diferentes atributos asociados a una sesión de varios jugadores. |

Para obtener más información acerca de las plantillas de sesión de varios jugadores, incluidas las diversas plantillas predefinidas que puede usar como punto de partida para el texto JSON, consulte [plantillas de sesión de varios jugadores](session-templates.md).

> **Importante:** Después de un título pasa certificación Final, sesiones de varios jugadores existentes en dicho título ya no se pueden modificar o eliminar.

## <a name="smartmatch-hoppers"></a>SmartMatch hoppers

Una adición opcional para el servicio de varios jugadores Xbox es el servicio de contactos basado en servidor Xbox, que proporciona un método de agrupación jugadores juntos según la información proporcionada por el título o almacenados en las estadísticas de usuarios o según las preferencias del usuario, o según la calidad de servicio.

Como contactos de Xbox One está basado en servidor, los usuarios pueden proporcionar una solicitud al servicio y, a continuación, se notifica más adelante, siempre que se encuentra una coincidencia. Es decir: no se obliga al usuario para esperar en el título mientras se realiza el proceso de contactos, son gratuitas reproducir la parte solo jugador de su título, o incluso para reproducir otros títulos y aún ser candidatos para los contactos. Esto elimina la necesidad de lograr una "masa crítica" de jugadores antes de que se pueden encontrar coincidencias.

Un hopper contactos debe basarse en una plantilla de sesión definidos previamente.

Cuando se crea un nuevo hopper contactos, debe definir lo siguiente:

| Campo | Descripción |
|---|---|
|Nombre| Escriba un nombre que caracteriza la hopper contactos y que recordará y reconocer fácilmente. El nombre debe ser una cadena de texto con un máximo de 140 caracteres. |
| Tamaño de grupo mín. | Especifique el número mínimo aceptable de jugadores. Valor mínimo es 1. |
| Tamaño de grupo máximo | Especifique el número máximo aceptable de jugadores. Valor máximo es 256. |
| Debe ciclos de expansión de regla | El valor predeterminado es 3. El valor predeterminado no debería necesitar ser cambiados rellenados normal del Reproductor. |
| Una clasificación hopper | Si un hopper está marcado como un rango Hopper permite a los jugadores en ese hopper deben coincidir entre sí aunque haya bloqueado entre sí. Esto ayuda a impedir que los usuarios intenten evitar los reproductores con mayor capacidad admitirlos. |
| Actualización automática de sesión | Cuando este campo está habilitado, los cambios realizados a la lista de miembros de la sesión o propiedades personalizadas de los miembros se propagarán automáticamente a una incidencia enviada anteriormente. |

> **Importante:** Después de un título pasa certificación Final, hoppers de contactos existente en dicho título ya no se pueden modificar o eliminar.

## <a name="custom-invite-strings"></a>Cadenas de invitación personalizado
Cuando el título envía una invitación a un reproductor para unirse a un juego multijugador, puede elegir mostrar una cadena de texto de invitación personalizado en lugar de la cadena de invitación de forma predeterminada.

Cuando se crea una nueva cadena de invitación personalizado, debe definir lo siguiente:

| Campo | Descripción |
|---|---|
| Id. | El Id. de personalizado invitar a la cadena que se usará para identificar la cadena. automáticamente se anexará "custominvitestrings_" al principio de su identificador. Valor máximo es 100 caracteres |
| Valor | El texto de la cadena de invitación personalizado que aparecerá en la notificación del sistema de invitación personalizado. Valor máximo es 100 caracteres |

## <a name="additional-information"></a>Información adicional

Para obtener más información acerca de cómo configurar el servicio de varios jugadores, consulte los artículos siguientes:

**Artículo** | **Descripción**
--- | ---
[Configurar el AppXManifest para varios jugadores](configure-your-appxmanifest-for-multiplayer.md) | Describe cómo configurar un archivo AppXManifest de UWP para que funcione con el servicio de varios jugadores de Xbox Live.
[Plantillas de sesión de varios jugadores](session-templates.md) | Ofrece una breve descripción de las plantillas de sesión de varios jugadores y proporciona varios ejemplos de plantillas que puede copiar y modificar para las sesiones de varios jugadores.
[Constantes de la plantilla de sesión](session-template-constants.md) | Describe los elementos predefinidos de una plantilla de sesión de varios jugadores.
[Sesiones de gran tamaño](large-sessions.md) | Describe cuándo y cómo utilizar las sesiones de gran tamaño.
