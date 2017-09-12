---
author: jnHs
Description: "Ve los nombres que has reservado para tu aplicación, reserva nombres adicionales (para otros idiomas o para cambiar el nombre de la aplicación) y elimina nombres reservados que ya no necesites."
title: "Administrar nombres de aplicación"
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 539ccc671ae3f66c6c17a077ac1c09d989d86fdc
ms.sourcegitcommit: d2ec178103f49b198da2ee486f1681e38dcc8e7b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2017
---
# <a name="manage-app-names"></a>Administrar nombres de aplicación


Puedes ver todos los nombres que has reservado para tu aplicación, reservar nombres adicionales (para otros idiomas o para cambiar el nombre de la aplicación) y eliminar nombres reservados que no necesites. Para ello, ve a la página **Administrar nombres de aplicación** en la sección **Administración de aplicaciones** para cualquiera de las aplicaciones en el panel del Centro de desarrollo de Windows.

## <a name="reserve-additional-names-for-your-app"></a>Reservar más nombres para la aplicación

Puedes reservar varios nombres de aplicación para la misma aplicación. Esto es especialmente útil si ofreces tu aplicación en varios idiomas y deseas usar nombres distintos para diferentes idiomas. También puedes reservar un nombre nuevo para cambiar el nombre de una aplicación.

En la sección **Reservar más nombres** de la página **Administrar nombres de aplicación**, verás un cuadro de texto. Escribe el nombre que te gustaría reservar y haz clic en **Comprobar disponibilidad**. Si el nombre está disponible, haz clic en **Reservar nombre de producto**.

> [!NOTE]
> Para obtener más información sobre la reserva de nombres de aplicación y sobre por qué un nombre determinado puede no estar disponible, consulta [Crear tu aplicación reservando un nombre](create-your-app-by-reserving-a-name.md).

Puedes seguir reservando nombres de aplicación adicionales aquí si lo deseas.

## <a name="delete-app-names"></a>Eliminar nombres de aplicación

Si ya no quieres usar un nombre que has reservado previamente, puedes liberarlo eliminándolo aquí. Asegúrate de que realmente quieres hacerlo, ya que esto significa que el nombre estará disponible inmediatamente para que otra persona lo reserve y use.

Para eliminar uno de los nombres reservados de la aplicación, busca el nombre que quieres dejar de usar y haz clic en **Eliminar**. En el cuadro de diálogo de confirmación, vuelve a hacer clic en **Eliminar** para confirmar.

Ten en cuenta que la aplicación debe tener al menos un nombre reservado. Para quitar completamente una aplicación del panel de información (y liberar todos los nombres reservados de la aplicación), haz clic en **Eliminar esta aplicación** en la página **Información general de la aplicación**. Si tienes un envío de la aplicación en curso, primero deberás eliminarlo. Si ya has publicado la aplicación en la Tienda, no puedes eliminarla del panel de información (aunque puedes usar la funcionalidad **Mostrar u ocultar productos** en la página **Información general** para ocultarla). 

## <a name="rename-an-app-that-has-already-been-published"></a>Cambiar el nombre de una aplicación ya publicada

Si la aplicación ya está en la Tienda y quieres cambiarle el nombre, puedes hacerlo reservando un nombre nuevo (siguiendo los pasos descritos anteriormente) y, a continuación, creando un nuevo envío de la aplicación.

Debes actualizar los paquetes de la aplicación para que incluyan el nuevo nombre a fin de que la Tienda los muestre con el nuevo nombre. En primer lugar, actualiza el archivo Package.StoreAssociation.xml para usar el nombre nuevo, ya sea manualmente o con Visual Studio (**Proyecto > Tienda > Asociar aplicación con la Tienda...**). Para más información, consulta [Empaquetado de aplicaciones para UWP](../packaging/packaging-uwp-apps.md).

También tendrás que actualizar el elemento [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-displayname) del manifiesto de la aplicación y actualizar todos los gráficos o texto que incluyan el nombre de la aplicación. 

> [!IMPORTANT]
> Asegúrate de actualizar el archivo Package.StoreAssociation.xml antes de cambiar el elemento **Package/Properties/DisplayName** de manifiesto de la aplicación o se puede producir un error.

También tendrás que revisar la descripción de la Tienda la aplicación y cambiar el nombre si lo mencionas ahí. Una vez que se ha publicado la aplicación con el nuevo nombre, puedes eliminar el nombre anterior que ya no necesitas.

 

 




