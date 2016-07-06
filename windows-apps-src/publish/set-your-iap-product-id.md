---
author: jnHs
Description: "Si se crea un nuevo IAP (producto desde la aplicación) en el panel del Centro de desarrollo de Windows, será necesario especificar un tipo de producto y asignarle a este un id. del producto."
title: Establecer el tipo de producto y el id. del producto del IAP
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.sourcegitcommit: ae4727974af632a275c102a6328734597cee3e9b
ms.openlocfilehash: 9faee009cd907cd8ccdeded019e23713cbc058f0

---

# Establecer el tipo de producto y el id. del producto del IAP

Un IAP debe estar asociado a una aplicación que ya hayas creado en el panel (incluso si todavía no la has enviado). Puedes encontrar el botón para **Crear un IAP nuevo** en la página **Introducción** de la aplicación o en su página **IAP**.

Una vez que hagas clic en el botón, verás la página **Crear un nuevo IAP**. Aquí deberás especificar un tipo de producto y asignarle un id. del producto.

## Tipo de producto

En primer lugar, debes indicar qué tipo de IAP ofreces. Esta selección se refiere al modo en la que el cliente puede usar tu IAP.

> **Nota** No podrás cambiar el tipo de producto después de guardar la página para crear el IAP. Si eliges el tipo de producto incorrecto, siempre puede eliminar el envío del IAP en curso y empezar la creación de un nuevo IAP desde el principio.

Selecciona el tipo de producto apropiado para tu IAP:

- Consumible: Un producto que se puede comprar, usar (consumir) y volverse a comprar. Los IAP consumibles se suelen usar para cosas como dinero de juego (oro, monedas, etc.) que se pueden comprar en cantidades establecidas que luego puede usar el cliente.
- Duradero: un producto que compra el usuario y le pertenece durante un período de tiempo específico. Los IAP duraderos a menudo se usan para desbloquear funcionalidad adicional en una aplicación. Los IAP duraderos no se consumen pero puedes establecer la **duración del producto** para que caduque después de un período establecido (con opciones de 1 a 365 días). La **Duración del producto** predeterminada para un IAP duradero es **Para siempre**, lo que significa que el IAP no caduca nunca. Esto se puede cambiar a una duración distinta en el paso [Propiedades del IAP](enter-iap-properties.md) del proceso de envío del IAP.

## Id. del producto

Escribe un id. del producto único para el IAP. Este es el mismo identificador al que debes hacer referencia en [el código de la aplicación para llamar al IAP](https://msdn.microsoft.com/library/windows/apps/mt219684).

A continuación se detallan algunos aspectos que se deben tener en cuenta al elegir un id. del producto:

-   Los clientes no verán este id. de producto. (Más adelante, puedes escribir un [título y descripción](create-iap-descriptions.md) que se muestre a los clientes.)
-   No se puede cambiar ni eliminar el id. de producto del IAP una vez que se haya publicado.
-   Un id. de producto no puede tener más de 100 caracteres de longitud.
-   Un id. de producto no puede incluir ninguno de los caracteres siguientes: **&lt;&gt; \* % & : \\ ? + ,**
-   Para ofrecer el IAP en todos los dispositivos, debes usar solo caracteres alfanuméricos, puntos o guiones bajos. Si usas otros tipos de caracteres, el IAP no estará disponible para su compra para clientes que ejecutan Windows Phone 8.1 o versiones anteriores.
-   Un id. de producto no tiene por qué ser único en la Tienda Windows, pero debe ser exclusivo para tu cuenta de desarrollador.
 







<!--HONumber=Jun16_HO5-->


