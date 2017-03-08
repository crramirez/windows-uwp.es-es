---
author: jnHs
Description: Si creas un nuevo complemento en el panel del Centro de desarrollo de Windows, es necesario que especifiques un tipo de producto y asignarle a este un id. del producto.
title: Establecer el tipo del producto de tu complemento y el id. del producto
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: abd6c27367476e5f1da11cde14b7d7f08105ad3e
ms.lasthandoff: 02/07/2017

---

# <a name="set-your-add-on-product-type-and-product-id"></a>Establecer el tipo del producto de tu complemento y el id. del producto

Un complemento debe estar asociado a una aplicación que ya hayas creado en el panel (incluso si todavía no la has enviado). Puedes encontrar el botón para **Crear un complemento nuevo** en la página **Introducción** de la aplicación o en su página **Complemento**.

Una vez que hagas clic en el botón, verás la página **Crear un nuevo complemento**. Aquí deberás especificar un tipo de producto y asignarle un id. del producto.

## <a name="product-type"></a>Tipo de producto

En primer lugar, debes indicar qué tipo de complemento ofreces. Esta selección se refiere al modo en la que el cliente puede usar tu complemento.

> **Nota** No podrás cambiar el tipo de producto después de guardar la página para crear el complemento. Si eliges el tipo de producto incorrecto, siempre puede eliminar el envío del complemento en curso y empezar la creación de un nuevo complemento desde el principio.

Si el producto se puede comprar, usar (consumir) y después se puede volver a comprar, tendrás que seleccionar uno de los tipos de producto **consumibles**. Los complementos consumibles se suelen usar para cosas como dinero de juego (oro, monedas, etc.) que se pueden comprar en cantidades establecidas que luego puede usar el cliente. Para obtener más información sobre cómo incluir complementos consumibles en tu aplicación, consulta [Habilitar compras de complementos consumibles](../monetize/enable-consumable-add-on-purchases.md).

Hay dos tipos de complementos consumibles que puedes seleccionar:

- **Consumible administrado por el desarrollador**: compatible con todas las versiones de SO. El saldo y el suministro deben administrarse dentro de la aplicación. 
- **Consumible administrado por la Tienda:** Microsoft realizará un seguimiento del saldo de todos los dispositivos de clientes que ejecuten la versión 1607 de Windows 10 o posterior. No es compatible con ninguna versión de SO anterior. Para usar esta opción, el producto principal se debe compilar con la versión 14393 del SDK de Windows 10 o posterior. Ten en cuenta que no podrás enviar un complemento consumible administrado por la Tienda a la Tienda hasta que se publique el producto principal (aunque puedes crear el envío en tu panel y empezar a trabajar en cualquier momento). Tendrás que escribir la cantidad del complemento consumible administrado por la Tienda en la página **Propiedades**.

Debes seleccionar **Duradero** si se puede comprar el producto solo una vez. Los complementos duraderos a menudo se usan para desbloquear funcionalidad adicional en una aplicación. Los complementos duraderos no se consumen pero puedes establecer la **duración del producto** para que caduque después de un período establecido (con opciones de 1 a 365 días). La **duración del producto** predeterminada para un complemento duradero es **para siempre**, lo que significa que el complemento no caduca nunca. Esto se puede cambiar a una duración distinta en el paso [Propiedades del complemento](enter-add-on-properties.md) del proceso de envío del complemento.

## <a name="product-id"></a>Id. del producto

Escribe un id. del producto único para el complemento. Este es el mismo identificador al que debes hacer referencia en [el código de la aplicación para llamar al complemento](https://msdn.microsoft.com/library/windows/apps/mt219684).

A continuación, se detallan algunos aspectos que se deben tener en cuenta al elegir un id. del producto:

-   Los clientes no verán este id. de producto. (Más adelante, puedes escribir un [título y descripción](create-add-on-descriptions.md) que se muestre a los clientes.)
-   No se puede cambiar ni eliminar el id. del producto del complemento una vez que se haya publicado.
-   Un id. del producto no puede tener más de 100 caracteres de longitud.
-   Un id. del producto no puede incluir ninguno de los caracteres siguientes: **&lt; &gt; \* % & : \\ ? + ,**
-   Para ofrecer el complemento en todos los dispositivos, debes usar solo caracteres alfanuméricos, puntos o guiones bajos. Si usas otros tipos de caracteres, el complemento no estará disponible para su compra para clientes que ejecutan Windows Phone 8.1 o versiones anteriores.
-   Un id. de producto no tiene por qué ser único en la Tienda Windows, pero debe ser exclusivo para tu cuenta de desarrollador.
 





