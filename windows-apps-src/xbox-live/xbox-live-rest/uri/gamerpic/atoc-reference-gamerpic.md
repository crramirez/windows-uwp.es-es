---
title: URI de Gamerpic
assetID: 811ab696-c433-aa54-90d8-66614ad09901
permalink: en-us/docs/xboxlive/rest/atoc-reference-gamerpic.html
description: " URI de Gamerpic"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a8ba79784ed73ae62e7fe8d65c626c3ebc6003a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618390"
---
# <a name="gamerpic-uris"></a>URI de Gamerpic
 
Esta sección proporcionan detalles sobre las direcciones de Gamerpic el identificador de recursos Universal (URI) y los métodos de protocolo de transporte de hipertexto (HTTP) asociados de servicios de Xbox Live para *gamerpics*.
 
Es el dominio para estos URI `gamerpics.xboxlive.com`.
 
El Gamerpic Service se ha diseñado para proporcionar a los usuarios más opciones de personalización, mediante la concesión de un título de la capacidad de permitir que el usuario generar un gamerpic de los personajes del juego (un carácter del juego en este escenario se refiere a un protagonista en juego; podría ser una persona un automóvil, una nave espacial o cualquier otra entidad que controla el usuario en el título).
 
El flujo básico de generar un gamerpic título es como sigue:
 
   * El título proporciona al usuario la capacidad para crear una imagen de sus caracteres del juego. 
     * Si no es así, el título puede del mensaje, a continuación, el usuario que no tienen los privilegios adecuados.
     * Si el usuario tiene el privilegio, el usuario puede continuar crear su gamerpic caracteres.
  
   * El usuario crea la imagen y el título, envía el archivo .png de 1080 x 1080 al servicio gamerpic.
   * El servicio almacena la imagen y establece la imagen como gamerpic nueva del usuario.
   * Las experiencias de llamada de gamerpic del usuario obtendrá la imagen actualizada.
  
La capacidad de establecer un gamerpic título se controla mediante un privilegio exclusivo del cumplimiento (211). Si la aplicación revoca el privilegio, el usuario se impedirá al guardar un gamerpic título y el servicio devolverá 403. Los títulos deberían llamar a CheckPrivilege para comprobar que los usuarios tienen permiso para compartir contenido (priv 211).
 
Actualmente, para poder usar este servicio, el título debe tener en la lista blanca. Para solicitar la aprobación, enviar por correo electrónico `slsgamerpics@microsoft.com`.
 
<a id="ID4EGC"></a>

 
## <a name="in-this-section"></a>En esta sección

[/users/me/gamerpic](uri-usersmegamerpic.md)

&nbsp;&nbsp;Tiene acceso a un gamerpic 1080 x 1080.
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)

   