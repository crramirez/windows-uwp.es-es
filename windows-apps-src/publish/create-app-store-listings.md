---
Description: La sección de anuncios de Store del proceso de envío es donde debe proporcionar el texto y las imágenes que los clientes verán al ver que la aplicación de la lista en la Microsoft Store.
title: Creación de descripciones de la Tienda de aplicaciones
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, enumeración, descripción, página de store, notas de la versión, título
ms.localizationpriority: medium
ms.openlocfilehash: a913c522450a8d28c03066c922df2e3e2972f92f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601860"
---
# <a name="create-app-store-listings"></a>Creación de descripciones de la Tienda de aplicaciones


La sección **Descripciones de Store** del [proceso de envío de aplicaciones](app-submissions.md) es donde proporcionas el texto y las [imágenes](app-screenshots-and-images.md) que los clientes verán al consultar la descripción dela aplicación en Microsoft Store.

Muchos de los campos de una **Descripción de la tienda** son opcionales, pero se recomienda proporcionar varias imágenes y tanta información como sea posible para que la lista destaque. El mínimo necesario para que el paso **Descripciones de la Tienda** se considere completado es una descripción de texto y al menos una [captura de pantalla](app-screenshots-and-images.md#screenshots).

> [!TIP]
> Opcionalmente puede [importar y exportar listas Store](import-and-export-store-listings.md) si prefiere escribir la información de anuncio sin conexión en un archivo .csv, en lugar de proporcionar información y cargar archivos directamente en el centro de partners. Usar la opción Importar y exportar puede ser especialmente conveniente si tienes descripciones en varios lenguajes, ya que te permite hacer varias actualizaciones a la vez. 

Si la aplicación publicada previamente admite Windows 8.x y Windows Phone 8.x o versiones anteriores, puede [crear anuncios de Store específicos de la plataforma](create-platform-specific-store-listings.md) para mostrarlos a los clientes. 

## <a name="store-listing-languages"></a>Idiomas de la descripción de la Tienda

Debes completar la página **Descripción de la Tienda** para al menos un idioma. Te recomendamos que proporciones una descripción de la Tienda en cada idioma que admiten los paquetes, pero que dispongas de flexibilidad para eliminar idiomas para los que no quieres proporcionar una descripción de la Tienda. También puedes crear descripciones de la Tienda en otros idiomas que no son compatibles con los paquetes.

> [!NOTE]
> Si tu envío ya incluye paquetes, te mostraremos los [idiomas](supported-languages.md) compatibles para tus paquetes en la página de descripción del envío (a menos que quites alguno).

Para agregar o eliminar idiomas para tus descripciones de la Store, haz clic en **Agregar/eliminar idiomas** desde la página de información general de envíos. Si ya has cargado paquetes, verás los idiomas enumerados en la sección **Idiomas admitidos por los paquetes**. Para quitar uno o más de estos idiomas, haz clic en **Quitar**. Si más adelante decides incluir un idioma que anteriormente eliminaste de esta sección, puedes hacer clic en **Agregar**.

En la sección **Idiomas de descripción de la Tienda adicionales**, puedes hacer clic en **Administrar idiomas adicionales** para agregar o quitar idiomas que *no* están incluidos en los paquetes. Activa las casillas para los idiomas que deseas agregar y, después, haz clic en **Actualizar**. Los idiomas que has seleccionado se mostrarán en la sección **Idiomas adicionales de descripción de la Tienda**. Para quitar uno o más de estos idiomas, haz clic en **Quitar** (o haz clic en **Administrar idiomas adicionales** y desactiva la casilla para los idiomas que quieres quitar).

Cuando hayas terminado de realizar las selecciones, haz clic en **Guardar** para volver a la página de información general del envío. 

## <a name="add-and-edit-store-listing-info"></a>Agregar y editar información de inscripción de Store

Para editar un listado de Store, seleccione el nombre del idioma de la página de información general de envío. Debe modificar cada idioma por separado, a menos que elija Exportar los anuncios de Store y trabajar sin conexión y, a continuación, importar todos los datos de lista a la vez. Para obtener más información acerca de cómo funciona esto, consulte [importar y exportar listas Store](import-and-export-store-listings.md).

Los campos disponibles se describen a continuación.

## <a name="product-name"></a>Nombre del producto

Este cuadro de lista desplegable le permite especificar el nombre que debe usarse en la lista de Store (si lo ha reservado más de un nombre de la aplicación).

Si ha cargado los paquetes en el mismo idioma que el Store en la lista están trabajando, se seleccionará el nombre usado en esos paquetes. Si necesita [cambiar el nombre de la aplicación](manage-app-names.md#rename-an-app-that-has-already-been-published) después de que ya se ha publicado, puede seleccionar un nombre reservado diferente cuando se crea un nuevo envío después de cargar los paquetes que utilizan el nuevo nombre.

Si no ha cargado los paquetes para el idioma que está trabajando y ha reservado más de un nombre, deberá seleccionar uno de los nombres reservados de aplicación, ya que no hay un paquete asociado en ese idioma desde el que se va a extraer el nombre.

> [!NOTE]
> El **nombre de producto** seleccione solo se aplica a la lista de Store en el idioma que está trabajando. No afecta el nombre que aparece cuando un cliente instala la aplicación; ese nombre procede del manifiesto del paquete que se instala. Para evitar confusiones, se recomienda que cada lenguaje paquetes y anuncios de Store usan el mismo nombre.

## <a name="description"></a>Descripción

El campo de descripción es donde puedes indicar a los clientes qué hace la aplicación. Este campo es necesario y acepta hasta 10 000 caracteres de texto sin formato.

Para obtener consejos para que la descripción destaque, consulta [Escribir una excelente descripción de la aplicación](write-a-great-app-description.md).

<span id="release-notes" />

## <a name="whats-new-in-this-version"></a>Novedades de esta versión

Si es la primera vez que envías la aplicación, deja este campo en blanco. Para obtener una actualización a una aplicación existente, esto es donde puede permitir a los clientes saber qué se ha cambiado en la versión más reciente. Este campo tiene un límite de 1500 caracteres. (Anteriormente, este campo se denominaba **Notas de la versión**).

## <a name="product-features"></a>Características del producto

Se trata de resúmenes de las funciones clave de la aplicación. Se muestran al cliente como lista de viñetas, en la sección **Funciones** de la descripción de Store de la aplicación, junto con la **descripción**. Hazlas breves, con unas cuantas palabras por función (y no más de 200 caracteres). Puedes incluir hasta 20 funciones.

> [!NOTE]
> Estas características aparecerán con viñetas en la lista de Store, por lo que no agregue sus propias viñetas.

## <a name="screenshots"></a>Las capturas de pantalla

Es necesaria una captura de pantalla para enviar la aplicación. Recomendamos proporcionar al menos cuatro capturas de pantalla para cada tipo de dispositivos que admita la aplicación, para que las personas puedan ver el aspecto que la aplicación tendrá en sus tipos de dispositivo.

Para obtener más información, consulta [capturas de pantalla e imágenes de la aplicación](app-screenshots-and-images.md#screenshots).


## <a name="store-logos"></a>Logotipos de la Store 

Los logotipos de la Store son imágenes opcionales que puedes subir para mejorar la forma en que la aplicación se muestra a los clientes. Además, también puedes especificar opcionalmente que solo las imágenes que subas aquí se deberían usar en la descripción de Store de la aplicación para clientes de Windows 10, en vez de permitir que Store use las imágenes de logotipos de tus paquetes de aplicaciones.

> [!IMPORTANT]
> Si tu aplicación admite Xbox, o si es compatible con Windows Phone 8.1 o versiones anteriores, debes proporcionar determinadas imágenes aquí para que la descripción aparezca correctamente en la Store. 

Para obtener más información, consulta [Logotipos de la Store](app-screenshots-and-images.md#store-logos).


## <a name="trailers-and-additional-assets"></a>Recursos adicionales y finalizadores

Puede enviar a los recursos adicionales para su producto, incluidos los finalizadores de vídeo e imágenes promocionales. Son todos opcionales, pero te recomendamos que tengas en cuenta subir como tantos como sea posible. Estas imágenes pueden ayudar a ofrecer una mejor idea de tu producto y una descripción más tentadora.

Para obtener más información, consulte [recursos adicionales y finalizadores](app-screenshots-and-images.md#trailers-and-additional-assets).

<a id="supplemental-information" />

## <a name="supplemental-fields"></a>Campos adicionales

Los campos de esta sección son todos opcionales. Revisa la información siguiente para determinar si proporcionar esta información sirve para tu envío. En particular, se recomienda la **Descripción corta** para la mayoría de los envíos. Los otros campos pueden ayudar a proporcionar una experiencia óptima para tu producto en diferentes escenarios.

### <a name="short-title"></a>Título corto

Se trata de una versión más corta del nombre de tu producto. Si se facilita, este nombre más corto puede aparecer en varios lugares de Xbox One (durante la instalación, en los logros, etc.) en lugar del título completo del producto.

Este campo tiene un límite de 50 caracteres.


### <a name="sort-title"></a>Ordenar título

Si tu producto pudiera escribirse o deletrearse de diferentes maneras, puedes introducir otra versión aquí. Esto permite a los clientes encontrar tu producto más rápidamente si escriben esa versión al buscar. 

Este campo tiene un límite de 255 caracteres.


### <a name="voice-title"></a>Título hablado

Se trata de un nombre alternativo para el producto que, si se facilita, puede usarse en la experiencia de audio en Xbox One cuando se usa Kinect o un casco.

Este campo tiene un límite de 255 caracteres.


### <a name="short-description"></a>Descripción corta

Una descripción más corta y pegadizo que se puede usar en la parte superior de la descripción de Store del producto. Si no se facilita, se utilizará en su lugar el primer párrafo (hasta 500 caracteres) de la [descripción](#description) más larga. Dado que la descripción también aparece debajo de este texto, te recomendamos que proporciones una descripción corta con otro texto para que la descripción de Store no sea repetitiva.

Para los juegos, la descripción corta también puede aparecer en la sección Información del Hub de juegos de Xbox One.

Para obtener mejores resultados, mantenga la descripción breve en 270 caracteres. El campo tiene un límite de 500 caracteres, pero en algunas vistas, se mostrarán solo los primeros 270 caracteres (con un vínculo disponible para ver el resto de la descripción breve).


### <a name="additional-system-requirements"></a>Requisitos adicionales del sistema

Si es necesario, puedes describir las configuraciones de hardware que requiere tu aplicación para funcionar correctamente (más allá de la información que proporcionaste en la sección **Requisitos del sistema** en las [Propiedades de la aplicación](enter-app-properties.md#system-requirements). Esto es especialmente importante, si la aplicación requiere hardware que quizás no esté disponible en todos los equipos. Por ejemplo, si tu aplicación solo funcionará correctamente con hardware USB externo, como una impresora 3D o un microcontrolador, se recomienda especificarlo aquí. La información que introduzcas se mostrará a los clientes que consulten la descripción de Store de la aplicación en Windows 10, versión 1607 o posterior (incluyendo Xbox), junto con los requisitos que indicaste en la página de propiedades del producto. 

Puedes escribir hasta 11 elementos para **Hardware mínimo** y **Hardware recomendado**. Estas se muestran al cliente en una lista con viñetas en la descripción de Microsoft Store. Hazlas breves, con unas cuantas palabras por elemento (y no más de 200 caracteres).

> [!NOTE]
> Los requisitos adicionales del sistema aparecerán con viñetas en la descripción de Store, por lo que no debes agregar tus propias viñetas.


<span id="shared-fields" />

## <a name="additional-information"></a>Información adicional

Los elementos que se describen a continuación ayudan a los clientes a descubrir y comprender el producto. (Esta sección se llamaba antes **Campos compartidos**).

### <a name="search-terms"></a>Términos de búsqueda

Los términos de búsqueda (anteriormente conocidos como palabras clave) son palabras simples o frases cortas que no se muestran a los clientes, pero pueden ayudar a hacer descubrible tu aplicación en Store cuando los clientes busquen usando estos términos. Puedes incluir hasta 7 términos de búsqueda con un máximo de 30 caracteres cada una y usar palabras no más de 21 entre todos los términos de búsqueda.

Al agregar términos de búsqueda, piensa en las palabras que los clientes podrían usar al buscar aplicaciones como la tuya, especialmente si no forman parte del nombre de la aplicación. Asegúrate de no usar cualquier término que no sea relevante para tu aplicación.

### <a name="copyright-and-trademark-info"></a>Información de copyright y marca comercial

Si quieres proporcionar información adicional de copyright o marcas comerciales, escríbela aquí. Este campo tiene un límite de 200 caracteres.


### <a name="additional-license-terms"></a>Términos de licencia adicionales

Deja este campo en blanco si quieres que la aplicación se licencie a los clientes en virtud de los términos establecidos en los **Términos de licencia de aplicaciones estándar** (a los que se vincula desde el [Acuerdo para desarrolladores de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)).

Si los términos de licencia son diferentes a los **Términos de licencia de aplicaciones estándar**, escríbelos aquí.

Si escribes una sola dirección URL en este campo, se mostrará a los clientes como un vínculo en el que pueden hacer clic para leer los términos de licencia adicionales. Esto es útil si los términos de licencia adicionales son muy largos, o si quieres incluir vínculos en los que se puede hacer clic o dar formato a los términos de licencia adicionales.

También puedes escribir hasta 10.000 caracteres de texto en este campo. Si lo haces, los clientes verán estos términos de licencia adicionales mostrados como texto sin formato.


### <a name="developed-by"></a>Desarrollado por

Escribe texto aquí si quieres incluir un campo **Desarrollado por** en la descripción de la Store de la aplicación. (El valor **Publicado por** indicará el nombre para mostrar del publicador asociado a tu cuenta, independientemente de si proporcionas un valor para el campo **Desarrollado por**).

Este campo tiene un límite de 255 caracteres.
 

<span id="privacy-policy" />

> [!NOTE]
> Los campos de **directiva de privacidad**, **sitio web** e **información de contacto de soporte técnico** están situados ahora en la página [Propiedades](enter-app-properties.md).

