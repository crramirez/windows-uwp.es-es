---
title: /users/me/consumables/{itemID}
assetID: 45724827-5e35-326f-3f17-f49e606d9e08
permalink: en-us/docs/xboxlive/rest/uri-inventoryconsumablesitemurl.html
description: Punto de conexión REST para fungible de Xbox para un usuario.
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4822180f11879417be67351f901474a38f89d82e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614090"
---
# <a name="usersmeconsumablesitemid"></a>/users/me/consumables/{itemID}
El conjunto completo de los detalles de un elemento de inventario consumibles específico de accesos.
Es el dominio para estos URI `inventory.xboxlive.com`.

  * [Parámetros de URI](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| itemID| string| el identificador único para cada usuario para un elemento de inventario singular|

<a id="ID4ERB"></a>


## <a name="valid-methods"></a>Métodos válidos

[POST ({itemID})](uri-inventoryconsumablesitemurlpost.md)

&nbsp;&nbsp;Indica que se ha utilizado todos o parte de un elemento de inventario consumibles y disminuye la cantidad de los consumibles por la cantidad solicitada.

<a id="ID4E4B"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4E6B"></a>


##### <a name="parent"></a>Primario

[URI de Marketplace](atoc-reference-marketplace.md)


<a id="ID4EJC"></a>


##### <a name="further-information"></a>Para obtener más información

[Encabezados comunes de EDS](../../additional/edscommonheaders.md)

 [Parámetros de EDS](../../additional/edsparameters.md)

 [Consulta de EDS refinadores](../../additional/edsqueryrefiners.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)
