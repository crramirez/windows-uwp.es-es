---
title: POST ({itemID})
assetID: 2c3c166b-e638-cfb9-d68e-9f8ab9a838d3
permalink: en-us/docs/xboxlive/rest/uri-inventoryconsumablesitemurlpost.html
description: " POST ({itemID})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 877986ce9d48269295a68dbfd644f14785916b88
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640860"
---
# <a name="post-itemid"></a>POST ({itemID})
Indica que se ha utilizado todos o parte de un elemento de inventario consumibles y disminuye la cantidad de los consumibles por la cantidad solicitada.
Es el dominio para estos URI `inventory.xboxlive.com`.

  * [Comentarios](#ID4EX)
  * [Parámetros de URI](#ID4EQB)
  * [Cuerpo de la solicitud](#ID4E2B)
  * [Cuerpo de respuesta](#ID4ENC)

<a id="ID4EX"></a>


## <a name="remarks"></a>Observaciones

   * Si la cantidad que se pide al consumir el llamador supera la fuente de alimentación restante del elemento, se rechazará la llamada.
   * La cantidad que se pide al consumir el llamador debe ser un entero positivo superior a 0. Se rechazarán las llamadas con un valor de consumo de 0 o inferior.
   * Si el autor de la llamada proporciona un identificador de transacción vacía, se rechazará la solicitud.
   * Si está disponible se registrará la notificación de título para que sea posible determinar qué título notifica el consumo.
   * Se omitirán otras publicaciones con el mismo transactionId para un período de tiempo.


> [!NOTE]
> El <b>encabezado x-xbl-contrato-version</b> para esta API es "4".


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| itemID| string| el identificador único para cada usuario para un elemento de inventario singular|

<a id="ID4E2B"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

<a id="ID4EBC"></a>


### <a name="sample-request"></a>Solicitud de ejemplo


```cpp
{
  "transactionId": String
  "removeQuantity": Int
}

```


El campo de cantidad quitar permite al llamador indicar la cantidad de consumibles que desean quitar de la cantidad restante de consumibles. El campo de Id. de transacción proporciona el autor de llamada con un medio para que vuelva a intentarlo con operaciones de contenido consumibles mientras se limita el riesgo de dos veces el mismo uso de recuento.

<a id="ID4ENC"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

La respuesta a la publicación, suponiendo que pasa la autenticación y se asigna el contexto de autorización adecuado es un una acknolodgement de recepción con el mismo transactionId pasa al servicio en la solicitud POST, dirección URL del elemento consumibles y el elemento nuevo valor de cantidad.

<a id="ID4EVC"></a>


### <a name="sample-response"></a>Respuesta de ejemplo


```cpp
{
  "transactionId": String
  "url": String
  "newQuantity": Int
}

```


<a id="ID4E6C"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EBD"></a>


##### <a name="parent"></a>Primario

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)


<a id="ID4ELD"></a>


##### <a name="further-information"></a>Para obtener más información

[Encabezados comunes de EDS](../../additional/edscommonheaders.md)

 [Parámetros de EDS](../../additional/edsparameters.md)

 [Consulta de EDS refinadores](../../additional/edsqueryrefiners.md)

 [URI de Marketplace](atoc-reference-marketplace.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)
