---
Description: Cuando envías un complemento, las opciones de la página Propiedades te ayudan a determinar el comportamiento que tendrá el complemento al ofrecerlo a los clientes.
title: Especificar las propiedades de complemento
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, complemento, propiedades, periodo de suscripción, duración del producto, tipo de contenido, IAP, compra desde la aplicación, producto en la aplicación
ms.localizationpriority: medium
ms.openlocfilehash: 9d092f443ab643b74cdd0221c96540fed0c7d474
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158001"
---
# <a name="enter-add-on-properties"></a>Especificar las propiedades de complemento

Cuando envías un complemento, las opciones de la página **Propiedades** te ayudan a determinar el comportamiento que tendrá el complemento al ofrecerlo a los clientes.

## <a name="product-type"></a>Tipo de producto

El tipo de producto se selecciona en el momento de [crear el complemento](set-your-add-on-product-id.md). Aquí se muestra el tipo de producto seleccionado, pero no se puede cambiar.

> [!TIP]
> Si no ha publicado el complemento, puede eliminarlo y empezar de nuevo si desea elegir un tipo de producto diferente.

Los campos que se ven en esta página variarán en función del tipo de producto del complemento.


## <a name="product-lifetime"></a>Duración del producto

Si seleccionó **durable** para el tipo de producto, aquí se muestra la **duración del producto** . La **duración del producto** predeterminada para un complemento duradero es **para siempre**, lo que significa que el complemento no caduca nunca. Si lo prefiere, puede cambiar la **duración del producto** para que el complemento expire después de una duración establecida (con opciones de 1-365 días).


## <a name="quantity"></a>Cantidad

Si seleccionó **consumible administrado** por el almacén para el tipo de producto, aquí se muestra la **cantidad** . Debes escribir un número entre 1 y 1000000. Esta cantidad se concederá al cliente cuando adquiera el complemento y la Tienda hará un seguimiento del saldo cuando la aplicación notifique el consumo del complemento por parte del cliente.


## <a name="subscription-period"></a>Período de suscripción

Si seleccionó **suscripción** para el tipo de producto, aquí se muestra el **período de suscripción** . Elija una opción para especificar la frecuencia con que se cobrará a un cliente por la suscripción. La opción predeterminada es **mensual**, pero también puede seleccionar **3 meses**, **6 meses**, **anualmente**o **24 meses**.

> [!IMPORTANT]
> Una vez publicado el complemento, no se puede cambiar la selección del **período de suscripción** .


## <a name="free-trial"></a>Evaluación gratuita

Si seleccionó **suscripción** para el tipo de producto, la **evaluación gratuita** también se muestra aquí. La opción predeterminada es **ninguna evaluación gratuita.** Si lo prefiere, puede permitir que los clientes usen el complemento de forma gratuita durante un período de tiempo determinado ( **1 semana** o **1 mes**). 

> [!IMPORTANT]
> Una vez publicado el complemento, no se puede cambiar la selección de la **versión de evaluación gratuita** .


## <a name="content-type"></a>Tipo de contenido

Independientemente del tipo de producto del complemento, debe indicar el tipo de contenido que está ofreciendo. Para la mayoría de complementos, el tipo de contenido debe ser **Descarga de software electrónico**. Si otra opción de la lista describe mejor el complemento (por ejemplo, si está ofreciendo una descarga de música o un libro electrónico), seleccione esa opción en su lugar.

Estas son las posibles opciones para el tipo de contenido de un complemento:

-   Descarga de software electrónico
-   Libros electrónicos
-   Número único de revista electrónica
-   Número único de diario electrónico
-   Descarga de música
-   Streaming de música
-   Servicios/almacenamiento de datos en línea
-   Software como servicio
-   Descarga de vídeo
-   Streaming de vídeo


## <a name="additional-properties"></a>Propiedades adicionales

Estos campos son opcionales para todos los tipos de complementos.

<span id="keywords" />

### <a name="keywords"></a>Palabras clave

Tienes la opción de proporcionar hasta diez palabras clave de hasta 30 caracteres cada una para cada complemento que envíes. Tu aplicación podrá, entonces, buscar los complementos que coincidan con estas palabras. Esta función te permite crear pantallas en la aplicación que pueden cargar complementos sin tener que especificar directamente el id. de producto en el código de la aplicación. Puedes cambiar las palabras clave del complemento en cualquier momento, sin tener que realizar cambios en el código de la aplicación ni volver a enviarla.

Para consultar este campo, use la propiedad [StoreProduct. keywords](/uwp/api/windows.services.store.storeproduct.Keywords) en el [espacio de nombres Windows. Services. Store](/uwp/api/Windows.Services.Store). (O bien, si está usando el [espacio de nombres Windows. ApplicationModel. Store](/uwp/api/Windows.ApplicationModel.Store), use la propiedad [ProductListing. keywords](/uwp/api/windows.applicationmodel.store.productlisting.Keywords) ).

> [!NOTE]
> Las palabras clave no están disponibles para su uso en paquetes destinados a Windows 8 y Windows 8.1.

<span id="custom-developer-data" />

### <a name="custom-developer-data"></a>Datos del desarrollador personalizados

Puede escribir hasta 3000 caracteres en el campo de **datos del desarrollador personalizado** (anteriormente denominado **etiqueta**) para proporcionar contexto adicional para el producto en la aplicación. A menudo, se trata de una cadena XML, pero se puede escribir cualquier cosa que se desee en este campo. A continuación, la aplicación puede consultar este campo para leer su contenido (aunque la aplicación no puede editar los datos y devolver los cambios).

Por ejemplo, supongamos que tiene un juego y está vendiendo un complemento que permite al cliente acceder a niveles adicionales. Mediante el campo **personalizado de datos para desarrolladores** , la aplicación puede consultar para ver qué niveles están disponibles cuando un cliente posee este complemento. Puede ajustar el valor en cualquier momento (en este caso, los niveles que se incluyen), sin tener que realizar cambios en el código en la aplicación o volver a enviar la aplicación, actualizando la información en el campo de **datos de desarrollador personalizado** del complemento y publicando después un envío actualizado para el complemento.

Para consultar este campo, use la propiedad [StoreSku. CustomDeveloperData](/uwp/api/windows.services.store.storesku.customdeveloperdata#Windows_Services_Store_StoreSku_CustomDeveloperData) en el [espacio de nombres Windows. Services. Store](/uwp/api/Windows.Services.Store). (O bien, si está usando el [espacio de nombres Windows. ApplicationModel. Store](/uwp/api/Windows.ApplicationModel.Store), use la propiedad [ProductListing. Tag](/uwp/api/windows.applicationmodel.store.productlisting.tag#Windows_ApplicationModel_Store_ProductListing_Tag) ).

> [!NOTE]
> El campo **datos de desarrollador personalizados** no está disponible para su uso en paquetes destinados a Windows 8 y Windows 8.1.

 

 

 