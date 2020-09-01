---
description: Obtenga información sobre cómo enviar complementos, también denominados productos en la aplicación, como elementos complementarios de la aplicación que los clientes pueden comprar.
title: Envíos de complementos
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, IAP, compras desde la aplicación, producto en la aplicación, envío de IAP
ms.localizationpriority: medium
ms.openlocfilehash: 25246d192f40bae096c33fae00feb1bfc2a4c530
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162155"
---
# <a name="add-on-submissions"></a>Envíos de complementos

Los complementos (a los que a veces se denominan productos desde la aplicación) son elementos complementarios para la aplicación que los clientes pueden adquirir. Un complemento puede ser una nueva característica divertida, un nuevo nivel de juego o cualquier otra cosa que cree que mantendrá a los usuarios. Los complementos no solo son una manera fantástica de ganar dinero, sino que promueven la participación y la interacción del cliente.

Los complementos se publican a través del [centro de Partners](https://partner.microsoft.com/dashboard)y requieren que tenga una [cuenta de desarrollador](https://developer.microsoft.com/store/register)activa. También tendrás que [habilitar los complementos](../monetize/in-app-purchases-and-trials.md) en el código de la aplicación.

El primer paso del proceso de envío del complemento es crear el complemento en el centro de partners mediante [la definición de su tipo de producto e identificador de producto](set-your-add-on-product-id.md). Después, creará un envío para que el complemento se pueda comprar a través de la Microsoft Store. Puedes enviar un app al mismo tiempo que [envías la aplicación](app-submissions.md) o puedes trabajar en él de forma independiente. Y puedes realizar [actualizaciones](#updating-an-add-on-after-publication) de los complementos una vez que la aplicación esté en la Tienda sin tener que volver a enviar la aplicación.

> [!NOTE]
> En esta sección de la documentación se describe cómo enviar complementos en el centro de Partners. Como alternativa, puede usar la [API de envío de Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar los envíos de complementos.


## <a name="checklist-for-submitting-an-add-on"></a>Lista de comprobación para el envío de un complemento

A continuación verás una lista de la información que se proporciona al crear el envío de un complemento. A continuación se indican los elementos que debes proporcionar. Algunos de ellos son opcionales o tienen valores predeterminados ya proporcionados que puedes cambiar según lo desees.


### <a name="create-a-new-add-on-page"></a>Crear la página de un nuevo complemento

| Nombre de campo                    | Notas                            |
|-------------------------------|----------------------------------|
| [**Tipo de producto**](set-your-add-on-product-id.md#product-type)      | Obligatorio |  
| [**Product ID**](set-your-add-on-product-id.md#product-id)          | Obligatorio |        


### <a name="properties-page"></a>Página de propiedades

| Nombre de campo                    | Notas                              |   
|-------------------------------|------------------------------------|
| [**Duración del producto**](enter-add-on-properties.md#product-lifetime)  | Es obligatorio si es el tipo de producto **Duradero**. No se aplica a otros tipos de producto. |
| [**Cantidad**](enter-add-on-properties.md#quantity)  | Es obligatorio si el tipo de producto es **Consumible administrado por la Tienda**. No se aplica a otros tipos de producto. |
| [**Período de suscripción**](enter-add-on-properties.md#subscription-period)          | Obligatorio si el tipo de producto es **suscripción**. No se aplica a otros tipos de producto.       |  
| [**Evaluación gratuita**](enter-add-on-properties.md#free-trial)          | Obligatorio si el tipo de producto es **suscripción**. No se aplica a otros tipos de producto.       |
| [**Tipo de contenido**](enter-add-on-properties.md#content-type)          | Obligatorio    |               
| [**Palabras clave**](enter-add-on-properties.md#keywords)                  | Opcional (hasta 10 palabras clave, con un límite de 30 caracteres cada una) |
| [**Datos del desarrollador personalizados**](enter-add-on-properties.md#custom-developer-data)   | Opcional (límite de 3000 caracteres)            |


### <a name="pricing-and-availability-page"></a>Página Precios y disponibilidad

| Nombre de campo                    | Notas                                       |
|-------------------------------|---------------------------------------------|
| [**Mercados**](set-add-on-pricing-and-availability.md#markets)  | Predeterminado: todos los mercados posibles |
| [**Visibilidad**](set-add-on-pricing-and-availability.md#visibility)   | Valor predeterminado: disponible para la compra. Se puede mostrar en la lista de la aplicación |
| [**Programación**](set-add-on-pricing-and-availability.md#schedule)    | Valor predeterminado: liberar lo antes posible
| [**Precios**](set-add-on-pricing-and-availability.md#pricing)                | Obligatorio                                    |
| [**Precios de venta**](put-apps-and-add-ons-on-sale.md)               | Opcional                    |


### <a name="store-listings"></a>Descripciones de la Tienda

Es obligatoria una descripción de la Tienda. Se recomienda proporcionar descripciones de la Tienda para cada [idioma](create-add-on-store-listings.md#store-listing-languages) que admita la aplicación.

| Nombre de campo                    | Notas                                       |
|-------------------------------|---------------------------------------------|
| [**Title**](create-add-on-store-listings.md#title)                    | Obligatorio (límite de 100 caracteres)           |
| [**Descripción**](create-add-on-store-listings.md#description)       | Opcional (límite de 200 caracteres)            |
| [**Icono**](create-add-on-store-listings.md#icon)                    | Opcional (.png, 300 x 300 píxeles)            |


Cuando hayas terminado de introducir esta información, haz clic en **Enviar a la Tienda**. En la mayoría de los casos, el proceso de certificación tarda aproximadamente una hora. Después, el complemento se publicará en la Tienda y estará listo para que los clientes puedan adquirirlo.

> [!NOTE]
> El complemento también se debe implementar en el código de la aplicación. Para obtener más información, consulte [compras y pruebas desde la aplicación](../monetize/in-app-purchases-and-trials.md).


## <a name="updating-an-add-on-after-publication"></a>Actualizar un complemento después de su publicación

Puedes realizar cambios en un complemento publicado en cualquier momento. Los cambios en los complementos se envían y publican independientemente de la aplicación, por lo que normalmente no es necesario actualizar toda la aplicación para realizar cambios en un complemento, como la actualización del precio o la descripción.

Para enviar actualizaciones, vaya a la página del complemento del centro de Partners y haga clic en **Actualizar**. Se creará un nuevo envío para el complemento con la información del envío anterior como punto de partida. Realice los cambios que desee y, a continuación, haga clic en **Enviar a la tienda**.

Si desea quitar un complemento que ha ofrecido anteriormente, puede hacerlo mediante la creación de un nuevo envío y el cambio de la opción de [distribución y visibilidad](set-add-on-pricing-and-availability.md) a **oculto en la tienda** con la opción **detener la adquisición** . Asegúrese de actualizar el código de la aplicación según sea necesario para quitar también las referencias al complemento (especialmente si la aplicación publicada anteriormente admite Windows 8.1 anterior; esta configuración de visibilidad no se aplicará a los clientes).

> [!IMPORTANT]
> Si la aplicación publicada anteriormente está disponible para los clientes en Windows 8. x, deberá crear y publicar un nuevo envío de aplicación para que las actualizaciones del complemento sean visibles para esos clientes. De forma parecida, si agregas nuevos complementos a una aplicación destinada a Windows 8.x después que dicha aplicación se haya publicado, tendrás que actualizar el código de la aplicación para hacer referencia a esos complementos y, a continuación, volver a enviar la aplicación. De lo contrario, los nuevos complementos no serán visibles para los clientes de Windows 8.x.
