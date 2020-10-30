---
description: Ve los nombres que has reservado para tu aplicación, reserva nombres adicionales (para otros idiomas o para cambiar el nombre de la aplicación) y elimina nombres reservados que ya no necesites.
title: Administrar nombres de aplicación
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP, nombres de aplicación, cambiar el nombre de la aplicación, actualizar el nombre de la aplicación, el nombre del juego, el nombre del producto
ms.localizationpriority: medium
ms.openlocfilehash: 846810d07b04f05904790bf1d9ad58e8463b9cfe
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033258"
---
# <a name="manage-app-names"></a>Administrar nombres de aplicación

La **Administración de nombres de aplicación** le permite ver todos los nombres que ha reservado para la aplicación, reservar nombres adicionales (para otros idiomas o cambiar el nombre de la aplicación) y eliminar los nombres que no necesita. Puede encontrar esta página en el [centro de Partners](https://partner.microsoft.com/dashboard) ; para ello, expanda la sección **Administración de aplicaciones** en el menú de navegación izquierdo de cualquiera de las aplicaciones.

> [!IMPORTANT]
> Puede reservar nombres adicionales para una aplicación y puede usar una de ellas en la versión publicada de la aplicación en lugar de la que ha reservado cuando creó la aplicación por primera vez en el centro de Partners. Sin embargo, tenga en cuenta que el primer nombre que se reserva para el producto se usará en algunos de sus [detalles de identidad](view-app-identity-details.md), como el **nombre de familia de paquete (PFN)** . Estos valores pueden ser visibles para algunos usuarios y no se pueden cambiar, por lo que debe asegurarse de que el nombre que se reserva primero es adecuado para este uso.


## <a name="reserve-additional-names-for-your-app"></a>Reservar más nombres para la aplicación

Puedes reservar varios nombres de aplicación para la misma aplicación. Esto es especialmente útil si ofreces tu aplicación en varios idiomas y deseas usar nombres distintos para diferentes idiomas. También puede reservar un nombre nuevo para cambiar el nombre de una aplicación, como se describe a continuación.

Para reservar un nombre de aplicación nuevo, busque el cuadro de texto en la sección **reservar más nombres** de la página **administrar nombres de aplicaciones** . Escribe el nombre que te gustaría reservar y haz clic en **Comprobar disponibilidad** . Si el nombre está disponible, haga clic en **reservar nombre de producto** . Puede reservar varios nombres de aplicación repitiendo estos pasos, si lo desea.

> [!NOTE]
> Para obtener más información sobre cómo reservar nombres de aplicaciones y por qué es posible que un nombre determinado no esté disponible, consulte [creación de una aplicación mediante la reserva de un nombre](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Eliminar nombres de aplicación

Si ya no quieres usar un nombre que has reservado previamente, puedes liberarlo eliminándolo aquí. Asegúrate de que realmente quieres hacerlo, ya que esto significa que el nombre estará disponible inmediatamente para que otra persona lo reserve y use.

Para eliminar uno de los nombres reservados de la aplicación, busca el nombre que quieres dejar de usar y haz clic en **Eliminar** . En el cuadro de diálogo de confirmación, vuelve a hacer clic en **Eliminar** para confirmar.

Tenga en cuenta que la aplicación debe tener al menos un nombre reservado. Para quitar completamente una aplicación del centro de Partners (y liberar todos los nombres que ha reservado para esa aplicación), haga clic en **eliminar esta aplicación** en la página de **información general** de la aplicación. Si tiene un envío de la aplicación en curso, deberá eliminar el envío primero. Tenga en cuenta que si ya ha publicado la aplicación en la tienda, no podrá eliminarla del centro de Partners (aunque puede usar la funcionalidad **Mostrar u ocultar productos** en la página de **información general** para ocultarla). 


## <a name="rename-an-app-that-has-already-been-published"></a>Cambiar el nombre de una aplicación ya publicada

Si la aplicación ya está en el almacén y desea cambiarle el nombre, puede hacerlo reservando un nuevo nombre para ella (siguiendo los pasos descritos anteriormente) y, después, creando un nuevo envío para la aplicación. 

Debe actualizar los paquetes de la aplicación para reemplazar el nombre anterior por el nuevo y cargar los paquetes actualizados en el envío de.
- En primer lugar, actualice el archivo de Package.StoreAssociation.xml para usar el nuevo nombre, ya sea manualmente o mediante Visual Studio ( **Project > Store > asociar la aplicación con la tienda...** ). Para obtener más información, consulte [empaquetar una aplicación para UWP con Visual Studio](/windows/msix/package/packaging-uwp-apps).
- También deberá actualizar el elemento [**Package/Properties/DisplayName**](/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) en el manifiesto de la aplicación y actualizar los gráficos o el texto que incluya el nombre de la aplicación. 
  > [!IMPORTANT]
  > Asegúrese de actualizar el archivo de Package.StoreAssociation.xml antes de cambiar el **paquete/propiedades/DisplayName** en el manifiesto de la aplicación, o puede que obtenga un error.

Para actualizar una lista de tiendas de modo que use el nuevo nombre, vaya a la [Página de lista](create-app-store-listings.md) de la tienda correspondiente a ese idioma y seleccione el nombre en la lista desplegable nombre del **producto** . Asegúrese de revisar la descripción y otras partes de la lista para ver las menciones del nombre y realizar las actualizaciones si es necesario.

> [!NOTE]
> Si la aplicación tiene paquetes o listas de tiendas en varios idiomas, deberá actualizar los paquetes o las listas de la tienda para cada idioma en el que sea necesario actualizar el nombre.

Una vez que la aplicación se ha publicado con el nuevo nombre, puede eliminar cualquier nombre anterior que ya no necesite usar.

> [!TIP]
> Cada aplicación aparece en el centro de Partners con el nombre que ha reservado para ella. Si ha seguido los pasos anteriores para cambiar el nombre de una aplicación y desea que aparezca en el centro de Partners con el nuevo nombre, debe eliminar el nombre original (haciendo clic en **eliminar** en la página **administrar nombres de aplicación** ). 

 

 
