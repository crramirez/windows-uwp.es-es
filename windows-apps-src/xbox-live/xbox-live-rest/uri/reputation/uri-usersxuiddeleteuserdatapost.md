---
title: POST (/users/xuid({xuid})/deleteuserdata)
assetID: 8be13ff9-5d42-43a1-f2fa-d550d845a552
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddeleteuserdatapost.html
description: " POST (/users/xuid({xuid})/deleteuserdata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dab43079dbba3729ff39f3a2116c377c3b73142a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604070"
---
# <a name="post-usersxuidxuiddeleteuserdata"></a>POST (/users/xuid({xuid})/deleteuserdata)
Restablece completamente los datos de reputación de un usuario de prueba. Solo para pruebas.

  * [Comentarios](#ID4EQ)
  * [Parámetros de URI](#ID4E5)
  * [Autorización](#ID4EJB)
  * [Encabezados de solicitud necesarios](#ID4E3B)
  * [Códigos de estado HTTP](#ID4EHC)
  * [Cuerpo de respuesta](#ID4EJF)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Observaciones

Llamar a esta API se quitará todos los elementos de comentarios y datos de reputación de un usuario. Los asociados pueden llamar a esta API en cualquier espacio aislado, excepto la venta directa. El equipo de la aplicación puede llamar a esta API con cualquier identificador de espacio aislado.

Es el dominio para estos URI `reputation.xboxlive.com`. Este URI siempre se llama en el puerto 10443.

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| xuid| entero sin signo de 64 bits| Id. de usuario de Xbox (XUID) del usuario cuyos datos se está eliminando.|

<a id="ID4EJB"></a>


## <a name="authorization"></a>Autorización

El espacio aislado de venta directa, **PartnerClaim** desde el equipo de cumplimiento.

Para todos los demás espacios aislados, **PartnerClaim** y **SandboxIdClaim**.

<a id="ID4E3B"></a>


## <a name="required-request-headers"></a>Encabezados de solicitud necesarios

**Tipo de contenido: application/json** y **X-Xbl-contrato-Version** (versión actual es 101).

<a id="ID4EHC"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP

El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descripción|
| --- | --- | --- | --- | --- | --- |
| 200| Aceptar| La sesión se ha recuperado correctamente.|
| 400| solicitud incorrecta| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.|
| 401| Sin autorización| La solicitud requiere autenticación del usuario.|
| 404| No encontrado| No se encontró el recurso especificado.|
| 500| error interno del servidor| El servidor encontró una condición inesperada que le impidió satisfacer la solicitud.|
| 503| servicio no disponible| Se ha limitado la solicitud, intente la solicitud de nuevo después del valor de reintento del cliente en segundos (por ejemplo, 5 segundos más tarde).|

<a id="ID4EJF"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

Ninguno si se ejecuta correctamente; en caso contrario, un [depuracuión puede contener (JSON)](../../json/json-serviceerror.md) documento.

<a id="ID4EWF"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EYF"></a>


##### <a name="parent"></a>Primario

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)
