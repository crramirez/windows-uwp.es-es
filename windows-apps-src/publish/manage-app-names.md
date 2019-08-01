---
Description: Ve los nombres que has reservado para tu aplicación, reserva nombres adicionales (para otros idiomas o para cambiar el nombre de la aplicación) y elimina nombres reservados que ya no necesites.
title: Administrar nombres de aplicación
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP, nombres de aplicación, cambiar el nombre de la aplicación, actualizar el nombre de la aplicación, el nombre del juego, el nombre del producto
ms.localizationpriority: medium
ms.openlocfilehash: 0022c53dc3afc7e710495900898d3fc5c81ea45a
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682685"
---
# <a name="manage-app-names"></a>Administrar nombres de aplicación

La **Administración de nombres de aplicación** le permite ver todos los nombres que ha reservado para la aplicación, reservar nombres adicionales (para otros idiomas o cambiar el nombre de la aplicación) y eliminar los nombres que no necesita. Puede encontrar esta página en el [centro de Partners](https://partner.microsoft.com/dashboard) ; para ello, expanda la sección **Administración de aplicaciones** en el menú de navegación izquierdo de cualquiera de las aplicaciones.

> [!IMPORTANT]
> Puede reservar nombres adicionales para una aplicación y puede usar una de ellas en la versión publicada de la aplicación en lugar de la que ha reservado cuando creó la aplicación por primera vez en el centro de Partners. Sin embargo, tenga en cuenta que el primer nombre que se reserva para el producto se usará en algunos de los detalles de su [identidad](view-app-identity-details.md), como el **nombre de familia de paquete (PFN)** . Estos valores pueden ser visibles para algunos usuarios y no se pueden cambiar, por lo que debe asegurarse de que el nombre que se reserva primero es adecuado para este uso.


## <a name="reserve-additional-names-for-your-app"></a>Reservar más nombres para la aplicación

Puedes reservar varios nombres de aplicación para la misma aplicación. Esto es especialmente útil si ofreces tu aplicación en varios idiomas y deseas usar nombres distintos para diferentes idiomas. También puede reservar un nombre nuevo para cambiar el nombre de una aplicación, como se describe a continuación.

Para reservar un nombre de aplicación nuevo, busque el cuadro de texto en la sección **reservar más nombres** de la página **administrar nombres de aplicaciones** . Escribe el nombre que te gustaría reservar y haz clic en **Comprobar disponibilidad**. Si el nombre está disponible, haz clic en **Reservar nombre de producto**. Puede reservar varios nombres de aplicación repitiendo estos pasos, si lo desea.

> [!NOTE]
> Para obtener más información sobre la reserva de nombres de aplicación y sobre por qué un nombre determinado puede no estar disponible, consulta [Crear tu aplicación reservando un nombre](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Eliminar nombres de aplicación

Si ya no quieres usar un nombre que has reservado previamente, puedes liberarlo eliminándolo aquí. Asegúrate de que realmente quieres hacerlo, ya que esto significa que el nombre estará disponible inmediatamente para que otra persona lo reserve y use.

Para eliminar uno de los nombres reservados de la aplicación, busca el nombre que quieres dejar de usar y haz clic en **Eliminar**. En el cuadro de diálogo de confirmación, vuelve a hacer clic en **Eliminar** para confirmar.

Tenga en cuenta que la aplicación debe tener al menos un nombre reservado. Para quitar completamente una aplicación del centro de Partners (y liberar todos los nombres que ha reservado para esa aplicación), haga clic en **eliminar esta aplicación** en la página de **información general** de la aplicación. Si tienes un envío de la aplicación en curso, primero deberás eliminarlo. Tenga en cuenta que si ya ha publicado la aplicación en la tienda, no podrá eliminarla del centro de Partners (aunque puede usar la funcionalidad **Mostrar u ocultar productos** en la página de **información general** para ocultarla). 


## <a name="rename-an-app-that-has-already-been-published"></a>Cambiar el nombre de una aplicación ya publicada

Si la aplicación ya está en la Tienda y quieres cambiarle el nombre, puedes hacerlo reservando un nombre nuevo (siguiendo los pasos descritos anteriormente) y, a continuación, creando un nuevo envío de la aplicación. 

Debe actualizar los paquetes de la aplicación para reemplazar el nombre anterior por el nuevo y cargar los paquetes actualizados en el envío de.
- En primer lugar, actualiza el archivo Package.StoreAssociation.xml para usar el nombre nuevo, ya sea manualmente o con Visual Studio (**Proyecto > Tienda > Asociar aplicación con la Tienda...** ). Para obtener más información, consulte [empaquetar una aplicación para UWP con Visual Studio](/windows/msix/package/packaging-uwp-apps).
- También tendrás que actualizar el elemento [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) del manifiesto de la aplicación y actualizar todos los gráficos o texto que incluyan el nombre de la aplicación. 
  > [!IMPORTANT]
  > Asegúrate de actualizar el archivo Package.StoreAssociation.xml antes de cambiar el elemento **Package/Properties/DisplayName** de manifiesto de la aplicación o se puede producir un error.

Para actualizar una lista de tiendas de modo que use el nuevo nombre, vaya a la [Página de lista](create-app-store-listings.md) de la tienda correspondiente a ese idioma y seleccione el nombre en la lista desplegable nombre del **producto** . Asegúrese de revisar la descripción y otras partes de la lista para ver las menciones del nombre y realizar las actualizaciones si es necesario.

> [!NOTE]
> Si la aplicación tiene paquetes o listas de tiendas en varios idiomas, deberá actualizar los paquetes o las listas de la tienda para cada idioma en el que sea necesario actualizar el nombre.

Una vez que la aplicación se ha publicado con el nuevo nombre, puede eliminar cualquier nombre anterior que ya no necesite usar.

> [!TIP]
> Cada aplicación aparece en el centro de Partners con el nombre que ha reservado para ella. Si ha seguido los pasos anteriores para cambiar el nombre de una aplicación y desea que aparezca en el centro de Partners con el nuevo nombre, debe eliminar el nombre original (haciendo clic en **eliminar** en la página **administrar nombres de aplicación** ). 

 

 




