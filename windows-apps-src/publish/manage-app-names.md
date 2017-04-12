---
author: jnHs
Description: "Ve los nombres que has reservado para tu aplicación, reserva nombres adicionales (para otros idiomas o para cambiar el nombre de la aplicación) y elimina nombres reservados que ya no necesites."
title: "Administrar nombres de aplicación"
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 02/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 0dc9582a0a041f6e30a2bebaa22843983d633cee
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manage-app-names"></a>Administrar nombres de aplicación


Puedes ver todos los nombres que has reservado para tu aplicación, reservar nombres adicionales (para otros idiomas o para cambiar el nombre de la aplicación) y eliminar nombres reservados que no necesites. Para ello, ve a la página **Administrar nombres de aplicación** en la sección **Administración de aplicaciones** para cualquiera de las aplicaciones en el panel del Centro de desarrollo de Windows.

## <a name="reserve-additional-names-for-your-app"></a>Reservar más nombres para la aplicación

Puedes reservar varios nombres de aplicación para la misma aplicación. Esto es especialmente útil si ofreces tu aplicación en varios idiomas y deseas usar nombres distintos para diferentes idiomas. También puedes esta opción para cambiar el nombre de una aplicación que aún no has publicado.

En la sección **Reservar más nombres** de la página **Administrar nombres de aplicación**, verás un cuadro de texto. Escribe el nombre que te gustaría reservar y haz clic en **Comprobar disponibilidad**. Si el nombre está disponible, haz clic en **Reservar nombre**.

> **Nota** Para obtener más información sobre la reserva de nombres de aplicación y sobre por qué un nombre determinado puede no estar disponible, consulta [Crear la aplicación reservando un nombre](create-your-app-by-reserving-a-name.md).

Puedes seguir reservando nombres de aplicación adicionales aquí si lo deseas.

## <a name="delete-app-names"></a>Eliminar nombres de aplicación

Si ya no quieres usar un nombre que has reservado previamente, puedes liberarlo eliminándolo aquí. Asegúrate de que realmente quieres hacerlo, ya que esto significa que el nombre estará disponible inmediatamente para que otra persona lo reserve y use.

Para eliminar uno de los nombres reservados de la aplicación, busca el nombre que quieres dejar de usar y haz clic en **Eliminar**. En el cuadro de diálogo de confirmación, vuelve a hacer clic en **Eliminar** para confirmar.

Ten en cuenta que la aplicación debe tener al menos un nombre reservado. Para quitar completamente una aplicación del panel (y liberar todos los nombres reservados de la aplicación), puedes hacer clic en **Eliminar esta aplicación** desde su página **Introducción**. Si has presentado una aplicación y está en curso, tendrás que eliminar esa presentación primero (y, si ya publicaste la aplicación a la Tienda, no puedes eliminarla del panel).

## <a name="rename-an-app-that-has-already-been-published"></a>Cambiar el nombre de una aplicación ya publicada

Si la aplicación ya está en la Tienda Windows y quieres cambiarle el nombre, puedes hacerlo reservando un nombre nuevo (siguiendo los pasos descritos anteriormente) y, a continuación, creando un nuevo envío de la aplicación. Ten en cuenta que tendrás que actualizar el paquete para que incluya el nuevo nombre a fin de que la Tienda muestre la aplicación con el nuevo nombre. Asegúrate de usar el nuevo nombre en el elemento [**Package/Properties/DisplayName**](https://msdn.microsoft.com/en-us/library/windows/apps/dn934748.aspx) del manifiesto de la aplicación y actualiza todos los gráficos o texto que incluyan el nombre de la aplicación. También tendrás que revisar la descripción de la aplicación y cambiar el nombre si lo mencionas en cualquier parte.

Una vez que se ha publicado la aplicación con el nuevo nombre, puedes eliminar el nombre anterior que ya no necesitas.

 

 




