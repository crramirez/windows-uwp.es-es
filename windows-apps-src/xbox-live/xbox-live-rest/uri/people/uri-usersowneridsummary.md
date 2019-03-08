---
title: /users/{ownerId}/summary
assetID: 63f8ed09-532d-381e-59e6-2849893df5bf
permalink: en-us/docs/xboxlive/rest/uri-usersowneridsummary.html
description: " /users/{ownerId}/summary"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f8ad32fb2033c97a408ccb0f6cc6871b01caf5c5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617810"
---
# <a name="usersowneridsummary"></a>/users/{ownerId}/summary
Acceso a los datos resumen acerca del propietario de la perspectiva del llamador.

  * [Parámetros de URI](#ID4EQ)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| ownerId| string| Identificador del usuario cuyo recurso se está obteniendo acceso. Los valores posibles son "me", xuid({xuid}) o gt({gamertag}). Valores de ejemplo: <code>me</code>, <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>|

<a id="ID4ESB"></a>


## <a name="valid-methods"></a>Métodos válidos

[GET (/users/{ownerId}/summary)](uri-usersowneridsummaryget.md)

&nbsp;&nbsp;Obtiene los datos de resumen sobre el propietario desde la perspectiva del llamador.

<a id="ID4E3B"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4E5B"></a>


##### <a name="parent"></a>Primario

[/users/{ownerId}/summary](uri-usersowneridsummaryget.md)
