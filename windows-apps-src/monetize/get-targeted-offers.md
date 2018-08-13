---
author: mcleanbyron
ms.assetid: A4C6098B-6CB9-4FAF-B2EA-50B03D027FF1
description: Usa este método en las API de ofertas dirigidas de Microsoft Store para obtener las ofertas dirigidas que están disponibles para el usuario actual en el contexto de la aplicación actual.
title: Obtener ofertas dirigidas
ms.author: mcleans
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, servicios Microsoft Store, API de ofertas de destino de Microsoft Store, obtener ofertas dirigidas
ms.localizationpriority: medium
ms.openlocfilehash: 188561fb5dc6dda318a0b2aa7f1d91aa04c32548
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/17/2018
ms.locfileid: "1661895"
---
# <a name="get-targeted-offers"></a>Obtener ofertas dirigidas

Usa este método para obtener las ofertas dirigidas que están disponibles para el usuario actual, en función de si el usuario es parte del segmento de cliente para la oferta dirigida o no. Para obtener más información, consulta [Administrar ofertas dirigidas usando los servicios de la Store](manage-targeted-offers-using-windows-store-services.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes [obtener un token de cuenta de Microsoft](manage-targeted-offers-using-windows-store-services.md#obtain-a-microsoft-account-token) para el usuario que inició sesión actualmente en la aplicación. Debes pasar este token en el encabezado de la solicitud ```Authorization``` para este método. La Store usa este token para obtener las ofertas dirigidas para el usuario actual.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción  |
|---------------|--------|--------------|
| Authorization | cadena | Obligatorio. El token de la cuenta de Microsoft para el usuario que inició sesión actualmente en la aplicación en la forma **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de la solicitud

Este método no tiene parámetros de URI ni de consulta.

### <a name="request-example"></a>Ejemplo de solicitud

```syntax
GET https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user HTTP/1.1
Authorization: Bearer <Microsoft Account token>
```

## <a name="response"></a>Respuesta

Este método devuelve un cuerpo de respuesta en formato JSON que contiene una matriz de objetos con los siguientes campos. Cada objeto en la matriz representa las ofertas dirigidas que están disponibles para el usuario especificado como parte de un segmento de cliente en particular.

| Campo      | Tipo   | Descripción         |
|------------|--------|------------------|
| ofertas      | matriz  | Una matriz de identificadores de producto para los complementos que están asociados con las ofertas dirigidas que están disponibles para el usuario actual. Estos id. de producto se especifican en la página **Ofertas dirigidas** para tu aplicación en el panel del Centro de desarrollo de Windows.            |
| trackingId  | cadena | Un GUID que, opcionalmente, puedes utilizar para realizar un seguimiento de la oferta dirigida en tu propio código o servicios. |


### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
[
  {
    "offers": [
      "10x gold coins",
      "100x gold coins"
    ],
    "trackingId": "5de5dd29-6dce-4e68-b45e-d8ee6c2cd203"
  }
]
```

## <a name="related-topics"></a>Artículos relacionados

* [Administrar ofertas dirigidas usando los servicios de la Store](manage-targeted-offers-using-windows-store-services.md)

 

 