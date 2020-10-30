---
description: La sección de listas de tiendas del proceso de envío de aplicaciones es donde se proporcionan el texto y las imágenes que verán los clientes al ver la lista de la aplicación en el Microsoft Store.
title: Creación de descripciones de la Tienda de aplicaciones
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
ms.date: 03/13/2019
ms.topic: article
keywords: Windows 10, UWP, lista, descripción, página de la tienda, notas de la versión, título
ms.localizationpriority: medium
ms.openlocfilehash: cb895dcabcc72361575f2d160b10fade03905d9a
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033298"
---
# <a name="create-app-store-listings"></a>Creación de descripciones de la Tienda de aplicaciones

La sección de **listas de tiendas** del [proceso de envío de aplicaciones](app-submissions.md) es donde se proporcionan el texto y [las imágenes](app-screenshots-and-images.md) que verán los clientes al ver la lista de la aplicación en el Microsoft Store.

Muchos de los campos de una **lista de tiendas** son opcionales, pero se recomienda proporcionar varias imágenes y tanta información como sea posible para que la lista destaque. El requisito mínimo necesario para que el paso de **lista de almacenes** se considere completo es una descripción de texto y al menos una [captura de pantalla](app-screenshots-and-images.md#screenshots).

> [!TIP]
> Opcionalmente, puede [importar y exportar los listados](import-and-export-store-listings.md) de la tienda si prefiere escribir la información de la lista sin conexión en un archivo. csv, en lugar de proporcionar información y cargar archivos directamente en el centro de Partners. El uso de la opción de importación y exportación puede ser especialmente útil si tiene listas en muchos idiomas, ya que le permite realizar varias actualizaciones a la vez.

Si la aplicación publicada previamente es compatible con Windows 8. x o Windows Phone 8. x o una versión anterior, puede [crear listas de tiendas específicas de la plataforma](create-platform-specific-store-listings.md) para que se muestren a esos clientes.

## <a name="store-listing-languages"></a>Idiomas de la descripción de la Tienda

Debes completar la página **Descripción de la Tienda** para al menos un idioma. Te recomendamos que proporciones una descripción de la Tienda en cada idioma que admiten los paquetes, pero que dispongas de flexibilidad para eliminar idiomas para los que no quieres proporcionar una descripción de la Tienda. También puedes crear descripciones de la Tienda en otros idiomas que no son compatibles con los paquetes.

> [!NOTE]
> Si el envío incluye paquetes ya, se mostrarán los [idiomas](supported-languages.md) admitidos en los paquetes en la página de información general del envío (a menos que quite cualquiera de ellos).

Para agregar o quitar idiomas de las listas de tiendas, haga clic en **Agregar o quitar idiomas** en la página de información general del envío. Si ya has cargado paquetes, verás los idiomas enumerados en la sección **Idiomas admitidos por los paquetes** . Para quitar uno o más de estos idiomas, haz clic en **Quitar** . Si más adelante decides incluir un idioma que anteriormente eliminaste de esta sección, puedes hacer clic en **Agregar** .

En la sección **Idiomas de descripción de la Tienda adicionales** , puedes hacer clic en **Administrar idiomas adicionales** para agregar o quitar idiomas que *no* están incluidos en los paquetes. Activa las casillas para los idiomas que deseas agregar y, después, haz clic en **Actualizar** . Los idiomas que has seleccionado se mostrarán en la sección **Idiomas adicionales de descripción de la Tienda** . Para quitar uno o más de estos idiomas, haz clic en **Quitar** (o haz clic en **Administrar idiomas adicionales** y desactiva la casilla para los idiomas que quieres quitar).

Cuando hayas terminado de realizar las selecciones, haz clic en **Guardar** para volver a la página de información general del envío.

## <a name="add-and-edit-store-listing-info"></a>Agregar y editar información de lista de tiendas

Para editar una lista de tiendas, seleccione el nombre de idioma en la página de información general del envío. Debe editar cada idioma por separado, a menos que decida exportar los listados de la tienda y trabajar sin conexión y, a continuación, importar todos los datos de la lista a la vez. Para obtener más información sobre cómo funciona, consulte [importación y exportación de listas de tiendas](import-and-export-store-listings.md).

Los campos disponibles se describen a continuación.

## <a name="product-name"></a>Nombre de producto

Este cuadro desplegable le permite especificar el nombre que se debe usar en la lista de tiendas (si ha reservado más de un nombre para la aplicación).

Si ha cargado paquetes en el mismo idioma que la lista de tiendas en la que está trabajando, se seleccionará el nombre usado en esos paquetes. Si necesita [cambiar el nombre de la aplicación](manage-app-names.md#rename-an-app-that-has-already-been-published) después de que ya se ha publicado, puede seleccionar un nombre reservado diferente aquí cuando cree un nuevo envío, después de cargar los paquetes que usan el nuevo nombre.

Si no ha cargado los paquetes para el idioma en el que está trabajando y ha reservado más de un nombre, deberá seleccionar uno de sus nombres de aplicación reservados, ya que no hay un paquete asociado en ese idioma desde el que extraer el nombre.

> [!NOTE]
> El **nombre de producto** que seleccione solo se aplicará a la lista de tiendas en el idioma en el que esté trabajando. No afecta al nombre mostrado cuando un cliente instala la aplicación; ese nombre procede del manifiesto del paquete que se instala. Para evitar confusiones, se recomienda que los paquetes y la lista de almacenes de cada idioma usen el mismo nombre.

## <a name="description"></a>Description

El campo de descripción es donde puedes indicar a los clientes qué hace la aplicación. Este campo es necesario y acepta hasta 10 000 caracteres de texto sin formato.

Para obtener consejos para que la descripción destaque, consulta [Escribir una excelente descripción de la aplicación](write-a-great-app-description.md).

<a name="release-notes"></a>

## <a name="whats-new-in-this-version"></a>Novedades de esta versión

Si es la primera vez que envía la aplicación, deje este campo en blanco. Para una actualización de una aplicación existente, aquí es donde puede permitir que los clientes sepan qué ha cambiado en la versión más reciente. Este campo tiene un límite de 1500 caracteres. (Anteriormente, este campo se llamaba **notas** de la versión).

## <a name="product-features"></a>Características del producto

Se trata de resúmenes de las funciones clave de la aplicación. Se muestran al cliente como una lista con viñetas en la sección **características** de la lista de la tienda de la aplicación, además de la **Descripción** . Hazlas breves, con unas cuantas palabras por función (y no más de 200 caracteres). Puedes incluir hasta 20 funciones.

> [!NOTE]
> Estas características aparecerán con viñetas en la lista de la tienda, por lo que no agregue sus propias viñetas.

## <a name="screenshots"></a>Capturas de pantalla

Se requiere una captura de pantalla para enviar la aplicación. Se recomienda proporcionar al menos cuatro capturas de pantallas para cada tipo de dispositivo que admita la aplicación para que los usuarios puedan ver el aspecto que tendrá la aplicación en el tipo de dispositivo.

Para obtener más información, consulta [capturas de pantalla e imágenes de la aplicación](app-screenshots-and-images.md#screenshots).

## <a name="store-logos"></a>Logotipos de Store

Los logotipos de la tienda son imágenes opcionales que puede cargar para mejorar la forma en que se muestra la aplicación a los clientes. Opcionalmente, también puede especificar que solo las imágenes que cargue aquí deben usarse en la lista de tiendas de la aplicación para los clientes de Windows 10 (incluida Xbox), en lugar de permitir que el almacén use imágenes de logotipo de los paquetes de la aplicación.

> [!IMPORTANT]
> Si la aplicación es compatible con Xbox, o si admite Windows Phone 8,1 o una versión anterior, debe proporcionar algunas imágenes aquí para que la lista se muestre correctamente en el almacén.

Para obtener más información, consulte los [logotipos](app-screenshots-and-images.md#store-logos)de la tienda.

## <a name="trailers-and-additional-assets"></a>Finalizadores y recursos adicionales

Puede enviar recursos adicionales para su producto, incluidos los finalizadores de vídeo e imágenes promocionales. Estos son todos opcionales, pero se recomienda que considere la posibilidad de cargar tantos como sea posible. Estas imágenes pueden ayudar a los clientes a ofrecer una mejor idea de lo que es el producto y crear una lista más atractiva.

Para obtener más información, consulte [finalizadores y recursos adicionales](app-screenshots-and-images.md#trailers-and-additional-assets).

<a name="supplemental-information"></a>

## <a name="supplemental-fields"></a>Campos adicionales

Todos los campos de esta sección son opcionales. Revise la información siguiente para determinar si tiene sentido proporcionar esta información para su envío. En concreto, se recomienda la **Descripción breve** para la mayoría de los envíos. Los demás campos pueden ayudar a proporcionar una experiencia óptima para su producto en diferentes escenarios.

### <a name="short-title"></a>Título corto

Una versión más corta del nombre del producto. Si se proporciona, este nombre más corto puede aparecer en varios lugares de Xbox One (durante la instalación, en logros, etc.) en lugar del título completo del producto.

Este campo tiene un límite de 50 caracteres.

### <a name="sort-title"></a>Título de ordenación

Si el producto se puede ordenar alfabéticamente o escribirse de diferentes maneras, puede especificar otra versión aquí. Esto permite a los clientes buscar el producto más rápidamente si escriben esa versión en durante las búsquedas.

Este campo tiene un límite de 255 caracteres.

### <a name="voice-title"></a>Título de la voz

Un nombre alternativo para el producto que, si se proporciona, se puede usar en la experiencia de audio en Xbox One al usar KINECT o auriculares.

Este campo tiene un límite de 255 caracteres.

### <a name="short-description"></a>Descripción breve

Una descripción más corta que se puede usar en la parte superior de la lista de tiendas del producto. Si no se proporciona, se usará en su lugar el primer párrafo (hasta 500 caracteres) de la [Descripción](#description) más larga. Dado que la descripción también aparece debajo de este texto, se recomienda proporcionar una breve descripción con texto diferente para que la lista de tiendas no sea repetida.

En el caso de los juegos, la descripción breve también puede aparecer en la sección de información de la central de juegos de Xbox One.

Para obtener los mejores resultados, mantenga su breve descripción en 270 caracteres. El campo tiene un límite de 1000 caracteres, pero en algunas vistas, solo se mostrarán los primeros 270 (con un vínculo disponible para ver el resto de la descripción breve).

### <a name="additional-system-requirements"></a>Requisitos adicionales del sistema

Si es necesario, puedes describir las configuraciones de hardware que requiere tu aplicación para funcionar correctamente (más allá de la información que proporcionaste en la sección **Requisitos del sistema** en las [Propiedades de la aplicación](enter-app-properties.md#system-requirements). Esto es especialmente importante, si la aplicación requiere hardware que quizás no esté disponible en todos los equipos. Por ejemplo, si la aplicación solo funcionará correctamente con hardware USB externo, como una impresora 3D o un microcontrolador, se recomienda escribirlos aquí. La información que escriba se mostrará a los clientes que vean la lista de tiendas de la aplicación en Windows 10, versión 1607 o posterior (incluida Xbox), junto con los requisitos indicados en la página de propiedades del producto.

Puedes escribir hasta 11 elementos para **Hardware mínimo** y **Hardware recomendado** . Estos se muestran al cliente como una lista con viñetas en la lista de la tienda. Hazlas breves, con unas cuantas palabras por elemento (y no más de 200 caracteres).

> [!NOTE]
> Los requisitos del sistema adicionales aparecerán con viñetas en la lista de la tienda, por lo que no agregue sus propias viñetas.

<a name="shared-fields"></a>

## <a name="additional-information"></a>Información adicional

Los elementos que se describen a continuación ayudan a los clientes a detectar y comprender el producto. (Esta sección se llamaba anteriormente **campos compartidos** ).

### <a name="search-terms"></a>Términos de búsqueda

Los términos de búsqueda (anteriormente denominados palabras clave) son palabras simples o frases cortas que no se muestran a los clientes, pero pueden ayudar a que su aplicación se pueda detectar en la tienda cuando los clientes realicen búsquedas con esos términos. Puede incluir hasta 7 términos de búsqueda con un máximo de 30 caracteres cada uno, y no puede usar más de 21 palabras separadas en todos los términos de búsqueda.

Al agregar términos de búsqueda, piense en las palabras que los clientes pueden usar al buscar aplicaciones como la suya, especialmente si no forman parte del nombre de la aplicación. Asegúrese de no usar ningún término de búsqueda que realmente no sea relevante para su aplicación.

### <a name="copyright-and-trademark-info"></a>Información de copyright y marca comercial

Si quieres proporcionar información adicional de copyright o marcas comerciales, escríbela aquí. Este campo tiene un límite de 200 caracteres.

### <a name="additional-license-terms"></a>Términos de licencia adicionales

Deja este campo en blanco si quieres que la aplicación se licencie a los clientes en virtud de los términos establecidos en los **Términos de licencia de aplicaciones estándar** (a los que se vincula desde el [Acuerdo para desarrolladores de aplicaciones](/legal/windows/agreements/app-developer-agreement)).

Si los términos de licencia son diferentes a los **Términos de licencia de aplicaciones estándar** , escríbelos aquí.

Si escribes una sola dirección URL en este campo, se mostrará a los clientes como un vínculo en el que pueden hacer clic para leer los términos de licencia adicionales. Esto es útil si los términos de licencia adicionales son muy largos, o si quieres incluir vínculos en los que se puede hacer clic o dar formato a los términos de licencia adicionales.

En este campo también puede escribir hasta 10 000 caracteres de texto. Si lo haces, los clientes verán estos términos de licencia adicionales mostrados como texto sin formato.

### <a name="developed-by"></a>Desarrollado por

Escriba aquí el texto si quiere incluir un campo **desarrollado por** en la lista de la tienda de la aplicación. (El campo **publicado por** mostrará el nombre para mostrar del publicador asociado a su cuenta, independientemente de si proporciona o no un valor para el campo **desarrollado por** ).

Este campo tiene un límite de 255 caracteres.
 
<a name="privacy-policy"></a>

> [!NOTE]
> Los **campos Directiva de privacidad** , **sitio web** y **información de contacto de soporte técnico** se encuentran ahora en la página de [propiedades](enter-app-properties.md) .
