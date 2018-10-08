---
author: jnHs
Description: If you've provided packages targeting different operating systems, you have the option to customize parts of your Store listing for different targeted operating systems.
title: Crear descripciones de Store específicas de plataformas
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.author: wdg-dev-content
ms.date: 3/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, personalizar, enumerar, descripción, anteriormente
ms.localizationpriority: medium
ms.openlocfilehash: 6f30a825cc7aec1b6f7dbf5cff0ea1c17c43ffd7
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "4414913"
---
# <a name="create-platform-specific-store-listings"></a>Crear descripciones de Store específicas de la plataforma


Si la aplicación tiene paquetes destinados a diferentes sistemas operativos, tienes la opción de personalizar partes de la descripción de Store para clientes con versiones anteriores del sistema operativo (Windows 8.x o versiones anteriores, o Windows Phone 8.x o versiones anteriores). 

> [!IMPORTANT]
> Los clientes de Windows 10 siempre verán la [Descripción de Store](create-app-store-listings.md) predeterminada. No verás la opción de crear descripciones de Microsoft Store específicas de la plataforma, a menos que ya hayas cargado paquetes que admitan una o más versiones anteriores del sistema operativo. 

Las descripciones de la Tienda específicas de la plataforma son útiles si quieres mencionar las características que solo aparecen en una versión de SO o si quieres proporcionar capturas de pantalla que son específicas de un SO particular (independientemente del tipo de dispositivo), en lugar de que todos los clientes vean la misma descripción de la Tienda.

> [!NOTE]
> Crear una descripción de Microsoft Store específica de la plataforma en un idioma no crea una descripción de Microsoft Store específica de la plataforma en otros idiomas que la aplicación admite. Deberás crear la descripción de la Tienda específica de la plataforma por separado para cada idioma. Ten en cuenta también que no se pueden importar ni exportar datos de descripciones de Microsoft Store para descripciones específicas de la plataforma.


## <a name="creating-a-platform-specific-store-listing"></a>Creación de una descripción de la Tienda específica de la plataforma

En la parte superior de tu página **Descripción de Store**, si tu aplicación admite versiones anteriores del sistema operativo (Windows 8.x o versiones anteriores y Windows Phone 8.x o versiones anteriores), puedes seleccionar **Creación de una descripción de Store de aplicaciones específicas de la plataforma**. 

> [!TIP]
> No verás la opción de crear descripciones de Microsoft Store específicas de la plataforma hasta después de haber cargado los paquetes.

Después de seleccionar esta opción, se te pedirá que elijas las versiones de SO de destino que admiten el envío. Cuando hayas creado descripciones de Microsoft Store específicas de la plataforma para todas las versiones de SO de destino de tu aplicación, no podrás realizar otra selección. (Windows 10 no está incluido en la lista de opciones, ya que los clientes de Windows 10 siempre verán la descripción de Microsoft Store predeterminada de la aplicación).

Puedes optar por usar tu descripción de Microsoft Store predeterminada como punto de partida, lo que mostrará todo el texto aplicable y las imágenes que hayas introducido para la descripción de Microsoft Store predeterminada; después, podrás realizar las modificaciones que quieras antes de guardar. También puedes empezar con una descripción de la Tienda completamente en blanco si lo prefieres.

Después de hacer clic en **Continuar**, la página **Descripción de Store** ahora incluirá una sección para la descripción de Microsoft Store específica de la plataforma que acabas de crear. Esta sección incluirá su propio conjunto de campos de **Descripción** (obligatorio), **Novedades de la versión**, **Capturas de pantalla**, **Icono de ventana de aplicación**, **Funciones de la aplicación** y **Requisitos adicionales del sistema**. Asegúrate de escribir en todos los campos de la descripción de la Tienda personalizada que quieres que muestren información, aunque sea la misma que la de la descripción de la Tienda predeterminada. Si dejas en blanco alguno de estos campos, no se mostrará información sobre ellos en la descripción de la Tienda personalizada.


> [!IMPORTANT]
> Los campos de la sección [Información adicional](create-app-store-listings.md#additional-information) de la descripción de Store no pueden personalizarse para distintas versiones del sistema operativo.
> 
> Además, dado que algunos de los campos de la página predeterminada [Descripción de Store](create-app-store-listings.md) solo se aplican a clientes de Windows 10, no verás las mismas opciones al crear una descripción de Store específica de la plataforma. Por ejemplo, no puedes agregar tráileres a una descripción de Microsoft Store específica de la plataforma, porque solo se muestran a los clientes de Windows 10, versión 1607 o posterior. 


## <a name="removing-a-platform-specific-store-listing"></a>Supresión de una descripción de Microsoft Store específica de la plataforma

Si creas una descripción de Microsoft Store específica de la plataforma y más adelante decides mostrar la descripción de Microsoft Store predeterminada a los clientes que usan ese sistema operativo, selecciona el vínculo **Eliminar** junto a la descripción.

Después de confirmar que quieres mostrar a esos clientes la descripción de Microsoft Store predeterminada, selecciona **Aceptar**. Se quitará la descripción de Microsoft Store específica de la plataforma (para todos los idiomas en los que existía) y los clientes de esa versión del sistema operativo ahora verán la descripción predeterminada de Microsoft Store. Si cambias de opinión, puedes seguir los pasos anteriores y crear otra descripción de Microsoft Store específica de la plataforma para ese sistema operativo.

 

 




