---
author: jnHs
Description: Las IAP se publican a través del panel del Centro de desarrollo de Windows.
title: Envíos de IAP
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
---

# Envíos de IAP


Un IAP (producto desde la aplicación) es un elemento complementario para una aplicación que los clientes pueden adquirir. Un IAP puede ser un complemento divertido, un nuevo nivel de juego o cualquier cosa que creas que puede atraer a los usuarios. Los IAP no solo son una manera fantástica de hacer dinero sino que promueven la participación y la interacción del cliente.

Las IAP se publican a través del panel del Centro de desarrollo de Windows. También tendrás que [habilitar los IAP](https://msdn.microsoft.com/library/windows/apps/mt219684) en el código de la aplicación.

El primer paso en el proceso de envío de IAP es crear el IAP en el panel [definiendo su id. de producto](set-your-iap-product-id.md). Después, puedes crear un envío para que se pueda adquirir el IAP a través de la Tienda Windows. Puedes enviar un IAP al mismo tiempo que [envías la aplicación](app-submissions.md) o puedes trabajar en él de forma independiente. Y puedes realizar [actualizaciones](#updating-an-iap-after-submission) de IAP una vez que la aplicación esté en la Tienda sin tener que volver a enviar la aplicación.

## Lista de comprobación para enviar un IAP


A continuación se incluye la información que puedes proporcionar al crear el envío de un IAP. Los elementos que debes proporcionar se indican a continuación. Algunos de ellos son opcionales o tienen valores predeterminados ya proporcionados que puedes cambiar según lo desees.

### Página Propiedades
| Nombre del campo                    | Notas                                       | Para más información                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Tipo de producto**              | Obligatorio. Si es **Durable**, es necesaria una **Duración del producto**. | [Especificar propiedades de IAP](enter-iap-properties.md)         |
| **Tipo de contenido**              | Obligatorio                                    | [Especificar propiedades de IAP](enter-iap-properties.md)                           | 
| **Palabras clave**                  | Opcional (hasta 10 palabras clave, límite de 30 caracteres cada una) | [Especificar propiedades de IAP](enter-iap-properties.md)                 |
| **Etiqueta**                       | Opcional (límite de 3000 caracteres)             | [Especificar propiedades de IAP](enter-iap-properties.md)                           |

### Página Precios y disponibilidad 
| Nombre del campo                    | Notas                                       | Para más información                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Precio base**                | Obligatorio                                    | [Establecer precios y disponibilidad de IAP](set-iap-pricing-and-availability.md)   |
| **Mercados y precios personalizados** | Valor predeterminado: disponible en todos los mercados posibles | [Establecer precios y disponibilidad de IAP](set-iap-pricing-and-availability.md)   |
| **Precio de oferta**              | Opcional                                    | [Establecer precios y disponibilidad de IAP](set-iap-pricing-and-availability.md)   |
| **Distribución y visibilidad** | Valor predeterminado: los clientes pueden encontrar el IAP explorando o buscando en la Tienda | [Establecer precios y disponibilidad de IAP](set-iap-pricing-and-availability.md) |
| **Fecha de publicación**              | Valor predeterminado: publicar tan pronto como el IAP supere la certificación | [Establecer precios y disponibilidad de IAP](set-iap-pricing-and-availability.md)   |

### Página Descripciones
Una descripción obligatoria. Se recomienda proporcionar descripciones para cada idioma que admite la aplicación.

| Nombre del campo                    | Notas                                       | Para más información       |
|-------------------------------|---------------------------------------------|---------------------|
| **Título**                     | Obligatorio (límite de 100 caracteres)              | [Crear descripciones de IAP](create-iap-descriptions.md)                     |
| **Descripción**               | Opcional (límite de 200 caracteres)              | [Crear descripciones de IAP](create-iap-descriptions.md)                     |
| **Icono**                      | Opcional (.png, 300 x 300 píxeles)             | [Crear descripciones de IAP](create-iap-descriptions.md)                     |

Cuando hayas terminado de especificar esta información, haz clic en **Enviar a la Tienda**. En la mayoría de los casos, el proceso de certificación tarda aproximadamente una hora. Después, el IAP se publicará en la Tienda y estará listo para que los clientes puedan comprarlo.

**Nota**  El IAP también debe implementarse en el código de la aplicación. Para más información, consulta [Habilitar compras de productos desde la aplicación](https://msdn.microsoft.com/library/windows/apps/mt219684).


## Actualizar un IAP después de su publicación


Puedes realizar cambios en un IAP publicado en cualquier momento. Los cambios de los IAP se envían y se publican independientemente de la aplicación, de modo que, por lo general, no es necesario actualizar toda la aplicación para realizar cambios en un IAP, como actualizar su precio o su descripción.

> **Importante**  Si la aplicación está disponible para los clientes de Windows 8.x, tendrás que crear y publicar un nuevo envío de aplicación para que las actualizaciones de los IAP estén visibles para dichos clientes. Del mismo modo, si agregas nuevos IAP a una aplicación destinada a Windows 8.x después publicarse la aplicación, tendrás que actualizar el código de la aplicación para hacer referencia a esos IAP y, a continuación, volver a enviar la aplicación. De lo contrario, las nuevas IAP no serán visibles para los clientes de Windows 8.x.

Para enviar actualizaciones, ve a la página del IAP en el panel y haz clic en **Actualizar**. Esto creará un nuevo envío para el IAP, con la información de tu envío anterior como punto de partida. Cambia la información que desees y, a continuación, haz clic en **Enviar a la Tienda**.

Si quieres quitar un IAP que ofrecías anteriormente, puedes hacerlo si creas un nuevo envío y cambias la opción [Distribución y visibilidad](set-iap-pricing-and-availability.md) a **Ya no está disponible para comprar. No se muestra en la descripción de la aplicación**. Asegúrate de actualizar el código de la aplicación según sea necesario para quitar también las referencias al IAP.



<!--HONumber=May16_HO2-->


