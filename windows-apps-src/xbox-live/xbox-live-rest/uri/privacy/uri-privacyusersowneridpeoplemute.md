---
title: /users/{ownerId}/people/mute
assetID: efb929d8-79a7-83f0-c348-c92ced42bc05
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemute.html
description: " /users/{ownerId}/people/mute"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 86010d11ff0a7ebe188766f51bc3160a4f2befa4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641550"
---
# <a name="usersowneridpeoplemute"></a>/users/{ownerId}/people/mute
Tiene acceso a la lista de silencio de un usuario.

  * [Parámetros de URI](#ID4EQ)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| ownerId| string| Obligatorio. Identificador del usuario cuyo recurso se está obteniendo acceso. Los valores posibles son "me" <code>xuid({xuid})</code>, o gt({gamertag}). Debe ser el usuario autenticado. Valores de ejemplo: <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>. Tamaño máximo: ninguno. |

<a id="ID4ETB"></a>


## <a name="valid-methods"></a>Métodos válidos

[GET (/users/{ownerId}/people/mute)](uri-privacyusersowneridpeoplemuteget.md)

&nbsp;&nbsp;Obtiene la lista de silencio de un usuario.

<a id="ID4E4B"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4E6B"></a>


##### <a name="parent"></a>Primario

[URI de privacidad](atoc-reference-privacyv2.md)
