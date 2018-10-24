---
author: Xansky
ms.assetid: 94B5B2E9-BAEE-4B7F-BAF1-DA4D491427D7
description: Usa este método en la API de compra de Microsoft Store para obtener las suscripciones que un usuario determinado tiene derecho a usar.
title: Obtener suscripciones para un usuario
ms.author: mhopkins
ms.date: 07/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de compra de Microsoft Store, suscripciones
ms.localizationpriority: medium
ms.openlocfilehash: c08964b991b0cecaef6d994d399ce97301a7e8e7
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "5431920"
---
# <a name="get-subscriptions-for-a-user"></a>Obtener suscripciones para un usuario

Usa este método en la API de compra de Microsoft Store para obtener los complementos de una suscripción que un usuario determinado tiene derecho a usar.

> [!NOTE]
> Este método solo puede usarse en cuentas de desarrollador aprovisionadas por Microsoft para que puedan crear complementos de una suscripción para aplicaciones para la Plataforma universal de Windows(UWP). Actualmente, los complementos de una suscripción no están disponibles para la mayoría de las cuentas de desarrollador.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, necesitarás:

* Un token de acceso de Azure AD que tiene el valor de URI de audiencia `https://onestore.microsoft.com`.
* Una clave de identificador de Microsoft Store que represente la identidad del usuario cuyas suscripciones quieres obtener.

Para obtener más información, consulta [Administrar los derechos de producto de un servicio](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado         | Tipo   | Descripción      |
|----------------|--------|-------------------|
| Authorization  | cadena | Obligatorio. Token de acceso de Azure AD con formato **Portador** &lt;*token*&gt;.                           |
| Host           | string | Debe establecerse en el valor **purchase.mp.microsoft.com**.                                            |
| Content-Length | number | Longitud del cuerpo de la solicitud.                                                                       |
| Content-Type   | string | Especifica los tipos de solicitud y respuesta. Actualmente, el único valor admitido es **application/json**. |


### <a name="request-body"></a>Cuerpo de la solicitud

| Parámetro      | Tipo   | Descripción     | Requerido |
|----------------|--------|-----------------|----------|
| b2bKey         | cadena | La [clave de identificador de Microsoft Store](view-and-grant-products-from-a-service.md#step-4) que represente la identidad del usuario cuyas suscripciones quieres obtener.  | Sí      |
| continuationToken |  cadena     |  Si el usuario tiene derecho a varias suscripciones, el cuerpo de la respuesta devuelve un token de continuación cuando se alcanza el límite de la página. Proporciona ese token de continuación aquí en llamadas posteriores para recuperar los productos restantes.    | No      |
| pageSize       | cadena | Número máximo de suscripciones que puede devolver una respuesta. El valor predeterminado es 25.     |  No      |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo usar este método para obtener los complementos de una suscripción que un usuario determinado tiene derecho a usar. Reemplaza el valor *b2bKey* por la [clave de Id. de Microsoft Store](view-and-grant-products-from-a-service.md#step-4) que representa la identidad del usuario cuyas suscripciones quieres obtener.

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: https://purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ..."
}
```


## <a name="response"></a>Respuesta

Este método devuelve un cuerpo de respuesta JSON que contiene una colección de objetos de datos que describen los complementos de una suscripción que el usuario tiene derecho a usar. En el siguiente ejemplo se muestra el cuerpo de respuesta para un usuario que tiene un derecho a una suscripción.

```json
{
  "items": [
    {
      "autoRenew":true,
      "beneficiary":"pub:gFVuEBiZHPXonkYvtdOi+tLE2h4g2Ss0ZId0RQOwzDg=",
      "expirationTime":"2017-06-11T03:07:49.2552941+00:00",
      "id":"mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac",
      "lastModified":"2017-01-08T21:07:51.1459644+00:00",
      "market":"US",
      "productId":"9NBLGGH52Q8X",
      "skuId":"0024",
      "startTime":"2017-01-10T21:07:49.2552941+00:00",
      "recurrenceState":"Active"
    }
  ]
}
```


### <a name="response-body"></a>Cuerpo de la respuesta

El cuerpo de la respuesta contiene los siguientes datos.

| Valor        | Tipo   | Descripción            |
|---------------|--------|---------------------|
| items | matriz | Una matriz de objetos que contienen datos sobre cada complemento de una suscripción que el usuario especificado tiene derecho a usar. Para obtener más información sobre los datos de cada objeto, consulta la tabla siguiente.  |


Cada objeto de la matriz *items* contiene los siguientes valores.

| Valor        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------|
| autoRenew | booleano |  Indica si la suscripción está configurada para renovarse automáticamente al final del período de suscripción actual.   |
| beneficiary | cadena |  El identificador del beneficiario del derecho que está asociado con esta suscripción.   |
| expirationTime | cadena | La fecha y la hora a las que expirará la suscripción, en formato ISO8601. Este campo solo está disponible cuando la suscripción esté en determinados estados. El momento de expiración normalmente indica cuándo expira el estado actual. Por ejemplo, para una suscripción activa, la fecha de expiración indica cuándo se producirá la próxima renovación automática.    |
| expirationTimeWithGrace | string | La fecha y hora que expirará la suscripción, incluido el período de gracia, en formato ISO 8601. Este valor indica cuándo el usuario perderán el acceso a la suscripción después de que se produjo un error de la suscripción a la renovación automática.    |
| id | cadena |  El identificador de la suscripción. Usa este valor para indicar qué suscripción quieres modificar cuando llamas al método [cambiar el estado de facturación de la suscripción de un usuario](change-the-billing-state-of-a-subscription-for-a-user.md).    |
| isTrial | booleano |  Indica si la suscripción es una versión de prueba.     |
| lastModified | cadena |  La fecha y la hora de la última modificación de la suscripción, en formato ISO8601.      |
| market | cadena | El código de país (en formato de dos letras ISO3166-1 alfa 2) en el que el usuario compró la suscripción.      |
| productId | cadena |  El [identificador de la Store](in-app-purchases-and-trials.md#store-ids) para el [producto](in-app-purchases-and-trials.md#products-skus-and-availabilities) que representa el complemento de la suscripción en el catálogo de Microsoft Store. Un ejemplo de Id. de Store para un producto es 9NBLGGH42CFD.     |
| skuId | cadena |  El [identificador de la Store](in-app-purchases-and-trials.md#store-ids) para la [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) que representa el complemento de la suscripción en el catálogo de Microsoft Store. Un ejemplo de Id. de Store para una SKU es 0010.    |
| startTime | cadena |  La fecha y hora de inicio de la suscripción, en formato ISO8601.     |
| recurrenceState | cadena  |  Uno de los siguientes valores:<ul><li>**None**:&nbsp;&nbsp;Indica una suscripción permanente.</li><li>**Active**:&nbsp;&nbsp;La suscripción está activa y el usuario tiene derecho a usar los servicios.</li><li>**Inactive**:&nbsp;&nbsp;La suscripción superó la fecha de expiración, y el usuario deshabilitó la opción de renovación automática de la suscripción.</li><li>**Canceled**:&nbsp;&nbsp;La suscripción se terminó a propósito antes de la fecha de expiración, con o sin reembolso.</li><li>**InDunning**:&nbsp;&nbsp;La suscripción está en *notificación* (es decir, la suscripción está próxima a la expiración, y Microsoft está intentando adquirir fondos para renovar la suscripción).</li><li>**Failed**:&nbsp;&nbsp;El período de notificación terminó y no se pudo renovar la suscripción después de varios intentos.</li></ul><p>**Nota:**</p><ul><li>**Inactive**/**Canceled**/**Failed** son estados terminales. Cuando una suscripción entra en uno de estos estados, el usuario debe comprar la suscripción para volver a activarla. El usuario no tiene derecho a usar los servicios de estos estados.</li><li>Cuando una suscripción está en estado **Canceled**, se actualizará expirationTime con la fecha y la hora de la cancelación.</li><li>El identificador de la suscripción seguirá siendo el mismo durante toda su vida. No cambiará si la opción de renovación automática está activada o desactivada. Si un usuario vuelve a comprar una suscripción después de alcanzar un estado terminal, se creará un nuevo identificador de suscripción.</li><li>El identificador de una suscripción debe usarse para ejecutar cualquier operación en una suscripción individual.</li><li>Cuando un usuario vuelve a comprar una suscripción después de cancelarla o interrumpirla, si consultas los resultados para el usuario, obtendrás dos entradas: una con el identificador de suscripción antiguo en un estado terminal y otra con el nuevo identificador de la suscripción en un estado activo.</li><li>Siempre resulta recomendable comprobar tanto recurrenceState como expirationTime, ya que las actualizaciones de recurrenceState potencialmente pueden retrasarse unos pocos minutos (o, en ocasiones, horas).       |
| cancellationDate | cadena   |  La fecha y la hora a las que se canceló la suscripción del usuario, en formato ISO8601.     |


## <a name="related-topics"></a>Temas relacionados

* [Administrar los derechos de producto de un servicio](view-and-grant-products-from-a-service.md)
* [Cambiar el estado de facturación de la suscripción de un usuario](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Consultar productos](query-for-products.md)
* [Notificar productos consumibles como completados](report-consumable-products-as-fulfilled.md)
* [Renovar una clave de id. de Microsoft Store](renew-a-windows-store-id-key.md)
