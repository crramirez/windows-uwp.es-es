---
author: jnHs
Description: View the names that you've reserved for your app, reserve additional names (for other languages or to change your app's name), and delete reserved names that you don't need anymore.
title: Administrar nombres de aplicación
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 10/02/2018
ms.topic: article
keywords: cambian de Windows 10, uwp, nombres de aplicación, app, actualización de la aplicación nombre, nombre del juego, nombre de producto
ms.localizationpriority: medium
ms.openlocfilehash: b35db620956e99791d03fb2d25dea8682d4ffaac
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "5919031"
---
# <a name="manage-app-names"></a>Administrar nombres de aplicación

Los **nombres de la aplicación de administrar** le permite ver todos los nombres que ha reservado para la aplicación de reserva nombres adicionales (para otros idiomas o para cambiar el nombre de la aplicación) y eliminar nombres que no necesites. Puede encontrar esta página en [ElCentro](https://partner.microsoft.com/dashboard) expandiendo la sección **administración de la aplicación** en el menú de navegación izquierdo para cualquiera de sus aplicaciones.

> [!IMPORTANT]
> Puede reservar nombres adicionales para una aplicación, y puede utilizar uno de éstos en la versión publicada de su aplicación en lugar de la que reserva cuando creó por primera vez su aplicación en el Centro para socios. Sin embargo, tenga en cuenta que se utilizará el nombre que se reserva para su producto en algunos de la TI de [Detalles de identidad](view-app-identity-details.md), como el **Nombre de la familia de paquete (PFN)**. Estos valores pueden ser visibles para algunos usuarios y no puede ser cambiado, así que asegúrese de que el nombre se reserva primero es adecuado para este uso.


## <a name="reserve-additional-names-for-your-app"></a>Reservar más nombres para la aplicación

Puedes reservar varios nombres de aplicación para la misma aplicación. Esto es especialmente útil si ofreces tu aplicación en varios idiomas y deseas usar nombres distintos para diferentes idiomas. También puede reservar un nuevo nombre para cambiar el nombre de una aplicación, como se describe a continuación.

Para reservar un nuevo nombre de la aplicación, busque el cuadro de texto en la sección **reservar más nombres** de la página **administrar los nombres de la aplicación** . Escribe el nombre que te gustaría reservar y haz clic en **Comprobar disponibilidad**. Si el nombre está disponible, haz clic en **Reservar nombre de producto**. Si lo desea puede reservar varios nombres de la aplicación, repita estos pasos.

> [!NOTE]
> Para obtener más información sobre la reserva de nombres de aplicación y sobre por qué un nombre determinado puede no estar disponible, consulta [Crear tu aplicación reservando un nombre](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Eliminar nombres de aplicación

Si ya no quieres usar un nombre que has reservado previamente, puedes liberarlo eliminándolo aquí. Asegúrate de que realmente quieres hacerlo, ya que esto significa que el nombre estará disponible inmediatamente para que otra persona lo reserve y use.

Para eliminar uno de los nombres reservados de la aplicación, busca el nombre que quieres dejar de usar y haz clic en **Eliminar**. En el cuadro de diálogo de confirmación, vuelve a hacer clic en **Eliminar** para confirmar.

Tenga en cuenta que su aplicación debe tener al menos un nombre reservado. Para quitar completamente una aplicación desde el Centro para socios (y liberar todos los nombres que se ha reservado para esa aplicación), haga clic en **Eliminar esta aplicación** desde la página de **Resumen de la aplicación** . Si tienes un envío de la aplicación en curso, primero deberás eliminarlo. Tenga en cuenta que si ya ha publicado la aplicación en el almacén, no podrá eliminarla desde el Centro para socios (aunque puede utilizar la funcionalidad de **los productos de mostrar u ocultar** en la página de **Resumen** para ocultarla). 


## <a name="rename-an-app-that-has-already-been-published"></a>Cambiar el nombre de una aplicación ya publicada

Si la aplicación ya está en la Tienda y quieres cambiarle el nombre, puedes hacerlo reservando un nombre nuevo (siguiendo los pasos descritos anteriormente) y, a continuación, creando un nuevo envío de la aplicación. 

Debe actualizar los paquetes de la aplicación para reemplazar el nombre antiguo con el nuevo y cargue los paquetes actualizados para su envío.
- En primer lugar, actualice el archivo de Package.StoreAssociation.xml para utilizar el nuevo nombre, ya sea manualmente o mediante Visual Studio (**proyecto > almacén > asociar la aplicación con el almacén...**). Para obtener más información, consulte el [paquete una aplicación UWP con Visual Studio](../packaging/packaging-uwp-apps.md).
- También tendrás que actualizar el elemento [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) del manifiesto de la aplicación y actualizar todos los gráficos o texto que incluyan el nombre de la aplicación. 
  > [!IMPORTANT]
  > Asegúrate de actualizar el archivo Package.StoreAssociation.xml antes de cambiar el elemento **Package/Properties/DisplayName** de manifiesto de la aplicación o se puede producir un error.

Para actualizar un listado de almacén para que utilice el nuevo nombre, vaya a la [página de listado de almacén](create-app-store-listings.md) para ese idioma y seleccione el nombre en la lista desplegable **nombre del producto** . Asegúrese de revisar la descripción y otras partes de la lista para cualquier mención del nombre y realizar actualizaciones si es necesario.

> [!NOTE]
> Si su aplicación tiene paquetes o los anuncios de tienda en varios idiomas, necesitará actualizar los paquetes o anuncios para todos los idiomas en los que es necesario actualizar el nombre de tienda.

Una vez que se ha publicado la aplicación con el nuevo nombre, puede eliminar los nombres antiguos que ya no se debe utilizar.

> [!TIP]
> Cada aplicación aparece en el Centro para socios con el nombre que reserva para él. Si ha seguido los pasos anteriores para cambiar el nombre de una aplicación y desea que aparezca en el Centro para socios con el nuevo nombre, debe eliminar el nombre original (haciendo clic en **Eliminar** en la página **Administrar nombres de la aplicación** ). 

 

 




