---
ms.assetid: 94B5B2E9-BAEE-4B7F-BAF1-DA4D491427D7
description: Use este método en la API de compra de Microsoft Store para obtener las suscripciones que un usuario determinado tiene derechos para usar.
title: Obtener suscripciones de un usuario
ms.date: 07/10/2018
ms.topic: article
keywords: Windows 10, UWP, API de compra de Microsoft Store, suscripciones
ms.localizationpriority: medium
ms.openlocfilehash: c77218cc7157e3d9a5508146395b284b57397d0f
ms.sourcegitcommit: 2a23972e9a0807256954d6da5cf21d0bbe7afb0a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2020
ms.locfileid: "94941801"
---
# <a name="get-subscriptions-for-a-user"></a>Obtener suscripciones de un usuario

Use este método en la API de compra de Microsoft Store para obtener los complementos de suscripción que un usuario determinado tiene derechos para usar.

> [!NOTE]
> Este método solo lo pueden usar las cuentas de desarrollador aprovisionadas por Microsoft para poder crear complementos de suscripción para aplicaciones Plataforma universal de Windows (UWP). Los complementos de suscripción actualmente no están disponibles para la mayoría de las cuentas de desarrollador.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, necesitarás:

* Azure AD token de acceso que tiene el valor de URI de audiencia `https://onestore.microsoft.com` .
* Una clave de identificador de Microsoft Store que representa la identidad del usuario cuyas suscripciones desea obtener.

Para obtener más información, consulte [Administración de derechos de producto desde un servicio](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado         | Tipo   | Descripción      |
|----------------|--------|-------------------|
| Authorization  | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;.                           |
| Host           | string | Debe establecerse en el valor **Purchase.MP.Microsoft.com**.                                            |
| Content-Length | número | Longitud del cuerpo de la solicitud.                                                                       |
| Content-Type   | string | Especifica los tipos de solicitud y respuesta. Actualmente, el único valor admitido es **application/json**. |


### <a name="request-body"></a>Cuerpo de la solicitud

| Parámetro      | Tipo   | Descripción     | Obligatorio |
|----------------|--------|-----------------|----------|
| b2bKey         | string | La [clave de identificador de Microsoft Store](view-and-grant-products-from-a-service.md#step-4) que representa la identidad del usuario cuyas suscripciones desea obtener.  | Sí      |
| continuationToken |  string     |  Si el usuario tiene derechos para varias suscripciones, el cuerpo de la respuesta devuelve un token de continuación cuando se alcanza el límite de páginas. Proporciona ese token de continuación aquí en llamadas posteriores para recuperar los productos restantes.    | No      |
| pageSize       | string | Número máximo de suscripciones que se van a devolver en una respuesta. El valor predeterminado es 25.     |  No      |


### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra cómo utilizar este método para obtener los complementos de suscripción que un usuario determinado tiene derechos para usar. Reemplace el valor de *b2bKey* por la [clave de identificador de Microsoft Store](view-and-grant-products-from-a-service.md#step-4) que representa la identidad del usuario cuyas suscripciones desea obtener.

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ..."
}
```


## <a name="response"></a>Response

Este método devuelve un cuerpo de respuesta JSON que contiene una colección de objetos de datos que describen los complementos de suscripción que el usuario tiene derechos para usar. En el ejemplo siguiente se muestra el cuerpo de la respuesta para un usuario que tiene derecho a una suscripción.

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

El cuerpo de la respuesta contiene los datos siguientes.

| Value        | Tipo   | Descripción            |
|---------------|--------|---------------------|
| items | array | Matriz de objetos que contienen datos sobre cada complemento de suscripción que el usuario especificado tiene derecho a usar. Para obtener más información acerca de los datos de cada objeto, vea la tabla siguiente.  |


Cada objeto de la matriz *Items* contiene los siguientes valores.

| Value        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------|
| Renovación de la suscripción | Boolean |  Indica si la suscripción está configurada para la renovación automática al final del período de suscripción actual.   |
| beneficiary | string |  El identificador del beneficiario del derecho asociado a esta suscripción.   |
| expirationTime | string | Fecha y hora de expiración de la suscripción, en formato ISO 8601. Este campo solo está disponible cuando la suscripción se encuentra en determinados Estados. La hora de expiración suele indicar cuándo expira el estado actual. Por ejemplo, para una suscripción activa, la fecha de expiración indica cuándo se producirá la siguiente renovación automática.    |
| expirationTimeWithGrace | string | Fecha y hora de expiración de la suscripción, incluido el período de gracia, en formato ISO 8601. Este valor indica si el usuario perderá el acceso a la suscripción después de que la suscripción no haya podido renovar automáticamente.    |
| id | string |  IDENTIFICADOR de la suscripción. Use este valor para indicar qué suscripción desea modificar al llamar al método de [cambio de estado de facturación de una suscripción para un usuario](change-the-billing-state-of-a-subscription-for-a-user.md) .    |
| isTrial | Boolean |  Indica si la suscripción es una versión de prueba.     |
| lastModified | string |  Fecha y hora en que se modificó por última vez la suscripción, en formato ISO 8601.      |
| market | string | El código de país (en formato de dos letras ISO 3166-1 alpha-2) en el que el usuario adquirió la suscripción.      |
| productId | string |  El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) para el [producto](in-app-purchases-and-trials.md#products-skus-and-availabilities) que representa el complemento de suscripción en el catálogo de Microsoft Store. Un ejemplo de un identificador de almacén para un producto es 9NBLGGH42CFD.     |
| skuId | string |  El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) de la [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) que representa el complemento de suscripción del catálogo de Microsoft Store. Un identificador de almacén de ejemplo para una SKU es 0010.    |
| startTime | string |  Fecha y hora de inicio de la suscripción, en formato ISO 8601.     |
| recurrenceState | string  |  Uno de los siguientes valores:<ul><li>**Ninguno**: &nbsp; &nbsp; indica una suscripción perpetua.</li><li>**Activa**: &nbsp; &nbsp; la suscripción está activa y el usuario tiene derecho a usar los servicios.</li><li>**Inactivo**: &nbsp; &nbsp; la suscripción está más allá de la fecha de expiración y el usuario desactivó la opción de renovación automática para la suscripción.</li><li>**Cancelado**: &nbsp; &nbsp; la suscripción se ha finalizado intencionadamente antes de la fecha de expiración, con o sin reembolso.</li><li>**InDunning** Inestabilidad: &nbsp; &nbsp; la suscripción se encuentra *dunning* en el fondo (es decir, la suscripción está a punto de expirar y Microsoft está intentando adquirir fondos para renovar la suscripción automáticamente).</li><li>**Error**: &nbsp; &nbsp; el período de estiba es superior y la suscripción no se pudo renovar después de varios intentos.</li></ul><p>**Nota:**</p><ul><li>**Inactivo** / **Cancelado** / **Error** : Estados de terminal. Cuando una suscripción entra en uno de estos Estados, el usuario debe volver a comprar la suscripción para activarla de nuevo. El usuario no tiene derecho a usar los servicios en estos Estados.</li><li>Cuando se **cancela** una suscripción, el expirationTime se actualizará con la fecha y la hora de la cancelación.</li><li>El identificador de la suscripción seguirá siendo el mismo durante toda la duración. No cambiará si la opción de renovación automática está activada o desactivada. Si un usuario compra una suscripción después de alcanzar un estado de terminal, se creará un nuevo identificador de suscripción.</li><li>El identificador de una suscripción debe usarse para ejecutar cualquier operación en una suscripción individual.</li><li>Cuando un usuario compra una suscripción después de cancelarla o descontinuarla, si consulta los resultados del usuario, obtendrá dos entradas: una con el identificador de suscripción anterior en un estado de terminal y otra con el nuevo identificador de suscripción en un estado activo.</li><li>Siempre es recomendable comprobar recurrenceState y expirationTime, ya que es posible que las actualizaciones de recurrenceState se retrasen unos minutos (o, en ocasiones, horas).       |
| cancellationDate | string   |  Fecha y hora en que se canceló la suscripción del usuario, en formato ISO 8601.     |


## <a name="related-topics"></a>Temas relacionados

* [Administrar los derechos de producto de un servicio](view-and-grant-products-from-a-service.md)
* [Cambiar el estado de facturación de la suscripción de un usuario](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Consultar productos](query-for-products.md)
* [Notificar productos consumibles como completados](report-consumable-products-as-fulfilled.md)
* [Renovar una clave de identificador de Microsoft Store](renew-a-windows-store-id-key.md)
