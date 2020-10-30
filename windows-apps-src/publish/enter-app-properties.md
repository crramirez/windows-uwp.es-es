---
description: La página Propiedades de la aplicación del proceso de envío de aplicaciones te permite definir la categoría de la aplicación e indicar las preferencias de hardware u otras declaraciones.
title: Especificar las propiedades de la aplicación
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, configuración de juegos, modo de visualización, requisitos del sistema, requisitos de hardware, hardware mínimo, hardware recomendado, Directiva de privacidad, información de contacto de soporte técnico, sitio web de la aplicación, información de soporte técnico
ms.localizationpriority: medium
ms.openlocfilehash: 0e02e92d8ae005e5906179f96598805f64e7c7ff
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035148"
---
# <a name="enter-app-properties"></a>Especificar las propiedades de la aplicación

La página **propiedades** del [proceso de envío](app-submissions.md) de la aplicación es donde se define la categoría de la aplicación y se especifica otra información y declaraciones. Asegúrese de proporcionar detalles completos y precisos sobre la aplicación en esta página.


## <a name="category-and-subcategory"></a>Categoría y subcategoría

Debe indicar la categoría (y subcategoría/género, si procede) que el almacén debe usar para clasificar la aplicación. Es necesario especificar una categoría para enviar la aplicación.

Para más información, consulta [Tabla de categoría y subcategoría](category-and-subcategory-table.md).


## <a name="support-info"></a>Información de soporte técnico

Esta sección le permite proporcionar información para ayudar a los clientes a comprender mejor su aplicación y cómo obtener soporte técnico.

### <a name="privacy-policy-url"></a>Dirección URL de la directiva de privacidad

Usted es responsable de garantizar que la aplicación cumple con las leyes y las regulaciones de privacidad, y para proporcionar una dirección URL válida de la Directiva de privacidad aquí si es necesario.

En esta sección, debe indicar si la aplicación tiene acceso, recopila o transmite cualquier [información personal](/legal/windows/agreements/store-policies#105-personal-information). Si responde **sí** , se requiere una dirección URL de la Directiva de privacidad. De lo contrario, es opcional (pero si determinamos que la aplicación requiere una directiva de privacidad y no ha proporcionado ninguna, el envío puede producir un error de certificación).

> [!NOTE]
> Si se detecta que los paquetes declaran [funcionalidades](../packaging/app-capability-declarations.md) que podrían permitir el acceso, la transmisión o la recopilación de información personal, esta pregunta se marcará como **sí** y se le pedirá que escriba una dirección URL de la Directiva de privacidad.

Para ayudarle a determinar si la aplicación requiere una directiva de privacidad, revise el [contrato para desarrolladores de aplicaciones](/legal/windows/agreements/app-developer-agreement) y las directivas de [Microsoft Store](/legal/windows/agreements/store-policies#105-personal-information). 

> [!NOTE]
> Microsoft no proporciona una directiva de Privacidad predeterminada para la aplicación. De igual modo, tu aplicación no está cubierta por ninguna directiva de privacidad de Microsoft. 


### <a name="website"></a>Sitio web

Escribe la dirección URL de la página web de tu aplicación. La dirección URL debe llevar a una página de tu propio sitio web y no a la descripción web de la aplicación en la Tienda. Este campo es opcional, pero se recomienda.

### <a name="support-contact-info"></a>Información de contacto de soporte técnico

Escriba la dirección URL de la página web en la que los clientes pueden ir para obtener soporte técnico con la aplicación o una dirección de correo electrónico a la que los clientes puedan ponerse en contacto para obtener soporte técnico. Se recomienda incluir esta información para todos los envíos, de modo que los clientes sepan cómo obtener soporte técnico en caso de que lo necesiten. Tenga en cuenta que Microsoft no proporciona soporte técnico a su aplicación.

> [!IMPORTANT]
> El campo **información de contacto de soporte técnico** es necesario si la aplicación o el juego está disponible en Xbox. De lo contrario, es opcional (pero se recomienda).


## <a name="game-settings"></a>Configuración del juego

Esta sección solo aparecerá si ha seleccionado **juegos** como categoría de producto. Aquí puede especificar las características que admite el juego. La información que proporcione en esta sección se mostrará en la lista de la tienda del producto.

Si el juego es compatible con cualquiera de las opciones de varios jugadores, asegúrese de indicar el número mínimo y máximo de reproductores para una sesión. No se puede especificar más de 1.000 reproductores como mínimo o máximo.

**Multijugador multiplataforma** significa que el juego admite sesiones de varios jugadores entre reproductores en equipos con Windows 10 y Xbox.


## <a name="display-mode"></a>Modo de pantalla

Esta sección le permite indicar si el producto está diseñado para ejecutarse en una vista envolvente (no en 2D) de [Windows Mixed Reality](https://developer.microsoft.com/mixed-reality) en dispositivos PC y/o HoloLens. Si indica que es, también necesitará:
- Seleccione el **hardware mínimo** o **el hardware recomendado** para la **realidad Windows Mixed Reality** en la sección [requisitos del sistema](#system-requirements) , que aparece en la parte inferior de la página de **propiedades** .
- Especifique la **configuración de límite** (si el equipo está seleccionada) para que los usuarios sepan si está pensada para usarse solo en una posición sentada o permanente, o si permite (o requiere) que el usuario se desplace mientras lo usa. 

Si ha seleccionado **juegos** como la categoría del producto, verá opciones adicionales en la selección del **modo de presentación** que le permiten indicar si el producto admite la salida de vídeo de resolución de 4k, la salida de vídeo de alto rango dinámico (HDR) o la frecuencia de actualización variable.

Si el producto no es compatible con ninguna de estas opciones del modo de presentación, deje todas las casillas desactivadas.


## <a name="product-declarations"></a>Declaraciones de producto

Puedes marcar casillas en esta sección para indicar si alguna de las declaraciones se aplica a la aplicación. Esto puede afectar a la manera en que se muestra la aplicación, a si se ofrece a determinados clientes o al modo en que los clientes pueden usarla.

Para obtener más información, vea [declaraciones del producto](./product-declarations.md).

## <a name="system-requirements"></a>Requisitos del sistema

En esta sección, tienes la opción de indicar si determinadas características de hardware son necesarias o recomendables para que la aplicación se ejecute correctamente y para interactuar con ella. Puedes marcar la casilla (o indicar la opción adecuada) para cada elemento de hardware donde quieras especificar **Requisitos mínimos de hardware** o **Hardware recomendado** .

Si haces selecciones para **Hardware recomendado** , esos elementos se mostrarán en la descripción de la Tienda del producto como hardware recomendado para los clientes de Windows 10, versión 1607 o posterior. Los clientes con versiones anteriores del sistema operativo no verán esta información.

Si haces selecciones para **Requisitos mínimos de hardware** , esos elementos se mostrarán en la descripción de la Tienda del producto como hardware requerido para los clientes de Windows 10, versión 1607 o posterior. Los clientes con versiones anteriores del sistema operativo no verán esta información. La Tienda también puede mostrar una advertencia a los clientes que vean la descripción de la aplicación en un dispositivo que no tenga el hardware necesario. Esto no impedirá que los usuarios descarguen la aplicación en dispositivos que no tengan el hardware apropiado, pero no podrán evaluarla ni opinar sobre ella desde esos dispositivos. 

El comportamiento para los clientes dependerá de los requisitos específicos y la versión del cliente de Windows:

- **Para los clientes de Windows 10, versión 1607 o posterior:**
     - Todos los requisitos mínimos y recomendados se mostrará en la descripción de la Tienda.
     - La Tienda buscará todos los requisitos mínimos y mostrará una advertencia a los clientes que usen un dispositivo que no cumpla los requisitos.
- **Para los clientes de versiones anteriores de Windows 10:**
     - Para la mayoría de los clientes, todos los requisitos de hardware mínimos y recomendados se mostrarán en la descripción de la Tienda (aunque los clientes que visualicen una versión anterior de cliente de la Tienda solo verán los requisitos mínimos de hardware).
     - La Tienda intentará comprobar los elementos que designes como **Requisitos mínimos de hardware** , con la excepción de **Memoria** , **DirectX** , **Memoria de vídeo** , **Gráficos** y **Procesador** . Estos no se comprobarán y los clientes no verán ninguna advertencia en los dispositivos que no cumplan estos requisitos. 
- **Para los clientes de Windows 8.x y versiones anteriores, o de Windows Phone 8.x y versiones anteriores:**
     - Si activas la casilla **Requisitos mínimos de hardware** para **Pantalla táctil** , este requisito se mostrará en la descripción de la Tienda de la aplicación y los clientes con dispositivos sin pantalla táctil verán una advertencia si intentan descargar la aplicación. No se comprobarán otros requisitos ni se mostrarán en la descripción de la Tienda.

También recomendamos agregar a la aplicación comprobaciones en tiempo de ejecución para el hardware especificado, dado que la Tienda no siempre puede detectar si al dispositivo de un cliente le faltan las características seleccionadas. De todos modos, el cliente podrá descargar la aplicación, aunque se le muestre una advertencia. Si quiere evitar que la aplicación para UWP se descargue completamente en un dispositivo que no cumple los requisitos mínimos de memoria o de nivel de DirectX, puede designar los requisitos mínimos en un [archivo XML StoreManifest](/uwp/schemas/storemanifest/storemanifestschema2015/schema-root).

> [!TIP]
> Si el producto requiere elementos adicionales que no aparecen en esta sección para que se ejecuten correctamente, como impresoras 3D o dispositivos USB, también puede especificar [requisitos del sistema adicionales](create-app-store-listings.md#additional-system-requirements) al crear la lista de la tienda.
