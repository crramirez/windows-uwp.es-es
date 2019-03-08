---
title: Enumeración de PermissionId
assetID: 3e7ee317-4946-1105-ecd7-1e26346deccb
permalink: en-us/docs/xboxlive/rest/privacy-enum-permissionid.html
description: " Enumeración de PermissionId"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aff463e2268af972536984a00e29348bf0a6859e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594690"
---
# <a name="permissionid-enumeration"></a>Enumeración de PermissionId
Detalles de la enumeración PermissionId.
Los identificadores de permiso se pueden usar con las direcciones URL de validación de permiso:

   * [GET (/users/{requestorId}/permission/validate)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidateget.md)
   * [POST (/users/{requestorId}/permission/validate)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidatepost.md)

Estos identificadores incluyen comprobaciones directas con una configuración concreta para un usuario, como la comprobación de una configuración de privacidad solo de un destino o un actor único privilegio. Además, hay identificadores que puede utilizarse con el permiso API e incorporar las comprobaciones contra varias opciones para las acciones de usuario específico de permiso.

<a id="ID4EIB"></a>


## <a name="permissions"></a>Permisos

Estos son los valores que un llamador puede usar para comprobar si se puede realizar una acción específica. A diferencia de la configuración anterior, estos encapsulan las directivas definidas por el servicio y no se puede cambiar directamente los usuarios, aunque en la mayoría de los casos, las directivas se basan en uno o más valores cuyos valores se definen por los usuarios. Estos son comprobaciones normalmente compuestas de más de un parámetro definido anteriormente. Por ejemplo: El <b>ViewProfile</b> permiso realiza una comprobación de que el destino <b>ShareProfile</b> configuración de privacidad y el solicitante <b>AllowProfileViewing</b> con privilegios.

En general, se recomienda que los llamadores de la solicitud un identificador de permiso para las acciones que deben comprobarse en lugar de comprobar directamente los privilegios y la configuración de privacidad. Esto permite cambiar constantemente a través del servicio tal y como se incorporan nuevas comprobaciones de las directivas de privacidad.

| Nombre del permiso| Descripción|
| --- | --- |
| CommunicateUsingText| Compruebe si el usuario puede enviar un mensaje con el contenido de texto para el usuario de destino|
| CommunicateUsingVideo| Compruebe si el usuario puede comunicarse mediante vídeo con el usuario de destino|
| CommunicateUsingVoice| Compruebe si el usuario puede comunicarse mediante voz con el usuario de destino|
| ViewTargetProfile| Compruebe si el usuario puede ver el perfil del usuario de destino|
| ViewTargetGameHistory| Compruebe si el usuario puede ver el historial de juegos del usuario de destino|
| ViewTargetVideoHistory| Compruebe si el usuario puede ver el vídeo ver un historial detallado del usuario de destino|
| ViewTargetMusicHistory| Compruebe si el usuario puede ver el historial de escucha música detallada del usuario de destino|
| ViewTargetExerciseInfo| Compruebe si el usuario puede ver la información de ejercicio del usuario de destino|
| ViewTargetPresence| Compruebe si el usuario puede ver el estado de conexión del usuario de destino|
| ViewTargetVideoStatus| Compruebe si el usuario puede ver los detalles del estado de vídeo de destinos (presencia en línea ampliada)|
| ViewTargetMusicStatus| Compruebe si el usuario puede ver los detalles del estado de música de destinos (presencia en línea ampliada)|
| ViewTargetUserCreatedContent| Compruebe si el usuario puede ver el contenido creado por el usuario de otros usuarios|
