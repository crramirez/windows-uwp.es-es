---
author: jnHs
Description: View the names that you've reserved for your app, reserve additional names (for other languages or to change your app's name), and delete reserved names that you don't need anymore.
title: Administrar nombres de aplicación
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 8/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, los nombres de la aplicación, cambian el nombre de la aplicación, nombre de la aplicación de actualización, nombre del juego, nombre de producto
ms.localizationpriority: medium
ms.openlocfilehash: f0d2c6f72e2f69f0b768af55f9bddeb9bb008027
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "2861884"
---
# <a name="manage-app-names"></a>Administrar nombres de aplicación

Los **nombres de la aplicación de administrar** le permite ver todos los nombres que ha reservado para la aplicación, reservar nombres adicionales (para otros idiomas o para cambiar el nombre de la aplicación) y eliminar los nombres no es necesario. Puede encontrar esta página en el [panel del centro de desarrollo de Windows](https://partner.microsoft.com/dashboard) , expanda la sección **administración de aplicaciones** en el menú de navegación izquierda para cualquiera de sus aplicaciones.


## <a name="reserve-additional-names-for-your-app"></a>Reservar más nombres para la aplicación

Puedes reservar varios nombres de aplicación para la misma aplicación. Esto es especialmente útil si ofreces tu aplicación en varios idiomas y deseas usar nombres distintos para diferentes idiomas. También puede reservar un nombre nuevo para cambiar el nombre de una aplicación, tal y como se describe a continuación.

Para reservar un nuevo nombre de la aplicación, busque el cuadro de texto en la sección **reservar más nombres** de la página **Administrar aplicación nombres** . Escribe el nombre que te gustaría reservar y haz clic en **Comprobar disponibilidad**. Si el nombre está disponible, haz clic en **Reservar nombre de producto**. Puede reservar varios nombres de aplicación, repita estos pasos, si así lo desea.

> [!NOTE]
> Para obtener más información sobre la reserva de nombres de aplicación y sobre por qué un nombre determinado puede no estar disponible, consulta [Crear tu aplicación reservando un nombre](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Eliminar nombres de aplicación

Si ya no quieres usar un nombre que has reservado previamente, puedes liberarlo eliminándolo aquí. Asegúrate de que realmente quieres hacerlo, ya que esto significa que el nombre estará disponible inmediatamente para que otra persona lo reserve y use.

Para eliminar uno de los nombres reservados de la aplicación, busca el nombre que quieres dejar de usar y haz clic en **Eliminar**. En el cuadro de diálogo de confirmación, vuelve a hacer clic en **Eliminar** para confirmar.

Tenga en cuenta que la aplicación debe tener al menos un nombre reservado. Para completamente quitar una aplicación de panel, (y todos los nombres que se ha reservado para esa aplicación de la versión), haga clic en **Eliminar esta aplicación** desde la página de **información general de la aplicación** . Si tienes un envío de la aplicación en curso, primero deberás eliminarlo. Tenga en cuenta que si ya ha publicado la aplicación en el almacén, no puede eliminarla desde el panel (aunque se puede usar la funcionalidad de **los productos de mostrar u ocultar** en la página de **información general** para ocultarlo). 


## <a name="rename-an-app-that-has-already-been-published"></a>Cambiar el nombre de una aplicación ya publicada

Si la aplicación ya está en la Tienda y quieres cambiarle el nombre, puedes hacerlo reservando un nombre nuevo (siguiendo los pasos descritos anteriormente) y, a continuación, creando un nuevo envío de la aplicación. 

Debe actualizar los paquetes de la aplicación para reemplazar el nombre antiguo con el nuevo y cargue los paquetes actualizados para su envío.
- En primer lugar, actualice el archivo de Package.StoreAssociation.xml para usar el nuevo nombre, ya sea manualmente o mediante el uso de Visual Studio (**proyecto > almacenamiento > asociar aplicación con el almacén de...**). Para obtener más información, vea el [paquete de una aplicación de UWP con Visual Studio](../packaging/packaging-uwp-apps.md).
- También tendrás que actualizar el elemento [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) del manifiesto de la aplicación y actualizar todos los gráficos o texto que incluyan el nombre de la aplicación. 
  > [!IMPORTANT]
  > Asegúrate de actualizar el archivo Package.StoreAssociation.xml antes de cambiar el elemento **Package/Properties/DisplayName** de manifiesto de la aplicación o se puede producir un error.

Para actualizar un listado de almacenamiento para que utilice el nuevo nombre, vaya a la [página de lista de almacenamiento](create-app-store-listings.md) para ese idioma y seleccione el nombre de la lista desplegable de **nombre de producto** . Asegúrese de revisar la descripción y otras partes del listado de cualquier menciones del nombre y realizar actualizaciones si es necesario.

> [!NOTE]
> Si la aplicación tiene paquetes o los anuncios de tienda en varios idiomas, que necesitará para actualizar los paquetes o listas para cada idioma en el que debe actualizarse el nombre de la tienda.

Una vez que la aplicación se ha publicado con el nuevo nombre, puede eliminar cualquier nombres más antiguos que ya no necesita usar.

> [!TIP]
> Cada aplicación aparece en el panel con el nombre que se reserva para él. Si ha seguido los pasos anteriores para cambiar el nombre de una aplicación, y le gustaría que aparezca en el panel con el nuevo nombre, debe eliminar el nombre original (haciendo clic en **Eliminar** en la página **Administrar aplicación nombres** ). 

 

 




