---
author: jnHs
Description: "Las IAP se publican a través del panel del Centro de desarrollo de Windows."
title: "Envíos de IAP"
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.sourcegitcommit: 97f4aee47cab9064ac053e7a6e16441d6960d41f
ms.openlocfilehash: 4a1764dfb8f94409aba973a28ba2999854179196

---

# Envíos de IAP


Un IAP (producto desde la aplicación) es un elemento complementario para una aplicación que los clientes pueden adquirir. Un IAP puede ser un complemento divertido, un nuevo nivel de juego o cualquier cosa que creas que puede atraer a los usuarios. Los IAP no solo son una manera fantástica de hacer dinero sino que promueven la participación y la interacción del cliente.

Las IAP se publican a través del panel del Centro de desarrollo de Windows. También tendrás que [habilitar los IAP](../monetize/enable-in-app-product-purchases.md) en el código de la aplicación.

El primer paso en el proceso de envío de un IAP es crear dicho IAP en el panel mediante la [definición de su tipo de producto y su id. de producto](set-your-iap-product-id.md). Después, es posible crear un envío para que el IAP se pueda adquirir a través de la Tienda Windows. Puedes enviar un IAP al mismo tiempo que [envías la aplicación](app-submissions.md) o puedes trabajar en él de forma independiente. Y puedes realizar [actualizaciones](#updating-an-iap-after-submission) de los IAP una vez que la aplicación esté en la Tienda sin tener que volver a enviar la aplicación.

## Lista de comprobación para enviar un IAP

A continuación verás una lista de la información que se proporciona al crear el envío de un IAP. A continuación se indican los elementos que debes proporcionar. Algunos de ellos son opcionales o tienen valores predeterminados ya proporcionados que puedes cambiar según lo desees.

### Crea una nueva página del IAP
| Nombre del campo                    | Notas                            | 
|-------------------------------|----------------------------------|
| [**Tipo de producto**](set-your-iap-product-id.md#product-type)      | Obligatorio. Si es **Duradero**, se necesita una **Duración del producto**. |  
| [**Id. del producto**](set-your-iap-product-id.md#product-id)          | Obligatorio |        

### Página Propiedades
| Nombre del campo                    | Notas                              |   
|-------------------------------|------------------------------------|
| [**Duración del producto**](enter-iap-properties.md#product-lifetime)  | Se requiere si es el tipo de producto **Duradero**. No es aplicable si el tipo de producto es **Consumible**. | 
| [**Tipo de contenido**](enter-iap-properties.md#content-type)          | Obligatorio       |               
| [**Palabras clave**](enter-iap-properties.md#keywords)                  | Opcional (hasta 10 palabras clave, con un límite de 30 caracteres cada una) | 
| [**Etiqueta**](enter-iap-properties.md#tag)                               | Opcional (límite de 3000 caracteres)             | 

### Página Precios y disponibilidad 
| Nombre del campo                    | Notas                                       | 
|-------------------------------|---------------------------------------------|
| [**Precio base**](set-iap-pricing-and-availability.md#base-price)                | Obligatorio                                    | 
| [**Mercados y precios personalizados**](set-iap-pricing-and-availability.md#markets-and-custom-prices)  | Valor predeterminado: Disponible en todos los mercados posibles | 
| [**Precio de oferta**](put-apps-and-iaps-on-sale.md)               | Opcional                             |
| [**Distribución y visibilidad**](set-iap-pricing-and-availability.md#distribution-and-visibility)   | Valor predeterminado: los clientes pueden encontrar IAP explorando o buscando en la Tienda | 
| [**Fecha de publicación**](set-iap-pricing-and-availability.md#publish-date)                | Valor predeterminado: Publicar tan pronto como el IAP supere la certificación |

### Página Descripciones
Una descripción obligatoria. Se recomienda proporcionar descripciones para cada [idioma](create-iap-descriptions.md#languages) que la aplicación admita.

| Nombre del campo                    | Notas                                       | 
|-------------------------------|---------------------------------------------|
| [**Título**](create-iap-descriptions.md#title)                    | Obligatorio (límite de 100 caracteres)              |
| [**Descripción**](create-iap-descriptions.md#description)       | Opcional (límite de 200 caracteres)              |
| [**Icono**](create-iap-descriptions.md#icon)                    | Opcional (.png, 300 x 300 píxeles)             | 

Cuando hayas terminado de introducir esta información, haz clic en **Enviar a la Tienda**. En la mayoría de los casos, el proceso de certificación tarda aproximadamente una hora. Después, el IAP se publicará en la Tienda y estará listo para que los clientes puedan comprarlo.

**Nota** El IAP también debe implementarse en el código de la aplicación. Para obtener más información, consulta [Habilitar compras de productos desde la aplicación](../monetize/enable-in-app-product-purchases.md).


## Actualizar un IAP después de su publicación

Puedes realizar cambios en un IAP publicado en cualquier momento. Los cambios de los IAP se envían y se publican independientemente de la aplicación, de modo que, por lo general, no es necesario actualizar toda la aplicación para realizar cambios en un IAP, como actualizar su precio o su descripción.

> **Importante**  Si la aplicación está disponible para los clientes de Windows 8.x, tendrás que crear y publicar un nuevo envío de aplicación para que las actualizaciones de los IAP estén visibles para dichos clientes. Del mismo modo, si agregas nuevos IAP a una aplicación destinada a Windows 8.x después publicarse la aplicación, tendrás que actualizar el código de la aplicación para hacer referencia a esos IAP y, a continuación, volver a enviar la aplicación. De lo contrario, las nuevas IAP no serán visibles para los clientes de Windows 8.x.

Para enviar actualizaciones, ve a la página del IAP en el panel y haz clic en **Actualizar**. Esto creará un nuevo envío para el IAP, con la información de tu envío anterior como punto de partida. Cambia la información que desees y, a continuación, haz clic en **Enviar a la Tienda**.

Si quieres quitar un IAP que ofrecías anteriormente, puedes hacerlo si creas un nuevo envío y cambias la opción [Distribución y visibilidad](set-iap-pricing-and-availability.md) a **Ya no está disponible para comprar. No se muestra en la descripción de la aplicación**. Asegúrate de actualizar el código de la aplicación según sea necesario para quitar también las referencias al IAP.




<!--HONumber=Jun16_HO4-->


