---
title: POST (/handles/query)
assetID: a1a47d49-5c3f-8021-a213-13eb8bddf16a
permalink: en-us/docs/xboxlive/rest/uri-handlesquerypost.html
description: " POST (/handles/query)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7540c117931c01c24c79cef6c8ab6540cb65bbcb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651510"
---
# <a name="post-handlesquery"></a>POST (/handles/query)
Crea consultas para los identificadores de sesión.

> [!IMPORTANT]
> Este método usa la multijugador 2015 y se aplica solo a esa versión multijugador y versiones posteriores. Está pensado para su uso con el contrato de la plantilla 104/105 o posterior y requiere un elemento de encabezado de X-Xbl-contrato-Version: 105/104 o posterior en cada solicitud.

  * [Comentarios](#ID4ET)
  * [Parámetros de URI](#ID4EDB)
  * [Parámetros de cadena de consulta](#ID4EQB)
  * [Códigos de estado HTTP](#ID4EBF)
  * [Cuerpo de la solicitud](#ID4EIF)
  * [Cuerpo de respuesta](#ID4ETF)

<a id="ID4ET"></a>


## <a name="remarks"></a>Observaciones

Este método HTTP/REST crea consultas de datos de identificador solo, sin ninguna información de sesión. Se puede ajustar mediante **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetActivitiesForSocialGroupAsync**.

El campo de tipo en el cuerpo de solicitud para este método debe establecerse en "actividad". Los elementos en el cuerpo de respuesta que se asignan directamente a las propiedades de la **MultiplayerActivityDetails**.

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

<a id="ID4EQB"></a>


## <a name="query-string-parameters"></a>Parámetros de cadena de consulta

Una consulta puede modificarse mediante los parámetros de cadena de consulta en la tabla siguiente.

| <b>Parámetro</b>| <b>Tipo</b>| <b>Descripción</b>|
| --- | --- | --- | --- |
| Palabra clave| string| Una palabra clave, por ejemplo, "foo", que se debe encontrar en las sesiones o plantillas si van a recuperar. |
| xuid| entero sin signo de 64 bits| El identificador de usuario de Xbox, por ejemplo, "123", para las sesiones incluir en la consulta. De forma predeterminada, el usuario debe ser activo en la sesión para que esta se incluyera. |
| reservas de direcciones| boolean| True para incluir las sesiones para el que el usuario se establece como un reproductor reservado pero no se ha unido para convertirse en un Reproductor de active. Este parámetro solo se usa al consultar sus propias sesiones, o cuando se consultan las sesiones de para servidores un usuario específico. |
| inactivo| boolean| True para incluir las sesiones que el usuario ha aceptado, pero no se reproduce activamente. Las sesiones en que el usuario es "listo" pero no "activo" cuentan como inactivas. |
| privado| boolean| True para incluir las sesiones privadas. Este parámetro solo se usa al consultar sus propias sesiones, o cuando se consultan las sesiones de para servidores un usuario específico. |
| visibility| string| Estado de visibilidad para las sesiones. Los valores posibles se definen mediante la <b>MultiplayerSessionVisibility</b>. Si este parámetro se establece en "open", la consulta debe incluir las sesiones abiertas solo. Si se establece en "private", la <i>privada</i> parámetro debe establecerse en true. |
| version| entero de 32 bits con signo| La versión máxima de la sesión que se debe incluir. Por ejemplo, un valor de 2 especifica que solo sesiones con una versión de sesión principal de 2 o menos se debe incluir. El número de versión debe ser menor o igual que la versión mod 100 de la solicitud contrato. |
| Take| entero de 32 bits con signo| El número máximo de sesiones para recuperar. Este número debe estar entre 0 y 100. |


Configuración *privada* o *reservas* a true requiere acceso de nivel de servidor a la sesión. Como alternativa, esta configuración requiere XUID del llamador de notificaciones para que coincida con el filtro XUID en el URI. En caso contrario, se devuelve el código de estado HTTP/403, si las sesiones de este tipo realmente existen o no.

<a id="ID4EBF"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP
El servicio devuelve un código de estado HTTP tal como se aplica a MPSD.  
<a id="ID4EIF"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Para todas las actividades de gráfico social "Favoritos" de un usuario, el cuerpo de solicitud tiene este aspecto:


```cpp
{
  "type": "activity",
  "scid": "B5B1F71F-A328-4371-89E0-C3AD222D0E92"  // optional filter on scid
  "owners": {
    "people": {
      "moniker": "favorites",
      "monikerXuid": "3210"
    }
  }
}

```


<a id="ID4ETF"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

Los controladores se recuperan como una matriz de estructuras de identificador, con un identificador único incrustado en cada estructura.


```cpp
{
  "results": [
    {
      "id": "11111111-ebe0-42da-885f-033860a818f6",
      "type": "activity",
      "version": 1,
      "sessionRef": {
        "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
        "templateName": "party",
        "name": "e3a836aeac6f4cbe9bcab985494d3175"
      },
      "titleId": "1234567",
      "ownerXuid": "3212",
      {
        "id": "11111111-ebe0-42da-885f-033860a818f7",
      "type": "activity",
      "version": 1,
      "sessionRef": {
         "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
        "templateName": "TitleStorageTestDefault",
        "name": "795fcaa7-8377-4281-bd7e-e86c12843632"
      },
      "titleId": "1234567",
      "ownerXuid": "3212",
    }
  }]
}

```


<a id="ID4E4F"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4E6F"></a>


##### <a name="parent"></a>Primario

[/handles/query](uri-handlesquery.md)
