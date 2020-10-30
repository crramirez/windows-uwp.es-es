---
description: Al crear un nuevo complemento en el centro de Partners, debe especificar un tipo de producto y asignarle un identificador de producto.
title: Establecer el tipo del producto de tu complemento y el id. del producto
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, complementos, IAP, duraderos, consumibles, suscripción, tipo de producto, ID. de producto, compra desde la aplicación, producto en la aplicación
ms.localizationpriority: medium
ms.openlocfilehash: 99360face123f5d674049b4069489f94f8c391a7
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030228"
---
# <a name="set-your-add-on-product-type-and-product-id"></a>Establecer el tipo del producto de tu complemento y el id. del producto

Un complemento debe estar asociado a una aplicación que haya creado en el centro de [Partners](https://partner.microsoft.com/dashboard) (incluso si todavía no lo ha enviado). Puede encontrar el botón para **crear un nuevo complemento** en la página de **información general** de la aplicación o en la página **Complementos** .

Después de seleccionar **crear un nuevo complemento** , se le pedirá que especifique un tipo de producto y que asigne un identificador de producto para el complemento.

## <a name="product-type"></a>Tipo de producto

En primer lugar, debes indicar qué tipo de complemento ofreces. Esta selección se refiere al modo en la que el cliente puede usar tu complemento.

> [!NOTE]
> No podrá cambiar el tipo de producto después de guardar esta página para crear el complemento. Si elige un tipo de producto equivocado, siempre puede eliminar el envío del complemento en curso y empezar de nuevo mediante la creación de un nuevo complemento.

<span id="durable" />

### <a name="durable"></a>Duradero

Seleccione **durable** como el tipo de producto si el complemento normalmente se compra solo una vez. Estos complementos se usan a menudo para desbloquear funcionalidad adicional en una aplicación.

La **duración del producto** predeterminada para un complemento duradero es **para siempre** , lo que significa que el complemento no caduca nunca. Tiene la opción de establecer la **duración del producto** en una duración diferente en el paso de [las propiedades](enter-add-on-properties.md) del proceso de envío del complemento. Si lo hace, el complemento expirará después de la duración que especifique (con opciones de 1-365 días), en cuyo caso un cliente podría comprarlo de nuevo después de que expire.

### <a name="consumable"></a>Consumibles

Si el complemento se puede comprar, usar (consumir) y volver a adquirirlo, querrá seleccionar uno de los tipos de producto **consumibles** . Los complementos consumibles se suelen usar para cosas como dinero de juego (oro, monedas, etc.) que se pueden comprar en cantidades establecidas que luego puede usar el cliente. Para obtener más información, consulte [Habilitar compras de complementos consumibles](../monetize/enable-consumable-add-on-purchases.md).

Hay dos tipos de complementos consumibles:
- **Consumible administrado por el desarrollador** : el saldo y el cumplimiento deben administrarse dentro de la aplicación. Se admite en todas las versiones del sistema operativo.
- **Consumible administrado por la Tienda:** Microsoft realizará un seguimiento del saldo de todos los dispositivos de clientes que ejecuten la versión 1607 de Windows 10 o posterior. No es compatible con ninguna versión de SO anterior. Para usar esta opción, el producto principal se debe compilar con la versión 14393 del SDK de Windows 10 o posterior. Además, tenga en cuenta que no puede enviar un complemento consumible administrado por el almacén a la tienda hasta que se publique el producto primario (aunque puede crear el envío en el centro de Partners y comenzar a trabajar en él en cualquier momento). Deberá escribir la cantidad del complemento consumible administrado por el almacén en el paso de **propiedades** de su envío.

### <a name="subscription"></a>Subscription

Si desea cobrar a los clientes de forma periódica para el complemento, elija **suscripción** .

Una vez que un cliente adquiere inicialmente un complemento de suscripción, se seguirán cobrando a intervalos recurrentes para seguir usando el complemento. El cliente puede cancelar la suscripción en cualquier momento para evitar cargos adicionales. Deberá especificar el período de suscripción y si quiere o no ofrecer una evaluación gratuita, en el paso de **propiedades** de su envío.

Los complementos de suscripción solo se admiten para los clientes que ejecutan Windows 10, versión 1607 o posterior. La aplicación primaria se debe compilar con el SDK de Windows 10 versión 14393 o posterior y debe usar la API de compras desde la aplicación en el espacio de nombres **Windows. Services. Store** en lugar del espacio de nombres **Windows. ApplicationModel. Store** . Para obtener más información, consulte [habilitación de complementos de suscripción para la aplicación](../monetize/enable-subscription-add-ons-for-your-app.md).

Debe enviar el producto primario para poder publicar complementos de suscripción en la tienda (aunque puede crear el envío en el centro de Partners y comenzar a trabajar en él en cualquier momento).

## <a name="product-id"></a>Product ID

Independientemente del tipo de producto que elija, tendrá que escribir un identificador de producto único para el complemento. Este nombre se usará para identificar el complemento en el centro de Partners, y puede usar este identificador para [hacer referencia al complemento en el código](../monetize/in-app-purchases-and-trials.md#how-to-use-product-ids-for-add-ons-in-your-code).

A continuación, se detallan algunos aspectos que se deben tener en cuenta al elegir un id. del producto:

-   Un ID. de producto debe ser único en el producto principal.
-   No se puede cambiar ni eliminar el id. del producto del complemento una vez que se haya publicado.
-   Un id. del producto no puede tener más de 100 caracteres de longitud.
-   Un ID. de producto no puede incluir ninguno de los siguientes caracteres: **&lt; &gt; \* % &: \\ ? +,**
-   Los clientes no verán el identificador del producto. (Más adelante, puedes escribir un [título y descripción](./create-app-store-listings.md) que se muestre a los clientes.)
-   Si la aplicación publicada anteriormente admite Windows Phone 8,1 o una versión anterior, solo debe usar caracteres alfanuméricos, puntos o guiones bajos en el identificador de producto. Si usas otros tipos de caracteres, el complemento no estará disponible para su compra para clientes que ejecutan Windows Phone 8.1 o versiones anteriores.

 
