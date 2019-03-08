---
title: Comprobar la configuración de privilegios de usuario en Unity
description: Obtenga información sobre cómo comprobar la configuración de privilegios de con signo en la cuenta de Xbox Live.
ms.assetid: ''
ms.date: 10/26/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, cuentas, cuentas de prueba, el control parental, privilegios de usuario, aplicación prohíbe, incremento de ventas
ms.openlocfilehash: b55ebf9b53cadf2e57317347adce19c3578f9d56
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620410"
---
# <a name="check-user-privilege-settings-in-unity"></a>Comprobar la configuración de privilegios de usuario en Unity
En Xbox Live, cuenta de cada usuario autenticado tiene asociados los privilegios. Privilegios controlan qué características de Xbox Live puede obtener acceso un usuario en un momento dado en el tiempo. Algunos de estos privilegios son características controlado por el sistema, mientras que otros usuarios pueden asociarse con juegos específicos o las suscripciones de la extensión. Además, el control parental y restricciones emitidos por el equipo de cumplimiento de Xbox Live pueden restringir los privilegios de un usuario. Estos privilegios tratan un número de escenarios comunes, incluido el contenido de varios jugadores, al acceder a generados por el usuario, charlar, o para la transmisión de vídeo. Juegos usan esta información para tomar decisiones de control y personalización de acceso.

## <a name="prerequisites"></a>Requisitos previos
Con el fin de determinar la configuración de privilegios de usuario, debe tener configurado el juego para la autenticación con Xbox Live e iniciada correctamente un usuario.

>[!IMPORTANT]
> Si va a probar su juego en el editor de Unity, el juego no está conectado al servicio de Xbox Live y usa datos falsos para simular una conexión. Esto da como resultado un valor null cuando se buscan privilegios. Para probar con datos reales, realice una compilación de la plataforma Universal de Windows de su juego Unity y abra el archivo de proyecto generado en Visual Studio.

Los artículos siguientes describen los pasos que puede llevar a cabo:

* [Inicie sesión en Xbox Live en Unity (compilación y prueba de inicio de sesión)](unity-prefabs-and-sign-in.md#build-and-test-sign-in)
* [Probar la compilación de juegos Unity en Visual Studio](test-visual-studio-build.md)

## <a name="determine-privileges"></a>Determinar los privilegios
Privilegios de un usuario se realizan en el token recibido durante la autenticación. En Unity, puede tener acceso a la lista de privilegios que tiene un usuario en el `XboxLiveUser` clase después de que el usuario inicie sesión correctamente. Privilegios se almacenan como una sola cadena separada por un espacio. Por ejemplo, es posible que vea un usuario con los privilegios siguientes:

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

Debug.Log(XboxLiveUser.User.Privileges);

//Console would read:
// Privileges: "188 191 192 193 194 195 196 198 199 200 201 203 204 205 206 207 208 211 214 215 216 217 220 224 227 228 235 238 245 247 249 252 254 255"
```

Si desea buscar un permiso concreto, también puede comprobar si el `Privileges` propiedad contiene el código asociado:

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

if (XboxLiveUser.User.Privileges.Contains("247"))
{
    Debug.Log("User has the user_created_content privilege");
}
```

## <a name="privilege-codes"></a>Códigos de privilegios
La siguiente es una lista de códigos de privilegios posibles que pueden devolver.

| Código  | Privilegio  | Descripción   |
|------ |-----------------------------  |-------------------    |
| 190   | Difusión             | Puede transmitir en vivo juego.     |
| 197   | view_friends_list     | Puede ver la lista de amigos del otro usuario.   |
| 198   | game_dvr              | Puede cargar grabar vídeos en el juego a la nube.      |
| 199   | share_kinect_content          | Kinect contenido grabado se puede cargar en la nube para el usuario y accesibles para cualquier persona. |
| 203   | multiplayer_parties           | Puede participar en una sesión de entidad.     |
| 205   | communication_voice_ingame    | Puede participar en el chat de voz durante las partes y sesiones de varios jugadores.    |
| 206   | communication_voice_skype     | Puede usar la comunicación por voz con Skype en Xbox One.   |
| 207   | cloud_gaming_manage_session   | Puede asignar y administrar un clúster de cálculo en la nube para una sesión de juegos hospedada.    |
| 208   | cloud_gaming_join_session     | Puede participar en una sesión de proceso en la nube.     |
| 209   | cloud_saved_games     | Puede guardar los juegos en almacenamiento de título en la nube.    |
| 211   | share_content     | Puede compartir contenido con otros usuarios.    |
| 214   | premium_content   | Puede comprar, descargar e iniciar contenido premium disponible con la suscripción de Xbox Live Gold.     |
| 219   | subscription_content  | Puede comprar y descargar contenido de la suscripción premium y usar las características de suscripción premium.     |
| 220   | social_network_sharing    | Puede compartir información de progreso en las redes sociales.    |
| 224   | premium_video     | Puede tener acceso a los servicios de vídeo premium.    |
| 235   | purchase_content  | Puede adquirir el contenido.     |
| 247   | user_created_content  | Puede descargar y ver creado contenido de usuario en línea.    |
| 249   | profile_viewing   | Puede ver perfiles de otros usuarios.   |
| 252   | Comunicaciones    | Puede usar texto asincrónica de mensajería con nadie.    |
| 254   | multiplayer_sessions  | Puede unirse a sesiones para un juego multijugador.   |
| 255   | add_friend    | Puede seguir a otros usuarios de Xbox Live y agregar a amigos de Xbox Live.   |
