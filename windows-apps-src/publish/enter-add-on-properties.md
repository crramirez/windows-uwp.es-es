---
author: jnHs
Description: "Cuando envías un complemento, las opciones de la página Propiedades te ayudan a determinar el comportamiento que tendrá el complemento al ofrecerlo a los clientes."
title: Especificar las propiedades de complemento
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 186088f249c2e6fe116c970bd1969fcb59863ba6
ms.lasthandoff: 02/07/2017

---

# <a name="enter-add-on-properties"></a>Especificar las propiedades de complemento


Cuando envías un complemento, las opciones de la página **Propiedades** te ayudan a determinar el comportamiento que tendrá el complemento al ofrecerlo a los clientes.

## <a name="product-type"></a>Tipo de producto

El tipo de producto se selecciona en el momento de [crear el complemento](set-your-add-on-product-id.md). Aquí se muestra el tipo de producto seleccionado, pero no se puede cambiar.

> **Nota** Si no has publicado el complemento, puedes eliminar el envío y empezar de nuevo si necesitas elegir un tipo de producto diferente. 

Según el tipo de producto seleccionado, puede que veas uno de los siguientes campos:

### <a name="product-lifetime"></a>Duración del producto
Si seleccionaste **Duradero** para el tipo de producto, aquí se muestra la **Duración del producto**. La **Duración del producto** predeterminada para un complemento duradero es **Para siempre**, lo que significa que el complemento no caduca nunca. Si lo prefieres, puedes definir la **Duración del producto** de forma que el complemento expire tras un período establecido (con opciones que van de 1 a 365 días). 

### <a name="quantity"></a>Cantidad
Si seleccionaste **Consumible administrado por la Tienda** para el tipo de producto, aquí se muestra la **Cantidad**. Debes escribir un número entre 1 y 1000000. Esta cantidad se concederá al cliente cuando adquiera el complemento y la Tienda hará un seguimiento del saldo cuando la aplicación notifique el consumo del complemento por parte del cliente.

## <a name="content-type"></a>Tipo de contenido

Sea cual sea el tipo de producto del complemento, también será necesario indicar el tipo de contenido que se ofrece. Para la mayoría de complementos, el tipo de contenido debe ser **Descarga de software electrónico**. Si hay otra opción de la lista que describa mejor tu complemento (por ejemplo, si ofreces una descarga de música o un libro electrónico), selecciona esa opción en su lugar. 

Estas son las posibles opciones para el tipo de contenido de un complemento:

-   Descarga de software electrónico
-   Libros electrónicos
-   Número único de revista electrónica
-   Número único de diario electrónico
-   Descarga de música
-   Streaming de música
-   Servicios/almacenamiento de datos en línea
-   Descarga de vídeo
-   Streaming de vídeo
-   Software como servicio

## <a name="keywords"></a>Palabras clave

Tienes la opción de proporcionar hasta diez palabras clave de hasta 30 caracteres cada una para cada complemento que envíes. Tu aplicación podrá, entonces, buscar los complementos que coincidan con estas palabras. Esta función te permite crear pantallas en la aplicación que pueden cargar complementos sin tener que especificar directamente el id. de producto en el código de la aplicación. Puedes cambiar las palabras clave del complemento en cualquier momento, sin tener que realizar cambios en el código de la aplicación ni volver a enviarla.

> **Nota** Las palabras clave no están disponibles para su uso en paquetes orientados a Windows 8 y Windows 8.1.

## <a name="custom-developer-data"></a>Datos del desarrollador personalizados

Es posible escribir hasta 3000 caracteres en el campo **Datos del desarrollador personalizados** (anteriormente denominado **Etiqueta**) para proporcionar contexto adicional para el producto desde la aplicación. A menudo, se realiza con una cadena XML, pero puedes introducir cualquier cosa que quieras en este campo.

Para consultar este campo, usa la propiedad [StoreSku.CustomDeveloperData](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.storesku.customdeveloperdata.aspx) del [espacio de nombres Windows.Services.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx). (O, si estás usando el [espacio de nombres Windows.ApplicationModel.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx), usa la propiedad [ProductListing.Tag](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.productlisting.tag.aspx)).

Por ejemplo, supongamos que tienes un juego y que vendes una bolsa de monedas de oro como complemento. Con el campo **Datos del desarrollador personalizados**, la aplicación puede buscar la bolsa de oro. Puedes ajustar el valor en cualquier momento (en este caso, el número de monedas de la bolsa) actualizando la información del campo **Datos del desarrollador personalizados** del complemento, sin tener que realizar cambios en el código de la aplicación ni volver a enviarla.

> **Nota**  El campo **Datos del desarrollador personalizados** no está disponible para su uso en paquetes destinados a Windows 8 y Windows 8.1.

 

 

 





