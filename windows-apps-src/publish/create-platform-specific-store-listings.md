---
Description: Si has proporcionado paquetes destinados a diferentes sistemas operativos, tienes la opción de personalizar partes de la descripción de la Tienda para cada sistema operativo de destino.
title: Creación de descripciones de la Tienda específicas de la plataforma
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, personalizar, enumerar, descripción, anteriormente
ms.localizationpriority: medium
ms.openlocfilehash: b0008095ac027a1ef1d17b655610a3ad50a6596b
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220508"
---
# <a name="create-platform-specific-store-listings"></a>Creación de descripciones de la Tienda específicas de la plataforma


Si la aplicación publicada anteriormente tiene paquetes que tienen como destino distintos sistemas operativos, tiene la opción de personalizar partes de la lista de tiendas para clientes con versiones anteriores del sistema operativo (Windows 8. x o anterior y/o Windows Phone 8. x o anterior). 

Los clientes de Windows 10 (incluido Xbox) siempre verán la [lista de tiendas](create-app-store-listings.md)predeterminada. No verá la opción de crear listas de tiendas específicas de la plataforma a menos que ya haya publicado la aplicación con paquetes que admitan una o varias versiones anteriores del sistema operativo. 

> [!IMPORTANT]
> Ya no se pueden cargar nuevos paquetes XAP compilados con los SDK de Windows Phone 8. x. Las aplicaciones que ya se encuentran en el almacén con paquetes XAP seguirán funcionando en dispositivos Windows 10 Mobile. Para obtener más información, consulte esta [entrada de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Las listas de tiendas específicas de la plataforma pueden ser útiles si desea mencionar características que solo aparecen en una versión del sistema operativo o desea proporcionar capturas de pantallas específicas de un sistema operativo determinado (independientemente del tipo de dispositivo).

> [!NOTE]
> Al crear una lista de almacenes específicos de la plataforma en un lenguaje no se crea una lista de almacenes específicos de la plataforma en otros idiomas que admite su aplicación. Deberás crear la descripción de la Tienda específica de la plataforma por separado para cada idioma. Además, tenga en cuenta que no puede [importar y exportar datos](import-and-export-store-listings.md) de la lista de almacenes para listas específicas de la plataforma.


## <a name="creating-a-platform-specific-store-listing"></a>Creación de una descripción de la Tienda específica de la plataforma

Cerca de la parte superior de la página de la descripción de la **tienda** , si la aplicación publicada anteriormente incluye paquetes que admiten versiones anteriores del sistema operativo ((Windows 8. x o anterior y/o Windows Phone 8. x o anterior), puede seleccionar **crear una lista de App Store específica de la plataforma**. Después de seleccionar esta opción, se le pedirá que elija entre las versiones de sistema operativo de destino que admite el envío. Una vez que haya creado listas de tiendas específicas de la plataforma para todas las versiones anteriores del sistema operativo a las que se destina la aplicación, no podrá realizar otra selección.

Puede usar la lista de tiendas (Windows 10) predeterminada como punto de partida, lo que le llevará por el texto y las imágenes correspondientes que ha escrito para la lista de tiendas predeterminada. podrá realizar los cambios que quiera antes de guardar. También puedes empezar con una descripción de la Tienda completamente en blanco si lo prefieres.

Después de hacer clic en **continuar**, la página de la tienda de **tiendas** incluirá ahora una sección para la lista de almacenes específicos de la plataforma que acaba de crear. En esta sección se incluye su propio conjunto de campos para la **Descripción** (obligatorio), las novedades **de esta versión**, las **capturas de pantallas**, el icono de icono de la **aplicación**, **las características**de la aplicación y **los requisitos del sistema adicionales**. Asegúrate de escribir en todos los campos de la descripción de la Tienda personalizada que quieres que muestren información, aunque sea la misma que la de la descripción de la Tienda predeterminada. Si dejas en blanco alguno de estos campos, no se mostrará información sobre ellos en la descripción de la Tienda personalizada.

> [!IMPORTANT]
> Los campos de la sección [información adicional](create-app-store-listings.md#additional-information) de la lista de tiendas no se pueden personalizar para diferentes versiones de sistema operativo.
> 
> Además, como algunos de los campos de la página de [lista de tiendas](create-app-store-listings.md) predeterminada solo se aplican a los clientes de Windows 10, no verá las mismas opciones al crear una lista de almacenes específicos de la plataforma. Por ejemplo, no puede Agregar finalizadores a una lista de almacenes específicos de la plataforma, ya que los finalizadores solo se muestran a los clientes de Windows 10, versión 1607 o posterior. 

Puede seguir editando los listados específicos de la plataforma según sea necesario para realizar cambios para los clientes en una determinada versión del sistema operativo.


## <a name="removing-a-platform-specific-store-listing"></a>Supresión de una descripción de la Tienda específica de la plataforma

Si crea una lista de tiendas específica de la plataforma y más adelante decide que prefiere mostrar la lista de tiendas predeterminada a los clientes de ese sistema operativo, seleccione el vínculo **eliminar** junto a esa lista.

Después de confirmar que le gustaría mostrar a los clientes la lista de tiendas predeterminada, seleccione **Aceptar**. La lista de almacenes específicos de la plataforma se quitará (para todos los idiomas en los que existía) y los clientes de esa versión del sistema operativo ahora verán la lista de tiendas predeterminada. Si cambia de opinión, puede crear otra lista de almacenes específicos de la plataforma para ese sistema operativo siguiendo los pasos indicados anteriormente.
 

 




