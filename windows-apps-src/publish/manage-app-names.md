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
keywords: Windows 10, uwp, nombres de aplicación, cambian el nombre de la aplicación, nombre de la aplicación de actualización, nombre del juego, nombre del producto
ms.localizationpriority: medium
ms.openlocfilehash: f0d2c6f72e2f69f0b768af55f9bddeb9bb008027
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "3390171"
---
# <a name="manage-app-names"></a>Administrar nombres de aplicación

Lo **nombres de aplicación de administrar** le permite ver todos los nombres que has reservado para tu aplicación, reservar nombres adicionales (para otros idiomas o para cambiar el nombre de la aplicación) y eliminar nombres no es necesario. Puedes encontrar esta página en el [panel del centro de desarrollo de Windows](https://partner.microsoft.com/dashboard) , expande la sección de **administración de aplicaciones** en el menú de navegación izquierdo para cualquiera de las aplicaciones.


## <a name="reserve-additional-names-for-your-app"></a>Reservar más nombres para la aplicación

Puedes reservar varios nombres de aplicación para la misma aplicación. Esto es especialmente útil si ofreces tu aplicación en varios idiomas y deseas usar nombres distintos para diferentes idiomas. También puedes reservar un nombre nuevo para cambiar el nombre de una aplicación, como se describe a continuación.

Para reservar un nombre nuevo de aplicación, busque el cuadro de texto en la sección **reservar más nombres** de la página **Administrar nombres de aplicación** . Escribe el nombre que te gustaría reservar y haz clic en **Comprobar disponibilidad**. Si el nombre está disponible, haz clic en **Reservar nombre de producto**. Puedes reservar varios nombres de aplicación, repita estos pasos, si lo deseas.

> [!NOTE]
> Para obtener más información sobre la reserva de nombres de aplicación y sobre por qué un nombre determinado puede no estar disponible, consulta [Crear tu aplicación reservando un nombre](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Eliminar nombres de aplicación

Si ya no quieres usar un nombre que has reservado previamente, puedes liberarlo eliminándolo aquí. Asegúrate de que realmente quieres hacerlo, ya que esto significa que el nombre estará disponible inmediatamente para que otra persona lo reserve y use.

Para eliminar uno de los nombres reservados de la aplicación, busca el nombre que quieres dejar de usar y haz clic en **Eliminar**. En el cuadro de diálogo de confirmación, vuelve a hacer clic en **Eliminar** para confirmar.

Ten en cuenta que la aplicación debe tener al menos un nombre reservado. Para completamente quitar una aplicación del panel (y liberar todos los nombres reservados de la aplicación), haz clic en **Eliminar esta aplicación** desde la página de **Introducción a la aplicación** . Si tienes un envío de la aplicación en curso, primero deberás eliminarlo. Ten en cuenta que si ya has publicado la aplicación a la tienda, no podrá eliminarla del panel de información (aunque puedes usar la funcionalidad de **Mostrar u ocultar productos** en la página de **Introducción** para ocultarla). 


## <a name="rename-an-app-that-has-already-been-published"></a>Cambiar el nombre de una aplicación ya publicada

Si la aplicación ya está en la Tienda y quieres cambiarle el nombre, puedes hacerlo reservando un nombre nuevo (siguiendo los pasos descritos anteriormente) y, a continuación, creando un nuevo envío de la aplicación. 

Debes actualizar los paquetes de la aplicación para reemplazar el nombre anterior por el nuevo y cargar los paquetes actualizados para su envío.
- En primer lugar, actualiza el archivo Package.StoreAssociation.xml para usar el nuevo nombre, ya sea manualmente o mediante el uso de Visual Studio (**proyecto > tienda > asociar aplicación con la tienda …**). Para obtener más información, consulta el [paquete de una aplicación para UWP con Visual Studio](../packaging/packaging-uwp-apps.md).
- También tendrás que actualizar el elemento [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) del manifiesto de la aplicación y actualizar todos los gráficos o texto que incluyan el nombre de la aplicación. 
  > [!IMPORTANT]
  > Asegúrate de actualizar el archivo Package.StoreAssociation.xml antes de cambiar el elemento **Package/Properties/DisplayName** de manifiesto de la aplicación o se puede producir un error.

Para actualizar una descripción de Store para que usa el nuevo nombre, ve a la [página de descripción de la tienda](create-app-store-listings.md) para ese idioma y selecciona el nombre de la lista desplegable de **nombre del producto** . Asegúrate de revisar la descripción y otras partes de la descripción para cualquier menciones del nombre y realizar actualizaciones si es necesario.

> [!NOTE]
> Si la aplicación tiene paquetes o descripciones de Store en varios idiomas, deberás actualizar los paquetes o descripciones para cada idioma en el que hay que actualizar el nombre de la tienda.

Una vez que se ha publicado la aplicación con el nuevo nombre, puedes eliminar cualquier nombre anterior que ya no necesitas usar.

> [!TIP]
> Cada aplicación aparece en el panel de uso del nombre que reservaste para ella. Si has seguido los pasos anteriores para cambiar el nombre de una aplicación, y que quieras que aparezca en el panel con el nuevo nombre, debe eliminar el nombre original (haciendo clic en **Eliminar** en la página **Administrar nombres de aplicación** ). 

 

 




