---
Description: Si has proporcionado paquetes destinados a diferentes sistemas operativos, tienes la opción de personalizar partes de la descripción de la Tienda para cada sistema operativo de destino.
title: Creación de descripciones de la Tienda específicas de la plataforma
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, personalizar, enumerar, descripción, anteriormente
ms.localizationpriority: medium
ms.openlocfilehash: 50c14ed3fa7f7f5f7d1b4c80bb8165b3f27e522a
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63777698"
---
# <a name="create-platform-specific-store-listings"></a>Creación de descripciones de la Tienda específicas de la plataforma


Si la aplicación publicada previamente tiene paquetes que tienen como destino distintos sistemas operativos, tiene la opción de personalizar los elementos de la lista de Store para los clientes de versiones anteriores del sistema operativo (Windows 8.x o una versión anterior o Windows Phone 8.x o versiones anteriores). 

Los clientes de Windows 10 (incluidos Xbox) verán siempre el valor predeterminado [lista Store](create-app-store-listings.md). No verá la opción para crear listas de Store específicos de la plataforma, a menos que ya ha publicado la aplicación con los paquetes que admiten una o varias versiones del sistema operativo anteriores. 

> [!IMPORTANT]
> A partir del 31 de octubre de 2018, los productos recién creada no pueden incluir los paquetes destinados a 8.x/Windows Windows Phone 8.x o versiones anteriores. Para obtener más información, consulte este [entrada de blog](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97).

Anuncios de Store específicos de la plataforma pueden ser útiles si desea hablar de las características que solo aparecen en una versión de sistema operativo, o quiere proporcionar capturas de pantalla son específicas de un sistema operativo determinado (independientemente del tipo de dispositivo).

> [!NOTE]
> Crear una plataforma específica Store enumerar en un lenguaje no crea una lista de Store específicos de la plataforma en otros lenguajes que admite la aplicación. Deberás crear la descripción de la Tienda específica de la plataforma por separado para cada idioma. Observe también que no se puede [importar y exportar datos de la lista de Store](import-and-export-store-listings.md) para programas específicos de la plataforma.


## <a name="creating-a-platform-specific-store-listing"></a>Creación de una descripción de la Tienda específica de la plataforma

Cerca de la parte superior de su **lista Store** página, si la aplicación publicada previamente incluye paquetes que admiten las versiones anteriores del sistema operativo ((Windows 8.x o una versión anterior o Windows Phone 8.x o una versión anterior), puede seleccionar **crear un anuncio de específicos de la plataforma app Store**. Después de seleccionar esta opción, se te pedirá que elijas las versiones de SO de destino que admiten el envío. Una vez que ya ha creado las listas de Store específicos de la plataforma para todas las versiones anteriores del sistema operativo se dirija su aplicación, no podrá realizar otra selección.

Puede usar la lista de Store predeterminada (Windows 10) como punto de partida, que le llevará a través de texto correspondiente y las imágenes que ha escrito para su Store predeterminada listado; a continuación, podrá realizar cualquier cambio que desee antes de guardar. También puedes empezar con una descripción de la Tienda completamente en blanco si lo prefieres.

Después de hacer clic en **Continuar**, la página **Descripción de la Tienda** ahora incluirá una sección para la descripción de la Tienda específica de la plataforma que acabas de crear. Esta sección incluirá su propio conjunto de campos de **Descripción** (obligatorio), **Novedades de la versión**, **Capturas de pantalla**, **Icono de ventana de aplicación**, **Funciones de la aplicación** y **Requisitos adicionales del sistema**. Asegúrate de escribir en todos los campos de la descripción de la Tienda personalizada que quieres que muestren información, aunque sea la misma que la de la descripción de la Tienda predeterminada. Si dejas en blanco alguno de estos campos, no se mostrará información sobre ellos en la descripción de la Tienda personalizada.

> [!IMPORTANT]
> Los campos de la sección [Información adicional](create-app-store-listings.md#additional-information) de la descripción de Store no pueden personalizarse para distintas versiones del sistema operativo.
> 
> Además, dado que algunos de los campos de la página predeterminada [Descripción de Store](create-app-store-listings.md) solo se aplican a clientes de Windows 10, no verás las mismas opciones al crear una descripción de Store específica de la plataforma. Por ejemplo, no puedes agregar tráileres a una descripción de Microsoft Store específica de la plataforma, porque solo se muestran a los clientes de Windows 10, versión 1607 o posterior. 

Aún puede editar anuncios específicos de la plataforma según sea necesario para realizar cambios para los clientes de una determinada versión del sistema operativo.


## <a name="removing-a-platform-specific-store-listing"></a>Supresión de una descripción de la Tienda específica de la plataforma

Si creas una descripción de Microsoft Store específica de la plataforma y más adelante decides mostrar la descripción de Microsoft Store predeterminada a los clientes que usan ese sistema operativo, selecciona el vínculo **Eliminar** junto a la descripción.

Después de confirmar que quieres mostrar a esos clientes la descripción de Microsoft Store predeterminada, selecciona **Aceptar**. Se quitará la descripción de Microsoft Store específica de la plataforma (para todos los idiomas en los que existía) y los clientes de esa versión del sistema operativo ahora verán la descripción predeterminada de Microsoft Store. Si cambias de opinión, puedes seguir los pasos anteriores y crear otra descripción de Microsoft Store específica de la plataforma para ese sistema operativo.
 

 




