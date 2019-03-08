---
Description: Ve los nombres que has reservado para tu aplicación, reserva nombres adicionales (para otros idiomas o para cambiar el nombre de la aplicación) y elimina nombres reservados que ya no necesites.
title: Administrar nombres de aplicación
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, uwp, los nombres de aplicación, cambian el nombre de la aplicación, nombre de la actualización de aplicación, nombre del juego, nombre de producto
ms.localizationpriority: medium
ms.openlocfilehash: a27955f64a36fadde9b0f1781337929ce6871a9c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657610"
---
# <a name="manage-app-names"></a>Administrar nombres de aplicación

El **administrar nombres de aplicación** le permite ver todos los nombres que has reservado para la aplicación, reservar nombres adicionales (para otros lenguajes o para cambiar el nombre de la aplicación) y elimine los nombres no es necesario. Puede encontrar en esta página [centro de partners](https://partner.microsoft.com/dashboard) expandiendo el **administración de aplicaciones** sección en el menú de navegación izquierdo para cualquiera de las aplicaciones.

> [!IMPORTANT]
> Puede reservar nombres adicionales para una aplicación y decide usar una de ellas en la versión publicada de la aplicación en lugar de la que reserva cuando creó por primera vez la aplicación en el centro de partners. Sin embargo, sea consciente de que se utilizará el nombre que se reserva para su producto en la parte de la TI de [detalles de la identidad](view-app-identity-details.md), como el **nombre de familia de paquete (PFN)**. Estos valores pueden ser visibles para algunos usuarios y no se puede cambiar, así que asegúrese de que el nombre en primer lugar reservar es adecuado para este uso.


## <a name="reserve-additional-names-for-your-app"></a>Reservar más nombres para la aplicación

Puedes reservar varios nombres de aplicación para la misma aplicación. Esto es especialmente útil si ofreces tu aplicación en varios idiomas y deseas usar nombres distintos para diferentes idiomas. También puede reservar un nombre nuevo para poder cambiar el nombre de una aplicación, como se describe a continuación.

Para reservar un nuevo nombre de aplicación, busque el cuadro de texto en el **reservar nombres más** sección de la **administrar nombres de aplicación** página. Escribe el nombre que te gustaría reservar y haz clic en **Comprobar disponibilidad**. Si el nombre está disponible, haz clic en **Reservar nombre de producto**. Puede reservar varios nombres de aplicación, repita estos pasos, si lo desea.

> [!NOTE]
> Para obtener más información sobre la reserva de nombres de aplicación y sobre por qué un nombre determinado puede no estar disponible, consulta [Crear tu aplicación reservando un nombre](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Eliminar nombres de aplicación

Si ya no quieres usar un nombre que has reservado previamente, puedes liberarlo eliminándolo aquí. Asegúrate de que realmente quieres hacerlo, ya que esto significa que el nombre estará disponible inmediatamente para que otra persona lo reserve y use.

Para eliminar uno de los nombres reservados de la aplicación, busca el nombre que quieres dejar de usar y haz clic en **Eliminar**. En el cuadro de diálogo de confirmación, vuelve a hacer clic en **Eliminar** para confirmar.

Tenga en cuenta que la aplicación debe tener al menos un nombre reservado. Para completamente quitar una aplicación de centro de partners (y liberar todos los nombres que has reservado para esa aplicación), haga clic en **eliminar esta aplicación** desde el **información general sobre aplicaciones** página. Si tienes un envío de la aplicación en curso, primero deberás eliminarlo. Tenga en cuenta que si ya ha publicado la aplicación en el Store, no se puede eliminar del centro de partners (aunque puede usar el **productos de mostrar u ocultar** funcionalidad en su **Introducción** página para ocultarla). 


## <a name="rename-an-app-that-has-already-been-published"></a>Cambiar el nombre de una aplicación ya publicada

Si la aplicación ya está en la Tienda y quieres cambiarle el nombre, puedes hacerlo reservando un nombre nuevo (siguiendo los pasos descritos anteriormente) y, a continuación, creando un nuevo envío de la aplicación. 

Debe actualizar los paquetes de la aplicación para reemplazar el nombre antiguo con uno nuevo y cargar los paquetes actualizados en su envío.
- En primer lugar, actualiza el archivo Package.StoreAssociation.xml para usar el nombre nuevo, ya sea manualmente o con Visual Studio (**Proyecto > Tienda > Asociar aplicación con la Tienda...**). Para obtener más información, consulte [empaquetar una aplicación para UWP con Visual Studio](../packaging/packaging-uwp-apps.md).
- También tendrás que actualizar el elemento [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) del manifiesto de la aplicación y actualizar todos los gráficos o texto que incluyan el nombre de la aplicación. 
  > [!IMPORTANT]
  > Asegúrate de actualizar el archivo Package.StoreAssociation.xml antes de cambiar el elemento **Package/Properties/DisplayName** de manifiesto de la aplicación o se puede producir un error.

Para actualizar un listado de Store para que use el nuevo nombre, vaya a la [página de listado de Store](create-app-store-listings.md) para ese lenguaje y seleccione el nombre de la **nombre de producto** lista desplegable. Asegúrese de revisar su descripción y otras partes de la lista de cualquier mención del nombre y realizar actualizaciones si es necesario.

> [!NOTE]
> Si la aplicación tiene los paquetes o programas Store en varios idiomas, deberá actualizar los paquetes o Store anuncios para cada idioma en el que el nombre debe actualizarse.

Una vez que se ha publicado la aplicación con el nuevo nombre, puede eliminar todos los nombres antiguos que ya no necesita usar.

> [!TIP]
> Cada aplicación aparece en el centro de partners mediante el nombre que reserva para él. Si ha seguido los pasos anteriores para cambiar el nombre de una aplicación y desea que aparezca en el centro de partners usando el nuevo nombre, debe eliminar el nombre original (haciendo clic en **eliminar** en el **administrar nombres de aplicación** página). 

 

 




