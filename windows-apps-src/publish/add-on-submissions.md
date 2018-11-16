---
author: jnHs
Description: Add-ons (or in-app products) are published through Partner Center.
title: Envíos de complementos
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows10, uwp, iap, compra desde la aplicación, producto desde la aplicación, envío de iap
ms.localizationpriority: medium
ms.openlocfilehash: 28fd2e104de12cc297ce5d28ddd18b0ce550a5d0
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6839418"
---
# <a name="add-on-submissions"></a>Envíos de complementos

Los complementos (a los que a veces se denominan productos desde la aplicación) son elementos complementarios para la aplicación que los clientes pueden adquirir. Un complemento puede ser una nueva característica, un juego nuevo nivel, o cualquier cosa que creas mantendrá a los usuarios interesados de diversión. Los complementos no solo son una manera fantástica de ganar dinero, sino que promueven la participación y la interacción del cliente.

Complementos se publican a través del [Centro de partners](https://partner.microsoft.com/dashboard)y requieren que tener una [cuenta de desarrollador](http://go.microsoft.com/fwlink/p/?LinkId=615100)de activa. También tendrás que [habilitar los complementos](../monetize/in-app-purchases-and-trials.md) en el código de la aplicación.

El primer paso en el proceso de envío de complemento es crear el complemento en el centro de partners mediante la [definición de su tipo de producto y el Id. del producto](set-your-add-on-product-id.md). Después de eso, crearás un envío para que el complemento se puede comprar a través de Microsoft Store. Puedes enviar un complemento al mismo tiempo que [envías la aplicación](app-submissions.md) o puedes trabajar en él de forma independiente. Y puedes realizar [actualizaciones](#updating-an-add-on-after-publication) de los complementos una vez que la aplicación esté en la Tienda sin tener que volver a enviar la aplicación.

> [!NOTE]
> En esta sección de la documentación se describe cómo enviar complementos en el centro de partners. Como alternativa, puedes usar la [API de envío de MicrosoftStore](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar los envíos de complementos.


## <a name="checklist-for-submitting-an-add-on"></a>Lista de comprobación para el envío de un complemento

A continuación verás una lista de la información que se proporciona al crear el envío de un complemento. A continuación se indican los elementos que debes proporcionar. Algunos de ellos son opcionales o tienen valores predeterminados ya proporcionados que puedes cambiar según lo desees.


### <a name="create-a-new-add-on-page"></a>Crear la página de un nuevo complemento

| Nombre del campo                    | Notas                            |
|-------------------------------|----------------------------------|
| [**Tipo de producto**](set-your-add-on-product-id.md#product-type)      | Obligatorio |  
| [**Id. del producto**](set-your-add-on-product-id.md#product-id)          | Obligatorio |        


### <a name="properties-page"></a>Página Propiedades

| Nombre del campo                    | Notas                              |   
|-------------------------------|------------------------------------|
| [**Duración del producto**](enter-add-on-properties.md#product-lifetime)  | Es obligatorio si es el tipo de producto **Duradero**. No se aplica a otros tipos de producto. |
| [**Cantidad**](enter-add-on-properties.md#quantity)  | Es obligatorio si el tipo de producto es **Consumible administrado por la Tienda**. No se aplica a otros tipos de producto. |
| [**Período de suscripción**](enter-add-on-properties.md#subscription-period)          | Es obligatorio si el tipo de producto es **Suscripción**. No se aplica a otros tipos de producto.       |  
| [**Evaluación gratuita**](enter-add-on-properties.md#free-trial)          | Es obligatorio si el tipo de producto es **Suscripción**. No se aplica a otros tipos de producto.       |
| [**Tipo de contenido**](enter-add-on-properties.md#content-type)          | Obligatorio    |               
| [**Palabras clave**](enter-add-on-properties.md#keywords)                  | Opcional (hasta 10 palabras clave, con un límite de 30 caracteres cada una) |
| [**Datos del desarrollador personalizados**](enter-add-on-properties.md#custom-developer-data)   | Opcional (límite de 3000 caracteres)            |


### <a name="pricing-and-availability-page"></a>Página Precios y disponibilidad

| Nombre del campo                    | Notas                                       |
|-------------------------------|---------------------------------------------|
| [**Mercados**](set-add-on-pricing-and-availability.md#markets)  | Opción predeterminada: Todos los mercados posibles |
| [**Visibilidad**](set-add-on-pricing-and-availability.md#visibility)   | Opción predeterminada: Disponible para la compra. Puede mostrarse en la descripción de la aplicación |
| [**Programación**](set-add-on-pricing-and-availability.md#schedule)    | Opción predeterminada: Se lanzará lo antes posible
| [**Precios**](set-add-on-pricing-and-availability.md#pricing)                | Obligatorio                                    |
| [**Precio de oferta**](put-apps-and-add-ons-on-sale.md)               | Opcional                    |


### <a name="store-listings"></a>Descripciones de la Tienda

Es obligatoria una descripción de la Tienda. Se recomienda proporcionar descripciones de la Tienda para cada [idioma](create-add-on-store-listings.md#store-listing-languages) que admita la aplicación.

| Nombre del campo                    | Notas                                       |
|-------------------------------|---------------------------------------------|
| [**Título**](create-add-on-store-listings.md#title)                    | Obligatorio (límite de 100 caracteres)           |
| [**Descripción**](create-add-on-store-listings.md#description)       | Opcional (límite de 200 caracteres)            |
| [**Icono**](create-add-on-store-listings.md#icon)                    | Opcional (.png, 300 x 300 píxeles)            |


Cuando hayas terminado de introducir esta información, haz clic en **Enviar a la Tienda**. En la mayoría de los casos, el proceso de certificación tarda aproximadamente una hora. Después, el complemento se publicará en la Tienda y estará listo para que los clientes puedan adquirirlo.

> [!NOTE]
> El complemento también debe implementarse en el código de la aplicación. Para obtener más información, consulta [Pruebas y compras desde la aplicación](../monetize/in-app-purchases-and-trials.md).


## <a name="updating-an-add-on-after-publication"></a>Actualizar un complemento después de su publicación

Puedes realizar cambios en un complemento publicado en cualquier momento. Cambios de los complementos se envía y publican independientemente de la aplicación, por lo que normalmente no es necesario actualizar toda la aplicación para realizar cambios en un complemento, como actualizar su precio o su descripción.

Para enviar actualizaciones, ve a la página del complemento en el centro de partners y haz clic en la **actualización**. Esto creará un nuevo envío del complemento, con la información de tu envío anterior como punto de partida. Realiza los cambios que desees y, a continuación, haz clic en **Enviar a la tienda**.

Para quitar un complemento que has estado ofreciendo anteriormente, crea un nuevo envío y cambia la opción [Distribución y visibilidad](set-add-on-pricing-and-availability.md) a **Oculto en la Tienda** con la opción **Detener la compra**. Asegúrate de actualizar el código de la aplicación según sea necesario para quitar también las referencias al complemento (especialmente si la aplicación publicada anteriormente admite Windows 8.1 y versiones anteriores; esta configuración de visibilidad no se aplicará a los clientes).

> [!IMPORTANT]
> Si la aplicación publicada anteriormente está disponible para los clientes de Windows 8.x, tendrás que crear y publicar un nuevo envío de aplicación para que las actualizaciones de los complementos sean visibles para dichos clientes. De forma parecida, si agregas nuevos complementos a una aplicación destinada a Windows8.x después que dicha aplicación se haya publicado, tendrás que actualizar el código de la aplicación para hacer referencia a esos complementos y, a continuación, volver a enviar la aplicación. De lo contrario, los nuevos complementos no serán visibles para los clientes de Windows8.x.
