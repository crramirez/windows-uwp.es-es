---
author: jnHs
Description: When submitting an add-on, the options on the Properties page help determine the behavior of your add-on when offered to customers.
title: Especificar las propiedades de los complementos
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.author: wdg-dev-content
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, complemento, propiedades, período de suscripción, duración del producto, tipo de contenido, iap, compra desde la aplicación, producto desde la aplicación
ms.localizationpriority: medium
ms.openlocfilehash: 73a494ea1899f3a764a668ae61c1235808eff1a7
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "4614159"
---
# <a name="enter-add-on-properties"></a>Especificar las propiedades de los complementos


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

Si seleccionaste **Suscripción** para el tipo de producto, aquí se muestra la opción **Período de suscripción**. Elige una opción para especificar la frecuencia a la que se le cobrará a un cliente por la suscripción. La opción predeterminada es **mensual**, pero también puedes seleccionar **3 meses**, **6 meses**, **anualmente**o **24 meses**.

> [!IMPORTANT]
> Después de publicar el complemento, no podrás cambiar la selección de **Período de suscripción**.


## <a name="free-trial"></a>Evaluación gratuita

Si seleccionaste **Suscripción** para el tipo de producto, aquí también se muestra la opción **Evaluación gratuita**. La opción predeterminada es **Sin prueba gratuita**. Si lo prefieres, puedes permitir que los clientes usen el complemento de forma gratuita durante un período de tiempo establecido (ya sea **1 semana** o **1 mes**). 

> [!IMPORTANT]
> Después de publicar el complemento, no podrás cambiar la selección de **Evaluación gratuita**.


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

Para consultar este campo, usa la propiedad [StoreProduct.Keywords](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Keywords) del [espacio de nombres Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.Services.Store). (O, si estás usando el [espacio de nombres Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store), usa la propiedad [ProductListing.Keywords](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.Keywords)).

> [!NOTE]
> Las palabras clave no están disponibles para su uso en paquetes destinados a Windows8 y Windows8.1.

<span id="custom-developer-data" />

### <a name="custom-developer-data"></a>Datos del desarrollador personalizados

Es posible escribir hasta 3000 caracteres en el campo **Datos del desarrollador personalizados** (anteriormente denominado **Etiqueta**) para proporcionar contexto adicional para el producto desde la aplicación. A menudo, se realiza con una cadena XML, pero puedes introducir cualquier cosa que quieras en este campo. Tu aplicación puede consultar este campo para leer su contenido (aunque la aplicación no puede editar los datos ni revertir los cambios).

Por ejemplo, supongamos que tienes un juego y que vendes un complemento que permite al cliente tener acceso a niveles adicionales. Con el campo **Datos del desarrollador personalizados**, la aplicación puede realizar una consulta para ver qué niveles están disponibles cuando un cliente tiene este complemento. Puedes ajustar el valor en cualquier momento (en este caso, los niveles que se incluyen), sin tener que realizar cambios en el código de la aplicación ni volver a enviar la aplicación, simplemente actualizando la información del campo **Datos del desarrollador personalizados** y publicando un envío actualizado para el complemento.

Para consultar este campo, usa la propiedad [StoreSku.CustomDeveloperData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.customdeveloperdata#Windows_Services_Store_StoreSku_CustomDeveloperData) del [espacio de nombres Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.Services.Store). (O, si estás usando el [espacio de nombres Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store), usa la propiedad [ProductListing.Tag](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.tag#Windows_ApplicationModel_Store_ProductListing_Tag)).

> [!NOTE]
> El campo **Datos del desarrollador personalizados** no está disponible para su uso en paquetes destinados a Windows8 y Windows8.1.

 

 

 
