---
Description: El primer paso para crear una nueva aplicación en el centro de Partners es reservar un nombre de aplicación. Aprende a reservar nombres de aplicación y busca sugerencias para elegir un buen nombre de aplicación.
title: Crear la aplicación reservando un nombre
keywords: Windows 10, UWP, reserva de nombres, nombre de aplicación, nombres de aplicación, nombres, nombre de producto, nomenclatura, nombre reservado, título, nombres, títulos
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e2b95e30faa64ce6507de5a57b350ec59ca683e1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164179"
---
# <a name="create-your-app-by-reserving-a-name"></a>Crear la aplicación reservando un nombre

El primer paso para crear una nueva aplicación en el [centro de Partners](https://partner.microsoft.com/dashboard) es reservar un nombre de aplicación. Cada nombre reservado (a veces conocido como *título*de la aplicación) debe ser único en todo el Microsoft Store.

Puede reservar un nombre para la aplicación aunque no haya empezado a compilar la aplicación todavía. Se recomienda hacerlo lo antes posible, para que nadie más pueda usar el nombre. Tenga en cuenta que tendrá que enviar la aplicación en tres meses para mantener el nombre reservado para su uso.

Al [cargar los paquetes de la aplicación](upload-app-packages.md), el valor [**Package/Properties/DisplayName**](/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) debe coincidir con el nombre que ha reservado para la aplicación. Si usas Microsoft Visual Studio para crear el paquete de la aplicación, este atributo se rellenará automáticamente.

> [!IMPORTANT]
> Puede reservar nombres adicionales para una aplicación, y puede optar por usar una de ellas en la versión publicada de la aplicación en lugar de la que reserva al crear la aplicación por primera vez en el centro de Partners. Sin embargo, tenga en cuenta que el primer nombre que escriba aquí se usará en algunos de los detalles de la [identidad](view-app-identity-details.md)de la aplicación, como el nombre de **familia del paquete (PFN)**. Estos valores pueden ser visibles para algunos usuarios y no se pueden cambiar, por lo que debe asegurarse de que el nombre que se reserva es adecuado para este uso.


## <a name="create-your-app-by-reserving-a-new-name"></a>Crear la aplicación reservando un nuevo nombre

Reservar un nombre es el primer paso en la creación de una aplicación en el centro de Partners. 

1.  En la página **información general** , haga clic en **crear una nueva aplicación**.
2.  En el cuadro de texto, escriba el nombre que desea usar y, después, seleccione **comprobar disponibilidad**. Si el nombre está disponible, verás una marca de verificación verde. (Si el nombre que escribiste ya está reservado o lo usa otro desarrollador, verás un mensaje que indica que el nombre no está disponible).
3.  Haga clic en **reservar nombre de producto**.

El nombre ya está reservado para usted y puede empezar a trabajar en su [envío](app-submissions.md) siempre que esté listo. 

> [!NOTE]
> Es posible que no pueda reservar un nombre, aunque no vea ninguna aplicación enumerada con ese nombre en el Microsoft Store. Por lo general, esto se debe a que otro desarrollador ha reservado el nombre para su aplicación pero aún no la ha enviado. Si no puede reservar un nombre para el que contenga la marca comercial u otro derecho legal, o si ve otra aplicación en el Microsoft Store con ese nombre, [póngase en contacto con Microsoft](https://www.microsoft.com/info/cpyrtInfrg.html).

Después de reservar un nombre, tiene tres meses para enviar la aplicación. Si no lo envía en tres meses, la reserva de nombres expirará y otro desarrollador podrá usar ese nombre para una aplicación. Se puede producir un error si intentas enviar una aplicación con un nombre que has dejado expirar.


## <a name="choosing-your-apps-name"></a>Elegir el nombre de la aplicación

Elegir el nombre correcto para la aplicación es una tarea importante. Elige un nombre que capte la atención de los clientes y los haga leer más sobre la aplicación. Estas son algunas sugerencias para que elijas un buen nombre de aplicación.

-   **Elige un nombre corto.** El espacio dedicado para mostrar el nombre de tu aplicación es limitado en la mayoría de los casos, por lo que sugerimos que uses el nombre más corto posible. Aunque el nombre de tu aplicación puede tener hasta 256 caracteres, puede suceder que el final de un nombre muy largo no siempre esté visible para los clientes.
    > [!NOTE]
    > El número real de caracteres mostrados en varias ubicaciones puede variar en función de la longitud asignada y de los tipos de caracteres usados en el nombre de la aplicación. Por ejemplo, en la fuente Segoe UI que usa Windows, caben unos 30 caracteres "I" en el mismo espacio que 10 caracteres "W". Debido a esta variación, asegúrese de probar la aplicación y comprobar cómo aparece su nombre en sus iconos (si elige superponer el nombre de la aplicación), en los resultados de la búsqueda y dentro de la propia aplicación. Ten en cuenta también cada idioma en el que ofreces tu aplicación. Ten en cuenta que los caracteres asiáticos suelen ser más anchos que los latinos, por lo que se muestran menos caracteres.
-   **Sé original.** Procura que el nombre de tu aplicación sea lo suficientemente distintivo como para que no se confunda fácilmente con una aplicación existente.
-   **No uses nombres registrados por otros.** Asegúrate de tener los derechos de usar el nombre que reservas. Si alguien más tiene los derechos de marca comercial de ese nombre, puede denunciar una infracción y no podrás seguir usándolo. Si esto sucede después de publicada la aplicación, se quitará de la Tienda. Deberás cambiar el nombre de tu aplicación, incluyendo todas las instancias del nombre que aparecen en la aplicación y el contenido, antes de que puedas [enviar la aplicación](app-submissions.md) nuevamente para su certificación.
-   **Evita agregar información diferenciadora al final del nombre.** Si la información que distingue las distintas aplicaciones se agrega al final de un nombre, los clientes pueden no verla, especialmente si el nombre es largo y puede parecer que todas las aplicaciones tienen el mismo nombre. Si esto es inevitable, use diferentes logotipos e imágenes de aplicación para facilitar la diferenciación de una aplicación de otra.
-   **No incluya emojis en su nombre.** No podrá reservar un nombre que incluya emojis u otros caracteres no admitidos.


## <a name="manage-additional-app-names"></a>Administrar nombres de aplicación adicionales

Puede Agregar y administrar nombres adicionales en la página **administrar nombres de aplicación** de la sección **Administración de aplicaciones** para cada una de las aplicaciones en el centro de Partners.

En algunos casos, puede que desee reservar varios nombres para usarlos en la misma aplicación, por ejemplo, cuando quiera ofrecer su aplicación en varios idiomas y desee usar nombres diferentes para cada idioma. Si quieres cambiar el nombre de la aplicación completamente, deberás reservar un nombre adicional.

En esta página también puedes eliminar cualquier nombre que hayas reservado pero que ya no quieras usar.

Para obtener más información, consulta [Administrar nombres de aplicación](manage-app-names.md).

 

 