---
author: jnHs
Description: "La sección de descripciones de la Tienda del proceso de envío de la aplicación es donde incluyes el texto y las imágenes que los clientes verán en la descripción de la Tienda de tu aplicación."
title: "Creación de descripciones de la Tienda de aplicaciones"
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: dcce4d53dd095c634f893d40f87eaf69cf546f1d
ms.lasthandoff: 02/07/2017

---

# <a name="create-app-store-listings"></a>Creación de descripciones de la Tienda de aplicaciones


La sección **Descripciones de la Tienda** del [proceso de envío de la aplicación](app-submissions.md) es donde proporcionas el texto y las [imágenes](app-screenshots-and-images.md) que los clientes verán en la descripción de la Tienda de tu aplicación.

Muchos de los campos de una **Descripción de la tienda** son opcionales, pero se recomienda proporcionar varias imágenes y tanta información como sea posible para que la lista destaque. El mínimo necesario para que el paso **Descripciones de la Tienda** se considere completado es una descripción de texto y al menos una [captura de pantalla](app-screenshots-and-images.md).

De manera predeterminada, usaremos la misma descripción de la Tienda (por idioma) para todos los sistemas operativos de destino. Si quieres usar una descripción de la Tienda personalizada para un sistema operativo específico, puedes [crear descripciones de la Tienda específicas de la plataforma](create-platform-specific-store-listings.md).

## <a name="store-listing-languages"></a>Idiomas de la descripción de la Tienda

Debes completar la página **Descripción de la Tienda** para al menos un idioma. Te recomendamos que proporciones una descripción de la Tienda en cada idioma que admiten los paquetes, pero que dispongas de flexibilidad para eliminar idiomas para los que no quieres proporcionar una descripción de la Tienda. También puedes crear descripciones de la Tienda en otros idiomas que no son compatibles con los paquetes.

> **Nota**  Si tu envío ya incluye paquetes, te mostraremos los [idiomas](supported-languages.md) compatibles para tus paquetes en la página de información general de envíos (a menos que elimines alguno).

Para agregar o eliminar idiomas para tus descripciones de la Tienda, haz clic en **Administrar idiomas de la descripción de la Tienda** desde la página de información general de envíos. Si ya has cargado paquetes, verás los idiomas enumerados en la sección **Idiomas admitidos por los paquetes**. Para quitar uno o más de estos idiomas, haz clic en **Quitar**. Si más adelante decides incluir un idioma que anteriormente eliminaste de esta sección, puedes hacer clic en **Agregar**.

En la sección **Idiomas de descripción de la Tienda adicionales**, puedes hacer clic en **Administrar idiomas adicionales** para agregar o quitar idiomas que *no* están incluidos en los paquetes. Activa las casillas para los idiomas que deseas agregar y, después, haz clic en **Actualizar**. Los idiomas que has seleccionado se mostrarán en la sección **Idiomas adicionales de descripción de la Tienda**. Para quitar uno o más de estos idiomas, haz clic en **Quitar** (o haz clic en **Administrar idiomas adicionales** y desactiva la casilla para los idiomas que quieres quitar).

Cuando hayas terminado de realizar las selecciones, haz clic en **Guardar** para volver a la página de información general del envío.

> **Nota** Al crear una descripción de la Tienda en un idioma que no sea compatible con los paquetes, debes indicar qué nombre de aplicación reservado debe mostrarse en esa descripción de la Tienda, ya que no hay ningún paquete asociado en ese idioma desde el que se pueda extraer el nombre. El nombre que elijas aquí solo se aplicará a la descripción de la Tienda para este idioma y no afectará al nombre que se muestra cuando un usuario instala la aplicación.

Para editar una descripción de la Tienda, haz clic en el nombre del idioma en la información general del envío. Las secciones de la página **Descripción de la Tienda** se describen a continuación.

## <a name="default-store-listing-fields"></a>Campos de descripción de la Tienda predeterminados

En la parte superior de la página **Descripción de la Tienda** se encuentran los campos asociados a tu descripción de la Tienda predeterminada para el idioma seleccionado. Estos campos se mostrarán a todos los clientes, a menos que tengas paquetes destinados a versiones anteriores del sistema operativo (Windows 8.x o versiones anteriores; Windows Phone 8.x o versiones anteriores) y crees descripciones de la Tienda específicas de la plataforma para incluir diferentes capturas de pantalla o informaciones para mostrar a los clientes con las versiones específicas del sistema operativo. Para obtener más información, consulta [Creación de descripciones de la Tienda específicas de la plataforma](create-platform-specific-store-listings.md).

### <a name="description"></a>Descripción

El campo de descripción es donde puedes indicar a los clientes qué hace la aplicación. Este campo es necesario y acepta hasta 10 000 caracteres de texto sin formato.

Para obtener consejos para que la descripción destaque, consulta [Escribir una excelente descripción de la aplicación](write-a-great-app-description.md).

### <a name="release-notes"></a>Notas de la versión

Si es la primera vez que vas a enviar la aplicación, probablemente quieras dejar este campo en blanco. Respecto a una actualización de una aplicación existente, aquí es donde puedes informar a los clientes de lo que ha cambiado en la versión más reciente. Este campo tiene un límite de 1500 caracteres.

### <a name="screenshots"></a>Capturas de pantalla

En la mayoría de los casos, verás varios campos para proporcionar capturas de pantalla para distintos tipos de dispositivos. No es necesario que proporciones capturas de pantalla independientes para cada tipo de dispositivo; solo es necesaria una captura de pantalla para el envío (aunque puedes proporcionar hasta nueve por tipo de dispositivo). En la mayoría de los casos, se recomienda proporcionar capturas de pantalla en todos los tipos de dispositivo que admita la aplicación, para que los clientes vean las imágenes que se asemejan al aspecto que tendrá la aplicación en su dispositivo.

Para obtener más información, consulta [capturas de pantalla e imágenes de la aplicación](app-screenshots-and-images.md).

### <a name="app-tile-icon"></a>Icono de la aplicación

La ventana de la aplicación se usa al mostrar la descripción de la Tienda de la aplicación a los clientes en Windows Phone 8.1 y versiones anteriores (y en algunos diseños de la Tienda para clientes en Windows 10). Debe ser un archivo .png que mida de 300 x 300 píxeles.

Para más información, consulta [Icono de la ventana de aplicaciones](app-screenshots-and-images.md#app-tile-icon).

### <a name="app-features"></a>Funciones de la aplicación

Se trata de resúmenes de las funciones clave de la aplicación. Se muestran al cliente en una lista con viñetas en su descripción de la Tienda de la aplicación, junto con su descripción. Hazlas breves, con unas cuantas palabras por función (y no más de 200 caracteres). Puedes incluir hasta 20 funciones.

**Nota**  Aparecerán con viñetas en la descripción de la Tienda, así que no agregues tus propias viñetas.

### <a name="additional-system-requirements"></a>Requisitos adicionales del sistema

Si es necesario, puedes describir las configuraciones de hardware que requiere tu aplicación para funcionar correctamente (más allá de la información que proporcionaste en la sección **Requisitos del sistema** en las [Propiedades de la aplicación](enter-app-properties.md#system-requirements). Esto es especialmente importante, si la aplicación requiere hardware que quizás no esté disponible en todos los equipos.

 Puedes escribir hasta 11 elementos para **Hardware mínimo** y **Hardware recomendado**.  Se muestran al cliente en una lista con viñetas en la descripción de la aplicación. Hazlas breves, con unas cuantas palabras por elemento (y no más de 200 caracteres). La información que escribas aquí se mostrará a los clientes que consultan la descripción de la Tienda de aplicaciones en Windows 10, versión 1607 o posterior, junto con los requisitos que indicaste en la página de propiedades del producto.

**Nota**  Aparecerán con viñetas en la información, así que no agregues tus propias viñetas.

## <a name="shared-fields"></a>Campos compartidos

Los elementos que se describen a continuación son todos campos compartidos y se aplicarán a todas las descripciones de la Tienda en un idioma determinado, independientemente del sistema operativo, incluso si [creas descripciones de la Tienda específicas de la plataforma](create-platform-specific-store-listings.md).

### <a name="keywords"></a>Palabras clave

Las palabras clave son palabras simples o frases cortas que no se muestran al cliente, pero que ayudan a que tu aplicación aparezca en los resultados de búsqueda relacionados con la palabra clave. Puedes incluir hasta 7 palabras clave con un máximo de 30 caracteres cada una.

Si quieres agregar palabras clave, piensa en las palabras que los clientes podrían usar al buscar aplicaciones como la tuya, especialmente si no forman parte del nombre de la aplicación. Asegúrate de no usar cualquier palabra clave que no sea relevante para tu aplicación.

### <a name="copyright-and-trademark-info"></a>Información de copyright y marca comercial

Si quieres proporcionar información adicional de copyright o marcas comerciales, escríbela aquí. Este campo tiene un límite de 200 caracteres.

### <a name="additional-license-terms"></a>Términos de licencia adicionales

Deja este campo en blanco si quieres que la aplicación se licencie a los clientes en virtud de los términos establecidos en los **Términos de licencia de aplicaciones estándar** (a los que se vincula desde el [Acuerdo para desarrolladores de aplicaciones](https://msdn.microsoft.com/library/windows/apps/hh694058)).

Si los términos de licencia son diferentes a los **Términos de licencia de aplicaciones estándar**, escríbelos aquí.

Si escribes una sola dirección URL en este campo, se mostrará a los clientes como un vínculo en el que pueden hacer clic para leer los términos de licencia adicionales. Esto es útil si los términos de licencia adicionales son muy largos, o si quieres incluir vínculos en los que se puede hacer clic o dar formato a los términos de licencia adicionales.

También puedes incluir hasta 10.000 caracteres de texto en este campo. Si lo haces, los clientes verán estos términos de licencia adicionales mostrados como texto sin formato.

### <a name="website"></a>Sitio web

Escribe la dirección URL de la página web de tu aplicación. La dirección URL debe llevar a una página de tu propio sitio web y no a la descripción web de la aplicación en la Tienda.

### <a name="support-contact-info"></a>Información de contacto de soporte técnico

Escribe la dirección de correo electrónico o la dirección URL de la página web donde los clientes pueden entrar y buscar soporte técnico para la aplicación.

**Importante**  Microsoft no proporciona a tus clientes soporte técnico para la aplicación.

### <a name="privacy-policy"></a>Directiva de privacidad

Si tienes una directiva de privacidad de la aplicación, escribe la dirección URL aquí. Eres responsable de garantizar que la aplicación cumpla con las leyes y normas de privacidad aplicables y de proporcionar una directiva de privacidad, si es necesario.

**Importante**  Microsoft no proporciona una directiva de privacidad predeterminada para tu aplicación. De igual modo, tu aplicación no está cubierta por ninguna directiva de privacidad de Microsoft. Para determinar si la aplicación requiere una directiva de privacidad, revisa el [Acuerdo para desarrolladores de aplicaciones](https://msdn.microsoft.com/library/windows/apps/hh694058) y las [Directivas de la Tienda Windows](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_5_1).

