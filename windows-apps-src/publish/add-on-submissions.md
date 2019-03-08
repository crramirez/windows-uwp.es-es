---
Description: Los complementos (o productos desde la aplicación) se publican a través del centro de partners.
title: Envíos de complementos
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, iap, compra desde la aplicación, producto desde la aplicación, envío de iap
ms.localizationpriority: medium
ms.openlocfilehash: 28383ed82c418ff15806c325d6eab5a05f9987bf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620370"
---
# <a name="add-on-submissions"></a>Envíos de complementos

Los complementos (a los que a veces se denominan productos desde la aplicación) son elementos complementarios para la aplicación que los clientes pueden adquirir. Un complemento puede ser un divertido nueva característica, una nueva partida nivel o nada más que se piensa que mantendrá el interés de los usuarios. Los complementos no solo son una manera fantástica de ganar dinero, sino que promueven la participación y la interacción del cliente.

Los complementos se publican a través de [centro de partners](https://partner.microsoft.com/dashboard)y tendrá que tener activa una [cuenta de desarrollador](https://go.microsoft.com/fwlink/p/?LinkId=615100). También tendrás que [habilitar los complementos](../monetize/in-app-purchases-and-trials.md) en el código de la aplicación.

Es el primer paso en el proceso de envío del complemento crear el complemento en el centro de partners por [definir su tipo de producto y el Id. de producto](set-your-add-on-product-id.md). Después, creará un envío para que el complemento se puede adquirir a través de la Microsoft Store. Puedes enviar un app al mismo tiempo que [envías la aplicación](app-submissions.md) o puedes trabajar en él de forma independiente. Y puedes realizar [actualizaciones](#updating-an-add-on-after-publication) de los complementos una vez que la aplicación esté en la Tienda sin tener que volver a enviar la aplicación.

> [!NOTE]
> En esta sección de la documentación se describe cómo enviar los complementos en el centro de partners. Como alternativa, puedes usar la [API de envío de Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar los envíos de complementos.


## <a name="checklist-for-submitting-an-add-on"></a>Lista de comprobación para el envío de un complemento

A continuación verás una lista de la información que se proporciona al crear el envío de un complemento. A continuación se indican los elementos que debes proporcionar. Algunos de ellos son opcionales o tienen valores predeterminados ya proporcionados que puedes cambiar según lo desees.


### <a name="create-a-new-add-on-page"></a>Crear la página de un nuevo complemento

| Nombre del campo                    | Notas                            |
|-------------------------------|----------------------------------|
| [**Tipo de producto**](set-your-add-on-product-id.md#product-type)      | Requerido |  
| [**Id. de producto**](set-your-add-on-product-id.md#product-id)          | Requerido |        


### <a name="properties-page"></a>Página Propiedades

| Nombre del campo                    | Notas                              |   
|-------------------------------|------------------------------------|
| [**Vigencia del producto**](enter-add-on-properties.md#product-lifetime)  | Es obligatorio si es el tipo de producto **Duradero**. No se aplica a otros tipos de producto. |
| [**Cantidad**](enter-add-on-properties.md#quantity)  | Es obligatorio si el tipo de producto es **Consumible administrado por la Tienda**. No se aplica a otros tipos de producto. |
| [**Período de suscripción**](enter-add-on-properties.md#subscription-period)          | Es obligatorio si el tipo de producto es **Suscripción**. No se aplica a otros tipos de producto.       |  
| [**Evaluación gratuita**](enter-add-on-properties.md#free-trial)          | Es obligatorio si el tipo de producto es **Suscripción**. No se aplica a otros tipos de producto.       |
| [**Tipo de contenido**](enter-add-on-properties.md#content-type)          | Requerido    |               
| [**Palabras clave**](enter-add-on-properties.md#keywords)                  | Opcional (hasta 10 palabras clave, con un límite de 30 caracteres cada una) |
| [**Datos personalizados para los desarrolladores**](enter-add-on-properties.md#custom-developer-data)   | Opcional (límite de 3000 caracteres)            |


### <a name="pricing-and-availability-page"></a>Página Precios y disponibilidad

| Nombre del campo                    | Notas                                       |
|-------------------------------|---------------------------------------------|
| [**Mercados**](set-add-on-pricing-and-availability.md#markets)  | Default: Todos los mercados posibles |
| [**Visibilidad**](set-add-on-pricing-and-availability.md#visibility)   | Default: Está disponible para su compra. Puede mostrarse en la descripción de la aplicación |
| [**Programación**](set-add-on-pricing-and-availability.md#schedule)    | Default: Versión tan pronto como sea posible
| [**Los precios**](set-add-on-pricing-and-availability.md#pricing)                | Requerido                                    |
| [**Precios de venta**](put-apps-and-add-ons-on-sale.md)               | Opcional                    |


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

Puedes realizar cambios en un complemento publicado en cualquier momento. Los cambios del complemento se envíen y se publican independientemente de la aplicación, por lo que generalmente no es necesario actualizar toda la aplicación con el fin de realizar cambios en un complemento, como actualizar su precio o descripción.

Para enviar las actualizaciones, vaya a la página del agregar en la Centro de partners y haga clic en **actualización**. Esto creará un nuevo envío para el complemento, utilizando la información de su envío anterior como punto de partida. Realice los cambios que desee y, a continuación, haga clic en **enviar al Store**.

Para quitar un complemento que has estado ofreciendo anteriormente, crea un nuevo envío y cambia la opción [Distribución y visibilidad](set-add-on-pricing-and-availability.md) a **Oculto en la Tienda** con la opción **Detener la compra**. Asegúrese de actualizar el código de la aplicación según sea necesario también quitar las referencias al complemento (especialmente si la aplicación publicada previamente es compatible con Windows 8.1 anteriormente; esta configuración de visibilidad no se aplica a los clientes).

> [!IMPORTANT]
> Si la aplicación publicada anteriormente está disponible para clientes en Windows 8.x, deberá crear y publicar un nuevo envío de la aplicación con el fin de que las actualizaciones del complemento sea visible para los clientes. De forma parecida, si agregas nuevos complementos a una aplicación destinada a Windows 8.x después que dicha aplicación se haya publicado, tendrás que actualizar el código de la aplicación para hacer referencia a esos complementos y, a continuación, volver a enviar la aplicación. De lo contrario, los nuevos complementos no serán visibles para los clientes de Windows 8.x.
