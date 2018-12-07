---
Description: The App properties page of the app submission process lets you define your app's category and indicate hardware preferences or other declarations.
title: Introducir las propiedades de la aplicación
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, configuración de juegos, modo de presentación, requisitos del sistema, requisitos de hardware, hardware mínimo, hardware recomendado, directiva de privacidad, información de contacto de soporte técnico, sitio web de aplicaciones, información de soporte técnico
ms.localizationpriority: medium
ms.openlocfilehash: 80220f8402b225691a2e4eb3202f1f04d48e06b4
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8791058"
---
# <a name="enter-app-properties"></a>Introducir las propiedades de la aplicación

La página **Propiedades** del [proceso de envío de aplicaciones](app-submissions.md) es donde defines la categoría de la aplicación y especificas otra información y declaraciones. Asegúrate de proporcionar detalles completos y precisos sobre tu aplicación en esta página.


## <a name="category-and-subcategory"></a>Categoría y subcategoría

Debes indicar la categoría (y la subcategoría o género, si procede) que debe usar la Store para clasificar la aplicación. Es necesario especificar una categoría para enviar la aplicación.

Para más información, consulta [Tabla de categoría y subcategoría](category-and-subcategory-table.md).


## <a name="support-info"></a>Información de soporte técnico

Esta sección te permite proporcionar información para ayudar a los clientes a saber más sobre tu aplicación y cómo obtener soporte técnico.

### <a name="privacy-policy-url"></a>Dirección URL de la directiva de privacidad

Eres responsable de garantizar que tu aplicación cumpla con las leyes y normas de privacidad aplicables y de proporcionar aquí una dirección URL válida de directiva de privacidad, si se solicita.

En esta sección, debes indicar si tu aplicación accede, recopila, transmite cualquier [información personal](https://docs.microsoft.com/legal/windows/agreements/store-policies#105-personal-information) o no lo hace. Si respondes **Sí**, es necesaria una dirección URL de la directiva de privacidad. En caso contrario, es opcional (aunque si determinamos que la aplicación requiere una directiva de privacidad y no has proporcionado una, puede que tu envío no supere la certificación).

> [!NOTE]
> Si detectamos que tus paquetes declaran [funcionalidades](../packaging/app-capability-declarations.md) que pudieran permitir que se accediera a información personal, o que se transmitiera o recopilara, marcaremos esta pregunta como **Sí**, y se te requerirá que introduzcas una dirección URL de directiva de privacidad.

Para ayudarte a determinar si la aplicación requiere una directiva de privacidad, revisa el [Acuerdo para desarrolladores de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) y las [Directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies#105-personal-information). 

> [!NOTE]
> Microsoft no proporciona una directiva de privacidad predeterminada para tu aplicación. De igual modo, tu aplicación no está cubierta por ninguna directiva de privacidad de Microsoft. 


### <a name="website"></a>Sitio web

Escribe la dirección URL de la página web de tu aplicación. La dirección URL debe llevar a una página de tu propio sitio web y no a la descripción web de la aplicación de Store. Este campo es opcional, pero recomendado.

### <a name="support-contact-info"></a>Información de contacto de soporte técnico

Introduce la dirección URL de la página web o una dirección de correo electrónico donde los clientes puedan acudir para obtener soporte técnico para la aplicación. Te recomendamos que incluyas esta información para todos los envíos, para que los clientes sepan cómo obtener soporte técnico si lo necesitan. Ten en cuenta que Microsoft no proporciona a tus clientes soporte técnico para tu aplicación.

> [!IMPORTANT]
> El campo **información de contacto de soporte técnico** es necesario si la aplicación o el juego está disponible en Xbox. En caso contrario, es opcional (pero recomendado).


## <a name="game-settings"></a>Configuración del juego

Esta sección solo aparecerán si seleccionaste **Juegos** como la categoría de tu producto. Aquí puedes especificar qué características son compatibles con el juego. La información que proporcionas en esta sección se mostrará en la tienda del producto de la descripción.

Si tu juego es compatible con cualquiera de las opciones multijugador, asegúrate de indicar el número mínimo y máximo de jugadores para una sesión. No se pueden especificar más de 1000 jugadores como mínimo o como máximo.

**Opción jugador multiplataforma** significa que el juego admite sesiones de varios jugadores entre jugadores en equipos con Windows 10 y Xbox.


## <a name="display-mode"></a>Modo de presentación

Esta sección te permite indicar si tu producto está diseñado para ejecutarse en una vista envolvente (no una vista 2D) para [Windows Mixed Reality](https://developer.microsoft.com/windows/mixed-reality) en equipos o dispositivos HoloLens. Si indicas que lo está, también deberás:
- Seleccionar **Minimum hardware** o **Hardware recomendado** para **Windows Mixed Reality immersive headset** en la sección [Requisitos del sistema](#system-requirements) que aparece más abajo en la página **Propiedades**.
- Especificar la opción **Boundary setup** (si el equipo está seleccionado) para que los usuarios sepan si se ha diseñado para usarse solo en una posición de sentado o de pie, o si permite (o si requiere) que el usuario se mueva al usarlo. 

Si has seleccionado **Juegos** como categoría del producto, verás opciones adicionales en la selección **Modo de presentación** que te permite indicar si tu producto admite la salida de vídeo de resolución de 4K, salida de vídeo de Alto rango dinámico (HDR) o pantallas de frecuencia de actualización variable.

Si tu producto no admite ninguna de estas opciones de modo de pantalla, deja todas las casillas de verificación desactivadas.


## <a name="product-declarations"></a>Declaraciones de producto

Puedes marcar casillas en esta sección para indicar si alguna de las declaraciones se aplica a la aplicación. Esto puede afectar a la manera en que se muestra la aplicación, a si se ofrece a determinados clientes o al modo en que los clientes pueden usarla.

Para obtener más información, consulta [Declaraciones de producto](app-declarations.md).

## <a name="system-requirements"></a>Requisitos del sistema

En esta sección, tienes la opción de indicar si determinadas características de hardware son necesarias o recomendables para que la aplicación se ejecute correctamente y para interactuar con ella. Puedes marcar la casilla (o indicar la opción adecuada) para cada elemento de hardware donde quieras especificar **Requisitos mínimos de hardware** o **Hardware recomendado**.

Si haces selecciones para **Hardware recomendado**, esos elementos se mostrarán en la descripción de la Tienda del producto como hardware recomendado para los clientes de Windows 10, versión 1607 o posterior. Los clientes con versiones anteriores del sistema operativo no verán esta información.

Si haces selecciones para **Requisitos mínimos de hardware**, esos elementos se mostrarán en la descripción de la Tienda del producto como hardware requerido para los clientes de Windows 10, versión 1607 o posterior. Los clientes con versiones anteriores del sistema operativo no verán esta información. La Tienda también puede mostrar una advertencia a los clientes que vean la descripción de la aplicación en un dispositivo que no tenga el hardware necesario. Esto no impedirá que los usuarios descarguen la aplicación en dispositivos que no tengan el hardware apropiado, pero no podrán evaluarla ni opinar sobre ella desde esos dispositivos. 

El comportamiento para los clientes dependerá de los requisitos específicos y la versión del cliente de Windows:

- **Para los clientes de Windows 10, versión 1607 o posterior:**
     - Todos los requisitos mínimos y recomendados se mostrará en la descripción de la Tienda.
     - La Tienda buscará todos los requisitos mínimos y mostrará una advertencia a los clientes que usen un dispositivo que no cumpla los requisitos.
- **Para los clientes de versiones anteriores de Windows 10:**
     - Para la mayoría de los clientes, todos los requisitos de hardware mínimos y recomendados se mostrarán en la descripción de la Tienda (aunque los clientes que visualicen una versión anterior de cliente de la Tienda solo verán los requisitos mínimos de hardware).
     - La Tienda intentará comprobar los elementos que designes como **Requisitos mínimos de hardware**, con la excepción de **Memoria**, **DirectX**, **Memoria de vídeo**, **Gráficos** y **Procesador**. Estos no se comprobarán y los clientes no verán ninguna advertencia en los dispositivos que no cumplan estos requisitos. 
- **Para los clientes de Windows 8.x y versiones anteriores, o de Windows Phone 8.x y versiones anteriores:**
     - Si activas la casilla **Requisitos mínimos de hardware** para **Pantalla táctil**, este requisito se mostrará en la descripción de la Tienda de la aplicación y los clientes con dispositivos sin pantalla táctil verán una advertencia si intentan descargar la aplicación. No se comprobarán otros requisitos ni se mostrarán en la descripción de la Tienda.

También recomendamos agregar a la aplicación comprobaciones en tiempo de ejecución para el hardware especificado, dado que la Tienda no siempre puede detectar si al dispositivo de un cliente le faltan las características seleccionadas. De todos modos, el cliente podrá descargar la aplicación, aunque se le muestre una advertencia. Si quieres evitar completamente que tu aplicación para UWP se descargue en un dispositivo que no cumpla con los requisitos mínimos de memoria o nivel de DirectX, puedes designar los requisitos mínimos en un [archivo XML de StoreManifest](https://docs.microsoft.com/uwp/schemas/storemanifest/storemanifestschema2015/schema-root).

> [!TIP]
> Si tu producto requiere elementos adicionales que no se indican en esta sección para que se ejecute correctamente, como impresoras 3D o dispositivos USB, también puedes especificar [requisitos del sistema adicionales](create-app-store-listings.md#additional-system-requirements) cuando creas tu descripción de Microsoft Store.





