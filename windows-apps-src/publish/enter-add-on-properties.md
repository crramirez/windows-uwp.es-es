---
author: jnHs
Description: "Cuando envías un complemento, las opciones de la página Propiedades te ayudan a determinar el comportamiento que tendrá el complemento al ofrecerlo a los clientes."
title: Especificar las propiedades de complemento
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 253e008d3622094dcfe765531d71e5f37b7777b0
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2017
---
# <a name="enter-add-on-properties"></a>Especificar las propiedades de complemento


Cuando envías un complemento, las opciones de la página **Propiedades** te ayudan a determinar el comportamiento que tendrá el complemento al ofrecerlo a los clientes.

## <a name="product-type"></a>Tipo de producto

El tipo de producto se selecciona en el momento de [crear el complemento](set-your-add-on-product-id.md). Aquí se muestra el tipo de producto seleccionado, pero no se puede cambiar.

> [!TIP]
> Si el complemento no se ha publicado, es posible eliminar el envío y empezar de nuevo si quieres elegir un tipo de producto distinto.

Los campos que ves en esta página variarán, en función del tipo de producto del complemento.

## <a name="product-lifetime"></a>Duración del producto


Si has seleccionado **Duradero** para el tipo de producto, aquí se muestra la opción **Duración del producto**. La opción predeterminada de **Duración del producto** para un complemento duradero es **Para siempre**, lo que significa que el complemento no caduca nunca. Si lo prefieres, puedes definir la **Duración del producto** de forma que el complemento expire tras un período establecido (con opciones que van de 1 a 365 días).

## <a name="quantity"></a>Cantidad


Si seleccionaste **Consumible administrado por la Tienda** para el tipo de producto, aquí se muestra la opción **Cantidad**. Debes escribir un número entre 1 y 1000000. Esta cantidad se concederá al cliente cuando adquiera el complemento y la Tienda hará un seguimiento del saldo cuando la aplicación notifique el consumo del complemento por parte del cliente.


## <a name="subscription-period"></a>Período de suscripción

Si seleccionaste **Suscripción** para el tipo de producto, aquí se muestra la opción **Período de suscripción**. Tendrás que elegir una de las opciones disponibles (**Mensualmente**, **3meses**, **6meses**, **Anualmente** o **24meses**) para indicar la frecuencia con la que se le cobrará la suscripción a un cliente. Ten en cuenta que después de publicar el complemento, no podrás cambiar la selección de **Período de suscripción**.

> [!NOTE]
> Actualmente, la capacidad para crear complementos de una suscripción solo está disponible para un conjunto de cuentas de desarrollador que participan en un programa de usuarios pioneros. En el futuro pondremos los complementos de suscripción a disposición de todas las cuentas de desarrollador. Ahora estamos proporcionando esta documentación preliminar para ofrecer a los desarrolladores una vista previa de esta característica. Para obtener más información, consulta [Habilitar complementos de una suscripción para tu aplicación](../monetize/enable-subscription-add-ons-for-your-app.md).


## <a name="free-trial"></a>Evaluación gratuita

En el caso de complementos de una suscripción, aquí se muestra también la opción **Evaluación gratuita**. Debes seleccionar si deseas permitir que los clientes usen el complemento de forma gratuita durante un período de tiempo establecido (ya sea **1semana** o **1mes**), o si lo ofreces **Sin evaluación gratuita**. Ten en cuenta que después de publicar el complemento, no podrás cambiar la selección de **Evaluación gratuita**.


## <a name="content-type"></a>Tipo de contenido

Sea cual sea el tipo de producto del complemento, será necesario indicar el tipo de contenido que se ofrece. Para la mayoría de complementos, el tipo de contenido debe ser **Descarga de software electrónico**. Si hay otra opción de la lista que describa mejor tu complemento (por ejemplo, si ofreces una descarga de música o un libro electrónico), selecciona esa opción en su lugar.

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

Para consultar este campo, usa la propiedad [StoreProduct.Keywords](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_Keywords) del [espacio de nombres Windows.Services.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx). (O, si estás usando el [espacio de nombres Windows.ApplicationModel.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx), usa la propiedad [ProductListing.Keywords](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting#Windows_ApplicationModel_Store_ProductListing_Keywords)).

> [!NOTE]
> Las palabras clave no están disponibles para su uso en paquetes destinados a Windows8 y Windows8.1.

<span id="custom-developer-data" />
### <a name="custom-developer-data"></a>Datos del desarrollador personalizados

Es posible escribir hasta 3000 caracteres en el campo **Datos del desarrollador personalizados** (anteriormente denominado **Etiqueta**) para proporcionar contexto adicional para el producto desde la aplicación. A menudo, se realiza con una cadena XML, pero puedes introducir cualquier cosa que quieras en este campo. Tu aplicación puede consultar este campo para leer su contenido (aunque la aplicación no puede editar los datos ni revertir los cambios).

Por ejemplo, supongamos que tienes un juego y que vendes un complemento que permite al cliente tener acceso a niveles adicionales. Con el campo **Datos del desarrollador personalizados**, la aplicación puede realizar una consulta para ver qué niveles están disponibles cuando un cliente tiene este complemento. Puedes ajustar el valor en cualquier momento (en este caso, los niveles que se incluyen), sin tener que realizar cambios en el código de la aplicación ni volver a enviar la aplicación, simplemente actualizando la información del campo **Datos del desarrollador personalizados** y publicando un envío actualizado para el complemento.

Para consultar este campo, usa la propiedad [StoreSku.CustomDeveloperData](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.storesku.customdeveloperdata.aspx) del [espacio de nombres Windows.Services.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx). (O, si estás usando el [espacio de nombres Windows.ApplicationModel.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx), usa la propiedad [ProductListing.Tag](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.productlisting.tag.aspx)).

> [!NOTE]
> El campo **Datos del desarrollador personalizados** no está disponible para su uso en paquetes destinados a Windows8 y Windows8.1.

 

 

 
