---
title: /users/{requestorId}/permission/validate
assetID: 400a9721-bf43-76df-4cd1-9f2ae6ca5035
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidate.html
description: " /users/{requestorId}/permission/validate"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4a062fd417bae37fd66c944e0e534ef7a50de5fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599370"
---
# <a name="usersrequestoridpermissionvalidate"></a>/users/{requestorId}/permission/validate
 
  * [Parámetros de URI](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| requestorId| string| Obligatorio. Identificador del usuario que realiza la acción. Los valores posibles son <code>xuid({xuid})</code> y <code>me</code>. Esto debe ser un usuario ha iniciado sesión. Valor de ejemplo: <code>xuid(0987654321)</code>.| 
  
<a id="ID4ETB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/users/{requestorId}/permission/validate)](uri-privacyusersrequestoridpermissionvalidateget.md)

&nbsp;&nbsp;Obtiene una respuesta Sí o no acerca de si el usuario puede realizar la acción especificada con un usuario de destino.

[POST (/users/{requestorId}/permission/validate)](uri-privacyusersrequestoridpermissionvalidatepost.md)

&nbsp;&nbsp;Obtiene un conjunto de respuestas de sí o no acerca de si el usuario puede realizar las acciones especificadas con un conjunto de usuarios de destino.
 
<a id="ID4EAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ECC"></a>

   [URI de privacidad](atoc-reference-privacyv2.md)

 [Enumeración PermissionId](../../enums/privacy-enum-permissionid.md)

   