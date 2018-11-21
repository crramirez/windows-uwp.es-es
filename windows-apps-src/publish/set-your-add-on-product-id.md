---
author: jnHs
Description: When you create a new add-on in Partner Center, you need to specify a product type and assign it a product ID.
title: Establecer el tipo del producto de tu complemento y el id. del producto
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, complementos, iap, duradero, consumible, suscripción, tipo de producto, id. de producto, compra desde la aplicación, producto desde la aplicación
ms.localizationpriority: medium
ms.openlocfilehash: 14d0cd40e0a7a170a835b000dc66ec683c2fb59c
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7443745"
---
# <a name="set-your-add-on-product-type-and-product-id"></a>Establecer el tipo del producto de tu complemento y el id. del producto

Un complemento debe estar asociado con una aplicación que has creado en [El centro de partners](https://partner.microsoft.com/dashboard) (incluso si no la has enviado aún). Puedes encontrar el botón para **Crear un complemento nuevo** en la página **Introducción** de la aplicación o en su página **Complemento**.

Después de seleccionar **Crear un nuevo complemento**, se te pedirá que especifiques un tipo de producto y asignes un id. de producto para el complemento.

## <a name="product-type"></a>Tipo de producto

En primer lugar, debes indicar qué tipo de complemento ofreces. Esta selección se refiere al modo en la que el cliente puede usar tu complemento.

> [!NOTE]
> No podrás cambiar el tipo de producto después de guardar la página para crear el complemento. Si eliges el tipo de producto incorrecto, siempre puedes eliminar el envío del complemento en curso y empezar la creación de un nuevo complemento desde el principio.

<span id="durable" />

### <a name="durable"></a>Duradero

Selecciona **Duradero** como el tipo de producto si el complemento normalmente se adquiere una sola vez. Estos complementos a menudo se usan para desbloquear funcionalidad adicional en una aplicación.

La **Duración del producto** predeterminada para un complemento duradero es **Para siempre**, lo que significa que el complemento no caduca nunca. Tienes la opción de establecer la **Duración del producto** en una duración distinta en el paso [Propiedades](enter-add-on-properties.md) del proceso de envío del complemento. Si lo haces, el complemento caducará después de la duración que especifiques (con opciones de 1 a 365 días), en cuyo caso un cliente podría volver a comprarlo después de que caduque.

### <a name="consumable"></a>Consumible

Si el complemento se puede comprar, usar (consumir) y después se puede volver a comprar, tendrás que seleccionar uno de los tipos de producto **consumibles**. Los complementos consumibles se suelen usar para cosas como dinero de juego (oro, monedas, etc.) que se pueden comprar en cantidades establecidas que luego puede usar el cliente. Para más información, consulta [Habilitar compras de complementos consumibles](../monetize/enable-consumable-add-on-purchases.md).

Hay dos tipos de complementos consumibles:
- **Consumible administrado por el desarrollador**: el saldo y el suministro deben administrarse dentro de la aplicación. Compatible en todas las versiones de SO.
- **Consumible administrado por la Tienda:** Microsoft realizará un seguimiento del saldo de todos los dispositivos de clientes que ejecuten la versión 1607 de Windows 10 o posterior. No es compatible con ninguna versión de SO anterior. Para usar esta opción, el producto principal se debe compilar con la versión 14393 del SDK de Windows 10 o posterior. Ten en cuenta que no podrás enviar un complemento consumible administradas por la tienda a la tienda hasta que se publique el producto principal (aunque puedes crear el envío en el centro de partners y empezar a trabajar en cualquier momento). Tendrás que escribir la cantidad del complemento consumible administrado por la Tienda en el paso **Propiedades** del envío.

### <a name="subscription"></a>Suscripción

Si deseas cobrar a los clientes de forma periódica para el complemento, elige **Suscripción**.

Después de que un cliente haya adquirido inicialmente un complemento de suscripción, tendrá que seguir pagando en intervalos periódicos para poder seguir usando el complemento. El cliente puede cancelar la suscripción en cualquier momento para evitar más cargos. Tendrás que especificar el período de suscripción y si ofreces una evaluación gratuita en el paso **Propiedades** del envío.

Los complementos de suscripción solo son compatibles con clientes que tienen Windows10, versión 1607 o posterior. La aplicación principal debe compilarse con SDK de Windows 10, versión 14393 o posterior, y debe usar la API de compra desde la aplicación en el espacio de nombres **Windows.Services.Store** en lugar de **Windows.ApplicationModel.Store**. Para obtener más información, consulta [Habilitar complementos de una suscripción para tu aplicación](../monetize/enable-subscription-add-ons-for-your-app.md).

Debes enviar el producto principal antes de poder publicar complementos de una suscripción a la tienda (aunque puedes crear el envío en el centro de partners y empezar a trabajar en cualquier momento).

## <a name="product-id"></a>Id. del producto

Independientemente del tipo de producto que elijas, necesitarás especificar un id. del producto único para el complemento. Este nombre se usará para identificar el complemento en el centro de partners, y puedes usar este identificador para que [hagan referencia al complemento en el código](../monetize/in-app-purchases-and-trials.md#how-to-use-product-ids-for-add-ons-in-your-code).

A continuación, se detallan algunos aspectos que se deben tener en cuenta al elegir un id. del producto:

-   Un identificador de producto debe ser único en el producto principal.
-   No se puede cambiar ni eliminar el id. del producto del complemento una vez que se haya publicado.
-   Un id. del producto no puede tener más de 100 caracteres de longitud.
-   Un id. del producto no puede incluir ninguno de los caracteres siguientes: **&lt; &gt; \* % & : \\ ? + ,**
-   Los clientes no verán el identificador de producto. (Más adelante, puedes escribir un [título y descripción](create-add-on-descriptions.md) que se muestre a los clientes.)
-   Si la aplicación publicada anteriormente es compatible con Windows Phone 8.1 o versiones anteriores, debes usar solo caracteres alfanuméricos, puntos o guiones bajos en tu identificador de producto. Si usas otros tipos de caracteres, el complemento no estará disponible para su compra para clientes que ejecutan Windows Phone 8.1 o versiones anteriores.

 
