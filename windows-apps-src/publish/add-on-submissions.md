---
author: jnHs
Description: "Los complementos se publican a través del panel del Centro de desarrollo de Windows."
title: "Envíos de complementos"
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
translationtype: Human Translation
ms.sourcegitcommit: b0d877e46ba6958bfc61dd87687c30e91b6cd937
ms.openlocfilehash: 7b44dabfd4badcad795e38a97590d98e1601092d

---

# Envíos de complementos

Los complementos (a los que a veces se denominan productos desde la aplicación) son elementos complementarios para la aplicación que los clientes pueden adquirir. Un complemento puede ser una nueva función complementaria divertida, un nuevo nivel de juego o cualquier cosa que creas que puede mantener la relación de los usuarios con la aplicación. Los complementos no solo son una manera fantástica de ganar dinero, sino que promueven la participación y la interacción del cliente.

Los complementos se publican a través del panel del Centro de desarrollo de Windows. También tendrás que [habilitar los complementos](../monetize/in-app-purchases-and-trials.md) en el código de la aplicación.

El primer paso en el proceso de envío de un complemento es crear dicho complemento en el panel mediante la [definición de su tipo de producto y su id. del producto](set-your-add-on-product-id.md). Después, es posible crear un envío para que el Tienda Windows se pueda adquirir a través de la Tienda Windows. Puedes enviar un app al mismo tiempo que [envías la aplicación](app-submissions.md) o puedes trabajar en él de forma independiente. Y puedes realizar [actualizaciones](#updating-an-add-on-after-submission) de los complementos una vez que la aplicación esté en la Tienda sin tener que volver a enviar la aplicación.

> **Nota** En esta sección de la documentación se describe cómo enviar complementos en el panel del Centro de desarrollo. Como alternativa, puedes usar la [API de envío de la Tienda Windows](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar los envíos de complementos.

## Lista de comprobación para el envío de un complemento

A continuación verás una lista de la información que se proporciona al crear el envío de un complemento. A continuación se indican los elementos que debes proporcionar. Algunos de ellos son opcionales o tienen valores predeterminados ya proporcionados que puedes cambiar según lo desees.

### Crear la página de un nuevo complemento
| Nombre del campo                    | Notas                            |
|-------------------------------|----------------------------------|
| [**Tipo de producto**](set-your-add-on-product-id.md#product-type)      | Obligatorio. Si es **Duradero**, se necesita una **Duración del producto**. |  
| [**Id. del producto**](set-your-add-on-product-id.md#product-id)          | Obligatorio |        

<span/>

### Página Propiedades
| Nombre del campo                    | Notas                              |   
|-------------------------------|------------------------------------|
| [**Duración del producto**](enter-add-on-properties.md#product-lifetime)  | Es obligatorio si es el tipo de producto **Duradero**. No se aplica a otros tipos de producto. |
| [**Cantidad**](enter-add-on-properties.md#quantity)  | Es obligatorio si el tipo de producto es **Consumible administrado por la Tienda**. No se aplica a otros tipos de producto.
| [**Tipo de contenido**](enter-add-on-properties.md#content-type)          | Obligatorio       |               
| [**Palabras clave**](enter-add-on-properties.md#keywords)                  | Opcional (hasta 10 palabras clave, con un límite de 30 caracteres cada una) |
| [**Datos del desarrollador personalizados**](enter-add-on-properties.md#custom-developer-data)                               | Opcional (límite de 3000 caracteres)             |

<span/>

### Página Precios y disponibilidad
| Nombre del campo                    | Notas                                       |
|-------------------------------|---------------------------------------------|
| [**Precio base**](set-add-on-pricing-and-availability.md#base-price)                | Obligatorio                                    |
| [**Mercados y precios personalizados**](set-add-on-pricing-and-availability.md#markets-and-custom-prices)  | Valor predeterminado: Disponible en todos los mercados posibles |
| [**Precio de oferta**](put-apps-and-add-ons-on-sale.md)               | Opcional                             |
| [**Distribución y visibilidad**](set-add-on-pricing-and-availability.md#distribution-and-visibility)   | Valor predeterminado: Los clientes pueden encontrar el complemento si exploran o buscan en la Tienda. |
| [**Fecha de publicación**](set-add-on-pricing-and-availability.md#publish-date)                | Valor predeterminado: Publicar tan pronto como el complemento supere la certificación. |

<span/>

### Descripciones de la Tienda
Es obligatoria una descripción de la Tienda. Se recomienda proporcionar descripciones de la Tienda para cada [idioma](create-add-on-descriptions.md#languages) que admita la aplicación.

| Nombre del campo                    | Notas                                       |
|-------------------------------|---------------------------------------------|
| [**Título**](create-add-on-store-listings.md#title)                    | Obligatorio (límite de 100 caracteres)              |
| [**Descripción**](create-add-on-store-listings.md#description)       | Opcional (límite de 200 caracteres)              |
| [**Icono**](create-add-on-store-listings.md#icon)                    | Opcional (.png, 300 x 300 píxeles)             |

<span/>

Cuando hayas terminado de introducir esta información, haz clic en **Enviar a la Tienda**. En la mayoría de los casos, el proceso de certificación tarda aproximadamente una hora. Después, el complemento se publicará en la Tienda y estará listo para que los clientes puedan adquirirlo.

**Nota** El complemento también debe implementarse en el código de la aplicación. Para obtener más información, consulta [Habilitar compras de productos desde la aplicación](../monetize/enable-in-app-product-purchases.md).


## Actualizar un complemento después de su publicación

Puedes realizar cambios en un complemento publicado en cualquier momento. Los cambios de los complementos se envían y se publican independientemente de la aplicación, de modo que, por lo general, no es necesario actualizar toda la aplicación para realizar cambios en un complemento, como actualizar su precio o su descripción.

> **Importante** Si la aplicación está disponible para los clientes de Windows8.x, tendrás que crear y publicar un nuevo envío de aplicación para que las actualizaciones de los complementos sean visibles para dichos clientes. De forma parecida, si agregas nuevos complementos a una aplicación destinada a Windows8.x después que dicha aplicación se haya publicado, tendrás que actualizar el código de la aplicación para hacer referencia a esos complementos y, a continuación, volver a enviar la aplicación. De lo contrario, los nuevos complementos no serán visibles para los clientes de Windows8.x.

Para enviar actualizaciones, ve a la página del complemento en el panel y haz clic en **Actualizar**. Esto creará un nuevo envío para el complemento, con la información de tu envío anterior como punto de partida. Cambia la información que desees y, a continuación, haz clic en **Enviar a la Tienda**.

Para quitar un complemento que has estado ofreciendo anteriormente, crea un nuevo envío y cambia la opción [Distribución y visibilidad](set-add-on-pricing-and-availability.md) a **Ya no está disponible para comprar. No se muestra en la descripción de la aplicación**. Asegúrate de actualizar el código de la aplicación según sea necesario para quitar también las referencias al complemento.



<!--HONumber=Nov16_HO1-->


