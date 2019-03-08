---
title: POST (/users/{requestorId}/permission/validate)
assetID: 7a5ea583-ffca-5da7-a02a-535c52535928
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidatepost.html
description: " POST (/users/{requestorId}/permission/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: edd91560ffb5d81b30da4b1453612cc5853a456f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623430"
---
# <a name="post-usersrequestoridpermissionvalidate"></a>POST (/users/{requestorId}/permission/validate)
Obtiene un conjunto de respuestas de sí o no acerca de si el usuario puede realizar las acciones especificadas con un conjunto de usuarios de destino.

  * [Comentarios](#ID4EQ)
  * [Parámetros de URI](#ID4ECB)
  * [Autorización](#ID4ENB)
  * [Encabezados de solicitud necesarios](#ID4ESC)
  * [Cuerpo de la solicitud](#ID4E4D)
  * [Códigos de estado HTTP](#ID4ETE)
  * [Encabezados de respuesta necesaria](#ID4EIG)
  * [Cuerpo de respuesta](#ID4E5H)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Observaciones

El cuerpo de solicitud toma una lista de usuarios y una lista de configuraciones, y el resultado es un resultado permitido o bloqueado por cada par de configuración del usuario.

En escenarios de varios jugadores entre la red (donde las comprobaciones de las comunicaciones de privacidad deben realizarse entre los usuarios que tienen un Id. de usuario de Xbox (XUID) y fuera de la red que no lo hacen), consulte [PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md) para los tipos de usuario.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| requestorId| string| Obligatorio. Identificador del usuario que realiza la acción. Los valores posibles son <code>xuid({xuid})</code> y <code>me</code>. Esto debe ser un usuario ha iniciado sesión. Valor de ejemplo: <code>xuid(0987654321)</code>.|

<a id="ID4ENB"></a>


## <a name="authorization"></a>Autorización

Notificaciones de autorización usa | Notificación| Tipo| ¿Obligatorio?| Valor de ejemplo|
| --- | --- | --- | --- | --- | --- | --- |
| Xuid| entero con signo de 64 bits| sí| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>Encabezados de solicitud necesarios

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valores de ejemplo: <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Valor de ejemplo: 1.|

<a id="ID4E4D"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

<a id="ID4EDE"></a>


### <a name="required-members"></a>Miembros requeridos

Consulte [PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md).


```cpp
{
    "users":
    [
        {"xuid":"12345"},
        {"xuid":"54321"}
    ],
    "permissions":
    [
        "ViewTargetGameHistory",
        "ViewTargetProfile"
    ]
}

```


<a id="ID4ETE"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP

El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| Aceptar| La sesión se ha recuperado correctamente.|
| 400| La solicitud no es válida.| Ejemplos: los identificadores de configuración incorrecta, los URI incorrecto, etcetera.|
| 404| El usuario especificado en el URI no existe.| No se encontró el recurso especificado.|

<a id="ID4EIG"></a>


## <a name="required-response-headers"></a>Encabezados de respuesta necesaria

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| string| El tipo MIME del cuerpo de la solicitud. Valor de ejemplo: <code>application/json</code>|
| Content-Length| string| El número de bytes que se va a enviar en la respuesta. Valor de ejemplo: 34|
| Cache-Control| string| Solicitud normal desde el servidor para especificar el comportamiento de almacenamiento en caché. Ejemplo: <code>no-cache, no-store</code>|

<a id="ID4E5H"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

Consulte [PermissionCheckBatchResponse (JSON)](../../json/json-permissioncheckbatchresponse.md).

<a id="ID4ELAAC"></a>


### <a name="sample-response"></a>Respuesta de ejemplo


```cpp
{
    "responses":
    [
        {
            "user": {"xuid":"12345"},
            "permissions":
            [
                {
                    "isAllowed":true
                },
                {
                    "isAllowed":true
                }
            ]
        },
        {
            "user": {"xuid":"54321"},
            "permissions":
            [
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"NotAllowed"}
                    ]
                },
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"PrivilegeRest", "restrictedSetting":"AllowProfileViewing"}
                    ]
                }
            ]
        }
    ]
}

```


<a id="ID4EVAAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EXAAC"></a>


##### <a name="parent"></a>Primario

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [Enumeración PermissionId](../../enums/privacy-enum-permissionid.md)
